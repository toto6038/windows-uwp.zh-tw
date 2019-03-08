---
title: Xbox One 多人遊戲工作階段目錄
description: 了解如何建立多人連線的工作階段使用 Xbox Live Mutliplayer 工作階段目錄 (MPSD) 服務。
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 52b363492d1cd17ae54ae5d23d04371c5adf73ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606813"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Xbox One 多人遊戲工作階段目錄

本主題提供使用新的 Xbox 一個多人遊戲工作階段目錄 (MPSD) 服務的多人連線的工作階段建立的概觀。 本文件主要導向 Xbox One 標題開發人員提交他們的工作階段範本直接到 Xbox 開發入口網站 (XDP)。 MPSD 服務可以使用來設定合作夥伴中心，但不是著重於這篇文章中。 它被要熟悉詞彙和概念 MPSD 組態中，使用方式，與相關聯和疑難排解的多人連線的工作階段。

## <a name="revision-summary"></a>修訂摘要

用戶端 XSAPI (Xbox 服務 API) 程式庫目前使用合約版本 104 MPSD 服務進行通訊時。 這個版本的文件更新合約版本 107，這是最新的合約版本 MPSD 服務所支援的工作階段範本結構描述。 一節摘要說明在版本 104 和 107 之間的變更[合約結構描述更新](#_Contract_schema_update)。

請注意，合約版本 104 所撰寫的範本必須進行更新，如果它們都同樣重新發行 107。 不過，服務都具有回溯相容性讓您可以繼續使用目前的程式庫，將會更新在未來的 XDK 版本。

先前的修訂版本，此文件的更新伺服器工作階段的相關資訊，並增加新的區段，Xbox Live 服務 Api 和 RESTful 服務呼叫的相關。 此外，資料表之查詢的工作階段狀態資訊一節中找到已更新，並修改的服務品質 (QoS) 的範本 區段。

## <a name="introduction"></a>簡介

Xbox One 上多人遊戲工作階段是安全的文件位於雲端上多人遊戲工作階段目錄 (MPSD)，而這份文件代表一群人玩遊戲。 具體而言，多人連線的工作階段已符合下列條件：

-   每個工作階段都有唯一的 URI。

-   工作階段的文件包含啟動載入資料，以及遊戲伺服器資訊的目前成員遊戲的設定。

-   項目建立並管理他們自己的工作階段。

-   工作階段可讓其成員之間的連線。

多人遊戲工作階段目錄可以跨所有用戶端集中遊戲工作階段中繼資料。 它提供所需的工作階段的相關協助您設定用戶端之間的安全裝置關聯的基本資訊。 它也提供基本的第一個開機中繼資料的用戶端啟動傳遞更特定的遊戲資料之前，先連接至遊戲。 處理程序生命週期管理 (PLM) 和 Xbox One 上的應用程式的工作切換本質，MPSD 可確保用戶端有正確的資訊連絡對等電腦與伺服器識別為屬於作用中的遊戲工作階段和座標使用 shell 和主控台作業系統保留、 啟動及管理玩家遊戲工作階段的存留期。

## <a name="terminology-used-in-this-document"></a>本文件中使用的術語

| 詞彙                 | 定義                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 多人連線的工作階段  | 安全的文件位於 Xbox Live 的雲端，並代表的是 （或將） 的使用者群組連接在一起，同時在 Xbox One 上播放的標題。 多人遊戲的所有層面，例如配對、 合作對象，在進行聯結等等 — 利用多人連線的工作階段。 |
| 遊戲的工作階段         | 這是實際的遊戲工作階段，以 MPSD，一起玩的使用者會公開。 所有的多人遊戲情節最後都會進入遊戲的工作階段。                                                                                                                               |
| 符合票證工作階段 | 這是用來追蹤在配對的相符項目票證提交的工作階段。                                                                                                                                                                                                                 |
| 非使用中的播放程式      | 玩家可以在工作階段中已設定為非使用中狀態。 標題，將使用者設定為非使用中狀態時，遊戲會受到限制，暫止，或否則非使用中所定義的標題。                                                                                     |

## <a name="the-multiplayer-session-directory"></a>多人連線的工作階段目錄

MPSD 有助於，並協助協調線上的播放程式之間的工作階段資訊的標題。 可以有不同類型的工作階段建立來完成不同工作的多人連線遊戲。 下表說明如何這類工作已完成在 Xbox 360 上與如何在 Xbox One 上完成它們的差異。

| 函式或工作                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **取得遊戲的工作階段資訊**     | **XSessionGetDetails**， **XSessionSearchByID**，或您自己的追蹤。                                              | 從服務要求工作階段資訊。                                                          |
| **移轉主機**                     | 當有需要標題就會呼叫**XSessionMigrateHost。**                                                           | 不需要建立新的工作階段，請只要指派新的主控件的 SessionID。                                  |
| **管理多個播放程式工作階段**  | 難處理一次的多個工作階段。 例如， **XNetReplaceKey**與**XNetUnregisterKey**。 | 服務為基礎的工作階段，讓清除器管理一個工作階段，並輕鬆地處理多個工作階段。    |
| **處理簽出，並中斷連線** | 必須處理中斷連線並登出以不同的方式，與**XCloseHandle**或是**XSessionDelete**分別。 | 簡化登挑出集中式的服務，並中斷連線的處理，並在遊戲的組態中所設定的逾時。 |

### <a name="session-patterns"></a>工作階段模式

-   遊戲的工作階段

    -   玩家的 XUIDs、 與安全的裝置位址資料屬性狀態的工作階段。 這會視為實際的遊戲工作階段。

    -   可以是對等、 對等電腦來裝載、 對等-伺服器或混合式。

-   符合票證工作階段

    -   提交至來尋找其他玩家玩配對的工作階段。 請注意，遊戲的工作階段也可能票證工作階段中，如果要尋找更多玩家的遊戲。

-   伺服器工作階段

    -   遊戲的工作階段建立，並使用透過 Xbox Live Compute 的處理。

[圖 1] 說明 MPSD 工作階段，其中藍色方塊代表 MPSD 工作階段，而紅色的方塊是使用它們的服務的使用方式。

圖 1. MPSD 工作階段使用。

## <a name="mpsd-sessions"></a>MPSD 工作階段

工作階段維護兩個相關實體清單：

1.  成員 （使用者） 加入或已獲邀加入工作階段。

2.  伺服器 （雲端伺服器或專用的伺服器） 已加入工作階段。

每個實體都有：

1.  在建立期間設定的常數值。

2.  可變動的屬性。

3.  僅限讀取中繼資料。

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>Xbox Live 服務 Api 和 RESTful 服務呼叫

有兩種方式可與 Xbox 一個工作階段和配對服務的介面。 第一種方式是使用標準 HTTPS 呼叫 RESTful Xbox Live 服務 Uri。 這可讓呼叫和配合這些服務，根據其伺服器和遊戲的組態項目的彈性。 這些 Uri 的清單可在[Xbox 一個 Development Kit (XDK) 文件](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx)下 「 Xbox Live RESTful 服務的參考。 」[1]

第二種方法是使用 Xbox Live 服務 Api，其做為 RESTful 服務 Uri 的包裝函式。 這些行為允許更傳統的方法，以在程式碼中使用 Api，而不需要處理的每個呼叫的 HTTPS 流量。 Xbox Live 服務 Api 的原始程式碼隨附與 Xbox Development Kit (XDK) 和可修改，並併入您的標題，依需要。 這些範例均使用 Xbox Live 服務 Api 來撰寫。 Xbox Live 服務 Api 的詳細資訊可在 Xbox One [XDK 文件](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx)下 「 Xbox Live 的服務參考 」。[2]

### <a name="mpsd-sessions-and-templates"></a>MPSD 工作階段和範本

使用內嵌透過 Xbox 開發入口網站 (XDP) 的範本會建立 MPSD 工作階段。 範本會定義所建立的工作階段架構的 JSON 文件。 範本會提供新的工作階段中的常數。

下列摘錄自[Player Rendezvous 程式碼範例](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx)是範本的組態範例。

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

比對的票證工作階段應該用於遊戲工作階段範本，使用中的 QoS 逾時值設定其**memberInitialization**物件。

圖 2. 範例 hopper。

![](../../images/whitepapers/mpsd_image1.png)

接下來的程式碼摘錄是端對端遊戲工作階段範本 (標題管理 QoS) 的範例。

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

這是對等-伺服器工作階段和原始範本的範例。

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

下列程式碼示範使用智慧比對的相符項目票證工作階段範本。

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>檢查哪一個範本已針對您 SCID

#### <a name="session-templates"></a>工作階段範本

可從 MPSD 擷取存在於 SCID，工作階段範本清單，以及特定的工作階段範本的詳細資料：

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>工作階段狀態資訊的查詢

工作階段可以在服務組態和工作階段範本層級進行查詢：

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

根據預設，這會傳回最多 100 個的非私用工作階段。 查詢可以使用這些查詢字串參數進行修改：

| 查詢字串             | 效果                                                                                                         | 描述                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| keyword=foo              | 只包含 工作階段中的關鍵字"foo"。                                                             |                                                                                                     |
| XUID=123                 | 只包含"123"的使用者的工作階段。                                                               | 根據預設，使用者必須是作用中要包含的工作階段。                                  |
| *reservations*=**true** | 包含工作階段的使用者會設定為保留的播放程式，但尚未加入成為作用中的播放程式。 | 只有當查詢自己的工作階段，或是查詢特定使用者的工作階段伺服器對伺服器時。 |
| *inactive*=**true**      | 包含工作階段的使用者已接受，但不在播放。                                     | 使用者是 「 準備 」，但不是 「 作用中 」 的工作階段計數為非作用中。                           |
| *private*=**true**       | 包括私用的工作階段。                                                                                      | 只有當查詢自己的工作階段，或是查詢伺服器對伺服器時。                            |
| *visibility*=open        | 只包含 工作階段保持 「 開啟 」。                                                                         | 如果設定為有 「 私用 」，「 私用 = true 」 也必須設定指示詞。                                 |
| *take*=5                 | 傳回最多五個工作階段。                                                                                    | 必須是介於 0 到 100 之間。                                                                          |

結果是 JSON 陣列的工作階段的參考。 某些工作階段資料是包含的內嵌的。

**請注意**關鍵字篩選器、 XUID 篩選，或兩者，必須包含每個查詢。

設定*私人*（它會傳回私用的工作階段） 或*保留項目*（這會傳回工作階段使用者尚未加入） 到**true**呼叫者必須具備工作階段中，或呼叫端的 XUID 宣告，以符合 XUID 篩選條件，在 URI 中的伺服器層級存取。 （不論是否實際存在的任何這類工作階段），否則會傳回 403 禁止 」。

下列程式碼的摘錄顯示查詢的回應範例。

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>工作階段範本屬性

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>合約結構描述更新

如已在這份文件開頭所述，最新的工作階段範本合約版本是 104 的 107，其中導入了一些變更結構描述從舊版。 如果它們都同樣重新發行 107 更新必須為合約版本 104 撰寫的範本。 以下是變更的摘要。

-   **/constants/system/managedInitialization**已重新命名為 **/constants/system/memberInitialization**。

    -   **AutoEvaluate**欄位已重新命名為**externalEvaluation**和它極性的變更，除了**false**保持預設值。

    -   預設值**membersNeededToStart**從 2 變更為 1。

    -   預設值**joinTimeout**從 5 秒變更為 10 秒。

    -   **MeasurementTimeout**從 5 秒變更為 30 秒的預設值。

-   **/constants/system/timeouts**會移除，而逾時重新命名且在重新定位 **/常數/系統**。

    -   **保留**逾時變成**reservedRemovalTimeout**。

    -   **非作用中**逾時變成**inactiveRemovalTimeout**和其新的預設值為 0，而不是 2 小時。

    -   **就緒**逾時變成**readyRemovalTimeout**。

    -   **SessionEmpty**逾時變成**sessionEmptyTimeout**。

-   **/constants/system/capabilities/gameplay**必須指定為 **，則為 true**代表實際的遊戲 （相對於協助程式工作階段，例如比對] 和 [大廳的工作階段），若要讓新的播放程式的工作階段和報告的評價。

### <a name="system-objects"></a>系統物件

每個工作階段的文件中的系統物件有固定的結構描述，強制執行由 MPSD 解譯。

在 PUT 要求的主體內，系統物件會進行驗證，並合併一樣的自訂物件。 但不同於自訂物件，它們會合併的系統進一步驗證及處理物件之後，根據這些結構描述。

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>Timeouts

可以變更工作階段計時器和其他外部事件。 **逾時**MPSD 中的物件會提供管理工作階段存留期和成員的基本機制。

**NextTimer**工作階段的欄位中提供的下一個計時器的時間。 用戶端可以使用這項資訊挑選良好下一次輪詢間隔。 此值也會傳回**Expires** HTTP 標頭。

逾時，會指定以毫秒為單位。 允許零，且表示逾時應立即。 如果指定的逾時未指定，則會視為無限。 因為逾時都有預設值，工作階段範本應該明確指定無限的逾時的 「 null 」。

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

**/Constants/system/sessionEmptyTimeout**值會設定的毫秒數之後的工作階段會變成空白，便會被刪除。 預設值為零，表示工作階段將會立即刪除。 如果未指定值，空白的工作階段會無限期存留。

#### <a name="member-timeouts"></a>成員逾時

三個其他逾時內 **/常數/系統**控制的成員可以處於特定狀態的時間量：

-   **reservedRemovalTimeout**

    -   在逾時到期時，會刪除此保留項目。 預設值是 30 秒。

-   **inactiveRemovalTimeout**

    -   預設為非作用中的成員移除從兩個小時後的工作階段。

-   **readyRemovalTimeout**

    -   「 就緒 」 的成員還原為非使用中狀態之後三分鐘預設。

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>在工作階段的文件中的成員初始化

### <a name="member-initialization"></a>成員初始化


**MemberInitialization**物件可控制初始化階段，工作階段建立之後，及/或新成員加入此工作階段。 在逾時和初始化階段會自動追蹤工作階段，包括 QoS 度量，如果設定了任何計量。

這些逾時的覆寫工作階段的保留，且準備好的逾時，具有成員**initializationEpisode**屬性集。

範例：

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**圖 3。成員初始設定流程。**

每個成員初始化的三個階段會逾時：

1.  **joiningTimeout**

    -   時間 （毫秒），使用者必須加入此工作階段數量。 無法加入成員的保留區會移除。

2.  **measuringTimeout**

    -   成員可上傳其度量的時間量。 無法上傳度量單位的成員會標示*failureReason*的 「 逾時 」。

3.  **evaluationTimeout**

    -   成員，才能進行，並上傳評估決策的時間量。 如果不收到任何決策時，它會計算為失敗。

**externalEvaluation**

-   MPSD 如何根據工作階段範本中提供的需求自動 QoS 評估。 ExternalEvaluation 設為 false 時，將執行評估。 **externalEvaluation**必須是 **，則為 true**當**evaluationTimeout**設定。 如果有兩個對等或等至主應用程式的需求，您仍然應該設定**externalEvaluation**要**false**才會有自動完成初始設定工作階段。

**membersNeededToStart**

-   這是要有 「 初始化 」 的成員保留的最小數目:"true"，並傳遞針對自動評估 QoS，才會成功。

### <a name="initialization-episodes"></a>初始化集數


當**memberInitialization**物件設定時，MPSD 會集中所進行的初始化階段。 建立工作階段，而且會包含在範本中定義的所有階段時，會啟動第一集。

受邀或加入影片已經在執行時的任何新成員將會標示為下一步 的時段。 影片的狀態或**memberInitialization**一般情況下可能會從擷取**初始化**工作階段的物件。

**請注意**注意，已完成初始設定之後，移除此物件。

範例：

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

階段會從聯結至測量來評估。 如果影片失敗，則階段設為*失敗*，因此無法初始化工作階段。 否則，當初始化影片完成時，**初始化**已移除的物件。

初始化失敗也可以追蹤每個成員。 它們是設定時轉換出聯結或測量階段，如果未通過此成員。

範例：
```
"initializationFailure": "latency",
```
依優先順序的詳細資訊，此屬性的值無法設定為*逾時，延遲、 bandwidthdown、 bandwidthup、 網路、 群組*，或*影集*。 網路值表示的網路組態及/或條件 (例如衝突的網路位址轉譯\[NAT\]) 防止 QoS 計量量值。 聯結的一端唯一可能的值是*群組*。 （從聯結的逾時，此保留項目已移除）。

如果**memberInitialization**設定和成員已新增 「 初始化 」: 為 true，此值設為該成員會參與初始化一集。 成員新增至新的工作階段，在建立時，使用值為 1，並為初始化時段結束時，它會移除。
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>符合票證工作階段

當 MPSD 工作階段用做比對的票證工作階段時，會使用一些特殊的工作階段屬性和常數。

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

當配對服務會將使用者新增至工作階段時，它會提供一些有關如何及為什麼它們符合的工作階段中的內容**matchmakingResult**欄位。

```
"matchmakingResult": {
```

這是使用者的一份**serverMeasurements**從配對的工作階段。

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>品質 (QoS) 服務範本的

對於遊戲工作階段範本，值可以加入，可通知 MPSD 協調網路層級與主控台社交平台。 這項協調的目的是建立工作階段時，使用者會收到通知，遊戲便準備好加入之前，驗證連接狀態的品質。

下列程式碼摘錄是 QoS 的對等電腦來裝載遊戲工作階段範本的範例：

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

此程式碼的摘錄是 QoS 的端對端遊戲工作階段範本的範例：

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>QoS 工作階段範本屬性

如果**memberInitialization**物件設定時，工作階段所預期的用戶端系統或執行工作階段建立後的初始設定及/或做為新成員加入此工作階段的標題。

在逾時和初始化階段會自動追蹤工作階段，包括 QoS 度量，如果設定了任何計量。

這些逾時的覆寫工作階段的保留，且準備好的逾時，具有成員**initializationEpisode**屬性集。

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

與 QoS 的遊戲工作階段需要連線功能。 如果未不指定任何計量，其預設內容會需要滿足 QoS 需求。 如果有指定，它們必須足以滿足 QoS 需求。

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

下列的臨界值套用至所有成員的工作階段中每個成對連線：

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

下列的臨界值適用於每個連線的主機候選項目從：

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

下列潛在伺服器連接字串應該被評估 （請注意，連接字串必須是小寫）：

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

這些旗標控制成員狀態並**activeTitle**，並不互斥 (，即同時設為錯誤 **，則為 true**)。 分別**false**等同於 「 不存在 」。 預設狀態為 「 作用中 」，亦即，兩者都不存在。

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>QoS 階段和工作階段初始化的詳細資料

MPSD 會追蹤，並報告製作遊戲的 QoS 結果，範本完成成員初始設定之後。 將以表示此作業的進度*初始化*物件中的工作階段的文件中所述[成員初始化](#_Member_initialization_in)上一節。 *初始化*物件具有*階段*屬性，表示目前所處階段的初始設定。 從階段進展*聯結*要*測量*來*評估*。

-   如果初始化影集\#1 失敗，則會設定階段*失敗*，因此無法初始化工作階段。 否則完成初始化影片時，「 初始化 」 移除的物件。 如果**externalEvaluation**設為**false**，評估階段會略過。 如果沒有**度量**也不**measurementServerAddresses**時，就會略過測量階段。

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   主機候選項目是依喜好順序列出的裝置權杖。 在設定之後*測量*階段初始化一集\#1，否則**peerToHostRequirements**設定和 **/properties/system/host**未設定。 之後會清除這些 **/properties/system/host**物件集。

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   *玩家代號*將只會設定屬性，如果成員已接受，而且找不到該成員的玩家代號宣告。

```json
"gamertag": "stacy",
```

-   *DeviceToken*成員上傳安全的裝置位址時，設定屬性。 它是不區分大小寫的字串可用於比較相等性。

```json
"deviceToken": "9f4032ba7",
```

-   *保留*使用者執行其第一個 PUT 要求的工作階段的文件之後，就會移除該值。 當玩家保留時，這表示它們已獲邀加入遊戲工作階段，但不是接受，也必須評估其連線。

```json
"reserved": true,
```

-   如果成員是作用中， *activeTitleId*是成員為作用中，以十進位的標題。

```json
"activeTitleId": "8397267",
```

-   這個屬性是指使用者已加入工作階段的時間。 如果*保留*是 **，則為 true**，然後*joinTime*會保留項目進行的時間。

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   如果這個成員是在屬性/系統/turn 陣列中，否則不，存在。

```json
"turn": true,
```

-   *InitializationFailure*轉換出聯結或測量階段，如果成員未成功完成階段時，設定屬性的成員物件上。 依優先順序的詳細資訊，它無法設定為*逾時*，*延遲*， *bandwidthdown*， *bandwidthup*，*網路*，*群組*，或*影集*。
    *網路*值表示的網路組態及/或條件 (例如衝突的網路位址轉譯\[Nat\]) 防止 QoS 計量量值。 聯結的一端唯一可能的值是*群組*。 （從聯結的逾時，此保留項目已移除）。*影集*失敗的 「 評估 」 階段之後未在聯結或測量失敗初始化一集的所有成員上設定值。

```json
"initializationFailure": "latency",
```

-   如果**memberInitialization**設定和成員已新增 「 初始化 」: 為 true，此值設為該成員會參與初始化一集。 使用值為 1 的成員新增至新的工作階段，在建立時間。 為初始化時段結束時，會移除它。

```json
"initializationEpisode": 1,
```

-   *下一步*屬性代表工作階段中的下一個成員的索引值。 它會與相同的值*下一步*屬性中**membersInfo**物件如下，若沒有新增更多的成員。

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Xbox 雲端運算的工作階段

Xbox 雲端運算的工作階段包含工作階段無法連線到遊戲伺服器使用的不區分大小寫的連接字串的已排序的清單。 一般而言，項目應該在清單中，使用第一個字串，但複雜的項目可以使用自訂的機制 （例如 Xbox Live Compute) 選擇其中一個其他 （比方說，根據負載）。

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

**/servers/{server-name}/properties/system **

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>未經處理的工作階段和自訂的標題屬性

工作階段可以用來儲存多人遊戲相關的自訂環境維護資訊 （中繼資料） 中。 遊戲資料或已儲存的資訊，都應該儲存在 TMS + +。

### <a name="property-bags"></a>屬性包

每個以上的物件標示為屬性包包含兩個選擇性的內部物件、 系統和自訂。

自訂的物件可以包含任何的 JSON。

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>作用中成員衰減

當 MPSD 偵測到使用者無法再參與標題時，作用中的成員會自動標記非作用中。 這種情形，例如，如果有逾時的使用者記錄。 這項機制是只為防護網;亦即，標題是以主動將成員標記為非作用中 （或移除工作階段） 仍然需要時成員離開，或切換離開標題，成為囓登 out 或其他方式。

## <a name="faq-and-troubleshooting"></a>常見問題和疑難排解

### <a name="how-do-i-call-mpsd-"></a>我要如何呼叫 MPSD？

使用憑證驗證： 用戶端 sessiondirectory.xboxlive.com

範例：

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

使用 XToken 驗證： sessiondirectory.xboxlive.com

範例：

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>我要如何找出要使用何種 SCID、 工作階段範本和沙箱？

可以在標題 XDP 網站上找到此資訊。 如果您還沒有存取權 XDP 請連絡您開發人員的帳戶管理員，取得您的資訊可協助人員。

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>是否有我可以比較要求本體範例，我自己嗎？


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>呼叫 MPSD 時收到 404 錯誤。

收集 Fiddler 追蹤，可協助取得詳細資訊，然後執行下列：

1.  檢查一般的 404 訊息 HttpResponse 主體的一部分傳回的訊息：

    -   *要求的服務設定為無效或未設定工作階段*。 請確認正在使用 SCID 正確無誤。

    -   *找不到要求的工作階段*。 確認在建立工作階段，而且工作階段範本正確，然後才能擷取它。 您可以使用 PUT 呼叫來建立工作階段。

2.  請檢查您正在使用的 URI。

3.  重新啟動主控台和/或再試一次新的使用者。

4.  搜尋[娛樂開發人員論壇](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx)錯誤程式碼或其他可能的解決方案。

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>呼叫 MPSD 時收到 403 錯誤。

這通常是權限或範圍問題。 收集 Fiddler 追蹤，可協助取得詳細資訊，然後執行下列：

1.  檢查訊息的常見 403 訊息的 HttpResponse 本文一部分傳回：

-   * 您無法存取的要求的服務組態。 *

    -   請確認您使用的帳戶具有存取權的沙箱。

    -   請確認您是在正確的沙箱中。

    -   如果您是使用憑證驗證，並收到此錯誤，請連絡您的補西牆。

-   *您無法存取要求的工作階段。私用工作階段只可以讀取的工作階段的成員。*

    -   您嘗試存取具有 「 私人 」。 可見性的工作階段 只有在工作階段中的成員可以讀取工作階段的文件。

-   *要求本文不能包含現有成員的參考，除非主體的驗證包括伺服器。*

    -   您無法加入至工作階段的另一位使用者代表使用者。 您只能邀請。 索引設定為 「 保留\_&lt;數字&gt;」 邀請播放程式。

### <a name="i-am-getting-a-412-precondition-failed-error"></a>我遇到 「 412 前置條件失敗的錯誤 」。

如果工作階段已經存在，這會傳回 412 前置條件失敗：

> PUT 的 serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/快速/工作階段/foo HTTP/1.1 內容類型： application/json-If-none-match: \*

如果工作階段的 etag 不符合 If-match 標頭，這會傳回 412 前置條件失敗：

> PUT 的 serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/快速/工作階段/foo HTTP/1.1 內容類型： application/json If-比對：9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>我會收到錯誤，例如 405、 409、 503 和 400when 呼叫 MPSD。

收集的 Fiddler 追蹤，可協助取得詳細資訊，然後檢查 訊息傳回為 HttpResponse 主體的一部分。 這會提供足夠的資訊，找出並修正錯誤，或搜尋[娛樂開發人員論壇](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx)尋找可能的解決方案。

您也可以取得回應內容，如果您使用 Xbox Live 服務 Api 藉由設定**DiagnosticsTraceLevel**錯誤的屬性，這些輸出中的偵錯輸出的資訊，或者您可以使用**XboxLiveContextSettings.ServiceCallRouted**事件輸出至您的標題 UI 的多個範例所示。

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>什麼可以或應該變更範本中的 我的標題？

工作階段範本不是預設值，而是多個區域變數。 不過，您無法覆寫在範本中，已設定的常數雖然您可以新增它們。

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>我收到錯誤，指出我的工作階段未初始化。

存在於您的範本 （遊戲、 合作對象及符合票證案例，通常） 成員初始化時，您需要先確定 「 初始化 = true"設定足夠的成員保留項目 （啟動所需的成員） 將 QoS，若要修正此問題。

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>我的工作階段尚未建立，並收到 HTTP 204 錯誤。

這表示已加入至工作階段建立時沒有任何使用者。 如果沒有任何使用者工作階段在建立時，將不會建立工作階段。 請確定您在建立工作階段新增至少一名玩家。 專用的伺服器的情況下，取得的玩家何者嘗試建立比對，或誰會建立比對，並讓該使用者初始的播放程式中的相符項目。 或者您可以變更或移除**sessionEmptyTimeout**。

### <a name="when-should-i-poll-the-mpsd"></a>何時輪詢 MPSD？

您應該避免輪詢 MPSD 工作階段。 概括而言，您可以藉由設計您的程式碼會利用 MPSD 工作階段，只會針對初始建立網路連線的每個用戶端，以及重新建立用戶端或已中斷連線的用戶端的網路狀態的方式。 務必也利用 MPSD 的 etag 為基礎的同步處理原始物件，如此便不需要重新整理工作階段狀態，若要解決競爭情形。

您有一組 N 用戶端時，這些原則的一般應用程式，全部都需要連接在一起，並在對等網狀結構中播放。 而不是定期查詢的新成員的工作階段，每個成員可以加入此工作階段、 連接至的成員已在工作階段中，並假設任何較新加入者將會執行相同。 請參閱如何實作此範例的對談與 Player Rendezvous 範例。

有一些罕見的情況下，可能需要很短; 輪詢如果您認為您需要執行這項操作，請連絡您的開發人員帳戶管理員。
