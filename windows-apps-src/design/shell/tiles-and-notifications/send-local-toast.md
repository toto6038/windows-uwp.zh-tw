---
author: andrewleader
Description: Learn how to send a local toast notification and handle the user clicking the toast.
title: 傳送本機快顯通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 傳送快顯通知, 通知, 傳送通知, 快顯通知, 如何, 快速入門, 開始使用, 程式碼範例, 逐步解說
ms.localizationpriority: medium
ms.openlocfilehash: 656e6123db1fc9ea0f3d8c6b6fb106864200e431
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "5445595"
---
# <a name="send-a-local-toast-notification"></a>傳送本機快顯通知


快顯通知是一則訊息，App 可以建構此訊息，並於使用者目前未在 App 內時，傳遞給使用者。 這項快速入門會逐步引導您使用新的調適型範本和互動式動作，完成建立、傳遞和顯示 Windows 10 快顯通知的步驟。 這些動作透過本機通知進行示範，本機通知是我們要實作的最簡單通知。

> [!IMPORTANT]
> 傳統型應用程式 (傳統型橋接器和傳統型 Win32) 對於傳送通知及處理啟用有不同的步驟。 請參閱 [傳統型應用程式](toast-desktop-apps.md)文件以了解如何實作快顯通知。

我們將會詳細討論下列內容：

### <a name="sending-a-toast"></a>傳送快顯通知

* 建構通知的視覺效果部分 (文字和影像)
* 將動作新增至通知
* 在快顯通知上設定到期時間
* 設定標記/群組，讓您稍後可以取代/移除快顯通知
* 使用本機 API 傳送快顯通知

### <a name="handling-activation"></a>處理啟用

* 在按一下內文或按鈕時處理啟用
* 處理前景啟用
* 處理背景啟用

> **重要 API**：[ToastNotification 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)、[ToastNotificationActivatedEventArgs 類別](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)


## <a name="prerequisites"></a>必要條件

要完全了解本主題，下列項目將有所幫助...

* 快顯通知詞彙與概念的作業知識。 如需詳細資訊，請參閱[快顯通知與控制中心概觀](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)。
* 熟悉 Windows 10 快顯通知內容。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。
* Windows 10 UWP app 專案

> [!NOTE]
> 不同於 Windows 8/8.1，您已不需要在 App 的資訊清單中宣告您的 App 能夠顯示快顯通知。 所有 App 都可以傳送和顯示快顯通知。

> [!NOTE]
> **Windows 8/8.1 應用程式**：請使用[封存文件](https://msdn.microsoft.com/library/windows/apps/xaml/hh868254.aspx)。


## <a name="install-nuget-packages"></a>安裝 NuGet 套件

建議您將下列兩個 NuGet 套件安裝到您的專案。 我們程式碼範例將會使用這些套件。 文章最後會提供不使用任何 NuGet 套件的「Vanilla」程式碼片段。

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)：透過物件而不透過原始 XML 來產生快顯通知承載。
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/)：使用 C# 產生和剖析查詢字串


## <a name="add-namespace-declarations"></a>加入命名空間宣告

`Windows.UI.Notifications` 包含快顯通知 API。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="send-a-toast"></a>傳送快顯通知

在 Windows 10 中，您的快顯通知內容是使用賦予通知外觀極大彈性的調適性語言進行描述。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。

### <a name="constructing-the-visual-part-of-the-content"></a>建構內容的視覺部分

我們這就開始建構內容的視覺部分，其中包括您想要讓使用者看到的文字及影像。

歸功於 Notifications 程式庫，產生 XML 內容很簡單。 如果您未安裝 NuGet 提供的 Notifications 程式庫，就必須手動建構 XML，很有可能會發生錯誤。

> [!NOTE]
> 您可以使用 App 套件、App 本機存放區或網頁中的影像。 從 Fall Creators Update 開始，一般連線的網頁影像可以高達 3 MB，而計量付費連線可以高達 1 MB。 在尚未執行 Fall Creators Update 的裝置上，網頁影像不得超過 200 KB。

