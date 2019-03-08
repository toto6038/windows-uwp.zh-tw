---
title: 多人遊戲工作階段範本
description: 了解 Xbox Live 多人連線的工作階段範本。
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、windows xbox one，多人遊戲，工作階段範本
ms.localizationpriority: medium
ms.openlocfilehash: 0bbe4f6a3afe2d39fb18b4d4bad13e2aa91d246e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599433"
---
# <a name="multiplayer-session-templates"></a>多人遊戲工作階段範本

本主題提供的多人連線的工作階段範本的簡短概觀，並提供數個範本，您可以複製並修改您的多人遊戲工作階段的範例。

多人連線的工作階段範本是用來建立多人連線的工作階段的藍圖。 所有工作階段必須根據預先定義的範本建立。 範本可定義將會從範本建立任何工作階段相同的常數。 遊戲一旦遊戲從範本建立工作階段，可以加入和修改工作階段的其他資料，但無法修改範本中所定義的常數。

 如需詳細資訊，請參閱 <<c0> [ 工作階段概觀](../multiplayer-appendix/mpsd-session-details.md)。

可以從多人遊戲工作階段目錄 (MPSD) 擷取的工作階段範本套用到服務設定識別項 (SCID)，以及特定的工作階段範本的內容清單。


## <a name="about-session-templates"></a>關於工作階段範本

工作階段範本會使用相同的格式為 HTTP PUT 要求來建立或修改工作階段。 差別範本僅限於使用常數 （沒有成員、 伺服器或屬性）。 常數可以是任何工作階段的常數，包括自訂的區段和系統常數的完整範圍。

### <a name="session-template-versions"></a>工作階段範本版本

本主題中定義的工作階段範本會使用範本合約版本 107 建構的。 當您可以使用它們來建立新的範本，請確定 107 會輸入為合約版本。

如果您使用 XSAPI 並監看偵錯工具中產生的要求時，您可能會注意到要求，使用範本合約版本 105。 MPSD 有效地 「 升級 」 這些要求版本 107 在執行階段。

> **注意：** 這是允許工作階段範本中所用的要求中使用不同的合約版本。

如有必要，您可以從版本 104/105 變更工作階段範本，第 107 版本項目。 如需指示，請參閱中的移轉指示[常見的問題時調整您的標題 2015年多人遊戲](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md)。


## <a name="session-template-default-values"></a>工作階段範本的預設值

工作階段範本所建立的每個工作階段會啟動一份範本。 在工作階段建立時，就可以提供範本不包含的值。 在某些情況下會提供預設值，當未不設定任何其他值。 比方說，是一組預設的逾時，合約版本 107:

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
您可以強制保持未設定，藉由指定 null 值。 這會覆寫任何預設設定，並防止不需在工作階段建立時設定的值。 比方說，若要移除的工作階段空逾時，可讓工作階段來繼續無限期，即使沒有任何成員，請將下列新增至工作階段範本：
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **重要事項：** 透過範本所設定的常數不能透過變更寫入至 MPSD。 若要變更的值，您必須建立並提交新的範本，內含所需的變更。


## <a name="example-session-templates"></a>範例工作階段範本
本節說明針對不同用途和網路拓撲的數個工作階段範本的範例。 請選擇一個最適合您的用戶端之前，檢查每個範本。 可以然後複製並貼入您的服務組態，進行必要的變更後，潛在的範本。

### <a name="standard-lobby-session"></a>標準大廳工作階段
您可以使用下列範本做為入門範本，建立您的遊戲大廳工作階段：

* 變更`maxMembersCount`值到最大的數字，您想要支援大廳工作階段中的播放程式。  
* 如果您的標題不支援從不同的平台 （例如 Xbox One 及 PC） 的播放程式播放在一起，您可以移除`crossPlay`項目。  
* 您可以變更其他值，但下列的值是可靠的值開始如果您不確定您的需要。


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>標準的遊戲工作階段，而不需要配對
您可以使用下列範本做為入門範本，來建立您的遊戲的遊戲工作階段，如果您的遊戲不包含匿名的配對，而且不需要 100 個以上的成員。

請注意，只有新標準大廳工作階段範本從指定的值如下所示：
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>將配對加入遊戲工作階段範本，同時可讓多人遊戲服務處理的服務會檢查品質。

您可以新增下列`memberInitialization`json 項目至您的遊戲範本，以加入配對的支援。

當您建立 SmartMatch hopper 時，使用此範本做為目標的工作階段範本程式 hopper。

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>加入遊戲工作階段範本，其中的服務會檢查品質都由標題受控的資料中心中的配對。



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>用戶端-伺服器遊戲工作階段的基本工作階段範本

您可以使用此範本不會執行等式通訊，也不會使用 Xbox Live 計算，但改為具有用戶端連線到協力廠商裝載的伺服器項目的。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>大廳/SmartMatch 票證工作階段範本對等式網路功能

使用此範本來建立大廳工作階段或只是為了用來將一群播放程式傳送至配對的 SmartMatch 票證工作階段。 範本並不是用來設定遊戲工作階段。 它被適用於用戶端使用對等或對等電腦來裝載網路拓撲。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>品質 (QoS) 服務範本的

如果您的用戶端會使用配對，並評估 QoS，您必須將一些常數加入工作階段範本，以通知 MPSD 協調與用戶端管理來加入工作階段的使用者。 這項協調驗證之前通知使用者，遊戲便準備好開始的連接狀態的品質。 如果用戶端-伺服器遊戲是協調之前會驗證連線品質的播放程式在群組進入配對。

#### <a name="peer-to-host-game-session-template-with-qos"></a>使用 QoS 的對等電腦來裝載遊戲工作階段範本

下列範例會將對等電腦來裝載遊戲工作階段範本提供 QoS。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>使用 QoS 的端對端遊戲工作階段範本

以下是範例中的 QoS 的端對端遊戲工作階段範本。
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>QoS 用戶端-伺服器 (Xbox Live Compute) 大廳/配對的工作階段範本

您可以使用此範本來建立大廳工作階段或配對的工作階段，使用 QoS。 此範本不是用來設定遊戲工作階段。
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Crossplay Xbox One 和 Windows 10 之間的工作階段範本

若要啟用 crossplay Xbox One 和 Windows 10 之間的多人使用此範本。 UserAuthorizationStyle 功能可讓您存取 Windows 10。 選擇性 crossPlay 功能表示您的標題支援邀請等聯結進行中平台之間的互動。
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
