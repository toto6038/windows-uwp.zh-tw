---
Description: 瞭解如何排程本機快顯通知，稍後再出現。
title: 排程快顯通知
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10，uwp，排程的快顯通知，scheduledtoastnotification，how to，快速入門，開始使用，程式碼範例，逐步解說
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386427"
---
# <a name="schedule-a-toast-notification"></a>排程快顯通知

排定的快顯通知可讓您將通知排程為稍後顯示，不論您的應用程式當時是否正在執行。 這適用于針對使用者顯示提醒或其他後續工作的案例，其中通知的時間和內容是預先知道的。

請注意，排定的快顯通知會有5分鐘的傳遞視窗。 如果電腦在排定的傳遞時間期間關閉，且保持關閉超過5分鐘，則通知將會「捨棄」，而不再與使用者相關。 如果您需要保證傳遞通知，而不論電腦關閉的時間長度，我們建議您使用具有時間觸發程式的背景工作，如[下列程式碼範例](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)所示。

> [!IMPORTANT]
> 桌面應用程式（MSIX/sparse 封裝和傳統 Win32）在傳送通知和處理啟用方面有些許不同的步驟。 請依照下列指示進行，但使用[桌面應用程式](toast-desktop-apps.md)檔中的 `DesktopNotificationManagerCompat` 類別取代 `ToastNotificationManager`。

> **重要 api**： [ScheduledToastNotification 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>必要條件

要完全了解本主題，下列項目將有所幫助...

* 快顯通知詞彙與概念的作業知識。 如需詳細資訊，請參閱快顯 [和操作中心總覽](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)。
* 熟悉 Windows 10 快顯通知內容。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。
* Windows 10 UWP app 專案


## <a name="install-nuget-packages"></a>安裝 NuGet 套件

建議您將下列兩個 NuGet 套件安裝到您的專案。 我們程式碼範例將會使用這些套件。

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)：透過物件而不透過原始 XML 來產生快顯通知承載。
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/)：使用 C# 產生和剖析查詢字串


## <a name="add-namespace-declarations"></a>加入命名空間宣告

`Windows.UI.Notifications` 包含快顯 Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>建立快顯通知內容

在 Windows 10 中，您的快顯通知內容是使用賦予通知外觀極大彈性的調適性語言進行描述。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。

感謝您的通知程式庫，產生 XML 內容相當簡單。 如果您未安裝 NuGet 提供的 Notifications 程式庫，就必須手動建構 XML，很有可能會發生錯誤。

您應一律設定 **Launch** 屬性，以便當使用者按下快顯通知主體並啟動您的應用程式時，您的應用程式知道應顯示哪些內容。

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
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
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>建立排定的快顯通知

一旦您初始化快顯通知內容，請建立新的[ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) ，並傳入內容的 XML 和您想要傳遞通知的時間。

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果您想要以程式設計方式取消、移除或取代已排程的通知，您必須使用 Tag 屬性（並選擇性地選擇群組屬性），以提供通知的主要金鑰。 然後，您可以在未來使用此主要金鑰來取消、移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>排程通知

最後，建立[ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier)並呼叫 AddToSchedule （），並傳入您排定的快顯通知。

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>取消已排程的通知

若要取消已排程的通知，您必須先取出所有已排程通知的清單。

然後，尋找符合您稍早指定之標記（和選擇性群組）的排程快顯，並呼叫 RemoveFromSchedule （）。

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>啟用處理

請參閱[傳送本機](send-local-toast.md)快顯通知檔，以深入瞭解如何處理啟用。 排定的快顯通知的啟用方式，與啟用本機快顯通知的處理相同。


## <a name="adding-actions-inputs-and-more"></a>新增動作、輸入等等

如需深入瞭解 [動作] 和 [輸入] 等 advanced 主題，請參閱[傳送本機](send-local-toast.md)快顯通知檔。 動作和輸入在本機快顯通知中的執行方式與在排程的快顯通知中相同。


## <a name="resources"></a>資源

* [快顯內容檔](adaptive-interactive-toasts.md)
* [ScheduledToastNotification 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
