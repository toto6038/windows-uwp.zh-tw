---
author: jwmsft
description: 說明 XAML 中附加屬性的概念並提供一些範例。
title: 附加屬性概觀
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: a0950bfc9d90ba893be8ca52cc295b38b142798e
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5637739"
---
# <a name="attached-properties-overview"></a>附加屬性概觀

「附加屬性」** 是一種 XAML 概念。 附加屬性可在物件上設定其他的屬性/值組，但屬性不是原始物件定義的一部分。 附加屬性一般定義為相依性屬性的特殊格式，在擁有者類型的物件模型中沒有傳統的屬性包裝函式。

## <a name="prerequisites"></a>先決條件

我們假設您已了解相依性屬性的基本概念，並已閱讀[相依性屬性概觀](dependency-properties-overview.md)。

## <a name="attached-properties-in-xaml"></a>XAML 中的附加屬性

在 XAML 中，您使用 _AttachedPropertyProvider.PropertyName_ 語法來設定附加屬性。 這裡是您如何在 XAML 中設定 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 的範例。

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> 我們只使用[**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771)做為範例中附加屬性而沒有充分解說您為何要使用它。 如果您想要進一步了解 **Canvas.Left** 的用途，以及 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 如何處理它的配置子項，請參閱 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 參考主題或[使用 XAML 定義版面配置](https://msdn.microsoft.com/library/windows/apps/mt228350)。

## <a name="why-use-attached-properties"></a>為什麼要使用附加屬性？

對於可能防止有關係的不同物件在執行階段彼此互通資訊的編碼慣例，附加屬性是一種可以避開這種編碼慣例的方式。 將屬性放在共同的基底類別上，讓每個物件都剛好能夠取得並設定該屬性，當然是一種可行的方式。 但是，到最後，僅僅是您可能想要這麼做的案例數目，就會讓您的基底類別塞滿可共用屬性。 它甚至可能引發一些情況，就是在數以百計的子系中，可能只有兩個嘗試使用某個屬性。 這並不是一個好的類別設計。 為了處理這樣的情況，便有了附加屬性的概念，讓物件可以為屬性指派它自己的類別結構所未定義的值。 在物件樹中建立各種物件之後，定義類別就可以在執行階段從子物件讀取這個值。

例如，子元素可以使用附加屬性來通知父元素它們在 UI 中的顯示方式。 這是 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) 附加屬性的例子。 **Canvas.Left** 會建立為附加屬性，因為它是要在 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 元素內含的元素中設定，而非在 **Canvas** 本身設定。 任何可能的子元素接著就可以使用 **Canvas.Left** 與 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772)，在 **Canvas** 配置容器父系內指定其配置位移。 附加屬性不但會完成這個動作，而且基本元素物件模型不會塞滿各種只能套用到一個可能的配置容器的屬性。 相反地，許多配置容器會實作自己的附加屬性集。

