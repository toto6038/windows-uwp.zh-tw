---
description: 本主題示範如何註冊和撤銷使用 C++/WinRT 的事件處理委派。
title: 藉由在 C++/WinRT 使用委派來處理事件
ms.date: 04/23/2019
ms.topic: article
keywords: Windows 10, uwp, 一般, c++, cpp, winrt, 投影, 投射, 控點, 事件, 委派
ms.localizationpriority: medium
ms.openlocfilehash: b64fbe93198af95402161873c1d68d0da41f33f7
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853377"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>藉由在 C++/WinRT 使用委派來處理事件

本主題示範如何註冊和撤銷使用 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的事件處理委派。 您可以使用任何標準 C++ 類函式的物件處理事件。

> [!NOTE]
> 如需安裝和使用 C++/WinRT Visual Studio 延伸模組 (VSIX) 與 NuGet 套件 (一起提供專案範本和建置支援) 的資訊，請參閱 [C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>使用 Visual Studio 2019 來新增事件處理常式

若要將事件處理常式新增至您的專案，最方便的方式是使用 Visual Studio 2019 中的 XAML 設計工具使用者介面 (UI)。 在 XAML 設計工具中開啟 XAML 頁面，然後選取您想要在其中處理事件的控制項。 在該控制項中的屬性頁上，按一下閃電圖示，即可列出源自該控制項的所有事件。 然後，連按兩下您想要處理的事件，例如：OnClicked  。

XAML 設計工具會將適當的事件處理常式函式原型 (以及虛設常式實作) 新增至您的來源檔案，方便您以自有的實作加以取代。

> [!NOTE]
> 一般而言，事件處理常式不需要在 Midl 檔 (`.idl`) 中描述。 因此，XAML 設計工具不會將事件處理常式函式原型新增至 Midl 檔。 只會將這些原型新增至您的 `.h` 和 `.cpp` 檔案。

## <a name="register-a-delegate-to-handle-an-event"></a>註冊委派以處理事件

有個簡單的範例，就是處理按鈕的 Click 事件。 通常會像這樣使用 XAML 標記，註冊成員函式來處理事件。

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

可以不在標記中以宣告方式執行，改以註冊成員函式處理事件。 在以下的程式碼範例中，可能不太明顯，但 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)呼叫的引數是 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) 委派的執行個體。 在此案例中，會使用 **RoutedEventHandler** 建構函式多載，其會採用物件和指標成員函式。

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> 註冊委派時，上述程式碼範例會傳遞原始「this」  指標 (指向目前物件)。 若要了解如何為目前物件建立強式或弱式參考，請參閱[如果您使用成員函式作為委派](weak-references.md#if-you-use-a-member-function-as-a-delegate)。

有其他方式可建構 **RoutedEventHandler**。 以下是從文件主題取得的語法區塊適用於 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (從網頁右上角中 [語言]  下拉式清單選擇 [C++/WinRT]  )。 請注意各種不同的建構函式：一種使用的是 lambda，另一種使用可用函式，還有一種 (上述中我們使用的) 是使用物件和指標成員函式。

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

函式呼叫運算子的語法也值得一看。 其會告訴您所需的委派參數。 如您所見，在此案例中，函式呼叫運算子的語法，與我們的 **MainPage::ClickHandler** 參數相符合。

> [!NOTE]
> 對於任何指定的事件，若要找出其委派的詳細資料以及該委派的參數，請先參閱事件本身的文件主題。 以 [UIElement.KeyDown 事件](/uwp/api/windows.ui.xaml.uielement.keydown)為例。 請前往該主題，並且從 [語言]  下拉式清單中選擇 [C++/WinRT]  。 在本主題開頭的語法區塊中，可看到以下項目。
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> 從這些資訊，可以發現 **UIElement.KeyDown** 事件 (討論主題) 具有 **KeyEventHandler** 此委派類型，因為這是使用此事件類型註冊委派時所傳遞的類型。 因此，請點擊主題的連結前往該 [KeyEventHandler 委派](/uwp/api/windows.ui.xaml.input.keyeventhandler)類型。 在這裡，語法區塊含有函式呼叫運算子。 而且，如上所述，其會告訴您所需的委派參數。
> 
> ```cppwinrt
> void operator()(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  如您所見，需要宣告委派，才能使用 **IInspectable** 做為傳送者，並且使用 [KeyRoutedEventArgs 類別](/uwp/api/windows.ui.xaml.input.keyroutedeventargs)的執行個體做為引數。
>
> 若要看看其他範例，可以用 [Popup.Closed 事件](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed)為例。 其委派類型是 [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler)。 因此委派會使用 **IInspectable** 做為傳送者，而另一個 **IInspectable** (因為這是 **EventHandler** 的類型參數) 則是做為引數。

如果事件處理常式的工作量不大，則可使用 lambda 函式取代成員函式。 再次提醒您，在上述程式碼範例中，看起來可能不太明顯，但 **RoutedEventHandler** 委派是建構自 lambda 函式，而其必須符合上述函式呼叫運算子的語法。

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

建立委派時，可以選擇更加明確一點。 比方說，你是想要傳遞委派，還是想要多次使用。

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>撤銷已註冊的委派

當您註冊委派時，通常會將權杖傳回給您。 您後續可以使用該權杖撤銷委派；這表示從事件中取消委派的註冊，且不會被呼叫，事件必須重新產生。

為了簡潔，上述的程式碼範例均不會示範如何執行。 但在下個程式碼範例中，會將權杖儲存於結構的私用資料成員中，並在解構函式中撤銷其處理常式。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            // ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

如上面的範例，而不是穩固參考資料，您可以將弱式參考而不是強式參考保存到按鈕 (請參閱 [C++/WinRT 中的強式和弱式參考](weak-references.md))。

> [!NOTE]
> 當事件來源同步引發其事件時，您可以撤銷您的處理常式並確信您不會再收到任何事件。 但針對非同步事件，即使在撤銷 (尤其是在建構函式內撤銷) 之後，執行中的事件可能會在開始解構之後到達您的物件。 在解構之前尋找取消訂閱的位置可減輕問題，或如需健全的解決方案，請參閱[使用事件處理委派安全地存取 this  指標](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。

或者，您註冊委派時，可以指定 **winrt::auto_revoke** (即類型為 [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t) 的值) 來要求事件撤銷 (類型為 [**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker))。 事件撤銷為您保留事件來源 (引發事件的物件) 的弱式參考。 您可以呼叫 **event_revoker::revoke** 成員函式，以此方式手動撤銷；但是事件撤銷會在其超出範圍時自動呼叫該函式本身。 **revoke** 函式會檢查事件來源是否仍然存在，如果存在，就會撤銷您的委派。 在此範例中，不需要儲存事件來源，並且也不需要解構函式。

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

以下是從文件主題取得的 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件語法區塊。 其會顯示三種不同的註冊和撤銷函式。 可以清楚看到，您需要以何種類型的事件撤銷，從第三方多載進行宣告。

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> 在上述程式碼範例中，`Button::Click_revoker` 是 `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>` 的類型別名。 有個類似的模式適用於所有 C++/WinRT 事件。 每個 Windows 執行階段事件均有 revoke 函式多載，其會傳回事件撤銷，而且該撤銷的類型是事件來源的成員。 因此，再舉另一個例子，[**CoreWindow::SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) 事件有註冊函式多載傳回類型 **CoreWindow::SizeChanged_revoker** 的值。

您可能會考慮在網頁瀏覽的案例中撤銷處理常式。 如果您要重複瀏覽網頁並返回，您離開網頁時，可以撤銷任何處理常式。 或者，如果您重新使用相同的網頁執行個體，則檢查預付碼值，而且只要在尚未設定 (`if (!m_token){ ... }`) 時才註冊。 第三個選項，是將事件撤銷儲存在網頁上做為資料成員。 本主題稍後會說明第四個選項，即是在 lambda 函式中擷取「this」  物件的強式或弱式參考。

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>如果您的自動撤銷委派無法註冊

如果您嘗試在註冊委派時指定 [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t)，且結果是 [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) 例外狀況，這通常表示事件來源不支援弱式參考。 這在 [**Windows.UI.Composition**](/uwp/api/windows.ui.composition) 命名空間等案例中是常見的狀況。 在此情況下，您無法使用自動撤銷功能。 您必須改回以手動撤銷您的事件處理常式。

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>非同步動作和作業的委派類型

上述範例使用 **RoutedEventHandler** 委派類型，但當然還有許多其委派類型。 例如，已完成的非同步動作和作業 (有進度或沒有進度) 和/或預期有對應類型委派的進行中事件。 例如，有進度的非同步作業的進行中事件 (可以是實作[**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)的任何項目) 需要 [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler) 類型的委派。 以下是使用 lambda 函式的撰寫委派類型的程式碼範例。 此範例也會顯示如何撰寫 [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler) 委派。

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
        {
            uint32_t bytes_retrieved = args.BytesRetrieved;
            // use bytes_retrieved;
        });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
        {
            SyndicationFeed syndicationFeed = sender.GetResults();
            // use syndicationFeed;
        });

    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

如上述「協同程式」意見建議，不使用已完成的非同步動作和作業的委派，您可能會發現它使用協同程序會更自然。 如需詳細資訊和程式碼範例，請參閱[使用 C++/WinRT 的並行和非同步作業](concurrency.md)。

> [!NOTE]
> 對非同步動作或作業實作多個「完成處理常式」  不是正確的操作。 要為其已完成的事件準備單一委派還是對其進行 `co_await`，您只能擇一來做。 如果兩者都做，第二個便會失敗。

如果您要繼續使用委派，而不是協同程式，則可以選擇更簡單的語法。

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>傳回一個值的委派類型

某些委派類型本身必須傳回一個值。 範例為 [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler)，其會傳回字串。 以下是撰寫該類型委派 (請注意 lambda 函式會傳回一個值) 的範例。

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>使用事件處理委派安全地存取「this」  指標

如果您使用物件的成員函式，或是從物件成員函式裡的 lambda 函式中處理一個事件，您會需要考量事件 (處理事件的物件) 和事件來源 (引發事件的物件) 的相對存留時間。 如需詳細資訊以及程式碼範例，請參閱 [C++/WinRT 中的強式和弱式參考](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。

## <a name="important-apis"></a>重要 API
* [winrt::auto_revoke_t marker struct](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak function](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [winrt::implements::get_strong function](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>相關主題
* [以 C++/WinRT 撰寫事件](author-events.md)
* [透過 C++/WinRT 的並行和非同步作業](concurrency.md)
* [C++/WinRT 中的強式和弱式參考](weak-references.md)
