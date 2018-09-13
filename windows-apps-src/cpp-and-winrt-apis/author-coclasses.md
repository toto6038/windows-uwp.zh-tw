---
author: stevewhims
description: C + + WinRT 可協助您撰寫傳統 COM 元件，就如同它可協助您撰寫 Windows 執行階段類別。
title: 撰寫 COM 元件使用 C + + /winrt
ms.author: stwhi
ms.date: 09/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 標準、 c + +、 cpp、 winrt、 投影、 作者，COM、 元件
ms.localizationpriority: medium
ms.openlocfilehash: 729cfae39f302ae6b5bae275d9e28a39f3d9503b
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2018
ms.locfileid: "3961610"
---
# <a name="author-com-components-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>撰寫使用 COM 元件[C + + /winrt](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

C + + /winrt 可協助您撰寫的傳統型元件物件模型 (COM) 元件 （或 coclasses），就如同它可協助您撰寫 Windows 執行階段類別。 以下是非常簡單的圖例，您可以測試一點，如果您將它貼到`main.cpp`的新**Windows 主控台應用程式 (C + + /winrt)** 專案。

```cppwinrt
// main.cpp : Defines the entry point for the console application.
#include "pch.h"

using namespace winrt;

int main()
{
    init_apartment();

    struct MyCoclass : winrt::implements<MyCoclass, IPersist>
    {
        HRESULT STDMETHODCALLTYPE GetClassID(CLSID* id) noexcept override
        {
            *id = IID_IPersist; // Doesn't matter what we return, for this example.
            return S_OK;
        }
    };

    auto mycoclass_instance{ winrt::make<MyCoclass>() };
    CLSID id{};
    winrt::check_hresult(mycoclass_instance->GetClassID(&id));
}
```

另請參閱[取用 COM 元件，使用 C + + /winrt](consume-com.md)。

## <a name="a-more-realistic-and-interesting-example"></a>更逼真且有趣的範例

本主題的其餘部分逐步解說如何建立最少的主控台應用程式專案使用 C + + /winrt 實作基本的 coclass 和類別原廠。 範例應用程式示範如何提供回呼按鈕的快顯通知，並 coclass （這會實作**INotificationActivationCallback** COM 介面） 可讓應用程式啟動並呼叫返回時使用者按一下該按鈕的快顯通知。

[傳送本機快顯通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)位於快顯通知功能區域相關的詳細背景。 無文件的在該節中的程式碼範例使用 C + + /winrt，不過，因此我們建議您想要顯示在本主題中的程式碼。

## <a name="create-a-windows-console-application-project-toastandcallback"></a>建立 Windows 主控台應用程式專案 (ToastAndCallback)

在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立**Visual c + +** > **的 Windows 桌面** > **Windows 主控台應用程式 (C + + /winrt)** 專案，並將它命名為*ToastAndCallback*。

開啟`main.cpp`，並移除 using 指示詞專案範本所產生。 在其位置，貼上下列程式碼 （這可讓我們程式庫、 標頭，以及我們需要的類型名稱）。

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

在 C + + /winrt，您實作 coclasses 和類別 factory 衍生自[**winrt:: implements**](/uwp/cpp-ref-for-winrt/implements)基礎結構。 在三個 using 指示詞上述後立即 (和之前`main`)，貼上下列程式碼來實作您的快顯通知 COM 啟動者元件。

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

上述 coclass 實作如下所示的相同模式[撰寫 Api 使用 C + + /winrt](/windows/uwp/cpp-and-winrt-apis/author-apis#if-youre-not-authoring-a-runtime-class)。 請注意，您可以使用這項技術，不只適用於 Windows 執行階段介面 （任何介面最終衍生自[**IInspectable**](https://msdn.microsoft.com/library/br205821)），但也實作 COM 介面 （最終衍生自[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)任何介面） 是。

在上述程式碼中 coclass，我們會實作**INotificationActivationCallback::Activate**方法，也就是當使用者按一下快顯通知上的 [回呼] 按鈕時所呼叫的函式。 但是，該函式可呼叫之前，您必須建立，coclass 的執行個體，而這就是**IClassFactory::CreateInstance**函式的工作。

我們只是實作 coclass 稱為通知， *COM 啟動者*，它有其類別識別碼 (CLSID) 的形式`callback_guid`識別項 (of 類型**的 GUID**），您看到以上。 我們會使用該識別碼更新版本，形式的 [開始] 功能表捷徑，以及 Windows 登錄項目。 COM 啟動者 CLSID，以及其相關聯的 COM 伺服器 （也就是我們在這裡建置的可執行檔的路徑） 的路徑是量的快顯通知會知道什麼類別來建立執行個體的當其回呼按一下按鈕的機制 (是否通知按一下控制中心中或不）。

## <a name="best-practices-for-implementing-com-methods"></a>實作 COM 方法的最佳做法

錯誤處理和資源管理技術可以瀏覽手中手。 它是更方便且不方便使用比錯誤碼的例外狀況。 此外，如果您使用資源的下載數-是-初始化 (RAII) 慣用語，則您可以避免明確檢查錯誤碼，並明確釋放資源。 這類明確檢查進行更錯綜複雜多餘的情況下，您的程式碼，它提供錯誤充分的隱藏的地方。 反之，請使用 RAII，並擲回/catch 例外狀況。 如此一來，您的資源配置是異常安全和您的程式碼很簡單。

不過，您我允許逸出您的 COM 方法實作的例外狀況。 您可以確保使用`noexcept`在您的 COM 方法的規範。 只要您處理這類您方法結束之前是方法的確定在您的呼叫圖形中任何一處回的例外狀況。 如果您使用`noexcept`，但您然後允許來逸出您方法中，例外狀況，則您的應用程式會終止。

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

專案範本會產生`main`為您的函式。 刪除這個`main`函式，並在它的位置中貼上此程式碼清單，其中包括程式碼來登錄您 coclass，然後傳送快顯通知能夠呼叫回您的應用程式。

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
    init_apartment();

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

建置應用程式，並再以系統管理員身分註冊，以及其他安裝程式中，若要執行的程式碼會造成中至少一次執行。 您是否正在執行以系統管理員身分，然後按下 T ' 會造成顯示快顯通知。 您可以按一下直接從突然向上，或從在控制中心和您的應用程式將會啟動快顯通知、 具現化，coclass 和 INotificationActivationCallback****回呼 ToastAndCallback**按鈕:: As 啟用**執行的方法。

## <a name="important-apis"></a>重要 API
* [IInspectable 介面](https://msdn.microsoft.com/library/br205821)
* [IUnknown 介面](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [winrt::implements 結構範本](/uwp/cpp-ref-for-winrt/implements)

## <a name="related-topics"></a>相關主題
* [使用 C++/WinRT 撰寫 API ](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [使用 COM 元件使用 C + + /winrt](consume-com.md)
* [傳送本機快顯通知](/windows/uwp/design/shell/tiles-and-notifications/send-local-toast)