為了實作附加屬性，[**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 類別會定義一個名稱為 [**Canvas.LeftProperty**](https://msdn.microsoft.com/library/windows/apps/br209272) 的靜態 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 欄位。 接著，**Canvas** 提供 [**SetLeft**](https://msdn.microsoft.com/library/windows/apps/br209273) 與 [**GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) 方法做為附加屬性的公開存取子，讓 XAML 設定及執行階段值都能被存取。 對於 XAML 與相依性屬性系統來說，這組 API 可以滿足啟用附加屬性的特定 XAML 語法的樣式，並將值儲存在相依性屬性儲存區。

## <a name="how-the-owning-type-uses-attached-properties"></a>擁有者類型如何使用附加屬性

雖然可以在任何 XAML 元素 (或任何基礎 [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)) 中設定附加屬性，但並不表示設定該屬性將會產生明確的結果，或是必定會存取該值。 定義附加屬性的類型通常依循下列其中一種案例：

- 定義附加屬性的類型在其他物件的關係中是父項。 子物件會設定附加屬性的值。 附加屬性擁有者類型具有某種固有的行為，這個行為會逐一查看其子元素、取得值，並在物件存留期中的某個時間點作用於這些值 (一個配置動作、[**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) 等等)。
- 定義附加屬性的類型會用來做為各種可能的父元素和內容模型的子元素，但是資訊不一定是配置資訊。
- 附加屬性會向服務報告資訊，而不會向另一個 UI 元素報告資訊。

如需這些案例和擁有者類型的詳細資訊，請參閱[自訂附加屬性](custom-attached-properties.md)的＜深入了解 Canvas.Left＞一節。

## <a name="attached-properties-in-code"></a>程式碼中的附加屬性

附加屬性有別於其他相依性屬性，並沒有簡單的取得和設定存取權的一般屬性包裝函式。 這是因為附加屬性對於設定該屬性的執行個體而言，不是以程式碼為中心的物件模型的必須部分。 (雖然不常見，但是可以定義含兩種用途的屬性，一個是讓其他類型可以在自身上設定的附加屬性，另一個是在擁有者類型含有傳統的屬性用法)。

有兩種方法可以在程式碼中設定附加屬性：使用屬性系統 API，或使用 XAML 樣式存取子。 就最終結果而言，這兩種技術大致上是相等的，因此使用何種技術通常取決於編碼樣式。

### <a name="using-the-property-system"></a>使用屬性系統

Windows 執行階段的附加屬性會實作為相依性屬性，這樣一來值可由屬性系統儲存在共用的相依性屬性儲存區中。 因此，附加屬性會在擁有的類別中公開相依性屬性識別碼。

如果要以程式碼設定附加屬性，您必須呼叫 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 方法，然後傳遞做為該附加屬性識別碼的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 欄位。 (也必須傳遞要設定的值)。

如果要以程式碼取得附加屬性的值，可以呼叫 [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) 方法，接著同樣傳遞做為識別碼的 [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) 欄位。

### <a name="using-the-xaml-accessor-pattern"></a>使用 XAML 存取子樣式

當 XAML 剖析為物件樹時，XAML 存取子必須能夠設定附加屬性值。 附加屬性的擁有者類型必須實作專用的存取子方法在表單中名為 **取得 * * * PropertyName*和 **設定 * * * PropertyName*。 這些專用的存取子方法，也是在程式碼中取得或設定附加屬性的一種方式。 就程式碼的觀點而言，附加屬性與擁有方法存取子 (而非屬性存取子) 的支援欄位類似，該支援欄位可以存在任何物件中，不需要具體地定義。

下面的範例顯示如何透過 XAML 存取子 API，在程式碼中設定附加屬性。 在此範例中，`myCheckBox` 是 [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316) 類別的執行個體。 最後一行是實際設定值的程式碼，前一行會建立執行個體與它們的父系-子系關係。 如果您使用屬性系統，語法是未標記為註解的最後一行。 如果您使用 XAML 存取子樣式，語法是標記為註解的最後一行。

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>自訂附加屬性

如需如何定義自訂附加屬性的程式碼範例，以及使用附加屬性的案例詳細資訊，請參閱[自訂附加屬性](custom-attached-properties.md)。

## <a name="special-syntax-for-attached-property-references"></a>附加屬性參照的特殊語法

附加屬性名稱中的點，是識別碼樣式的重要部分。 有時候，如果某個語法或某個狀況將點視為含有其他意義時，意思就會不明確。 例如，在繫結路徑的情況下，會將點視為物件模型周遊。 在這類不明確的大多數情況下，附加屬性有一個特殊語法可以將內部的點仍舊剖析為附加屬性的 _owner_**.**_property_ 分隔符號。

- 如果要將附加屬性指定為動畫之目標路徑的一部分，請用括號 ("()") 括住附加屬性名稱，例如 "(Canvas.Left)"。 如需詳細資訊，請參閱 [Property-path 語法](property-path-syntax.md)。

> [!WARNING]
> Windows 執行階段 XAML 實作的侷限是您無法動畫顯示自訂附加的屬性。

- 如果要為從資源檔到 **x:Uid** 的資源參考指定一個附加屬性做為目標屬性，請使用特殊語法，將程式碼樣式的完整 **using:** 宣告插入方括弧 ("\[\]") 中，以建立一個明確的範圍分隔。 例如，假設那里一個元素`<TextBlock x:Uid="Title" />`，目標是**Canvas.Top**值，該執行個體上的資源檔中的資源索引鍵是 「 Title.\[using:Windows.UI.Xaml.Controls\]Canvas.Top 」。 如需資源檔案和 XAML 的詳細資訊，請參閱[快速入門：翻譯 UI 資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)。

## <a name="related-topics"></a>相關主題

- [自訂附加屬性](custom-attached-properties.md)
- [相依性屬性概觀](dependency-properties-overview.md)
- [使用 XAML 定義版面配置](https://msdn.microsoft.com/library/windows/apps/mt228350)
- [快速入門：翻譯 UI 資源](https://msdn.microsoft.com/library/windows/apps/hh943060)
- [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)
- [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)
