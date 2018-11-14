---
author: mijacobs
Description: Periodic notifications, which are also called polled notifications, update tiles and badges at a fixed interval by downloading content from a cloud service.
title: 定期通知概觀
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d02bfb8b8bd112a969895d4f2bd5d324fce9d6b8
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "6450253"
---
# <a name="periodic-notification-overview"></a>定期通知概觀
 


定期通知也稱為輪詢通知，可以在固定的時間間隔從雲端服務下載內容來更新磚和徽章。 若要使用定期通知，您的用戶端應用程式需要提供兩個資訊：

-   Windows 為應用程式輪詢磚或徽章更新的統一資源識別元 (URI) 網路位置
-   輪詢 URI 的頻率

定期通知可以讓您使用最少的雲端服務與用戶端投資設備來提供動態磚更新。 定期通知是將相同內容散佈給廣大群眾的理想方式。

**注意：** 您可以了解更多的 Windows8.1 下載的[推播和定期通知範例](http://go.microsoft.com/fwlink/p/?linkid=231476)，並重複使用其原始程式碼，在 windows 10 應用程式中。

 

## <a name="how-it-works"></a>運作方式


定期通知要求應用程式要裝載雲端服務。 安裝應用程式的所有使用者將會定期輪詢服務。 在每個輪詢間隔 (如每小時一次)，Windows 會傳送一個 HTTP GET 要求到 URI，下載回應要求時提供的要求磚或徽章內容 (使用 XML 格式)，然後在應用程式磚上顯示內容。

請注意，定期更新不可以與快顯通知搭配使用。 最適合提供快顯通知的方式是透過[排程](https://msdn.microsoft.com/library/windows/apps/hh465417)或[推播](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252)通知。

## <a name="uri-location-and-xml-content"></a>URI 位置和 XML 內容


任何有效的 HTTP 或 HTTPS 網址都可以做為 URI 進行輪詢。

雲端伺服器的回應包括下載的內容。 從 URI 傳回的內容必須符合[磚](adaptive-tiles-schema.md)或[徽章](https://msdn.microsoft.com/library/windows/apps/br212851) XML 結構描述規格，而且必須使用 UTF-8 編碼。 您可以使用已定義的 HTTP 標頭來指定通知的[到期時間](#expiration-of-tile-and-badge-notifications)或標記。

## <a name="polling-behavior"></a>輪詢行為


呼叫這些方法的其中一個開始輪詢：

-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (磚)
-   [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (徽章)
-   [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (磚)

呼叫其中一個方法時，會立即輪詢 URI，並使用收到的內容來更新磚或徽章。 在這次初始輪詢之後，Windows 會在固定的時間間隔繼續提供更新。 輪詢會一直持續，直到您明確加以停止 (透過 [**TileUpdater.StopPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate))、解除安裝 app 或移除磚 (次要磚) 後才會停止。 否則，即使不再啟動您的 app，Windows 還是會繼續輪詢磚或徽章的更新。

### <a name="the-recurrence-interval"></a>週期間隔

您可以指定週期間隔作為上述方法的參數。 請注意，雖然 Windows 盡力依要求來輪詢，但是這個間隔並不是很精確。 要求的輪詢間隔會因外界因素最多延遲 15 分鐘，由 Windows 判定。

### <a name="the-start-time"></a>開始時間

您可以選擇指定每日的特定時間來開始輪詢。 請考慮一天僅變更磚內容一次的應用程式。 在這種情況中，建議將輪詢時間設為接近雲端服務更新的時間。 例如，如果每日購物網站會在早上 8 點發佈當日特惠資訊，那麼可以在早上 8 點後不久就輪詢新的磚內容。

如果提供開始時間，則對方法的第一次呼叫就會立即輪詢內容。 接下來，提供的開始時間的 15 分鐘內就會開始一般輪詢。

### <a name="automatic-retry-behavior"></a>自動重試行為

只有裝置連線時才能輪詢 URI。 如果網路可用但是因為任何原因而無法連絡 URI，則會略過這次輪詢間隔，然後在下次間隔時間再次輪詢 URI。 如果達到輪詢間隔時裝置正處於關機、睡眠或休眠狀態，則會在裝置從關機或睡眠狀態回復時輪詢 URI。

### <a name="handling-app-updates"></a>處理應用程式更新

若您發佈會變更您輪詢 URI 的應用程式更新，您應新增每日[時間觸發程序背景工作](../../../launch-resume/run-a-background-task-on-a-timer-.md)，讓其使用新的 URI 呼叫 StartPeriodicUpdate 以確保您的動態磚使用新的 URI。 否則，如果使用者收到您的應用程式更新，但不啟動您的應用程式，他們的動態磚將會持續使用舊的 URI，而若 URI 現為無效或是傳回的裝載參考本機影像已不存在，則舊的 URI 可能無法顯示。

## <a name="expiration-of-tile-and-badge-notifications"></a>磚和徽章通知的到期時間


根據預設，定期磚和徽章通知會在下載的三天後到期。 當通知到期的時候，會從徽章、磚或佇列中移除內容，不再對使用者顯示。 最佳做法是使用一個對您的 app 或通知有意義的時間，在所有的定期磚和徽章通知上設定明確的到期時間，以確保內容不超過內容的時效性。 明確的到期時間對於已定義存留時間的內容而言很重要。 如果您的雲端服務無法使用，或使用者在一段時間從網路中斷連線時，它也可以確保會移除過時內容。

您的雲端服務會在回應承載中包含 X-WNS-Expires HTTP 標頭，以設定通知的到期日期和時間。 X-WNS-Expires HTTP 標頭必須符合 [HTTP-date 格式](http://go.microsoft.com/fwlink/p/?linkid=253706)。 如需詳細資訊，請參閱 [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 或 [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_)。

例如，在股市交易日，您可以將股價更新到期時間設定為輪詢間隔時間的兩倍 (例如，如果是每半小時輪詢一次，則是接收後的一小時)。 另一個範例是新聞應用程式可能決定每日新聞磚更新的適當到期時間為一天。

## <a name="periodic-notifications-in-the-notification-queue"></a>通知佇列中的定期通知


您可以使用定期磚更新搭配[通知循環](https://msdn.microsoft.com/library/windows/apps/hh781199)。 根據預設，[開始] 畫面上的磚會顯示單一通知的內容，直到新通知取代目前的通知為止。 啟用循環後，佇列最多可以保留 5 個通知，而磚會循環顯示這些通知。

如果佇列達到 5 個通知的容量，下一個新通知將會取代佇列中最舊的通知。 不過，您可以在通知設定標記，影響佇列的取代原則。 標記是應用程式專用、無大小寫之分的字串，最多可有 16 個英數字元，在回應承載的 [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) HTTP 標頭中指定。 Windows 會將傳入通知上的標記與已經在佇列中所有通知的標記相比較。 如果找到相符的標記，新的通知會取代具有相同標記的佇列通知。 如果沒有找到相符的標記，會套用預設的取代規則，由新的通知取代佇列中最舊的通知。

您可以使用通知佇列和標記來實作各種不同的通知情形。 例如，股票應用程式可以傳送 5 個通知，每個通知各與不同的股票有關，並使用股票名稱做標記。 這樣可以避免佇列包含 2 則相同股票的通知，而且其中較舊的是已過期的通知。

如需詳細資訊，請參閱[使用通知佇列](https://msdn.microsoft.com/library/windows/apps/hh781199)。

### <a name="enabling-the-notification-queue"></a>啟用通知佇列

若要實作通知佇列，請先為您的磚啟用佇列 (請參閱[如何搭配本機通知使用通知佇列](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/01/05/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications/))。 在 app 存留期間只需要執行啟用佇列一次，但也可以在每次 app 啟動時進行呼叫。

### <a name="polling-for-more-than-one-notification-at-a-time"></a>一次輪詢多個通知

您必須針對 Windows 為磚下載的每個通知提供唯一 URI。 使用 [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) 方法，通知佇列中一次可提供最多 5 個 URI。 單一通知承載會在相同的時間或接近相同的時間輪詢每個 URI。 每個輪詢的 URI 可以傳回本身的到期時間與標記值。

## <a name="related-topics"></a>相關主題


* [定期通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [如何設定徽章的定期通知](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [如何設定磚的定期通知](https://msdn.microsoft.com/library/windows/apps/hh761476)
 
