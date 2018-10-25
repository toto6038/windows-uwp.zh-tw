---
author: stevewhims
description: 本主題示範如何註冊和撤銷使用 C++/WinRT 的事件處理委派。
title: 藉由在 C++/WinRT 使用委派來處理事件
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、一般、c++、cpp、winrt、投影、投射、控點、事件、委派
ms.localizationpriority: medium
ms.openlocfilehash: 96655c14f9c21f804ef5ebfdfe73cee0b04edfe3
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "5483598"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>藉由在 C++/WinRT 使用委派來處理事件

本主題示範如何註冊和撤銷使用的事件處理委派[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。 您可以使用任何標準 C++ 類函式的物件處理事件。

> [!NOTE]
> 如需有關安裝和使用 C++/WinRT Visual Studio 擴充功能 (VSIX) (提供專案範本的支援，以及 C++/WinRT MSBuild 屬性和目標) 的資訊，請參閱 [C++/WinRT 和 VSIX 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)。

## <a name="register-a-delegate-to-handle-an-event"></a>註冊委派，處理事件

簡單範例為處理按鈕的按一下事件。 通常像這樣使用 XAML 標記，註冊成員函式來處理事件。

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

在標記中不以宣告方式執行，您可以命令註冊成員函式處理事件。 下列程式碼範例所示可能不明顯，但 [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)呼叫的引數是[**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler)委派的執行個體。 在此案例中，我們會使用 **RoutedEventHandler** 建構函式多載，其採用物件和指標成員函式。

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> 註冊委派，上述程式碼範例會將傳遞原始*這個*指標 （指向目前的物件）。 若要了解如何建立強式或弱式參考目前的物件，請參閱**如果您使用成員函式做為委派**子[安全地存取*此*指標事件處理委派與](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)一節中。

有其他方式可建構 **RoutedEventHandler**。 以下是從文件主題取得的語法區塊適用於 [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (從頁面中 **語言** 下拉式清單選擇 *C++/WinRT*)。 請注意的各種建構函式：一種是 lambda；另一種是可用函式；以及另一種是 (上述中我們使用的) 物件和指標成員函式。

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

函式呼叫運算子的語法對查看也很有幫助。 它會告訴您所需的委派參數。 如您所見，在此案例中函式呼叫運算子語法與我們的**MainPage::ClickHandler**參數相符合。

如果您不在事件處理中執行多項工作，則您可以使用 lambda 函式而非成員函式。 再次提醒您，從上述程式碼範例所示中它可能不明顯，但 **RoutedEventHandler** 委派會從 lambda 函式建構，再次提醒，必須符合函式呼叫運算子的語法。

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

建立委派時，您可以選擇較為明確一點。 例如，如果您想要通過它，或超過一次使用它。

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

當您註冊委派時，通常會將權杖傳回給您。 您後續可以使用該權杖撤銷委派；這表示從事件中取消委派的註冊，且不會被呼叫，事件必須重新產生。 為了簡單起見，上述的程式碼範例皆無顯示如何執行。 但下一個程式碼範例將預付碼儲存於結構的私用資料成員中，並在解構函式中撤銷其處理常式。

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

而不是強式參考資料，如上述範例所示，您可以儲存至按鈕的弱式參考 (請參閱[強式和弱式參考，在 C + + /winrt](weak-references.md))。

或者，當您註冊委派時，您可以指定**winrt:: auto_revoke** （也就是類型[**winrt:: auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)的值） 來要求事件撤銷 (of 類型[**winrt:: event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker))。 事件撤銷會為您保留事件來源 （引發事件的物件） 的弱式參考。 您可以藉由呼叫 **event_revoker::revoke** 成員函式手動撤銷；但是事件撤銷會在其超出範圍時自動呼叫該函式本身。 **revoke** 函式會檢查事件來源是否仍然存在，如果是，撤銷您的委派。 在此範例中，不需要儲存事件來源，並且也不需要解構函式。

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

以下是從文件主題取得語法封鎖適用於[**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)事件。 它會顯示三種不同註冊和撤銷功能。 您可以完全看到是哪種類型的事件撤銷，您會需要從第三多載宣告。

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
> 在程式碼上述範例中，`Button::Click_revoker`是類型別名`winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`。 一個類似的模式適用於所有 C++/WinRT 事件。 每個 Windows 執行階段事件已撤銷函式多載傳回事件撤銷，並撤銷的類型是事件來源的成員。 已啟用，若要進行另一個範例， [**corewindow:: Sizechanged**](/uwp/api/windows.ui.core.corewindow.sizechanged)事件傳回的值類型**CoreWindow::SizeChanged_revoker**登錄函式多載。


您可能要考慮在網頁瀏覽的案例中撤銷處理常式。 如果您正重複瀏覽網頁並返回，您離開網頁時，可以撤銷任何處理常式。 或者，如果您重新使用相同的網頁執行個體，然後檢查您的權證值，且如果尚未設定 (`if (!m_token){ ... }`)，，只有註冊。 第三個選項是將事件撤銷儲存在網頁上做為資料成員。 本主題稍後說明，第四個選項是在您的 lambda 函式中擷取*this*物件的強式或弱式參考。

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>非同步動作和作業的委派類型

上述範例使用**RoutedEventHandler**委派類型，但當然還有許多其委派類型。 例如，已完成非同步動作和作業 (有與沒有進度) 和/或預期委派對應類型的進行中事件。 例如，有進度的非同步作業的進行中事件 (可以是實作[**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)的任何項目) 需要 [**AsyncOperationProgressHandler**](/uwp/api/windows.foundation.asyncoperationprogresshandler)類型的委派。 以下是使用 lambda 函式的撰寫委派類型的程式碼範例。 此範例也會顯示如何撰寫 [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler) 委派。

```cppwinrt
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
> 不正確實作一個以上的非同步動作或作業*完成處理常式*。 您可以讓任一單一委派的已完成的事件，或者您可以`co_await`它。 如果您有兩者，則第二個將會失敗。

如果您堅持使用委派，而不是協同程式，您可以選擇簡單的語法。

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
});
```

## <a name="delegate-types-that-return-a-value"></a>傳回一個值的委派類型

某些委派類型本身必須傳回一個值。 範例為[**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler)，其會傳回字串。 以下是撰寫該類型委派 (請注意 lambda 函式傳回一個值) 的範例。

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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>安全地存取*此*指標與事件處理委派

如果您處理事件的物件的成員函式，或從 lambda 函式內物件的成員函式，則您需要考量事件收件者 （處理事件的物件） 和事件來源 （該物件的相對存留時間引發事件）。 如需詳細資訊和程式碼範例，請參閱[強式和弱式參考，在 C + + /winrt](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)。

## <a name="important-apis"></a>重要 API
* [winrt:: auto_revoke_t 標記結構](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak function](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::implements::get_strong function](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>相關主題
* [在 C++/WinRT 中撰寫事件 ](author-events.md)
* [使用 C++/WinRT 的並行和非同步作業](concurrency.md)
* [強式和弱式參考，在 C + + /winrt](weak-references.md)
