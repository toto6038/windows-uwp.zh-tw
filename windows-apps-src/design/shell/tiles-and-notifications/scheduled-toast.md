---
description: 瞭解如何排程稍後會顯示本機快顯通知。
title: 排程快顯通知
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10、uwp、排程的快顯通知、scheduledtoastnotification、操作說明、快速入門、使用者入門、程式碼範例、逐步解說
ms.localizationpriority: medium
ms.openlocfilehash: 2a138458634f0246d7e6bed9d6d65c2479dac3c9
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030691"
---
# <a name="schedule-a-toast-notification"></a>排程快顯通知

排定的快顯通知可讓您排程稍後將出現的通知，而不論您的應用程式是否正在執行。 這在顯示提醒或其他使用者後續工作的案例中很有用，因為通知的時間和內容事先是已知的。

請注意，排程的快顯通知有5分鐘的傳遞視窗。 如果電腦在排定的傳遞時間內關閉，且維持超過5分鐘的時間，則會「捨棄」通知，告知使用者不再相關。 如果您需要保證傳遞通知，而不是電腦的關機時間長度，建議您使用具有時間觸發程式的背景工作，如下列程式 [代碼範例](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)所示。

> [!IMPORTANT]
> 桌面應用程式 (MSIX/sparse 套件和傳統桌面) 都有稍微不同的步驟來傳送通知和處理啟用。 請依照下列指示操作，但將取代為 `ToastNotificationManager` `DesktopNotificationManagerCompat` [桌面應用程式](toast-desktop-apps.md) 檔中的類別。

> **重要 api** ： [ScheduledToastNotification 類別](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Prerequisites

要完全了解本主題，下列項目將有所幫助...

* 快顯通知詞彙與概念的作業知識。 如需詳細資訊，請參閱[快顯通知與控制中心概觀](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10)。
* 熟悉 Windows 10 快顯通知內容。 如需詳細資訊，請參閱[快顯通知內容文件](adaptive-interactive-toasts.md)。
* Windows 10 UWP app 專案


## <a name="step-1-install-nuget-package"></a>步驟1：安裝 NuGet 套件

安裝 node.js. [ [通知] NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。 我們的程式碼範例會使用此套件。 在本文結尾，我們將提供不使用任何 NuGet 套件的「一般」程式碼片段。 此套件可讓您在不使用 XML 的情況下建立快顯通知。


## <a name="step-2-add-namespace-declarations"></a>步驟2：加入命名空間宣告

`Windows.UI.Notifications` 包含快顯通知 Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-schedule-the-notification"></a>步驟3：排程通知

我們將使用以文字為基礎的簡單通知，提醒學生今天所關注的作業。 建立通知和排程！

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("itemsDueToday", ToastActivationType.Foreground)
    .AddText("ASTR 170B1")
    .AddText("You have 3 items due today!");
    .GetToastContent();

    
// Create the scheduled notification
var toast = new ScheduledToastNotification(content.GetXml(), DateTime.Now.AddSeconds(5));


// Add your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="provide-a-primary-key-for-your-toast"></a>提供快顯通知主索引鍵

如果您想要以程式設計方式取消、移除或取代已排程的通知，您必須使用 [標記] 屬性 (並選擇性地使用 [群組] 屬性) ，以提供通知的主要金鑰。 然後，您可以在未來使用此主要金鑰來取消、移除或取代通知。

若要查看更多有關取代/移除已傳送快顯通知的詳細資料，請參閱[快速入門： 管理控制中心的快顯通知 (XAML)](/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 與 Group 結合可以做為主複合索引鍵。 Group 是較通用的識別碼，其中可以指定像是 "wallPosts"、"messages"、"friendRequests" 等群組。然而，Tag 則必須要在群組中唯一辨識通知本身。 然後可以使用一般群組，透過 [RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 移除該群組中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
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
