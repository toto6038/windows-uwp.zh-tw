---
title: 呼叫 Xbox Live 的最佳作法
description: 深入了解呼叫 Xbox Live Api 的最佳作法。
ms.assetid: f4c7156b-7736-41e5-9b3d-e87cc8dd2531
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，最佳作法
ms.localizationpriority: medium
ms.openlocfilehash: 55e05ef7de2e2981f9f5af86623a8d8413ce2c99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613703"
---
# <a name="best-practices-for-calling-xbox-live"></a>呼叫 Xbox Live 的最佳作法

您可以從兩種主要方式呼叫的 Xbox Live 服務： 使用 Xbox 服務 API 」 (XSAPI)，或直接呼叫 REST 端點。 不論如何您的程式碼會呼叫 Xbox Live，務必要具有適當的呼叫模式，然後重試邏輯。

若要了解如何撰寫適當的重試邏輯，就必須了解 REST 端點-兩種**等冪**並**非等冪**。 我們將討論每一種下方
 
## <a name="non-idempotent-endpoints"></a>非等冪端點

HTTP 方法的副作用時重複呼叫會被視為**非等冪**。 這表示，如果用戶端所呼叫的端點，而且網路逾時，就會發生，並不安全重試方法，因為已更新資源，但網路無法通知呼叫端已順利完成。 錯誤，而不是重試時用戶端必須先查詢以查看呼叫是否成功。 只有當呼叫不成功時，才應該它並重試。

在 Xbox Services API 中，某些 Api 會在內部會標示為呼叫非冪等性端點。 這表示，如果呼叫這些端點時，就會發生失敗，Api 將不會自動重試的端點。

非等冪 Api 的完整清單如下：

* game\_server\_platform\_service::allocate\_cluster()
<br>
* game\_server\_platform\_service::allocate\_cluster\_inline()
<br>
* game\_server\_platform\_service::allocate\_session\_host()
<br>
* matchmaking\_service::create\_match\_ticket()
<br>
* 多人遊戲\_service::write\_session()
<br>
* multiplayer\_service::write\_session\_by\_handle()
<br>
* multiplayer\_service::send\_invites()
<br>
* reputation\_service::submit\_batch\_reputation\_feedback()
<br>
* reputation\_service::submit\_reputation\_feedback()
 

## <a name="idempotent-methods"></a>具有等冪性方法

**具有等冪性**另一方面的 HTTP 方法執行不會讓 「 副作用。 這又表示它們是安全地重試。 在 Xbox Services API 中，所有具有等冪性方法會自動重試在某些情況下。

具有等冪性 Api 的完整清單是不被視為非等冪上述的所有 Api。


## <a name="retry-logic-best-practices"></a>重試邏輯最佳作法

針對具有等冪性呼叫這些條件應該會自動重試：

* 所有的網路錯誤
* 401:未經授權
* 408:RequestTimeout
* 429:太多要求
* 500:InternalError
* 502:BadGateway
* 503:ServiceUnavailable
* 504:GatewayTimeout


在 UWP、 401:未經授權會被視為的特殊。 它會指出的 Xbox Live 的驗證權杖過期，因此 Xbox 服務 API 呼叫來重新整理權杖的 OS，並再執行一次重試。

執行重試時，它會是不呼叫服務，直到已達到 「 Retry-after 」 標頭時的最佳作法。 XSAPI 現在會實作此最佳做法。 如果失敗的 HTTP 狀態碼和 「 Retry-after 」 標頭未傳回任何 api，其他後重試時間之前相同的 API 呼叫會立即傳回，原始的錯誤，而不叫用服務。

重試呼叫時執行指數退避法使用隨機的抖動，以分散至服務的負載的最佳作法。 XSAPI 開頭為 2 秒預設延遲來控制使用 xbox\_live\_內容\_settings::set\_http\_重試\_delay()。 這表示根據預設，每個重試沒有指數退避法的 2、 4、 8、 等秒和它 jitters 目前和下一個輪詢值根據進一步 spread 放大負載的回應時間分散給整組裝置嘗試重試之間的延遲。

