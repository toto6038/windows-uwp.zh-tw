---
description: 當您使用C#、Visual Basic 或 Visual C++ component extensions （C++/cx）做為程式設計語言，以及 UI 定義的 XAML 時，我們會說明 Windows 執行階段應用程式中事件的程式設計概念。
title: 事件和路由事件總覽
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 759e47348198feedbf7b1e3ee2c0bfc2da1da671
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690393"
---
# <a name="events-and-routed-events-overview"></a>事件和路由事件總覽

**重要 Api**
- [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

當您使用C#、Visual Basic 或 Visual C++ component extensions （C++/cx）做為程式設計語言，以及 UI 定義的 XAML 時，我們會說明 Windows 執行階段應用程式中事件的程式設計概念。 您可以在 XAML 中指派事件的處理常式做為 UI 元素的一部分，也可以在程式碼中加入處理常式。 Windows 執行階段支援*路由事件*：某些輸入事件和資料事件可以由引發事件之物件以外的物件處理。 當您定義控制項範本或使用頁面或版面配置容器時，路由事件很有用。

## <a name="events-as-a-programming-concept"></a>事件作為程式設計概念

一般而言，程式設計 Windows 執行階段應用程式時的事件概念，類似于最熱門程式設計語言中的事件模型。 如果您知道如何使用 Microsoft .NET 或C++活動，您有一首開始。 但是，您不需要知道要執行一些基本工作（例如附加處理常式）的事件模型概念。

當您使用C#、Visual Basic 或C++/cx 作為程式設計語言時，會在標記（XAML）中定義 UI。 在 XAML 標記語法中，某些在標記專案和執行時間程式碼實體之間連接事件的原則，與其他 Web 技術（例如 ASP.NET 或 HTML5）類似。

**請注意**  為 XAML 定義的 UI 提供執行時間邏輯的程式碼，通常稱為程式*代碼後*置或程式碼後置檔案。 在 [Microsoft Visual Studio 解決方案] 視圖中，此關聯性會以圖形方式顯示，而程式碼後置檔案則是相依和嵌套的檔案，與它所參考的 XAML 頁面。

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>按鈕。按一下：事件和 XAML 簡介

Windows 執行階段應用程式的其中一個最常見程式設計工作，就是將使用者輸入捕獲到 UI。 例如，您的 UI 可能會有使用者必須按一下才能提交資訊或變更狀態的按鈕。

您可以藉由產生 XAML 來定義 Windows 執行階段應用程式的 UI。 此 XAML 通常是 Visual Studio 中設計介面的輸出。 您也可以在純文字編輯器或協力廠商 XAML 編輯器中撰寫 XAML。 產生該 XAML 時，您可以在定義所有其他 XAML 屬性來建立該 UI 專案的屬性值時，同時連接個別 UI 元素的事件處理常式。

若要在 XAML 中連接事件，您可以指定您已定義之處理常式方法的字串格式名稱，或稍後將在程式碼後置中定義。 例如，此 XAML 會定義一個[**按鈕**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)物件，其中含有指派為屬性的其他屬性（[x：Name 屬性](x-name-attribute.md)、[**內容**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)），並藉由參考名為 `ShowUpdatesButton_Click`的方法，將按鈕的[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件的處理常式連線：

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**提示**  *事件配*線是一種程式設計的詞彙。 其參考的程式或程式碼，指出事件出現的位置應該叫用已命名的處理方法。 在大部分程式性程式碼模型中，事件配線是隱含或明確的 "AddHandler" 程式碼，它會同時命名事件和方法，而且通常牽涉到目標物件實例。 在 XAML 中，"AddHandler" 是隱含的，而事件配量完全是由將事件命名為物件元素的屬性名稱，並將處理常式命名為該屬性的值。

您可以使用程式設計語言撰寫實際的處理常式，以用於您所有的應用程式程式碼和程式碼後置。 在屬性 `Click="ShowUpdatesButton_Click"`中，您已建立一個合約，在 XAML 進行標記編譯和剖析時，您 IDE 的組建動作中的 XAML 標記編譯步驟和最終的 XAML 剖析會在應用程式載入時找到名為 `ShowUpdatesButton_Click` 的方法，做為應用程式共同的一部分取消. `ShowUpdatesButton_Click` 必須是一個方法，它會針對[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件的任何處理程式，執行相容的方法簽章（根據委派）。 例如，此程式碼會定義 `ShowUpdatesButton_Click` 處理常式。

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

在此範例中，`ShowUpdatesButton_Click` 方法是以[**RoutedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler)委派為基礎。 您知道這是要使用的委派，因為您會在[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)方法的語法中看到名為的委派。

**秘訣**  Visual Studio 提供便利的方式來命名事件處理常式，並在編輯 XAML 時定義處理常式方法。 當您在 XAML 文字編輯器中提供事件的屬性名稱時，請等候一段時間，直到 Microsoft IntelliSense 清單顯示為止。 如果您按一下清單中的 [ **&lt;新增事件處理常式&gt;** ]，Microsoft Visual Studio 將會根據專案的**x：Name** （或類型名稱）、事件名稱和數值尾碼來建議方法名稱。 接著，您可以用滑鼠右鍵按一下選取的事件處理常式名稱，然後按一下 [**流覽到事件處理常式**]。 這會直接流覽至新插入的事件處理常式定義，如 XAML 頁面的程式碼後置檔案的程式碼編輯器視圖中所示。 事件處理常式已經有正確的簽章，包括事件所*使用的傳送者參數和*事件資料類別。 此外，如果程式碼後置中已經有具有正確簽章的處理常式方法，該方法的名稱會與 **&lt;新事件處理常式&gt;** 選項一起出現在 [自動完成] 下拉式選單中。 您也可以按下 Tab 鍵做為快捷方式，而不是按一下 IntelliSense 清單專案。

## <a name="defining-an-event-handler"></a>定義事件處理常式

針對屬於 UI 元素並在 XAML 中宣告的物件，事件處理常式程式碼會定義在部分類別中，做為 XAML 頁面的程式碼後置。 事件處理常式是您在與 XAML 相關聯之部分類別中撰寫的方法。 這些事件處理常式是以特定事件所使用的委派為基礎。 您的事件處理常式方法可以是公用或私用。 私用存取的運作方式是因為 XAML 所建立的處理常式和實例最後會透過程式碼產生來聯結。 一般來說，我們建議您在類別中將事件處理常式方法設為私用。

**請注意**，不會C++在部分類別中定義  的事件處理常式，而是在標頭中宣告為私用類別成員。 C++專案的組建動作會負責產生支援 XAML 型別系統和程式碼後置模型的程式碼C++。

### <a name="the-sender-parameter-and-event-data"></a>*寄件者*參數和事件資料

您為事件撰寫的處理常式可以存取兩個值，以作為叫用處理程式之每個案例的輸入。 第一個這種值是*sender*，這是附加處理常式之物件的參考。 *Sender*參數的類型為基底**物件**類型。 常見的技巧*是將傳送者轉換成*更精確的類型。 如果您預期會在*寄件者*物件本身上檢查或變更狀態，這項技術會很有用。 根據您自己的應用程式設計，通常會根據處理常式的附加位置或其他設計細節，知道可以安全*地將傳送者轉換成*的類型。

第二個值是事件資料，通常會在語法定義中以*e*參數的形式出現。 您可以查看指派給所處理特定事件之委派的*e*參數，然後使用 Visual Studio 中的 IntelliSense 或物件瀏覽器，藉此探索哪些事件資料的屬性可用。 或者，您可以使用 Windows 執行階段的參考檔。

對於某些事件，事件資料的特定屬性值與瞭解事件發生的重要性一樣重要。 這特別適用于輸入事件。 對於指標事件，發生事件時指標的位置可能很重要。 對於鍵盤事件，所有可能的按鍵按下都會引發[**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)和[**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)事件。 若要判斷使用者所按的金鑰，您必須存取事件處理常式可用的[**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) 。 如需處理輸入事件的詳細資訊，請參閱[鍵盤互動](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)和[處理指標輸入](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input)。 輸入事件和輸入案例通常會有本主題未涵蓋的其他考慮，例如指標事件的指標捕捉，以及鍵盤事件的輔助按鍵和平臺按鍵碼。

### <a name="event-handlers-that-use-the-async-pattern"></a>使用**非同步**模式的事件處理常式

在某些情況下，您會想要使用在事件處理常式內使用**非同步**模式的 api。 例如，您可以在[**AppBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar)中使用[**按鈕**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)來顯示檔案選擇器並與其互動。 不過，許多檔案選擇器 Api 都是非同步。 它們必須在**非同步**/awaitable 範圍內呼叫，而且編譯器會強制執行此工作。 因此，您可以在事件處理常式中加入**async**關鍵字，讓處理常式現在是**非同步** **void**。 現在，您的事件處理常式允許進行**非同步**/awaitable 呼叫。

如需使用**非同步**模式進行使用者互動事件處理的範例，請參閱檔案[存取和](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10))選擇器（[ C#使用或 Visual Basic 系列建立您的第一個 Windows 執行階段應用程式](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10))的一部分）。 另請參閱 [以 C 呼叫非同步 Api]。

## <a name="adding-event-handlers-in-code"></a>在程式碼中加入事件處理常式

XAML 不是將事件處理常式指派給物件的唯一方法。 若要將事件處理常式加入至程式碼中的任何指定物件（包括 XAML 中無法使用的物件），您可以使用語言特定的語法來新增事件處理常式。

在C#中，語法是使用`+=`運算子。 您可以藉由參考運算子右邊的事件處理常式方法名稱來註冊處理常式。

如果您使用程式碼，將事件處理常式新增至出現在執行時間 UI 中的物件，常見的作法是新增這類處理常式來回應物件存留期事件或回呼（例如[**載入**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded)或[**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)），以便讓事件處理常式在相關物件在執行時間已準備好供使用者起始的事件之用。 這個範例會顯示頁面結構的 XAML 外框，然後提供將C#事件處理常式加入至物件的語言語法。

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

**請注意**  有更詳細的語法存在。 在2005中C# ，新增了名為委派推斷的功能，可讓編譯器推斷新的委派實例，並啟用上一個較簡單的語法。 Verbose 語法的功能與上一個範例相同，但在註冊之前會明確建立新的委派實例，因此不會利用委派推斷。 這個明確的語法較不常見，但是在某些程式碼範例中，您可能還是會看到它。

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Visual Basic 語法有兩種可能性。 其中一種是將C#語法平行處理，並直接附加至實例。 這需要**AddHandler**關鍵字，以及可引用處理程式方法名稱的**AddressOf**運算子。

Visual Basic 語法的另一個選項是在事件處理常式上使用**Handles**關鍵字。 這項技術適用于處理常式預期會在載入時存在於物件中，而且會在整個物件存留期間保存的情況。 在 XAML 中定義的物件上使用**控制碼**，需要您提供**名稱** / **x：Name**。 這個名稱會成為實例所需的實例限定詞。**控制碼**語法的*事件*部分。 在此情況下，您不需要以物件存留期為基礎的事件處理常式來起始附加其他事件處理常式;當您編譯 XAML 頁面時，會建立**控制碼**連接。

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**注意**  VISUAL STUDIO 及其 XAML 設計介面通常會升級實例處理技術，而不是**控制碼**關鍵字。 這是因為在 XAML 中建立事件處理常式配量是一般設計人員工作流程的一部分，而**控制碼**關鍵字技術與 XAML 中的事件處理常式串聯不相容。

在C++/cx 中，您也會使用 **+=** 語法，但基本C#形式的差異如下：

- 不存在任何委派推斷，因此您必須使用委派實例的**ref new** 。
- 委派的函式有兩個參數，而且需要目標物件做為第一個參數。 通常您會指定**此**。
- 委派的函式需要方法位址做為第二個參數，因此 **&** 參考運算子會在方法名稱之前。

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>在程式碼中移除事件處理常式

通常不需要在程式碼中移除事件處理常式，即使您已在程式碼中新增它們也一樣。 大部分 Windows 執行階段物件（例如頁面和控制項）的物件存留期行為，會在物件與主[**視窗**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window)及其視覺化樹狀結構中斷連接時損毀，而且任何委派參考也會終結。 .NET 會透過垃圾收集來執行這項C++處理，而/cx Windows 執行階段預設會使用弱式參考。

在某些罕見的情況下，您會想要明確地移除事件處理常式。 這些區域包括：

- 您為靜態事件新增的處理常式，無法以傳統方式進行垃圾收集。 Windows 執行階段 API 中的靜態事件範例是[**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget)和[**剪貼**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard)簿類別的事件。
- 測試程式碼，您想要立即移除處理常式，或是在執行時間將事件的舊/新事件處理常式交換至何處。
- 自訂**移除**存取子的執行。
- 自訂靜態事件。
- 頁面導覽的處理常式。

[**FrameworkElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded)或[**NavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom)是具有適當位置的狀態管理和物件存留期的可能事件觸發程式，因此您可以使用它們來移除其他事件的處理常式。

例如，您可以使用這段程式碼，從目標物件**textBlock1**中移除名為**textBlock1\_PointerEntered**的事件處理常式。

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

您也可以移除透過 XAML 屬性新增事件之案例的處理常式，這表示已在產生的程式碼中加入處理常式。 如果您提供已附加處理常式之專案的**名稱**值，這會更容易執行，因為這會為稍後的程式碼提供物件參考;不過，如果物件沒有**名稱**，您也可以逐步解說物件樹狀結構，以找出必要的物件參考。

如果您需要移除/Cx 中C++的事件處理常式，您將需要註冊權杖，您應該從`+=`事件處理常式註冊的傳回值收到該 token。 這是因為您在C++/cx 語法中，`-=` 取消註冊的右邊所用的值是 token，而不是方法名稱。 對於C++/cx，您無法移除已加入為 XAML 屬性的處理常式，因為C++/cx 產生的程式碼不會儲存權杖。

## <a name="routed-events"></a>路由事件

使用C#的 Windows 執行階段，Microsoft Visual Basic 或C++/cx 針對大部分 UI 元素上出現的一組事件，支援路由事件的概念。 這些事件適用于輸入和使用者互動案例，並會在[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)基類上執行。 以下是路由事件的輸入事件清單：

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**CoNtextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**CoNtextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**System.windows.dragdrop.dragenter>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**System.windows.dragdrop.dragover>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**下拉式**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**擁有**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**System.windows.uielement.manipulationcompleted>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**System.windows.uielement.manipulationdelta>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**System.windows.uielement.manipulationinertiastarting>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**System.windows.uielement.manipulationstarted>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**System.windows.uielement.manipulationstarting>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**System.windows.forms.control.previewkeydown>** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**點選**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

路由事件是一種事件，可能會從子物件傳入（*路由*）至物件樹狀結構中的每個後續父物件。 您 UI 的 XAML 結構接近此樹狀目錄，而該樹狀目錄的根是 XAML 中的根項目。 真正的物件樹狀結構可能與 XAML 元素的嵌套不同，因為物件樹狀結構不包含 XAML 語言功能，例如屬性專案標記。 您可以從引發事件的任何 XAML 物件專案子專案，向包含它的父物件元素，將路由事件想像為反*升*。 事件和其事件資料可以沿著事件路由在多個物件上處理。 如果沒有任何專案具有處理常式，則路由可能會繼續進行，直到達到根項目為止。

如果您知道像是動態 HTML （DHTML）或 HTML5 之類的 Web 技術，可能已經熟悉反*升*事件的概念。

當路由事件冒泡到其事件路由時，任何附加的事件處理常式都會存取事件資料的共用實例。 因此，如果有任何事件資料可由處理常式寫入，則對事件資料所做的任何變更將會傳遞至下一個處理常式，而且可能不再代表事件中的原始事件資料。 當事件有路由事件行為時，參考檔將會包含關於路由行為的備註或其他標記法。

### <a name="the-originalsource-property-of-routedeventargs"></a>**RoutedEventArgs**的**OriginalSource**屬性

當事件反升出事件路由*時，傳送者就不再是*與事件引發物件相同的物件。 相反地，*寄件者*是附加所叫用之處理常式的物件。

在某些情況下 *，傳送者並不*有趣，而且您會想要在引發指標事件時，將指標移到哪個可能的子物件，或是當使用者按下鍵盤按鍵時，具有較大 UI 中的物件。 在這些情況下，您可以使用[**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource)屬性的值。 在路由上的所有點上， **OriginalSource**會報告引發事件的原始物件，而不是附加處理常式的物件。 不過，對於[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)輸入事件，該原始物件通常是在頁面層級 UI 定義 XAML 中不會立即顯示的物件。 相反地，該原始來源物件可能是控制項的樣板化部分。 例如，如果使用者將指標停留在[**按鈕**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)的邊緣上方，則大部分指標事件的**OriginalSource**是[**範本**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template)中的[**框線**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)範本部分，而不是**按鈕**本身。

**提示**  輸入事件反升會特別適用于您要建立樣板化控制項的情況。 具有範本的任何控制項都可以有其取用者套用的新範本。 嘗試重新建立工作範本的取用者可能會意外地排除預設範本中宣告的某些事件處理。 您仍然可以在類別定義中附加處理常式做為[**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)覆寫的一部分，藉以提供控制層級的事件處理。 接著，您可以在具現化時攔截會反升至控制項根目錄的輸入事件。

### <a name="the-handled-property"></a>已**處理**的屬性

特定路由事件的數個事件資料類別包含名為「已**處理**」的屬性。 如需範例，請參閱[**PointerRoutedEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled)、 [**KeyRoutedEventArgs.** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled)已處理、 [**DragEventArgs**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled)。 在所有情況下，已**處理**的都是可設定的布林值屬性。

將已**處理**的屬性設定為**true**會影響事件系統行為。 當**處理**為**true**時，會停止大部分事件處理常式的路由;事件不會沿著路由繼續，以通知其他該特定事件案例的附加處理常式。 「已處理」的意義在於事件的內容，以及您的應用程式對您的回應程度。 基本上，「已**處理**」是簡單的通訊協定，可讓應用程式程式碼指出事件出現時不需要對任何容器進行反升，您的應用程式邏輯已負責處理所需的作業。 相反地，您必須注意，您不會處理可能應該進行反升的事件，讓內建的系統或控制行為能夠發揮作用。例如，處理部分或選取控制項專案內的低層級事件可能會造成損害。 選取控制項可能會尋找輸入事件，以得知選取範圍應變更。

並非所有路由事件都可以用這種方式取消路由，而且您可以告訴它，因為它們沒有已**處理**的屬性。 例如， [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)和[**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)會進行反升，但它們一律會反升至根，而其事件資料類別沒有可影響該行為的已**處理**屬性。

##  <a name="input-event-handlers-in-controls"></a>控制項中的輸入事件處理常式

特定 Windows 執行階段控制項有時會在內部針對輸入事件使用已**處理**的概念。 這可能會使它看起來像是永遠不會發生的輸入事件，因為您的使用者程式碼無法處理它。 例如，[**按鈕**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)類別包含故意處理一般輸入事件[**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)的邏輯。 它會這麼做，因為按鈕會引發按下指標的輸入所起始的[**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件，以及其他輸入模式，例如處理按鍵（如 Enter 鍵），而此按鍵會在焦點時叫用按鈕。 針對**按鈕**的類別設計，原始輸入事件會以概念方式處理，而使用者程式碼之類的類別取用者可以改與控制項相關的**Click**事件互動。 Windows 執行階段 API 參考中的特定控制項類別主題，通常會注意到該類別所執行的事件處理行為。 在某些情況下，您可以藉由**在**_事件_方法上覆寫來變更行為。 例如，您可以藉由覆寫[**Control.** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown)來變更[**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)衍生類別對索引鍵輸入的反應。

##  <a name="registering-handlers-for-already-handled-routed-events"></a>為已處理的路由事件註冊處理常式

稍早我們曾說過，將**處理**的設定設為**true** ，可防止呼叫大部分的處理常式。 但[**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)方法所提供的技術可讓您附加一律針對路由叫用的處理常式，即使之前在路由中的某個其他處理常式已在共用事件資料中**處理**為**true**也一樣。 如果您使用的控制項已處理其內部合成或控制項特定邏輯中的事件，這項技術會很有用。 但您仍想要從控制項實例或應用程式 UI 來回應它。 但請謹慎使用這項技巧，因為它可能會違反所**處理**的目的，而且可能會中斷控制項的預期互動。

只有具有對應之路由事件識別碼的路由事件可以使用[**addhandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)事件處理技術，因為此識別碼是**AddHandler**方法的必要輸入。 如需有路由事件識別碼可用的事件清單，請參閱[**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)的參考檔。 在大部分的情況下，這是我們稍早所示的路由事件清單。 例外狀況是清單中的最後兩個專案： [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)和[**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)沒有路由事件識別碼，因此您無法對這些專案使用**AddHandler** 。

## <a name="routed-events-outside-the-visual-tree"></a>視覺化樹狀結構外的路由事件

某些物件會參與與主要視覺化樹狀結構的關聯性，在概念上類似于主要視覺效果的重迭。 這些物件不是一般父子式關聯性的一部分，會將所有樹狀結構元素連接到視覺效果根。 這是[**任何顯示的**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)快顯或[**工具提示**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)的情況。 如果您想要從快**顯或** **工具提示**處理路由事件，請將處理常式放在快顯或**工具提示** **中的特定**UI 專案上，而**不是快顯或** **工具提示**元素本身。 請不要依賴針對快顯或**工具提示**內容所執行之任何組合**內的路由**。 這是因為路由事件的事件路由只會沿著主要視覺化樹狀結構運作。 快顯或**工具提示**不會視為子公司 UI 元素的父系，而且永遠不會收到路由事件，即使它正嘗試使用**像是快顯預設背景**之類的專案做為輸入事件的捕捉**區也一樣**。

## <a name="hit-testing-and-input-events"></a>點擊測試和輸入事件

判斷滑鼠、觸控和手寫筆輸入是否可以看到 UI 中的元素和位置，稱為*點擊測試*。 針對觸控動作，以及對觸控動作結果的互動特定或操作事件，必須讓元素進行點擊測試，才能成為事件來源，並引發與動作相關聯的事件。 否則，動作會將專案傳遞至視覺化樹狀結構中可與該輸入互動的任何基礎元素或父元素。 有數個因素會影響點擊測試，但是您可以藉由檢查[**system.windows.uielement.ishittestvisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible)屬性來判斷指定的元素是否可以引發輸入事件。 只有當元素符合下列準則時，這個屬性才會傳回**true** ：

- 元素的[**可見度**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility)屬性值為[**可見**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility)。
- 元素的**背景**或**填滿**屬性值不是**null**。 **Null** [**筆刷**](/uwp/api/Windows.UI.Xaml.Media.Brush)值會產生透明度和點擊測試隱藏。 （若要讓專案變成透明，但也可進行測試，請使用[**透明**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent)筆刷，而不是**null**）。

**注意：**   **背景**和**填滿**不是由[**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)定義，而是由不同的衍生類別（例如[**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)和[**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape)）所定義。 但是，您用於前景和背景屬性之筆刷的含意，對於點擊測試和輸入事件都是相同的，不論哪一個子類別會執行屬性。

- 如果元素是控制項，其[**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled)屬性值必須為**true**。
- 元素的配置中必須有實際的維度。 [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight)和[**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth)為0的元素不會引發輸入事件。

有些控制項具有用於點擊測試的特殊規則。 例如， [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)沒有**Background**屬性，但仍會在其維度的整個區域內達到可測試性。 [**影像**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)和[**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement)控制項會在其定義的矩形維度上進行點擊測試，而不論顯示的媒體來源檔案中的透明內容（例如 Alpha 色板）。 [**Web**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView)工作控制項有特殊的點擊測試行為，因為輸入可以由裝載的 HTML 和引發腳本事件來處理。

大部分的[**面板**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel)類別和[**框線**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border)不會在其本身的背景中進行點擊測試，但仍可處理從其包含的專案路由的使用者輸入事件。

您可以判斷哪些元素位於與使用者輸入事件相同的位置，而不論元素是否按下測試。 若要這麼做，請呼叫[**FindElementsInHostCoordinates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)方法。 顧名思義，這個方法會在相對於指定之主專案的位置尋找元素。 不過，套用的轉換和配置變更可以調整元素的相對座標系統，因此會影響在指定位置找到的元素。

## <a name="commanding"></a>命令

少數的 UI 元素支援*命令*。 命令會在其基礎的執行中使用輸入相關的路由事件，並藉由叫用單一命令處理常式來啟用相關 UI 輸入（特定指標動作，特定的快速鍵）的處理。 如果 UI 元素可以使用命令，請考慮使用其命令 Api，而不是任何離散的輸入事件。 您通常**會將系結參考用於**定義資料之視圖模型的類別屬性。 屬性會保存已命名的命令，以執行語言特定的**ICommand**命令模式。 如需詳細資訊，請參閱[**ButtonBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command)。

## <a name="custom-events-in-the-windows-runtime"></a>Windows 執行階段中的自訂事件

為了定義自訂事件，您新增事件的方式，以及對類別設計的意義，非常依賴您所使用的程式語言。

- 對於C#和 Visual Basic，您要定義 CLR 事件。 您可以使用標準 .NET 事件模式，只要您未使用自訂存取子（**新增**/**移除**）。 其他秘訣：
    - 對於事件處理常式而言，使用[**EventHandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1)是個不錯的主意，因為它有內建的轉譯可 Windows 執行階段一般事件委派[**EventHandler<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler)。
    - 請勿將您的事件資料類別做為您的[**系統 EventArgs**](https://docs.microsoft.com/dotnet/api/system.eventargs) ，因為它不會轉譯為 Windows 執行階段。 請使用現有的事件資料類別或完全沒有基類。
    - 如果您使用的是自訂存取子，請參閱[Windows 執行階段元件中的自訂事件和事件存取](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140))子。
    - 如果您不清楚標準 .NET 事件模式是什麼，請參閱定義[自訂 Silverlight 類別的事件](https://docs.microsoft.com/previous-versions/windows/)。 這是針對 Microsoft Silverlight 所撰寫，但它仍然是標準 .NET 事件模式的程式碼和概念的絕佳總和。
- 若C++為/cx，請參閱[事件（C++/cx）](https://docs.microsoft.com/cpp/cppcx/events-c-cx)。
    - 即使是您自己的自訂事件用法，也請使用已命名的參考。 請勿將 lambda 用於自訂事件，它可以建立迴圈參考。

您無法為 Windows 執行階段宣告自訂路由事件。路由事件僅限於來自 Windows 執行階段的集合。

定義自訂事件通常是在定義自訂控制項的練習過程中完成。 這是一個常見的模式，其中具有具有屬性變更回呼的相依性屬性，而且也會定義在某些或所有案例中，由相依性屬性回呼所引發的自訂事件。 您的控制項取用者無法存取您所定義的屬性變更回呼，但提供通知事件是下一個最佳做法。 如需詳細資訊，請參閱[自訂](custom-dependency-properties.md)相依性屬性。

## <a name="related-topics"></a>相關主題

* [XAML 總覽](xaml-overview.md)
* [快速入門：觸控輸入](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [鍵盤互動](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [.NET 事件和委派](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [建立 Windows 執行階段元件](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
