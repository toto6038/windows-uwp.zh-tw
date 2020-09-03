---
description: 這是有關於從 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 逐步移植到 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 的進階主題。 其會顯示平行模式程式庫 (PPL) 工作和協同程式在同一個專案中並存的方式。
title: C++/WinRT 與 C++/CX 之間的非同步和相互操作
ms.date: 08/06/2020
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 移植, 移轉, 相互操作, C++/CX, PPL, 工作, 協同程式
ms.localizationpriority: medium
ms.openlocfilehash: 1beff7fe5595a2601d56d65b52ca51eacedee47f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157392"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>C++/WinRT 與 C++/CX 之間的非同步和相互操作

> [!TIP]
> 我們建議您從頭開始閱讀本主題，但您也可以直接跳到[將非同步 C++/CX 移植到 C++/WinRT](#overview-of-porting-ccx-async-to-cwinrt) 一節中的相互操作技術摘要。

這是有關於從 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 逐步移植到 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 的進階主題。 本主題會就 [C++/WinRT 與 C++/CX 之間的相互操作](./interop-winrt-cx.md)主題進行加強說明。

若因程式碼基底的大小或複雜度而必須逐步移植您的專案，您將需要適當的移植程序，讓 C++/CX 和 C++/WinRT 程式碼能夠在一段時間內並存於相同的專案中。 如果您有非同步程式碼，則您在逐步移植原始程式碼時，可能需要讓平行模式程式庫 (PPL) 工作鏈結和協同程式並存於您的專案中。 本主題著重於非同步 C++/CX 程式碼與非同步 C++/WinRT 程式碼之間的相互操作技術。 您可以個別或搭配使用這些技術。 這些技術可讓您在移植整個專案的過程中，以逐步、受控的方式進行本機變更，而不需要在整個專案進行期間以不受控的方式進行每個變更。

閱讀本主題之前，建議您先閱讀 [C++/WinRT 與 C++/CX 之間的相互操作](./interop-winrt-cx.md)。 該主題會說明如何準備您的專案以進行逐步移植。 其中也會介紹兩個可用來將 C++/CX 物件轉換為 C++/WinRT 物件 (或反向操作) 的 Helper 函式。 這個關於非同步的主題會以該資訊為基礎，並使用這些 Helper 函式。

> [!NOTE]
> 從 C++/CX 逐步移植到 C++/WinRT 有一些限制。 如果您有 [Windows 執行階段元件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)專案，則無法進行逐步移植，而必須一次移植整個專案。 此外，針對 XAML 專案，您的 XAML 頁面類型無論何時都必須完全是 C++/WinRT *或*完全是 C++/CX。 如需詳細資訊，請參閱主題[從 C++/CX 移至 C++/WinRT](./move-to-winrt-from-cx.md)。

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>用整個主題來闡述非同步程式碼相互操作的原因

從 C++/CX 移植到 C++/WinRT 通常不難，但從[平行模式程式庫 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 工作移至協同程式則屬例外。 兩者的模型不同。 從 PPL 工作到協同程式之間並沒有自然的一對一對應，且沒有簡單的方式 (適用於所有案例) 可機械性地移植程式碼。

好消息是，從工作轉換到協同程式後，會產生顯著的簡化。 而且開發小組會定期報告他們在移植其非同步程式碼方面克服的困難，而其餘的移植工作大多是機械性的。

演算法最初通常是針對同步 API 撰寫的。 然後，演算法轉變為工作和明確的接續&mdash;結果常會意外混淆了基礎邏輯。 例如，迴圈變成遞迴；if-else 分支變成工作的內嵌樹狀結構 (鏈結)；共用變數變成 **shared_ptr**。 若要解構 PPL 原始程式碼常見的非自然結構，建議您先回過頭了解原始程式碼的意圖 (也就是，探索原始的同步版本)。 然後將 `co_await` (合作等待) 插入到適當位置。

因此，如果您要從 C# (而不是 C++/CX) 版的非同步程式碼開始進行移植，該做法將可提供更簡單明確的移植程序。 C# 程式碼會使用 `await`。 因此，C# 程式碼在本質上即會依循從同步版本開始，然後在適當之處插入 `await` 的原則。

如果您*沒有* C# 版本的專案，則可以使用本主題中所述的技術。 一旦您移植到 C++/WinRT 後，非同步程式碼的結構就能更容易地移植到 C# (如果您想要這樣做的話)。

## <a name="some-background-in-asynchronous-programming"></a>非同步程式設計的一些背景

為了對非同步程式設計的概念和術語有共通的參考框架，我們將先簡單描述 Windows 執行階段非同步程式設計，以及兩個 C++ 語言投影如何以不同的方式在其上分層。

您的專案具有能以非同步方式運作的方法，而且有兩個主要類型。

- 在您執行其他動作之前，通常會想要等候非同步工作完成。 傳回非同步作業物件的方法，便是您可以指望的其中一個方法。
- 但有時您不想要或不需要等待以非同步方式執行的工作完成。 在這種情況下，非同步方法*不*傳回非同步作業物件會更有效率。 這類非同步方法&mdash;您不會指望的方法&mdash;稱為*射後不理*方法。

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Windows 執行階段非同步物件 (**IAsyncXxx**)

**Windows::Foundation** Windows 執行階段命名空間包含四種非同步作業物件。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1)、
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1)，以及
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)。

