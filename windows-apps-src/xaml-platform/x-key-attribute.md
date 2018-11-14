---
author: jwmsft
description: 唯一識別建立和參照為資源的元素，存在 ResourceDictionary 中。
title: xKey 屬性
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8d48ccb93a411e92b57059192de38366f27353a3
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6654181"
---
# <a name="xkey-attribute"></a>x:Key 屬性


唯一識別建立和參照為資源的元素，存在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中。

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

| 詞彙 | 說明 |
|------|-------------|
| object | 可共用的任何物件。 請參閱 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)。 |
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

-   字元限制在低 ASCII 範圍，更具體地說，僅限羅馬字母大寫與小寫字元、數字以及底線 (\_) 字元。
-   不支援 Unicode 字元範圍。
-   名稱開頭不可以是數字。

## <a name="remarks"></a>備註

[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的子元素通常包含 **x:Key** 屬性，該屬性會指定該字典內的唯一索引鍵值。 XAML 處理器在載入期間會強制執行索引鍵唯一性。 非唯一的 **x:Key** 值將導致 XAML 剖析例外狀況。 如果是 [{StaticResource} 標記延伸](staticresource-markup-extension.md)所要求，任何未解析的索引鍵也會導致 XAML 剖析例外狀況。

**x:Key** 與 [x:Name](x-name-attribute.md) 並非相同的概念。 **x:Key** 專門用在資源字典中。 x:Name 則用於 XAML 的所有區域。 使用索引鍵值的 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 呼叫將不會擷取索引資源。 資源字典中定義的物件可能會有 **x:Key**、**x:Name** 或是兩者。 金鑰和名稱不需要符合。

請注意，在顯示的隱含語法中，[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 物件在 XAML 處理器如何產生新物件以填入 [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740) 集合方面是隱含的。

等同於指定 **x:Key** 的程式碼，是使用含有基礎 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 的索引鍵的任何作業。 例如，當您新增資源到 **ResourceDictionary** 時，在資源的標記中套用的 **x:Key** 等同於 **Insert** 的 *key* 參數值。

如果資源字典中的項目為目標的 [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) 或 [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391)，則可以忽略 **x:Key** 的值；不管是哪一種情況，資源項目的隱含索引鍵都是會解譯為字串的 **TargetType** 值。 如需詳細資訊，請參閱[快速入門：設定控制項的樣式](https://msdn.microsoft.com/library/windows/apps/hh465498)和 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)。

