---
description: '瞭解桌面 c # 應用程式如何傳送本機快顯通知，以及如何處理使用者按一下快顯通知。'
title: 從傳統型 C# 應用程式傳送本機快顯通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: 'windows 10、win32、desktop、快顯通知、傳送快顯通知、傳送本機快顯通知、桌面橋接器、msix、稀疏套件、c #、c 清晰、快顯通知、wpf、傳送快顯通知 wpf、傳送快顯通知（c #）、傳送通知 wpf、傳送通知 c #、快顯通知 wpf、快顯通知#'
ms.localizationpriority: medium
ms.openlocfilehash: cb91a76db38623b533a925ea1df4728bc0fead78
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034471"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>從傳統型 C# 應用程式傳送本機快顯通知

傳統型應用程式 (包括封裝 [MSIX](/windows/msix/desktop/source-code-overview) 應用程式、使用 [稀疏套件](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 來取得套件身分識別的應用程式，以及傳統的非封裝桌面應用程式) 可以傳送互動式快顯通知，就像 Windows 應用程式一樣。 不過，由於不同的啟用配置以及如果您不是使用 MSIX 或稀疏套件，桌面應用程式可能會有一些特殊步驟。

> [!IMPORTANT]
> 如果您在撰寫 UWP app，請參閱 [UWP 文件](send-local-toast.md)。 如需其他桌面語言，請參閱 [Win32 c + + WRL](send-local-toast-desktop-cpp-wrl.md)。


## <a name="step-1-install-the-notifications-library"></a>步驟1：安裝通知程式庫

`Microsoft.Toolkit.Uwp.Notifications`在您的專案中安裝[NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。

此 [通知程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 會新增相容性程式庫程式碼，以使用來自桌面應用程式的快顯通知。 它也會參考 UWP Sdk，並可讓您使用 c # （而不是原始 XML）來建立通知。 本快速入門的其餘部分取決於通知程式庫。


## <a name="step-2-implement-the-activator"></a>步驟2：執行啟動項

您必須執行快顯通知的處理常式，以便在使用者按一下您的快顯通知時，您的應用程式可以執行某些動作。 您的快顯通知若要保留在控制中心 (因為可能在您的應用程式關閉幾天時，使用者才按下快顯通知)，此為必要步驟。 這個類別可以放在專案的任何位置。

建立新的 **MyNotificationActivator** 類別，並擴充 **NotificationActivator** 類別。 新增下列三個屬性，並使用許多線上 GUID 產生器中的其中一個來為您的應用程式建立唯一的 GUID CLSID。 此 CLSID (類別識別碼) 是控制中心得知 COM 啟用哪個類別的方式。

**MyNotificationActivator.cs** (建立此檔案) 

```csharp
// The GUID CLSID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        // TODO: Handle activation
    }
}
```


## <a name="step-3-register-with-notification-platform"></a>步驟3：向通知平臺註冊

接著，您必須向通知平台註冊。 有不同的步驟取決於您使用的是 MSIX/稀疏套件或傳統桌面。 如果兩者都支援，您必須完成兩個步驟 (但不需要分支程式碼，我們的程式庫會為您處理！)。


#### <a name="msixsparse-packages"></a>[MSIX/sparse 封裝](#tab/msix-sparse)

如果您使用的是 [MSIX](/windows/msix/desktop/source-code-overview) 或 [sparse 封裝](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) (或者，如果您同時支援這兩個) ，請在 **package.appxmanifest** 中新增：

1. **xmlns:com** 的宣告
2. **xmlns:desktop** 的宣告
3. 在 **IgnorableNamespaces** 屬性中， **com** 和 **desktop**
4. com：使用步驟 #2 中的 GUID 之 COM 啟動項的 **擴充** 功能。 請務必包含 `Arguments="-ToastActivated"`，讓您知道您的啟動是來自快顯通知
5. **desktop：** **toastNotificationActivation** 的擴充功能，可將您的快顯啟動程式 CLSID (步驟 #2) 的 GUID。

「Package.appxmanifest」

```xml
<!--Add these namespaces-->
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


#### <a name="unpackaged"></a>[且](#tab/classic)

如果您不是使用 MSIX/sparse (或同時支援這兩個) ，您必須在 [開始] 中，將應用程式使用者模型識別碼 (AUMID) 和快顯啟動程式 CLSID， (應用程式快捷方式上步驟 #2) 中的 GUID。

挑選會識別您桌面應用程式的唯一 AUMID。 這通常是 [CompanyName].[AppName] 的形式，但您應確保這在所有應用程式中都是唯一的 (可以在結尾處加上幾個數字)。

### <a name="step-31-wix-installer"></a>步驟3.1： WiX 安裝程式

如果您為安裝程式使用 WiX，請編輯 **Product.wxs** 檔案以新增兩個捷徑內容到您的 [開始] 功能表捷徑，如下所示。 請確定步驟 #2 中的 GUID 包含在中， `{}` 如下所示。

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


### <a name="step-32-register-aumid-and-com-server"></a>步驟3.2：註冊 AUMID 和 COM 伺服器

然後，不論您的安裝程式，在您的應用程式啟動程式碼中 (在) 呼叫任何通知 Api 之前，請呼叫 **RegisterAumidAndComServer** 方法，並從步驟 #2 指定您的通知啟動程式類別和上面使用的 AUMID。

```csharp
// Register AUMID and COM server (for MSIX/sparse package apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

如果您同時支援 MSIX/sparse 封裝和傳統桌面，則可以隨意呼叫這個方法，而不需要。 如果您是在 MSIX/sparse 套件中執行，這個方法只會立即傳回。 不需要分支程式碼。

這個方法可讓您呼叫 Compat API 來傳送和管理通知，而不需要持續提供 AUMID。 它會插入 COM 伺服器的 LocalServer32 登錄機碼。

---


## <a name="step-4-register-com-activator"></a>步驟4：註冊 COM 啟動項

針對 MSIX/sparse 封裝和傳統桌面應用程式，您必須註冊您的通知啟動程式類型，才能處理快顯通知。

在您應用程式的啟動程式碼中，呼叫下列 **RegisterActivator** 方法，並傳入您在步驟 #2 中建立之 **NotificationActivator** 類別的實作為。 必須呼叫此方法，您才能接收快顯通知啟用。

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-5-send-a-notification"></a>步驟5：傳送通知

傳送通知與 UWP app 相同，除了您將使用 **DesktopNotificationManagerCompat** 類別來建立 **ToastNotifier** 。 相容性程式庫會自動處理 MSIX/sparse 套件與傳統桌面之間的差異，因此您不需要將程式碼派生。 針對傳統桌面，相容性程式庫會快取您在呼叫 **RegisterAumidAndComServer** 時所提供的 AUMID，如此您就不需要擔心何時提供或不提供 AUMID。

> [!NOTE]
> 安裝 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，讓您可以使用如下所示的 C# 來建構通知，而不是使用原始 XML。

如果您要製作 XML) ，請務必使用下列 (或 ToastGeneric **範本，因為** 舊版 Windows 8.1 快顯通知範本將不會啟動您在步驟 #2 中建立的 COM 通知啟動程式。

> [!IMPORTANT]
> 只有在其資訊清單中具有網際網路功能的 MSIX/sparse 封裝應用程式才支援 Http 映射。 傳統桌面應用程式不支援 HTTP 映射;您必須將映射下載至本機應用程式資料，並在本機進行參考。

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContentBuilder()
    .AddToastActivationInfo("action=viewConversation&conversationId=5", ToastActivationType.Foreground)
    .AddText("Hello world!")
    .GetToastContent();

// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 傳統桌面應用程式無法使用舊版的快顯通知範本 (例如 ToastText02) 。 指定 COM CLSID 時，啟用舊版範本將會失敗。 您必須使用的 Windows 10 ToastGeneric 範本，如上方所示。


## <a name="step-6-handling-activation"></a>步驟6：處理啟用

當使用者按下您的快顯通知，就會叫用您 **NotificationActivator** 類別的 **OnActivated** 方法。

在 OnActivated 方法中，您可以剖析快顯通知中所指定的引數並取得使用者鍵入或選取的使用者輸入，再據以啟用您的應用程式。

> [!NOTE]
> UI 執行緒上不會呼叫 **OnActivated** 方法。 如果您想要執行 UI 執行緒作業，您必須呼叫 `Application.Current.Dispatcher.Invoke(callback)`。

```csharp
// The GUID must be unique to your app. Create a new GUID if copying this code.
[ClassInterface(ClassInterfaceType.None)]
[ComSourceInterfaces(typeof(INotificationActivationCallback))]
[Guid("replaced-with-your-guid-C173E6ADF0C3"), ComVisible(true)]
public class MyNotificationActivator : NotificationActivator
{
    public override void OnActivated(string invokedArgs, NotificationUserInput userInput, string appUserModelId)
    {
        Application.Current.Dispatcher.Invoke(delegate
        {
            // Tapping on the top-level header launches with empty args
            if (arguments.Length == 0)
            {
                // Perform a normal launch
                OpenWindowIfNeeded();
                return;
            }

            // Parse the query string (using NuGet package QueryString.NET)
            QueryString args = QueryString.Parse(invokedArgs);

            // See what action is being requested 
            switch (args["action"])
            {
                // Open the image
                case "viewImage":

                    // The URL retrieved from the toast args
                    string imageUrl = args["imageUrl"];

                    // Make sure we have a window open and in foreground
                    OpenWindowIfNeeded();

                    // And then show the image
                    (App.Current.Windows[0] as MainWindow).ShowImage(imageUrl);

                    break;

                // Background: Quick reply to the conversation
                case "reply":

                    // Get the response the user typed
                    string msg = userInput["tbReply"];

                    // And send this message
                    SendMessage(msg);

                    // If there's no windows open, exit the app
                    if (App.Current.Windows.Count == 0)
                    {
                        Application.Current.Shutdown();
                    }

                    break;
            }
        });
    }

    private void OpenWindowIfNeeded()
    {
        // Make sure we have a window open (in case user clicked toast while app closed)
        if (App.Current.Windows.Count == 0)
        {
            new MainWindow().Show();
        }

        // Activate the window, bringing it to focus
        App.Current.Windows[0].Activate();

        // And make sure to maximize the window too, in case it was currently minimized
        App.Current.Windows[0].WindowState = WindowState.Normal;
    }
}
```

為了在您的應用程式關閉時正確支援啟動，請在 `App.xaml.cs` 檔案中覆寫 **OnStartup** 方法 (對於 WPF 應用程式) 以判斷是否由快顯通知啟動。 如果啟動是來自快顯通知，則會有「-ToastActivated」的啟動引數。 當您看到此項目時，應停止執行任何正常啟用程式碼，並允許 **OnActivated** 程式碼處理啟動。

```csharp
protected override async void OnStartup(StartupEventArgs e)
{
    // Register AUMID, COM server, and activator
    DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
    DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();

    // If launched from a toast
    if (e.Args.Contains("-ToastActivated"))
    {
        // Our NotificationActivator code will run after this completes,
        // and will show a window if necessary.
    }

    else
    {
        // Show the window
        // In App.xaml, be sure to remove the StartupUri so that a window doesn't
        // get created by default, since we're creating windows ourselves (and sometimes we
        // don't want to create a window if handling a background activation).
        new MainWindow().Show();
    }

    base.OnStartup(e);
}
```


### <a name="activation-sequence-of-events"></a>事件的啟用順序

對於 WPF，啟用順序如下...

如果您的應用程式已在執行中：

1. 會呼叫 **NotificationActivator** 中的 **OnActivated**

如果您的應用程式未執行：

1. 使用「-ToastActivated」的 **Args** 呼叫 `App.xaml.cs` 中的 **OnStartup**
2. 會呼叫 **NotificationActivator** 中的 **OnActivated**


### <a name="foreground-vs-background-activation"></a>前景和背景啟用
對於傳統型應用程式，前景與背景啟用的處理方式相同 - 都會呼叫 COM 啟動者。 接著將由您應用程式的程式碼決定是否顯示視窗，或僅執行一些工作然後就結束。 因此，在快顯通知內容中指定 **Background** 的 **ActivationType** 並不會變更行為。


## <a name="step-7-remove-and-manage-notifications"></a>步驟7：移除和管理通知

移除和管理通知與 UWP app 相同。 不過，建議您使用我們的相容性程式庫來取得 **DesktopNotificationHistoryCompat** ，如此一來，如果您使用的是傳統桌面，就不需要擔心提供 AUMID。

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-8-deploying-and-debugging"></a>步驟8：部署和調試

若要部署和 MSIX 應用程式，請參閱 [執行、偵測和測試已封裝的桌面應用程式](/windows/msix/desktop/desktop-to-uwp-debug)。

若要部署和偵測傳統桌面應用程式，您必須先透過安裝程式安裝您的應用程式一次，然後才能正常進行偵錯工具，讓 AUMID 和 CLSID 的開始快速鍵存在。 [開始] 畫面捷徑出現後，您可以使用 Visual Studio 的 F5 來偵錯。

如果您的通知不會出現在您的傳統桌面應用程式中 (而且) 不會擲回任何例外狀況，這可能表示 (透過安裝程式) 安裝您的應用程式，或是您在程式碼中使用的 AUMID 與開始快速鍵中的 AUMID 不相符。

如果您的通知會顯示，但不會保留在控制中心 (快顯視窗關閉後就消失)，這表示您未正確實作 COM 啟動者。

如果您已安裝 MSIX/sparse 封裝和傳統桌面應用程式，請注意 MSIX/sparse 套件應用程式會在處理快顯快顯時取代傳統傳統型應用程式。 這表示，從傳統桌面應用程式快顯通知時，仍然會在按一下時啟動 MSIX/sparse 套件應用程式。 卸載 MSIX/sparse 封裝應用程式將會還原回傳統桌面應用程式。


## <a name="known-issues"></a>已知問題

**已修正：按一下快顯通知之後應用程式不會成為焦點** ：在組建 15063 與更早版本中，當我們啟用 COM 伺服器時前景權限無法傳輸至您的應用程式。 因此，當您嘗試將它移動到前景時，您的應用程式只會閃爍。 此問題沒有解決方法。 我們在組建 16299 與更高版本中已修正這個問題。


## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/desktop-toasts)
* [桌面應用程式的快顯通知](toast-desktop-apps.md)
* [快顯通知內容文件](adaptive-interactive-toasts.md)
