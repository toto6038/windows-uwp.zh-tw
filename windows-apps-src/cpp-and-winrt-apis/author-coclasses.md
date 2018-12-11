---
description: C + + WinRT 可協助您撰寫傳統 COM 元件，就如同它可協助您撰寫 Windows 執行階段類別。
title: 使用 C++/WinRT 撰寫 COM 元件
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 作者，COM、 元件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e6b77f8be6c75070336ad48f0c6471fc0a824a4c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8927362"
---
# <a name="author-com-components-with-cwinrt"></a>使用 C++/WinRT 撰寫 COM 元件

[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)可協助您撰寫傳統元件物件模型 (COM) 元件 （或 coclasses），就如同它可協助您撰寫 Windows 執行階段類別。 以下是簡單的圖例，如果您貼到程式碼，您可以測試出`pch.h`和`main.cpp`的新**Windows 主控台應用程式 (C + + WinRT)** 專案。

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

另請參閱[取用 COM 元件使用 C + + /winrt](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>更逼真且有趣的範例

本主題的其餘部分逐步解說如何建立最少的主控台應用程式專案使用 C + + 來實作基本 coclass （COM 元件或 COM 類別） 和類別原廠 WinRT。 範例應用程式示範如何提供回呼按鈕與快顯通知，並 coclass （這會實作**INotificationActivationCallback** COM 介面） 可讓應用程式啟動並呼叫返回時使用者按一下該按鈕的快顯通知。

[傳送本機快顯通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)位於快顯通知功能區域相關的詳細背景。 無文件的該區段中的程式碼範例使用 C + + /winrt，不過，因此我們建議您想要顯示在本主題中的程式碼。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>建立 Windows 主控台應用程式專案 (ToastAndCallback)

在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立**Visual c + +** > **的 Windows 桌面** > **Windows 主控台應用程式 (C + + /winrt)** 專案，並將它命名為*ToastAndCallback*。

開啟`pch.h`，並新增`#include <unknwn.h>`之前包含任何 C + /winrt 標頭。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

開啟`main.cpp`，並移除 using 指示詞所產生的專案範本。 在其位置，貼上下列程式碼 （這可讓我們的程式庫、 標頭和我們需要的類型名稱）。

```cppwinrt
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
```

## <a name="implement-the-coclass-and-class-factory"></a>實作 coclass 和類別 factory

在 C + + /winrt，您實作 coclasses 和類別 factory 衍生自[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)基礎結構。 在三個 using 指示詞上述後立即 (和之前`main`)，貼上此程式碼來實作您的快顯通知 COM 啟動者元件。

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

上述 coclass 實作如下所示的相同模式[撰寫 Api 使用 C + + /winrt](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)。 因此，您可以使用相同的技術來實作 COM 介面，以及 Windows 執行階段介面。 COM 元件和 Windows 執行階段類別會公開他們透過介面的功能。 每個 COM 介面最終衍生自[**IUnknown 介面**](https://msdn.microsoft.com/library/windows/desktop/ms680509)的介面。 Windows 執行階段根據 COM&mdash;一個區別正在 Windows 執行階段介面最終是衍生自[**IInspectable 介面**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)（而且**IInspectable**衍生自**IUnknown**）。

在上述的程式碼中 coclass，我們會實作**INotificationActivationCallback::Activate**方法，也就是當使用者按一下快顯通知上的 [回呼] 按鈕時所呼叫的函式。 但是，該函式可呼叫之前，您必須建立，coclass 的執行個體，而這就是**IClassFactory::CreateInstance**函式的工作。

我們實作 coclass 稱為*COM 啟動者*的通知，以及它有其類別識別碼 (CLSID) 的形式`callback_guid`識別項 (of 類型**的 GUID**），您看到以上。 我們會使用該識別碼更新版本，形式的 [開始] 功能表捷徑和 Windows 登錄項目。 COM 啟動者 CLSID，以及其相關聯的 COM 伺服器 （也就是我們要在這裡建置的可執行檔的路徑） 的路徑是所依據的快顯通知會知道什麼類別來建立執行個體的當其回呼按鈕的機制 (是否通知按一下控制中心中或不）。

## <a name="best-practices-for-implementing-com-methods"></a>實作 COM 方法的最佳做法

錯誤處理和資源管理技術可以瀏覽手中手。 它是更方便且不方便使用比錯誤碼的例外狀況。 此外，如果您採用資源-取得-是-初始化 (RAII) 慣用語，則您可以避免明確檢查錯誤碼，並明確釋放資源。 這類明確檢查進行更錯綜複雜多餘的情況下，您的程式碼，它提供 bug 充分的隱藏的地方。 反之，請使用 RAII，並擲回/catch 例外狀況。 如此一來，您的資源配置是異常安全和您的程式碼很簡單。

不過，您我允許逸出您的 COM 方法實作的例外狀況。 您可以確保使用`noexcept`在您的 COM 方法的規範。 只要您處理這類您方法結束之前是方法的確定擲回任何位置中，呼叫圖形中的例外狀況。 如果您使用`noexcept`，但您再允許來逸出您方法中，例外狀況，則您的應用程式會終止。

## <a name="add-helper-types-and-functions"></a>新增協助程式類型和函式

在此步驟中，我們將新增一些協助程式類型和函式，可讓程式碼的其餘部分使用。 是的之前, `main`，新增下列項目。

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>實作的剩餘的函式，以及 wmain 進入點函式

專案範本會產生`main`為您的函式。 刪除這個`main`函式，並在其位置貼上此程式碼清單，其中包括程式碼來登錄您 coclass，然後傳遞能夠呼叫回您的應用程式的快顯通知。

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
        std::wcout << this_app_name << L" is running" << (runningAsAdmin ? L" (Administrator)." : L".") << std::endl;

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

## <a name="how-to-test-the-example-application"></a>如何在測試範例應用程式

建置應用程式，並再以系統管理員身分註冊，以及其他安裝程式執行的程式碼會造成中至少一次執行。 您是否正在執行以系統管理員身分，然後按下 T ' 會造成顯示快顯通知。 您可以按一下無論是直接從快顯通知突然向上，或從在控制中心和您的應用程式將會啟動、 具現化，coclass 和 INotificationActivationCallback**的**回撥 ToastAndCallback**按鈕:: As 啟用**執行方法。

## <a name="in-process-com-server"></a>同處理序 COM 伺服器

上述*ToastAndCallback*範例應用程式的函式做為本機 （或處理程序） COM 伺服器。 這表示[LocalServer32](/windows/desktop/com/localserver32) Windows 登錄機碼，您用來註冊其 coclass 的 CLSID。 本機的 COM 伺服器裝載其 coclass(es) 內可執行的二進位檔 ( `.exe`)。

或者 （和可以說是更有可能），您可以選擇來裝載您 coclass(es) 動態連結程式庫內 ( `.dll`)。 DLL 的形式的 COM 伺服器稱為同處理序 COM 伺服器，它由 Clsid 將使用[InprocServer32](/windows/desktop/com/inprocserver32) Windows 登錄機碼來登錄。

### <a name="create-a-dynamic-link-library-dll-project"></a>建立動態連結程式庫 (DLL) 專案

您可以開始在 Microsoft Visual Studio 中建立新的專案中建立處理程序的 COM 伺服器的工作。 建立**Visual c + +** > **的 Windows 桌面** > **動態連結程式庫 (DLL)** 專案。

若要新增 C + + WinRT 支援新的專案，請依照下列步驟中所述[修改 Windows 傳統型應用程式專案新增 C + + /winrt 支援](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>實作 coclass、 類別原廠，以及同處理序伺服器匯出

開啟`dllmain.cpp`，並將如下所示的程式碼清單新增到它。

如果您已經有 DLL，實作 C + + /winrt Windows 執行階段類別，則您已經將會有**DllCanUnloadNow**函式，如下所示。 如果您想要將 coclasses 新增到該 DLL，您可以新增**DllGetClassObject**函式。

如果沒有您想要繼續留，與相容的現有[Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)程式碼，則您可以從顯示的程式碼移除 WRL 組件。

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
            *ppvObject = winrt::make<MyCoclass>().get();
            return S_OK;
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

### <a name="support-for-weak-references"></a>弱式參考的支援

另請參閱[弱式參考資料在 C + + /winrt](weak-references.md#weak-references-in-cwinrt)。

C + + /winrt （具體而言， [**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)基礎結構範本） 為您實作[**IWeakReferenceSource**](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) （或任何衍生自**IInspectable**的介面），實作您的類型。

這是因為**IWeakReferenceSource**和[**IWeakReference**](/windows/desktop/api/weakreference/nn-weakreference-iweakreference)專為 Windows 執行階段類型。 因此，您可以開啟弱式參考支援適用於您 coclass 只需透過將**winrt::Windows::Foundation::IInspectable** （或衍生自**IInspectable**介面） 新增到您的實作。

```cppwinrt
struct MyCoclass : winrt::implements<MyCoclass, IMyComInterface, winrt::Windows::Foundation::IInspectable>
{
    //  ...
};
```

## <a name="important-apis"></a>重要 API
* [IInspectable 介面](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [IUnknown 介面](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API ](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [使用 C++/WinRT 來使用 COM 元件](consume-com.md)
* [傳送本機快顯通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