```csharp
// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "https://picsum.photos/360/202?image=883";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// Construct the visuals of the toast
ToastVisual visual = new ToastVisual()
{
    BindingGeneric = new ToastBindingGeneric()
    {
        Children =
        {
            new AdaptiveText()
            {
                Text = title
            },
 
            new AdaptiveText()
            {
                Text = content
            },
 
            new AdaptiveImage()
            {
                Source = image
            }
        },
 
        AppLogoOverride = new ToastGenericAppLogo()
        {
            Source = logo,
            HintCrop = ToastGenericAppLogoCrop.Circle
        }
    }
};
```


### <a name="constructing-actions-part-of-the-content"></a>建構內容的動作部分

我們現在來將動作新增至內容。

在下方範例中，我們包含一個可讓使用者輸入文字的輸入項目，當使用者按下其中一個按鈕或快顯通知本身時，就會將文字傳回給應用程式。

接著建立兩個按鈕，每個按鈕各自指定其啟用類型、內容和引數。
* **ActivationType** 用來指定使用者執行此動作時要如何啟用您的 App。 您可以選擇在前景在啟動 App、啟動背景工作，或透過通訊協定啟動另一個 App。 無論您的應用程式選擇前景或背景，您一律會收到使用者輸入和您指定的引數，因此您的應用程式可以執行正確的動作，例如傳送訊息或開啟對話。

```csharp
// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Construct the actions for the toast (inputs and buttons)
ToastActionsCustom actions = new ToastActionsCustom()
{
    Inputs =
    {
        new ToastTextBox("tbReply")
        {
            PlaceholderContent = "Type a response"
        }
    },
 
    Buttons =
    {
        new ToastButton("Reply", new QueryString()
        {
            { "action", "reply" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background,
            ImageUri = "Assets/Reply.png",
 
            // Reference the text box's ID in order to
            // place this button next to the text box
            TextBoxId = "tbReply"
        },
 
        new ToastButton("Like", new QueryString()
        {
            { "action", "like" },
            { "conversationId", conversationId.ToString() }
 
        }.ToString())
        {
            ActivationType = ToastActivationType.Background
        },
 
        new ToastButton("View", new QueryString()
        {
            { "action", "viewImage" },
            { "imageUrl", image }
 
        }.ToString())
    }
};
```


### <a name="combining-the-above-to-construct-the-full-content"></a>結合以上所述來建構完整內容

內容的建構完成了，現在可以用來執行個體化 [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) 物件。

**注意**：若要指定使用者點選快顯通知內文時需要進行什麼類型的啟用作業，您也可以在根元素內提供啟用類型。 點選快顯通知內文通常都會在前景啟動您的 App 來建立一致的使用者體驗，但您還是可以使用其他啟用類型來配合對使用者最有意義的特定案例。

您應一律設定 **Launch** 屬性，以便當使用者按下快顯通知主體並啟動您的應用程式時，您的應用程式知道應顯示哪些內容。

```csharp
// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = visual,
    Actions = actions,
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
 
    }.ToString()
};
 
// And create the toast notification
var toast = new ToastNotification(toastContent.GetXml());
```


## <a name="set-an-expiration-time"></a>設定到期時間

在 Windows 10 中，所有快顯通知都會在使用者關閉或忽略之後進入控制中心，因此使用者可以在快顯視窗消失後查看您的通知。

不過，如果通知中的訊息僅與一段時間有關，您應該在快顯通知上設定到期時間，這樣才不會讓使用者在您的 App 中看到過時的資訊。 例如，如果促銷期限為 12 小時，可將到期時間設為 12 個小時。 在下面程式碼中，我們將到期時間設定為 2 天。

> [!NOTE]
> 本機快顯通知的預設及最長到期時間為 3 天。

