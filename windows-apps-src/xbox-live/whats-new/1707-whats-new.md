---
title: 新功能在 Xbox Live Api-2017 年 7 月
description: 新功能在 Xbox Live Api-2017 年 7 月
ms.assetid: ''
ms.date: 07/16/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox 一個新功能、 2017 年 7 月
ms.localizationpriority: medium
ms.openlocfilehash: 47db721a98b71fd6f4b5175a88ddccd00048d8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618823"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>新功能的 Xbox Live api-2017 年 7 月

請參閱[的新功能-2017 年 6 月](1706-whats-new.md)的新增功能的文章在 2017 年 6 月版本。

您也可以檢查[Xbox Live API GitHub 認可記錄](https://github.com/Microsoft/xbox-live-api/commits/master)若要查看所有最新的程式碼變更，到 Xbox Live Api。

## <a name="xbox-live-features"></a>Xbox Live 功能

### <a name="multiplayer-updates"></a>多人遊戲的更新

現在查詢活動的控制代碼和搜尋功能在處理在回應中包含自訂的工作階段屬性。

### <a name="tournaments"></a>比賽

新的 Api 已新增以支援比賽。 您現在可以使用 xbox::services::tournaments::tournament_service 類別來存取比賽服務從您的標題。
這些新聯賽 Api 適用於下列案例：
* 查詢服務，以尋找目前的標題中的所有現有的比賽。
* 從服務擷取聯賽的相關詳細資料。
* 查詢服務，以擷取一份聯賽的小組。
* 從服務擷取相關的小組聯賽。 詳細資料。
* 追蹤變更比賽與小組使用即時活動 (RTA) 的訂用帳戶。
