---
author: jwmsft
description: 我們將描述使用 C#、Visual Basic 或 Visual C++ 元件延伸 (C++/CX) 做為程式設計語言，並使用 XAML 定義 UI 時，Windows 執行階段 app 中之事件的程式設計概念。
title: 事件與路由事件概觀
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.author: jimwalk
ms.date: 07/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6ca58613a5874cde10d2bb5322c3f930e1fbce44
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052554"
---
# <a name="events-and-routed-events-overview"></a>事件與路由事件概觀

**重要 API**
-   [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)
-   [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)

我們將描述使用 C#、Visual Basic 或 Visual C++ 元件延伸 (C++/CX) 做為程式設計語言，並使用 XAML 定義 UI 時，Windows 執行階段 app 中之事件的程式設計概念。 您可以在 XAML 中指派事件的處理常式，以做為 UI 元素宣告的一部分，或是在程式碼中新增處理常式。 Windows 執行階段支援*路由事件*：某些輸入事件與資料事件，可以由引發事件之物件以外的物件來處理。 當您定義控制項範本或是使用頁面或配置容器時，路由事件非常實用。

## <a name="events-as-a-programming-concept"></a>程式設計概念的事件

一般而言，進行 Windows 執行階段應用程式的程式設計時，所使用的事件概念與最受歡迎的程式設計語言中的事件模型類似。 如果您已經知道如何使用 Microsoft .NET 或 C++ 事件，就能輕易理解。 然而，您只需對事件模型概念有基本的認識，就能夠執行一些基本的工作，像是附加處理常式。

當您使用 C#、Visual Basic 或 C++/CX 做為程式設計語言時，UI 會定義在標記 (XAML) 中。 就 XAML 標記語法而言，在標記元素與執行階段程式碼實體間之連接事件的一些原理，與其他 Web 技術 (像是 ASP.NET 或 HTML5) 類似。

**注意**  為 XAML 定義的 UI 提供執行階段邏輯的程式碼，通常稱為*程式碼後置*或程式碼後置檔案。 在 Microsoft Visual Studio 方案檢視中，會以圖形顯示這個關係，而程式碼後置檔案對它所參考的 XAML 頁面而言，是相依與巢狀的檔案。

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click：事件與 XAML 的簡介

Windows 執行階段應用程式最常見的一項程式設計工作，是將使用者輸入擷取到 UI。 例如，您的 UI 可能包含按鈕，使用者必須按下按鈕才能送出資訊或變更狀態。

您是透過產生 XAML 的方式定義 Windows 執行階段應用程式的 UI。 此 XAML 通常是 Visual Studio 設計介面的輸出。 您也可以在純文字編輯器或其他廠商的 XAML 編輯器中撰寫 XAML。 在產生該 XAML 的過程中，您可以連接個別 UI 元素的事件處理常式，同時定義建立該 UI 元素的屬性值的其他所有 XAML 屬性。

