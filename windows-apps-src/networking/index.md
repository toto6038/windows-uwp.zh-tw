---
author: stevewhims
ms.assetid: 7bb9fd81-8ab5-4f8d-a854-ce285b0669a4
description: 存取網路和 Web 服務的技術。
title: 網路和 Web 服務
ms.author: stwhi
ms.date: 11/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e01ac3a0dcab0bc82835b97d70477bf585ab4570
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5553359"
---
# <a name="networking-and-web-services"></a>網路和 Web 服務

下列網路和 Web 服務技術可供通用 Windows 平台 (UWP) 開發人員使用。

| 主題 | 描述 |
| - | - |
| [網路功能基本知識](networking-basics.md) | 您對於任何具備網路功能的 app 所需執行的動作。 |
| [哪一種網路功能技術？](which-networking-technology.md) | 適用於 UWP 開發人員的網路功能技術快速概觀，並建議您如何選擇最適合您的 app 的技術。 |
| [背景網路通訊](network-communications-in-the-background.md) | 若要繼續網路通訊 (不是在背景中)，應用程式可以使用背景工作與通訊端代理程式或控制通道觸發程序。 |
| [通訊端](sockets.md) | 通訊端是低階資料傳輸技術，許多網路通訊協定在其上實作。 UWP 為用戶端-伺服器或對等應用程式提供 TCP 與 UDP 通訊端類別，不需要指定連線是長期或已建立的連線。 |
| [WebSocket](websockets.md) | WebSocket 提供了一項機制，可讓用戶端與伺服器之間透過使用 HTTP(S) 的 Web 快速且安全地進行雙向通訊，並支援 UTF-8 和二進位訊息。 |
| [HttpClient](httpclient.md) | 使用 [Windows.Web.Http](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間 API，透過 HTTP 2.0 與 HTTP 1.1 通訊協定傳送和接收資訊。 |
| [RSS/Atom 摘要](web-feeds.md) | 使用 [Windows.Web.Syndication](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空間中的功能，以 RSS 和 Atom 標準為依據產生的同步發佈摘要，來抓取或建立最新最熱門的網頁內容。 |
| [背景傳輸](background-transfers.md) | 使用背景傳輸 API 在網路上可靠地複製檔案。 |