```csharp
toast.ExpirationTime = DateTime.Now.AddDays(2);
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果想以程式設計方式移除或取代您傳送的通知，您必須使用 Tag 屬性 (並選擇性使用 Group 屬性) 提供通知的主索引鍵。 那麼，日後就可以使用這個主索引鍵來移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx)。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "wallPosts";
```


## <a name="send-the-notification"></a>傳送通知

初始化快顯通知之後，只要建立 [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier)，再呼叫 Show()，將您的快顯通知傳入即可。

```csharp
ToastNotificationManager.CreateToastNotifier().Show(toast);
```


## <a name="clear-your-notifications"></a>清除您的通知

UWP app 會負責移除並清除其本身的通知。 啟動您的 App 時，我們並不會自動清除您的通知。

Windows 只有在使用者明確按一下通知時，才會自動移除通知。

以下是一個範例，示範訊息中心 App 應該會做些什麼...

1. 使用者在交談中收到多個關於新訊息的快顯通知
2. 使用者點選其中一個的快顯通知來開啟交談
3. App 開啟交談，然後清除該交談的所有快顯通知 (方法是針對該交談在 App 提供的群組上使用 [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_))
4. 使用者的控制中心現在會正確反映通知狀態，因為沒有該交談的任何過時通知留存在控制中心上。

若要了解清除所有通知，或移除特定通知，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/dn631260.aspx)。


## <a name="handling-activation"></a>處理啟用

在 Windows 10 中，當使用者按一下您的快顯通知時，有兩種方式可以讓快顯通知啟用您的 App...

* 前景啟用
* 背景啟用

> [!NOTE]
> 注意：如果您使用 Windows 8.1 中的舊版快顯通知範本，將會改為呼叫 **OnLaunched**，而非 **OnActivated**。 下列文件僅適用於採用 Notifications 程式庫 (如果使用原始 XML，則為 ToastGeneric 範本) 的現代化 Windows 10 通知。


### <a name="handling-foreground-activation"></a>處理前景啟用

在 Windows 10 中，當使用者按一下現代化快顯通知 (或快顯通知上的按鈕) 時，會透過新的啟用類型 (**ToastNotification**) 叫用 **OnActivated** 而不是叫用 **OnLaunched**。 因此，開發人員可以輕鬆地區分快顯通知啟用並相應執行工作。

以下所示範例中，您可以擷取您起初在快顯通知中提供的引數字串。 您也可以擷取使用者在文字方塊及選取方塊中提供的輸入。

> [!IMPORTANT]
> 就像您的 **OnLaunched** 程式碼一樣，您必須初始化畫面並啟用視窗。 **如果使用者按一下您的快顯通知，並不會呼叫 OnLaunched**，即使您的 App 已關閉，然後正在首次啟動，也不會。 我們經常建議將 **OnLaunched** 和 **OnActivated** 結合成 `OnLaunchedOrActivated` 方法，因為兩者都需要進行相同的初始設定。

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        var toastActivationArgs = e as ToastNotificationActivatedEventArgs;
                 
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


## <a name="handling-background-activation"></a>處理背景啟用

當您在快顯通知 (或快顯通知內的按鈕) 上指定背景啟用時，將會執行背景工作，而不是啟用前景 App。

如需背景工作的詳細資訊，請參閱[使用背景工作支援 App](/launch-resume/support-your-app-with-background-tasks.md)。

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


然後在 App.xaml.cs 中覆寫 OnBackgroundActivated 方法，與前景啟用相似，您可以擷取預先定義引數及使用者輸入。

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



## <a name="plain-vanilla-code-snippets"></a>簡單易懂的「Vanilla」程式碼片段

如果您無法使用 NuGet 提供的 Notifications 程式庫，則可以手動建構 XML (如下所示) 來建立 [ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)。

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

* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-sending-local-toast)
* [快顯通知內容文件](adaptive-interactive-toasts.md)
* [ToastNotification 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 類別](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
