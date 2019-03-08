---
title: 遊戲伺服器通用資源識別元 (URI) 參考
assetID: bbd7e3f3-77ac-6ffd-8951-fe4b8b48eb4c
permalink: en-us/docs/xboxlive/rest/atoc-gsdk-uri-reference.html
description: " 遊戲伺服器通用資源識別元 (URI) 參考"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a9a0a38cff9214485b2d7e8b1f8a28acb3207444
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662593"
---
# <a name="game-server-universal-resource-identifier-uri-reference"></a>遊戲伺服器通用資源識別元 (URI) 參考
用戶端用來建立遊戲伺服器開發套件標題的伺服器執行個體的 Uri。 這些 uri 的網域`gameserverds.xboxlive.com`和`gameserverms.xboxlive.com`。
 
<a id="ID4EY"></a>

 
## <a name="in-this-section"></a>本節內容

[/qosservers](uri-qosservers.md)

&nbsp;&nbsp;若要取得可用的 QoS 伺服器清單用於 Xbox Live 計算用戶端呼叫的 URI。

[/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

&nbsp;&nbsp;允許用戶端建立一個標題的 Xbox Live 計算伺服器執行個體的 URI。

[/titles/{titleId}/variants](uri-titlestitleidvariants.md)

&nbsp;&nbsp;若要取得可用的變體標題的用戶端呼叫的 URI。

[/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

&nbsp;&nbsp;要求 Xbox Live Compute sessionhost 配置指定的標題的識別碼。

[/titles/{titleId}/sessions/{sessionId}/allocationStatus](uri-titlestitleidsessionssessionidallocationstatus.md)

&nbsp;&nbsp;針對給定的書名識別碼和工作階段識別碼，取得票證要求的狀態。
 