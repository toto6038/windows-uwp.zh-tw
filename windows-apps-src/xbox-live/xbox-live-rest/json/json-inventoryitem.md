---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " inventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628493"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
Core 清查項目所表示的權限可以授與的標準項目。
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

InventoryItem 物件具有下列規格。

| 成員| 類型| 描述|
| --- | --- | --- |
| URL| 字串| 這個特定的清查項目的唯一識別碼。|
| itemType| 字串| 項目的類型。 目前的值為 <ul><li><b>未知</b></li><li><b>遊戲</b></li><li><b>影片</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>海報</b></li><li><b>播客</b></li><li><b>影像</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>佈景主題</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>相簿</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>Bundle</b></li><li><b>XnaCommunityGame</b></li><li><b>Promotional</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>App</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>訂用帳戶</b><br/><br/> **注意：** 遊戲所指定的**GameV2**，會消耗**GameConsumable**，且持久的 DLC **GameContent**。 |
  | 容器 | 字串 | 這是 「 容器 」，其中包含這個項目組。 屬於特定的容器項目都可以查詢使用者的清查。 項目新增至庫存採購單時，會決定這些容器。 |
  | 取得 | DateTime | 日期和時間的項目加入至使用者的清查。 |
  | startDate | DateTime | 日期和時間的項目或將成為可供使用。 |
  | endDate | DateTime | 日期和時間的項目或會變成無法使用。 |
  | 狀態 | 字串 | 項目的狀態。 允許的值為**Enabled**， **Suspended**，**已過期**， **Canceled**， **Renewed**。  |
  | trial | 布林值 | 必要。 如果此權利是試用版，則為 true否則為 false。 如果您購買試用版的權利，並接著購買完整版，您會收到兩個。 |
  | trialTimeRemaining | TimeSpan | 可為 null。 試用版，以分鐘為單位所剩多少時間。 |
  | 可使用 | 陣列 | 如果可取用的項目，這包含內嵌表示法的唯一識別碼 （連結） 需求的可取用的清查項目，以及其目前的數量。 |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>範例 JSON 語法


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>需求的可取用的清查項目

需求的可取用的實體會提供最少的一組屬性的需求的可取用的項目。

| 成員| 類型| 描述|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| URL| 字串| 唯一識別碼的特定需求的可取用的清查項目。|
| quantity| 32 位元帶正負號的整數| 此清查項目目前的數量。|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>請參閱

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>父系

[JavaScript 物件標記法 (JSON) 物件參考](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>參考

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)
