---
本文涵蓋四個通知選項&\#8212;本機、排程、定期和推播&\#8212;，它們會傳遞磚和徽章更新及快顯通知內容。
選擇通知傳遞方法
ms.assetid: FDB43EDE-C5F2-493F-952C-55401EC5172B
選擇通知傳遞方法
template: detail.hbs
---

# 選擇通知傳遞方法


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文涵蓋四個通知選項：本機、排程、定期和推播，它們會傳遞磚和徽章更新及快顯通知內容。 即使使用者沒有與應用程式直接互動，磚或快顯通知仍然可以將資訊傳遞給使用者。 應用程式的性質及內容與您想傳遞的資訊，可以協助您判斷哪種通知方法最適合您的案例。

## <span id="Notification_delivery_methods__overview"> </span> <span id="notification_delivery_methods__overview"> </span> <span id="NOTIFICATION_DELIVERY_METHODS__OVERVIEW"> </span>通知傳遞方法概觀


應用程式可以使用四種機制來傳遞通知：

-   **本機**
-   **排程**
-   **定期**
-   **推播**

下表摘要說明通知傳遞類型。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">傳遞方法</th>
<th align="left">搭配使用項目</th>
<th align="left">說明</th>
<th align="left">範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">本機</td>
<td align="left">磚、徽章、快顯通知</td>
<td align="left">在應用程式執行時傳送通知的一組 API 呼叫，會直接更新磚或徽章，或傳送快顯通知。</td>
<td align="left"><ul>
<li>音樂應用程式會更新磚以顯示「現正播放」的內容。</li>
<li>遊戲應用程式在使用者離開遊戲時，會以使用者的最高分數來更新磚。</li>
<li>啟動 app 時，便會清除其字符指出 app 有新資訊的徽章。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">排程</td>
<td align="left">磚、快顯通知</td>
<td align="left">預先排程通知以在您指定的時間更新的一組 API 呼叫。</td>
<td align="left"><ul>
<li>行事曆應用程式會為即將發生的會議設定快顯通知提醒。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">定期</td>
<td align="left">磚、徽章</td>
<td align="left">透過輪詢雲端服務來取得新內容，並以固定時間間隔來定期更新磚與徽章的通知。</td>
<td align="left"><ul>
<li>氣象應用程式每 30 分鐘會更新一次顯示氣象預測的磚。</li>
<li>「每日特賣」網站會在每天早上更新當日的特價商品。</li>
<li>顯示事件倒數天數的磚會在每日午夜更新顯示的倒數天數。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">推播</td>
<td align="left">磚、徽章、快顯通知、原始通知</td>
<td align="left">即使您的應用程式未執行，仍然會從雲端伺服器傳送的通知。</td>
<td align="left"><ul>
<li>購物應用程式會傳送快顯通知，讓使用者知道他們所追蹤物品的拍賣活動。</li>
<li>新聞應用程式會在發生突發新聞時更新磚。</li>
<li>運動應用程式會隨時以即時賽況來更新磚。</li>
<li>通訊應用程式會提供關於內送郵件或電話的警示。</li>
</ul></td>
</tr>
</tbody>
</table>

 

## <span id="Local_notifications"> </span> <span id="local_notifications"> </span> <span id="LOCAL_NOTIFICATIONS"> </span>本機通知


在應用程式執行時更新應用程式磚或徽章，或是產生快顯通知，是最簡單的通知傳遞機制；它只需要本機 API 呼叫。 每個應用程式都可以在磚上顯示實用或有趣的資訊，即使該內容只會在使用者啟動並與應用程式互動時才變更。 即使您也使用了其他的通知機制，本機通知還是讓應用程式磚保持最新狀態的好辦法。 例如，相片應用程式磚可以顯示最近新增相簿中的相片。

我們建議第一次啟動您的應用程式時在本機更新它的磚，或者至少在使用者進行變更之後立即更新磚 (您的應用程式正常會在磚做出的反映)。 該更新要等到使用者離開應用程式才能顯示，但在應用程式使用期間進行此變更，可確保使用者離開時磚已經更新為最新資訊。

