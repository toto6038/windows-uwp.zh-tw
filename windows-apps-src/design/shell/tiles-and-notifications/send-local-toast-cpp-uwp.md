---
description: 瞭解如何從 c + + UWP 應用程式傳送本機快顯通知，並處理使用者按一下快顯通知。
title: 從 c + + UWP 應用程式傳送本機快顯通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from a C++ UWP app
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10、uwp、傳送快顯通知、通知、傳送通知、快顯通知、操作說明、快速入門、使用者入門、程式碼範例、逐步解說、cpp、c + +、uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5960d6f2a63ea2aa0aea26392db5958825aebf2b
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804962"
---
# <a name="send-a-local-toast-notification-from-c-uwp-apps"></a>從 c + + UWP 應用程式傳送本機快顯通知

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> 如果您要撰寫 c + + 非 UWP 應用程式，請參閱 [c + + WRL](send-local-toast-desktop-cpp-wrl.md) 檔。 如果您要撰寫 c # 應用程式，請參閱 [c # 檔](send-local-toast.md)。



## <a name="step-1-install-nuget-package"></a>步驟1：安裝 NuGet 套件

[!INCLUDE [nuget package](includes/nuget-package.md)]

我們的程式碼範例會使用此套件。 此套件可讓您在不使用 XML 的情況下建立快顯通知。


## <a name="step-2-add-namespace-declarations"></a>步驟2：加入命名空間宣告

```cpp
using namespace Microsoft::Toolkit::Uwp::Notifications;
```


## <a name="step-3-send-a-toast"></a>步驟3：傳送快顯通知

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)
    ->AddText("Andrew sent you a picture")
    ->AddText("Check this out, The Enchantments in Washington!")
    ->Show();
```


## <a name="step-4-handling-activation"></a>步驟4：處理啟用

當使用者按一下您的通知 (或具有前景啟用) 通知的按鈕時，將會叫用 **您的應用程式** **OnActivated** 。

**App.xaml.cpp**

```cpp
void App::OnActivated(IActivatedEventArgs^ e)
{
    // Handle notification activation
    if (e->Kind == ActivationKind::ToastNotification)
    {
        ToastNotificationActivatedEventArgs^ toastActivationArgs = (ToastNotificationActivatedEventArgs^)e;

        // Obtain the arguments from the notification
        ToastArguments^ args = ToastArguments::Parse(toastActivationArgs->Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        auto userInput = toastActivationArgs->UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]


## <a name="activation-in-depth"></a>深入啟用

讓您的通知可採取動作的第一個步驟，就是將一些啟動引數新增至您的通知，讓您的應用程式可以知道當使用者按一下通知時要啟動的內容 (在此案例中，我們會包含一些資訊，稍後會告訴我們，我們應該開啟交談，而我們知道要開啟哪個特定交談) 。

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())

    // Arguments returned when user taps body of notification
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)

    ->AddText("Andrew sent you a picture")
    ->Show();
```



## <a name="adding-images"></a>新增影像

您可以在通知中加入豐富的內容。 我們會將內嵌影像和設定檔新增 (應用程式標誌覆寫) 映射。

[!INCLUDE [images note](includes/images-note.md)]

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ...

    // Inline image
    ->AddInlineImage(ref new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    ->AddAppLogoOverride(ref new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop::Circle)
    
    ->Show();
```



## <a name="adding-buttons-and-inputs"></a>新增按鈕和輸入

您可以新增按鈕和輸入，讓您的通知成為互動。 按鈕可以啟動您的前景應用程式、通訊協定或背景工作。 我們會新增回復文字方塊、[贊] 按鈕，以及開啟影像的 [View] 按鈕。

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```cpp
// Construct the content
(ref new ToastContentBuilder())
    ->AddArgument("conversationId", 9813)
    ...

    // Text box for replying
    ->AddInputTextBox("tbReply", "Type a response")

    // Buttons
    ->AddButton((ref new ToastButton())
        ->SetContent("Reply")
        ->AddArgument("action", "reply")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("Like")
        ->AddArgument("action", "like")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("View")
        ->AddArgument("action", "view"))
    
    ->Show();
```

前景按鈕的啟用方式會與 (您的應用程式的主要快顯通知主體相同。 OnActivated 將會) 呼叫。



## <a name="handling-background-activation"></a>處理背景啟用

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
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```


## <a name="set-an-expiration-time"></a>設定到期時間

在 Windows 10 中，所有快顯通知都會在使用者關閉或忽略之後進入控制中心，因此使用者可以在快顯視窗消失後查看您的通知。

不過，如果通知中的訊息僅與一段時間有關，您應該在快顯通知上設定到期時間，這樣才不會讓使用者在您的 App 中看到過時的資訊。 例如，如果促銷期限為 12 小時，可將到期時間設為 12 個小時。 在下面程式碼中，我們將到期時間設定為 2 天。

> [!NOTE]
> 本機快顯通知的預設及最長到期時間為 3 天。

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("Expires in 2 days...")
    ->Show(toast =>
    {
        toast->ExpirationTime = DateTime::Now->AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果想以程式設計方式移除或取代您傳送的通知，您必須使用 Tag 屬性 (並選擇性使用 Group 屬性) 提供通知的主索引鍵。 那麼，日後就可以使用這個主索引鍵來移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("New post on your wall!")
    ->Show(toast =>
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

```cpp
ToastNotificationManagerCompat::History->Clear();
```



## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [快顯通知內容文件](adaptive-interactive-toasts.md)
* [ToastNotification 類別](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 類別](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)