若要在 XAML 中連接事件，請指定您已定義或稍後將在程式碼後置中定義之處理常式方法的字串格式名稱。 例如，下列 XAML 定義一個將其他屬性 ([x:Name 屬性](x-name-attribute.md)、[**Content**](https://msdn.microsoft.com/library/windows/apps/br209366)) 指派為屬性的 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 物件，並參考名為 `ShowUpdatesButton_Click` 的方法來連接按鈕的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件的處理常式：

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**提示**  *事件連接*是一個程式設計術語。 它指的是您在表示發生某個事件時應叫用具名處理常式方法的處理程序或程式碼。 在大部分的程序性程式碼模型中，事件連接是隱含或明確的 "AddHandler" 程式碼，可以為事件和方法命名，通常包含目標物件執行個體。 在 XAML 中，"AddHandler" 是隱含的，而事件連接完全是由下列兩個動作所組成：將事件命名為物件元素的屬性名稱，以及將處理常式命名為該屬性的值。

您是以用來撰寫所有 app 之程式碼和程式碼後置的程式設計語言，來撰寫實際的處理常式。 使用屬性 `Click="ShowUpdatesButton_Click"` 會建立一個協定，也就是當 XAML 是以標記編譯和剖析時，您的 IDE 建置動作的 XAML 標記編譯步驟和應用程式載入時的最終 XAML 剖析，都可以在應用程式的程式碼中找到名為 `ShowUpdatesButton_Click` 的方法。 `ShowUpdatesButton_Click` [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件的任何處理常式實作相容之方法簽章 (依據委派) 的方法。 例如，下列程式碼會定義 `ShowUpdatesButton_Click` 處理常式。

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

在此範例中，`ShowUpdatesButton_Click` 方法是以 [**RoutedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br208812) 委派為基礎。 您會知道這就是要使用的委派，因為 MSDN 參考頁面上 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 方法的語法中將會提及該委派。

**提示**  Visual Studio 在您編輯 XAML 時，會提供一個命名事件處理常式並定義處理常式方法的便利方式。 當您在 XAML 文字編輯器中提供事件的屬性名稱時，請稍候片刻，讓 Microsoft IntelliSense 清單顯示。 如果您按一下清單中的 **\[&lt;新事件處理常式&gt;\]**，Microsoft Visual Studio 會根據元素的 **x:Name** (或類型名稱)、事件名稱以及數值尾碼來建議方法名稱。 然後，您可以在選取的事件處理常式名稱上按一下滑鼠右鍵，再按一下 [**巡覽至事件處理常式**]。 這會直接瀏覽到新插入的事件處理常式定義，如您在 XAML 頁面程式碼後置檔案的程式碼編輯器檢視中所見。 事件處理常式已經有正確的簽章，其中包含事件使用的 *sender* 參數及事件資料類別。 此外，如果您的程式碼後置中已經有正確簽章的處理常式方法，這個方法的名稱會顯示在自動完成下拉式清單中，連同顯示 **\[&lt;新事件處理常式&gt;\]** 選項。 您也可以按下 Tab 鍵做為快速鍵，以取代按一下 IntelliSense 清單項目。

## <a name="defining-an-event-handler"></a>定義事件處理常式

對於在 XAML 中宣告的 UI 元素物件，事件處理常式程式碼是定義在做為 XAML 頁面程式碼後置的部分類別中。 事件處理常式是您撰寫為部分類別中一部分的方法，這個部分類別與您的 XAML 相關聯。 這些事件處理常式是根據特定事件使用的委派。 您的事件處理常式方法可以是公用或私用的。 使用私用存取的原因，是因為程式碼產生最終會聯結 XAML 建立的處理常式與執行個體。 一般而言，建議您在類別中將事件處理常式方法建立為私用的。

**注意**  C++ 的事件處理常式不會在部分類別中定義，它們會在標頭中宣告為私用類別成員。 C++ 專案的建置動作會負責處理程式碼的產生，而這些程式碼可支援 C++ 的 XAML 類型系統和程式碼後置模型。

### <a name="the-sender-parameter-and-event-data"></a>*sender* 參數與事件資料

您為事件撰寫的處理常式可以存取兩個值，這兩個值可做為您的處理常式被叫用時的輸入使用。 第一個值是 *sender*，為針對附加處理常式之物件的參考。 *sender* 參數的類型是基底 **Object** 類型。 常見的技巧是將 *sender* 轉換成更精確的類型。 如果您想在 *sender* 物件本身上進行檢查或變更狀態，這個技巧就很有用。 依據您 app 的設計，您通常可以依據處理程式的附加位置或是其他設計細節，來了解是否可以安全地將 *sender* 轉換成某個類型。

第二個值是事件資料，通常以 *e* 參數的形式顯示在語法定義中。 您可以查看指派給您正在處理的特定事件之委派的 *e* 參數，然後在 Visual Studio 中使用 IntelliSense 或物件瀏覽器，以探索可用的事件資料屬性。 或者，您可以使用 Windows 執行階段參考文件。

對某些事件來說，事件資料的特定屬性值與知道事件發生一樣重要。 輸入事件更是如此。 對指標事件來說，事件發生時的指標位置可能很重要。 對鍵盤事件來說，所有可能的按鍵動作都會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 和 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 為了判斷使用者按下哪個按鍵，您必須存取可供事件處理常式存取的 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)。 如需處理輸入事件的詳細資訊，請參閱[鍵盤互動](https://msdn.microsoft.com/library/windows/apps/mt185607)和[處理指標輸入](https://msdn.microsoft.com/library/windows/apps/mt404610)。 輸入事件與輸入案例通常有本主題未涵蓋的其他考量，像是指標事件的指標擷取，以及鍵盤事件的輔助按鍵與平台按鍵程式碼。

### <a name="event-handlers-that-use-the-async-pattern"></a>使用 **async** 模式的事件處理常式

在某些情況下，您會想要在事件處理常式內使用運用 **async** 模式的 API。 例如，您可能會在 [**AppBar**](https://msdn.microsoft.com/library/windows/apps/hh701927) 中使用 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 來顯示檔案選擇器並與其互動。 不過，許多檔案選擇器 API 都是非同步的。 它們必須在 **async**/awaitable 範圍內呼叫，而且編譯器會強制這項條件。 因此，您可以新增 **async** 關鍵字到您的事件處理常式，來使處理常式成為 **async** **void**。 現在，您的事件處理常式已可做出 **async**/awaitable 呼叫。

如需使用 **async** 模式的使用者互動事件處理範例，請參閱[檔案存取和選擇器](https://msdn.microsoft.com/library/windows/apps/jj655411) ([使用 C# 或 Visual Basic 建立您的第一個 Windows 執行階段應用程式](https://msdn.microsoft.com/library/windows/apps/hh974581)系列中的一部分)。 另請參閱 [在 C 中呼叫非同步 API]。

## <a name="adding-event-handlers-in-code"></a>在程式碼中新增事件處理常式

XAML 不是將事件處理常式指派給物件的唯一方法。 如果要將事件處理常式新增到程式碼中的任何指定物件 (包含無法在 XAML 中使用的物件)，您可以使用語言特定的語法來新增事件處理常式。

在 C# 中的語法是使用 `+=` 運算子。 您要透過參考運算子右邊的事件處理常式方法名稱來登錄處理常式。

如果您使用程式碼新增事件處理常式到顯示在執行階段 UI 中的物件，常見的做法是新增這類處理常式以回應物件存留期事件或回呼，例如 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 或 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737)，這樣的話，相關物件的事件處理常式就會為使用者在執行階段起始的事件做好準備。 此範例說明頁面結構的 XAML 大綱，然後提供可將事件處理常式新增到物件的 C# 語言語法。

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**注意**  還有更詳細的語法。 在 2005 年時，C# 新增一項稱為委派推斷的功能，可以讓編譯器推斷新的委派執行個體，並啟用更簡單的舊語法。 詳細語法在功能上與上一個範例相同，但會在登錄之前先明確地建立新的委派執行個體，因此不會利用委派推斷。 這個明確的語法較不常見，但是在有些程式碼範例中還是可能會看到它。

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 語法有兩種可能性。 一種會呼應 C# 語法，並直接將處理常式附加到執行個體。 這種方法需要 **AddHandler** 關鍵字，也需要解除參照處理常式方法名稱的 **AddressOf** 運算子。

Visual Basic 語法的另一種選項，是在事件處理常式使用 **Handles** 關鍵字。 若預期處理常式於載入期間會存在於物件上，並於整個物件存留期中持續存在，便適合使用這個技巧。 若要在 XAML 中定義的物件上使用 **Handles**，您必須提供 **Name** / **x:Name**。 這個名稱會成為 **Handles** 語法之一部分的 *Instance.Event* 所需要的執行個體限定詞。 在這個案例中，您不需要物件存留期型的事件處理常式，就能夠開始附加其他事件處理常式；當您編譯 XAML 頁面時，就會建立 **Handles** 連線。

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**注意**  Visual Studio 和它的 XAML 設計介面通常會升級執行個體處理技巧，而非 **Handles** 關鍵字。 這是因為在 XAML 中建立事件處理常式連接，是設計人員與開發人員之間一般工作流程的一部分，而 **Handles** 關鍵字技巧與連接 XAML 中的事件處理常式不相容。

在 C + + /CX，您也使用**+=** 語法，但與基本 C# 格式有一些差異：

-   不具備委派推斷功能，因此必須針對委派執行個體使用 **ref new**。
-   委派建構函式有兩個參數，並且需要以目標物件做為第一個參數。 通常您是指定**這個**。
-   委派建構函式需以方法位址做為第二個參數，因此 **&** 參考運算子必須位於方法名稱之前。

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>移除程式碼中的事件處理常式

通常，即使您在程式碼中加入事件處理常式，也沒有必要將該事件處理常式從程式碼移除。 在物件與主要 [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 及其視覺化樹狀結構中斷連線之後，大多數 Windows 執行階段物件 (例如頁面和控制項) 的物件存留期行為會將物件摧毀，也會一併摧毀所有委派參考。 .NET 會透過記憶體回收執行這項工作，而搭配 C++/CX 的 Windows 執行階段則是預設使用弱式參照。

在某些罕見的情況下，您確實會想要明確移除事件處理常式。 其中包括：

-   您為靜態事件新增的處理常式，這些是無法以常規方式進行記憶體回收的處理常式。 Windows 執行階段 API 中的靜態事件範例就是 [**CompositionTarget**](https://msdn.microsoft.com/library/windows/apps/br228126) 和 [**Clipboard**](https://msdn.microsoft.com/library/windows/apps/br205867) 類別的事件。
-   將處理常式移除時間設為立即的測試程式碼，或是在執行階段交換事件的新/舊事件處理常式的程式碼。
-   自訂 **remove** 存取子的實作。
-   自訂靜態事件。
-   適用於頁面瀏覽的處理常式。

[**FrameworkElement.Unloaded**](https://msdn.microsoft.com/library/windows/apps/br208748) 或 [**Page.NavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 是在狀態管理中擁有適當位置和物件存留期的事件觸發程序，您可以使用這些事件觸發程序來移除其他事件的處理常式。

例如，您可以使用這個程式碼將名稱為 **textBlock1\_PointerEntered** 的事件處理常式從目標物件 **textBlock1** 中移除。

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

當事件是透過 XAML 屬性新增的情況下，也就是說當處理常式是在產生的程式碼中新增的情況下，您也可以移除處理常式。 如果您為處理常式所附加的元素提供了 **Name** 值，在執行上就會比較容易，因為那會在稍後為程式碼提供一個物件參照；不過，在物件沒有 **Name** 的情況下，您也可以瀏覽物件樹狀結構來找出所需的物件參照。

如果您需要移除 C++/CX 中的事件處理常式，您將需要一個註冊 Token，而您應該已經從 `+=` 事件處理常式註冊的傳回值收到這個權杖 Token。 那是因為您在 C++/CX 語法中用於 `-=` 取消註冊右邊的值就是該 Token，不是方法名稱。 以 C++/CX 來說，您不能移除新增為 XAML 屬性的處理常式，因為 C++/CX 產生的程式碼不會儲存 Token。

## <a name="routed-events"></a>路由事件

搭配 C#、Microsoft Visual Basic 或 C++/CX 的 Windows 執行階段可以針對大多數 UI 元素上的一組事件，支援路由事件的概念。 這些事件用於輸入與使用者互動案例，並且會在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 基底類型中實作。 下列是路由事件的輸入事件清單：

-   [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922)
-   [**DragEnter**](https://msdn.microsoft.com/library/windows/apps/br208923)
-   [**DragLeave**](https://msdn.microsoft.com/library/windows/apps/br208924)
-   [**DragOver**](https://msdn.microsoft.com/library/windows/apps/br208925)
-   [**Drop**](https://msdn.microsoft.com/library/windows/apps/br208926)
-   [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)
-   [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945)
-   [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946)
-   [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947)
-   [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)
-   [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951)
-   [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)
-   [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)
-   [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)
-   [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)
-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)
-   [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)
-   [**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984)
-   [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)
-   [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927)
-   [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943)

路由事件是可能由子物件傳遞 (*路由*) 到它在物件樹狀結構中之每個後續父物件的事件。 UI 的 XAML 結構接近這個物件樹，它的根是 XAML 中的根元素。 實際的物件樹狀結構可能與 XAML 元素巢狀結構有些不同，因為物件樹狀結構未包含 XAML 語言功能，像是屬性元素標記。 您可以將路由事件想像成引發事件的任一 XAML 物件元素子元素 (針對包含它的父物件元素所引發) 中的「事件反昇」**(Bubbling)。 事件與其事件資料可沿著事件路由在多個物件中處理。 如果元素未具備處理常式，路由可能會持續進行，直到到達根元素為止。

如果您了解動態 HTML (DHTML) 或 HTML5 之類的 Web 技術，可能已經熟知「事件反昇」**(Bubbling) 的事件概念。

當路由事件經由它的事件路由反昇時，任何附加的事件處理常式都會存取事件資料的共用執行個體。 因此，如果某個處理常式可以寫入任何事件資料，對事件資料所做的任何變更將會傳遞給下一個處理常式，因此可能不再是該事件中的原始事件資料。 當某個事件具有路由事件行為時，參考文件將會包含關於該路由行為的備註或其他附註。

### <a name="the-originalsource-property-of-routedeventargs"></a>**RoutedEventArgs** 的 **OriginalSource** 屬性

當某個事件沿著事件路徑向上反昇時，*sender* 和事件引發物件已不再是相同的物件。 *sender* 將變成是附加叫用之處理常式的物件。

在某些情況下，*sender* 不是您感興趣的目標，您反而是想了解其他資訊，像是指標事件觸發時，指標在哪個可能的子物件上方，或當使用者按下鍵盤按鍵時，較大 UI 中的哪個物件是焦點。 在這些情況下，您可以使用 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 屬性的值。 在路由的所有點中，**OriginalSource** 會報告引發事件的原始物件，而非附加處理常式的物件。 不過，對於 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 輸入事件來說，該原始物件通常是無法立即在頁面層級的 UI 定義 XAML 中所能看到的物件。 該原始來源物件可能是控制項的範本組件。 例如，如果使用者將滑鼠指標停留在 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的邊緣，對於大多數的指標事件來說，**OriginalSource** 是 [**Template**](https://msdn.microsoft.com/library/windows/apps/br209465) 中的 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 範本組件，而非 **Button** 本身。

**提示**  如果您正在建立範本化的控制項，輸入事件將會特別有用。 控制項的取用者可以為含有範本的任一控制項套用新範本。 正嘗試重建工作範本的取用者，可能會不慎刪除在預設範本中宣告的某些事件處理。 您仍然可以提供控制項層級的事件處理，方法是將處理常式附加為類別定義中的 [**OnApplyTemplate**](https://msdn.microsoft.com/library/windows/apps/br208737) 覆寫的一部分。 接著，您可以攔截具現化時向上反昇至控制項根目錄的輸入事件。

### <a name="the-handled-property"></a>**Handled** 屬性

特定路由事件的數種事件資料類別包含一個名為 **Handled** 的屬性。 例如，請參閱 [**PointerRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943079)、[**KeyRoutedEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 以及 [**DragEventArgs.Handled**](https://msdn.microsoft.com/library/windows/apps/br242375)。 在所有情況下，**Handled** 都是可設定的布林值屬性。

將 **Handled** 屬性設為 **true** 會影響事件系統行為。 當 **Handled** 為 **true** 時，大多數事件處理常式的路由會停止；事件不會繼續沿著路由向其他附加的處理常式通知該特定事件案例。 「handled」在事件內容中的意義，以及您的 app 會如何回應它，將由您決定。 **Handled** 基本上是簡單的通訊協定，可讓 app 程式碼表明某個事件的發生不需反昇至任何容器，您的 app 邏輯已處理需完成的工作。 但相反地，您必須小心自己是否沒有處理到應該反昇的事件，導致內建的系統或控制項行為無法作用。例如，處理選取控制項的組件或項目內的低層級事件可能會有不良影響。 選取控制項可能會尋找輸入事件，以確認選取項目應變更。

並非所有路由事件都能以這種方式取消路由，而您也可以依據它們沒有 **Handled** 屬性來分辨。 例如，[**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 與 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 會反昇，但它們永遠會一路反昇至根目錄，而它們的事件資料類別並沒有能夠影響該行為的 **Handled** 屬性。

##  <a name="input-event-handlers-in-controls"></a>控制項中的輸入事件處理常式

特定的 Windows 執行階段控制項有時會在內部對輸入事件使用 **Handled** 概念。 這樣可以使輸入事件看似從未發生，因為您的使用者程式碼無法處理它。 例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 類別包含會刻意處理一般輸入事件 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 的邏輯。 它會這麼做的原因，是因為按鈕會引發由 pointer-pressed 輸入與其他輸入模式 (例如可以在身為焦點時叫用按鈕的處理按鍵，如 ENTER 鍵) 起始的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 針對 **Button** 類別設計的目的，原始輸入事件將會以概念方式處理，而類別取用者 (例如您的使用者程式碼) 可改為與控制項相關的 **Click** 事件互動。 Windows 執行階段 API 參考資料中特定控制項類別的主題，通常會提到類別會實作的事件處理行為。 在某些情況下，您可以透過覆寫 **On***Event* 方法來變更行為。 例如，您可以覆寫 [**Control.OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982)，以變更 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 衍生類別回應按鍵輸入的方式。

##  <a name="registering-handlers-for-already-handled-routed-events"></a>登錄已處理之路由事件的處理常式

前文曾經提到，將 **Handled** 設為 **true** 可以避免呼叫大多數的處理常式。 不過，[**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 方法能提供一個技巧，即使路由中的部分其他處理常式已經在共用事件資料中將 **Handled** 設為 **true**，您還是可以為路由附加永遠會被叫用的處理常式。 如果您使用的控制項已透過在其內部結合來處理該事件，或是針對控制項特定的邏輯，這個技巧就很有用。 但您仍然想從控制項執行個體或您的 app UI 對它做出回應。 不過使用這個技巧時請小心，因為它可能會與 **Handled** 的目的衝突，並且可能會中斷控制項的目標互動。

只有包含對應的路由事件識別碼的路由事件可以使用 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 事件處理方法技術，因為識別碼是 **AddHandler** 方法的必要輸入。 請參閱 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 的參考文件，以取得含有可用路由事件識別碼的事件清單。 這與我們之前說明的路由事件清單大致相同。 例外的是清單中的最後兩個項目：[**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 沒有路由事件識別碼，因此您不能將 **AddHandler** 用於這兩個事件。

## <a name="routed-events-outside-the-visual-tree"></a>視覺化樹狀結構以外的路由事件

某些物件會參與和主要視覺化樹狀結構的關係，在概念上像是重疊了主要視覺化樹狀結構。 這些物件不是將所有樹狀結構元素與視覺化根目錄連接的一般父系-子系關係的一部分。 任何顯示的 [**Popup**](https://msdn.microsoft.com/library/windows/apps/br227842) 或 [**ToolTip**](https://msdn.microsoft.com/library/windows/apps/br227608) 皆為這種案例。 如果您想處理 **Popup** 或 **ToolTip** 中的路由事件，請在 **Popup** 或 **ToolTip** 內的特定 UI 元素中放置處理常式，而非在 **Popup** 或 **ToolTip** 元素本身。 不要仰賴針對 **Popup** 或 **ToolTip** 內容執行之任何結合內的路由。 這是因為路由事件的事件路由只會沿著主要的視覺化樹狀結構進行。 **Popup** 或 **ToolTip** 不會被視為附屬 UI 元素的父系，並且永遠不會接收路由事件，即使它嘗試使用像是 **Popup** 之類的預設背景做為輸入事件的擷取區域也一樣。

## <a name="hit-testing-and-input-events"></a>點擊測試和輸入事件

判斷滑鼠、觸控以及手寫筆輸入是否能夠在 UI 中看見元素以及在何處看見的動作，稱為「點擊測試」**。 對於觸控動作以及因為觸控動作而引發的互動特定或操作事件，元素必須具有點擊測試可見性，才能成為事件來源並引發與動作相關聯的事件。 否則，動作會透過這個元素傳送至視覺化樹狀結構中可與該輸入進行互動的任何基礎元素或父項元素。 影響點擊測試的因素有很多，不過您可以檢查元素的 [**IsHitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 屬性，判斷指定的元素是否會引發輸入事件。 這個屬性只在元素符合以下條件時才會傳回 **true**：

-   元素的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 屬性值是 [**Visible**](https://msdn.microsoft.com/library/windows/apps/br209006)。
-   元素的 **Background** 或 **Fill** 屬性值不是 **null**。 **null** [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 值會導致透明而看不到點擊測試。 (若要讓元素變成透明但仍可以進行點擊測試，請使用 [**Transparent**](https://msdn.microsoft.com/library/windows/apps/hh748061) 筆刷而不要使用 **null**)。

**注意**  **Background** 和 **Fill** 不是由 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 定義的，而是由不同的衍生類別 (如 [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) 和 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)) 定義的。 不過，您為前景和背景屬性使用的筆刷含意，與點擊測試及輸入事件是相同的，無論該屬性是由哪個子類別實作。

-   如果元素是控制項，它的 [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/br209419) 屬性值必須是 **true**。
-   元素在配置中必須具有實際的尺寸。 [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) 和 [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) 為 0 的元素不會引發輸入事件。

部分控制項擁有特殊的點擊測試規則。 例如，[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 沒有 **Background** 屬性，但是在其尺寸的整個區域內仍然可以進行點擊測試。 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 和 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 控制項在它們的定義矩形尺寸中可以進行點擊測試，無論媒體來源檔案中的透明內容 (如 alpha 通道) 是否顯示。 [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 控制項具有特殊點擊測試行為，因為輸入可由裝載 HTML 處理並產生指令碼事件。

大部分的 [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) 類別和 [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) 在自己的背景中無法進行點擊測試，但是仍然可以處理從它們包含之元素所路由的使用者輸入事件。

無論元素是否可以進行點擊測試，您都可以判斷哪些元素與使用者輸入事件位於相同的位置。 若要這樣做，請呼叫 [**FindElementsInHostCoordinates**](https://msdn.microsoft.com/library/windows/apps/br243039) 方法。 如名稱所示，這個方法會在與指定主機元素相關的位置尋找元素。 不過，套用的轉換和配置變更會調整元素的相對座標系統，進而影響在指定位置找到的元素。

## <a name="commanding"></a>命令

少數 UI 元素支援命令功能**。 命令功能會在它的基礎實作中使用輸入相關的路由事件，並透過叫用單一命令處理常式來處理相關 UI 輸入 (某種指標動作，特定的快速鍵)。 如果某個 UI 元素擁有命令功能，請考慮使用它的命令 API，而非任何特定輸入事件。 您通常使用 **Binding** 參照到某個定義資料檢視模型的類別屬性。 這些屬性擁有具名命令，會實作語言特定的 **ICommand** 命令模式。 如需詳細資訊，請參閱 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)。

## <a name="custom-events-in-the-windows-runtime"></a>Windows 執行階段中的自訂事件

在定義自訂事件的目的上，您新增事件的方式及對您類別設計的意義，與您所使用的程式設計語言息息相關。

-   對於 C# 和 Visual Basic，您定義的是 CLR 事件。 只要您不是使用自訂存取子 (**add**/**remove**)，您就可以使用標準的 .NET 事件模式。 其他提示：
    -   對事件處理常式來說，使用 [**System.EventHandler<TEventArgs>**](https://msdn.microsoft.com/library/windows/apps/xaml/db0etb8x.aspx) 是一個相當好的方式，因為它具有內建的 Windows 執行階段泛型事件委派 [**EventHandler<T>**](https://msdn.microsoft.com/library/windows/apps/br206577) 轉譯。
    -   請勿以 [**System.EventArgs**](https://msdn.microsoft.com/library/windows/apps/xaml/system.eventargs.aspx) 做為您事件資料類別的基底，因為它不會轉譯成 Windows 執行階段。 使用現有的事件資料類別或完全不使用基底類別。
    -   如果您使用自訂存取子，請參閱 [Windows 執行階段元件中的自訂事件和事件存取子](https://msdn.microsoft.com/library/windows/apps/xaml/hh972883.aspx)。
    -   如果您不清楚什麼是標準 .NET 事件模式，請參閱[定義自訂 Silverlight 類別的事件](http://msdn.microsoft.com/library/dd833067.aspx)。 這是針對 Microsoft Silverlight 撰寫的，但是對於標準 .NET 事件模式仍提供了良好的程式碼及概念的總和。
-   如需 C++/CX 的相關資訊，請參閱[事件 (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh755799.aspx)。
    -   使用具名參照，即使是在您自己的自訂事件使用方式中也一樣。 請勿將 Lambda 用於自訂事件，它會建立循環參照。

您無法為 Windows 執行階段宣告自訂路由事件；路由事件受限於來自 Windows 執行階段的集合。

定義自訂事件通常是在練習定義自訂控制項的過程中一併完成的。 擁有一個含有屬性變更回呼的相依性屬性，並且也定義一個在某些或所有情況下會由相依性屬性回呼引發的自訂事件，是常見的模式。 您控制項的取用者無法存取您定義的屬性變更回呼，但是提供一個通知事件則是最佳的下一步。 如需詳細資訊，請參閱[自訂相依性屬性](custom-dependency-properties.md)。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [快速入門：觸控輸入](https://msdn.microsoft.com/library/windows/apps/xaml/hh465387)
* [鍵盤互動](https://msdn.microsoft.com/library/windows/apps/mt185607)
* [.NET 事件與委派](http://go.microsoft.com/fwlink/p/?linkid=214364)
* [建立 Windows 執行階段元件](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399)
