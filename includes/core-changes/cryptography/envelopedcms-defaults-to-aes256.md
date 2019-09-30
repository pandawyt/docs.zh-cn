---
ms.openlocfilehash: 7682eae5a28d4ef87d6622b775e0f2f9408bcbae
ms.sourcegitcommit: a4b10e1f2a8bb4e8ff902630855474a0c4f1b37a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2019
ms.locfileid: "71117113"
---
### <a name="envelopedcms-defaults-to-aes-256-encryption"></a><span data-ttu-id="60304-101">EnvelopedCms 默认为 AES-256 加密</span><span class="sxs-lookup"><span data-stu-id="60304-101">EnvelopedCms defaults to AES-256 encryption</span></span>

<span data-ttu-id="60304-102">`EnvelopedCms` 使用的默认对称加密算法已由 TripleDES 改为 AES-256</span><span class="sxs-lookup"><span data-stu-id="60304-102">The default symmetric encryption algorithm used by `EnvelopedCms` has changed from TripleDES to AES-256</span></span>

#### <a name="details"></a><span data-ttu-id="60304-103">详细信息</span><span class="sxs-lookup"><span data-stu-id="60304-103">Details</span></span>

<span data-ttu-id="60304-104">在 .NET Core 预览版 7 和更早版本中，如果 <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> 用于对数据加密但未通过构造函数重载指定对称加密算法，则使用 TripleDES/3DES/3DEA/DES3-EDE 算法对数据加密。</span><span class="sxs-lookup"><span data-stu-id="60304-104">In .NET Core Preview 7 and earlier versions, when <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> is used to encrypt data without specifying a symmetric encryption algorithm via a constructor overload, the data was encrypted with the TripleDES/3DES/3DEA/DES3-EDE algorithm.</span></span>

<span data-ttu-id="60304-105">从 .NET Core 3.0 预览版 8 开始（通过 [System.Security.Cryptography.Pkcs](https://www.nuget.org/packages/System.Security.Cryptography.Pkcs/) NuGet 包的 4.6.0 版本），默认算法已改为 AES-256 以实现算法现代化并提高默认选项的安全性。</span><span class="sxs-lookup"><span data-stu-id="60304-105">Starting with .NET Core 3.0 Preview 8 (via version 4.6.0 of the [System.Security.Cryptography.Pkcs](https://www.nuget.org/packages/System.Security.Cryptography.Pkcs/) NuGet package), the default algorithm has been changed to AES-256 for algorithm modernization and to improve the security of default options.</span></span> <span data-ttu-id="60304-106">如果消息收件人证书具有（非 EC）Diffie-Hellman 公钥，则由于底层平台的限制，加密操作可能会失败并显示 <xref:System.Security.Cryptography.CryptographicException>。</span><span class="sxs-lookup"><span data-stu-id="60304-106">If a message recipient certificate has a (non-EC) Diffie-Hellman public key, the encryption operation may fail with a <xref:System.Security.Cryptography.CryptographicException> due to limitations in the underlying platform.</span></span>

<span data-ttu-id="60304-107">在下面的示例代码中，如果在 .NET Core 3.0 预览版 7 或更早版本上运行，则使用 TripleDES 对数据加密。</span><span class="sxs-lookup"><span data-stu-id="60304-107">In the following sample code, the data is encrypted with TripleDES if running on .NET Core 3.0 Preview 7 or earlier.</span></span> <span data-ttu-id="60304-108">如果在.NET Core 3.0 预览版 8 或更高版本上运行，则使用 AES-256 对数据加密。</span><span class="sxs-lookup"><span data-stu-id="60304-108">If running on .NET Core 3.0 Preview 8 or later, it is encrypted with AES-256.</span></span>

```csharp
EnvelopedCms cms = new EnvelopedCms(content);
cms.Encrypt(recipient);
return cms.Encode();
```

#### <a name="version-introduced"></a><span data-ttu-id="60304-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="60304-109">Version introduced</span></span>

<span data-ttu-id="60304-110">3.0 预览版 8</span><span class="sxs-lookup"><span data-stu-id="60304-110">3.0 Preview 8</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="60304-111">建议的操作</span><span class="sxs-lookup"><span data-stu-id="60304-111">Recommended action</span></span>

<span data-ttu-id="60304-112">如果该更改有负面影响，则可以通过在包含 <xref:System.Security.Cryptography.Pkcs.AlgorithmIdentifier> 类型参数的 <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> 构造函数中显式指定加密算法标识符来还原 TripleDES 加密，例如：</span><span class="sxs-lookup"><span data-stu-id="60304-112">If you are negatively impacted by the change, you can restore TripleDES encryption by explicitly specifying the encryption algorithm identifier in an <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> constructor that includes a parameter of type <xref:System.Security.Cryptography.Pkcs.AlgorithmIdentifier>, such as:</span></span>

```csharp
Oid tripleDesOid = new Oid("1.2.840.113549.3.7", null);
AlgorithmIdentifier tripleDesIdentifier = new AlgorithmIdentifier(tripleDesOid);
EnvelopedCms cms = new EnvelopedCms(content, tripleDesIdentifier);

cms.Encrypt(recipient);
return cms.Encode()l
```

#### <a name="category"></a><span data-ttu-id="60304-113">类别</span><span class="sxs-lookup"><span data-stu-id="60304-113">Category</span></span>

<span data-ttu-id="60304-114">密码</span><span class="sxs-lookup"><span data-stu-id="60304-114">Cryptography</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="60304-115">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="60304-115">Affected APIs</span></span>

- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.ContentInfo)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.SubjectIdentifierType,System.Security.Cryptography.Pkcs.ContentInfo)?displayProperty=nameWithType>

<!--

### Affected APIs

- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.#ctor`
- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.#ctor(System.Security.Cryptography.Pkcs.ContentInfo)`
- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.SubjectIdentifierType,System.Security.Cryptography.Pkcs.ContentInfo)`

-->