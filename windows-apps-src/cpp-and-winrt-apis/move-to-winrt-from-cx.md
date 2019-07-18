---
description: 本主題示範如何將 C++/CX 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 C++/CX 移到 C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 連接埠, 移轉, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: d2b92bf5e265c2d596a7fc7eb54b127010cee897
ms.sourcegitcommit: a7a1e27b04f0ac51c4622318170af870571069f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717606"
---
# <a name="move-to-cwinrt-from-ccx"></a>從 C++/CX 移到 C++/WinRT

本主題示範如何將 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 專案移植到其在 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的對等項目。

## <a name="porting-strategies"></a>移植策略

如果您想要將 C++/CX 程式碼逐漸移植到 C++/WinRT，您可以使用此策略。 C++/CX 和 C++/WinRT 程式碼可以共存在相同的專案中，但 XAML 編譯器支援與 Windows 執行階段元件除外。 對於這些兩個例外狀況，您必須將相同專案中的目標設為 C++/CX 或 C++/WinRT。

> [!IMPORTANT]
> 如果您的專案組建了 XAML 應用程式，那麼其中一種建議的工作流程是，先使用一個 C++/WinRT 專案範本，在 Visual Studio 中建立新項目 (請參閱[適用於 C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 然後，開始從 C++/CX 專案複製原始程式碼和標記。 您可以使用 [專案]  \> [新增項目...]  來新增 XAML 頁面\> **Visual C++**  > **空白頁 (C++/WinRT)** 。
>
> 或者，您可以使用 Windows 執行階段元件，在移植 XAML C++/CX 專案時，將程式碼從中分解出來。 請盡可能將 C++/CX 程式碼移動到元件中，再將 XAML 專案變更為 C++/WinRT。 或是將 XAML 專案保留為 C++/CX，而建立新的 C++/WinRT 元件，並開始將 C++/CX 程式碼從 XAML 專案移植到元件中。 您也可以在同個解決方案中，一起使用 C++/CX 元件專案與 C++/WinRT 元件專案，從應用程式專案中參照這兩個元件專案，並逐漸從一個專案移植到另一個專案。 如需在相同專案中使用兩種語言投影的更多詳細資料，請參閱 [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 以及根命名空間 **Windows** 的 Windows SDK 宣告類型。 投影到 C++/WinRT 的 Windows 類型有與 Windows 類型相同的完整名稱，但它放在 C++ **winrt** 命名空間。 這些不同的命名空間，可讓您以自己的速度從 C++/CX 移植至 C++/WinRT。

請記住上述例外狀況，將 C++/CX 專案移植到 C++/WinRT 的第一個步驟是手動新增 C++/WinRT 支援 (請參閱[適用於 C++/WinRT 的 Visual Studio 支援](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 若要這麼做，請將 [Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)安裝到您的專案中。 在 Visual Studio 中開啟專案，按一下 [專案]  \>[管理 NuGet 套件...]  \>[瀏覽]  、在搜尋方塊中輸入或貼上 **Microsoft.Windows.CppWinRT**、在搜尋結果中選取項目，然後按一下 [安裝]  以安裝適用於該專案的套件。 該變更的其中一個效果，是會關閉專案中支援的 C++/CX。 最好關閉支援，這樣組建訊息有助於找出 (並移植) C++/CX 上所有的相依性，或您可以重新開啟支援 (在專案屬性中，**C/C++** \> **一般** \> **使用 Windows 執行階段擴充功能** \> **是 (/ZW)** )，請逐漸移植。

請務必將專案屬性 [一般]  \>[目標平台版本]  設為 10.0.17134.0 (Windows 10，版本 1803) 或更高版本。

在您先行編譯的標頭檔案 (通常是 `pch.h`) 中，加入 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果加入了任何 C++/WinRT 投影的 Windows API 標頭 (例如 `winrt/Windows.Foundation.h`)，則不需要像這樣明確地加入 `winrt/base.h`，因為會自動包含在內。

如果您的專案也會使用 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 類型，請參閱[從 WRL 移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="parameter-passing"></a>參數傳遞
撰寫 C++/CX 原始程式碼時，會傳遞 C++/CX 類型做為控制帽 (\^) 參考的函式參數。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

在 C++/WinRT 中，針對同步函式，依預設應該使用 `const&` 參數。 如此可避免複本以及連鎖額外負荷。 但您的協同程式應該使用透過值傳遞，以確保他們透過值擷取，並避免存留期問題 (如需詳細資訊，請參閱[使用 C++/WinRT 進行並行和非同步作業](concurrency.md))。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 物件基本上是一個值，用於保留介面指標到支援 Windows 執行階段物件。 當您複製 C++/WinRT 物件時，編譯器會複製封裝的介面指標，遞增其參考次數。 最終複本的破壞會導致參考次數遞減。 因此，只有在必要時才會產生複本的額外負荷。

## <a name="variable-and-field-references"></a>變數和欄位參考資料
撰寫 C++/CX 原始程式碼時，使用控制帽 (\^) 變數參考 Windows 執行階段物件，以及箭頭 (-&gt;) 運算子以取值控制帽變數。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

藉由移除其控制帽，並將箭頭運算子 (-&gt;) 變更為點運算子 (.)，對於移植到對等項目 C++/WinRT 程式碼有很大的幫助。 C++/WinRT 投影類型為值，而非指標。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

預設建構函式 C++/CX 控制帽指標將其初始化為 null。 以下是 C++/CX 程式碼範例，我們在其中建立了一個正確類型的變數/欄位，但尚未初始化。 換句話說，其最初並非是參考到 **TextBlock**；我們打算稍後再指派一個參考。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

如需了解 C++/WinRT 中的對等項目，請參閱[延遲初始化](consume-apis.md#delayed-initialization)。

## <a name="properties"></a>屬性
C++/CX 語言擴充功能包括了屬性的概念。 當撰寫 C++/CX 原始程式碼時，可以如欄位一般存取其屬性。 標準 C++ 不具屬性的概念，因此在 C++/WinRT 中，您要呼叫 get 和 set 函式。

在接下來的範例中，**XboxUserId**、**UserState**、**PresenceDeviceRecords**，和 **Size** 都是屬性。

### <a name="retrieving-a-value-from-a-property"></a>從屬性擷取一個值
以下是如何使用 C++/CX 取得屬性值的方法。

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

對等項目 C++/WinRT 原始程式碼呼叫與屬性具有相同名稱，但不含任何參數的函式。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

請注意，**PresenceDeviceRecords** 函式傳回 Windows 執行階段物件，其本身有一個 **Size** 函式。 由於傳回的物件也是 C++/WinRT 投影類型，因此我們使用點運算子取值以呼叫 **Size**。

### <a name="setting-a-property-to-a-new-value"></a>將屬性設定為新值
將屬性設定為新值，按照類似的模式。 首先要看 C++/CX。

```cppcx
record->UserState = newValue;
```

若要執行 C++/WinRT 中的對等項目，請您呼叫具有與屬性相同名稱的函式，並傳遞引數。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>建立類別的執行個體
透過控制代碼使用 C++/CX 物件，通常稱它為控制帽 (\^) 參考。 透過 `ref new` 關鍵字建立新的物件，依序呼叫 [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 以啟動新的執行階段類別執行個體。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 物件是值；讓您可以在堆疊上配置它，或做為物件的欄位。 您「從不」  使用 `ref new` (或 `new`) 配置 C++/WinRT 物件。 幕後仍持續呼叫 **RoActivateInstance**。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

如果將資源初始化的成本相當高，則常會延遲初始化，直到有實際需求時再進行。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

已將相同的程式碼移植至 C++/WinRT。 請注意 **std:: nullptr_t** 建構函式的實作。 如需該建構函式的詳細資訊，請參閱[使用 C++/WinRT 取用 API](consume-apis.md#delayed-initialization)。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>從基礎執行階段類別轉換至衍生的執行階段類別
讓您所知的 reference-to-base 參考到衍生類型的物件，算是相當常見。 在 C++/CX 中，要使用 `dynamic_cast` 將 reference-to-base *轉換*為 reference-to-derived。 `dynamic_cast` 實際上只是對 [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 的隱藏呼叫。 這是一個典型的範例&mdash;您在處理相依性屬性變更事件，而且希望從 **DependencyObject** 轉換回擁有相依性屬性的實際類型。

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

對等項目 C++/WinRT 程式碼藉由呼叫 [ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) 函式，取代 `dynamic_cast`，該函式會封裝 **QueryInterface**。 您也可以選擇呼叫 [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，如果未傳回查詢所需的介面 (要求類型的預設介面)，則會擲回例外狀況。 這是 C++/WinRT 程式碼範例。

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>使用委派的事件處理
以下是在 C++/CX 中處理事件的一般範例，這種情形下，是使用 lambda 函式做為委派。

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

您可以選擇實作委派做為可用功能，或做為指標成員函式，而不是 lambda 函式。 如需詳細資訊，請參閱[透過使用 C++/WinRT 中的委派來處理事件](handle-events.md)。

如果您正從在內部使用事件和委派的 C++/CX 程式碼基底進行移植 (並非所有二進位檔案)，則 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 可協助您在 C++/WinRT 中複寫該模式。 另請參閱[參數化委派、簡單的訊號，以及專案中的回呼](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)。

## <a name="revoking-a-delegate"></a>撤銷委派
您在 C++/CX 中使用 `-=` 運算子以撤銷前一個事件註冊。

```cppcx
myButton->Click -= token;
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
myButton().Click(token);
```

如需詳細資訊以及選項，請參閱[撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>將 C++/CX **平台**類型對應至 C++/WinRT 類型
C++/CX 在**平台**命名空間中提供幾種資料類型。 這些類型不是標準 C++，因此您只能在啟用 Windows 執行階段語言擴充功能時使用它們 (Visual Studio 專案屬性 **C/C++**  > **一般** >  **使用 Windows 執行階段擴充功能** > **是 (/ZW)** )。 下列表格有助於您從**平台**類型移植至 C++/WinRT 中的對等項目。 完成後，由於 C++/WinRT 是標準的 C++，您可以關閉 `/ZW` 選項。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | 請參閱[移植 **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>將 **Platform::Agile\^** 移植到 **winrt::agile_ref**
C++/CX 中的 **Platform::Agile\^** 類型代表可從任何執行緒存取的 Windows 執行階段類別。 C++/WinRT 對等項目是 [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)。

在 C++/CX 中。

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

在 C++/WinRT 中。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>移植 **Platform::Array\^**
您的選項包括使用初始化清單、**std::array** 或 **std::vector**。 如需詳細資訊以及程式碼範例，請參閱[標準初始化清單](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists)和[標準陣列和向量](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)。

### <a name="port-platformexception-to-winrthresulterror"></a>將 **Platform::Exception\^** 移植到 **winrt::hresult_error**
Windows 執行階段 API 傳回非 S\_OK HRESULT 時，C++/CX 中產生 **Platform::Exception\^** 類型。 C++/WinRT 對等項目是 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

若要移植至 C++/WinRT，請將所有使用 **Platform::Exception\^** 的程式碼變更為使用 **winrt::hresult_error**。

在 C++/CX 中。

```cppcx
catch (Platform::Exception^ ex)
```

在 C++/WinRT 中。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT 提供這些例外類別。

| 例外類型 | 基底類別 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | 呼叫 [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

請注意，每個類別 (透過 **hresult_error** 基底類別) 提供一個 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 函式，傳回錯誤的 HRESULT，以及一個[**訊息**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function)函式，傳回該 HRESULT 的字串表示方法。

以下是在 C++/CX 中擲回例外狀況的範例。

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的對等項目。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>將 **Platform::Object\^** 移植到 **winrt::Windows::Foundation::IInspectable**
就像所有的 C++/WinRT 類型，**winrt::Windows::Foundation::IInspectable** 是一種值類型。 以下是您如何將該類型的變數初始化為 null 的方法。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>將 **Platform::String\^** 移植到 **winrt::hstring**
**Platform::String\^** 等同於 Windows 執行階段 HSTRING ABI 類型。 對於 C++/WinRT，對等項目是 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，您可以使用 C++ 標準程式庫寬字串類型 (例如 **std::wstring**) 來呼叫 Windows 執行階段 API，及/或寬字串常值。 如需詳細資訊和程式碼範例，請參閱 [C++/WinRT 中的字串處理](strings.md)。

使用 C++/CX，您可以存取 [**Platform::String::Data**](https://docs.microsoft.com/cpp/cppcx/platform-string-class?view=vs-2019#data) 屬性來擷取字串做為 C-style **const wchar_t\*** 陣列 (例如，將它傳遞至 **std::wcout**)。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

若要使用 C++/WinRT 進行相同的動作，您可以使用 [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 函式，取得 null 終止的 C 式字串版本，就如同您可以從 **std::wstring** 取得一樣。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

實作採用或傳回字串的 API 時，您通常會變更任何使用 **Platform::String\^** 來使用 **winrt::hstring** 的 C++/CX 程式碼。

以下是採用字串的 C++/CX API 範例。

```cppcx
void LogWrapLine(Platform::String^ str);
```

對於 C++/WinRT，您可以在 [MIDL 3.0](/uwp/midl-3) 中宣告這類 API。

```idl
// LogType.idl
void LogWrapLine(String str);
```

然後，C++/WinRT 工具鏈將為您產生看起來像這樣的原始程式碼。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX 提供 [Object::ToString](/cpp/cppcx/platform-object-class?view=vs-2017#tostring) 方法。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/WinRT 不會直接提供此功能，但您可以轉向使用替代方案。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>重要 API
* [winrt::delegate 結構範本](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 結構](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [以 C++/WinRT 撰寫事件](author-events.md)
* [透過 C++/WinRT 的並行和非同步作業](concurrency.md)
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)
* [Microsoft 介面定義語言 3.0 參考資料](/uwp/midl-3)
* 從 WRL 移到 [C++/WinRT](move-to-winrt-from-wrl.md)
* [C++/WinRT 中的字串處理](strings.md)
