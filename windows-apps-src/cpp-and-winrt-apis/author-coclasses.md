---
description: C++/WinRT 可協助您撰寫傳統的 COM 元件，因為它可協助您撰寫 Windows 執行階段類別。
title: 使用 C++/WinRT 撰寫 COM 元件
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, 標準, c++, cpp, winrt, 投影, 撰寫, COM, 元件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 641d8bd5828fb02321663a6212ce073e228bc657
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804612"
---
# <a name="author-com-components-with-cwinrt"></a>使用 C++/WinRT 撰寫 COM 元件

[C++/WinRT](./intro-to-using-cpp-with-winrt.md) 可協助您撰寫傳統的元件物件模型 (COM) 元件 (或 coclass)，因為有助於撰寫 Windows 執行階段類別。 本主題會說明作法。

## <a name="how-cwinrt-behaves-by-default-with-respect-to-com-interfaces"></a>搭配 COM 介面的 C++/WinRT 有什麼預設行為

C++/ WinRT 的 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 範本是直接或間接衍生您執行階段類別和啟用處理站的基底。

依預設， **winrt：： implements** 會以無訊息方式忽略傳統 COM 介面。 任何針對傳統 COM 介面發出的 **QueryInterface** (QI) 呼叫都會因為 **E_NOINTERFACE** 而失敗。 根據預設， **winrt：： implements** 僅支援 c + +/WinRT 介面。

* **winrt：： IUnknown** 是 c + +/WinRT 介面，因此 **winrt：： implements** 支援 **winrt：： iunknown** 型介面。
* **winrt：： implements** 預設不支援 [**：： IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) 本身。

稍後您會看到如何克服預設不支援的案例。 但是，首先要說明一個程式碼範例，以說明預設會發生什麼事。

```idl
// Sample.idl
namespace MyProject 
{
    runtimeclass Sample
    {
        Sample();
        void DoWork();
    }
}

// Sample.h
#include "pch.h"
#include <shobjidl.h> // Needed only for this file.

namespace winrt::MyProject::implementation
{
    struct Sample : implements<Sample, IInitializeWithWindow>
    {
        IFACEMETHOD(Initialize)(HWND hwnd);
        void DoWork();
    }
}
```

而以下是用來取用 **Sample** 類別的用戶端程式碼。

```cppwinrt
// Client.cpp
Sample sample; // Construct a Sample object via its projection.

// This next line doesn't compile yet.
sample.as<IInitializeWithWindow>()->Initialize(hwnd); 
```

### <a name="enabling-classic-com-support"></a>啟用傳統 COM 支援

好消息是，在您包含任何 c + +/WinRT 標頭之前，讓 **winrt：： implements** 支援傳統 COM 介面所採取的所有動作都是 `unknwn.h` 先包含標頭檔。

