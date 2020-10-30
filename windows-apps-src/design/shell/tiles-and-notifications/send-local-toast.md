---
description: 瞭解如何從 UWP 應用程式傳送本機快顯通知，並處理使用者按一下快顯通知。
title: 從 UWP 應用程式傳送本機快顯通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, 傳送快顯通知, 通知, 傳送通知, 快顯通知, 如何, 快速入門, 開始使用, 程式碼範例, 逐步解說
ms.localizationpriority: medium
ms.openlocfilehash: b532e041ffbbcf4a2ecac0e3386430b65d833f2d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034481"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>從 UWP 應用程式傳送本機快顯通知


快顯通知是一則訊息，App 可以建構此訊息，並於使用者目前未在 App 內時，傳遞給使用者。 這項快速入門會逐步引導您使用新的調適型範本和互動式動作，完成建立、傳遞和顯示 Windows 10 快顯通知的步驟。 這些動作透過本機通知進行示範，本機通知是我們要實作的最簡單通知。

> [!IMPORTANT]
> 傳統型應用程式 (包括封裝 [MSIX](/windows/msix/desktop/source-code-overview) 應用程式、使用 [稀疏套件](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 來取得套件身分識別的應用程式，以及傳統非封裝的桌面應用程式) 有不同的步驟來傳送通知和處理啟用。 請參閱 [傳統型應用程式](toast-desktop-apps.md)文件以了解如何實作快顯通知。

> **重要 API** ： [ToastNotification 類別](/uwp/api/Windows.UI.Notifications.ToastNotification)、 [ToastNotificationActivatedEventArgs 類別](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)



## <a name="step-1-install-nuget-package"></a>步驟1：安裝 NuGet 套件

安裝 node.js. [ [通知] NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。 我們的程式碼範例會使用此套件。 在本文結尾，我們將提供不使用任何 NuGet 套件的「一般」程式碼片段。 此套件可讓您在不使用 XML 的情況下建立快顯通知。


## <a name="step-2-add-namespace-declarations"></a>步驟2：加入命名空間宣告

`Windows.UI.Notifications` 包含快顯通知 Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>步驟3：傳送快顯通知

在 Windows 10 中，您的快顯通知內容是使用賦予通知外觀極大彈性的調適性語言進行描述。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。

我們將從以文字為基礎的簡單通知開始。 使用通知程式庫) 來建立通知內容 (，並顯示通知！

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show();
```

## <a name="step-4-handling-activation"></a>步驟4：處理啟用

當使用者按一下您的通知 (或具有前景啟用) 通知的按鈕時，將會叫用應用程式的 **App.xaml.cs** **OnActivated** 。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> 就像您的 **OnLaunched** 程式碼一樣，您必須初始化畫面並啟用視窗。 **如果使用者按一下您的快顯通知，並不會呼叫 OnLaunched** ，即使您的 App 已關閉，然後正在首次啟動，也不會。 我們經常建議將 **OnLaunched** 和 **OnActivated** 結合成 `OnLaunchedOrActivated` 方法，因為兩者都需要進行相同的初始設定。


## <a name="activation-in-depth"></a>深入啟用

讓您的通知可採取動作的第一個步驟，就是將一些啟動引數新增至您的通知，讓您的應用程式可以知道當使用者按一下通知時要啟動的內容 (在此案例中，我們會包含一些資訊，稍後會告訴我們，我們應該開啟交談，而我們知道要開啟哪個特定交談) 。

建議您安裝 [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/) NuGet 套件，以協助針對您的通知引數來建立和剖析查詢字串，如下所示。

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


以下是更複雜的處理啟用範例 .。。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="adding-images"></a>新增影像

您可以在通知中加入豐富的內容。 我們會將內嵌影像和設定檔新增 (應用程式標誌覆寫) 映射。

> [!NOTE]
> 您可以使用 App 套件、App 本機存放區或網頁中的影像。 從 Fall Creators Update 開始，一般連線的網頁影像可以高達 3 MB，而計量付費連線可以高達 1 MB。 在尚未執行 Fall Creators Update 的裝置上，網頁影像不得超過 200 KB。

> [!IMPORTANT]
> 只有在其資訊清單中具有網際網路功能的 UWP/MSIX/sparse 應用程式中，才支援 Http 映射。 桌面非 MSIX/稀疏應用程式不支援 HTTP 映射;您必須將映射下載至本機應用程式資料，並在本機進行參考。

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>新增按鈕和輸入

您可以新增按鈕和輸入，讓您的通知成為互動。 按鈕可以啟動您的前景應用程式、通訊協定或背景工作。 我們會新增回復文字方塊、[贊] 按鈕，以及開啟影像的 [View] 按鈕。

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

前景按鈕的啟動方式與主要的快顯通知主體相同， (您的 App.xaml.cs OnActivated 將被呼叫) 。



## <a name="handling-background-activation"></a>處理背景啟用

當您在快顯通知 (或快顯通知內的按鈕) 上指定背景啟用時，將會執行背景工作，而不是啟用前景 App。

如需背景工作的詳細資訊，請參閱[使用背景工作支援 App](/windows/uwp/launch-resume/support-your-app-with-background-tasks)。

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


然後在您的 App.xaml.cs 中，覆寫 OnBackgroundActivated 方法。 然後，您可以取得預先定義的引數和使用者輸入，類似于前景啟用。

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

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果想以程式設計方式移除或取代您傳送的通知，您必須使用 Tag 屬性 (並選擇性使用 Group 屬性) 提供通知的主索引鍵。 那麼，日後就可以使用這個主索引鍵來移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>清除您的通知

UWP app 會負責移除並清除其本身的通知。 啟動您的 App 時，我們並不會自動清除您的通知。

Windows 只有在使用者明確按一下通知時，才會自動移除通知。

以下是一個範例，示範訊息中心 App 應該會做些什麼...

1. 使用者在交談中收到多個關於新訊息的快顯通知
2. 使用者點選其中一個的快顯通知來開啟交談
3. App 開啟交談，然後清除該交談的所有快顯通知 (方法是針對該交談在 App 提供的群組上使用 [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_))
4. 使用者的控制中心現在會正確反映通知狀態，因為沒有該交談的任何過時通知留存在控制中心上。

若要了解清除所有通知，或移除特定通知，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))。

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>純文字程式碼片段

如果您不是使用來自 NuGet 的通知程式庫，您可以手動建立您的 XML，如下所示來建立 [ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification)。

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>資源

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [快顯通知內容文件](adaptive-interactive-toasts.md)
* [ToastNotification 類別](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 類別](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
