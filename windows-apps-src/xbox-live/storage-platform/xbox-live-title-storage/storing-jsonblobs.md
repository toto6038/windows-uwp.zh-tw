---
title: 儲存的 JSON blob
description: 了解如何在 Xbox Live 的標題儲存體中儲存的 JSON blob。
ms.assetid: f424aca1-e671-4e31-84c6-098fda4a9558
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 標題儲存體
ms.localizationpriority: medium
ms.openlocfilehash: dabcd0634bb20bc6b82ae302c77338b5bb726a63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595063"
---
# <a name="storing-a-json-blob-in-xbox-live-title-storage"></a>在 Xbox Live 的標題儲存體中儲存的 JSON blob

1.  傳送要求，使用*PUT*方法，以將資料傳送至標題儲存體。

        PUT https://titlestorage.xboxlive.com/json/users/xuid(1245111)/scids/{scid}/data/{pathAndFileName},json
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 240
        Connection: Keep-Alive



-   使用者必須更新該工作階段中。

-   STSTokenString 是為求簡單明瞭的預留位置，而且應該以驗證要求所傳回的語彙基元取代。

2.  傳送的 JSON 物件。

        {
            "startlevel":"1",
            "expression":"smile"
        }

#### <a name="reference"></a>參考

**/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json)**
