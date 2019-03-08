---
title: 精選的統計資料和排行榜 2017
description: 了解如何設定在合作夥伴中心內的 Xbox Live 功能統計資料和排行榜 2017
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live、 Xbox、 遊戲、 uwp、 windows 10，其中一個 Xbox、 精選的統計資料和排行榜、 排行榜、 統計資料 2017 起，合作夥伴中心
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655103"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>在合作夥伴中心設定精選的統計資料和排行榜 2017

與統計資料服務互動遊戲，統計資料，必須定義在[合作夥伴中心](https://partner.microsoft.com/dashboard)。 所有精選的統計資料會顯示在 GameHub，讓它自動做為排行榜。 我們將儲存的未經處理的值，不過，遊戲將擁有的邏輯來判斷是否應提供新的值。

![遊戲中樞上的成就 頁面的螢幕擷取畫面](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png)上圖顯示在您的標題 GameHub 精選的統計資料的外觀。 精選的統計資料會顯示具有紅色方塊。

使用資料平台 2017 時，您只需要設定用來在玩家的 GameHub 頁面上顯示為精選的社交排行榜 」 狀態。

您可以使用合作夥伴中心設定精選的統計資料和您的遊戲與相關聯的排行榜。 新增組態，執行下列動作：

1. 瀏覽至**精選的統計資料和排行榜**一節以取得您的標題，位於**Services** > **Xbox Live**  >  **精選統計資料和排行榜**。
2. 按一下 **新增**按鈕會開啟強制回應表單。 一旦填入，按一下**儲存**。

![新的精選 stat/排行榜對話方塊的映像](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

**顯示名稱**欄位便是使用者會看到 GameHub 中。 這個字串中可當地語系化**當地語系化字串**Xbox Live 服務的組態。

**識別碼**欄位是統計資料的名稱，而是如何您將會參照您的統計資料更新您的程式碼時。 請參閱[更新的統計資料](../../leaderboards-and-stats-2017/player-stats-updating.md)如需詳細資訊。

**格式**是統計資料的資料格式。選項包括整數、 小數、 百分比、 短的時間範圍、 長的時間範圍和字串。

每個**格式**選項可讓在可接受的值，或當選取向下格式化下拉式清單底下的某些資訊。

* 整數統計資料會接受整數，例如 1、 2 或 100。
* 十進位的統計資料會接受具有兩個小數位數，例如 1.05 或 12.00 小數點的數字
* 百分比的統計資料會接受介於 0 到 100 之間的整數。 '%' 將會附加至整數值的結尾。 （例如 0%、 100%）
* 簡短的時間範圍的統計資料會遵循 02:10:30，hh: mm： 格式，並會要求您提供的統計資料的時間單位。 可用的時間單位為毫秒、 秒、 分鐘、 小時和天數。
* 長的時間範圍狀態會遵循 1 d h 10 2m Xd Xh Xml 格式，此統計資料會也要求您提供的統計資料的時間單位。

**排序**欄位可讓您變更排行榜遞增或遞減的排序次序。

請設定精選的統計資料和排行榜時，注意下列需求：

| 開發人員類型 | 需求 | 限制 |
|----------------|-------------|-------|
| Xbox Live 創作者計畫 | 不需要將任何統計資料指定為精選的統計資料 | 20 |
| ID@Xbox 與 Microsoft 合作夥伴 | 您必須指定至少 3 個精選的統計資料 | 20 |

## <a name="next-steps"></a>後續步驟

接下來您將需要更新您的程式碼的統計資料。  請參閱[更新的統計資料](../../leaderboards-and-stats-2017/player-stats-updating.md)如需詳細資訊。
