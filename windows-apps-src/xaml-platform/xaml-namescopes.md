---
description: XAML 命名範圍會儲存 XAML 定義的物件名稱與它們的執行個體對應項之間的關係。 這個概念與其他程式設計語言和技術中較廣義的「命名範圍」一詞類似。
title: XAML 命名範圍
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ffe6119e9cac162486d23472f5d5876924b7ef9e
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7855376"
---
# <a name="xaml-namescopes"></a>XAML 命名範圍


*「XAML 命名範圍」* 會儲存 XAML 定義的物件名稱與它們的執行個體對應項之間的關係。 這個概念與其他程式設計語言和技術中較廣義的「命名範圍」 ** 一詞類似。

## <a name="how-xaml-namescopes-are-defined"></a>XAML 命名範圍如何定義

XAML 命名範圍中的名稱，可以讓使用者程式碼參考原先在 XAML 中宣告的物件。 剖析 XAML 的內部結果，是執行階段會建立一組保留這些物件在 XAML 宣告中的一些或所有關係的物件。 這些關係會維持為已建立的物件的特定物件屬性，或向程式撰寫模型 API 中的公用程式方法公開。

XAML 命名範圍中的名稱的最典型用法，是做為物件執行個體的參考，這是透過專案建置動作的標記編譯階段與部分類別範本中產生的 **InitializeComponent** 方法啟用的。

您也可以在執行階段自行使用公用程式方法 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)，以傳回在 XAML 標記中隨名稱一起定義的物件的參考。

### <a name="more-about-build-actions-and-xaml"></a>建置動作與 XAML 的更多資訊

技術上，XAML 本身會進行標記編譯器階段，同時，XAML 與 XAML 為程式碼後置定義的部分類別會一起編譯。 在標記中定義 **Name** 或 [x:Name 屬性](x-name-attribute.md)的每個物件元素，都會產生一個含有與 XAML 名稱相符的名稱的內部欄位。 這個欄位最初是空白的。 接著，類別會產生在所有 XAML 都載入後才會呼叫的 **InitializeComponent** 方法。 在 **InitializeComponent** 邏輯中，每個內部欄位都會填入對應的名稱字串的 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 傳回值。 編譯完成後，您可以在 Windows 執行階段應用程式專案的 /obj 子資料夾中查看為每個 XAML 頁面建立的 ".g" (產生的) 檔案，以觀察這個基礎結構。 您也可以查看做為結果組件成員的欄位與 **InitializeComponent** 方法 (如果您反映它們或以其他方式檢查它們的介面語言內容的話)。

**注意：** 專為 VisualC + + 元件延伸 (C + + /CX) 的應用程式， **X:name**參考的支援欄位不建立的 XAML 檔案的根元素。 如果您需要從 C++/CX 程式碼後置參考根物件，請使用其他 API 或樹狀目錄周遊。 例如，您可以呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 以取得已知名稱的子元素，然後再呼叫 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739)。

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>在執行階段使用 XamlReader.Load 建立物件

XAML 也可以做為 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 方法的字串輸入，這個行為類似初始的 XAML 來源剖析作業。 **XamlReader.Load** 會在執行階段建立新的中斷連接的物件樹。 中斷連接的樹狀目錄可以附加到某個主要物件樹的某個點。 您必須明確地連接已建立的物件樹，可以透過將它新增到內容屬性集合 (像是 **Children**) 或設定其他接受物件值的屬性 (例如，為 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 屬性值載入新的 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101)) 來完成。

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>XamlReader.Load 的 XAML 命名範圍含意

