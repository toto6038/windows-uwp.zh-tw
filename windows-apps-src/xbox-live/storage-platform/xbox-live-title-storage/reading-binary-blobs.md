---
title: 讀取二進位 blob
description: 深入了解讀取 Xbox Live 的標題儲存體中的二進位 blob。
ms.assetid: 9b8e0c35-0cea-4491-bf30-22fad224f11b
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 標題儲存體
ms.localizationpriority: medium
ms.openlocfilehash: e2a0be72142a3e11c7c680cfc998287f396c2afc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641653"
---
# <a name="reading-a-binary-blob-in-xbox-live-title-storage"></a>讀取在 Xbox Live 的標題儲存體中的二進位 blob

1.  傳送要求，使用*取得*從標題的儲存體讀取資料的方法。 此範例會使用全域的標題儲存體。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/userinfo.bin,binary
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive



-   使用者必須更新該工作階段中。

-   STSTokenString 是為求簡單明瞭的預留位置，而且應該以驗證要求所傳回的語彙基元取代。

#### <a name="reference"></a>參考

**/global/scids/{scid}/data/{pathAndFileName},{type}**
