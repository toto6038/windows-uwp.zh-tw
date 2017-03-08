---
author: DelfCo
ms.assetid: 2CC2E526-DACB-4008-9539-DA3D0C190290
description: "適用於 UWP 開發人員的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。"
title: "哪一種網路功能技術？"
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3ee88a22fe50fc3d782febafdc82c2a68c386dab
ms.lasthandoff: 02/07/2017

---

# <a name="which-networking-technology"></a>哪一種網路功能技術？

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

適用於 UWP 開發人員的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。

## <a name="sockets"></a>通訊端

當您與另一個裝置通訊，並想要使用自己的通訊協定時，請使用[通訊端](sockets.md)。

通用 Windows 平台 (UWP) 開發人員有兩種通訊端實作可使用：[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 和 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)。 如果您要撰寫新的程式碼，請善加利用 Windows.Networking.Sockets，因為它是針對 UWP 開發人員設計的新型 API。 如果您要使用跨平台的網路程式庫或其他現有的 Winsock 程式碼，或偏好 Winsock API，請逕行使用。

### <a name="when-to-use-sockets"></a>使用通訊端的時機

-   這兩個通訊端實作都可讓您使用自己選擇的通訊協定、使用 TCP 或 UDP 與其他裝置進行通訊。

-   請根據體驗和您可能使用的任何現有程式碼，選擇與您的需求最相符的通訊端 API。

### <a name="when-not-to-use-sockets"></a>不應使用通訊端的時機

