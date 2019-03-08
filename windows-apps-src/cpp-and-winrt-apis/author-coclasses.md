---
description: C + + /cli WinRT 可協助您撰寫傳統的 COM 元件，就像它可協助您撰寫 Windows 執行階段類別。
title: 使用 C++/WinRT 撰寫 COM 元件
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 作者、 COM、 元件
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e6b77f8be6c75070336ad48f0c6471fc0a824a4c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616563"
---
# <a name="author-com-components-with-cwinrt"></a>使用 C++/WinRT 撰寫 COM 元件

[C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)可協助您撰寫傳統元件物件模型 (COM) 元件 （或 coclass），就像它可協助您撰寫 Windows 執行階段類別。 以下是簡單的說明，如果您貼上程式碼，您可以測試出`pch.h`並`main.cpp`的新**Windows 主控台應用程式 (C + + /cli WinRT)** 專案。

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

另請參閱[取用 COM 元件使用 C + + /cli WinRT](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>更真實且有趣的範例

本主題的其餘部分將逐步建立最基本的主控台應用程式專案使用 C + + /cli WinRT 來實作基本的 coclass （COM 元件或 COM 類別） 和類別的 factory。 範例應用程式示範如何傳遞回呼按鈕和 coclass 的快顯通知 (它會實作**INotificationActivationCallback** COM 介面) 可讓應用程式啟動並呼叫當使用者按一下該按鈕，在快顯通知後。

快顯通知功能區域的詳細背景，請參閱[傳送本機的快顯通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)。 沒有任何文件中的該節中的程式碼範例使用 C + + /cli WinRT，不過，因此，建議您偏好使用本主題中所顯示的程式碼。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>建立 Windows 主控台應用程式專案 (ToastAndCallback)

在 Microsoft Visual Studio 中，從建立新的專案開始。 建立**Visual c + +** > **Windows Desktop** > **Windows 主控台應用程式 (C + + /cli WinRT)** 專案，並將它命名*ToastAndCallback*。

開啟`pch.h`，並新增`#include <unknwn.h>`之前包含任何 C + /cli WinRT 標頭。

```cppwinrt
// pch.h
#pragma once
#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
```

開啟`main.cpp`，並移除 using 指示詞所產生的專案範本。 在其位置中，貼上下列程式碼 （這會提供程式庫、 標頭，以及我們需要的類型名稱）。

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

## <a name="implement-the-coclass-and-class-factory"></a>實作的 coclass 和類別的 factory

在 C + + /cli WinRT，您實作 coclass 和 class factory，藉由衍生自[ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements)基底結構。 後面三個 using 指示詞如上所示 (以及之前`main`)，貼上下列程式碼來實作您的快顯通知 COM 啟動項元件。

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

上述 coclass 的實作會遵循相同的模式所示[作者 Api，使用 C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)。 因此，您可以使用相同的技巧來實作 COM 介面，以及 Windows 執行階段介面。 COM 元件和 Windows 執行階段類別會公開其功能，透過介面。 每個 COM 介面最終都會衍生自[ **IUnknown 介面**](https://msdn.microsoft.com/library/windows/desktop/ms680509)介面。 Windows 執行階段以 COM 為基礎&mdash;正在 Windows 執行階段介面最終的一個差異是衍生自[ **IInspectable 介面**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (和**IInspectable**衍生自**IUnknown**)。

在上述程式碼中 coclass，我們會實作**INotificationActivationCallback::Activate**方法，這是當使用者按一下快顯通知的回撥 按鈕時所呼叫的函式。 但是可以呼叫該函式之前，coclass 的執行個體必須先建立，而且正在的**iclassfactory:: Createinstance**函式。

我們實作的 coclass 就所謂*COM activator*通知，以及它會有類別識別碼 (CLSID) 的形式`callback_guid`識別項 (型別的**GUID**)，您在上面看見。 我們將使用該識別項之後，在 [開始] 功能表捷徑和 Windows 登錄項目表單中。 COM activator CLSID，以及其相關聯的 COM 伺服器 （也就是我們要建置的可執行檔的路徑） 的路徑是用快顯通知會知道哪些類別來建立執行個體時其回呼按一下按鈕的機制 (是否按一下通知中 行動作業中心與否）。

## <a name="best-practices-for-implementing-com-methods"></a>實作的 COM 方法的最佳作法

技術來處理錯誤及資源管理即可手動在庫存。 它會更方便且實際使用的錯誤碼的例外狀況。 如果您採用資源-擷取-即-初始化 (RAII) 慣用語，然後您可以避免明確檢查錯誤代碼，以及明確釋放資源。 這類明確檢查進行更錯綜複雜過長的情況下，您的程式碼並提供 bug 許多地方可以隱藏。 使用 RAII，而擲回/catch 例外狀況。 如此一來，您的資源配置是例外狀況安全，而且您的程式碼很簡單。

不過，您不得允許例外狀況逸出您的 COM 方法實作。 您可以藉由確定`noexcept`COM 方法上的規範。 只要您處理這類方法結束之前沒有關係中擲回任何位置呼叫歷程圖，在方法中，例外狀況。 如果您使用`noexcept`，但您再允許例外狀況逸出您的方法，則會終止您的應用程式。

## <a name="add-helper-types-and-functions"></a>新增協助程式類型和函式

在此步驟中，我們將新增一些協助程式類型和函式，其餘的程式碼可使用。 是之前, `main`，新增下列。

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

## <a name="implement-the-remaining-functions-and-the-wmain-entry-point-function"></a>實作其餘的函式和 wmain 進入點函式

專案範本會產生`main`為您的函式。 刪除這個`main`函式，並在其位置中貼上此程式碼清單，其中包含要註冊您的 coclass 的程式碼，然後再傳遞回呼叫您的應用程式的支援快顯通知。

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

## <a name="how-to-test-the-example-application"></a>如何測試範例應用程式

建置應用程式，並再導致註冊和其他設定，若要執行的程式碼的系統管理員身分重新執行至少一次。 您是否系統管理員，以執行它，然後按下 'T' 會造成顯示快顯通知。 您也可以按一下**回撥 ToastAndCallback**直接從快顯通知快顯，或行動作業中心和您的應用程式將會啟動，具現化，coclass 的按鈕和**INotificationActivationCallback::Activate**執行方法。

## <a name="in-process-com-server"></a>同處理序 COM 伺服器

*ToastAndCallback*上述的範例應用程式函式做為本機 （或跨處理序） 的 COM 伺服器。 這會由[LocalServer32](/windows/desktop/com/localserver32) Windows 登錄機碼，您用來註冊其 coclass 的 CLSID。 本機 COM 伺服器裝載可執行檔的二進位檔及其 coclass(es) ( `.exe`)。

或者 （並說是更有可能），您可以選擇裝載的動態連結程式庫內您 coclass(es) ( `.dll`)。 COM 伺服器 DLL 的形式就所謂的同處理序 COM 伺服器，以及其中會顯示正在註冊使用 Clsid [InprocServer32](/windows/desktop/com/inprocserver32) Windows 登錄機碼。

### <a name="create-a-dynamic-link-library-dll-project"></a>建立動態連結程式庫 (DLL) 專案

您可以開始在 Microsoft Visual Studio 中建立新的專案建立同處理序 COM 伺服器的工作。 建立**Visual c + +** > **Windows Desktop** > **動態連結程式庫 (DLL)** 專案。

若要加入 C + + /cli WinRT 支援加入至新的專案，請依照下列所述的步驟[修改 Windows 桌面應用程式專案，以加入 C + + /cli WinRT 支援](/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

### <a name="implement-the-coclass-class-factory-and-in-proc-server-exports"></a>實作 coclass、 class factory，與跨處理序伺服器匯出

開啟`dllmain.cpp`，並為其新增如下所示的程式碼清單。

如果您已實作 C + + /cli WinRT Windows 執行階段類別，則您已經擁有**DllCanUnloadNow**函式，如下所示。 如果您想要將 coclass 加入至該 DLL 中，則您可以加入**DllGetClassObject**函式。

如果沒有現有[Windows 執行階段 c + + 範本庫 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)從顯示的程式碼的程式碼，您想要保持相容，則您可以移除 WRL 組件。

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

另請參閱[弱式參考在 C + + /cli WinRT](weak-references.md#weak-references-in-cwinrt)。

C + + /cli WinRT (具體而言， [ **winrt::implements** ](/uwp/cpp-ref-for-winrt/implements)基底結構範本) 實作[ **IWeakReferenceSource** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreferencesource) ，如果您型別會實作[ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (或衍生自任何介面**IInspectable**)。

這是因為**IWeakReferenceSource**並[ **IWeakReference** ](/windows/desktop/api/weakreference/nn-weakreference-iweakreference)專為 Windows 執行階段類型。 因此，您可以開啟弱式參考支援您 coclass 只要加**winrt::Windows::Foundation::IInspectable** (或介面衍生自**IInspectable**) 到您的實作。

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
* [撰寫 Api 使用 C + + /cli WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [使用 COM 元件使用 C + + /cli WinRT](consume-com.md)
* [傳送本機的快顯通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
