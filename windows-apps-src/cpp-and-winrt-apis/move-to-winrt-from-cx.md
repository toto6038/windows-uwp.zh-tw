---
author: stevewhims
description: 本主題示範如何將 C++/CX 程式碼移植到其在 C++/WinRT 中的對等項目。
title: 從 C++/CX 移到 C++/WinRT
ms.author: stwhi
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10，uwp、標準、c++、cpp、winrt、投影、連接埠、移轉、C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: da6226158056cbbf0b51b46be0b17fe7e478dd01
ms.sourcegitcommit: 929fa4b3273862dcdc76b083bf6c3b2c872dd590
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1935746"
---
# <a name="move-to-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-from-ccx"></a>從 C++/CX 移到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 
本主題示範如何將 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 程式碼移植到其在 C++/WinRT 中的對等項目。

> [!NOTE]
> [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 以及根命名空間 **Windows** 的 Windows SDK 宣告類型。 投影到 C++/WinRT 的 Windows 類型有與 Windows 類型相同的完整名稱，但它放在 C++ **winrt** 命名空間。 這些不同的命名空間，可讓您以自己的速度從 C++/CX 移植至 C++/WinRT。

移植 C+/WinRT 中的第一個步驟是手動新增 C++/WinRT 支援您的專案 (請參閱 [Visual Studio 支援 C++/WinRT，以及 VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix))。 若要這樣做，請編輯您的 `.vcxproj` 檔案、尋找 `<PropertyGroup Label="Globals">`，然後在群組屬性裡設定屬性 `<CppWinRTEnabled>true</CppWinRTEnabled>`。 該項變更的一個效果是專案中支援的 C++/CX 為關閉。 最好關閉支援，讓您可以找出並移植您 C++/CX 上所有的相依性，或您可以重新將支援開啟 (專案屬性中，**C/C++** \> **一般** \> ** 使用 Windows 執行階段擴充功能 ** \> **是 (/ZW)**)，並逐漸移植。

將專案屬性**一般** \> **目標平台版本**設置為 10.0.17134.0 (Windows 10，版本 1803) 或更高。

在您先行編譯的標頭檔案 (通常是 `pch.h`)，包含 `winrt/base.h`。

```cppwinrt
#include <winrt/base.h>
```

如果包含任何 C++/WinRT 投影 Windows API 標頭 (例如，`winrt/Windows.Foundation.h`)，則您不需要像這樣明確包含 `winrt/base.h`，因為它會自動為您包含。

如果您的專案也會使用 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)類型，請參閱 [從 WRL 移到 C++/WinRT](move-to-winrt-from-wrl.md)。

## <a name="parameter-passing"></a>參數傳遞
撰寫 C++/CX 原始碼時，您傳遞 C++/CX 類型做為控制帽 (\^) 參考的函式參數。

```cpp
void LogPresenceRecord(PresenceRecord^ record);
```

C++/WinRT 中，針對同步函式，您應該使用預設的 `const&` 參數。 將可避免複本以及連鎖額外負荷。 但您的協同程式應該使用透過值傳遞，以確保他們透過值擷取，並避免存留期問題 (如需詳細資訊，請參閱 [並行和非同步作業 C++/WinRT](concurrency.md))。

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

C++/WinRT 物件基本上是保留介面指標以返回 Windows 執行階段物件的值。 當您複製 C++/WinRT 物件時，編譯器複製封裝的介面指標，遞增其參考次數。 最終複本的破壞導致遞減參考次數。 因此，只在必要時收取複製的額外負荷。

## <a name="variable-and-field-references"></a>變數和欄位參考資料
撰寫 C++/CX 原始碼時，使用控制帽 (\^) 變數參考 Windows 執行階段物件，以及箭號 (-&gt;) 運算子以取值控制帽變數。

```cpp
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

當轉換成對等項目 C++/WinRT 程式碼，您基本上要移除控制帽和將箭號運算子 (-&gt;) 變更為點運算子（.），因為 C++/WinRT 投影類型是值，而不是指標。

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

## <a name="properties"></a>屬性
C++/CX 語言擴充功能包括屬性的概念。 當撰寫 C++/CX 原始碼，如果它就像個欄位時，您可以存取屬性。 標準 C++ 不具屬性的概念，因此在 C++/WinRT 中，您要呼叫 get 並設定函式。

在接下來的範例中，**XboxUserId**、**UserState**、**PresenceDeviceRecords**，和 **Size** 為所有屬性。

### <a name="retrieving-a-value-from-a-property"></a>從屬性擷取一個值
以下是您如何在 C+/CX 取得屬性值的方法。

```cpp
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

```cpp
record->UserState = newValue;
```

