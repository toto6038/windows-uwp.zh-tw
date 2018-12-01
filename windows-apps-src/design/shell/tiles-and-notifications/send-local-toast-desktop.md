---
Description: Learn how Win32 C# apps can send local toast notifications and handle the user clicking the toast.
title: 從傳統型 C# 應用程式傳送本機快顯通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from desktop C# apps
template: detail.hbs
ms.date: 01/23/2018
ms.topic: article
keywords: windows 10, uwp, win32, 傳統型, 快顯通知, 傳送快顯通知, 傳送本機快顯通知, 傳統型橋接器, C#, c sharp
ms.localizationpriority: medium
ms.openlocfilehash: 2c32ba5928d3c272ef77a70790640346fb000762
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8337184"
---
# <a name="send-a-local-toast-notification-from-desktop-c-apps"></a>從傳統型 C# 應用程式傳送本機快顯通知

傳統型應用程式 (傳統型橋接器和傳統的 Win32) 可以像通用 Windows 平台 (UWP) App 一樣傳送互動式快顯通知。 但是，如果您不使用傳統型橋接器，則由於不同的啟用配置和可能缺乏套件識別資料，傳統型應用程式有幾個特殊步驟。

> [!IMPORTANT]
> 如果您在撰寫 UWP app，請參閱 [UWP 文件](send-local-toast.md)。 對於其他傳統型語言，請參閱[傳統型 C++ WRL](send-local-toast-desktop-cpp-wrl.md)。


## <a name="step-1-enable-the-windows-10-sdk"></a>步驟 1：啟用 Windows 10 SDK

如果您尚未為 Win32 應用程式啟用 Windows 10 SDK，您必須先執行此步驟。

在專案上按一下滑鼠右鍵，然後選取 **\[卸載專案\]**。

![卸載專案](images/win32-unload-project.png)

接著再次以滑鼠右鍵按一下您的專案，然後選取 **\[編輯 [projectname].csproj\]**

![編輯專案](images/win32-edit-project.png)

在現有的 `<TargetFrameworkVersion>` 節點下面，新增 `<TargetPlatformVersion>` 節點來指定您支援的最低 Windows 10 版本。 實際使用的 SDK 會是您在開發電腦上安裝的最新 SDK。 如此就會指定您允許的最低版本 (並可讓您參考 Windows SDK)。

```xml
...
<TargetFrameworkVersion>...</TargetFrameworkVersion>
<TargetPlatformVersion>10.0.10240.0</TargetPlatformVersion>
...
```

儲存您的變更，然後重新載入您的專案。

![重新載入專案](images/win32-reload-project.png)


## <a name="step-2-reference-the-apis"></a>步驟 2：參考 API

開啟 \[參考管理員\] (在專案上按一下滑鼠右鍵，選取 **\[新增 -> 參考\]**)，然後選取 **\[Windows -> 核心\]** 並加入下列參考：

* Windows.Data
* Windows.UI

![參考管理員](images/win32-add-windows-reference.png)


## <a name="step-3-copy-compat-library-code"></a>步驟 3：複製 Compat 程式庫程式碼