您可能可以明確地執行該動作，或藉由包括一些其他標頭檔 (例如 `ole2.h`) 來間接執行。 其中一個建議方法是加入 `wil\cppwinrt.h`標頭檔，這是 [Windows Implementation Libraries (WIL)](https://github.com/Microsoft/wil) 的一部分。 `wil\cppwinrt.h` 標頭檔不只可確保 `unknwn.h` 在 `winrt/base.h` 之前加入，還可以設定項目，讓 C++/WinRT 和 WIL 了解彼此的例外狀況和錯誤代碼。

然後，您可以將 **<>** 用於傳統 COM 介面，而上述範例中的程式碼會進行編譯。

> [!NOTE]
> 在上述範例中，即使在用戶端中啟用傳統 COM 支援 (程式碼) 的程式碼，如果您還沒有在伺服器中啟用傳統 COM 支援 () 執行類別的程式碼，則在用戶端中， **as<>** 的呼叫將會損毀，因為 **IInitializeWithWindow** 的 QI 將會失敗。

### <a name="a-local-non-projected-class"></a>區域 (非投射的) 類別

區域類別是在相同的編譯單位 (應用程式或其他二進位) 中執行和取用的類別;因此沒有它的投射。

以下是僅實作為 *傳統 COM 介面* 的區域類別範例。

```cppwinrt
struct LocalObject :
    winrt::implements<LocalObject, IInitializeWithWindow>
{
    ...
};
```

如果您執行該範例，但未啟用傳統 COM 支援，則下列程式碼會失敗。

```cppwinrt
winrt::make<LocalObject>(); // error: ‘first_interface’: is not a member of ‘winrt::impl::interface_list<>’
```

同樣地， **IInitializeWithWindow** 不會被辨識為 COM 介面，因此 c + +/WinRT 會忽略它。 在 **LocalObject** 範例的情況下，忽略 COM 介面的結果表示 **LocalObject** 完全沒有介面。 但每個 COM 類別都必須執行至少一個介面。

## <a name="a-simple-example-of-a-com-component"></a>COM 元件的簡單範例

以下是使用 C++/WinRT 撰寫 COM 元件的簡單範例。 這是微型應用程式的完整清單，如果將程式碼貼到新 **Windows 主控台應用程式 (C++/WinRT)** 專案的 `pch.h` 和 `main.cpp` 中，即可試用程式碼。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"

struct __declspec(uuid("ddc36e02-18ac-47c4-ae17-d420eece2281")) IMyComInterface : ::IUnknown
{
    virtual HRESULT __stdcall Call() = 0;
};

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    winrt::init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist, IStringable, IMyComInterface>
    {
        HRESULT __stdcall Call() noexcept override
        {
            return S_OK;
        }

        HRESULT __stdcall GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }

        winrt::hstring ToString()
        {
            return L"MyCoclass as a string";
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
    winrt::check_hresult(mycoclass_instance.as<IMyComInterface>()->Call());
}
```

另請參閱[使用 C++/WinRT 取用 COM 元件](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>更貼近現實的有趣範例

本主題接下來的部分，會介紹如何建立一個最小型的主控台應用程式專案，該專案會使用 C++/WinRT，實作基本的 coclass (COM 元件或 COM 類別) 和 Class Factory。 應用程式範例會示範如何使用回呼按鈕來提供快顯通知，而且 coclass (其會實作 **INotificationActivationCallback** COM 介面) 可在使用者點擊快顯通知上的按鈕時，啟動並回呼應用程式。

關於快顯通知功能區域的更多背景資訊，請參閱[傳送本機快顯通知](../design/shell/tiles-and-notifications/send-local-toast.md)。 但是，文件中該章節的程式碼範例皆未使用 C++/WinRT，因此，建議您使用本主題所列的程式碼。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>建立 Windows Console Application 專案 (ToastAndCallback)

請先在 Microsoft Visual Studio 中，建立新的專案。 建立 **Windows 主控台應用程式 (C++/WinRT)** 專案，並將其命名為 *ToastAndCallback*。

開啟 `pch.h`，並在括住的任何 C++/WinRT 標頭之前加上 `#include <unknwn.h>`。 結果顯示如下；您可以將 `pch.h` 的內容取代為此清單。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

開啟 `main.cpp`，並移除專案範本產生的 using 指示詞。 在適當的位置插入以下程式碼 (其會提供我們需要的程式庫、標頭和類型名稱)。 結果顯示如下；您可以將 `main.cpp` 的內容取代為此清單 (我們也在以下清單中移除 `main` 的程式碼，因為我們稍後會替換該函式)。

```cppwinrt
// main.cpp : Defines the entry point for the console application.

#include "pch.h"

#pragma comment(lib, "advapi32")
#pragma comment(lib, "ole32")
#pragma comment(lib, "shell32")

#include <iomanip>
#include <iostream>
#include <notificationactivationcallback.h>
#include <propkey.h>
#include <propvarutil.h>
#include <shlobj.h>
#include <winrt/Windows.UI.Notifications.h>
#include <winrt/Windows.Data.Xml.Dom.h>

using namespace winrt;
using namespace Windows::Data::Xml::Dom;
using namespace Windows::UI::Notifications;

int main() { }
```

尚未建置專案；新增程式碼完成後，將提示您建置並執行。

## <a name="implement-the-coclass-and-class-factory"></a>實作 coclass 和 class factory

在 C++/WinRT 中，可藉由衍生自 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基礎結構的方式，來實作 coclass 和 class factory。 緊接在上述的三個 using 指令詞之後 (且在 `main`之前)，請貼上此程式碼，即可實作您的快顯通知 COM 啟動器元件。

```cppwinrt
static constexpr GUID callback_guid // BAF2FA85-E121-4CC9-A942-CE335B6F917F
{
    0xBAF2FA85, 0xE121, 0x4CC9, {0xA9, 0x42, 0xCE, 0x33, 0x5B, 0x6F, 0x91, 0x7F}
};

std::wstring const this_app_name{ L"ToastAndCallback" };

struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};

struct callback_factory : implements<callback_factory, IClassFactory>
{
    HRESULT __stdcall CreateInstance(
        IUnknown* outer,
        GUID const& iid,
        void** result) noexcept final
    {
        *result = nullptr;

        if (outer)
        {
            return CLASS_E_NOAGGREGATION;
        }

        return make<callback>()->QueryInterface(iid, result);
    }

    HRESULT __stdcall LockServer(BOOL) noexcept final
    {
        return S_OK;
    }
};
```

上面的 coclass 的實作遵循的是[使用 C++/WinRT 撰寫 API](./author-apis.md#if-youre-not-authoring-a-runtime-class) 中示範的模式。 因此，您可以使用相同的技巧來實作 COM 介面以及 Windows 執行階段介面。 COM 元件和 Windows 執行階段類別會透過介面公開其功能。 每個 COM 介面最終都是衍生自 [**IUnknown 介面**](/windows/desktop/api/unknwn/nn-unknwn-iunknown)介面。 Windows 執行階段是以 COM 為基礎 &mdash; 差別在於 Windows 執行階段介面最終是衍生自 [**IInspectable 介面**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (而 **IInspectable** 衍生自 **IUnknown**)。

在上述程式碼的 coclass 中，要實作 **INotificationActivationCallback::Activate** 方法，使用者在快顯通知上點擊回呼按鈕時，所呼叫的函式即是該方法。 但是在呼叫該函式之前，需要先建立一個 coclass 執行個體，這是 **IClassFactory::CreateInstance** 函式的工作。

我們剛剛實作的 coclass 稱為通知的 *COM 啟動器*，而且其類別識別碼 (CLSID) 是採用 `callback_guid` 識別碼 (類型為 **GUID**) 的形式，如上所示。 稍後會以 [開始] 功能表捷徑和 Windows 登錄項目的形式，來使用該識別碼。 COM 啟動器 CLSID，及其相關 COM 伺服器的路徑 (這是我們在此處建置的可執行路徑) 是一種機制，透過該機制，在點擊其回呼按鈕時 (無論是否在「控制中心」點擊通知)，快顯通知會知道要建立的執行個體類別。

## <a name="best-practices-for-implementing-com-methods"></a>實作 COM 方法的最佳做法

錯誤處理和資源管理的技巧可以並行採用。 使用例外狀況會比使用錯誤碼更加方便實用。 如果您部署的是 resource-acquisition-is-initialization (RAII) 慣用語，那麼可以不用明確檢查錯誤碼，之後再明確釋出資源。 此類明確檢查會讓您的程式碼過於複雜，而且會有許多地方隱藏錯誤。 請改用 RAII，並擲回/攔截例外狀況。 如此一來，您的資源配置就不會出現異常狀況，且程式碼能保持簡單明瞭。

不過，您不得允許例外狀況逸出您的 COM 方法實作。 您可以在 COM 方法上使用 `noexcept` 規範，藉以確保這點。 只要在方法結束之前處理例外狀況，就可以在方法的呼叫歷程圖中的任何位置擲回例外狀況。 如果您使用 `noexcept`，但隨後允許例外狀況逸出您的方法，則會終止您的應用程式。

## <a name="add-helper-types-and-functions"></a>新增協助程式類型和函式

在此步驟中，會新增一些其他程式碼使用的協助程式類型和函式。 因此，在 `main` 之前，請新增以下內容。

```cppwinrt
struct prop_variant : PROPVARIANT
{
    prop_variant() noexcept : PROPVARIANT{}
    {
    }

    ~prop_variant() noexcept
    {
        clear();
    }

    void clear() noexcept
    {
        WINRT_VERIFY_(S_OK, ::PropVariantClear(this));
    }
};

struct registry_traits
{
    using type = HKEY;

    static void close(type value) noexcept
    {
        WINRT_VERIFY_(ERROR_SUCCESS, ::RegCloseKey(value));
    }

    static constexpr type invalid() noexcept
    {
        return nullptr;
    }
};

using registry_key = winrt::handle_type<registry_traits>;

std::wstring get_module_path()
{
    std::wstring path(100, L'?');
    uint32_t path_size{};
    DWORD actual_size{};

    do
    {
        path_size = static_cast<uint32_t>(path.size());
        actual_size = ::GetModuleFileName(nullptr, path.data(), path_size);

        if (actual_size + 1 > path_size)
        {
            path.resize(path_size * 2, L'?');
        }
    } while (actual_size + 1 > path_size);

    path.resize(actual_size);
    return path;
}

std::wstring get_shortcut_path()
{
    std::wstring format{ LR"(%ProgramData%\Microsoft\Windows\Start Menu\Programs\)" };
    format += (this_app_name + L".lnk");

    auto required{ ::ExpandEnvironmentStrings(format.c_str(), nullptr, 0) };
    std::wstring path(required - 1, L'?');
    ::ExpandEnvironmentStrings(format.c_str(), path.data(), required);
    return path;
}
```

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>實作其餘函式和 wmain 進入點函式

刪除 `main` 函式，並在其位置貼上此程式碼清單，其中包含註冊 coclass 的程式碼，然後傳遞可回呼您應用程式的快顯通知。

```cppwinrt
void register_callback()
{
    DWORD registration{};

    winrt::check_hresult(::CoRegisterClassObject(
        callback_guid,
        make<callback_factory>().get(),
        CLSCTX_LOCAL_SERVER,
        REGCLS_SINGLEUSE,
        &registration));
}

void create_shortcut()
{
    auto link{ winrt::create_instance<IShellLink>(CLSID_ShellLink) };
    std::wstring module_path{ get_module_path() };
    winrt::check_hresult(link->SetPath(module_path.c_str()));

    auto store = link.as<IPropertyStore>();
    prop_variant value;
    winrt::check_hresult(::InitPropVariantFromString(this_app_name.c_str(), &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ID, value));
    value.clear();
    winrt::check_hresult(::InitPropVariantFromCLSID(callback_guid, &value));
    winrt::check_hresult(store->SetValue(PKEY_AppUserModel_ToastActivatorCLSID, value));

    auto file{ store.as<IPersistFile>() };
    std::wstring shortcut_path{ get_shortcut_path() };
    winrt::check_hresult(file->Save(shortcut_path.c_str(), TRUE));

    std::wcout << L"In " << shortcut_path << L", created a shortcut to " << module_path << std::endl;
}

void update_registry()
{
    std::wstring key_path{ LR"(SOFTWARE\Classes\CLSID\{????????-????-????-????-????????????})" };
    ::StringFromGUID2(callback_guid, key_path.data() + 23, 39);
    key_path += LR"(\LocalServer32)";
    registry_key key;

    winrt::check_win32(::RegCreateKeyEx(
        HKEY_CURRENT_USER,
        key_path.c_str(),
        0,
        nullptr,
        0,
        KEY_WRITE,
        nullptr,
        key.put(),
        nullptr));
    ::RegDeleteValue(key.get(), nullptr);

    std::wstring path{ get_module_path() };

    winrt::check_win32(::RegSetValueEx(
        key.get(),
        nullptr,
        0,
        REG_SZ,
        reinterpret_cast<BYTE const*>(path.c_str()),
        static_cast<uint32_t>((path.size() + 1) * sizeof(wchar_t))));

    std::wcout << L"In " << key_path << L", registered local server at " << path << std::endl;
}

void create_toast()
{
    XmlDocument xml;

    std::wstring toastPayload
    {
        LR"(
<toast>
  <visual>
    <binding template='ToastGeneric'>
      <text>)"
    };
    toastPayload += this_app_name;
    toastPayload += LR"(
      </text>
    </binding>
  </visual>
  <actions>
    <action content='Call back )";
    toastPayload += this_app_name;
    toastPayload += LR"(
' arguments='the_args' activationKind='Foreground' />
  </actions>
</toast>)";
    xml.LoadXml(toastPayload);

    ToastNotification toast{ xml };
    ToastNotifier notifier{ ToastNotificationManager::CreateToastNotifier(this_app_name) };
    notifier.Show(toast);
    ::Sleep(50); // Give the callback chance to display.
}

void LaunchedNormally(HANDLE, INPUT_RECORD &, DWORD &);
void LaunchedFromNotification(HANDLE, INPUT_RECORD &, DWORD &);

int wmain(int argc, wchar_t * argv[], wchar_t * /* envp */[])
{
    winrt::init_apartment();

    register_callback();

    HANDLE consoleHandle{ ::GetStdHandle(STD_INPUT_HANDLE) };
    INPUT_RECORD buffer{};
    DWORD events{};
    ::FlushConsoleInputBuffer(consoleHandle);

    if (argc == 1)
    {
        LaunchedNormally(consoleHandle, buffer, events);
    }
    else if (argc == 2 && wcscmp(argv[1], L"-Embedding") == 0)
    {
        LaunchedFromNotification(consoleHandle, buffer, events);
    }
}

void LaunchedNormally(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    try
    {
        bool runningAsAdmin{ ::IsUserAnAdmin() == TRUE };
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (administrator)." : L" (NOT as administrator).") << std::endl;

        if (runningAsAdmin)
        {
            create_shortcut();
            update_registry();
        }

        std::wcout << std::endl << L"Press 'T' to display a toast notification (press any other key to exit)." << std::endl;

        ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
        if (towupper(buffer.Event.KeyEvent.uChar.UnicodeChar) == L'T')
        {
            create_toast();
        }
    }
    catch (winrt::hresult_error const& e)
    {
        std::wcout << L"Error: " << e.message().c_str() << L" (" << std::hex << std::showbase << std::setw(8) << static_cast<uint32_t>(e.code()) << L")" << std::endl;
    }
}

void LaunchedFromNotification(HANDLE consoleHandle, INPUT_RECORD & buffer, DWORD & events)
{
    ::Sleep(50); // Give the callback chance to display its message.
    std::wcout << std::endl << L"Press any key to exit." << std::endl;
    ::ReadConsoleInput(consoleHandle, &buffer, 1, &events);
}
```

## <a name="how-to-test-the-example-application"></a>如何測試範例應用程式

請組建應用程式，然後以系統管理員身分執行至少一次，以便執行註冊、其他設定和程式碼。 有一種方法是以系統管理員身分執行 Visual Studio，然後從 Visual Studio 執行該應用程式。 在工作列中的 Visual Studio 上按右鍵，即會顯示捷徑清單，請在捷徑清單上的 Visual Studio 按右鍵，然後按一下 [以系統管理員身分執行]。 同意提示，然後開啟專案。 當您執行應用程式時，會顯示訊息，註明是否正在以系統管理員身分執行應用程式。 如果不是，則不會執行註冊和其他設定。 該註冊和其他設定必須執行至少一次，才能使應用程式正常運作。

無論是否以系統管理員的身分執行應用程式，按 T 會顯示快顯通知。 您也可以直接在彈出的快顯通知或控制中心中，按一下 [回呼 ToastAndCallback] 按鈕，如此會啟動您的應用程式，將 coclass 具現化，並執行 **INotificationActivationCallback::Activate** 方法。

## <a name="in-process-com-server"></a>同處理序 COM 伺服器

上述的 *ToastAndCallback* 範例應用程式是做為本機 (或跨處理序) COM 伺服器。 這是透過 [LocalServer32](/windows/desktop/com/localserver32) Windows 登錄機碼 (用於登錄其 coclass 的 CLSID) 來表示。 本機 COM 伺服器會在可執行的二進位 (`.exe`) 內託管其 coclass。

或者 (可能性高更高) 可以選擇在動態連結程式庫 (`.dll`) 中託管您的 coclass。 DLL 形式的 COM 伺服器稱為同處理序 COM 伺服器，這是透過使用 [InprocServer32](/windows/desktop/com/inprocserver32) Windows 登錄機碼登錄的 CLSID 來表示。

### <a name="create-a-dynamic-link-library-dll-project"></a>建立動態連結程式庫 (DLL) 專案

您可以在 Microsoft Visual Studio 中建立新專案，藉此開始建立同處理序 COM 伺服器的任務。 ) 專案中建立 **Visual C++**  >  **Windows 桌面**  >  **動態連結程式庫 (DLL** 。

若要將 C++/WinRT 支援新增至新專案，請依照[修改 Windows 傳統型應用程式專案以新增 C++/WinRT 支援](./get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)所述的步驟進行。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>實作 coclass、class factory 以及同處理序伺服器的匯出

開啟 `dllmain.cpp`，將其加入程式碼清單，如下所示。

如果您已經有 DLL 實作了 C++/WinRT Windows 執行階段類別，則就已經具備了 **DllCanUnloadNow** 函式，如下所示。 如果要將 coclasses 加入該 DLL，則可加入 **DllGetClassObject** 函式。

如果沒有現有 [Windows 執行階段 C++ 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 程式碼需要保持相容，則可以將 WRL 組件從顯示的程式碼中移除。

```cppwinrt
// dllmain.cpp

struct MyCoclass : winrt::implements<MyCoclass, IPersist>
{
    HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
    {
        *id = IID_IPersist; // Doesn't matter what we return, for this example.
        return S_OK;
    }
};

struct __declspec(uuid("85d6672d-0606-4389-a50a-356ce7bded09"))
    MyCoclassFactory : winrt::implements<MyCoclassFactory, IClassFactory>
{
    HRESULT STDMETHODCALLTYPE CreateInstance(IUnknown *pUnkOuter, REFIID riid, void **ppvObject) noexcept override
    {
        try
        {
            return winrt::make<MyCoclass>()->QueryInterface(riid, ppvObject);
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }

    HRESULT STDMETHODCALLTYPE LockServer(BOOL fLock) noexcept override
    {
        // ...
        return S_OK;
    }

    // ...
};

HRESULT __stdcall DllCanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT __stdcall DllGetClassObject(GUID const& clsid, GUID const& iid, void** result)
{
    try
    {
        *result = nullptr;

        if (clsid == __uuidof(MyCoclassFactory))
        {
            return winrt::make<MyCoclassFactory>()->QueryInterface(iid, result);
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetClassObject(clsid, iid, result);
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...)
    {
        return winrt::to_hresult();
    }
}
```

### <a name="support-for-weak-references"></a>支援弱式參考

另請參閱 [C++/WinRT 中的弱式參考](weak-references.md#weak-references-in-cwinrt)。

如果您的類型實作的是 [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (或衍生自 **IInspectable** 的任何介面)，則 C++/WinRT (具體來說，是 [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) 基礎架構範本) 會實作 [**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource)。

這是因為 **IWeakReferenceSource** 和 [**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference) 是專為 Windows 執行階段類型所設計的。 因此，只要將 **winrt::Windows::Foundation::IInspectable** (或衍生自 **IInspectable** 的介面) 加入實作，即可開啟您的 coclass 的弱式參考支援。

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>重要 API
* [IInspectable 介面](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown 介面](/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API](./author-apis.md)
* [使用 C++/WinRT 取用 COM 元件](consume-com.md)
* [傳送本機快顯通知](../design/shell/tiles-and-notifications/send-local-toast.md)