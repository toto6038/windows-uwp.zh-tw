---
author: Jwmsft
Description: Learn how to enable peer-to-peer navigation between two basic pages in an Universal Windows Platform (UWP) app.
title: 兩個頁面之間的對等瀏覽
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 91a1ca0ee99833280aaa41ca4d9c94d043a78e0a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5689668"
---
# <a name="implement-navigation-between-two-pages"></a>在兩頁間實作瀏覽

了解如何使用框架和頁面，以便在您的 App 中能夠進行基本對等瀏覽。 

> **重要 API**：[**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 類別、[**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 類別、[**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) 命名空間

![對等瀏覽](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. 建立空白的應用程式

1.  在 Microsoft Visual Studio 功能表，選擇 **\[檔案\]** > **\[新增專案\]**。
2.  在 **\[新增專案\]** 對話方塊的左窗格中，選擇 **\[Visual C#\]** > **\[Windows\]** > **\[通用\]** 或 **\[Visual C++\]** > **\[Windows\]** > **\[通用\]** 節點。
3.  在中央窗格中，選擇 **\[空白 app\]**。
4.  在 **\[名稱\]** 方塊中輸入 **NavApp1**，然後選擇 **\[確定\]** 按鈕。
    隨即建立您的方案，而且專案檔案會出現在 **\[方案總管\]** 中。
5.  若要執行程式，請從功能表依序選擇 **\[偵錯\]** > **\[開始偵錯\]**，或按 F5。
    隨即顯示空白頁面。
6.  若要停止偵錯並回到 Visual Studio，請結束 app，或從功能表按一下 **\[停止偵錯\]**。

## <a name="2-add-basic-pages"></a>2. 新增基本頁面

接下來，將兩個頁面新增到專案。

1.  在 **\[方案總管\]** 中，以滑鼠右鍵按一下 **\[BlankApp\]** 專案節點以開啟捷徑功能表。
2.  從捷徑功能表選擇 **\[新增\]** > **\[新增項目\]**。
3.  在 **\[加入新項目\]** 對話方塊中，選擇中間窗格的 **\[空白頁\]**。
4.  在 **\[名稱\]** 方塊中，輸入 **Page1** (或 **Page2**)，然後按 **\[新增\]** 按鈕。
5. 重複步驟 1 到 4，新增第二個頁面。

現在，這些檔案應該會列在您的 NavApp1 專案中。

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

在 Page1.xaml 中，新增下列內容：

-   名為 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素，做為根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 將 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 屬性變更為 `Page 1`。
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)元素，做為根[**格線**](https://msdn.microsoft.com/library/windows/apps/br242704)和之後的子元素`pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)元素。
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

在 Page1.xaml 程式碼後置檔案中，新增下列程式碼來處理您先前新增的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件，以瀏覽至 Page2.xaml。

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

在 Page2.xaml 中，新增下列內容：

-   名為 `pageTitle` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 元素，做為根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 的子元素。 將 [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) 屬性的值變更為 `Page 2`。
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739)元素，做為根[**格線**](https://msdn.microsoft.com/library/windows/apps/br242704)和之後的子元素`pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652)元素。
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

在 Page2.xaml 程式碼後置檔案中，新增下列程式碼來處理您先前新增的 [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) 的 `Click` 事件，以瀏覽至 Page1.xaml。

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> 針對 C++ 專案，您必須在參考其他頁面之每個頁面的標頭檔中新增 `#include` 指示詞。 針對這裡提供的頁面間瀏覽範例，page1.xaml.h 檔案包含 `#include "Page2.xaml.h"`，page2.xaml.h 則包含 `#include "Page1.xaml.h"`。

準備好頁面之後，現在我們必須讓 Page1.xaml 在 app 啟動時顯示。

開啟 App.xaml 程式碼後置檔案並變更 `OnLaunched` 處理常式。

在此，我們要在 [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 的呼叫中指定 `Page1`，而不是 `MainPage`。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> 這裡的程式碼會使用傳回值的[**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)擲回應用程式例外狀況，如果瀏覽到 app 的初始視窗框架失敗。 當 **Navigate** 傳回 **true** 時，表示已在瀏覽。

現在，建置並執行 app。 按一下顯示為 [按一下以移至頁面 2] 的連結。 最上方顯示 [第 2 頁] 的第二頁應該會載入並顯示在框架中。

### <a name="about-the-frame-and-page-classes"></a>關於 Frame 和 Page 類別

將更多功能新增到 App 前，我們先來看看前面新增的頁面如何在 App 內提供瀏覽。

首先，以 App.xaml 程式碼後置檔案的 `App.OnLaunched` 方法為 app 建立 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)，稱為 `rootFrame`。 **Frame** 類別支援各種不同的瀏覽方法，例如 [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694)、[**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) 和 [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693)，也支援不同的各種屬性，例如 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543)、[**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) 和 [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995)。
 
[**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) 方法是用來顯示這個 **Frame** 中的內容。 根據預設，此方法會載入 MainPage.xaml。 在我們的範例中，`Page1` 會傳遞至 **Navigate** 方法，此方法就會在 **Frame** 中載入 `Page1`。 

`Page1` 是 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 類別的子類別。 **Page** 類別具有唯讀的 **Frame** 屬性，這個屬性會取得包含 **Page** 的 **Frame**。 當 `Page1` 中 **HyperlinkButton** 的 **Click** 事件處理常式呼叫 `this.Frame.Navigate(typeof(Page2))` 時，**Frame** 會顯示 Page2.xaml 的內容。

最後，每當頁面載入框架時，該頁面就會做為 [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) 新增到 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) 的 [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) 或 [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547)，以便進行[歷程記錄和向後瀏覽](navigation-history-and-backwards-navigation.md)。

## <a name="3-pass-information-between-pages"></a>3. 在頁面之間傳送資訊

我們的 App 可以在兩個頁面之間瀏覽，但這只是最基本的功能。 通常，當應用程式有多個頁面時，這些頁面需要共用資訊。 讓我們將一些資訊從第一頁傳送到第二頁。

在 Page1.xaml 中，您已新增**HyperlinkButton**較舊版本以取代下列[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)。

在此，我們新增 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 標籤和 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) `name` 以輸入文字字串。

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

在`HyperlinkButton_Click`事件處理常式 Page1.xaml 程式碼後置檔案中，新增參數參考`Text`屬性的`name` **TextBox**到`Navigate`方法。

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

在 Page2.xaml 中，使用下列 **StackPanel** 取代您稍早新增的 **HyperlinkButton**。

以下，我們新增 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 以顯示從 Page1 傳來的文字字串。

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

在 Page2.xaml 程式碼後置檔案中，新增下列內容覆寫 `OnNavigatedTo` 方法：

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

執行 app，在文字方塊中輸入您的名稱，然後按一下 **\[按一下以移至頁面 2\]** 連結。 

當 `Page1` 中 **HyperlinkButton** 的 **Click** 事件呼叫 `this.Frame.Navigate(typeof(Page2), name.Text)` 時，`name.Text` 屬性會傳送到 `Page2`，而事件資料的值會用於頁面上所顯示的訊息。

## <a name="4-cache-a-page"></a>4. 快取頁面

頁面內容和狀態預設不會快取，因此，若您想要快取資訊，就必須在 App 的每個頁面中啟用。

在基本對等範例中沒有返回按鈕 (我們會在[返回瀏覽](navigation-history-and-backwards-navigation.md)中示範返回瀏覽)，但如果您真的在 `Page2` 上按一下返回按鈕，`Page1` 上的 **TextBox** (以及任何其他欄位) 會設為其預設狀態。 解決此問題的方式之一，是使用 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) 屬性來指定頁面要新增至框架的頁面快取。 

在 `Page1` 的建構函式中，您可以將 **NavigationCacheMode** 設定為 **\[已啟用\]**，以保留頁面的所有內容和狀態值，直到超過框架的頁面快取。 如果您想要略過 [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) 限制，可將 [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506)設定為 [**必要**](https://msdn.microsoft.com/library/windows/apps/br243284)，這樣就會指定瀏覽歷史記錄中可針對框架快取的頁數。 不過，務必記住，根據裝置的記憶體限制，快取大小限制可能會非常重要。

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
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
