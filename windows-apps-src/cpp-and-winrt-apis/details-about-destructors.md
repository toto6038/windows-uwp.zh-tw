---
description: C++/WinRT 2.0 可讓您順延實作類型的損毀，並在損毀期間安全地進行查詢。 本主題將說明這些功能，並說明使用這些功能的時機。
title: 解構函式的詳細資料
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c+ +, cpp, winrt, 投影, 延遲解構, 安全查詢
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387795"
---
# <a name="details-about-destructors"></a>解構函式的詳細資料

C++/WinRT 2.0 可讓您順延實作類型的損毀，並在損毀期間安全地進行查詢。 本主題將說明這些功能，並說明使用這些功能的時機。

## <a name="deferred-destruction"></a>延遲解構

在[診斷直接配置](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc)主題中，我們提到您的實作類型不能有私人解構函式。

擁有公用解構函式的優點在於它能夠延遲解構，此功能可在物件上偵測最終 [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) 呼叫，然後取得該物件的擁有權來無限期延遲其解構。

回想一下，傳統 COM 物件本質上為參考計數物件；參考計數是透過 [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) 和 **IUnknown::Release** 函式來管理。 在傳統的 **Release** 實作中，一旦參考計數到達 0，就會叫用傳統 COM 物件的 C++ 解構函式。

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` 會在釋出物件所佔用記憶體之前，呼叫物件的解構函式。 假設您不需要在解構函式中執行任何有趣的作業，這就夠用。

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

我們提到的「有趣」  是什麼意思？ 一方面，解構函式原本就會同步。 您無法切換執行緒&mdash;或許會終結不同內容中的某些執行緒特定資源。 您無法可靠地查詢物件是否有您可能需要才能釋放特定資源的其他介面。 清單隨即出現。 在您的解構不是很簡單的情況下，您需要更有彈性的解決方案。 這就是 C++/WinRT 的 **final_release** 函式的出處。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

我們已更新 **Release** 的 C++/WinRT 實作，以在物件的參考計數轉換為 0 時立即呼叫 **final_release**。 在該狀態下，物件可確信沒有進一步的未處理參考，且現在有本身專屬的擁有權。 基於這個理由，它可以將本身的擁有權轉移給靜態 **final_release** 函式。

換句話說，物件本身已從支援共用擁有權轉換成獨佔擁有的物件。 **std::unique_ptr** 具有物件的獨佔擁有權，所以當 **std::unique_ptr** 超出範圍時(前提是它不會移到前面的其他位置)，它會在其語法中自然地終結物件&mdash;因此需要公用解構函式。 這就是關鍵。 您可以無限期地使用物件，但前提是 **std::unique_ptr** 讓物件保持運作。 以下說明您可以將物件移到別處的方式。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

請將此視為更具決定性的記憶體回收程式。 或許功能更切實際且更加強大，您可以將 **final_release** 函式變成協同程式，並在一個位置處理其最終解構，同時能夠視需要暫停和切換執行緒。

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

暫停會指出傳回呼叫執行緒&mdash;其原先起始對 **IUnknown::Release** 函式的呼叫&mdash;的原因，因而對呼叫端發出訊號，表示物件一旦保留就無法再透過該介面指標取得。 UI 架構通常需要確保物件會在原先建立物件的特定 UI 執行緒上終結。 這項功能會使符合這類需求變簡單，因為解構會與釋出物件分開。

## <a name="safe-queries-during-destruction"></a>解構期間的安全查詢

以延遲解構的概念為基礎，就能夠在解構期間安全地查詢介面。

傳統 COM 是以兩個中央概念為基礎。 第一個概念是參考計數，第二個概念則是查詢介面。 除了 **AddRef** 和 **Release**以外，**IUnknown** 介面還提供 [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void)。 特定 UI 架構 (例如 XAML) 會大量使用該方法，以便在模擬其可組合的類型系統時周遊 XAML。 請考慮一個簡單範例。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

這「似乎」  可能無害。 此 XAML 頁面想要清除其在解構函式中的資料內容。 但是 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 是 **FrameworkElement** 基底類別的屬性，且存留於不同的 **IFrameworkElement**介面。 因此，C++/WinRT 必須插入對 **QueryInterface** 的呼叫，以便在能夠呼叫 **DataContext** 屬性之前，先尋找正確的 vtable。 但我們甚至在解構函式中的原因是參考計數已轉換為 0。 在此呼叫 **QueryInterface** 會暫時擠掉該參考計數，而當它再次傳回 0 時，物件就會再次解構。

已強化 C++/WinRT 2.0 來支援此功能。 以下是 Release 的 C++/WinRT 2.0 實作 (採用簡化的形式)。

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

如您所預測，它會先減少參考計數，然後只有在沒有未處理參考時才會採取動作。 不過，在呼叫本主題所述的靜態 **final_release**函式之前，它會將參考計數設定為 1，藉此使其穩定。 我們將此稱為「防彈跳」  (從電子工程借用一詞)。 這是防止最終參考釋出的關鍵。 一旦發生這種情況，參考計數就不穩定，且無法可靠地支援對 **QueryInterface** 的呼叫。

在釋出最後參考發行之後呼叫 **QueryInterface** 很危險，因為參考計數可能會無限期地成長。 您必須負責只呼叫不會延長物件存留期的已知程式碼路徑。 C++/WinRT 會確保這些 **QueryInterface** 呼叫能夠可靠地進行，對您作出讓步。

它會藉由穩定參考計數來執行這項工作。 當最終參考釋出時，實際的參考計數可以是 0，或一些無法預測的值。 如果涉及弱式參考，則可能會發生後者的情況。 不論是哪一種情況，如果對 **QueryInterface** 進行後續呼叫，這就無法維持，因為必定會導致參考計數暫時增加&mdash;因此增加防彈跳的參考。 將它設定為 1，可確保對 **Release** 的最終呼叫永遠不再發生於此物件。 這就是我們所要的，因為 **std::unique_ptr**目前擁有此物件，但是對 **QueryInterface**/**Release** 配對的限定呼叫會很安全。

請考慮更有趣的範例。

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

首先會呼叫 **final_release**函式，並通知實作應該進行清除。 在此，**final_release** 剛好是協同程式。 若要模擬第一個暫停點，首先要在執行緒集區上等待幾秒鐘。 然後，它會在頁面的發送器執行緒上繼續。 最後一個步驟涉及查詢，因為 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) 是 **DependencyObject** 基底類別的屬性。 最後，藉由將 `nullptr` 指派給 **std::unique_ptr**，實際刪除此頁面。 接著會呼叫頁面的解構函式。

在解構函式內，我們會清除資料內容；;如我們所知，這需要查詢 **FrameworkElement** 基底類別。

這可能是因為 C++/WinRT 2.0 所提供的參考計數防彈跳 (或參考計數穩定) 所致。