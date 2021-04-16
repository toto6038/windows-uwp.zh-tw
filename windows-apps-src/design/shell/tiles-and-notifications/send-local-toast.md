---
description: '瞭解如何從 c # 應用程式傳送本機快顯通知，並處理使用者按一下快顯通知。'
title: '從 c # 應用程式傳送本機快顯通知'
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from C# apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: 'windows 10、uwp、傳送快顯通知、通知、傳送通知、快顯通知、如何、快速入門、使用者入門、程式碼範例、逐步解說、c #、csharp、win32、desktop'
ms.localizationpriority: medium
ms.openlocfilehash: 23896b65bf2e4e0a9fc2edf5744b647d6d9c26ed
ms.sourcegitcommit: 6cd970686d1ea7176b7e6651f349a14551709820
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2021
ms.locfileid: "107559390"
---
# <a name="send-a-local-toast-notification-from-c-apps"></a>從 c # 應用程式傳送本機快顯通知

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> 如果您要撰寫 c + + 應用程式，請參閱 [c + + UWP](send-local-toast-cpp-uwp.md) 或 [c + + WRL](send-local-toast-desktop-cpp-wrl.md) 檔。


## <a name="step-1-install-nuget-package"></a>步驟1：安裝 NuGet 套件

[!INCLUDE [nuget package](includes/nuget-package.md)]

[!INCLUDE [nuget package .NET warnings](includes/nuget-package-dotnet-warnings.md)]

我們的程式碼範例會使用此套件。 此套件可讓您在不使用 XML 的情況下建立快顯通知，也可讓桌面應用程式傳送快顯通知。


## <a name="step-2-send-a-toast"></a>步驟2：傳送快顯通知

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```csharp
// Requires Microsoft.Toolkit.Uwp.Notifications NuGet package version 7.0 or greater
new ToastContentBuilder()
    .AddArgument("action", "viewConversation")
    .AddArgument("conversationId", 9813)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, The Enchantments in Washington!")
    .Show(); // Not seeing the Show() method? Make sure you have version 7.0, and if you're using .NET 5, your TFM must be net5.0-windows10.0.17763.0 or greater
```

請嘗試執行此程式碼，您應該會看到通知出現！


## <a name="step-3-handling-activation"></a>步驟3：處理啟用

顯示通知之後，您可能需要處理使用者按一下通知 (是否會在使用者按一下、開啟您的應用程式，或在使用者按一下通知) 時執行動作。

處理啟用的步驟會因 UWP、Desktop (MSIX) 和 Desktop (未封裝) 應用程式而有所不同。


#### <a name="uwp"></a>[UWP](#tab/uwp)

當使用者按一下您的通知 (或具有前景啟用) 通知的按鈕時，將會叫用應用 **程式的** **OnActivated** ，而且將會傳回您新增的引數。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        ToastArguments args = ToastArguments.Parse(toastActivationArgs.Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]

#### <a name="desktop-msix"></a>[Desktop (MSIX) ](#tab/desktop-msix)

首先，在您的 **package.appxmanifest** 中，新增：