[從 GitHub 複製 DesktopNotificationManagerCompat.cs 檔案](https://raw.githubusercontent.com/WindowsNotifications/desktop-toasts/master/CS/DesktopToastsApp/DesktopNotificationManagerCompat.cs)到您的專案。 Compat 程式庫消除了桌面通知的許多複雜性。 下列指令需要 Compat 程式庫。


## <a name="step-4-implement-the-activator"></a>步驟 4：實作啟動者

您必須實作快顯通知啟用的處理常式，以便當使用者按下快顯通知，您的應用程式可以執行某個動作。 您的快顯通知若要保留在控制中心 (因為可能在您的應用程式關閉幾天時，使用者才按下快顯通知)，此為必要步驟。 這個類別可以放在專案的任何位置。

延伸 **NotificationActivator** 類別然後新增下列三個屬性，並使用眾多線上 GUID 產生器的其中一個，為您的應用程式建立唯一 GUID CLSID。 此 CLSID (類別識別碼) 是控制中心得知 COM 啟用哪個類別的方式。

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


## <a name="step-5-register-with-notification-platform"></a>步驟 5：向通知平台註冊

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

#### <a name="step-51-wix-installer"></a>步驟 5.1：WiX 安裝程式

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


#### <a name="step-52-register-aumid-and-com-server"></a>步驟 5.2：註冊 AUMID 和 COM 伺服器

接著，無論安裝程式為何，在您應用程式的啟動程式碼中 (在呼叫任何通知 API 之前)，呼叫 **RegisterAumidAndComServer** 方法，指定步驟 #4 中的通知啟動者類別和上面使用的 AUMID。

```csharp
// Register AUMID and COM server (for Desktop Bridge apps, this no-ops)
DesktopNotificationManagerCompat.RegisterAumidAndComServer<MyNotificationActivator>("YourCompany.YourApp");
```

如果您同時支援傳統型橋接器和傳統型 Win32，可以隨意呼叫這個方法。 如果您在傳統型橋接器之下執行，此方法會立即傳回。 不需要分支程式碼。

這個方法可讓您呼叫 Compat API 來傳送和管理通知，而不需要持續提供 AUMID。 它會插入 COM 伺服器的 LocalServer32 登錄機碼。


## <a name="step-6-register-com-activator"></a>步驟 6： 註冊 COM 啟動者

對於傳統型橋接器和傳統型 Win32 應用程式，都必須註冊通知啟動者類型，以便處理快顯通知啟用。

在您應用程式的啟動程式碼中，呼叫下列 **RegisterActivator** 方法，傳遞您在步驟 #4 建立的 **NotificationActivator** 類別實作。 必須呼叫此方法，您才能接收快顯通知啟用。

```csharp
// Register COM server and activator type
DesktopNotificationManagerCompat.RegisterActivator<MyNotificationActivator>();
```


## <a name="step-7-send-a-notification"></a>步驟 7：傳送通知

傳送通知與 UWP app 相同，除了您將使用 **DesktopNotificationManagerCompat** 類別來建立 **ToastNotifier**。 Compat 程式庫會自動處理傳統型橋接器和傳統型 Win32 之間的差異，所以您不需要分支您的程式碼。 對於傳統型 Win32，Compat 程式庫會快取您在呼叫 **RegisterAumidAndComServer** 時提供的 AUMID，因此您不必擔心何時要提供或不提供 AUMID。

> [!NOTE]
> 安裝 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，讓您可以使用如下所示的 C# 來建構通知，而不是使用原始 XML。

請務必使用如下所示的 **ToastContent** (如果您正在撰寫 XML 則使用 ToastGeneric 範本)，因為舊版 Windows 8.1 快顯通知範本不會啟用您在步驟 #4 所建立的 COM 通知啟動者。

> [!IMPORTANT]
> 只有資訊清單中具備網際網路功能的傳統型橋接器應用程式才支援 http 影像。 傳統型 Win32 應用程式不支援 http 影像；您必須將影像下載至本機應用程式資料，並在本機參考它。

```csharp
// Construct the visuals of the toast (using Notifications library)
ToastContent toastContent = new ToastContent()
{
    // Arguments when the user taps body of toast
    Launch = "action=viewConversation&conversationId=5",

    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Hello world!"
                }
            }
        }
    }
};

// Create the XML document (BE SURE TO REFERENCE WINDOWS.DATA.XML.DOM)
var doc = new XmlDocument();
doc.LoadXml(toastContent.GetContent());

// And create the toast notification
var toast = new ToastNotification(doc);

// And then show it
DesktopNotificationManagerCompat.CreateToastNotifier().Show(toast);
```

> [!IMPORTANT]
> 傳統 Win32 應用程式無法使用舊版的快顯通知 (例如 ToastText02) 的範本。 指定 COM CLSID 時，啟用舊版範本將會失敗。 您必須使用的 Windows 10 ToastGeneric 範本，如上方所示。


## <a name="step-8-handling-activation"></a>步驟 8：處理啟用

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


## <a name="step-9-remove-and-manage-notifications"></a>步驟 9：移除和管理通知

移除和管理通知與 UWP app 相同。 不過，我們建議您使用我們的 Compat 程式庫來取得 **DesktopNotificationHistoryCompat**，使用傳統型 Win32 時就無需擔心提供 AUMID。

```csharp
// Remove the toast with tag "Message2"
DesktopNotificationManagerCompat.History.Remove("Message2");

// Clear all toasts
DesktopNotificationManagerCompat.History.Clear();
```


## <a name="step-10-deploying-and-debugging"></a>步驟 10：部署和偵錯

若要部署和偵錯您的傳統型橋接器應用程式，請參閱[執行、偵錯以及測試封裝的傳統型應用程式](/porting/desktop-to-uwp-debug.md)。

若要部署和偵錯您的傳統型 Win32 應用程式，在正常偵錯前，您必須透過安裝程式安裝一次您的應用程式，以便顯示包含您的 AUMID 與 CLSID 的 [開始] 畫面捷徑。 [開始] 畫面捷徑出現後，您可以使用 Visual Studio 的 F5 來偵錯。

如果通知無法顯示在傳統型 Win32 應用程式中 (也未傳回例外)，這可能表示 [開始] 畫面捷徑不存在 (透過安裝程式安裝您的應用程式)，或您用於程式碼的 AUMID 不符合 [開始] 畫面捷徑中的 AUMID。

如果您的通知會顯示，但不會保留在控制中心 (快顯視窗關閉後就消失)，這表示您未正確實作 COM 啟動者。

如果您已同時安裝傳統型橋接器和傳統型 Win32 應用程式，請注意在處理快顯通知啟用時，傳統型橋接器應用程式將會取代傳統型 Win32 應用程式。 這表示在點按時，傳統型 Win32 應用程式的快顯通知仍會啟動傳統型橋接器應用程式。 解除安裝傳統型橋接器應用程式可將啟用還原回傳統型 Win32 應用程式。


## <a name="known-issues"></a>已知問題

**已修正：按一下快顯通知之後應用程式不會成為焦點**：在組建 15063 與更早版本中，當我們啟用 COM 伺服器時前景權限無法傳輸至您的應用程式。 因此，當您嘗試將它移動到前景時，您的應用程式只會閃爍。 此問題沒有解決方法。 我們在組建 16299 與更高版本中已修正這個問題。


## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/desktop-toasts)
* [傳統型應用程式的快顯通知:](toast-desktop-apps.md)
* [快顯通知內容文件](adaptive-interactive-toasts.md)

