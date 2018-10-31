---
author: manoskow
Description: Learn how to group notifications in Action Center using collections.
title: 快顯通知集合
label: Toast Collections
template: detail.hbs
ms.author: mijacobs
ms.date: 05/16/2018
ms.topic: article
keywords: Windows 10, uwp, 通知, 集合, 集合, 群組通知, 群組通知, 群組, 組織, 重要訊息中心, 快顯通知
ms.localizationpriority: medium
ms.openlocfilehash: be7c4ec2e9a47eeeb00663ae94f89e44c6751352
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5821822"
---
# <a name="grouping-toast-notifications-with-collections"></a>使用集合來群組快顯通知
使用集合來組織應用程式在重要訊息中心的快顯通知。 集合有助於使用者更容易在重要訊息中心找出資訊，並讓開發人員更好管理他們的通知。  下列 API 允許移除、建立和更新通知集合。

> [!IMPORTANT]
> **需要 Creators Update**：您必須以 SDK 15063 為目標並執行組建 15063 或更新版本，才能使用快顯通知集合。 相關 API 包含 [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection)，以及 [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager)

您可以參閱以下訊息中心應用程式的範例，根據聊天群組分隔通知；每一個標題（Comp Sci 160A 聊天專案、直接訊息、Lacrosse 小組聊天）皆為個別的集合。  請注意如何群組不同的通知就像來自個別的應用程式一樣，即使這些通知皆來自相同的應用程式。  如果您要尋找更精細的方式組織您的通知，請參閱 [快顯通知標頭](toast-headers.md)。  
![有兩個不同通知群組的集合範例](images/toast-collection-example.png)

## <a name="creating-collections"></a>建立集合
建立每個集合時，您需要提供顯示名稱與圖示，在重要訊息中心中顯示為的集合標題一部分，如上圖所示。 使用者點閱集合標題時，集合也需要啟動引數來協助應用程式在應用程式中對的位置瀏覽。  

### <a name="create-a-collection"></a>建立集合

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>將通知傳送到集合
我們將涵蓋從三個不同的快顯通知管線傳送通知：本機、排程，以及推播。  針對這每一個範例，我們會建立範例快顯通知，來立即傳送以下的程式碼，然後聚焦在如何透過每個管線將快顯通知新增至集合。

建構通知承載：

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>將快顯通知傳送到集合

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>將排程快顯通知新增至集合

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>將推播快顯通知傳送到集合
針對推播快顯通知，您需要將 X-WNS-CollectionId 標頭新增至 POST 訊息。
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>管理集合
#### <a name="create-the-toast-collection-manager"></a>建立快顯通知集合管理員
針對 [管理集合] 章節中其餘的程式碼片段，我們會使用下列 collectionManager。
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>取得所有集合

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>取得已建立的集合數目

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>移除集合

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>更新集合
您可以使用相同的 ID 建立新的集合，並儲存集合的新執行個體，來更新集合。
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>管理集合中的快顯通知
#### <a name="group-and-tag-properties"></a>群組與標籤屬性
群組和標籤屬性一起在集合中唯一識別一個通知。  群組（和標籤）做為識別您通知的複合主要金鑰（超過一個識別碼）。 例如，如果您想要移除或取代通知，您必須可以指定 *哪些通知* 您想要移除/取代；藉由指定標籤與群組來完成。 訊息中心應用程式的範例。  開發人員可以使用交談 ID 做為群組，以及訊息 ID 做為標籤。

#### <a name="remove-a-toast-from-a-collection"></a>從集合移除快顯通知
您可以使用標籤和群組 ID 來移除個別快顯通知，或在集合中清除所有快顯通知。
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>在集合中清除所有快顯通知
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>通知視覺化工具中的集合。
您可以使用 [通知視覺化工具](notifications-visualizer.md) 工具可協助您設計集合。 遵循下列步驟：

* 按一下右下角的齒輪圖示。 
* 選取 [快顯通知集合]。
* 上述快顯通知的預覽，有 [快顯通知集合] 下拉式選單。 選取管理集合。
* 按一下 [新增集合]，填寫集合的詳細資料，並儲存。
* 您可以新增多個集合，或按一下 [管理集合方塊] 回到主畫面的螢幕。
* 從 [快顯通知集合] 下拉式選單選取您想要新增到集合的快顯通知。
* 當您引發快顯通知時，會將它新增至重要訊息中心的適當集合中。


## <a name="other-details"></a>其他詳細資料
您所建立的快顯通知集合也會反映在使用者的通知設定中。  使用者可以切換適用於每個個別集合的設定，來開關這些子群組。  如果應用程式的最上層通知已關閉，也會關閉所有集合通知。  此外，預設下每個集合會在重要訊息中心裡顯示 3 個通知，且使用者可以展開以顯示最多 20 個通知。

## <a name="related-topics"></a>相關主題

* [快顯通知內容](adaptive-interactive-toasts.md)
* [快顯通知標頭](toast-headers.md)
* [GitHub 上的 Notifications 程式庫 (屬於 Windows 社群工具組)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)