當 API 呼叫是本機呼叫時，通知可以參考網頁影像。 如果網路影像無法下載、已損毀或不符合影像規格，磚與快顯通知的應對方式是不同的：

-   磚：不會顯示更新
-   快顯通知：會顯示通知，但是包含預留位置影像

本機通知不會過期，但是最佳做法是設定明確的到期時間。

如需詳細資訊，請參閱這些主題：

-   [傳送本機磚通知](tiles-and-notifications-sending-a-local-tile-notification.md)
-   [通用 Windows 平台 (UWP) 通知程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Scheduled_notifications"> </span> <span id="scheduled_notifications"> </span> <span id="SCHEDULED_NOTIFICATIONS"> </span>排程通知


排程通知是本機通知的子集，它們可以指定應該更新磚或顯示快顯通知的確切時間。 排程通知適合事先已知道要更新之內容 (像是會議邀請) 的情況。 如果您事先並不清楚通知內容，那就應該使用推播或定期通知。

根據預設，排程的通知會在傳遞的三天後到期。 如有需要，您可以指定明確的到期時間來覆寫這個預設設定。

如需詳細資訊，請參閱這些主題：

-   [排程通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh761464)
-   [通用 Windows 平台 (UWP) 通知程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Periodic_notifications"> </span> <span id="periodic_notifications"> </span> <span id="PERIODIC_NOTIFICATIONS"> </span>定期通知


定期通知可以讓您使用最少的雲端服務與用戶端投資設備來提供動態磚更新。 它們也是將相同內容散佈給廣大群眾的絕佳方式。 您的用戶端程式碼可以指定雲端位置 URL，好讓 Windows 輪詢以取得磚或徽章的更新，以及指定輪詢該位置的頻率。 Windows 會在每個輪詢間隔期間連線 URL，下載指定的 XML 內容並在磚上顯示內容。

應用程式必須裝載雲端服務才能使用定期通知，而所有已安裝應用程式的使用者將以指定的間隔輪詢這項服務。 請注意，定期更新無法用於快顯通知；快顯通知適合排程或推播通知。

根據預設，定期通知會在輪詢發生的三天後到期。 如有需要，您可以指定明確的到期時間來覆寫這個預設設定。

如需詳細資訊，請參閱這些主題：

-   [定期通知概觀](tiles-and-notifications-periodic-notification-overview.md)
-   [通用 Windows 平台 (UWP) 通知程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

## <span id="Push_notifications"> </span> <span id="push_notifications"> </span> <span id="PUSH_NOTIFICATIONS"> </span>推播通知


若要交流即時資料或針對使用者個人化的資料，就非常適合使用推播通知。 推播通知用於在無法預測的時間產生的內容，像是突發新聞、社交網路更新或立即訊息。 當資料 (像是遊戲進行時的遊戲分數) 有時效性而不適合定期通知時，也可以使用推播通知。

推播通知需要能夠管理推播通知通道與選擇何時將通知傳送給何人的雲端服務。

根據預設，推播通知會在 Windows 推播通知服務 (WNS) 收到它們的三天後到期。 如有需要，您可以指定明確的到期時間來覆寫這個預設設定。

如需詳細資訊，請參閱：

-   [Windows 推播通知服務 (WNS) 概觀](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
-   [推播通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh761462)
-   [通用 Windows 平台 (UWP) 通知程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)

**注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## <span id="related_topics"> </span>相關主題


* [傳送本機磚通知](tiles-and-notifications-sending-a-local-tile-notification.md)
* [推播通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh761462)
* [排程通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh761464)
* [快顯通知的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465391)
* [定期通知概觀](tiles-and-notifications-periodic-notification-overview.md)
* [Windows 推播通知服務 (WNS) 概觀](tiles-and-notifications-windows-push-notification-services--wns--overview.md)
* [GitHub 上的通用 Windows 平台 (UWP) 通知程式碼範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)
 

 






<!--HONumber=Mar16_HO1-->


