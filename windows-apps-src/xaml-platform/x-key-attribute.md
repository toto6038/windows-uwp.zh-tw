---
description: 唯一識別建立和參照為資源的元素，存在 ResourceDictionary 中。
title: xKey 屬性
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 462a1323766a4fb2cc1c8bfdc119c72069fd59e4
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2021
ms.locfileid: "105938993"
---
# <a name="xkey-attribute"></a>x:Key 屬性


唯一識別建立和參照為資源的元素，存在 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 中。

## <a name="xaml-attribute-usage"></a>XAML 屬性用法

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>XAML 屬性用法 (隱含 **ResourceDictionary**)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>XAML 值

| 詞彙 | 描述 |
|------|-------------|
| 物件 (object) | 可共用的任何物件。 請參閱 [ResourceDictionary 與 XAML 資源參考](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)。 |
| stringKeyValue | 做為索引鍵的合法字串，必須符合 _XamlName_ 文法。 請參閱下面的＜XamlName 文法＞。 |

##  <a name="xamlname-grammar"></a>XamlName 文法

下列是字串的基準文法，該字串在通用 Windows 平台 (UWP) XAML 實作中被當做是一個索引鍵：

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   字元會限制為較低的 ASCII 範圍，更明確地說，是以羅馬字母大寫和小寫字母、數位和底線 (\_) 字元。
-   不支援 Unicode 字元範圍。
-   名稱開頭不可以是數字。

## <a name="remarks"></a>備註

[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 的子元素通常包含 **x:Key** 屬性，該屬性會指定該字典內的唯一索引鍵值。 XAML 處理器在載入期間會強制執行索引鍵唯一性。 非唯一的 **x:Key** 值將導致 XAML 剖析例外狀況。 如果是 [{StaticResource} 標記延伸](staticresource-markup-extension.md)所要求，任何未解析的索引鍵也會導致 XAML 剖析例外狀況。

**x:Key** 與 [x:Name](x-name-attribute.md) 並非相同的概念。 **x:Key** 專門用在資源字典中。 x:Name 則用於 XAML 的所有區域。 使用索引鍵值的 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) 呼叫將不會擷取索引資源。 資源字典中定義的物件可能會有 **x:Key**、**x:Name** 或是兩者。 金鑰和名稱不需要符合。

請注意，在顯示的隱含語法中，[**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 物件在 XAML 處理器如何產生新物件以填入 [**Resources**](/uwp/api/windows.ui.xaml.frameworkelement.resources) 集合方面是隱含的。

等同於指定 **x:Key** 的程式碼，是使用含有基礎 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 的索引鍵的任何作業。 例如，當您新增資源到 **ResourceDictionary** 時，在資源的標記中套用的 **x:Key** 等同於 **Insert** 的 *key* 參數值。

如果資源字典中的項目為目標的 [**Style**](/uwp/api/Windows.UI.Xaml.Style) 或 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)，則可以忽略 **x:Key** 的值；不管是哪一種情況，資源項目的隱含索引鍵都是會解譯為字串的 **TargetType** 值。 如需詳細資訊，請參閱[快速入門：設定控制項的樣式](/previous-versions/windows/apps/hh465498(v=win.10))和 [ResourceDictionary 與 XAML 資源參考](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)。