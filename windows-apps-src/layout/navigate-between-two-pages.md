---
author: Jwmsft
Description: "了解如何在通用 Windows 平台 (UWP) app 的基本兩個對等頁面中瀏覽。"
title: "兩個頁面之間的對等瀏覽"
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: 324bdcd8ae61826a2be765f6a6a93441036d6984

---

# <a name="peer-to-peer-navigation-between-two-pages"></a>兩個頁面之間的對等瀏覽

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

了解如何在通用 Windows 平台 (UWP) app 的基本兩個對等頁面中瀏覽。

![兩個頁面對等瀏覽範例](images/nav-peertopeer-2page.png)

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)</li>
<li>[**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)</li>
<li>[**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)</li>
</ul>
</div>



## <a name="create-the-blank-app"></a>建立空白 App


1.  在 Microsoft Visual Studio 功能表，選擇 [檔案] &gt; [新增專案]****。
2.  在 [新增專案]**** 對話方塊的左窗格中，選擇 [Visual C#] -&gt; [Windows] -&gt; [通用]**** 或 [Visual C++] -&gt; [Windows] -&gt; [通用]**** 節點。
3.  在中央窗格中，選擇**空白 app**。
4.  在 [名稱]**** 方塊中輸入 **NavApp1**，然後選擇 [確定]**** 按鈕。

    隨即建立您的方案，而且專案檔案會出現在 [方案總管]**** 中。

5.  若要執行程式，請從功能表依序選擇 [偵錯]**** &gt; [開始偵錯]****，或按 F5。

    隨即顯示空白頁面。

6.  按 Shift+F5 可停止偵錯並返回 Visual Studio。

## <a name="add-basic-pages"></a>新增基本頁面

接下來，將兩個內容頁面新增到專案。

執行下列步驟兩次，新增兩個要互相瀏覽的頁面。

1.  在 [方案總管]**** 中，以滑鼠右鍵按一下 [BlankApp]**** 專案節點以開啟捷徑功能表。
2.  從捷徑功能表選擇 [新增]**** &gt; [新增項目]****。
3.  在 [加入新項目]**** 對話方塊中，選擇中間窗格的 [空白頁]****。
4.  在 [名稱]**** 方塊中，輸入 **Page1** (或 **Page2**)，然後按 [新增]**** 按鈕。

這些檔案現在應該會列在您的 NavApp1 專案中。

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

 

將下列內容新增到 Page1.xaml 的 UI。

-   新增名為 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素，做為根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 將 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 屬性變更為 `Page 1`。

```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   新增下列 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 元素，做為根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素並位於 `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素之後。

    
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

將下列程式碼新增到 Page1.xaml 程式碼後置檔案中的 `Page1` 類別，以處理您先前新增之 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件。 在此，我們瀏覽至 Page2.xaml。

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

對 Page2.xaml 的 UI 進行下列變更。

-   新增名為 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素，做為根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 將 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 屬性的值變更為 `Page 2`。

```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   新增下列 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 元素，做為根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素並位於 `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素之後。

```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

將下列程式碼新增到 Page2.xaml 程式碼後置檔案中的 `Page2` 類別，以處理您先前新增之 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件。 在此，我們瀏覽至 Page1.xaml。

> [!NOTE]
> 針對 C++ 專案，您必須在參考其他頁面之每個頁面的標頭檔中新增 `#include` 指示詞。 針對這裡提供的頁面間瀏覽範例，page1.xaml.h 檔案包含 `#include "Page2.xaml.h"`，page2.xaml.h 則包含 `#include "Page1.xaml.h"`。

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```
```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

準備好內容頁面之後，現在我們必須讓 Page1.xaml 在 app 啟動時顯示。

開啟 app.xaml 程式碼後置檔案並變更 `OnLaunched` 處理常式。

在此，我們要在 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 的呼叫中指定 `Page1`，而不是 `MainPage`。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> protected override void OnLaunched(LaunchActivatedEventArgs e)
> {
>     Frame rootFrame = Window.Current.Content as Frame;
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == null)
>     {
>         // Create a Frame to act as the navigation context and navigate to the first page
>         rootFrame = new Frame();
> 
>         rootFrame.NavigationFailed += OnNavigationFailed;
> 
>         if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
>         {
>             //TODO: Load state from previously suspended application
>         }
> 
>         // Place the frame in the current Window
>         Window.Current.Content = rootFrame;
>     }
> 
>     if (rootFrame.Content == null)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame.Navigate(typeof(Page1), e.Arguments);
>     }
>     // Ensure the current window is active
>     Window.Current.Activate();
> }
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
> {
>     auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);
> 
>     // Do not repeat app initialization when the Window already has content,
>     // just ensure that the window is active
>     if (rootFrame == nullptr)
>     {
>         // Create a Frame to act as the navigation context and associate it with
>         // a SuspensionManager key
>         rootFrame = ref new Frame();
> 
>         rootFrame->NavigationFailed += 
>             ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
>                 this, &amp;App::OnNavigationFailed);
> 
>         if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
>         {
>             // TODO: Load state from previously suspended application
>         }
>         
>         // Place the frame in the current Window
>         Window::Current->Content = rootFrame;
>     }
> 
>     if (rootFrame->Content == nullptr)
>     {
>         // When the navigation stack isn&#39;t restored navigate to the first page,
>         // configuring the new page by passing required information as a navigation
>         // parameter
>         rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
>     }
> 
>     // Ensure the current window is active
>     Window::Current->Activate();
> }
> ```

**注意**：如果瀏覽到 app 的初始視窗框架失敗，這裡的程式碼就會使用傳回值 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 來擲回應用程式例外狀況。 當 **Navigate** 傳回 **true** 時，表示已在瀏覽。

現在，建置並執行 app。 按一下顯示為 [按一下以移至頁面 2] 的連結。 最上方顯示 [第 2 頁] 的第二頁應該會載入並顯示在框架中。

## <a name="frame-and-page-classes"></a>Frame 和 Page 類別

將更多功能新增到 app 前，我們先來看看前面新增的頁面如何為 app 提供瀏覽支援。

首先，以 App.xaml 程式碼後置檔案的 `App.OnLaunched` 方法為 app 建立 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`)。 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法是用來顯示這個 **Frame** 中的內容。