在本主題中使用的簡稱 **IAsyncXxx** 時，可能是泛指這些類型。或是討論四種類型的其中之一 (無須指明是哪一個)。

### <a name="ccx-async"></a>非同步 C++/CX

非同步 C++/CX 程式碼會使用[平行模式程式庫 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 工作。 PPL 工作以 [**concurrency::task**](/cpp/parallel/concrt/reference/task-class) 類別來表示。

一般而言，非同步 C++/CX 方法會使用 Lambda 函式搭配 [**concurrency::create_task**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) 和 [**concurrency::task::then**](/cpp/parallel/concrt/reference/task-class#then)，將 PPL 工作鏈結在一起。 每個 Lambda 函式會傳回一項工作，而此工作完成時將會產生一個值，傳入工作*接續*的 Lambda 中。

或者，非同步 C++/CX 方法可以呼叫 [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) 以建立 **IAsyncXxx**\^，而不呼叫 **create_task** 以建立工作。

因此，非同步 C++/CX 方法的傳回類型可以是 PPL 工作，或 **IAsyncXxx**\^。

無論是何種類型，方法本身都會使用 `return` 關鍵字來傳回非同步物件，而在該物件完成時，將會產生呼叫端實際需要的值 (可能是檔案、位元組陣列或布林值)。

> [!NOTE]
> 如果非同步 C++/CX 方法傳回 **IAsyncXxx**\^，則 **TResult** (如果有的話) 會限定為 Windows 執行階段類型。 例如，布林值是 Windows 執行階段類型；但 C++/CX 投影的類型 (例如 **Platform::Array<byte>** ^) 則不是。

### <a name="cwinrt-async"></a>非同步 C++/WinRT

C++/WinRT 會將 C++ 協同程式整合到程式設計模型中。 協同程式和 `co_await` 陳述式提供了一種自然的方式來合作等待結果。

每個 **IAsyncXxx** 類型皆投影到 **winrt::Windows::Foundation** C++/WinRT 命名空間中的對應類型。 讓我們將其稱為 **winrt::IAsyncXxx** (相較於 C++/CX 的 **IAsyncXxx**\^)。

C++/WinRT 協同程式的傳回類型是 **winrt::IAsyncXxx**，或稱 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)。 協同程式會使用 `co_return` 關鍵字來合作傳回呼叫端實際需要的值 (可能是檔案、位元組陣列或布林值)，而不是使用 `return` 關鍵字傳回非同步物件。

如果方法至少包含一個 `co_await` 陳述式 (或至少一個 `co_return` 或 `co_yield`)，則該方法將因此成為協同程式。

如需詳細資訊和程式碼範例，請參閱[使用 C++/WinRT 的並行和非同步作業](./concurrency.md)。

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Direct3D 遊戲範例 (**Simple3DGameDX**)

本主題包含數個特定程式設計技術的逐步解說，會說明如何逐步移植非同步程式碼。 為了作為案例研究，我們將使用 C++/CX 版的 [Direct3D 遊戲範例](/samples/microsoft/windows-universal-samples/simple3dgamedx/) (名為 **Simple3DGameDX**)。 我們將提供一些範例，說明如何取用該專案中的原始 C++/CX 原始程式碼，並將其非同步程式碼逐步移植到 C++/WinRT。

- 從上述連結下載 ZIP，並將其解壓縮。
- 在 Visual Studio 中開啟 C++/CX 專案 (位於名為 `cpp` 的資料夾中)。
- 接著，您必須將 C++/WinRT 支援新增至專案。 如需該作業所需的步驟，請參閱[取用 C++/CX 專案並新增 C++/WinRT 支援](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support)。 在該節中，將 `interop_helpers.h` 標頭檔新增至專案的步驟非常重要，因為我們將仰賴本主題中的 Helper 函式進行操作。
- 最後，將 `#include <pplawait.h>` 新增至 `pch.h`。 這可為您提供協同程式對 PPL 的支援 (後續小節將詳細說明該支援)。

還不要建置，否則您會收到有關 **byte** 不明確的錯誤。 以下是解決此問題的方法。

- 開啟 `BasicLoader.cpp`，並註解 `using namespace std;`。
- 在相同的原始程式碼檔案中，接著您必須將 **shared_ptr** 限定為 **std::shared_ptr**。 您可以在該檔案內使用搜尋和取代來執行此動作。
- 然後將 **vector** 限定為 **std::vector**，並將 **string** 限定為 **std::string**。

專案此時會重新建置、具有 C++/WinRT 支援，並包含 **from_cx** 和 **to_cx** 相互操作 Helper 函式。

至此，**Simple3DGameDX** 專案已準備就緒，您可以依照本主題中的程式碼逐步解說加以操作。

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>將 C++/CX 非同步移植到 C++/WinRT 的概觀

簡言之，在進行移植時，我們會將 PPL 工作鏈結變更為 `co_await` 的呼叫。 我們會將 PPL 工作的方法傳回的值變更為 C++/WinRT **winrt::IAsyncXxx** 物件。 我們也會將 **IAsyncXxx**\^ 變更為 C++/WinRT **winrt::IAsyncXxx**。

您應該記得，協同程式是任何會呼叫 `co_xxx` 的方法。 C++/WinRT 協同程式會使用 `co_return` 來合作傳回其值。 端賴協同程式對 PPL 的支援 (`pplawait.h`)，您也可以使用 `co_return` 從協同程式傳回 PPL 工作。 此外，您也可以 `co_await` 工作和 **IAsyncXxx**。 但您無法使用 `co_return` 傳回 **IAsyncXxx**\^。 下表說明圖片中有 `pplawait.h` 的各種非同步技術之間的相互操作支援。

|方法|是否可以進行 `co_await`？|是否可以從中 `co_return`？|
|-|-|-|
|方法傳回 **task\<void\>**|是|是|
|方法傳回 **task\<T\>**|否|是|
|方法傳回 **IAsyncXxx**^|是|否。 但您會以 **create_async** 包覆使用 `co_return` 的工作。|
|方法傳回 **winrt::IAsyncXxx**|是|是|

請使用下表直接跳到本主題中描述了相關相互操作技術的章節，或直接從這裡繼續閱讀。

|非同步相互操作技術|本主題中的章節|
|-|-|
|使用 `co_await` 等待射後不理方法內的 **task\<void\>** 方法，或等待建構函式內的方法。|[ 等待射後不理方法內的 **task\<void\>** ](#await-taskvoid-within-a-fire-and-forget-method)|
|使用 `co_await` 等待 **task\<void\>** 方法內的 **task\<void\>** 方法。|[等待 **task\<void\>** 方法內的 **task\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|使用 `co_await` 等待 **task\<T\>** 方法內的 **task\<void\>** 方法。|[等待 **task\<T\>** 方法內的 **task\<void\>** ](#await-taskvoid-within-a-taskt-method)|
|使用 `co_await` 等待 **IAsyncXxx**^ 方法。|[等待 **task** 方法中的 **IAsyncXxx**^，讓專案的其餘部分保持不變](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|在 **task\<void\>** 方法內使用 `co_return`。|[等待 **task\<void\>** 方法內的 **task\<void\>** ](#await-taskvoid-within-a-taskvoid-method)|
|在 **task\<T\>** 方法內使用 `co_return`。|[等待 **task** 方法中的 **IAsyncXxx**^，讓專案的其餘部分保持不變](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|以 **create_async** 包覆使用 `co_return` 的工作。|[以 **create_async** 包覆使用 `co_return` 的工作](#wrap-create_async-around-a-task-that-uses-co_return)|
|移植 **concurrency::wait**。|[將 **concurrency::wait** 移植到 `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)|
|傳回 **winrt::IAsyncXxx**，而非 **task\<void\>** 。|[將 **task\<void\>** 傳回類型移植到 **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|
|將 **winrt::IAsyncXxx\<T\>** (T 是基本類型) 轉換成 **task\<T\>** 。|[將 **winrt::IAsyncXxx\<T\>** (T 是基本類型) 轉換成 **task\<T\>** ](#convert-a-winrtiasyncxxxt-t-is-primitive-to-a-taskt)|
|將 **winrt::IAsyncXxx\<T\>** (T 是 Windows 執行階段類型) 轉換成 **task\<T^\>** 。|[將 **winrt::IAsyncXxx\<T\>** (T 是 Windows 執行階段類型) 轉換成 **task\<T^\>** ](#convert-a-winrtiasyncxxxt-t-is-a-windows-runtime-type-to-a-taskt)|

以下是說明部分支援的簡短程式碼範例。

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that. But you can do
    // the following.
    return concurrency::create_async([=]() -> concurrency::task<bool> {
        co_return true;
        });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    co_return co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

> [!IMPORTANT]
> 即使有這些絕佳的相互操作選項，移植仍需仰賴我們精準地選擇變更，而不會影響物件的其餘部分。 我們要避免盲目徒勞，而應將整個專案的結構理出頭緒。 為此，我們必須以特定順序執行操作。 接下來，我們將更加深入地探討一些範例，以了解如何進行這類非同步相關的移植/相互操作變更。

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>等待 **task\<void\>** 方法，讓專案的其餘部分保持不變

傳回 **task\<void\>** 的方法會以非同步方式執行工作，而且會傳回非同步作業物件，但最終不會產生值。 我們可以依此方式 `co_await` 方法。

因此，在逐步移植非同步程式碼時，找出呼叫這類方法的位置可說是理想的起始點。 這些位置牽涉到建立和/或傳回工作。 其也可能牽涉到不會將任何值從每個工作傳遞至其接續工作的工作鏈結種類。 在這類位置中，您可以直接將非同步程式碼取代為 `co_await` 陳述式，如下所示。

> [!NOTE]
> 隨著本主題的推進，您將了解此策略的優點。 一旦您透過 `co_await` 以獨佔方式呼叫特定 **task\<void\>** 方法，就可以放心地將該方法移植到 C++/WinRT，並讓其傳回 **winrt::IAsyncXxx**。

我們看看有哪些範例。 開啟 **Simple3DGameDX** 專案 (請參閱 [Direct3D 遊戲範例](#the-direct3d-game-sample-simple3dgamedx))。

> [!IMPORTANT]
> 在下列範例中，當您看到方法的實作方式已變更時，請記住，對於正在變更的方法，我們不需要變更其*呼叫端*。 這些變更已當地語系化，且在專案中不會重疊顯示。

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>等待射後不理方法內的 **task\<void\>**

我們將從等待*射後不理*方法內的 **task\<void\>** 開始著手，因為這是最簡單的案例。 這些是以非同步方式執行的方法，但方法的呼叫端不會等待該工作完成。 您在呼叫方法後即無須理會，儘管該方法會以非同步方式完成。

在專案中查看其相依性圖形的根目錄，以找出 `void` 方法，其中包含 **create_task** 和/或僅呼叫 **task\<void\>** 方法的工作鏈結。

在 **Simple3DGameDX** 中，您就會在方法 **GameMain::Update** 的實作中找到類似的程式碼。 該項目位於原始程式碼檔案 `GameMain.cpp` 中。

#### <a name="gamemainupdate"></a>**GameMain::Update**

以下片段摘錄自 C++/CX 版的方法，其中顯示以非同步方式完成的兩個方法部分。

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

您可以看到 **Simple3DGame::LoadLevelAsync** 方法的呼叫 (會傳回 PPL **task\<void\>** )。 其後有*接續*執行了某項同步工作。 **LoadLevelAsync** 是非同步的，但不會傳回值。 因此，不會有任何值從工作傳至接續。

我們可以在這兩個地方對程式碼進行相同類型的變更。 在下列清單後面會說明此程式碼。 我們可以在這裡討論如何以安全的方式在類別成員協同程式中存取 *this* 指標。 但讓我們推遲到後面的章節再討論 ([關於 `co_await` 和*這個*指標的推遲討論](#the-deferred-discussion-about-co_await-and-the-this-pointer))&mdash;目前，此程式碼已可運作。

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

如您所見，由於 **LoadLevelAsync** 傳回工作，因此我們可加以 `co_await`。 而且我們不需要明確的接續&mdash;只有在 **LoadLevelAsync** 完成時，才會執行 `co_await` 後續的程式碼。

引進 `co_await` 會將方法轉換成協同程式，因此我們無法就讓其傳回 `void`。 這屬於射後不理方法，因此我們將其改為傳回 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)。

您也需要編輯 `GameMain.h`。 您也必須在該處的宣告中，將 **GameMain::Update** 的傳回類型從 `void` 變更為 **winrt::fire_and_forget**。

您可以對您的專案複本進行這項變更，遊戲仍會以相同方式建置並執行。 原始程式碼基本上仍是 C++/CX，但此時會使用與 C++/WinRT 相同的模式，因此，我們距離機械性移植其餘程式碼的目標，又更接近了一步。

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

**GameMain::ResetGame** 是另一個射後不理方法，也會呼叫 **LoadLevelAsync**。 因此，如果您想要練習，則可以在該處進行相同的程式碼變更。

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

**GameMain::OnDeviceRestored** 又更有意思了，因為其中更深入地內嵌了非同步程式碼，包括無作業的工作。 以下將概述方法的非同步部分 (同步程式碼在此較不重要，以省略符號表示)。

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

首先，在 `GameMain.h` 和 `.cpp` 中，將 **GameMain::OnDeviceRestored** 的傳回類型從 `void` 變更為 **winrt::fire_and_forget**。 您也必須開啟 `DeviceResources.h`，並且對 **IDeviceNotify::OnDeviceRestored** 的傳回類型進行相同的變更。

若要移植非同步程式碼，請移除所有 **create_task** 和 **then** 呼叫及其大括弧，並將方法簡化成一系列的陳述式。

將任何會傳回工作的 `return` 變更為 `co_await`。 您將會看到一個未傳回任何項目的 `return`，請直接將其刪除。 完成後，無作業工作將會消失，而方法的非同步部分大致上會顯示如下。 同樣地，較無趣的同步程式碼已省略。

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

如您所見，這種形式的非同步結構大幅簡化，而且更容易閱讀。

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

**GameMain::GameMain** 建構函式會以非同步方式執行工作，且專案的任何部分都不會等待該工作完成。 同樣地，此清單列出非同步部分。

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

但是，建構函式無法傳回 **winrt::fire_and_forget**，因此我們會將非同步程式碼移至新的 **GameMain::ConstructInBackground** 射後不理方法中、將程式碼簡化為 `co_await` 陳述式，然後從建構函式呼叫新的方法。 結果如下。

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

現在，**GameMain** 中的所有射後不理方法&mdash;事實上，所有的非同步程式碼&mdash;都已轉換為協同程式。 如果您想要的話，或許您可以尋找其他類別的射後不理方法，並進行類似的變更。

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>先前提及的 `co_await` 和 *this* 指標的相關討論

當我們對 **GameMain::Update** 進行變更時，我延遲了關於 *this* 指標的討論。 我們在這裡討論一下。

這適用於我們到目前為止已變更的所有方法，且適用於*所有*協同程式，而不只是射後不理方法。 將 `co_await` 導入方法中，將會導入*暫停點*。 因此，我們必須謹慎處理 *this* 指標；我們在每次存取類別成員時都必然會在暫停點*之後*使用該指標。

簡言之，解決方案是呼叫 [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)。 但如需問題和解決方案的完整討論，請參閱[安全地存取類別成員協同程式中的 *this* 指標](./weak-references.md#safely-accessing-the-this-pointer-in-a-class-member-coroutine)。

您只能在衍生自 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 的類別中呼叫 **implements::get_strong**。

#### <a name="derive-gamemain-from-winrtimplements"></a>從 **winrt::implements** 衍生 **GameMain**

首先我們必須在 `GameMain.h` 中進行變更。

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

**GameMain** 會繼續實作 **DX::IDeviceNotify**，但我們會將其變更為衍生自 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements)。

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

接下來，在 `App.cpp` 中，您會發現此方法。

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

但現在 **GameMain** 衍生自 **winrt::implements**，我們需要以不同方式加以建構。 在此案例中，我們將使用 [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self) 函式範本。 如需詳細資訊，請參閱[具現化並傳回實作類型與介面](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces)。

將該行程式碼取代為以下內容。

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

為了關閉該變更的迴圈，我們也需要變更 *m_main* 的類型。 在 `App.h` 中，您會發現此程式碼。

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

將 *m_main* 的宣告變更為下列內容。

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>我們現在可以呼叫 **implements::get_strong**

針對 **GameMain::Update**，以及任何新增了 `co_await` 的其他方法，您可以依照下列方式在協同程式開頭呼叫 **get_strong**，以確保強式參考在協同程式完成前會持續存留。

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>等待 **task\<void\>** 方法內的 **task\<void\>**

下一個最簡單的案例是在本身會傳回 **task\<void\>** 的方法內等待 **task\<void\>** 。 這是因為我們可以 `co_await` **task\<void\>** ，而且我們可以從其中一個 `co_return`。

您可以在 **Simple3DGame::LoadLevelAsync** 方法中找到一個非常簡單的範例。 該範例位於原始程式碼檔案 `Simple3DGame.cpp` 中。

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

其中只有一些同步程式碼，接著會傳回由 **GameRenderer::LoadLevelResourcesAsync** 建立的工作。

我們不會傳回該工作，而是加以 `co_await`，然後 `co_return` 產生的 `void`。

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

這看起來不像是重大變更。 但既然我們要透過 `co_await` 呼叫 **GameRenderer::LoadLevelResourcesAsync**，因此我們可以放心地進行移植以傳回 **winrt::IAsyncXxx** 而不是 task。 我們稍後會在[將 **task\<void\>** 傳回類型移植到 **winrt::IAsyncXxx**](#port-a-taskvoid-return-type-to-winrtiasyncxxx) 章節中這麼做。

### <a name="await-taskvoid-within-a-taskt-method"></a>等待 **task\<T\>** 方法內的 **task\<void\>**

雖然在 **Simple3DGameDX** 中找不到適合的範例，但我們可以建立假設性範例以顯示模式。

下列程式碼範例中的第一行示範 **task\<void\>** 的簡單 `co_await`。 然後，為了滿足 **task\<T\>** 傳回類型，我們需要以非同步方式傳回 **StorageFile\^** 。 若要這麼做，我們 `co_await` Windows 執行階段 API，並 `co_return` 產生的檔案。

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

我們甚至可以移植方法的更多內容到 C++/WinRT，如下所示。

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

在該範例中，*m_renderer* 資料成員仍為 C++/CX。

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>等待 **task** 方法中的 **IAsyncXxx**^，讓專案的其餘部分保持不變

我們已了解如何 `co_await` **task\<void\>** 。 您也可以 `co_await` 會傳回 **IAsyncXxx** 的方法，無論是您專案中的方法，還是非同步 Windows API (例如，我們在上一節合作等待的 [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync))。

如需可在何處進行這類程式碼變更的範例，請查看 **BasicReaderWriter::ReadDataAsync** (您會發現其已在 `BasicReaderWriter.cpp` 中實作)。

以下是原始 C++/CX 版本。

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

下面列出的程式碼顯示，我們可以 `co_await` 會傳回 **IAsyncXxx**^ 的 Windows API。 不只如此，我們也可以 `co_return` **BasicReaderWriter::ReadDataAsync** 以非同步方式傳回的值 (在此案例中為位元組陣列)。 第一個步驟說明如何僅進行這些變更；我們將在下一節實際將 C++/CX 程式碼移植到 C++/WinRT。

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

同樣地，對於正在變更的方法，我們不需要變更其*呼叫端*，因為傳回類型並未變更。

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>將 **ReadDataAsync** (大部分) 移植到 C++/WinRT，讓專案的其餘部分保持不變

我們可以進一步操作，將方法的*絕大部分*移植到 C++/WinRT，而不需要變更專案的任何其他部分。

此方法對專案其餘部分的唯一相依性是 **BasicReaderWriter::m_location** 資料成員，此為 C++/CX **StorageFolder**^。 若要讓該資料成員保持不變，並將參數類型和傳回類型保持不變，我們只需執行幾項轉換即可&mdash;開頭和結尾處各一項轉換。 為此，我們可以使用 **from_cx** 和 **to_cx** 相互操作 Helper 函式。

以下是 **BasicReaderWriter::ReadDataAsync** 在將其大多數的實作移植到 C++/WinRT 之後的外觀。 這是*逐步移植*的絕佳範例。 而在此方法所處的階段中，我們可以不再將其視為*使用某些 C++/WinRT 技術的 C++/CX 方法*，而將其視為*與 C++/CX 相互操作的 C++/WinRT 方法*。

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> 在上述 **ReadDataAsync** 中，我們會建構並傳回新的 C++/CX 陣列。 當然，我們這麼做是為了滿足方法的傳回類型 (如此，我們就不需要變更專案的其餘部分)。
>
> 您在自己的專案中可能會遇到其他範例，像是，在移植之後，您在方法結束後只獲得了 C++/WinRT 物件。 若要 `co_return`，請直接呼叫 **to_cx** 加以轉換。 下一節有更多關於這方面的資訊以及範例。

## <a name="convert-a-winrtiasyncxxxt-to-a-taskt"></a>將 **winrt::IAsyncXxx\<T\>** 轉換為 **task\<T\>**

本節將說明您已將非同步方法移植到 C++/WinRT (因此方法會傳回 **WinRT::IAsyncXxx\<T\>** ) 的情況，但您仍讓 C++/CX 程式碼呼叫該方法，就好像其仍會傳回 task 一樣。

- 其中一個情況是 **T** 是基本類型，這不需要轉換。
- 另一個情況是 **T** 是 Windows 執行階段類型，在這種情況下，您必須將其轉換成 **T**^。

### <a name="convert-a-winrtiasyncxxxt-t-is-primitive-to-a-taskt"></a>將 **winrt::IAsyncXxx\<T\>** (T 是基本類型) 轉換成 **task\<T\>**

當您以非同步方式傳回基本類型值 (我們將使用布林值來說明) 時，便適用本節的模式。 設想這樣的範例，您已經移植到 C++/WinRT 的方法具有此簽章。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<bool>
MyClass::GetBoolMemberFunctionAsync()
{
    bool value = ...
    co_return value;
}
```

您可以將該方法的呼叫轉換成像這樣的 task。

```cppcx
task<bool> MyClass::RetrieveBoolTask()
{
    co_return co_await GetBoolMemberFunctionAsync();
}
```

或像下面這樣。

```cppcx
task<bool> MyClass::RetrieveBoolTask()
{
    return concurrency::create_task(
        [this]() -> concurrency::task<bool> {
            auto result = co_await GetBoolMemberFunctionAsync();
            co_return result;
        });
}
```

請注意，lambda 函式的 **task** 傳回類型是明確的，因為編譯器無法加以推算。

我們也可以從任意的這類工作鏈結中呼叫方法。 同樣地，使用明確的 lambda 傳回類型。

```cppcx
...
.then([this]() -> concurrency::task<bool> {
    co_return co_await GetBoolMemberFunctionAsync();
}).then([this](bool result) {
    ...
});
...
```

### <a name="convert-a-winrtiasyncxxxt-t-is-a-windows-runtime-type-to-a-taskt"></a>將 **winrt::IAsyncXxx\<T\>** (T 是 Windows 執行階段類型) 轉換成 **task\<T^\>**

當您以非同步方式傳回 Windows 執行階段值 (我們將使用 **StorageFile** 值來說明) 時，便適用本節的模式。 設想這樣的範例，您已經移植到 C++/WinRT 的方法具有此簽章。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
MyClass::GetStorageFileMemberFunctionAsync()
{
    co_return co_await winrt::Windows::Storage::StorageFile::GetFileFromPathAsync
    (L"MyFile.txt");
}
```

接下來列出的這個內容顯示如何將該方法的呼叫轉換成工作。 請注意，我們需要呼叫 **to_cx** 相互操作 helper 函式，以將傳回的 C++/WinRT 物件轉換成 C++/CX 控制代碼 (也稱為 *hat*) 物件。

```cppcx
task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    winrt::Windows::Storage::StorageFile storageFile =
        co_await GetStorageFileMemberFunctionAsync();
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

以下是更簡潔的版本。

```cppcx
task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    co_return to_cx<Windows::Storage::StorageFile>(GetStorageFileMemberFunctionAsync());
}
```

您甚至可以選擇將該模式包裝成可重複使用的函式範本，並加以 `return`，就像平常傳回工作一樣。

```cppcx
template<typename ResultTypeCX, typename Awaitable>
concurrency::task<ResultTypeCX^> to_task(Awaitable awaitable)
{
    co_return to_cx<ResultTypeCX>(co_await awaitable);
}

task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    return to_task<Windows::Storage::StorageFile>(GetStorageFileMemberFunctionAsync());
}
```

如果您喜歡這種想法，則可以將 **to_task** 新增至 `interop_helpers.h`。

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>以 **create_async** 包覆使用 `co_return` 的工作

您無法直接 `co_return` **IAsyncXxx**\^，但可以達到類似的目的。 如果您的工作會合作傳回值，則可以將其包裝在 [**concurrency::create_async**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) 的呼叫內。

以下是假設性範例，因為我們無法從 **Simple3DGameDX** 舉出適當範例。

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ MyClass::RetrieveBoolAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await GetBoolMemberFunctionAsync();
            co_return result;
        });
}
```

如您所見，您可以從任何可 `co_await` 的方法取得傳回值。

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>將 **concurrency::wait** 移植到 `co_await winrt::resume_after`

在若干位置，**Simple3DGameDX** 使用 [**concurrency::wait**](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait) 短時間暫停了執行緒。 以下為範例。

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

C++/WinRT 版的 **concurrency::wait** 是 **winrt::resume_after** 結構。 我們可以在 PPL 工作內 `co_await` 該結構。 以下是程式碼範例。

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

請留意我們已進行的其他兩個變更。 我們已將 **GameConstants::InitialLoadingDelay** 的類型變更為 **std::chrono::duration**，並且將 Lambda 函式的傳回類型設為明確，因為編譯器已無法再加以推算。

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>將 **task\<void\>** 傳回類型移植到 **winrt::IAsyncXxx**

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

在我們處理 **Simple3DGameDX** 的這個階段中，專案中所有呼叫 **Simple3DGame::LoadLevelAsync** 的位置都會使用 `co_await` 加以呼叫。

這表示我們可以直接將該方法的傳回類型從 **task\<void\>** 變更為 **winrt::Windows::Foundation::IAsyncAction** (讓其餘部分保持不變)。

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

現在，將該方法的其餘部分及其相依性 (例如 *m_level* 等等) 移植到 C++/WinRT 的程序應該已相當機械性。

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

以下是 **GameRenderer::LoadLevelResourcesAsync** 的原始 C++/CX 版本。

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

**Simple3DGame::LoadLevelAsync** 是專案中唯一會呼叫 **GameRenderer::LoadLevelResourcesAsync**的位置，且已使用 `co_await` 加以呼叫。

因此，**GameRenderer::LoadLevelResourcesAsync** 不需要再傳回工作&mdash;可以改為傳回 **winrt::Windows::Foundation::IAsyncAction**。 實作本身很簡單，可以完全移植到 C++/WinRT。 這牽涉到我們在[將 **concurrency::wait** 移植到 `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after) 中所做的相同變更。 對於專案的其餘部分並沒有太多相依性，因此無須顧慮。

以下是方法完全移植到 C++/WinRT 之後所呈現的外觀。

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="the-goalmdashfully-port-a-method-to-cwinrt"></a>目標&mdash;將方法完全移植到 C++/WinRT

讓我們以最終目標的範例來總結這個逐步解說，做法是將方法 **BasicReaderWriter::ReadDataAsync** 完整移植到 C++/WinRT。

上一次查看這個方法時，(在[將 **ReadDataAsync** (大部分) 移植到 C++/WinRT，讓專案的其餘部分保持不變](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)章節)，*大部分*都已移植到 C++/WinRT。 但仍會傳回 **Platform::Array\<byte\>** ^ 的工作。

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

我們不會傳回工作，而是會將其變更為傳回 **IAsyncOperation**。 而且我們不會透過該 **IAsyncOperation** 傳回位元組陣列，而是會改為傳回 C++/WinRT [**IBuffer**](/uwp/api/windows.storage.streams.ibuffer) 物件。 這也需要對呼叫網站上的程式碼進行些許變更，誠如所見。

以下是方法在移植其實作、參數和 *m_location* 資料成員以使用 C++/WinRT 語法和物件之後的外觀。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

如您所見，**BasicReaderWriter::ReadDataAsync** 本身變得更簡單，因為我們已在其本身的方法中加入從緩衝區中擷取位元組的同步邏輯。

但現在我們需要從 C++/CX 中的這種結構移植呼叫位置。

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

移植到 C++/WinRT 中的此模式。

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>重要 API

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>相關主題

* [從 C++/CX 移到 C++/WinRT](./move-to-winrt-from-cx.md)
* [C++/WinRT 與 C++/CX 之間的互通性](./interop-winrt-cx.md)
* [透過 C++/WinRT 的並行和非同步作業](./concurrency.md)
* [C++/WinRT 中的強式和弱式參考](./weak-references.md)
* [使用 C++/WinRT 撰寫 API](./author-apis.md)