若要執行 C++/WinRT 中的對等項目，請您呼叫有相同名稱的函式做為屬性，並傳遞引數。

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>建立類別的執行個體
透過一個控制代碼使用 C++/CX 物件，通常稱它為控制帽 (\^) 參考資料。 透過 `ref new` 關鍵字建立一個新的物件，依序呼叫 [**RoActivateInstance**](https://msdn.microsoft.com/library/br224646) 啟動一個新的執行階段類別執行個體。

```cpp
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

```cpp
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

## <a name="event-handling-with-a-delegate"></a>使用委派的事件處理
以下是在 C++/CX 中處理事件的一般範例，這種情形下，使用 lambda 函式做為委派。

```cpp
auto token = myButton->Click += ref new RoutedEventHandler([&](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
});
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
auto token = myButton().Click([&](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
});
```

您可以選擇實作委派做為可用功能，或做為指標成員函式，而不是 lambda 函式，。 如需詳細資訊，請參閱 [透過使用 C++/WinRT 中的委派處理事件](handle-events.md)。

如果您正從在內部使用事件和委派的 C++/CX 程式碼基底進行移植 (並非所有二進位檔案)，則 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 可協助您在 C++/WinRT 中複寫該模式。 也請參閱 [winrt::delegate&lt;...T&gt;](author-events.md#winrtdelegate-t)。

## <a name="revoking-a-delegate"></a>撤銷委派
您在 C++/CX 中使用 `-=` 運算子撤銷前一個事件註冊。

```cpp
myButton->Click -= token;
```

這是 C++/WinRT 中的對等項目。

```cppwinrt
myButton().Click(token);
```

如需詳細資訊以及選項，請參閱 [撤銷已註冊的委派](handle-events.md#revoke-a-registered-delegate)。

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>將 C++/CX **平台** 類型對應至 C++/WinRT 類型
C++/CX 在 **平台** 命名空間中提供幾種資料類型。 這些類型不是標準 C++，因此您只能在啟用 Windows 執行階段語言擴充功能時使用它們 (Visual Studio 專案屬性 **C/C++** > **一般** > ** 使用 Windows 執行階段擴充功能** > **是 (/ZW)**)。 下列表格有助於您從 **平台** 類型移植至 C++/WinRT 中他們的對等項目。 一旦完成，因為 C++/WinRT 是標準 C++，您便可以關閉 `/ZW` 選項。

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>將 **Platform::Object\^** 移植到 **winrt::Windows::Foundation::IInspectable**
就像所有的 C++/WinRT 類型，**winrt::Windows::Foundation::IInspectable** 是一種值類型。 以下是您如何將該類型的變數初始化為 null 的方法。

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>將 **Platform::String\^** 移植至 **winrt::hstring**
**Platform::String\^** 等同於 Windows 執行階段 HSTRING ABI 類型。 針對 C++/WinRT，對等項目是 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring)。 但使用 C++/WinRT，您可以使用 C++ 標準程式庫寬字串類型例如 **std::wstring** 來呼叫 Windows 執行階段 API，且/或寬字串常值。 如需詳細資訊和程式碼範例，請參閱 [在 C++/WinRT 中處理字串](strings.md)。

使用 C++/CX，您可以存取 [**Platform::String::Data**](https://docs.microsoft.com/en-us/cpp/cppcx/platform-string-class#data) 屬性來擷取字串做為 C-style **const wchar_t\*** 陣列 (例如，將它傳遞至 **std::wcout**)。

```C++
auto var = titleRecord->TitleName->Data();
```

若要使用 C++/WinRT 進行相同的動作，您可以使用 [**hstring::c_str**](/uwp/api/windows.foundation.uri#hstringcstr-function) 函式，取得 null 終止的 C 式字串版本，就如同您可以從 **std::wstring** 取得一樣。

```C++
auto var = titleRecord.TitleName().c_str();
```

實作採用或傳回字串的 API 時，您通常會變更任何使用 **Platform::String\^** 來使用 **winrt::hstring** 的 C++/CX 程式碼。

以下是採用字串的 C++/CX API 範例。

```cpp
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

### <a name="port-platformexception-to-winrthresulterror"></a>將 **Platform::Exception\^** 移植至 **winrt::hresult_error**
Windows 執行階段 API 傳回非 S\_OK HRESULT 時，C++/CX 中產生 **Platform::Exception\^** 類型。 C++/WinRT 對等項目是 [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)。

若要移植至 C++/WinRT，請變更所有使用 **Platform::Exception\^** 的程式碼來使用 **winrt::hresult_error**。

在 C++/CX 中

```cpp
catch (Platform::Exception^ ex)
```

在 C++/WinRT 中。

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT 提供這些例外類別。

| 例外類型 | 基底類別 | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | 呼叫 [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) |
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

請注意，每個類別 (透過 **hresult_error** 基底類別) 提供一個 [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrortoabi-function) 函式，傳回錯誤的 HRESULT，以及一個 [**訊息**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresulterrormessage-function) 函式，傳回該 HRESULT 的字串表示方法。

以下是在 C++/CX 中擲回一個例外狀況的範例。

```cpp
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

以及 C++/WinRT 中的對等項目。

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

## <a name="important-apis"></a>重要 API
* [winrt::delegate 結構範本](/uwp/cpp-ref-for-winrt/delegate)
* [winrt::hresult_error 結構](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring 結構](/uwp/cpp-ref-for-winrt/hstring)
* [Winrt 命名空間](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相關主題
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [在 C++/WinRT 中撰寫事件](author-events.md)
* [使用 C++/WinRT 的並行和非同步作業。](concurrency.md)
* [使用 C++/WinRT 使用 API ](consume-apis.md)
* [藉由在 C++/WinRT 使用委派來處理事件](handle-events.md)
* [Microsoft 介面定義語言 3.0 參考資料](/uwp/midl-3)
* [從 WRL 移到 C++/WinRT](move-to-winrt-from-wrl.md)
* [C++/WinRT 中的字串處理](strings.md)
