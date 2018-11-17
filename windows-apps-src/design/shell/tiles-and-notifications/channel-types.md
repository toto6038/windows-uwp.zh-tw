---
author: adwilso
Description: Windows Push Notification Services (WNS) enables third-party developers to send toast, tile, badge, and raw updates from their own cloud service. There are many ways to send the notifications depending on the needs of your application
title: 選擇正確的推播通知通道類型
ms.author: mijacobs
ms.date: 07/07/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db2cf1c732669a2ae45b8a5eb427e8864446c800
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2018
ms.locfileid: "7161603"
---
# <a name="choosing-the-right-push-notification-channel-type"></a>選擇正確的推播通知通道類型

本文說明三種類型的 UWP 推播通知通道 (主要、次要及替代)，這些通道可幫助您將內容傳遞到應用程式。 

（如需如何建立推播通知的詳細資訊，請參閱[Windows 推播通知服務 (WNS) 概觀](../tiles-and-notifications/windows-push-notification-services--wns--overview.md)）。 

## <a name="types-of-push-channels"></a>推播通道的類型 

有三種類型的推播通道可用於將通知推送到 UWP 應用程式。 這些類型包括： 

[主要通道](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - 「傳統」推播通道。 可由市集中的任何應用程式用於傳送快顯通知、磚、原始通知或徽章通知 (快顯通知/磚/徽章描述的連結)

[次要磚通道](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - 用於推播次要磚的更新。 僅能用於將磚或徽章通知傳送至釘選到使用者開始畫面上的次要磚

[替代通道](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanagerforuser#Methods_) - Creators Update 中新增的一種通道。 它允許將原始通知傳送到任何 UWP 應用程式，包括未在市集中註冊的通知。 

> [!NOTE]
> 無論您使用何種推播通道，只要您的應用程式在裝置上執行，便一律能夠傳送本機快顯通知、磚或徽章通知。 它可以從前景應用程式處理程序或從背景工作傳送本機通知。 


## <a name="primary-channels"></a>主要通道

這些是 Windows 上目前最常用的通道，幾乎適用於任何要透過 Microsoft Store 散發應用程式的案例。 它們允許您將所有類型的通知傳送到應用程式。 

### <a name="what-do-primary-channels-enable"></a>主要通道會啟用哪些項目？

-   **將磚或徽章更新傳送至主要磚。** 如果使用者選擇將您的磚釘選在 [開始] 畫面上，這是您展示的機會。 傳送具有實用資訊的更新，或您應用程式內體驗的提醒。 
-   **傳送快顯通知。** 快顯通知讓您有機會立即在使用者面前取得一些資訊。 這類通知是由命令介面繪製在大多數應用程式的頂端，並存留在控制中心中，讓使用者能夠返回並在稍後與這類通知互動。 
-   **傳送原始通知以觸發背景工作。** 有時候您會想要根據通知代表使用者進行一些工作。 原始通知可讓應用程式的背景工作執行 
-   **由 Windows 使用 TLS 提供中的傳輸中訊息加密。** 當透過連線進入 WNS 與前往使用者裝置時加密訊息。  

### <a name="limitations-of-primary-channels"></a>主要通道的限制

-   需要使用 WNS REST API 推送通知，這不是標準的跨裝置廠商。 
-   只能為每個應用程式建立一個通道 
-   您的應用程式必須在 Microsoft Store 中註冊

### <a name="creating-a-primary-channel"></a>正在建立主要通道 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
```

## <a name="secondary-tile-channels"></a>次要磚通道

這些通道可用於將磚與徽章更新推送到次要磚。 應用程式可將它們用於通知使用者有一些他們可在應用程式中互動的有趣動作或資訊，例如在群組聊天中的新訊息及最新的運動分數。 

### <a name="what-do-secondary-tile-channels-enable"></a>次要磚通道會啟用哪些項目？

-   將磚或徽章通知傳送到次要磚。 次要磚是將使用者提取回您應用程式的好方法。 這些是使用者所關注資訊的深度連結，將相關資訊放置在磚上可幫助一再將使用者帶回。
-   在各種磚之間分開通道 (並到期)。 這可讓您在使用者可釘選在其開始畫面上的各種類型次要磚之間將後端的邏輯分開。 
-   由 Windows 使用 TLS 提供中的傳輸中訊息加密。 當透過連線進入 WNS 與前往使用者裝置時加密訊息。  

### <a name="limitations-of-secondary-tile-channels"></a>次要磚通道的限制

-   無允許的快顯通知或原始通知。 系統會忽略傳送給次要磚的快顯通知或原始通知。
-   您的應用程式必須在 Microsoft Store 中註冊


### <a name="creating-a-secondary-tile-channel"></a>建立次要磚通道 

```csharp
PushNotificationChannel channel = 
    await PushNotificationChannelManager.CreatePushNotificationChannelForSecondaryTileAsync(tileId);
```

## <a name="alternate-channels"></a>替代通道

替代通道可讓應用程式無須向 Microsoft Store 註冊或建立應用程式所使用主要通道之外的推播通道，即可傳送推播通知。 
 
### <a name="what-do-alternate-channels-enable"></a>替代通道會啟用哪些項目？
-   將推播通知傳送到在任何 Windows 裝置上執行的 UWP。 替代通道僅允許原始通知。
-   允許應用程式為應用程式內的不同功能建立多個原始推送通道。 應用程式可以建立最多 1000 個替代通道，每個通道的有效期間為 30 天。 這些通道每一個都能由應用程式分開管理或撤銷。
-   無須向 Microsoft Store 註冊應用程式，便可建立替代推播通道。 如果您的應用程式即將安裝在裝置上，但未將它向 Microsoft Store 註冊，則該應用程式仍能夠接收推播通知。
-   伺服器可使用 W3C 標準 REST API 與 VAPID 通訊協定推播通知。 替代通道會使用 W3C 標準通訊協定，這可讓您簡化必須維護的伺服器邏輯。
-   完整、端對端、訊息加密。 當主要通道一邊傳輸，一邊提供加密時，如果您想要有額外的安全性，則替代通道可讓應用程式傳遞加密標頭以保護訊息。 

### <a name="limitations-of-alternate-channels"></a>替代通道的限制
-   應用程式無法傳送快顯通知、磚、或徽章類型通知。 替代通道會限制您傳送其他通知類型的能力。 您的應用程式仍能從您的背景工作傳送本機通知。 
-   需要與主要和次要磚通道不同的 REST API。 使用標準 W3C REST API，表示您的應用程式需要有不同的邏輯，才能傳送推播快顯通知或磚更新

### <a name="creating-an-alternate-channel"></a>建立替代通道 

```csharp
PushNotificationChannel webChannel = 
    await PushNotificationChannelManager.Current.CreateRawPushNotificationChannelWithAlternateKeyForApplicationAsync(applicationServerKey, appChannelId);
```

## <a name="channel-type-comparison"></a>通道類型比較
以下是不同類型通道之間的快速比較：

<table>

<tr class="header">
<th align="left"><b>類型</b></th>
<th align="left"><b>推播快顯通知？</b></th>
<th align="left"><b>推播磚/徽章？</b></th>
<th align="left"><b>推播原始通知？</b></th>
<th align="left"><b>驗證</b></th>
<th align="left"><b>API</b></th>
<th align="left"><b>需要市集註冊？</b></th>
<th align="left"><b>通道</b></th>
<th align="left"><b>加密</b></th>
</tr>


<tr class="odd">
<td align="left">主要</td>
<td align="left">是</td>
<td align="left">是 - 僅主要磚</td>
<td align="left">是</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">是</td>
<td align="left">每個應用程式一個</td>
<td align="left">傳輸中</td>
</tr>
<tr class="even">
<td align="left">次要磚</td>
<td align="left">否</td>
<td align="left">是 - 僅次要磚</td>
<td align="left">否</td>
<td align="left">OAuth</td>
<td align="left">WNS REST API</td>
<td align="left">是</td>
<td align="left">每個次要磚一個</td>
<td align="left">傳輸中</td>
</tr>
<tr class="odd">
<td align="left">替代</td>
<td align="left">否</td>
<td align="left">否</td>
<td align="left">是</td>
<td align="left">VAPID</td>
<td align="left">WebPush W3C 標準</td>
<td align="left">否</td>
<td align="left">每個應用程式 1,000 佪</td>
<td align="left">標頭傳遞時可採用傳輸中 + 端對端加密 (需要應用程式程式碼)</td>
</tr>
</table>

## <a name="choosing-the-right-channel"></a>選擇正確通道

一般而言，我們建議在您的應用程式中使用主要通道，但有一些例外： 

1. 如果您將磚更新推播至次要磚，請使用次要磚推播通道。
2. 如果您正將通道傳出至其他服務 (例如瀏覽器)，請使用替代通道。
3. 如果您正建立一個將不會列在 Windows 市集中的應用程式 (例如 LOB 應用程式)，請使用替代通道。
4. 如果您的伺服器上有您想要重複使用的現有 Web 推播程式碼，或您必須在後端服務中有多個通道，請使用替代通道。

## <a name="related-articles"></a>相關文章

* [傳送本機磚通知](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [調適型和互動式快顯通知](../tiles-and-notifications/adaptive-interactive-toasts.md)
* [快速入門：傳送推播通知](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)
* [如何透過推播通知更新徽章](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [如何要求、建立以及儲存通知通道](https://msdn.microsoft.com/library/windows/apps/hh465412)
* [如何攔截執行應用程式的通知](https://msdn.microsoft.com/library/windows/apps/hh465450)
* [如何使用 Windows 推播通知服務 (WNS) 進行驗證](https://msdn.microsoft.com/library/windows/apps/hh465407)
* [推播通知服務要求和回應標頭](https://msdn.microsoft.com/library/windows/apps/hh465435)
* [推播通知的指導方針和檢查清單](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [原始通知](raw-notification-overview.md)
