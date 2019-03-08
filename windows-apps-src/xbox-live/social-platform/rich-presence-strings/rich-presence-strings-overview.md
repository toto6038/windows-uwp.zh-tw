---
title: 豐富顯示狀態
description: 了解 Xbox Live 的豐富的目前狀態可協助提升您的標題。
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，完整的目前狀態
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0d0189954ed8fa9a0bc7651d6a90b9789c388
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604113"
---
# <a name="rich-presence"></a>豐富顯示狀態

藉由使用豐富的組態選項，您的遊戲可以播放程式正在做什麼現在所通告的。 您的遊戲比方說，可以使用豐富的組態選項的字串，例如顯示所有的玩家的遊戲的玩家，狀態*離開*。 會顯示連接到 Xbox Live 的玩家以豐富的目前狀態資訊。 在理想情況下，豐富的組態選項字串告知 Xbox Live 的玩家，播放程式做什麼，以及其中遊戲中播放程式會執行。 Xbox 360 上，但新的實作會遵循一項服務方案為娛樂豐富的組態選項字串的概念是 Xbox One 上相同。 在本節中的主題描述如何設定您豐富的組態選項的字串以及如何設定使用者播放您的標題的字串。


## <a name="definitions"></a>定義

**列舉型別**  
列舉型別是維度的一份某些遊戲中。 這些遊戲中維度的範例包括武器、 字元類別、 對應等等。 我們想要看到一份可能武器，在您的遊戲中，一份所有可能的字元類別或對應，依此類推。

**地區設定字串組**  
每個可能的豐富的組態選項字串必須以指定字串可以/應使用何種地區設定相關聯的地區設定。 每個列舉型別也會有一組以及地區設定字串組。

**String-set**  
字串集合組成群組的地區設定字串組。 此設定會定義所有可能的地區設定的豐富的組態選項字串的可能值或列舉類型的所有可能的地區設定可能的值。

**易記名稱**  
有兩種類型的易記名稱：

**豐富的組態選項的字串**  
字串集合的易記名稱是字串的用來參考字串組形式的唯一識別碼。

**列舉型別**  
這些易記名稱會用來唯一識別特定的列舉型別，例如武器列舉或字元類別列舉型別。


## <a name="in-this-section"></a>本節內容

[豐富的組態選項設定](rich-presence-strings-configuration.md)  
如何設定豐富的組態選項，以用於您的標題。

[正在更新字串豐富的組態選項](rich-presence-strings-updating-strings.md)  
如何從您的標題更新豐富的目前狀態的字串。

[豐富的組態選項的最佳作法](rich-presence-strings-best-practices.md)  
在您的標題，使用豐富的組態選項的最佳作法。

[豐富的組態選項的原則和限制](rich-presence-strings-policies-and-limitations.md)  
在您的標題中使用豐富的組態選項的相關原則。

[豐富的組態選項的附錄](rich-presence-strings-appendix.md)  
其他範例和詳細資料平台與豐富的組態選項。

[程式設計 Xbox Live 的豐富的組態選項](programming-rich-presence.md)  
示範如何使用 Xbox Live 的豐富的組態選項。
