---
title: 將二進位 blob 儲存
description: 了解如何在 Xbox Live 的標題儲存體中儲存的二進位 blob。
ms.assetid: a0da36ef-5a5a-466d-80a8-e055ba7e4cdc
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 標題儲存體
ms.localizationpriority: medium
ms.openlocfilehash: 70e620f2e992dfdd240f71b5c73165f13d4ef5cb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660093"
---
# <a name="storing-a-binary-blob-in-xbox-live-title-storage"></a>在 Xbox Live 的標題儲存體中儲存二進位 blob

1.  傳送要求，使用下列方法，以將資料傳送至標題儲存體。

        PUT https://titlestorage.xboxlive.com/trustedplatform/users/xuid(1245111)/scids/{scid}/data/lastturn.bin,binary              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Content-Length: 40
        Connection: Keep-Alive


-   使用者必須更新該工作階段中。

-   STSTokenString 是為求簡單明瞭的預留位置，而且應該以驗證要求所傳回的語彙基元取代。

2.  傳送二進位資料。 因為資料會透過 HTTP 傳輸，資料必須受到條件約束的可接受的字元集。 必須編碼資訊，例如影像或音訊資料。 您可以選取任何會產生 HTTP 相容字元的編碼方式。
d
```
  01EAEFBAD05903A4
  1EA2311656677DFF
  CF00
```

#### <a name="reference"></a>參考

**/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}**