[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 建立的新物件樹定義的初步 XAML 命名範圍，會評估在提供的 XAML 中為了唯一性而定義的任何名稱。 如果此時提供的 XAML 中有名稱在內部不是唯一的，**XamlReader.Load** 會產生例外狀況。 中斷連接的物件樹在連接到主應用程式物件樹時，不會嘗試合併它的 XAML 命名範圍與主應用程式的 XAML 命名範圍。 在連接樹狀目錄後，您的 app 會有一個統一的物件樹，但該樹狀目錄內有分離的 XAML 命名範圍。 分割會出現在物件之間的連接點，就是您設定某個屬性做為從 **XamlReader.Load** 呼叫傳回的值的位置。

含有分離與中斷連接的 XAML 命名範圍所造成的困難，是 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 方法的呼叫與直接管理的物件參考無法再用於統一的 XAML 命名範圍。 相反地，呼叫 **FindName** 的特定物件表示範圍，而該範圍就是呼叫物件所在的 XAML 命名範圍。 在直接管理的物件參考案例中，範圍是以程式碼存在的類別表示。 應用程式內容的「頁面」執行階段互動的程式碼後置，通常存在支援根「頁面」的部分類別中，因此 XAML 命名範圍是根 XAML 命名範圍。

如果您呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 以取得根 XAML 命名範圍中的具名物件，這個方法將找不到 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 建立的分離 XAML 命名範圍中的物件。 相反地，如果您從自分離的 XAML 命名範圍中取得的物件呼叫 **FindName**，這個方法將找不到根 XAML 命名範圍中的具名物件。

在使用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 呼叫時，這個分離的 XAML 命名範圍問題只會影響在 XAML 命名範圍中依名稱尋找物件。

如果要取得在其他 XAML 命名範圍中定義的物件參考，您可以使用下列幾種方法：

-   使用已知存在物件樹結構中的 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739) 和 (或) 集合屬性 (像是 [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514) 傳回的集合)，在個別的步驟中瀏覽整個樹狀目錄。
-   如果您是從分離的 XAML 命名範圍呼叫，並且想要根 XAML 命名範圍，取得目前顯示的主視窗的參考就很容易。 您可以使用一行含有呼叫 `Window.Current.Content` 的程式碼，就能從目前的應用程式視窗取得視覺化根目錄 (根 XAML 元素，也稱為內容來源)。 您可以接著轉換成 [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706)，然後從這個範圍呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
-   如果您是從根 XAML 命名範圍呼叫，並想要某個分離的 XAML 命名範圍中的物件，最佳做法是事先在您的程式碼中規劃，並保留 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 傳回並新增到主要物件樹的物件參考。 這個物件現在是可以在分離的 XAML 命名範圍內呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 的有效物件。 您可以讓這個物件做為全域變數，或使用方法參數以其他方式傳遞它。
-   您可以檢查視覺化樹狀目錄，以徹底避開名稱與 XAML 命名範圍考量。 [**VisualTreeHelper**](https://msdn.microsoft.com/library/windows/apps/br243038) API 可以讓您純粹依據位置與索引，以周遊視覺化樹狀目錄中的父物件與子集合。

## <a name="xaml-namescopes-in-templates"></a>範本中的 XAML 命名範圍

XAML 中的範本可以讓您直接重新使用和重新套用內容，但範本可能也會包含具有定義在範本層級的名稱的元素。 同一個範本可能會多次使用在一個頁面中。 因此，範本會定義自己的 XAML 命名範圍，與套用樣式或範本的包含頁面無關。 請參考下列範例：

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

在這個範例中，同一個範本套用到兩個不同的控制項中。 如果範本沒有分離的 XAML 命名範圍，範本中使用的 "MyTextBlock" 名稱會導致名稱衝突。 每個具現化範本都有自己的 XAML 命名範圍，因此，在這個範例中，每個具現化範本的 XAML 命名範圍都只會包含一個名稱。 不過，根 XAML 命名範圍不會包含任一範本中的名稱。

由於 XAML 命名範圍是獨立的，要從套用範本的頁面範圍尋找範本中的具名元素，需要使用不同的方法。 您必須先取得套用範本的物件，然後再呼叫 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)，而不是針對物件樹中的某個物件呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。 如果您是控制項作者，並且正在為套用的範本中的特定具名元素產生控制項本身定義的行為慣例，您可以使用控制項實作程式碼中的 **GetTemplateChild** 方法。 **GetTemplateChild** 方法受到保護，只有控制項作者才能存取。 此外，為命名組件與範本組件，並將這些報告為套用到控制項類別的屬性值，控制項作者也需要依循一些慣例。 這個方法讓想要套用不同範本的控制項使用者可以探索重要組件的名稱，並且需要取代具名組件才能維持控制項功能。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [x:Name 屬性](x-name-attribute.md)
* [快速入門：控制項範本](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)
* [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)
* [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)
 

