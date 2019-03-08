---
title: 正在更新字串豐富的組態選項
description: 了解如何更新為 Xbox Live 的豐富的目前狀態的字串。
ms.assetid: eb2bb82e-8730-4d74-9b33-95d133360e44
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live、 xbox、 遊戲、 uwp、 windows 10、 xbox，完整的目前狀態
ms.localizationpriority: medium
ms.openlocfilehash: ac4549301c60eafb935dab0ac9c5b5028452edfb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610393"
---
# <a name="rich-presence-updating-strings"></a>正在更新字串豐富的組態選項

若要更新您的標題中的豐富的組態選項字串，您可以呼叫寫入 Title URI，以適當的參數 JSON 物件中。 此 restful 呼叫也會由 Xbox 服務 Api 包裝。 請參閱**Microsoft.Xbox.Services.Presence 命名空間**如需相關的 API 資訊。

URI 看起來像這樣：

          POST /users/xuid({xuid})/devices/current/titles/current

以下是設定豐富的組態選項字串的欄位。 有標題，此處未列出的寫入目前狀態的相關其他選擇性欄位。

## <a name="titlerequest-object"></a>TitleRequest 物件

屬性 | 類型 | 要求會 | 描述
---|---|---|---
活動|ActivityRequest|N|描述在項目資訊 （豐富的組態選項和媒體資訊，如果有的話） 的記錄

## <a name="activityrequest-object"></a>ActivityRequest 物件

屬性 | 類型 | 要求會 | 描述
---|---|---|---
richPresence|RichPresenceRequest|N|應該使用豐富的組態選項字串的 friendlyName。

## <a name="richpresencerequest-object"></a>RichPresenceRequest 物件

屬性 | 類型 | 要求會 | 描述
---|---|---|---
Id|字串|Y|應該使用豐富的組態選項字串的 friendlyName
scid|字串|Y|告訴我們，並定義豐富的組態選項字串的 Scid。

例如，如果我想要更新的使用者其 xuid 為 12345 的豐富的目前狀態，我呼叫看起來，如下所示：

          POST /users/xuid(12345)/devices/current/titles/current


使用下列 JSON 主體：

```json
          {
            activity:
            {
              richPresence:
              {
                id:"playingMap",
                scid:"0000-0000-0000-0000-01010101"
              }
            }
          }
```

使用 API 的包裝函式，這會是呼叫**PresenceService.SetPresenceAsync 方法**

如果您保留的資料平台最新狀態，則您不必每次重設豐富是否存在字串的資料來填入空白的變更。 在上述範例中，我們知道您想要使用目前的對應。 目前狀態將會查閱中的資料平台的資料，當使用者嘗試讀取要填入的目前值的字串。 因此，即使播放程式正在切換從對應至對應到對應，您不需要重設的豐富存在字串中您的遊戲，只要您適當的事件傳送至的資料平台。 請記住，可能需要幾秒鐘的資料，找出其方法，透過資料平台。

然後，當有人嘗試讀取使用者 12345 的豐富的目前狀態，服務將看看哪些地區設定所要求，並傳回之前適當地格式化字串。

在此情況下，假設使用者想要讀取的英文字串。 閱讀完整的目前狀態，如下所示運作 (如需此呼叫的詳細資訊，請參閱**GET (/users/xuid({xuid}))**

          GET /users/xuid(12345)?level=all

這個包裝函式的 API 是**PresenceService.GetPresenceAsync 方法**

以下您所要求的使用者，其 xuid 為 12345 PresenceRecord 狀況。 與您要求的詳細資料的層級 「 全部 」。 如果未指定"all"，將不會傳回豐富的組態選項。

它會傳回下列 JSON 回應中：

```json
          {
            xuid:"12345",
            state:"online",
            devices:
            [
              {
                type:"D",
                titles:
                [
                  {
                    id:"12345",
                    name:"Buckets are Awesome",
                    lastModified:"2012-09-17T07:15:23.4930000",
                    placement: "full",
                    state:"active",
                    activity:
                    {
                      richPresence:"Playing on map:Mountains"
                    }
                  }
                ]
              }
            ]
          }
```
