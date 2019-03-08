---
title: 讀取組態 blob
description: 了解如何讀取 Xbox Live 的標題儲存體中的組態 blob。
ms.assetid: ee62d221-69b9-4f52-9b5d-5a44d04de548
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 71607f05f07ee4a98f73a19c28c85dc325041a10
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599393"
---
# <a name="reading-a-configuration-blob-in-xbox-live-title-storage"></a>讀取在 Xbox Live 的標題儲存體中的組態 blob

*組態 blob* Xbox Live 的標題儲存體中保存遊戲資料的檔案。 資料是 JSON 物件，包含可篩選才能傳遞到遊戲的虛擬節點。 如需有關設定 blob 的詳細資訊，請參閱 <<c0>  **標題儲存體 Uri**。

### <a name="to-read-a-configuration-blob"></a>若要讀取的組態 blob

1.  傳送要求，使用下列方法，以從標題的儲存體讀取資料。

        GET https://titlestorage.xboxlive.com/global/scids/{scid}/data/config.json,config              
        Content-Type: application/octet-stream
        x-xbl-contract-version: 1
        Authorization: XBL3.0 x=<userHash>;<STSTokenString>
        Connection: Keep-Alive


-   使用者必須更新該工作階段中。
-   STSTokenString 是為求簡單明瞭的預留位置，而且應該以驗證要求所傳回的語彙基元取代。

#### <a name="reference"></a>參考

**/global/scids/{scid}/data/{pathAndFileName},{type}**
**GET (/global/scids/{scid}/data/{pathAndFileName},{type})**
