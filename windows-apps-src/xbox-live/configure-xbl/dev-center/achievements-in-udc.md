---
title: 成就 2017
description: 說明如何設定成就，以及在合作夥伴中心，以提供獎勵。
ms.assetid: ''
ms.date: 11/10/2017
ms.topic: article
ms.localizationpriority: medium
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，其中一個 xbox、 udc，通用的開發人員中心
ms.openlocfilehash: 9ef2365ee560e273108c38570697d599adde4c0b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655573"
---
# <a name="configure-achievements-2017-in-partner-center"></a>在合作夥伴中心設定成積 2017

> [!IMPORTANT]
> 僅適用於成就ID@Xbox或受管理的合作夥伴。 不支援在 Xbox Live 創作者計劃參與的遊戲。

您可以使用[合作夥伴中心](https://partner.microsoft.com/dashboard)若要設定[成就 2017年](../../achievements-2017/simplified-achievements.md)與您的遊戲相關聯。 加入新的成就，透過下列方式：

1. 瀏覽至您的標題，位於的成就區段**Services** > **Xbox Live** > **成就**。
2. 按一下 **新的成就**按鈕，然後填寫表單。  完成後，按一下**儲存**。

![若要在合作夥伴中心建立新的成就的螢幕擷取畫面](../../images/dev-center/achievement-table.png)

## <a name="description"></a>描述
[描述] 部分是成就的您可以在此輸入您，例如名稱和鎖定/解除鎖定的描述的基本概念。 您可以加入當地語系化支援成就藉由存取**當地語系化字串**服務中的 [組態] 區段[合作夥伴中心](https://partner.microsoft.com/dashboard)。

![在合作夥伴中心內設定新的成就時的描述欄位的螢幕擷取畫面](../../images/dev-center/achievements-2.png)

**成就名稱**欄位是公開的成就的名稱。

**鎖定描述**欄位是播放器將會看到當他們有不解除鎖定成就的描述。 它具有長度限制為 100 的字元。

**解除鎖定的描述**欄位是播放程式將會看到，一旦他們已解鎖成就的描述。 它具有長度限制為 100 的字元。

## <a name="details"></a>詳細資料
詳細資料 區段用來將重要的資訊，例如影像、 成就的型別、 玩家分數 reward （如果有的話），直到解除鎖定是否隱藏成就。

![在合作夥伴中心內設定新的成就時的詳細資料欄位的螢幕擷取畫面](../../images/dev-center/achievements-3.png)

**影像圖示**欄位是將與成就一起顯示的映像。 它必須是 1920 x 1080 png。

**基底成就**可用於您的玩家何時已釋放初始的遊戲。 **非基底成就**初始遊戲已發行 （例如使用新的 DLC 內容） 之後就可以使用。

**玩家分數**欄位是當您的成就解除鎖定時，會將獎品的玩家分數點的數量。 每個成就可以獎勵之間 0-200 點。  

**公用**成就是到所有播放程式，不論是否在已解鎖成就或不可見。 **祕密**成就會隱藏，直到已解除鎖定。

**成就深層連結**可讓您從成就，可讓您連結至您在何處可以獲得成就的遊戲中取得的參數。 深層連結取得的 API 回應中傳回。 指定的 URL 必須包含`ms-xbl-{titleID}://`前置詞。

> [!TIP]
> 成就的深層連結需要您的遊戲十六進位 TitleId。 您可以上找到[Xbox Live 的安裝程式](xbox-live-setup.md)畫面[合作夥伴中心](https://developer.microsoft.com/dashboard)。

## <a name="additional-rewards"></a>其他的獎勵
在某些情況下，您可能想要提供的遊戲中 reward 或美工圖案，當播放程式會解除鎖定的成就。 您可以定義獎勵 （如果有的話） 中的成就與相關聯**額外的獎勵**一節。 成就可以包含兩個額外的獎勵-每種獎勵類型的其中一個。 您可以閱讀更多資訊[成就獎勵](../../achievements-2017/achievement-rewards.md)文章。

若要建立新的獎勵，按一下**新增 Reward**按鈕**其他獎勵**區段，然後填寫表單。

![加入合作夥伴中心成就的獎勵的螢幕擷取畫面](../../images/dev-center/achievement-reward.png)

### <a name="reward-details"></a>Reward 詳細資料
填寫 Reward 詳細資料，以建立新的獎勵的關聯。 完成後，按一下**新增**。

![在合作夥伴中心設定成就的獎項詳細資料的螢幕擷取畫面](../../images/dev-center/achievements-5.png)

有兩種類型的可建立的成就獎勵。 這些報告包括：

1. **封面**可以使用型別，如果您想要提出回饋播放程式的一些事項，例如高解析度的概念封面、 早期設計繪圖，而建立的藝術資產或其他數位作品。 藝術資產會顯示在 Xbox One 儀表板，並可以也會顯示在小幫手體驗中藉由查詢成績追蹤服務。
2. **遊戲中**可以使用型別，如果您想要使用自訂的遊戲中獎勵獎勵播放程式，而不需要更新您的標題。 某些可能的情況下會額外遊戲中的貨幣/點或特殊字元、 武器或對應的存取權。

**顯示名稱**欄位是播放器將會看到 reward 的名稱。 它有 57 字元限制。

**描述**欄位是 reward 播放程式會看到的而且必須包含的任何兌換指示的描述。 它有 90 個字元限制。

**映像**或是**封面**欄位是 reward 相關聯的映像。 如果類型設為作品，這會是它們會獲得回報的圖檔。 否則它將代表他們將會收到遊戲中 reward。 映像必須 1920 x 1080 png。

**遊戲中的值**欄位才會顯示您選取 reward 種**遊戲中**。 它用來指定傳遞至您的遊戲程式碼可以用來解除鎖定在遊戲 reward 的值。
