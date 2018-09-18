---
author: andrewleader
Description: Learn how to use Universal Dismiss on your toast notifications.
title: 通用關閉
label: Universal Dismiss
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 快顯通知, 雲端的控制中心, 通用關閉, 通知, 跨裝置, 關閉一個就關閉全部
ms.localizationpriority: medium
ms.openlocfilehash: 90ad60949504d4478341ff9455fe0f7da90d78a9
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "4023602"
---
# <a name="universal-dismiss"></a>通用關閉

通用關閉由雲端的控制中心 (Action Center in the Cloud) 提供，這表示當您關閉一部裝置上的通知，其他裝置上的相同通知也會一起關閉。

> [!IMPORTANT]
> **需要年度更新版**：您的目標必須是 SDK 14393 並執行組建 14393 或更新版本，才能使用通用關閉。

本案例的常見範例是行事曆提醒... 您在兩部裝置上都裝有行事曆應用程式... 您在手機和桌上型電腦同時收到通知... 您按一下桌上型電腦的關閉... 因為有通用關閉，所以手機上的提醒也同時關閉了！ **啟用通用關閉只需要一行程式碼！**

<img alt="Diagram of Universal Dismiss" src="images/universal-dismiss.gif" width="406"/>

在此案例中，關鍵事實是**在多部裝置上安裝相同的應用程式**，這表示**每部裝置都會收到通知**。 行事曆應用程式是典型範例，因為您通常會同時在 Windows 電腦和手機上安裝相同的行事曆應用程式，而每個應用程式執行個體都會傳送提醒到每部裝置上。 透過新增支援通用關閉，那些相同提醒的執行個體可以跨裝置連結。


## <a name="how-to-enable-universal-dismiss"></a>如何啟用通用關閉

身為開發人員，啟用通用關閉非常簡單。 您只需要提供 ID，讓我們可以跨裝置連結每個通知，如此當使用者在一部裝置上關閉通知時，另一部裝置上的相對應連結通知也會關閉。

![通用關閉 RemoteId 圖表](images/universal-dismiss-remoteid.jpg)

> **RemoteId**：*跨裝置*唯一識別通知的識別碼。

只需一行程式碼即可新增 RemoteId，啟用支援通用關閉！ 如何產生您的 RemoteId 操之在您 - 然而，您必須先確認它可以跨裝置唯一識別通知，並可從在不同裝置上執行的不同應用程式執行個體產生相同的識別碼。

例如，在我的家庭作業規劃工具應用程式中，我透過指定「提醒」類型來產生 RemoteId，接著包含線上帳戶識別碼和家庭作業項目的線上識別碼。 無論是哪一個裝置傳送通知，我都能一致地產生完全相同的 RemoteId，因為這些線上識別碼在裝置間共用。

```csharp
var toast = new ScheduledToastNotification(content.GetXml(), startTime);
 
// If the RemoteId property is present
if (ApiInformation.IsPropertyPresent(typeof(ScheduledToastNotification).FullName, nameof(ScheduledToastNotification.RemoteId)))
{
    // Assign the RemoteId to add support for Universal Dismiss
    toast.RemoteId = $"reminder_{account.AccountId}_{homework.Identifier}"
}
  
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```

下列程式碼同時在我的手機與傳統型應用程式上執行，這表示兩部裝置上的通知將會有相同 RemoteId。

這就是您必須執行的作業！ 當使用者關閉 (或按下) 通知，我們會檢查是否有 RemoteId，如果有，我們就會關閉使用者所有裝置上的該 RemoteId。

**已知問題**：透過 `ToastNotificationHistory.GetHistory()` API 擷取 **RemoteId** 一律傳回空字串，而非您指定的 **RemoteId**。 請不要擔心，所有項目都能運作 - 它只是擷取已損壞的值。

> [!NOTE]
> 如果使用者或企業停用您應用程式的[通知鏡像](notification-mirroring.md) (或完全停用通知鏡像)，則通用關閉將無法運作，因為在雲端中沒有您的通知。


## <a name="supported-devices"></a>支援的裝置

從年度更新版開始，Windows Mobile 和Windows 電腦都支援通用關閉。 通用關閉可雙向運作：在電腦和電腦之間、電腦和手機之間，以及手機和手機之間。