---
title: 成就獎勵
description: 描述如何設定提供獎勵的成就。
ms.assetid: b6fc5bdb-ba7b-4687-985e-894182f066da
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 成就、 獎勵
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbcb06a2aba927301cf982d09fdb55192ec84c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617693"
---
# <a name="achievement-rewards"></a>成就獎勵

下圖說明如何為開發人員可能用來管理生命週期的標題。 新的成就系統設計為具有更大的彈性提升我們熟悉的技師修理 — 中如何成就是解除鎖定，和哪些權益中如何及何時會加入他們，他們將傳遞給使用者 — 可讓開發人員執行為服務的標題增加價值，並維護一段時間的使用者參與。

##### <a name="figure-1---how-a-title-might-drive-user-behavior"></a>圖 1.   如何標題可能驅使使用者行為。 #####
![rewarding_achievements](../images/omega/achievements_overview_01_drive_behavior.png)

### <a name="flexible-options-for-rewarding-achievement"></a>奬勵成就的彈性選項 ###
使用 Xbox One，我們已擴充的成就系統以支援更有彈性的報酬選項。 玩家分數會保持為跨 Xbox Live 的生態系統，但現在會追蹤使用者的單一、 共同的遊戲分數寶貴 reward — 開發人員或發行者，可以使用做為更廣泛的獎勵，同時在標題中的傳遞機制的成就與外部的標題。

可以設定多個獎勵，最多一個 reward reward 型別的每個成就。 沒有明確的報酬; 也可以設定的成就在此情況下，成就的圖示會當做 visual 徽章的玩家可以獲得成就。

Xbox Live 支援下列類型的報酬：

* 玩家分數
* 美工圖案
* 應用程式內獎勵

#### <a name="gamerscore"></a>玩家分數 ####
我們目前致力於維持已與我們的 Xbox Live 使用者打造的玩家分數值的完整性。 沒有每位使用者只能有一個玩家分數 ！ 在現有的 Xbox Live 平台，例如 Xbox 360、 Xbox One 或 Windows 10 的使用者獲得任何玩家分數都會計算到該使用者的單一玩家分數。

當使用者解除鎖定玩家分數成就時，Xbox Live 自動增加使用者的玩家分數所設定的金額。

有一些的限制的項目可能會提供玩家分數作為其成就上報酬。 在上看到的原則文件 https://developer.xboxlive.com/最新的資訊。

#### <a name="art"></a>美工圖案 ####
您有一些有趣的概念美工圖案的繪製在項目的起始階段早期的設計工具嗎？ 您有可以裝飾您的中樞應用程式，當播放程式瀏覽的美觀、 高解析度的映像嗎？ 或許您的應用程式支援多個面板？ 與封面 reward 中，您可以電源清、 美觀體驗在您的標題和更新版本，必須獲得您的玩家。

高解析度的概念封面、 早期設計繪圖，而建立的藝術資產及其他數位作品資產可能作為報酬中提供給使用者，用於解除鎖定的成就。 這些資產可能會顯示在 Xbox 一個儀表板體驗，並可以顯示在小幫手體驗，只要查詢成績追蹤服務來擷取相關的中繼資料。

#### <a name="in-app-rewards"></a>應用程式內獎勵 ####
我們為了讓開發人員更多的彈性和控制力成就提供獎勵引進應用程式內獎勵。 應用程式內獎勵可讓您使用的成就，直接向您的使用者提供自訂的遊戲中獎勵，而不需要更新您的標題。 您只需要設定成就 reward 與程式碼、 識別碼或片語會辨識您的標題，以及當使用者解除鎖定的成就，Xbox LIVE 會將傳送該程式碼到您的標題，藉此通知傳遞給使用者的報酬的標題。

Reward 本身是由開發人員。 Reward 概念包括：

* 額外遊戲中貨幣或點
* 特殊字元、 武器或對應的存取權
* 暫存的體驗乘數。

### <a name="configuring-in-app-rewards"></a>設定應用程式內獎勵 ###
設定應用程式內 reward，分別是相當簡單。 成就擁有者必須提供 reward 名稱、 reward 描述和獎勵值除了 reward 圖示。 這個獎勵值取決於開發人員，而且必須是項目，其中一個遊戲可以解譯及正確處理或使用者可以輸入標題專屬 reward 兌換體驗的一部分。

遊戲可解譯獎勵值的範例可能是 5 位數的數字或特殊字串的遊戲，或該遊戲的服務知道對應到特定的遊戲中項目。 開發人員可能想要利用標題受控儲存體 (TMS) 服務，讓您輕鬆地加入新的獎勵值經過一段時間，遊戲將會了解如何讀取。

使用者必須提交獎勵值的範例可能是特殊的程式碼或使用者輸入到 附屬應用程式內或開發人員網站上的標題中的兌換體驗的字串。

### <a name="redeeming-in-app-rewards"></a>兌換獎勵應用程式 ###
當使用者兌換在遊戲 reward，應用程式內報酬才會生效。 標題必須知道使用者已取消鎖定讓標題可以正確地傳遞給使用者的報酬設有應用程式內 reward 成就。 若要這樣做，標題應該執行下列作業：

1. 查詢標題啟動或從擱置，若要查看哪些解除鎖定的成果有應用程式內獎勵，並取得 reward 程式碼，每個標題繼續成就服務。 這一律應該為了確保可攔截任何可能已解除鎖定標題未執行時，或在另一個主控台上的成就。  

    若要查詢，您可以使用 RESTful 成就 Uri 或 Api Microsoft.Xbox.Services.Achievements 命名空間中。

2. 註冊您的成就的其中一個已解除鎖定時收到通知。 雖然可能適合大部分的項目，這是選擇性的。 請注意，標題會才會收到此通知標題確實正在執行時解除鎖定。 這是上一個步驟為何如此重要的另一個原因。

   若要註冊的成就通知，使用 AchievementNotifier.GetTitleIdFilteredSource 方法。 本主題包含範例程式碼，說明如何註冊。
