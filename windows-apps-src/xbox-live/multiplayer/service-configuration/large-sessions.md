---
title: 大型工作階段
description: 了解如何使用大型的工作階段與 Xbox Live 多人遊戲平台。
ms.date: 07/11/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10，一個 xbox、 多人遊戲、 大型的工作階段、 新的播放程式
ms.localizationpriority: medium
ms.openlocfilehash: dcd7b27bc0aea8b8406e7eccb420140cf32c1ed5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618543"
---
# <a name="large-sessions"></a>大型工作階段

如果您需要可以處理 100 個以上成員的多人遊戲工作階段，您必須使用所謂的大型的工作階段。 此案例中是最常用來玩家的線上 (MMO) 遊戲和廣播 （大部分的成員是 spectators），但可能會有其他樣式的遊戲，以及應用程式。

在某些情況下，您可能也想要使用大型的工作階段，即使在處理較小的播放程式的群組。 如果您想要位於相同的工作階段，但不是一定知道彼此的存在如果它們不會彼此遇到遊戲中的多個播放程式，您可以使用大型的工作階段的 「 遇到"屬性。

大型的工作階段目前不支援所[Xbox 整合式多人遊戲 (XIM)](../xbox-integrated-multiplayer.md)或是[多人遊戲管理員 (MPM)](../multiplayer-manager.md)，因此您必須使用多人遊戲 2015 Api 來使用直接呼叫的多人遊戲服務目錄 (MPSD)。

略為不同於一般的工作階段處理大型的工作階段：

* 包含比一般的工作階段，但效率較少資訊。
* 支援最多 10,000 個成員。
* 您無法訂閱至大型的工作階段。
* 最近使用的播放清單成員的大型的工作階段沒有自動納入。

## <a name="recent-players"></a>新的播放程式

Xbox Live 功能之一是，當 Xbox Live 的玩家玩多人遊戲與新的人員，在遊戲後他們會看到這些播放程式在其儀表板**最近的玩家**清單。 如果播放程式會有絕佳的體驗，與新玩家在遊戲中，它們可能會想要玩並一次，或將它們新增為 friend。 擁有與玩家體驗不佳，他們可能會想要避免未來，及/或遊戲結束後報告不當的行為。

與一般的工作階段，Xbox Live 會自動將播放程式相同的工作階段中最近使用的播放清單。 不過，您使用大型的工作階段，如果您必須採取一些額外的步驟，以確保會正確填入新的播放清單。

## <a name="set-up-a-large-session"></a>設定大型的工作階段

若要設定工作階段一樣大，新增`“large”: true`功能一節中的工作階段範本。 可讓您設定`maxMembersCount`最多 10,000 個。 工作階段範本像下方應該會運作：

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>使用大型的工作階段

當寫入 MPSD 大型的工作階段，我們建議您不能超過 10 秒的寫入。 這通常是每位玩家的平均寫入每隔 2 分鐘的 1000年播放程式工作階段 （例如加入/退出）。

其他屬性不應保存在大型的工作階段。

### <a name="associating-players-from-the-same-large-session"></a>從相同的大型工作階段相關聯的播放程式

當您從 MPSD 擷取大型的工作階段時，成員的清單不會與回應，再回來實際上沒有任何方法，以取得完整的清單。 相反地，如果呼叫端工作階段中，其成員記錄將會在 「 成員 」 集合中，只有一個標示為 「 我 」 （就如同在要求中）。

這表示用戶端成員只能在工作階段中，更新自己的項目，而且將會依賴伺服器提供 Xbox Live 可用來建立關聯的播放程式播放一起在通用識別項。

有兩種方式，以指出該人員的工作階段中同時播放 （適用於更新信譽和新的播放程式狀態）。

#### <a name="1-persistent-groups"></a>1.持續性的群組

如果一群人保持在一起依據進行中，可能與人員，並從它，您可以提供群組的名稱 (例如，guid – 遵循相同的命名規則，與一般的工作階段)。  當每個成員出現，並會從群組，他們才可以新增或移除自己的 「 群組 」 屬性，也就是字串陣列的群組名稱：

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2.簡短遇到

如果兩個人員有簡短的單次發生，遊戲可以改用"遇到"陣列。 提供每遇到一個名稱，且之後發生，這兩個 （或所有） 參與者會將名稱寫入自己"遇到"的屬性：

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

您可以使用 「 群組 」 和 「 遇到 「 相同的名稱 – 例如，如果一名玩家"往來與 」 群組，群組中的人員不需要採取任何動作 （假設它們先前新增的群組名稱至其 「 群組 」），並已發生的人員會 upload"遇到 」 清單中的群組名稱。 這會造成個人以查看所有成員的群組為最新的播放程式，反之亦然。

遇到計為 30 秒內已群組的成員。 因為遇到會被視為一次性事件，是一律會立即處理，然後清除 從工作階段 」 遇到"陣列。  它永遠不會出現在回應中。  （「 群組 」 陣列隨身碟，直到改變或移除，或者成員離開工作階段）。
