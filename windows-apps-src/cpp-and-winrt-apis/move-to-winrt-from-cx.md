---
description: 本主題示範如何將 C++/CX 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 C++/CX 移到 C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: Windows 10，uwp、標準、c++、cpp、winrt、投影、連接埠、移轉、C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7fbe10e41da1b330d6f5042bea109a8a0e04f8ad
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360154"
---
# <a name="move-to-cwinrt-from-ccx"></a>從 C++/CX 移到 C++/WinRT

本主題說明如何在程式碼移植[ C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)專案，以在其對等[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

## <a name="porting-strategies"></a>移轉策略

如果您想要逐漸移植您C++/CX 程式碼，以C++/WinRT，就可以。 C++/CX 和C++/WinRT 程式碼可以共存於相同的專案，但 XAML 編譯器支援與 Windows 執行階段元件的例外狀況。 對於這些兩個例外狀況，您必須為目標C++/CX 或C++/WinRT 相同專案中的。

> [!IMPORTANT]
> 如果您的專案組建的 XAML 應用程式，則有一個建議的工作流程是先建立新的專案，在 Visual Studio 中使用的其中一個C++/WinRT 專案範本 (請參閱[Visual Studio 支援C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 然後，啟動 複製來源的程式碼和標記從C++/CX 專案。 您可以加入新的 XAML 頁面，具有**專案** \> **加入新項目...** \> **視覺化C++**   > **空白頁 (C++/WinRT)** 。
>
> 或者，您可以使用之因素的程式碼從 XAML 的 Windows 執行階段元件C++/CX 專案移轉。 盡可能將移動C++/CX 程式碼，以及您可以在元件中，然後將 XAML 專案變更為C++/WinRT。 或其他保留的 XAML 專案設為C++/CX，建立新的C++/WinRT 元件，並開始著手移轉C++/CX 程式碼移出 XAML 專案和元件。 您也可以C++/CX 元件專案，以及C++/WinRT 元件專案，在相同方案中，這兩個參考的應用程式專案，然後逐漸從兩個不同連接埠。 請參閱[之間的 Interop C++/WinRT 和C++/CX](interop-winrt-cx.md)如需詳細資訊，在相同專案中使用兩個語言投影。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 以及根命名空間 **Windows** 的 Windows SDK 宣告類型。 投影到 C++/WinRT 的 Windows 類型有與 Windows 類型相同的完整名稱，但它放在 C++ **winrt** 命名空間。 這些不同的命名空間，可讓您以自己的速度從 C++/CX 移植至 C++/WinRT。

記住先前所述的例外狀況，第一個步驟移植C++/CX 專案C++/WinRT 是以手動方式加入C++/WinRT 支援 (請參閱[Visual Studio 支援C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package))。 若要這樣做，請安裝[Microsoft.Windows.CppWinRT NuGet 套件](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)到您的專案。 開啟的專案在 Visual Studio 中，按一下**專案** \> **管理 NuGet 套件...** \> **瀏覽**中，輸入或貼上**Microsoft.Windows.CppWinRT**在 [搜尋] 方塊中，選取項目在搜尋結果中，然後按一下**安裝**安裝適用於該專案的套件。 該項變更的一個效果是專案中支援的 C++/CX 為關閉。 它是個不錯的主意，離開您的相依性的所有已關閉，以便建置訊息會協助您尋找 （和連接埠） 的支援C++/CX，或者您可以支援重新開啟 (在專案屬性中， **C /C++**  \> **一般** \> **使用 Windows 執行階段擴充功能** \> **是 (/ZW)** )，並逐漸連接埠。

請確定該專案屬性**一般** \> **目標平台版本**設 10.0.17134.0 (Windows 10 1803年版) 或更新版本。

在您先行編譯的標頭檔案 (通常是 `pch.h`)，包含 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果包含任何 C++/WinRT 投影 Windows API 標頭 (例如，`winrt/Windows.Foundation.h`)，則您不需要像這樣明確包含 `winrt/base.h`，因為它會自動為您包含。

如果您的專案也會使用 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)類型，請參閱 [從 WRL 移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="parameter-passing"></a>參數傳遞
寫入時C++傳遞 /CX 原始程式碼中， C++/CX 類型為 hat 的函式參數 (\^) 的參考。

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT 中，針對同步函式，您應該使用預設的 `const&` 參數。 將可避免複本以及連鎖額外負荷。 但您的協同程式應該使用透過值傳遞，以確保他們透過值擷取，並避免存留期問題 (如需詳細資訊，請參閱 [並行和非同步作業 C++/WinRT](concurrency.md))。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 物件基本上是保留介面指標以返回 Windows 執行階段物件的值。 當您複製 C++/WinRT 物件時，編譯器複製封裝的介面指標，遞增其參考次數。 最終複本的破壞導致遞減參考次數。 因此，只在必要時收取複製的額外負荷。

## <a name="variable-and-field-references"></a>變數和欄位參考資料
寫入時C++/CX 原始程式碼中，您使用 hat (\^) 變數可以參考 Windows 執行階段物件和箭號 (-&gt;) 運算子來取值 （dereference） hat 變數。

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

當移植到對等項目C++/WinRT 程式碼中，您可以取得方式移除層面，以及變更箭號運算子 (-&gt;) 來點運算子 （.）。 C++/ WinRT 投影類型的值，且不是指標。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

預設建構函式，C + /CX hat 指標 」 將它初始化為 null。 以下是C++/CX 程式碼範例，我們可以在其中建立變數/欄位的正確類型，但具有未初始化的一個。 換句話說，它不一開始請參閱**TextBlock**; 我們想要稍後指定的參考。

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

在對等的C++/WinRT，請參閱[延遲初始化](consume-apis.md#delayed-initialization)。

## <a name="properties"></a>屬性
C++/CX 語言擴充功能包括屬性的概念。 當撰寫 C++/CX 原始碼，如果它就像個欄位時，您可以存取屬性。 標準 C++ 不具屬性的概念，因此在 C++/WinRT 中，您要呼叫 get 並設定函式。

在接下來的範例中，**XboxUserId**、**UserState**、**PresenceDeviceRecords**，和 **Size** 為所有屬性。

### <a name="retrieving-a-value-from-a-property"></a>從屬性擷取一個值
以下是您如何在 C+/CX 取得屬性值的方法。

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

對等項目 C++/WinRT 原始碼呼叫具有相同名稱的屬性，但不含任何參數的函式。

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

請注意，**PresenceDeviceRecords** 函式傳回 Windows 執行階段物件，其本身有一個 **Size** 函式。 由於傳回的物件也是 C++/WinRT 投影類型，因此我們解除參照使用點運算子呼叫 **Size**。

### <a name="setting-a-property-to-a-new-value"></a>將屬性設定給新的值
將屬性設定給新的值，按照類似的模式。 首先，在 C++/CX 中

```cppcx
record->UserState = newValue;
```

若要執行 C++/WinRT 中的對等項目，請您呼叫有相同名稱的函式做為屬性，並傳遞引數。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>建立類別的執行個體
您使用C++/CX 物件，透過它，通常稱為 hat 的控制代碼 (\^) 的參考。 透過 `ref new` 關鍵字建立一個新的物件，依序呼叫 [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) 啟動一個新的執行階段類別執行個體。

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

C++/WinRT 物件是值；讓您可以在堆疊上配置它，或做為物件的欄位。 您 *從未* 使用 `ref new` (或 `new`) 配置 C++/WinRT 物件。 幕後，仍持續呼叫 **RoActivateInstance**。

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

如果初始化資源的成本高，則常延遲直到有實際需求時才將它初始化。

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

已將相同的程式碼移植至 C++/WinRT。 請注意使用 `nullptr` 建構函式。 如需該建構函式的詳細資訊，請參閱 [使用 C++/WinRT 使用 API](consume-apis.md)。

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

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>轉換為衍生的一個基底的執行階段類別
通常會有一個參考-至-基底所參考的物件衍生型別。 在C++//CX 中，使用`dynamic_cast`要*轉換*參考-基底成參考衍生。 `dynamic_cast`其實只是隱藏呼叫能[ **QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))。 以下是一個典型的例子&mdash;所處理的相依性屬性變更事件，而且您想要將轉換成**DependencyObject**回實際擁有的相依性屬性的型別。

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

對等項目C++/WinRT 程式碼取代`dynamic_cast`藉由呼叫[ **IUnknown::try_as** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)函式，其會封裝**QueryInterface**。 您也可以選擇呼叫[ **IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)，相反地，這會擲回例外狀況如果未傳回查詢所需的介面 （您正在要求之類型的預設介面）。 以下是C++/WinRT 程式碼範例。

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
以下是在 C++/CX 中處理事件的一般範例，這種情形下，使用 lambda 函式做為委派。

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

您可以選擇實作委派做為可用功能，或做為指標成員函式，而不是 lambda 函式，。 如需詳細資訊，請參閱 [透過使用 C++/WinRT 中的委派處理事件](handle-events.md)。

如果您正從在內部使用事件和委派的 C++/CX 程式碼基底進行移植 (並非所有二進位檔案)，則 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 可協助您在 C++/WinRT 中複寫該模式。 另請參閱[參數化委派、 簡單的訊號，並在專案中的回呼](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project)。

## <a name="revoking-a-delegate"></a>撤銷委派
您在 C++/CX 中使用 `-=` 運算子撤銷前一個事件註冊。

```cppcx
myButton->Click -= token;
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
myButton().Click(token);
```

如需詳細資訊以及選項，請參閱 [撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>將 C++/CX **平台** 類型對應至 C++/WinRT 類型
C++/CX 在 **平台** 命名空間中提供幾種資料類型。 這些類型不是標準 C++，因此您只能在啟用 Windows 執行階段語言擴充功能時使用它們 (Visual Studio 專案屬性 **C/C++**  > **一般** >  **使用 Windows 執行階段擴充功能** > **是 (/ZW)** )。 下列表格有助於您從 **平台** 類型移植至 C++/WinRT 中他們的對等項目。 一旦完成，因為 C++/WinRT 是標準 C++，您便可以關閉 `/ZW` 選項。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | 請參閱[連接埠**platform:: array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagileref"></a>連接埠**platform:: agile\^** 到**winrt::agile_ref**
**Platform:: agile\^** 輸入C++/CX 表示可以從任何執行緒存取的 Windows 執行階段類別。 C++/WinRT 對等項目是[ **winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref)。

在 C++/CX 中

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

在 C++/WinRT 中。

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>連接埠**platform:: array\^**
您的選項包括使用初始設定式清單**std:: array**，或有**std:: vector**。 如需詳細資訊，以及程式碼範例，請參閱[標準的初始設定式清單](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists)並[標準的陣列和向量](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors)。

### <a name="port-platformexception-to-winrthresulterror"></a>連接埠**platform:: exception\^** 到**winrt::hresult_error**
**Platform:: exception\^** 型別所產生的C++Windows 執行階段 API 會傳回非 S /CX\_[確定] HRESULT。 C++/WinRT 對等項目是 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

要移植到C++/WinRT，變更使用的所有程式碼**platform:: exception\^** 使用**winrt::hresult_error**。

在 C++/CX 中

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

請注意，每個類別 (透過 **hresult_error** 基底類別) 提供一個 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) 函式，傳回錯誤的 HRESULT，以及一個 [**訊息**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function) 函式，傳回該 HRESULT 的字串表示方法。

以下是在 C++/CX 中擲回一個例外狀況的範例。

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的對等項目。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>連接埠**platform:: object\^** 到**winrt::Windows::Foundation::IInspectable**
就像所有的 C++/WinRT 類型，**winrt::Windows::Foundation::IInspectable** 是一種值類型。 以下是您如何將該類型的變數初始化為 null 的方法。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>連接埠**platform:: exception\^** 到**winrt::hstring**
**Platform:: string\^** 相當於 Windows 執行階段 HSTRING ABI 型別。 針對 C++/WinRT，對等項目是 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，您可以使用 C++ 標準程式庫寬字串類型例如 **std::wstring** 來呼叫 Windows 執行階段 API，且/或寬字串常值。 如需詳細資訊和程式碼範例，請參閱 [在 C++/WinRT 中處理字串](strings.md)。

使用C++//CX 中，您可以存取[ **Platform::String::Data** ](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data)屬性來擷取為 C 樣式字串**const wchar_t\*** 陣列 （例如，若要將它傳遞給**std::wcout**)。

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

若要使用 C++/WinRT 進行相同的動作，您可以使用 [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) 函式，取得 null 終止的 C 式字串版本，就如同您可以從 **std::wstring** 取得一樣。

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

說到實作 Api 接受或傳回字串，通常會變更任何C++使用 /CX 程式碼**platform:: string\^** 使用**winrt::hstring**改為。

以下是採用字串的 C++/CX API 範例。

```cppcx
void LogWrapLine(Platform::String^ str);
```

針對 C++/WinRT 您可以像這樣宣告 API 在 [MIDL 3.0](/uwp/midl-3) 中。

```idl
// LogType.idl
void LogWrapLine(String str);
```

然後，C++/WinRT 工具鏈將為您產生看起來像這樣的原始碼。

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++提供 /CX [object:: tostring](/cpp/cppcx/platform-object-class?view=vs-2017#tostring)方法。

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

C++/ WinRT 不會直接提供這項功能，但您可以啟用以替代項目。

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

## <a name="important-apis"></a>重要 API
* [winrt::delegate struct template](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [撰寫中的事件C++/WinRT](author-events.md)
* [並行和非同步作業C++/WinRT](concurrency.md)
* [使用 C++/WinRT 取用 API](consume-apis.md)
* [您可以使用中的委派處理事件C++/WinRT](handle-events.md)
* [C++/WinRT 與 C++/CX 之間的互通性](interop-winrt-cx.md)
* [Microsoft 介面定義語言 3.0 參考](/uwp/midl-3)
* 從 WRL 移到 [C++/WinRT](move-to-winrt-from-wrl.md)
* [字串處理 C + /cli WinRT](strings.md)
