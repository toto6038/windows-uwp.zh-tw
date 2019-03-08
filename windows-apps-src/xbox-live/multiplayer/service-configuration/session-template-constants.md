---
title: 工作階段範本常數
description: 描述在 Xbox Live 多人連線的工作階段範本中定義的系統常數。
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、windows xbox one，多人遊戲，工作階段範本
ms.localizationpriority: medium
ms.openlocfilehash: 5bb4c6f39bab8779b5675a7b9c81b883965676c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646903"
---
# <a name="session-template-constants"></a>工作階段範本常數

下表描述多人連線的工作階段範本，請使用工作階段範本版本 107 預先定義的項的目。

## <a name="system"></a>system

系統常數  | 描述 | 有效的值 | 預設值
--|-- | -- | --
version | 工作階段範本版本。 | 1 - n | 無
maxMembersCount | 多人遊戲活動支援的工作階段總數成員位置數目。 | 1-100 的正常工作階段，101 + 大型的工作階段 | 100
可見度 | 工作階段，這表示是否其他使用者可以看到及/或加入工作階段的可見性狀態。 | 私用、 顯示、 開啟 | 開啟
inviteProtocol | 設定這個常數，以 「 game 」 可讓受邀者接收快顯通知，當他們受邀至工作階段。 | 遊戲、 tournamentgame、 聊天、 gameparty | 無
reservedRemovalTimeout  | 成員保留，以毫秒為單位的逾時。 值為 0 表示立即逾時。 如果在逾時是 null，它會被視為無限。 | 0-n null | 30000
inactiveRemovalTimeout  | 成員，才能被視為非使用中，以毫秒為單位的逾時。 值為 0 表示立即逾時。 如果在逾時是 null，它會被視為無限。 | 0-n null | 0
readyRemovalTimeout | 成員，才能被視為已準備好，以毫秒為單位的逾時。 值為 0 表示立即逾時。 如果在逾時是 null，它會被視為無限。 | 0-n null | 180000
sessionEmptyTimeout | 為空的工作階段，以毫秒為單位的逾時。 值為 0 表示立即逾時。 如果在逾時是 null，它會被視為無限。 | 0-n null | 0
[**capabilities**](#capabilities) | 指定工作階段的功能。 請參閱以下的 「 功能 」 一節。 | 不適用 | 不適用
[**metrics**](#metrics) | 指定一組定義的標題服務品質需求，例如延遲和頻寬，工作階段中的成員必須滿足的速度。  | 不適用 | 不適用
[**memberInitialization**](#memberInitialization) | 指定新成員加入此工作階段時，會強制執行的逾時和初始化需求。 請參閱下的成員初始設定一節。 | 不適用 | 不適用
[**peerToPeerRequirements**](#peerToPeerRequirements) | 指定網路服務需求品質的對等網狀結構的連線。 請參閱下方的對等需求一節。 |不適用 | 不適用
[**peerToHostRequirements**](#peerToHostRequirements) | 指定網路服務品質需求對主機連線的對等。 請參閱下的主機需求一節的對等。 | 不適用 | 不適用
[**measurementServerAddresses**](#measurementserveraddresses) | 指定潛在的資料中心，用來判斷 QoS 的度量的集合。 請參閱下面的 measurementServerAddresses 章節。 | 不適用 | 不適用
[**cloudComputePackage**](#cloudComputePackage) | ? | 不適用 | 不適用
[**arbitration**](#arbitration) | 指定要提交在比賽中的仲裁結果之成員的逾時。 請參閱下面的 cloudComputePackage 章節。 | 不適用 | 不適用
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | 指定標題應一律具有工作階段的 「 讀取 」 權限的識別碼的清單。 請參閱下面的 broadcastViewerTitleIds 章節。 | 不適用 | 不適用
[**ownershipPolicies**](#ownershipPolicies) | 指定工作階段擁有權相關的原則。 請參閱下面的 OwnershipPolicies 章節。 | 不適用 | 不適用


## <a name="capabilities"></a>capabilities
功能是選擇性設定工作階段設定範本中的布林值。 如果不需要任何功能，空白 'capabilities' 物件應該是在範本中，以避免功能指定在工作階段建立後，除非標題想要動態工作階段功能。

功能 |  description | 有效的值 | 預設值
-- | -- | -- | -- |
連線 | 表示工作階段是否支援對等連線。 如果此值為 false，然後在工作階段無法啟用任何計量和工作階段的成員無法設定其 SecureDeviceAddress。 無法在大型的工作階段設定。 | true、 false | false
suppressPresenceActivityCheck | 如果為 true，會關閉目前狀態檢查。 | true、 false | false
玩遊戲 | 指出工作階段是否代表實際的遊戲，而不是安裝程式] / [功能表時間，例如大廳或配對。 如果為 true，工作階段處於遊戲模式。 | true、 false | false
大型 | 指出工作階段是否為大型的工作階段 （100 個以上的成員）。 大型的工作階段不支援用於多人連線管理員。 | true、 false | false
connectionRequiredForActiveMembers | 表示是否需要連接在順序中的成員可以使用中。 | true、 false | false
cloudCompute | 可讓用戶端要求會配置雲端計算執行個體代表工作階段。 | true、 false | false
autoPopulateServerCandidates | 自動計算，並將 'serverMeasurements' 'serverConnectionStringCandidates'。 這項功能無法在大型的工作階段設定。 | true、 false | false
userAuthorizationStyle | 表示工作階段是否支援從沒有強式標題身分識別平台的呼叫。 這項功能無法在大型的工作階段設定。</br></br>設定`userAuthorizationStyle`能夠`true`預設值`readRestriction`並`joinRestriction`工作階段`local`而非`none`。 這表示項目必須使用搜尋功能在處理或傳輸加入遊戲工作階段的控制代碼。| true、 false | false
crossplay | 指出工作階段支援跨 play PC 和 Xbox One 裝置之間。 | true、 false | false
廣播 | 指出工作階段代表廣播。 工作階段名稱必須是 xuid 的廣播者。 需要 「 大型 」 的功能。 | true、 false | false
小組 | 指出工作階段代表聯賽小組。 這項功能無法在 '大型' 或 '遊戲' 的工作階段設定。 | true、 false | false
仲裁 | 表示工作階段，必須建立服務主體，將加上 '仲裁' 的伺服器項目。 無法設定 「 大 」 的工作階段，但需要 '遊戲'。 | true、 false | false
hasOwners | 表示工作階段具有特定成員之擁有者為基礎的安全性原則。 | true、 false | false
可搜尋 | 指出工作階段可以成為目標的工作階段的搜尋控制代碼。 如果 'userAuthorizationStyle' 功能設定，然後 'searchable' 功能 nelze nastavit，pokud 未設定 'hasOwners' 功能。 | true、 false | false

範例：

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>計量
如果`metrics`屬性沒有指定，則會預設為所需滿足服務需求品質的值。  
如果有指定，則值必須足以滿足服務需求品質。
這個項目才有效，如果工作階段有`connectivity`功能集。

計量 | 描述 | 有效的值 | 預設值
-- | -- | -- | --
延遲 | | true、 false | 請參閱描述
bandwidthDown | | true、 false | 請參閱描述
bandwidthUp | | true、 false | 請參閱描述
自訂 | | true、 false | 請參閱描述

範例：
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
如果`memberInitialization`物件設定時，工作階段所預期的用戶端系統或執行工作階段建立後的初始設定及/或做為新成員加入此工作階段的標題。  
在逾時和初始化階段會自動追蹤工作階段，包括 QoS 度量，如果設定了任何計量。  
這些逾時的覆寫工作階段的保留，且準備好的逾時，具有 'initializationEpisode' 成員設定。  
無法指定大型的工作階段。

項目  | 描述 | 有效的值 | 預設值
-- | -- | -- | --
joinTimeout | 表示成員已加入工作階段的毫秒數。 移除保留的使用者將會失敗。</br>**注意：** 預設持續時間已足夠一般標題執行，但可能會導致將逾時，如果 MPSD 流程期間，正在偵錯標題。 這些情況下覆寫，並增加工作階段的這個預設值。| 0 - n | 10000
measurementTimeout | 表示工作階段的成員可上傳度量的毫秒數。 無法上傳度量單位的成員會標示為 「 逾時 」 的失敗原因。  | 0 - n | 30000
evaluationTimeout | 指出外部的評估已上傳度量的毫秒數。 | 0 -n | 5000
externalEvaluation | 如果為 true，表示標題的程式碼會評估人員聯結 QoS 測量基礎。 多人遊戲服務不會執行任何 QoS 邏輯，並往前移動初始化階段負責標題。 標題通常不需要如此。 | true、 false | false
membersNeededToStart | 啟動工作階段中的，初始化時段零只有所需的成員數目。 | 1-maxMembersCount | 1

範例：
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

對等網路需求 | 描述 | 預設值
-- | -- |--
latencyMaximum | 最大延遲，以毫秒為單位，任何兩個用戶端之間。 | 250
bandwidthMinimum | 在任何兩個用戶端之間的千位元 / 每秒的最小頻寬。 | 10000

範例：
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

對等主應用程式網路需求 | 描述 | 有效的值 | 預設值
-- | -- | -- | --
latencyMaximum | 最大延遲，以毫秒為單位，如主機連線的對等。 | | 250
bandwidthDownMinimum | 每秒千位元的主控件的對等所傳送的資訊中的最小頻寬。 | | 100000
bandwidthUpMinimum | 每秒千位元資訊從對等體傳送至主應用程式中的最小頻寬。 | | 1000
hostSelectionMetric | 指出哪些度量可用來選取主機。 | bandwidthup、 bandwidthdown、 頻寬和延遲 | 延遲

範例：
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
潛在的伺服器連接字串應被評估的一組。 連接字串必須是小寫。
無法指定大型的工作階段。

連接字串會定義格式如下：

`"<server name>" : {deviceAddress}`

其中的裝置位址描述如下：

伺服器連接字串 | 描述
-- | --
secureDeviceAddress | Base-64 編碼的伺服器安全的裝置位址

範例：
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
指定雲端的內容計算配置的封裝。 需要`cloudCompute`設定功能。

雲端計算屬性 | 描述
-- | -- | -- | --
titleId | 表示的標題的雲端識別碼計算配置的封裝。
gsiSet | 表示 GSI 組配置的雲端計算套件。
Variant | 表示配置雲端計算套件的變體。

範例：
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>仲裁
指定仲裁處理程序的逾時值。 需要`arbitration`設定功能。 中的工作階段中定義的仲裁開始時間 */servers/arbitration/constants/system/startTime*項目。

timeout | 描述 | 有效的值 | 預設值
-- | -- | -- | --
forfeitTimeout | 表示的時間，仲裁開始時間 （毫秒），待決定 | 0 - n | 60000
arbitrationTimeout | 指出的時間，以毫秒為單位從仲裁開始時間的仲裁結果逾時。這個值不能小於`forfeitTimeout`值 | 0 - n | 300000

範例：
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

指定標題的項目，應一律具有廣播的工作階段的 「 讀取 」 權限識別碼的陣列。

範例：
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
指定如何處理當最後一個擁有者離開工作階段的工作階段。 需要`hasOwners`設定功能。

擁有權原則 | 描述 | 有效的值 | 預設值
-- | -- | -- | --
移轉 | 指出過去的擁有者離開工作階段時，就會發生的行為。 如果移轉原則設定為"endsession 」，過期的工作階段。 如果移轉原則設定為 「 舊 」，請選取 最舊的聯結時間成員，才能成為新的擁有者的工作階段。 | "oldest", "endsession" | 「 endsession"

範例：
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
