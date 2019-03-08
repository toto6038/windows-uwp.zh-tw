---
Description: 了解如何將本機的快顯通知的傳送和處理使用者按一下快顯通知 Win32 c + + WRL 應用程式。
title: 從傳統型 C++ WRL 應用程式傳送本機快顯通知
label: Send a local toast notification from desktop C++ WRL apps
template: detail.hbs
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp, win32, 傳統型, 快顯通知, 傳送快顯通知, 傳送本機快顯通知, 傳統型橋接器, C++, cpp, cplusplus, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 82de349009350c970fce923a2aa503df0801c3b7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609843"
---
# <a name="send-a-local-toast-notification-from-desktop-c-wrl-apps"></a>從傳統型 C++ WRL 應用程式傳送本機快顯通知

傳統型應用程式 (傳統型橋接器和傳統的 Win32) 可以像通用 Windows 平台 (UWP) App 一樣傳送互動式快顯通知。 但是，如果您不使用傳統型橋接器，則由於不同的啟用配置和可能缺乏套件識別資料，傳統型應用程式有幾個特殊步驟。

> [!IMPORTANT]
> 如果您在撰寫 UWP app，請參閱 [UWP 文件](send-local-toast.md)。 對於其他傳統型語言，請參閱[傳統型 C#](send-local-toast-desktop.md)。


## <a name="step-1-enable-the-windows-10-sdk"></a>步驟 1：啟用 Windows 10 SDK

如果您尚未為 Win32 應用程式啟用 Windows 10 SDK，您必須先執行此步驟。 有幾個關鍵步驟...

1. 新增 `runtimeobject.lib` 到 **\[其他相依性\]**
2. 以 Windows 10 SDK 為目標

以滑鼠右鍵按一下您的專案，選取 **\[內容\]**。

在上方 **\[設定\]** 功能表中，選取 **\[所有設定\]**，將下列變更套用至偵錯與發行版本。

在 **\[連結器 -> 輸入\]** 下方，新增 `runtimeobject.lib` 至 **\[其他相依性\]**。

然後在 **\[一般\]** 下方，確定 **\[Windows SDK 版本\]** 設為 10.0 或更高版本 (而非 Windows 8.1)。


## <a name="step-2-copy-compat-library-code"></a>步驟 2：複製 相容性程式庫程式碼

從 GitHub 複製 [DesktopNotificationManagerCompat.h](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.h) 和 [DesktopNotificationManagerCompat.cpp](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CPP-WRL/DesktopToastsCppWrlApp/DesktopNotificationManagerCompat.cpp) 檔案到您的專案。 Compat 程式庫消除了桌面通知的許多複雜性。 下列指令需要 Compat 程式庫。

如果您正在使用預先編譯標頭，請務必將 `#include "stdafx.h"` 設為 DesktopNotificationManagerCompat.cpp 檔案的第一行。


## <a name="step-3-include-the-header-files-and-namespaces"></a>步驟 3：包含標頭檔和命名空間

加入 Compat 程式庫標頭檔案，以及與使用 UWP 快顯通知 API 相關的標頭檔案和命名空間。

```cpp
#include "DesktopNotificationManagerCompat.h"
#include <NotificationActivationCallback.h>
#include <windows.ui.notifications.h>

using namespace ABI::Windows::Data::Xml::Dom;
using namespace ABI::Windows::UI::Notifications;
using namespace Microsoft::WRL;
```


## <a name="step-4-implement-the-activator"></a>步驟 4：實作啟動程式

您必須為快顯通知啟用實作處理常式，以便使用者按下您的快顯通知時，您的應用程式可執行某個動作。 您的快顯通知若要保留在控制中心 (因為可能在您的應用程式關閉幾天時，使用者才按下快顯通知)，此為必要步驟。 這個類別可以放在專案的任何位置。

如下所示實作 **INotificationActivationCallback** 介面，包括 UUID，同時呼叫 **CoCreatableClass** 以將您的類別標幟為可由 COM 建立。 對於 UUID，使用眾多線上 GUID 產生器的其中一個，建立唯一 GUID。 此 GUID CLSID (類別識別碼) 是控制中心得知 COM 啟用哪個類別的方式。

```cpp
// The UUID CLSID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public:
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        // TODO: Handle activation
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```


## <a name="step-5-register-with-notification-platform"></a>步驟 5：使用通知平台註冊

接著，您必須向通知平台註冊。 根據您使用的是傳統型橋接器或傳統型 Win32，步驟會有所不同。 如果兩者都支援，您必須完成兩個步驟 (但不需要分支程式碼，我們的程式庫會為您處理！)。


### <a name="desktop-bridge"></a>傳統型橋接器

如果您使用傳統型橋接器 (或者如果兩者都支援)，在 **Package.appxmanifest** 中，新增：

1. **xmlns:com** 的宣告
2. **xmlns:desktop** 的宣告
3. 在 **IgnorableNamespaces** 屬性中，**com** 和 **desktop**
4. 使用步驟 #4 的 GUID，新增 COM 啟動者的 **com:Extension**。 請務必包含 `Arguments="-ToastActivated"`，讓您知道您的啟動是來自快顯通知
5. **windows.toastNotificationActivation** 的 **desktop:Extension**，宣告您的快顯通知啟動者 CLSID (步驟 #4 的 GUID)。

**Package.appxmanifest**

```xml
<Package
  ...
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="... com desktop">
  ...
  <Applications>
    <Application>
      ...
      <Extensions>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```


### <a name="classic-win32"></a>傳統型 Win32

如果您使用傳統型 Win32 (或者如果兩者都支援)，您必須在 [開始] 畫面的應用程式捷徑上宣告應用程式使用者模型識別碼 (AUMID) 和快顯通知啟動者 CLSID (步驟 #4 的 GUID)。

選擇可識別您的 Win32 應用程式的唯一 AUMID。 這通常是 [CompanyName].[AppName] 的形式，但您應確保這在所有應用程式中都是唯一的 (可以在結尾處加上幾個數字)。

#### <a name="step-51-wix-installer"></a>步驟 5.1:WiX 安裝程式

如果您為安裝程式使用 WiX，請編輯 **Product.wxs** 檔案以新增兩個捷徑內容到您的 [開始] 功能表捷徑，如下所示。 請務必將步驟 #4 的 GUID 以`{}` 括住，如下所示。

**Product.wxs**

```xml
<Shortcut Id="ApplicationStartMenuShortcut" Name="Wix Sample" Description="Wix Sample" Target="[INSTALLFOLDER]WixSample.exe" WorkingDirectory="INSTALLFOLDER">
                    
    <!--AUMID-->
    <ShortcutProperty Key="System.AppUserModel.ID" Value="YourCompany.YourApp"/>
    
    <!--COM CLSID-->
    <ShortcutProperty Key="System.AppUserModel.ToastActivatorCLSID" Value="{replaced-with-your-guid-C173E6ADF0C3}"/>
    
</Shortcut>
```

> [!IMPORTANT]
> 為了確實使用通知，在正常偵錯前，您必須透過安裝程式安裝一次您的應用程式，以便顯示包含您的 AUMID 與 CLSID 的 [開始] 畫面捷徑。 [開始] 畫面捷徑出現後，您可以使用 Visual Studio 的 F5 來偵錯。


#### <a name="step-52-register-aumid-and-com-server"></a>步驟 5.2:註冊 AUMID 和 COM 伺服器

接著，無論安裝程式為何，在您應用程式的啟動程式碼中 (在呼叫任何通知 API 之前)，呼叫 **RegisterAumidAndComServer** 方法，指定步驟 #4 中的通知啟動者類別和上面使用的 AUMID。

```cpp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"YourCompany.YourApp", __uuidof(NotificationActivator));
```

如果您同時支援傳統型橋接器和傳統型 Win32，可以隨意呼叫這個方法。 如果您在傳統型橋接器之下執行，此方法會立即傳回。 不需要分支程式碼。

這個方法可讓您呼叫 Compat API 來傳送和管理通知，而不需要持續提供 AUMID。 它會插入 COM 伺服器的 LocalServer32 登錄機碼。


## <a name="step-6-register-com-activator"></a>步驟 6：註冊 COM 啟動程式

對於傳統型橋接器和傳統型 Win32 應用程式，都必須註冊通知啟動者類型，以便處理快顯通知啟用。

在應用程式的啟動程式碼中，呼叫下列 **RegisterActivator** 方法。 必須呼叫此方法，您才能接收快顯通知啟用。

```cpp
// Register activator type
hr = DesktopNotificationManagerCompat::RegisterActivator();
```


## <a name="step-7-send-a-notification"></a>步驟 7：傳送通知

傳送通知與 UWP app 相同，除了您將使用 **DesktopNotificationManagerCompat** 來建立 **ToastNotifier**。 Compat 程式庫會自動處理傳統型橋接器和傳統型 Win32 之間的差異，所以您不需要分支您的程式碼。 對於傳統型 Win32，Compat 程式庫會快取您在呼叫 **RegisterAumidAndComServer** 時提供的 AUMID，因此您不必擔心何時要提供或不提供 AUMID。

請務必使用如下所示的 **ToastGeneric** 繫結，因為舊版 Windows 8.1 快顯通知範本不會啟用您在步驟 #4 所建立的 COM 通知啟動者。

> [!IMPORTANT]
> 只有資訊清單中具備網際網路功能的傳統型橋接器應用程式才支援 http 影像。 傳統型 Win32 應用程式不支援 http 影像；您必須將影像下載至本機應用程式資料，並在本機參考它。

```cpp
// Construct XML
ComPtr<IXmlDocument> doc;
hr = DesktopNotificationManagerCompat::CreateXmlDocumentFromString(
    L"<toast><visual><binding template='ToastGeneric'><text>Hello world</text></binding></visual></toast>",
    &doc);
if (SUCCEEDED(hr))
{
    // See full code sample to learn how to inject dynamic text, buttons, and more

    // Create the notifier
    // Classic Win32 apps MUST use the compat method to create the notifier
    ComPtr<IToastNotifier> notifier;
    hr = DesktopNotificationManagerCompat::CreateToastNotifier(&notifier);
    if (SUCCEEDED(hr))
    {
        // Create the notification itself (using helper method from compat library)
        ComPtr<IToastNotification> toast;
        hr = DesktopNotificationManagerCompat::CreateToastNotification(doc.Get(), &toast);
        if (SUCCEEDED(hr))
        {
            // And show it!
            hr = notifier->Show(toast.Get());
        }
    }
}
```

> [!IMPORTANT]
> 傳統 Win32 應用程式無法使用舊版的快顯通知 (例如 ToastText02) 的範本。 指定 COM CLSID 時，啟用舊版範本將會失敗。 您必須使用的 Windows 10 ToastGeneric 範本，如上方所示。


## <a name="step-8-handling-activation"></a>步驟 8：處理啟用

當使用者按下您的快顯通知，或快顯通知中的按鈕，就會叫用您 **NotificationActivator** 類別的 **Activate** 方法。

在 Activate 方法中，您可以剖析快顯通知中所指定的引數並取得使用者鍵入或選取的使用者輸入，再據以啟用您的應用程式。

> [!NOTE]
> **Activate** 方法是從不同於主要執行緒的個別執行緒進行呼叫。

```cpp
// The GUID must be unique to your app. Create a new GUID if copying this code.
class DECLSPEC_UUID("replaced-with-your-guid-C173E6ADF0C3") NotificationActivator WrlSealed WrlFinal
    : public RuntimeClass<RuntimeClassFlags<ClassicCom>, INotificationActivationCallback>
{
public: 
    virtual HRESULT STDMETHODCALLTYPE Activate(
        _In_ LPCWSTR appUserModelId,
        _In_ LPCWSTR invokedArgs,
        _In_reads_(dataCount) const NOTIFICATION_USER_INPUT_DATA* data,
        ULONG dataCount) override
    {
        std::wstring arguments(invokedArgs);
        HRESULT hr = S_OK;

        // Background: Quick reply to the conversation
        if (arguments.find(L"action=reply") == 0)
        {
            // Get the response user typed.
            // We know this is first and only user input since our toasts only have one input
            LPCWSTR response = data[0].Value;

            hr = DesktopToastsApp::SendResponse(response);
        }

        else
        {
            // The remaining scenarios are foreground activations,
            // so we first make sure we have a window open and in foreground
            hr = DesktopToastsApp::GetInstance()->OpenWindowIfNeeded();
            if (SUCCEEDED(hr))
            {
                // Open the image
                if (arguments.find(L"action=viewImage") == 0)
                {
                    hr = DesktopToastsApp::GetInstance()->OpenImage();
                }

                // Open the app itself
                // User might have clicked on app title in Action Center which launches with empty args
                else
                {
                    // Nothing to do, already launched
                }
            }
        }

        if (FAILED(hr))
        {
            // Log failed HRESULT
        }

        return S_OK;
    }

    ~NotificationActivator()
    {
        // If we don't have window open
        if (!DesktopToastsApp::GetInstance()->HasWindow())
        {
            // Exit (this is for background activation scenarios)
            exit(0);
        }
    }
};

// Flag class as COM creatable
CoCreatableClass(NotificationActivator);
```

為了在您的應用程式關閉時正確支援啟動，請在 WinMain 函數中判斷是否由快顯通知啟動。 如果啟動是來自快顯通知，則會有「-ToastActivated」的啟動引數。 當您看到此項目時，應停止執行任何正常啟用程式碼，並允許 **NotificationActivator** 視需要處理啟動視窗。

```cpp
// Main function
int WINAPI wWinMain(_In_ HINSTANCE hInstance, _In_opt_ HINSTANCE, _In_ LPWSTR cmdLineArgs, _In_ int)
{
    RoInitializeWrapper winRtInitializer(RO_INIT_MULTITHREADED);

    HRESULT hr = winRtInitializer;
    if (SUCCEEDED(hr))
    {
        // Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
        hr = DesktopNotificationManagerCompat::RegisterAumidAndComServer(L"WindowsNotifications.DesktopToastsCpp", __uuidof(NotificationActivator));
        if (SUCCEEDED(hr))
        {
            // Register activator type
            hr = DesktopNotificationManagerCompat::RegisterActivator();
            if (SUCCEEDED(hr))
            {
                DesktopToastsApp app;
                app.SetHInstance(hInstance);

                std::wstring cmdLineArgsStr(cmdLineArgs);

                // If launched from toast
                if (cmdLineArgsStr.find(TOAST_ACTIVATED_LAUNCH_ARG) != std::string::npos)
                {
                    // Let our NotificationActivator handle activation
                }

                else
                {
                    // Otherwise launch like normal
                    app.Initialize(hInstance);
                }

                app.RunMessageLoop();
            }
        }
    }

    return SUCCEEDED(hr);
}
```


### <a name="activation-sequence-of-events"></a>事件的啟用順序

啟用順序如下...

如果您的應用程式已在執行中：

1. 會呼叫 **NotificationActivator** 中的 **Activate**

如果您的應用程式未執行：

1. 您的應用程式是由 EXE 啟動，您會取得「-ToastActivated」的命令列引數
2. 會呼叫 **NotificationActivator** 中的 **Activate**


### <a name="foreground-vs-background-activation"></a>前景和背景啟用
對於傳統型應用程式，前景與背景啟用的處理方式相同 - 都會呼叫 COM 啟動者。 接著將由您應用程式的程式碼決定是否顯示視窗，或僅執行一些工作然後就結束。 因此，在快顯通知內容中指定 **background** 的 **activationType** 並不會變更行為。


## <a name="step-9-remove-and-manage-notifications"></a>步驟 9：移除及管理通知

移除和管理通知與 UWP app 相同。 不過，我們建議您使用我們的 Compat 程式庫來取得 **DesktopNotificationHistoryCompat**，使用傳統型 Win32 時就無需擔心提供 AUMID。

```cpp
std::unique_ptr<DesktopNotificationHistoryCompat> history;
auto hr = DesktopNotificationManagerCompat::get_History(&history);
if (SUCCEEDED(hr))
{
    // Remove a specific toast
    hr = history->Remove(L"Message2");

    // Clear all toasts
    hr = history->Clear();
}
```


## <a name="step-10-deploying-and-debugging"></a>步驟 10：部署和偵錯

若要部署和偵錯您的傳統型橋接器應用程式，請參閱[執行、偵錯以及測試封裝的傳統型應用程式](/windows/uwp/porting/desktop-to-uwp-debug)。

若要部署和偵錯您的傳統型 Win32 應用程式，在正常偵錯前，您必須透過安裝程式安裝一次您的應用程式，以便顯示包含您的 AUMID 與 CLSID 的 [開始] 畫面捷徑。 [開始] 畫面捷徑出現後，您可以使用 Visual Studio 的 F5 來偵錯。

如果通知無法顯示在傳統型 Win32 應用程式中 (也未傳回例外)，這可能表示 [開始] 畫面捷徑不存在 (透過安裝程式安裝您的應用程式)，或您用於程式碼的 AUMID 不符合 [開始] 畫面捷徑中的 AUMID。

如果您的通知會顯示，但不會保留在控制中心 (快顯視窗關閉後就消失)，這表示您未正確實作 COM 啟動者。

如果您已同時安裝傳統型橋接器和傳統型 Win32 應用程式，請注意在處理快顯通知啟用時，傳統型橋接器應用程式將會取代傳統型 Win32 應用程式。 這表示在點按時，傳統型 Win32 應用程式的快顯通知仍會啟動傳統型橋接器應用程式。 解除安裝傳統型橋接器應用程式可將啟用還原回傳統型 Win32 應用程式。

如果您收到 `HRESULT 0x800401f0 CoInitialize has not been called.`，在呼叫 API 前，請務必先在您的應用程式中呼叫 `CoInitialize(nullptr)`。

如果您在呼叫 Compat API 時收到 `HRESULT 0x8000000e A method was called at an unexpected time.`，這可能表示您無法呼叫必要的 Register 方法 (若是傳統型橋接器應用程式，則表示您目前未在傳統型橋接器內容中執行您的應用程式)。

如果您收到數個 `unresolved external symbol` 編譯錯誤，您可能在步驟 #1 忘記新增 `runtimeobject.lib` 至 **\[其他相依性\]** (或是只將它新增至偵錯設定而未新增至發行設定)。


## <a name="handling-older-versions-of-windows"></a>處理舊版 Windows

如果您支援 Windows 8.1 或更低版本，建議您在呼叫任何 **DesktopNotificationManagerCompat** API 或傳送任何 ToastGeneric 快顯通知之前，先在執行階段查看是否執行 Windows 10。

Windows 8 已引進快顯通知，但使用[舊版快顯通知範本](https://docs.microsoft.com/en-us/previous-versions/windows/apps/hh761494(v=win.10))，例如 ToastText01。 由於快顯通知只是短暫快顯而不會保留，因此啟用是由記憶體中 **ToastNotification** 類別上的 **Activated** 事件處理。 Windows 10 引進[互動式 ToastGeneric 快顯通知](adaptive-interactive-toasts.md)，同時引進了控制中心，通知會在此處保留多天。 引進控制中心需要同時引進 COM 啟動者，以讓您的快顯通知可在建立數天之後啟用。

| 作業系統 | ToastGeneric | COM 啟動器 | 舊版快顯通知範本 |
| -- | ------------ | ------------- | ---------------------- |
| Windows 10 | 支援 | 支援 | 支援 (但不會啟用 COM 伺服器) |
| Windows 8.1 / 8 | 無 | 無 | 支援 |
| Windows 7 和更舊版本 | 無 | 無 | 無 |

若要查看您是否正在執行 Windows 10，請加入 `<VersionHelpers.h>` 標頭並檢查 **IsWindows10OrGreater** 方法。 如果這傳回 true，請繼續呼叫本文中提及的所有方法！ 

```cpp
#include <VersionHelpers.h>

if (IsWindows10OrGreater())
{
    // Running Windows 10, continue with sending Windows 10 toasts!
}
```


## <a name="known-issues"></a>已知問題

**已修正：按一下快顯通知後，應用程式不成為焦點**:在組建 15063 及更早版本中，前景權限未傳輸您的應用程式時，我們已啟用的 COM 伺服器。 因此，當您嘗試將它移動到前景時，您的應用程式只會閃爍。 此問題沒有解決方法。 我們在組建 16299 與更高版本中已修正這個問題。


## <a name="resources"></a>資源

* [在 GitHub 上的完整程式碼範例](https://github.com/WindowsNotifications/desktop-toasts)
* [從桌面應用程式的快顯通知](toast-desktop-apps.md)
* [快顯通知內容的文件](adaptive-interactive-toasts.md)
