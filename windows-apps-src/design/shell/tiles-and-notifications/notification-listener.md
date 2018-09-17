---
author: andrewleader
Description: Learn how to use Notification Listener to access all of the user's notifications.
title: 通知接聽程式
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 通知接聽程式, usernotificationlistener, 文件, 存取通知
ms.localizationpriority: medium
ms.openlocfilehash: f4d8cb9ef7589bd8f0c56586ab8fcfec7c1f01e3
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2018
ms.locfileid: "3987143"
---
# <a name="notification-listener-access-all-notifications"></a>通知接聽程式：存取所有通知

通知接聽程式可供存取使用者的通知。 智慧手錶與其他穿戴式裝置可以使用通知接聽程式來傳送手機通知給穿戴式裝置。 家庭自動化應用程式可以使用通知接聽程式，在收到通知時執行特定動作，例如在接到電話時讓燈光閃爍。 

> [!IMPORTANT]
> **需要年度更新版**：您的目標必須是 SDK 14393 並執行組建 14393 或更新版本，才能使用通知接聽程式。


> **重要 API**：[UserNotificationListener 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener)、[UserNotificationChangedTrigger 類別](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>新增使用者通知功能來啟用接聽程式 

若要使用通知接聽程式，您必須新增使用者通知接聽程式功能到您的應用程式資訊清單。

1. 在 Visual Studio 的 \[方案總管\] 中，按兩下 `Package.appxmanifest` 檔案以開啟資訊清單設計工具。
2. 開啟 \[功能\] 索引標籤。
3. 勾選 **\[使用者通知接聽程式\]** 功能。


## <a name="check-whether-the-listener-is-supported"></a>檢查是否支援接聽程式

如果您的應用程式支援舊版 Windows 10，則需使用 [ApiInformation 類別](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation)來檢查是否支援接聽程式。  如果接聽程式不受支援，請避免執行任何接聽程式 API 呼叫。

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>要求存取接聽程式

由於接聽程式允許存取使用者的通知，使用者必須提供存取其通知的權限給您的應用程式。 在應用程式首次執行期間，您應該要求存取權以使用通知接聽程式。 如果您想，也可以在呼叫 [RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync) 之前顯示初步 UI，來解釋您的應用程式為何需要使用者通知的存取權，讓使用者了解為什麼他們應允許存取。

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

使用者隨時可以透過 Windows 設定撤銷存取權。 因此，您的應用程式在執行使用通知接聽程式的程式碼之前，應該一律透過 [GetAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) 方法檢查存取狀態。 如果使用者撤銷存取權，API 將會失敗且不顯示訊息，而非擲回例外狀況 (例如，取得所有通知的 API 只會傳回空的清單)。


## <a name="access-the-users-notifications"></a>存取使用者的通知

利用通知接聽程式，您可以取得使用者目前通知的清單。 只需呼叫 [GetNotificationsAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) 方法，並指定您要取得的通知類型 (目前支援的通知類型只有快顯通知)。

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>顯示通知

每個通知會表示為 [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification)，提供通知來源應用程式的相關資訊、通知建立的時間、通知的 ID 和通知本身內容。

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

[AppInfo](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.AppInfo) 屬性提供顯示通知所需的資訊。

> [!NOTE]
> 我們建議在 try/catch 中包含處理單一通知的所有程式碼，以避免擷取單一通知時發生例外狀況。 您不應只因一個特定通知發生問題，就完全無法顯示其他通知。

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

通知本身的內容 (例如通知文字) 包含在 [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.Notification) 屬性中。 此屬性包含通知的視覺效果部分。 (如果您熟悉如何在 Windows 上傳送通知，就會注意到 [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification) 物件中的 [Visual](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification.Visual) 和 [Visual.Bindings](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationvisual.Bindings) 屬性對應至彈出通知時開發人員傳送的內容)。

我們想尋找快顯通知繫結 (對於錯誤驗證程式碼，您應檢查繫結不是 null)。 從繫結可以取得文字元素。 您可以選擇顯示任意數目的文字元素。 (理論上，您應該將它們全部顯示)。您可以選擇以不同方式處理文字元素；例如，將第一個文字視為標題，後續文字視為本文)。

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>移除特定通知