1. **xmlns:com** 的宣告
1. **xmlns:desktop** 的宣告
1. 在 **IgnorableNamespaces** 屬性中，**com** 和 **desktop**
1. **desktop：** **toastNotificationActivation** 的延伸模組，可使用您選擇的新 GUID)  (宣告您的快顯啟動程式 CLSID。
1. 僅限 MSIX：使用步驟 #4 中的 GUID 之 COM 啟動項的 **com： Extension** 。 請務必包含， `Arguments="-ToastActivated"` 讓您知道您的啟動是來自通知

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

        <!--Specify which CLSID to activate when toast clicked-->
        <desktop:Extension Category="windows.toastNotificationActivation">
          <desktop:ToastNotificationActivation ToastActivatorCLSID="replaced-with-your-guid-C173E6ADF0C3" /> 
        </desktop:Extension>

        <!--Register COM CLSID LocalServer32 registry key-->
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="YourProject\YourProject.exe" Arguments="-ToastActivated" DisplayName="Toast activator">
              <com:Class Id="replaced-with-your-guid-C173E6ADF0C3" DisplayName="Toast activator"/>
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>

      </Extensions>
    </Application>
  </Applications>
 </Package>
```

然後， **在應用程式的啟始程式碼中** (適用于 WPF) 的 OnStartup，訂閱 OnActivated 事件。

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]


[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]


#### <a name="desktop-unpackaged"></a>[Desktop (未封裝的) ](#tab/desktop)

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-sequence.md)]

**在應用程式的啟始程式碼中** (適用于 WPF) 的 OnStartup，請訂閱 OnActivated 事件。

[!INCLUDE [desktop toast activation sequence](includes/desktop-toast-activation-code.md)]

---


## <a name="step-4-handling-uninstallation"></a>步驟4：處理卸載

#### <a name="uwp"></a>[UWP](#tab/uwp)

您不需要採取任何動作！ 卸載 UWP 應用程式時，系統會自動清除所有通知和任何其他相關資源。

#### <a name="desktop-msix"></a>[Desktop (MSIX) ](#tab/desktop-msix)

您不需要採取任何動作！ 卸載 MSIX 應用程式時，系統會自動清除所有通知和任何其他相關資源。

#### <a name="desktop-unpackaged"></a>[Desktop (未封裝的) ](#tab/desktop)

如果您的應用程式具有卸載程式，您應該在您的卸載程式中呼叫 `ToastNotificationManagerCompat.Uninstall();` 。 如果您的應用程式是沒有安裝程式的「可移植應用程式」，除非您有要在應用程式關閉後保存的通知，否則請考慮在應用程式結束時呼叫此方法。

卸載方法將清除任何已排程和目前的通知、移除任何相關聯的登錄值，並移除程式庫所建立的所有相關暫存檔案。

---


## <a name="adding-images"></a>新增影像

您可以在通知中加入豐富的內容。 我們會將內嵌影像和設定檔新增 (應用程式標誌覆寫) 映射。

[!INCLUDE [images note](includes/images-note.md)]

> [!IMPORTANT]
> 只有在其資訊清單中具有網際網路功能的 UWP/MSIX/sparse 應用程式中，才支援 Http 映射。 桌面非 MSIX/稀疏應用程式不支援 HTTP 映射;您必須將映射下載至本機應用程式資料，並在本機進行參考。

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content and show the toast!
new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .Show();
```



## <a name="adding-buttons-and-inputs"></a>新增按鈕和輸入

您可以新增按鈕和輸入，讓您的通知成為互動。 按鈕可以啟動您的前景應用程式、通訊協定或背景工作。 我們會新增回復文字方塊、[贊] 按鈕，以及開啟影像的 [View] 按鈕。

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```csharp
int conversationId = 384928;

// Construct the content
new ToastContentBuilder()
    .AddArgument("conversationId", conversationId)
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Buttons
    .AddButton(new ToastButton()
        .SetContent("Reply")
        .AddArgument("action", "reply")
        .SetBackgroundActivation())

    .AddButton(new ToastButton()
        .SetContent("Like")
        .AddArgument("action", "like")
        .SetBackgroundActivation())

    .AddButton(new ToastButton()
        .SetContent("View")
        .AddArgument("action", "viewImage")
        .AddArgument("imageUrl", image.ToString()))
    
    .Show();
```

前景按鈕的啟用方式會與 (您的應用程式的主要快顯通知主體相同。) 會呼叫 OnActivated。

請注意，當按一下按鈕時，也會傳回新增至最上層快顯通知 (例如對話識別碼) 的引數，只要按鈕使用 AddArgument API 就會傳回 (如果您在按鈕上自訂指派引數，則不會包含最上層引數) 。



## <a name="handling-background-activation"></a>處理背景啟用

#### <a name="uwp"></a>[UWP](#tab/uwp)

當您在快顯通知 (或快顯通知內的按鈕) 上指定背景啟用時，將會執行背景工作，而不是啟用前景 App。

如需背景工作的詳細資訊，請參閱[使用背景工作支援 App](../../../launch-resume/support-your-app-with-background-tasks.md)。

如果您的目標組建為 14393 或更高版本，則可以同處理序背景工作，這將情況大幅簡化了。 請注意，同處理序背景工作無法在舊版 Windows 上執行。 我們會在此程式碼範例中使用同處理序背景工作。

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


然後，在您的 App.config 中，覆寫 OnBackgroundActivated 方法。 然後，您可以取得預先定義的引數和使用者輸入，類似于前景啟用。

**App.xaml.cs**

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                ToastArguments arguments = ToastArguments.Parse(details.Argument);
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```

#### <a name="desktop"></a>[桌上型電腦](#tab/desktop-msix+desktop)

針對桌面應用程式，背景啟用的處理方式與前景啟用相同 (您的 **OnActivated** 事件處理常式將會) 觸發。 您可以選擇在處理啟用之後，不顯示任何 UI 並關閉您的應用程式。

---


## <a name="set-an-expiration-time"></a>設定到期時間

在 Windows 10 中，所有快顯通知都會在使用者關閉或忽略之後進入控制中心，因此使用者可以在快顯視窗消失後查看您的通知。

不過，如果通知中的訊息僅與一段時間有關，您應該在快顯通知上設定到期時間，這樣才不會讓使用者在您的 App 中看到過時的資訊。 例如，如果促銷期限為 12 小時，可將到期時間設為 12 個小時。 在下面程式碼中，我們將到期時間設定為 2 天。

> [!NOTE]
> 本機快顯通知的預設及最長到期時間為 3 天。

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .Show(toast =>
    {
        toast.ExpirationTime = DateTime.Now.AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果想以程式設計方式移除或取代您傳送的通知，您必須使用 Tag 屬性 (並選擇性使用 Group 屬性) 提供通知的主索引鍵。 那麼，日後就可以使用這個主索引鍵來移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```csharp
// Create toast content and show the toast!
new ToastContentBuilder()
    .AddText("New post on your wall!")
    .Show(toast =>
    {
        toast.Tag = "18365";
        toast.Group = "wallPosts";
    });
```



## <a name="clear-your-notifications"></a>清除您的通知

應用程式會負責移除和清除自己的通知。 啟動您的 App 時，我們並不會自動清除您的通知。

Windows 只有在使用者明確按一下通知時，才會自動移除通知。

以下是一個範例，示範訊息中心 App 應該會做些什麼...

1. 使用者在交談中收到多個關於新訊息的快顯通知
2. 使用者點選其中一個的快顯通知來開啟交談
3. App 開啟交談，然後清除該交談的所有快顯通知 (方法是針對該交談在 App 提供的群組上使用 [RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_))
4. 使用者的控制中心現在會正確反映通知狀態，因為沒有該交談的任何過時通知留存在控制中心上。

若要了解清除所有通知，或移除特定通知，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))。

```csharp
ToastNotificationManagerCompat.History.Clear();
```



## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [快顯通知內容文件](adaptive-interactive-toasts.md)
* [ToastNotification 類別](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 類別](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