標題應如何控制時間花在重試呼叫。 使用 XSAPI，開發人員可以直接控制這個使用函式 xbox\_live\_內容\_settings::set\_http\_逾時\_window()。 根據預設，這是設定為 20 秒。 設定為 0 秒，這將會有效地關閉重試邏輯。 XSAPI 現在也會動態調整多少時間左仍會保留在 http 為基礎的內部 HTTP 逾時\_逾時\_window()。 內部的 HTTP 逾時控制多久 OS 花費在 HTTP 網路作業之前它會中止。 此呼叫將不會重試除非那里維持至少 5 秒處於 http\_逾時\_window() 提供足以合理的時間才能完成呼叫。 這項規則不適用於第一次呼叫，所以設定 http\_逾時\_window() 設為 0 是可以接受的而且會產生單一呼叫中。 此邏輯的效果，http\_逾時\_window() 在 API 呼叫會傳回時更具決定性。 如果傳回的 「 Retry-after 」 標頭，沒有 reties 以前的 「 Retry-after 」 時間到達之後。 如果 「 Retry-after 」 時間是在 http\_逾時\_window()，然後呼叫傳回的 http 結尾\_逾時\_window()。


## <a name="error-handling"></a>錯誤處理

標題的開發人員應該**一律**使用適當的錯誤處理**每個**服務呼叫，他們需要以確保它們會正確處理失敗的回應。
 
有許多真實世界的情況，可能會導致要求到 Xbox Live，例如傳回失敗碼

1.  無法使用網路。 比方說，在裝置遺失 4g，遺失 Wi-fi 或網路已關閉。
2.  太多負載的負載 (503) 上的服務
3.  失敗發生在服務 (500)
4.  太多要求，傳送至服務 (429)
5.  寫入作業衝突 (412)。 例如，多人連線的工作階段中的其他播放器提交變更第一次
6.  使用者已被禁用，或沒有權限
7.  使用者已簽署外

適當的錯誤處理常式，請務必以確保正常運作的遊戲： 在這些情況。

XSAPI 有兩種類型的錯誤處理模式。 其中一個模式時使用 WinRT Api，從 C + + /CX，C\#，或 Javascript，並使用新的 c + + Api 時的另一種模式。 完整詳細資料如果您的錯誤處理的最佳作法，請參閱 Xbox Live 的文件頁面，而此一 」 的錯誤處理 」 並涵蓋了這段影片中，請參閱溝通[ *Xfest 2015 影片*](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx)呼叫*XSAPI:C + +，任何例外狀況 ！*


## <a name="best-calling-patterns"></a>最佳的呼叫模式

### <a name="usebatching-requests"></a>使用批次要求

批次或彙總的一組要求至單一呼叫，就會支援某些端點。 例如，使用 Xbox Live 設定檔服務，您可以要求單一使用者的設定檔或一組使用者設定檔。 因此如果您需要使用者設定檔的一組使用者時，會針對每個使用者設定檔一次呼叫的端點或其中一個 API 非常沒有效率。 每個呼叫加入驗證的額外負荷很多。 因此，傳遞您想要對 API，一次的相關資訊，如此端點可以在相同的時間處理所有使用者設定檔，並傳回單一回應的所有使用者。

### <a name="use-the-real-time-activity-rta-service-instead-of-polling"></a>使用即時活動 (RTA) 服務，而非輪詢

另一個最佳做法是使用即時活動 (RTA) 服務而定期輪詢。 此服務會公開用戶端上服務的目標資源變更時，傳送通知給用戶端 websocket。 RTA 服務就會提供目前狀態變更、 統計資料的變更，多人連線的工作階段的文件變更和社交的關聯性變更的通知。 若要知道什麼資訊用戶端有興趣，用戶端必須先訂閱項目透過 websocket。 這可避免輪詢服務以偵測變更，因為剛好在項目變更時，才會通知您。

RTA 服務做為一組 XSAPI 會公開訂閱用戶端可以使用的 Api。 每個 Api 具有對應\*\_變更\_處理常式中的項目變更時呼叫的回呼函式應用程式開發介面。

* 出席\_service::subscribe\_要\_裝置\_存在\_變更
<br>
* 出席\_service::subscribe\_要\_標題\_存在\_變更
<br>
* user\_statistics\_service::subscribe\_to\_statistic\_change
<br>
* social\_service::subscribe\_to\_social\_relationship\_change<br>
 

## <a name="use-xbox-live-client-side-managers"></a>使用 Xbox Live 的用戶端端管理員

新 XSAPI 中我們現在有一組做為執行提取功能為特定案例的所有繁重的快取和狀態機器的管理員。


### <a name="social-manager"></a>身分證管理員

身分證 Manager 沒有朋友清單和設定檔的所有繁重工作。 它會保留您的朋友清單、 其設定檔，以及其目前狀態的資料最新狀態使用 RTA 服務。 「 管理員 」 會公開同步的 API，是易記的非常遊戲引擎。 遊戲，可以呼叫其 Api 常見問題，因為它會維護記憶體中的快取服務中最新的資訊。 如需詳細資訊，請參閱的 Xbox Live 文件頁面 」 社交 Manager 簡介 」

