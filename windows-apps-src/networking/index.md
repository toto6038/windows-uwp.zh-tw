---
author: DelfCo
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: "存取網路和 Web 服務的技術。"
title: "網路和 Web 服務"
translationtype: Human Translation
ms.sourcegitcommit: 82edf9c3ee7f7303788b7a1272ecb261d3748c5a
ms.openlocfilehash: 03a5ce2b8d5f501d4254cbe0ee3d47f575775f7a

---

# 網路和 Web 服務

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

下列網路和 Web 服務技術可供通用 Windows 平台 (UWP) 開發人員使用。

| 主題                                                                                   | 描述                                                                      |
|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [網路功能基本知識](networking-basics.md)                                               | 您對於任何具備網路功能的 app 所需執行的動作。                     |
| [哪一種網路功能技術？](which-networking-technology.md)                          | 適用於 UWP 開發人員的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。               |
| [背景網路通訊](network-communications-in-the-background.md) | 使用背景工作的 app 及兩種當它們不在前景時保持通訊的機制：通訊端代理程式和控制通道觸發程序。                  |
| [通訊端](sockets.md)                                                                   | 您可以使用 [Windows.Networking.Sockets](https://msdn.microsoft.com/library/windows/apps/xaml/windows.networking.sockets.aspx) 和 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523)，以 UWP app 開發人員的身分與其他裝置通訊。 本主題提供使用 Windows.Networking.Sockets 命名空間執行網路功能作業的詳細指導方針。 |
| [WebSocket](websockets.md)                                                             | WebSocket 提供了一項機制，可讓用戶端與伺服器之間透過使用 HTTP(S) 的 Web 快速且安全地進行雙向通訊。                 |
| [HttpClient](httpclient.md)                                                             | 使用 [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間 API，透過 HTTP 2.0 與 HTTP 1.1 通訊協定傳送和接收資訊。             |
| [RSS/Atom 摘要](web-feeds.md)                                                          | 使用 [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空間中的功能，以 RSS 和 Atom 標準為依據產生的同步發佈摘要，來抓取或建立最新最熱門的網頁內容。                   |
| [背景傳輸](background-transfers.md)                                         | 使用背景傳輸 API 在網路上可靠地複製檔案。           |



<!--HONumber=Aug16_HO5-->


