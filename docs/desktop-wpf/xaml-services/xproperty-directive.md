---
title: x:Property 指令
ms.date: 03/30/2017
ms.assetid: 618555a8-c893-455c-810f-ac54cd24ef10
ms.openlocfilehash: 2804ec935d0626cba9ef050f70a3266cf23bcce0
ms.sourcegitcommit: c2d9718996402993cf31541f11e95531bc68bad0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "81432676"
---
# <a name="xproperty-directive"></a>x:Property 指令

声明标记中的 XAML 属性。

## <a name="xaml-object-element-usage"></a>XAML 对象元素用法

```xaml
<object x:Class="className">
  <x:Members>
    <x:Property Name="propertyName" Type="propertyType"/>
    additionalProperties
  </x:Members>
</object>
```

## <a name="xaml-values"></a>XAML 值

|||
|-|-|
|`className`|XAML 生产的备用类或分部类的名称。|
|`propertyName`|正在定义的属性的成员名称。|
|`propertyType`|指定此属性类型的类型名称（或特定于框架的字符串形式）。|

## <a name="remarks"></a>备注

在 .NET XAML 服务实现中， `x:Property` 没有直接的类型支持，但受 <xref:System.Windows.Markup.PropertyDefinition> 类支持。 在 XAML 节点流中，`x:Property` 元素表示为 XAML 语言 XAML 命名空间中名为 `Property` 的成员。 成员 `Property` 拥有标记声明的属性。

的表示义`Name``Type`不在 .NET XAML 服务级别分配。 它们作为字符串值存储在初始的 XAML 节点流中，将在之后根据可能由特定框架强加的规则进行解释。 该含义可能与 XAML 名称和 XAML 类型含义一致，或者可能仅在备用类型系统中有效，这取决于实现。

若要支持将 `x:Members` 实际用作一种在标记中指定成员定义的方法，成员必须与可修改的类相关联。 预期模型是 `x:Members` 作为指定 `x:Class` 的类型的成员存在。 但是，在 .NET XAML 服务级别不支持关联类型和成员或生成动态成员定义的机制。 此功能由具有支持 XAML 的成员定义的应用程序模型的单个框架实现。 通常，需要 MSBUILD 生成操来支持此功能，这些操作以标记编译 XAML 并将其与隐藏代码相集成或者从 XMAL 生成纯程序集。

## <a name="xproperty-for-windows-workflow-foundation"></a>适用于 Windows Workflow Foundation 的 x:Property

对于 Windows Workflow Foundation，`x:Property` 定义完全在 XAML 中构成的自定义活动的成员，或具有隐藏代码的活动设计器的 XMAL 定义的动态成员。 此外，必须在 XAML 生产的根元素上指定 `x:Class`。 这不是 .NET XAML 服务级别的要求，但当支持自定义活动和 Windows 工作流基础 XAML 的 MSBUILD 生成操作加载 XAML 生产时，这将成为一项要求。 Windows 工作流基础不使用纯 XAML 类型名称作为其`x:Property``Type`属性的预期值，而是使用此处未记录的约定。 有关详细信息，请参阅[动态活动创建](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd807392(v=vs.100))。