---
ms.openlocfilehash: b697bbf6844759de8babd4245667e7b333884ff8
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90538914"
---
### <a name="ca2200-rethrow-to-preserve-stack-details"></a><span data-ttu-id="269f0-101">CA2200:再次引发以保留堆栈详细信息</span><span class="sxs-lookup"><span data-stu-id="269f0-101">CA2200: Rethrow to preserve stack details</span></span>

<span data-ttu-id="269f0-102">从 .NET 5.0 开始，默认启用 .NET 代码分析器规则 [CA2200](/visualstudio/code-quality/ca2200)。</span><span class="sxs-lookup"><span data-stu-id="269f0-102">.NET code analyzer rule [CA2200](/visualstudio/code-quality/ca2200) is enabled, by default, starting in .NET 5.0.</span></span> <span data-ttu-id="269f0-103">它针对再次引发异常的任何 `catch` 块生成一个生成警告，并在 `throw` 语句中显式指定该异常。</span><span class="sxs-lookup"><span data-stu-id="269f0-103">It produces a build warning for any `catch` blocks that rethrow an exception and the exception is explicitly specified in the `throw` statement.</span></span>

#### <a name="change-description"></a><span data-ttu-id="269f0-104">更改说明</span><span class="sxs-lookup"><span data-stu-id="269f0-104">Change description</span></span>

<span data-ttu-id="269f0-105">从 .NET 5.0 开始，.NET SDK 包括 [.NET 源代码分析器](../../../../docs/fundamentals/productivity/code-analysis.md)。</span><span class="sxs-lookup"><span data-stu-id="269f0-105">Starting in .NET 5.0, the .NET SDK includes [.NET source code analyzers](../../../../docs/fundamentals/productivity/code-analysis.md).</span></span> <span data-ttu-id="269f0-106">其中一些规则会默认启用，包括 [CA2200](/visualstudio/code-quality/ca2200)。</span><span class="sxs-lookup"><span data-stu-id="269f0-106">Several of these rules are enabled, by default, including [CA2200](/visualstudio/code-quality/ca2200).</span></span> <span data-ttu-id="269f0-107">如果项目包含违反此规则的代码，并已被配置为将警告视为错误，则此更改可能会中断生成。</span><span class="sxs-lookup"><span data-stu-id="269f0-107">If your project contains code that violates this rule and is configured to treat warnings as errors, this change could break your build.</span></span>

<span data-ttu-id="269f0-108">规则 CA2200 会标记代码，其中重新引发了异常，并且在 `throw` 语句中指定了异常变量。</span><span class="sxs-lookup"><span data-stu-id="269f0-108">Rule CA2200 flags code where exceptions are rethrown and the exception variable is specified in the `throw` statement.</span></span> <span data-ttu-id="269f0-109">引发异常时，它所包含的部分信息是堆栈跟踪。</span><span class="sxs-lookup"><span data-stu-id="269f0-109">When an exception is thrown, part of the information it carries is the stack trace.</span></span> <span data-ttu-id="269f0-110">堆栈跟踪是一个方法调用层次结构列表，它以引发异常的方法开头，以捕获异常的方法结尾。</span><span class="sxs-lookup"><span data-stu-id="269f0-110">The stack trace is a list of the method call hierarchy that starts with the method that throws the exception and ends with the method that catches the exception.</span></span> <span data-ttu-id="269f0-111">如果通过在 `throw` 语句中指定异常来重新引发该异常，则将在当前方法处重启堆栈跟踪，并且引发该异常的原始方法与当前方法之间的方法调用的列表将丢失。</span><span class="sxs-lookup"><span data-stu-id="269f0-111">If an exception is rethrown by specifying the exception in the `throw` statement, the stack trace restarts at the current method and the list of method calls between the original method that threw the exception and the current method is lost.</span></span> <span data-ttu-id="269f0-112">若要保留原始堆栈跟踪信息和异常信息，请在未指定异常的情况下使用 `throw` 语句。</span><span class="sxs-lookup"><span data-stu-id="269f0-112">To keep the original stack trace information with the exception, use the `throw` statement without specifying the exception.</span></span>

<span data-ttu-id="269f0-113">以下代码片段不会针对规则 CA2200 生成警告。</span><span class="sxs-lookup"><span data-stu-id="269f0-113">The following code snippet does not produce a warning for rule CA2200.</span></span> <span data-ttu-id="269f0-114">但是，注释行将触发冲突。</span><span class="sxs-lookup"><span data-stu-id="269f0-114">The commented line *would* trigger a violation, however.</span></span>

```csharp
catch (ArithmeticException e)
{
    // throw e;
    throw;
}
```

#### <a name="version-introduced"></a><span data-ttu-id="269f0-115">引入的版本</span><span class="sxs-lookup"><span data-stu-id="269f0-115">Version introduced</span></span>

<span data-ttu-id="269f0-116">5.0 RC1</span><span class="sxs-lookup"><span data-stu-id="269f0-116">5.0 RC1</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="269f0-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="269f0-117">Recommended action</span></span>

- <span data-ttu-id="269f0-118">在未显式指定异常的情况下再次引发异常。</span><span class="sxs-lookup"><span data-stu-id="269f0-118">Rethrow exceptions without specifying the exception explicitly.</span></span> <span data-ttu-id="269f0-119">有关详细信息，请参阅 [CA2200](/visualstudio/code-quality/ca2200)。</span><span class="sxs-lookup"><span data-stu-id="269f0-119">For more information, see [CA2200](/visualstudio/code-quality/ca2200).</span></span>

- <span data-ttu-id="269f0-120">若要完全禁用代码分析，请在项目文件中将 `EnableNETAnalyzers` 设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="269f0-120">To disable code analysis completely, set `EnableNETAnalyzers` to `false` in your project file.</span></span> <span data-ttu-id="269f0-121">有关详细信息，请参阅 [EnableNETAnalyzers](../../../../docs/core/project-sdk/msbuild-props.md#enablenetanalyzers)。</span><span class="sxs-lookup"><span data-stu-id="269f0-121">For more information, see [EnableNETAnalyzers](../../../../docs/core/project-sdk/msbuild-props.md#enablenetanalyzers).</span></span>

#### <a name="category"></a><span data-ttu-id="269f0-122">类别</span><span class="sxs-lookup"><span data-stu-id="269f0-122">Category</span></span>

<span data-ttu-id="269f0-123">代码分析</span><span class="sxs-lookup"><span data-stu-id="269f0-123">Code analysis</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="269f0-124">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="269f0-124">Affected APIs</span></span>

<span data-ttu-id="269f0-125">无法通过 API 分析检测到。</span><span class="sxs-lookup"><span data-stu-id="269f0-125">Not detectable via API analysis.</span></span>

<!--

#### Affected APIs

Not detectable via API analysis.

-->