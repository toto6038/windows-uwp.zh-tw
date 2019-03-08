---
title: Xbox Live 的儲存體平台
description: 深入了解 Xbox Live 儲存體平台，其中包含連接的儲存體和標題的儲存體。
ms.assetid: 3c92549c-65fd-4d26-a693-3aded8bae498
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8104c0945fb6987611e71e2ccb0fada2b746c03c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660193"
---
# <a name="xbox-live-storage-platform---connected-storage-title-storage"></a>Xbox Live 的儲存體平台-已連接的儲存體，標題儲存體

Xbox Live 可讓發行者將儲存在雲端的全域項目資料與播放程式的特定資料。

## <a name="in-this-section"></a>本節內容

[連接儲存體](connected-storage/connected-storage-overview.md)自動使用每個使用者連接儲存體 API 來儲存的資料在 PC 和 Xbox One 主控台，漫遊使用者和也是可供離線使用。 使用這項服務，以便之後在裝置之間切換，重新啟動標題時，請繼續順利進行的遊戲。 您應該經常儲存進度資料，例如清查、 遊戲狀態和目前的位置，在遊戲中使用連接儲存體服務。 連接的儲存體服務是多個錯誤容錯的雲端儲存體服務，並較不易受到網路和電源失敗。

[Xbox Live 的標題儲存體](xbox-live-title-storage/xbox-live-title-storage.md)Xbox Live 標題儲存體服務可用來儲存及共用遊戲資料和雲端中的標題資產。 在所有平台上執行的遊戲可以使用此連線。 此服務會提供更充分掌控資料可見性，取用者，以及全域每一位使用者的資料除了標題資料。 適合用來儲存播放程式統計資料、 玩家排名和標題資產，如 unlockable 藝術品和新的地圖標題的儲存體。