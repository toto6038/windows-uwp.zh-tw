---
author: andrewleader
Description: Learn how to use a progress bar within your toast notification.
title: 快顯通知進度列和資料繫結
label: Toast progress bar and data binding
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 快顯通知, 進度列, 快顯通知進度列, 通知, 快顯通知資料繫結
ms.localizationpriority: medium
ms.openlocfilehash: b99c2479bef3c10ecc82707e475f49fd2b9014ec
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2018
ms.locfileid: "4537084"
---
# <a name="toast-progress-bar-and-data-binding"></a>快顯通知進度列和資料繫結

在快顯通知中使用進度列可以將長期執行的作業狀態傳達給使用者，例如下載、視訊呈現、練習目標等。

> [!IMPORTANT]
> **需要 Creators Update 和 Notifications 程式庫 1.4.0**：您的目標必須是 SDK 15063 並執行組建 15063 或更高版本，才能在快顯通知上使用進度列。 您必須使用版本 1.4.0 或更高版本的 [UWP Community Toolkit Notifications NuGet 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以便在快顯通知內容中建構進度列。

快顯通知中的進度列可以是「不定的」(無特定值，由動畫點指出正在發生作業) 或「確定的」(填入列的特定百分比，例如 60%)。

> **重要 API**：[NotificationData 類別](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata)、[ToastNotifier.Update 方法](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update)、[ToastNotification 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> 僅桌上型電腦支援快顯通知中的進度列。 在其他裝置上，會從您的通知捨棄進度列。

下圖顯示確定的進度列，並標示所有對應的屬性。

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| 屬性 | 類型 | 必要 | 描述 |
|---|---|---|---|
| **Title** | string 或 [BindableString](toast-schema.md#bindablestring) | false | 取得或設定選用標題字串。 支援資料繫結。 |
| **Value** | double 或 [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) 或 [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | 取得或設定進度列的值。 支援資料繫結。 預設為 0。 可以是之間 0.0 和 1.0 之間的 double、`AdaptiveProgressBarValue.Indeterminate` 或 `new BindableProgressBarValue("myProgressValue")`。 |
| **ValueStringOverride** | string 或 [BindableString](toast-schema.md#bindablestring) | false | 取得或設定要顯示的選用字串，用於取代預設百分比字串。 如果未提供此項，將會顯示「70%」之類的內容。 |
| **Status** | string 或 [BindableString](toast-schema.md#bindablestring) | true | 取得或設定狀態字串 (必要)，它會顯示在左側進度列的下方。 這個字串應該反映作業的狀態，例如「正在下載...」或「正在安裝...」 |


以下是產生上方所見通知的方式...

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

不過，您需要動態更新進度列的值，進度列才能看起來是「活動的」。 做法是使用資料繫結來更新快顯通知。


## <a name="using-data-binding-to-update-a-toast"></a>使用資料繫結來更新快顯通知

使用資料繫結包含下列步驟...

1. 建構可使用資料繫結欄位的快顯通知內容
2. 指派 **Tag** (也可以是 **Group**) 到您的 **ToastNotification**
3. 定義您 **ToastNotification** 的初始 **Data** 值
4. 傳送快顯通知
5. 使用 **Tag** 和 **Group**，以新值更新 **Data** 的值

下列程式碼片段顯示步驟 1-4。 下一個程式碼片段顯示如何更新快顯通知的 **Data** 值。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

接著，當您想要變更 **Data** 值，請使用 [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) 方法來提供新的資料，而不需要重新建構整個快顯通知承載。

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

使用 **Update** 方法而非更換整個快顯通知，也可確保快顯通知停在控制中心的相同位置而不會向上或向下移動。 如果因為填入進度列而使得快顯通知每隔幾秒就跳到控制中心的頂端，會令使用者感到相當困惑！

**Update** 方法會傳回列舉 [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult)，這可讓您知道更新是否成功或是否找不到通知 (這表示使用者可能已關閉通知，因此您應該停止傳送更新)。 在進度作業完成 (例如下載完成) 前，不建議您彈出另一個快顯通知。


## <a name="elements-that-support-data-binding"></a>支援資料繫結的元素
快顯通知中的下列元素支援資料繫結

- **AdaptiveProgress** 上的所有屬性
- 最上層 **AdaptiveText** 元素上的 **Text** 屬性


## <a name="update-or-replace-a-notification"></a>更新或更換通知

從 Windows 10 開始，您一律可以藉由傳送具有相同 **Tag** 和 **Group** 的新快顯通知來**取代**通知。 那麼，**更換**快顯通知和**更新**快顯通知資料有何不同？

| | 更換 | 更新 |
| -- | -- | --
| **控制中心中的位置** | 通知會移到控制中心的頂端。 | 通知停留在控制中心的相同位置。 |
| **修改內容** | 完全可以變更快顯通知的所有內容/配置 | 只能變更支援資料繫結的屬性 (進度列和最上層的文字) |
| **以快顯視窗方式重新出現** | 若保留 **SuppressPopup** 設定為 `false` 可以快顯通知快顯方式重新出現 (或設定為 true，則以無訊息方式傳送至控制中心) | 不會以快顯視窗方式重新出現；快顯通知資料以無訊息方式在控制中心內更新 |
| **使用者關閉** | 無論使用者是否關閉您先前的通知，一律會傳送更換快顯通知 | 如果使用者關閉您的快顯通知，快顯通知更新將會失敗 |

一般而言，**更新適用於...**

- 在短時間內頻繁變更，且不需要吸引使用者注意的資訊
- 快顯通知內容的細微變更，例如從 50% 變更到 65%

很多時候，在完成更新系列之後 (例如檔案已下載)，建議您在最後一個步驟進行更換，因為...

- 最後通知可能有大幅配置變更，例如移除進度列、新增新的按鈕等
- 使用者可能已關閉擱置中的進度通知，因為他們不在意觀看下載過程，但仍想在作業完成時獲得快顯視窗的通知


## <a name="related-topics"></a>相關主題

- [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [快顯通知內容文件](adaptive-interactive-toasts.md)