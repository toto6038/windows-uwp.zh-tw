---
title: 多人連線的工作階段詳細資料
description: 深入了解 Xbox Live 多人遊戲工作階段的詳細資料。
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，其中，多人遊戲 2015 中，工作階段、 mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 3eb92af2d478fa41150f375dd19ef347d2b6f2e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616083"
---
# <a name="mpsd-session-details"></a>MPSD 工作階段詳細資料

本文包括下列章節：
* 工作階段概觀
* 工作階段功能
* 工作階段大小
* 工作階段的使用者狀態
* 可見性和 Joinability
* 工作階段逾時
* 在單一主控台上的多個登入的使用者
* 處理程序生命週期管理
* 非使用中工作階段的清理
* 工作階段仲裁程式

## <a name="session-overview"></a>工作階段概觀

多人遊戲工作階段目錄 (MPSD) 工作階段的工作階段名稱，並識別為工作階段範本，也就是提供工作階段的預設設定的 JSON 文件的執行個體。 範本是以服務設定識別項 (SCID)，服務組態的一部分，也就是 GUID。 此範本可於[Xbox 開發人員入口網站 (XDP)](https://xdp.xboxlive.com)並[合作夥伴中心](https://partner.microsoft.com/dashboard)。 服務組態是用來擷取、 管理和安全性原則的開發人員專用資源。 透過 MPSD 存取工作階段時，主體的授權會針對服務組態，根據開發人員透過 XDP 或合作夥伴中心設定的存取原則。 載入工作階段之後存取服務組態已獲授權時，次要的存取權檢查，例如工作階段的成員資格驗證，會執行工作階段層級。

本主題假設您的範本使用合約版本 107，這是用於目前 MPSD Xbox One 的版本。 如果您已經定義合約版本 105 （等於 104） 為基礎的範本，您必須變更這些支援版本 107。 如需相關指示，請參閱 <<c0> [ 常見的多人遊戲 2015年移轉問題](common-issues-when-adapting-multiplayer.md)。

| 重要事項                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 透過範本設定的功能不能透過變更寫入至 MPSD。 若要變更的值，您必須建立並提交新的範本，內含所需的變更。 透過範本未設定任何項目可透過寫入變更為 MPSD。 |

### <a name="session-reference"></a>工作階段參考

每個 MPSD 工作階段唯一所參考的工作階段的參考，表示在由多人遊戲的 WinRT API **MultiplayerSessionReference 類別**。 工作階段參考包含下列字串值：

-   服務設定識別元 (SCID)
-   工作階段範本名稱
-   工作階段名稱

工作階段的參考會對應至的 URI 來識別工作階段，如下所示。 在下列範例對應中，「 授權 」 是 sessiondirectory.xboxlive.com。

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>工作階段的項目

每個工作階段包含強制執行可變動性和安全性規則，會因工作階段項目，以及唯讀內部管理資訊 （中繼資料） 的項目群組。 本章節描述的 JSON 檔案來設定您的工作階段中，和您選擇的範本 JSON 檔案中包含的工作階段項目群組。

| 注意                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 如果您使用 HTTP/REST 實作的 WinRT 包裝函式，您的工作階段和範本必須定義精確地反映 WinRT 功能的 JSON 物件。 |

在每個項目群組中，有兩個內部物件：

-   系統物件，這些物件有固定的結構描述，強制執行由 MPSD 解譯。 他們會驗證和合併。 由於 MPSD 定義，而且知道這些代表什麼意思，它可以處理它們。 系統物件的完整定義，請參閱參考**MultiplayerSession 類別**和參考**工作階段目錄 Uri**。
-   自訂物件，這些物件是選擇性的且沒有結構描述。 它們用來儲存有關多人遊戲的中繼資料。 MPSD 無法解譯此資料，因為它不會作用時。 標題受控儲存體 (TMS) 應該儲存遊戲資料或已儲存的資訊。 如需 TMS 相關資訊，請參閱 < **Xbox Live 的標題儲存體**。

以下是自訂的 JSON 物件的範例：
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>工作階段的常數

建立者或工作階段範本只能在建立期間設定工作階段的常數。 /Constants/system 物件用來定義常數，用來多人遊戲系統，所知透過 MPSD。 與這個物件相關聯的 WinRT 包裝函式由**MultiplayerSessionConstants 類別**。

/Constants/system 物件可以定義項目，包括功能物件、 計量物件、 managedInitialization （範本合約版本 104/105） 或 peerToPeerRequirements memberInitialization （合約版本 107） 物件的數目物件，peerToHostRequirements 物件和 measurementsServerAddresses 物件。


#### <a name="session-properties"></a>工作階段屬性

使用 /properties/system 物件來定義 MPSD 工作階段屬性。 與這個物件相關聯的 WinRT 包裝函式是**MultiplayerSessionProperties 類別**。 在任何時間，工作階段屬性是可寫入的工作階段的成員。 工作階段屬性，以 JSON 格式的範例包括： joinRestriction，initializationSucceeded，和配對的物件。 如需使用此項目群組的範例，請參閱 <<c0> [ 目標工作階段初始化與 QoS](smartmatch-matchmaking.md)。


#### <a name="member-constants"></a>成員常數

針對每個工作階段成員的聯結次組成員常數。 JSON 物件是 /members/ {index} / 常數/系統。 代表工作階段成員的 WinRT 包裝函式類別是**MultiplayerSessionMember 類別**。


## <a name="member-properties"></a>成員屬性

成員屬性都只能由工作階段的成員可寫入的。 它們 /members/ {index} / 屬性/系統物件，並設定的項目會反映**MultiplayerSessionMember 類別**。 以下是範例：

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>伺服器項目

伺服器是非使用者加入或已獲邀加入工作階段。 相關聯的 JSON 物件是 /servers/ {伺服器-名稱} / 常數/系統及 /servers/ {伺服器-名稱} / 屬性/系統。 這些物件是由伺服器只可寫入的。

| 注意                                                         |
|---------------------------------------------------------------------------|
| 目前未使用 /servers/ {伺服器-名稱} / 常數/系統物件。 |


### <a name="session-configuration"></a>工作階段設定

有兩種方式，來控制工作階段的組態：

-   使用內嵌透過 XDP 或合作夥伴中心的工作階段範本。
-   使用多人遊戲和配對 WinRT Api 或 REST Api 呼叫。 您仍然必須使用範本，但範本可能沒有包含您想要設定的值。 請注意，您的標題無法覆寫已經在範本中設定的常數。

若要定義工作階段本身提供為個別的 JSON 文件。 此外，開發人員必須實作任何所需的特定標題的 WinRT 包裝函式功能。 JSON 文件和任何 WinRT 包裝函式程式碼的內容必須精確地說，反映彼此，並必須反映最新的範本合約版本。

工作階段的結構描述所建立的工作階段版本 （主要版本） 與 （次要版本） 通訊協定修訂版本。 版本會結合成 X Xbl-合約版本標頭，因為 「 100\*主要 + 次要 」。 比方說，v1.7 標題會包含下列標頭上每個 REST 要求，假設第 107 的最新的範本合約版本：X-Xbl-合約-版本：107.

| 注意                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| 建議針對大部分的標題 （使用 XSAPI） 若要使用合約版本 105、 和工作階段範本版本 107。 |


### <a name="session-templates"></a>工作階段範本

每個工作階段範本是 JSON 文件，一部分的服務組態，定義正在建立工作階段架構，並提供新的工作階段的常數。 如需詳細資訊，請參閱 < [MPSD 工作階段範本](../service-configuration/session-templates.md)。

## <a name="session-capabilities"></a>工作階段功能

功能是 MPSD 工作階段中，設定行為 MPSD 應套用至該工作階段的常數。 您最常用於 XDP 和合作夥伴中心在工作階段範本中設定的功能。 它們是設定 /constants/system/capabilities 物件中。 如果不需要任何功能，使用空的功能物件。

| 注意                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| 項目幾乎不會變更，或存取多人遊戲的 WinRT API 或配對 WinRT API 所使用的工作階段功能。 |

工作階段功能由**MultiplayerSessionCapabilities 類別**。 它們是布林值，指出工作階段可以支援：

-   連線能力
-   玩遊戲
-   大型的大小
-   使用中成員所需的連接

**MultiplayerSessionConstants 類別**定義是關於工作階段功能的下列屬性：

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| 注意                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| 如果標題所定義的動態工作階段功能，對應的屬性設為工作階段的常數，則為 true。 |

## <a name="session-size"></a>工作階段大小

MPSD 工作階段的大小取決成員數目，在該工作階段。


### <a name="maximum-session-size"></a>最大工作階段大小

工作階段的大小上限是它能容納的工作階段成員數目上限。 它由**MultiplayerSessionConstants.MaxMembersInSession 屬性**。 最大成員大小設 /constants/system 物件中。

最大工作階段大小是介於 1 和 100 個工作階段的成員，並在建立時未設定預設值為 100。 如果所需的大小超過 100 時，工作階段就會稱為 「 大 」 的工作階段，並以特殊方式設定。

工作階段設定的最大的大小，可能會導致要問題顯示為完整是在特定期間中斷連線案例的開啟位置。 比方說，如果播放程式會變成中斷連接，因為網路或電源失敗而導致，延遲是無法立即反映在工作階段。 成員設定為非使用中使用中所述，中斷連線偵測功能[MPSD 變更通知處理和中斷連線偵測](multiplayer-session-directory.md)。

相較之下，會使用活動訊號偵測中斷連線的對等網狀結構通常是注意到中斷連線的兩到三秒內，並可以立即開啟的 player 位置。 不過，仲裁程式不能移除其他成員。

### <a name="large-sessions"></a>大型的工作階段

大型的 MPSD 工作階段可以有多達 1000 個成員，但有一些停用，例如取得一份所有成員的工作階段功能。 表示工作階段 largeness **MultiplayerSessionCapabilities.Large 屬性**。 此屬性設定為 true，以指出大型的工作階段中，且 「 大型 」 功能指示 /constants/system/capabilities 物件中。 

## <a name="session-user-states"></a>工作階段的使用者狀態



MPSD 會定義使用者狀態為已新增到工作階段之使用者的狀態。 可能的使用者狀態定義詳如所**MultiplayerSessionStatus 列舉**。 使用者也會被視為擁有的狀態為 「 可用 」 才能新增到工作階段。

**MultiplayerSession.SetCurrentUserStatus 方法**可用來變更工作階段使用者狀態。 這項變更可針對 REST 進行設定 /members/ {index} / 屬性/系統正確在遊戲工作階段的 JSON 文件中。


### <a name="reserved-user-state"></a>保留的使用者狀態

仲裁程式已選取其中一個開啟位置的填滿使用者工作階段內時，使用者會置於保留的使用者狀態。 處於此狀態中，當使用者尚未正式接受邀請才能在工作階段或已加入工作階段的開始與對等連線。


### <a name="active-user-state"></a>作用中使用者狀態

當使用者處於作用中狀態時，標題已加入工作階段代表使用者和使用者主動參與工作階段。 只要他/她玩遊戲，使用者會繼續處於此狀態。

標題第一次啟動時，它應該檢查使用者已經屬於任何工作階段中，通常是藉由檢查工作階段狀態。 如果使用者屬於工作階段，標題就可以直接跳到遊戲，並設使用者狀態的作用中的任何參與的本機成員。

工作階段中播放時，使用者應保留在作用中狀態。 如果使用者離開遊戲中的 UI 透過工作階段，使用者應該會從與工作階段**MultiplayerSession.Leave 方法**。 如果使用者是只能暫時放下遊戲，因為標題會受到限制，當標題應該保留使用者處於作用中狀態的合理的時間。 它會適用於將使用者狀態變更為非使用中，如果使用者尚未指定標題一段時間之後傳回。


### <a name="inactive-user-state"></a>非使用中的使用者狀態

在非使用中狀態下，使用者目前未參與遊戲，但仍有工作階段中的 已儲存的位置。 換句話說，使用者是 「 未作用中。 」

它是使用者自己的主控台，負責設定該使用者的狀態非使用中工作階段中。 仲裁程式無法執行此動作。 在其中使用者會放入的非作用中狀態的範例案例包括：

-   標題會收到的暫止的事件。
-   使用者已取消 （任何輸入或控制站回應） 的一段時間的項目所定義。 我們建議兩分鐘具競爭力的多人遊戲。
-   標題已在限制模式中，針對多個兩分鐘，或項目所定義的期間。 此限制的模式逾時期限是預期的一段時間，使用者可能會離開標題使用相關的應用程式或其他相關標題的體驗。
-   使用者已從工作階段被強制中斷。 請參閱[MPSD 變更通知處理，並中斷連線偵測](multiplayer-session-directory.md)。

如果標題會啟動特定工作階段成員的使用者狀態設定為非使用中，已暫止的標題，或使用者已經閒置工作階段中的時間太長。 因為標題會重新啟動，指示會是使用者想要繼續遊戲他或她所屬的工作階段。 如果使用者的狀態是作用中標題啟動時，這種情況可能是因為網路中斷連線或其中的標題無法設定為非使用中的使用者，中斷前的另一種情況。 在這兩種情況下，您的標題應該嘗試重新連線的使用者與遊戲和其他使用者繼續播放，或從工作階段中移除使用者。

### <a name="user-state-when-the-session-is-over"></a>使用者狀態時工作階段是透過

透過工作階段時，已停用遊戲。 標題必須允許所有使用者移除使用自行**MultiplayerSession.Leave 方法**。 當使用者離開工作階段時，會自動清除與使用者相關聯的工作階段活動。

## <a name="visibility-and-joinability"></a>可見性和 Joinability

工作階段存取由兩項設定控制 MPSD 層級： 工作階段的可見性和工作階段 joinability。 本主題中的可見性和 joinability 建議適用於最常見的 「 標題 」 案例。 標題應該遵循這些設定，可能的話，而且應該使用標題中的邏輯對於新的播放程式是否即允許工作階段到最後、 授權決定。


### <a name="session-visibility"></a>工作階段的可見性

工作階段的可見性被以所設定的工作階段建立的常數。 它通常定義在工作階段範本，並決定哪些類型的使用者具有讀取和寫入權限的工作階段。 工作階段的可見性的可能值所定義**MultiplayerSessionVisibility 列舉**。 在 JSON 檔案中的可見性常數所允許的設定是開啟、 可見且私用。


#### <a name="recommended-game-session-visibility-open"></a>建議您遊戲的工作階段的可見性： 開啟

開啟遊戲的工作階段不需要玩家保留項目，可簡化邀請程序。 仲裁程式不會保留在 MPSD 玩家之後，已傳送邀請，但只會追蹤受邀的播放程式在本機。 因此，玩家可以立即連線到仲裁程式，並判斷是否應加入的工作階段，會遭到拒絕，或應該等候 （如果支援等待播放程式）。 仲裁者是最終授權單位和回應，並指示新的成員，或是留在保留工作階段。

您必須啟動標題，並在進行最後決定之前，連接到仲裁程式受邀的播放程式，才能使用開啟的遊戲工作階段的可見性。 可接受工作階段已滿，或已拒絕邀請，向使用者顯示錯誤訊息。

若要建立仲裁程式的連線，安全的裝置位址則是必要項目。 **MultiplayerSessionProperties.HostDeviceToken 屬性**用來了解哪一個工作階段的成員是工作階段，目前的仲裁程式，而其保護的受邀的播放程式應該用於連線的裝置位址。

### <a name="session-joinability"></a>工作階段 Joinability

工作階段 joinability 決定哪些類型的使用者可以加入工作階段。 它都可以在工作階段期間以動態方式設定。 工作階段 joinability 的可能值如下：

-   無 （預設值）--有哪些人可以加入工作階段沒有限制。
-   本機-只有本機使用者可以將工作階段。
-   接著，只有本機使用者和後面接著其他工作階段成員的使用者可以加入未保留工作階段。

工作階段仲裁程式可以建立私用的工作階段，透過 joinability 設定。 本機或接著進行 joinability 會限制存取工作階段，並使其私用。

此外，仲裁者應該追蹤的 joinability 可能視主機層級拒絕，因此較舊的工作階段邀請的工作階段。 比方說，如果任何受邀的玩家有未抵達聯結的工作階段，直到工作階段已滿，仲裁者可以指示聯結的播放程式工作階段已被鎖定，而且他們需要自動將工作階段。

## <a name="session-timeouts"></a>工作階段逾時

可以變更工作階段計時器和其他外部事件。 工作階段逾時定義的工作階段的成員可以處於特定狀態之前保持自動變成非作用中或從工作階段中移除的週期。 MPSD 也支援管理工作階段存留期的逾時。

| 注意                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 逾時設定可在 /constants/system/timeouts，或在受管理的初始設定物件中，提供範本合約版本 104/105。 107 （含） 以後版本，，/常數/系統中，或在受管理的初始化物件，會個別進行設定。 |

當計時器到期時，MPSD 不會自動更新工作階段，並通知仲裁程式那時的任何變更。 工作階段以及逾時的狀態，才會更新之前讀取或寫入要求傳送。 立即更新可確保傳回的資料是最新的日期。

| 注意                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| 工作階段逾時並不堆疊，並只有一個適用於狀態轉換針對每個工作階段上的成員更新。 |


### <a name="currently-defined-timeouts"></a>目前定義的逾時

本章節描述目前 MPSD 所定義的逾時。 所有的逾時，會指定以毫秒為單位。 允許值為 0，且表示立即逾時。 逾時不含值會被視為無限的。 因為逾時都有預設值，您應該明確指定無限逾時為 null。
#### <a name="evaluationtimeout"></a>evaluationTimeout

此逾時表示工作階段的成員，才能進行，並上傳評估決策的時間的量。 如果不收到任何決策，決定都會被視為失敗。 此逾時被放在受管理的初始化物件。


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

此逾時設定已加入工作階段，但目前未參與遊戲中的工作階段成員。 依預設從 2 小時後，工作階段移除成員。

| 注意                                                                      |
|----------------------------------------------------------------------------------------|
| 此逾時指定非使用中的逾時，提供範本合約版本 104/105。 |

在許多情況下，建議將非使用中的逾時設定為 0，造成任何使用者都要立即移除從工作階段和要清除的對應位置設定為非使用中狀態。 因此，如果使用者已進入非作用中或達到非作用中狀態時，可以快速地加入新的播放程式，此行為是迫切需要的最具競爭力的多人遊戲。 聯合或其他多人遊戲設計，您可以您讓使用者有更多時間重新連線，如果它們已中斷連線，或不會參與一段時間的標題的標題。 請注意，沒有單一解決方案，以符合所有的設計案例。

#### <a name="jointimeout"></a>joinTimeout

此逾時表示使用者已加入工作階段的毫秒數。 移除保留的使用者將會失敗。 此逾時被放在受管理的初始化物件。


#### <a name="measurementtimeout"></a>measurementTimeout

此逾時表示工作階段的成員必須上傳測量的時間量。 無法上傳度量單位的成員會標示為 「 逾時 」 的失敗原因。 此逾時被放在受管理的初始化物件。

| 注意                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 在配對，會強制執行 QoS 度量的 45 秒逾時。 因此，我們建議使用小於或等於為 30 秒戰時測量逾時。 |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

設定此逾時工作階段成員已加入工作階段，並嘗試取得到遊戲。 這通常表示殼層已加入使用者，代表標題，並正在啟動標題。 成員會從工作階段中移除，並放置在非使用中狀態 3 分鐘之後, 預設。

| 注意                                                          |
|----------------------------------------------------------------------------|
| 此逾時，會指定合約版本 104/105 的準備逾時。 |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

設定此逾時的工作階段成員已新增至工作階段的其他人，但尚未加入工作階段。 刪除此保留項目，而逾時到期時，該成員視為非使用中。 預設值為 30 秒。

| 注意                                                             |
|-------------------------------------------------------------------------------|
| 此逾時，會指定合約版本 104/105 的保留逾時。 |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

此逾時之後的工作階段變成空的當它被刪除時，表示毫秒的數。 預設值為 0。

| 注意                                                                 |
|-----------------------------------------------------------------------------------|
| 此逾時，會指定合約版本 104/105 的 sessionEmpty 逾時。 |


### <a name="session-timeout-example"></a>工作階段逾時的範例

1.  四位玩家啟動工作階段。
2.  兩名玩家，A 和 B，會因電源故障而中斷。 在遊戲中其狀態會保持作用。
3.  正確使用的其他兩個玩家，C 和 D，結束**MultiplayerSession.Leave 方法**。
4.  工作階段維持開啟，A 和 B 已中斷連線的播放程式，但仍在作用中狀態。
5.  幾天後，播放程式的傳回，並啟動遊戲。
6.  播放程式 A 的遊戲會檢查的是成員的工作階段 （執行 「 讀取 」），並尋找被遺棄的工作階段，從幾天前。
7.  工作階段會針對兩個仍在 （A 和 B） 的工作階段的球員存在檢查。
    1.  因為播放程式的執行標題，播放程式的目前狀態檢查成功，並在比對的播放程式的作用中狀態維持不變。
    2.  播放程式 B 並未執行標題。 因此，播放程式 B 的失敗和服務的目前狀態檢查會將播放程式 B 的狀態設定為非使用中。 到目前為止，非作用中的逾時開始播放程式 b。

8.  播放程式的結束正確使用的工作階段**離開**方法。
9.  在非使用中的逾時到期對於播放程式 B，人員會從上的下一個要讀取或寫入的工作階段移除任何人。
10. 工作階段現在會有零的成員，和從服務中移除。

如果範例工作階段的非作用中逾時設為 0，播放程式 B 發生逾時組態選項的核取之後，立即在步驟 7a 和工作階段寫入可能會移除。 在此情況下，工作階段隨即關閉，而不需要額外的讀取或寫入的工作階段。


## <a name="multiple-signed-in-users-on-a-single-console"></a>在單一主控台上的多個登入的使用者


當多個使用者時它登入相同的主控台中，是在遊戲工作階段中，而其他使用者則不是工作階段中的某些使用者可以或不在目前的標題中。 也能夠接收和接受多個使用者，對遊戲工作階段的成員資格造成的影響遊戲的邀請。 請考慮下列幾點，若要能夠正確處理所有的工作階段的成員資格案例要標題。

在常見的案例中，新播放程式會登入，會變成在遊戲中，使用，而且必須新增至現有的遊戲工作階段。 做為建立新的遊戲工作階段中，標題應該只能新增使用者時才適用在進行遊戲。

使用多個登入的使用者時，一或多個使用者也可以接收另一個遊戲工作階段的邀請。 標題不需要任何特定的方式處理這些案例。 工作階段狀態和成員的事件通知的遊戲的工作階段和使用者成員資格的任何更新的標題。

線上工作階段中處理多個登入的使用者，標題所訂閱的所有使用者，使用個別的 shoulder 點選**XboxLiveContext 類別**每位使用者的物件。 使用標題**MultiplayerSession.ChangeNumber 屬性**判斷工作階段中的特定變更，並忽略重複的潛在點選。


## <a name="process-lifecycle-management"></a>處理程序生命週期管理


就像非多人遊戲標題，標題暫止和終止的處理程序生命週期事件，可能會遇到多人連線的工作階段中的標題。 工作階段仲裁程式因此應該定期儲存工作階段狀態。 如果仲裁程式暫停時，標題應該嘗試進行仲裁程式移轉並視需要儲存遊戲狀態，好讓新的仲裁程式可以還原工作階段狀態。 只要工作階段仍然有效 MPSD 中就可能被暫停及繼續更新版本中，完整多人遊戲工作階段。 只有一個指定對等，通常該遊戲的主機，應更新全域的遊戲狀態。


### <a name="storage-of-game-metadata"></a>遊戲的中繼資料的儲存體

標題 MPSD 工作階段中，儲存遊戲的中繼資料。 遊戲的中繼資料會顯示工作階段資料，並啟用要尋找及加入遊戲工作階段的標題所需的資訊。 標題會播放程式特定的中繼資料的自訂屬性區段中儲存的工作階段成員，比方說，播放程式的色彩，慣用的播放程式武器，適用於工作階段等等。整個工作階段的中繼資料，例如，目前的對應，會儲存在 MPSD 工作階段的全域的自訂屬性區段。


### <a name="storage-of-game-state"></a>遊戲狀態的儲存體

遊戲狀態儲存在標題管理的儲存體 (TMS)，使用**標題儲存體服務**。 使用此位置的儲存體可讓要移轉仲裁程式，而不需要權限問題的標題。 請參閱[移轉仲裁程式](migrating-an-arbiter.md)。

| 注意                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| 標題不應嘗試將遊戲狀態儲存到 TMS 頻率高於每 5 分鐘，除非它已遭到擱置。 |

## <a name="cleanup-of-inactive-sessions"></a>非使用中工作階段的清理

SessionEmptyTimeout 設為 0 時，如果 MPSD 工作階段會在最後一個等玩家要離開工作階段時自動刪除。 若要了解如何防止未使用的工作階段包含播放程式損毀或中斷連線之後，請參閱[MPSD 變更通知處理和中斷連線的偵測](multiplayer-session-directory.md)。 不適當處理的當機之後未使用的工作階段中斷連線或標題查詢工作階段，播放程式時可能會造成問題。

清除非作用中的工作階段的建議的方式是將標題查詢特定使用者的所有工作階段藉由呼叫**MultiplayerService.GetSessionsAsync 方法**然後評估工作階段。 當它遇到過時的工作階段時，標題就會呼叫**MultiplayerSession.Leave 方法**工作階段中的所有本機播放程式。 此呼叫最後會卸除成員計數為 0，並清除 工作階段。

## <a name="session-arbiter"></a>工作階段仲裁程式


多人遊戲的一些方法只應該由一部用戶端在遊戲工作階段內呼叫。 此用戶端是主控台工作階段中，參與仲裁程式或主應用程式呼叫。 如果至少一個工作階段的成員是在遊戲中，工作階段應該有仲裁程式來監視進行中的聯結。


### <a name="setting-the-arbiter"></a>設定仲裁程式

當它建立一個工作階段時，用戶端會指定為仲裁程式的單一主控台。 請參閱[How to:MPSD 工作階段設定仲裁程式](multiplayer-how-tos.md)。


### <a name="saving-session-state"></a>正在儲存工作階段狀態

中所述**程序生命週期管理**，仲裁者應該定期儲存工作階段狀態。 新的仲裁程式必須能夠將工作階段狀態在仲裁程式移轉的情況下還原項目所。 如需詳細資訊，請參閱 <<c0> [ 移轉仲裁程式](multiplayer-how-tos.md)。


### <a name="managing-game-session-members-and-joins-in-progress"></a>管理遊戲工作階段的成員和進行中的聯結

工作階段仲裁程式的最重要的角色是管理進入玩遊戲的工作階段的使用者。 這包括處理遊戲的邀請，通知等待播放程式，以及使用結束遊戲的玩家。


#### <a name="receiving-notifications"></a>接收通知

仲裁者必須接聽新要加入的遊戲工作階段的球員**RealTimeActivityService.MultiplayerSessionChanged 事件**。


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>找出玩家，以填滿空的遊戲工作階段的位置

仲裁程式會找出玩家，以填滿空的遊戲工作階段的位置的其中一個這些作業： 如果您的標題使用大廳工作階段或其他機制來允許延遲的聯結，尋找新的工作階段的成員，使用該機制。
-   建立另一個相符項目票證工作階段。

另請參閱[How to:填入開啟工作階段位置戰時](multiplayer-how-tos.md)。


#### <a name="handling-invited-session-members"></a>處理受邀成員的工作階段

仲裁者必須監視受邀的工作階段的成員，並套用最小間隔為單一使用者的邀請。 另請參閱[How to:傳送遊戲的邀請](multiplayer-how-tos.md)。
