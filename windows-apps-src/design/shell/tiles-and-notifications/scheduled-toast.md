---
Description: 瞭解如何排程稍後會顯示本機快顯通知。
title: 排程快顯通知
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10、uwp、排程的快顯通知、scheduledtoastnotification、操作說明、快速入門、使用者入門、程式碼範例、逐步解說
ms.localizationpriority: medium
ms.openlocfilehash: bc80cf04c1e1461612401ef4ced898058e2dd4ac
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172352"
---
# <a name="schedule-a-toast-notification"></a>排程快顯通知

排定的快顯通知可讓您排程稍後將出現的通知，而不論您的應用程式是否正在執行。 這在顯示提醒或其他使用者後續工作的案例中很有用，因為通知的時間和內容事先是已知的。

請注意，排程的快顯通知有5分鐘的傳遞視窗。 如果電腦在排定的傳遞時間內關閉，且維持超過5分鐘的時間，則會「捨棄」通知，告知使用者不再相關。 如果您需要保證傳遞通知，而不是電腦的關機時間長度，建議您使用具有時間觸發程式的背景工作，如下列程式 [代碼範例](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)所示。

> [!IMPORTANT]
> MSIX/sparse 封裝和傳統 Win32)  (的桌面應用程式，會有稍微不同的步驟來傳送通知和處理啟用。 請依照下列指示操作，但將取代為 `ToastNotificationManager` `DesktopNotificationManagerCompat` [桌面應用程式](toast-desktop-apps.md) 檔中的類別。

> **重要 api**： [ScheduledToastNotification 類別](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>先決條件

要完全了解本主題，下列項目將有所幫助...

* 快顯通知詞彙與概念的作業知識。 如需詳細資訊，請參閱快顯 [和控制中心總覽](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10)。
* 熟悉 Windows 10 快顯通知內容。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。
* Windows 10 UWP app 專案


## <a name="install-nuget-packages"></a>安裝 NuGet 套件

建議您將下列兩個 NuGet 套件安裝到您的專案。 我們程式碼範例將會使用這些套件。

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)：透過物件而不透過原始 XML 來產生快顯通知承載。
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/)：使用 C# 產生和剖析查詢字串


## <a name="add-namespace-declarations"></a>新增命名空間宣告

`Windows.UI.Notifications` 包含快顯通知 Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>建造快顯通知內容

在 Windows 10 中，您的快顯通知內容是使用賦予通知外觀極大彈性的調適性語言進行描述。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。

多虧了通知程式庫，產生 XML 內容很簡單。 如果您未安裝 NuGet 提供的 Notifications 程式庫，就必須手動建構 XML，很有可能會發生錯誤。

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

當您初始化快顯通知內容之後，請建立新的 [ScheduledToastNotification](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) 並傳入內容的 XML，以及您想要傳遞通知的時間。

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果您想要以程式設計方式取消、移除或取代已排程的通知，您必須使用 [標記] 屬性 (並選擇性地使用 [群組] 屬性) ，以提供通知的主要金鑰。 然後，您可以在未來使用此主要金鑰來取消、移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>排程通知

最後，建立 [ToastNotifier](/uwp/api/windows.ui.notifications.toastnotifier) 並呼叫 AddToSchedule ( # A1，並傳入排程的快顯通知。

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>取消已排程的通知

若要取消已排程的通知，您必須先取得所有排程通知的清單。

然後，尋找符合標記 (的排程快顯，並選擇性地分組您先前指定的) ，然後呼叫 RemoveFromSchedule ( # A3。

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

若要深入瞭解如何處理啟用，請參閱「 [傳送本機](send-local-toast.md) 快顯通知」檔。 啟用排定的快顯通知，其處理方式與啟用本機快顯通知的方式相同。


## <a name="adding-actions-inputs-and-more"></a>新增動作、輸入和其他

若要深入瞭解動作和輸入等 advanced 主題，請參閱「 [傳送本機](send-local-toast.md) 快顯通知」檔。 動作和輸入在本機快顯通知中的運作方式，與在排程快顯通知中的運作方式相同。


## <a name="resources"></a>資源

* [快顯通知內容文件](adaptive-interactive-toasts.md)
* [ScheduledToastNotification 類別](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)