**注意**  
[**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 類別支援各種不同的瀏覽方法，例如 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)、[**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) 和 [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693)，也支援不同的各種屬性，例如 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543)、[**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) 和 [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995)。

 
在我們的範例中，`Page1` 會傳遞至 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法。 這個方法會將 app 目前視窗的內容設定為 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 並將您指定的頁面內容載入 **Frame** (在我們的範例中為 Page1.xaml 或預設的 MainPage.xaml)。

`Page1` 是 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 類別的子類別。 **Page** 類別具有唯讀的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) 屬性，這個屬性會取得包含 **Page** 的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 當 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br227737) 的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br242739) 事件處理常式呼叫 ` Frame.Navigate(typeof(Page2))` 時，app 視窗中的 **Frame** 會顯示 Page2.xaml 的內容。

每當頁面載入框架時，就會以 [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) 將該頁面新增到 [**Frame**](https://msdn.microsoft.com/library/windows/apps/dn279543) 的 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) 或 [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/br227504)。

## <a name="pass-information-between-pages"></a>在頁面之間傳送資訊

我們的 app 可以在兩個頁面之間瀏覽，但這只是最基本的功能。 通常，當應用程式有多個頁面時，這些頁面需要共用資訊。 讓我們將一些資訊從第一頁傳送到第二頁。

在 Page1.xaml 中，使用下列 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br242739) 取代您稍早新增的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br209635)。

在此，我們新增 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 標籤和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (`name`) 以輸入文字字串。

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

在 Page1.xaml 程式碼後置檔案的 `HyperlinkButton_Click` 事件處理常式中，將參考 `name` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 之 `Text` 屬性的參數新增到 `Navigate` 方法。

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

在 Page2.xaml 程式碼後置檔案中，以下列內容覆寫 `OnNavigatedTo` 方法：

> [!div class="tabbedCodeSnippets"]
```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```
```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

執行 app，在文字方塊中輸入您的名稱，然後按一下 [按一下以移至頁面 2]**** 連結。 當您呼叫 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br227737) 的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br242739) 事件中的 `this.Frame.Navigate(typeof(Page2), tb1.Text)` 時，`name.Text` 屬性會傳送到 `Page2`，而事件資料的值會用來做為在頁面上顯示的訊息。

## <a name="cache-a-page"></a>快取頁面

頁面內容和狀態預設不會快取，您必須在 app 的每個頁面中啟用。

在基本對等範例中沒有返回按鈕 (我們會在[返回按鈕瀏覽](navigation-history-and-backwards-navigation.md)中示範返回瀏覽)，但如果您真的在 `Page2` 上按一下返回按鈕，`Page1` 上的 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (以及任何其他欄位) 會設為其預設狀態。 解決此問題的方式之一，是使用 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 屬性來指定頁面要新增至框架的頁面快取。

在 `Page1` 的建構函式中，將 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 設定為 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/br243284)。 這會保留頁面的所有內容和狀態值，直到超出框架的頁面快取為止。

如果您想要忽略框架的快取大小限制，請將 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 設定為 [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284)。 不過，根據裝置的記憶體限制，快取大小限制可能會非常重要。

> [!NOTE]
> [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 屬性會指定框架的瀏覽歷程記錄中可以快取的頁面數。

> [!div class="tabbedCodeSnippets"]
```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```
```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>相關文章

* [UWP app 的瀏覽設計基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [索引標籤和樞紐的指導方針](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [瀏覽窗格的指導方針](https://msdn.microsoft.com/library/windows/apps/dn997766)
 

 







<!--HONumber=Dec16_HO2-->


