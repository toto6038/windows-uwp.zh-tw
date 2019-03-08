---
title: 多人遊戲工作階段狀態碼
description: 描述要求的多人連線的工作階段時，從 Xbox Live 服務傳回的狀態碼。
ms.assetid: 4ab320d6-8050-41a9-9f00-faaad3b128fd
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，其中，多人遊戲 2015，狀態碼、 工作階段
ms.localizationpriority: medium
ms.openlocfilehash: 8fbddd0070eb24d6fc050c59fa2a0197f98ee08c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646303"
---
# <a name="multiplayer-session-status-codes"></a>多人遊戲工作階段狀態碼

本主題列出有關工作階段的多人連線的狀態碼。

| 注意                                                                                                         |
|---------------------------------------------------------------------------------------------------------------------------|
| 4xx 狀態碼傳回的工作階段一律會傳回整個工作階段中，即使 URI 會指向工作階段項目。 |


| 狀態碼 | 字串              | Content-Type     | 內文    | 描述 |
|----|
| 200         | 確定                  | 應用程式/json | 工作階段 | 已成功讀取 (GET) 或更新 (PUT)。                                                                                                                                                                                                                                                                                                             |
| 201         | 建立時間             | 應用程式/json | 工作階段 | 已成功建立。                                                                                                                                                                                                                                                                                                                                 |
| 202         | 接受            | text/plain       | 無    | 要求被接受，但尚未完成。                                                                                                                                                                                                                                                                                             |
| 204         | 沒有內容          |                  |         | 取得工作階段，在工作階段不存在。 取得工作階段項目，工作階段存在但項目則否。 在 工作階段的 PUT、 工作階段已刪除的 PUT 作業的結果。 PUT 或 DELETE 的工作階段項目，當作業開始，但是工作階段或項目不存在時，就已存在的工作階段。 |
| 304         | 未修改        |                  |         | 在 GET-If-none-match 標頭、 工作階段並沒有變更。                                                                                                                                                                                                                                                                                        |
| 400         | 不正確的要求         | text/plain       | 訊息 | 要求會假設為在第一個檢查無效。 遺漏必要的欄位，或 JSON 檔案格式不正確。 本文包含的其他詳細資料。                                                                                                                                                                                        |
| 403         | 已禁止           | text/plain       | 訊息 | 要求可能在某些內容中，有效，但不適合其內容。 授權失敗。                                                                                                                                                                                                                                                |
|             |                     | 應用程式/json | 工作階段 | 無法更新使用者的工作階段，而可以讀取。                                                                                                                                                                                                                                                                                           |
| 404         | 找不到           | text/plain       | 訊息 | 無法存取工作階段，因為 URI 無效;找不到控制代碼、 SCID 或工作階段範本;找不到 hopper;無法存取工作階段項目，因為工作階段不會結束。或項目查閱無效的工作階段。                                                                                 |
| 405         | 不允許的方法  | text/plain       | 訊息 | 要求 URI 是合理的但這個動作是錯誤。 例如，要求時，進行後置作業需要 PUT 作業。                                                                                                                                                                                                                 |
| 409         | 衝突            | text/plain       | 訊息 | 無法更新工作階段，因為要求的工作階段與不相容。 比方說，在要求中的常數與常數衝突中的工作階段或工作階段範本，或已加入或移除從大型的工作階段以外的呼叫端的成員。                                                                         |
| 412         | 先決條件失敗 |                  |         | 無法滿足的 If-match 標頭或 （適用於非 GET 作業），-If-none-match 標頭。                                                                                                                                                                                                                                           |
|             |                     | 應用程式/json | 工作階段 | 在現有的工作階段的 PUT 或 DELETE 作業，無法滿足 If-match 標頭。 工作階段的目前狀態會與目前的 ETag 值一起傳回。                                                                                                                                                                      |
| 429 | 太多要求 | 應用程式/json | 訊息 | 在服務呼叫已受節流控制，因為超過更細緻的速率限制的限制。 如需詳細資訊，請參閱 < [Fine 精細速率限制](../../using-xbox-live/best-practices/fine-grained-rate-limiting.md)。 |
| 503         | 服務無法使用 | text/plain       | 無    | 服務多載，而且應該稍後重試要求。 此程式碼包含 Retry-after 標頭的用戶端應接受。                                                                                                                                                                                                              |