如果您的穿戴式裝置或服務允許使用者關閉通知，您可以移除實際通知，讓使用者稍後不會在他們的手機或電腦上看到。 只需提供您要移除之通知的通知識別碼即可 (從 [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification) 物件取得)： 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>清除所有通知

[UserNotificationListener.ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) 方法會清除所有使用者的通知。 使用此方法應小心謹慎。 您只應在穿戴式裝置或服務會顯示「所有」通知時，才清除所有通知。 如果穿戴式裝置或服務只會顯示特定通知，當使用者按下 \[清除通知\] 按鈕，使用者只預期移除這些特定通知；然而，呼叫 [ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) 方法實際上會移除所有通知，包括穿戴式裝置或服務未顯示的通知。

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>新增/關閉通知的背景工作

啟用應用程式來接聽通知的常見方式是設定背景工作，無論您的應用程式是否正在執行，都讓您知道何時新增或關閉了通知。

歸功於年度更新版新增了[單一處理程序模型](../../../launch-resume/create-and-register-an-inproc-background-task.md)，新增背景工作非常簡單。 在主要應用程式的程式碼中，在您已經取得通知接聽程式的使用者存取權並取得執行背景工作的存取權後，只需註冊新的背景工作，並使用[快顯通知類型](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationkinds)設定 [UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) 即可。

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

然後，在 App.xaml.cs 中，覆寫 [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) 方法 (如果尚未覆寫)，並在工作名稱上使用 switch 陳述式來判斷您的眾多背景工作觸發程序中是哪一個被叫用。

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

背景工作是只是「拍肩提醒」：它並不提供新增或移除哪個特定通知的任何相關資訊。 背景工作被觸發時，您應該同步穿戴式裝置上的通知，讓它們反映平台中的通知。 這樣可確保如果您的背景工作失敗，穿戴式裝置上的通知仍可在背景工作下一次執行時復原。

`SyncNotifications` 是您實作的方法；下一節將顯示方式。 


## <a name="determining-which-notifications-were-added-and-removed"></a>判斷新增和移除了哪些通知

在 `SyncNotifications` 方法中，若要判斷已新增或移除哪些通知 (同步穿戴式裝置上的通知)，您需要計算目前通知集合以及平台中通知之間的差異。

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Othwerise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>新增/關閉通知的前景事件

> [!IMPORTANT] 
> 已知問題： 前景事件最新版 Windows 中，會造成 CPU 迴圈與先前無法運作，之前。 請勿使用前景事件。 在 Windows 的未來更新，我們將會修正此問題。

而不是使用前景事件，使用較舊版本顯示為[單一處理程序模型](../../../launch-resume/create-and-register-an-inproc-background-task.md)的背景工作的程式碼。 背景工作也可讓您關閉或執行您的應用程式時收到變更事件通知這兩個。

```csharp
// Subscribe to foreground event (DON'T USE THIS)
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // NOTE: This event WILL CAUSE CPU LOOPS, DO NOT USE. Use the background task instead.
}
```


## <a name="how-to-fix-delays-in-the-background-task"></a>如何修正背景工作的延遲

在測試應用程式時，您可能會注意到背景工作有時會延遲，且可能有幾分鐘的時間不會觸發。 若要修正這個問題，請提示使用者移至 \[系統設定\] -> \[系統\] -> \[電池\] -> \[依 App 的電池使用情況\]，在清單中找到您的應用程式、選取它，然後變更為 \[一律允許在背景中執行\]。 設定好之後，背景工作應一律會在收到通知的一秒內便觸發。