-   請不要使用通訊端實作您自己的 HTTP(S) 堆疊。 請改用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)。
-   如果 WebSocket ([**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 類別) 符合您的通訊需求 (進入/來自網頁伺服器的 TCP)，請考慮使用 WebSocket，而不要自行花費時間和開發資源實作通訊端的類似功能。

## <a name="websockets"></a>WebSocket

[WebSocket](websockets.md) 通訊協定定義了一項機制，可讓用戶端與伺服器之間透過 Web 快速且安全地進行雙向通訊。 資料會立即透過全雙工單一通訊端連線來傳輸，因此能即時從這兩個端點傳送與接收訊息。 WebSocket 十分適用於立即社交網路通知和最新資訊顯示 (如遊戲統計資料) 需具備安全性，且需要快速資料傳輸的即時遊戲。 UWP 開發人員可以使用 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 類別與支援 Websocket 通訊協定的伺服器連接。

### <a name="when-to-use-websockets"></a>使用 Websocket 的時機

-   當您想要持續地在裝置與伺服器之間傳送和接收資料時。

### <a name="when-not-to-use-websockets"></a>不應使用 Websocket 的時機

-   如果您不常傳送或接收資料，您可能會發現提出從裝置到伺服器的個別 HTTP 要求，會比建立並維護 WebSocket 連線更為容易。
-   Websocket 可能不適用於非常大量的情況。 請考慮先將您的資料流模型化並模擬通過 Websocket 的流量，再確認要在您的設計中加以使用。

## <a name="httpclient"></a>HttpClient

使用 HTTP(S) 與 Web 服務或網頁伺服器通訊時，請使用 [HttpClient](httpclient.md) (和其餘 [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間 API)。

### <a name="when-to-use-httpclient"></a>使用 HttpClient 的時機

-   使用 HTTP(S) 與 Web 服務通訊時。
-   上傳或下載少量的小檔案時。
-   如果 WebSocket ([**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 類別) 符合您的通訊需求 (前往或來自網頁伺服器的 TCP)，且相關的網頁伺服器支援 WebSocket，請考慮使用 WebSocket，而不要自行花費時間和開發資源實作 HttpClient 的類似功能。
-   當您在網路上串流內容。

### <a name="when-not-to-use-httpclient"></a>不應使用 HttpClient 的時機

-   如果您要傳輸大型檔案或大量檔案，請考慮改用背景傳輸。
-   如果您想要根據連線類型限制上傳/下載限制，或想要儲存進度並在中斷之後繼續上傳/下載，則您需要使用前景傳輸。
-   如果您想在兩個裝置通訊，而兩者都未設計成為 HTTP(S) 伺服器，則應使用通訊端。 不要嘗試實作您自己的 HTTP 伺服器，而應使用 [HttpClient](httpclient.md) 與伺服器通訊。

## <a name="background-transfers"></a>背景傳輸

當您想要可靠地透過網路傳輸檔案時，請使用[背景傳輸 API](background-transfers.md)。 背景傳輸 API 提供進階的上傳和下載功能，這些功能會在 app 暫停期間於背景執行，並在 app 終止後保留。 API 會監視網路狀態，並自動在連線中斷時暫停和繼續傳輸，傳輸作業會是數據用量感知和電池用量感知，這表示下載活動會根據您目前的連線能力與裝置電池狀態進行調整。 當您的應用程式執行於行動裝置或電池供電的裝置時，會用到這些功能。 API 適用於上傳和下載使用 HTTP(S) 的大型檔案。 也支援 FTP，但只限於下載項目。

Windows 10 中新增的背景傳輸功能，是能夠在檔案傳輸完成時觸發後續處理，以便您更新本機目錄、啟用其他應用程式，或在下載完成時通知使用者。

### <a name="when-to-use-background-transfers"></a>使用背景傳輸的時機

-   使用背景傳輸可以可靠地傳輸大型檔案或大量檔案。
-   當您想要以背景工作進行檔案傳輸的後續處理時，請使用具有背景傳輸完成群組的背景傳輸。
-   如果您想要在網路中斷之後繼續進行中的傳輸，請使用背景傳輸。
-   如果您想要根據網路條件 (如使用計量付費數據傳輸方案時) 變更傳輸行為，請使用背景傳輸。

### <a name="when-not-to-use-background-transfers"></a>不應使用背景傳輸的時機

-   如果您要傳輸少量的小檔案，而且您在傳輸完成時不需要執行任何後續處理，請考慮使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) PUT 或 POST 方法。
-   如果您想要串流資料並於抵達本機時使用它，請使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)。

## <a name="additional-network-related-technologies"></a>其他網路相關技術

### <a name="connection-quality"></a>連線品質

[**Windows.Networking.Connectivity**](https://msdn.microsoft.com/library/windows/apps/br207308) API 可讓您存取網路連線、成本和使用資訊。 如需關於使用此 API 的詳細資訊，請參閱[存取網路連線狀態和管理網路成本](https://msdn.microsoft.com/library/windows/apps/hh452983)。

### <a name="dns-service-discovery"></a>DNS 服務探索

[**Windows.Networking.ServiceDiscovery.Dnssd**](https://msdn.microsoft.com/library/windows/apps/dn895183) API 可讓您使用 IETF [RFC 2782](http://go.microsoft.com/fwlink/?LinkId=524158) 中所述的 DNS-SD 通訊協定，將網路服務通告到網路上的其他裝置。

### <a name="communicating-over-bluetooth"></a>透過藍牙通訊

除了其他眾多功能外，[**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/dn263413) API 還可讓您使用藍牙連接到其他裝置並傳送資料。 如需詳細資訊，請參閱[使用 RFCOMM 傳送或接收檔案](https://msdn.microsoft.com/library/windows/apps/mt270289)。

### <a name="push-notifications-wns"></a>推播通知 (WNS)

[**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) API 可讓您使用 Windows 通知服務 (WNS) 透過網路接收推播通知。 如需關於使用此 API 的詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](https://msdn.microsoft.com/library/windows/apps/mt187203)。

### <a name="near-field-communications"></a>近距離無線通訊

[**Windows.Networking.Proximity**](https://msdn.microsoft.com/library/windows/apps/br241250) API 可讓您將近距離無線通訊用在支援裝置鄰近性和輕觸的 app 上，使資料傳輸更為容易。 如需關於使用此 API 的詳細資訊，請參閱[支援鄰近性和輕觸](https://msdn.microsoft.com/library/windows/apps/hh465229)。

### <a name="rssatom-feeds"></a>RSS/Atom 摘要

[**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) API 可讓您使用 RSS 和 Atom 格式管理新聞訂閱方式摘要。 如需關於使用此 API 的詳細資訊，請參閱 [RSS/Atom 摘要](web-feeds.md)。

### <a name="wi-fi-enumeration-and-connection-control"></a>Wi-Fi 列舉和連線控制

[**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/dn975224) API 可讓您列舉 Wi-Fi 介面卡、掃描可用的 Wi-Fi 網路，以及將介面卡連接到網路。

### <a name="radio-control"></a>無線電控制

[**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/dn996447) API 可讓您尋找和控制本機裝置上的無線電，包括 Wi-Fi 和藍牙。

### <a name="wi-fi-direct"></a>Wi-Fi Direct

[**Windows.Devices.WiFiDirect**](https://msdn.microsoft.com/library/windows/apps/dn297687) API 可讓您使用 Wi-Fi Direct 與其他本機裝置連接和通訊，以建立臨機操作的區域無線網路。

### <a name="wi-fi-direct-services"></a>Wi-Fi Direct 服務

[**Windows.Devices.WiFiDirect.Services**](https://msdn.microsoft.com/library/windows/apps/dn996481) API 可讓您提供 Wi-Fi Direct 服務及加以連接。 Wi-Fi Direct 服務是 Wi-Fi Direct 臨機操作網路上的某個裝置 (服務廣告商) 透過 Wi-Fi Direct 連線將功能提供給另一個裝置 (服務取用者) 的方式。

### <a name="mobile-operators"></a>電信業者

Windows 10 為廣泛的開發人員對象公開了一些先前僅公開給裝置製造商和電信業者的 API。 請注意，雖然這些 API 現已公開，但仍受控於在應用程式發行之前必須先由 Microsoft 核准的特定應用程式功能。 這些 API 的實際使用，主要仍將受限於裝置製造商和電信業者。

### <a name="network-operations"></a>網路作業

[**Windows.Networking.NetworkOperators**](https://msdn.microsoft.com/library/windows/apps/br241148) API 主要負責處理手機的設定與佈建。 因此，使用其控制功能的權限，會受限於裝置製造商及電信業者。

### <a name="sms"></a>SMS

[**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/br206567) 命名空間會以低層級實體的形式處理 SMS 與相關訊息。 它提供給電信業者用於 app 導向的 SMS 用途，且受控於將不會核准給大多數 app 開發人員使用的功能。 如果您要撰寫用來處理訊息的應用程式，您應改用 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/dn642321) API，因為它不只可用來處理 SMS 訊息，也可處理其他來源 (例如即時聊天應用程式) 的訊息，而帶來更豐富的聊天/傳訊使用體驗。