### <a name="multiplayer-manager"></a>多人遊戲管理員

多人連線的工作階段管理多人遊戲 Manager 是直接的解決方案，適用於傳統的多人連線遊戲。 多人遊戲管理員 API 包含播放程式名冊和工作階段管理、 處理遊戲的邀請，在進行中，配對，聯結和外掛到您現有的網路解決方案。 它會實作傳統的多人遊戲流程周圍的所有繁重工作。 如需詳細資訊，請參閱的 Xbox Live 文件頁面 」 的多人 Manager 簡介 」


## <a name="throttling-fine-grained-rate-limiting"></a>節流 （沒問題更細緻的速率限制）

Xbox Live 服務的節流以將極端負載放在服務上時，防止任何單一裝置。 請務必知道您的標題已節流處理。 您可以指示是否您的標題已節流處理幾個不同的方式：


### <a name="monitor-for-http-status-code-429"></a>HTTP 狀態碼 429 的監視器

您可以使用 Fiddler，並觀看是否會傳回 HTTP 狀態碼 429。 JSON 回應會包含有關如何端點已受節流控制詳細資料。 例如：

```json
{
  "version":1,
  "currentRequests":13,
  "maxRequests":10,
  "periodInSeconds":120,
  "limitType":"Rate"
}
```

如果您使用 XSAPI，Api 會傳回 http\_狀態\_429\_太\_許多\_要求錯誤而設定的錯誤訊息，是關於如何 API 已受節流控制的詳細資料。

### <a name="debug-asserts"></a>偵錯判斷提示

當使用 XSAPI，如果呼叫已節流處理，在開發人員沙箱中，並使用偵錯組建的標題，它會判斷提示，立即讓開發人員知道會發生節流。 這是為了避免不小心遺失 429 的節流錯誤，因為撰寫不正確的程式碼。 如果您想要停用這些判斷提示至未修正違規的程式碼繼續運作，您可以呼叫此 API:


> xboxLiveContext-&gt;settings （）-&gt;停用\_判斷提示\_for\_xbox\_live\_節流\_中\_開發\_沙箱 (xbox\_live\_內容\_節流\_setting::this\_程式碼\_需要\_至\_是\_變更\_要\_避免\_節流);

但請注意，此 API 會防止您從進行節流的標題。 您的標題將仍會受到節流控制。 這只會停用時使用偵錯組建會在開發沙箱中判斷提示。 

### <a name="xbox-live-trace-analyzer-tool"></a>Xbox Live 追蹤分析師 工具

另一個選項是記錄的 Xbox Live 的呼叫追蹤和分析 [追蹤使用[Xbox Live 追蹤分析師] 工具。](https://docs.microsoft.com/windows/uwp/xbox-live/tools/analyze-service-calls)

若要錄製追蹤，您可以使用 Fiddler 以記錄。SAZ 檔案，或使用 XSAPI 的內建的追蹤記錄。 如需詳細資訊，若要使用 XSAPI 中追蹤開啟請參閱的 Xbox Live 文件頁面 「 Xbox Live 服務的分析呼叫 」 的方式。 追蹤之後，Xbox Live 追蹤分析工具就會發出警告時偵測節流處理的呼叫。

## <a name="is-xbox-live-up"></a>Xbox Live 嗎？

Xbox Live 是公開 Xbox Live 的功能，例如設定檔、 朋友和目前狀態、 統計資料、 排行榜、 成積、 多人遊戲和配對的微服務的集合。 沒有單一伺服器或端點，會定義如果 Xbox Live 已啟動。 如果一部伺服器故障時，Xbox Live 的微服務的其餘部分大多無關，而且應該操作。

如果單一服務發生暫時性中斷，務必知道這個服務呼叫是否為任務關鍵性的遊戲。 嘗試提供合理的體驗，而有間歇性的網路或服務問題。 比方說，如果 「 狀態 」 服務會傳回呼叫可能的失敗不會為任務關鍵性的遊戲。 因此只會回報給使用者最後一個已知的目前狀態，而不是 reporting Xbox Live 已關閉。

Xbox Live，也會遵循最終一致性的一致性模型。 這表示，如果進行任何新的更新，所以，最後該資源的所有要求會都報告最後一個更新的值。 這實際上表示一小段資訊的過時資料的地方會傳播。
