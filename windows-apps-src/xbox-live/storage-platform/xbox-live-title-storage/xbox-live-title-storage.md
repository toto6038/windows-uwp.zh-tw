---
title: Xbox Live 的標題儲存體
description: 了解如何使用 Xbox Live 的標題儲存體在雲端中儲存遊戲的標題資訊。
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 遊戲, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 472a2131c113cdde5d7383e169e4fbe638e4e555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623923"
---
# <a name="xbox-live-title-storage"></a>Xbox Live 的標題儲存體

Xbox Live 的標題儲存體服務會提供在雲端中儲存遊戲的標題資訊的方式。 在所有平台上執行的遊戲可以使用這項服務。

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Xbox Live 的標題儲存體的功能

某些 Xbox Live 的標題儲存體的高階功能包括但不限於：

-   可以讓使用者、 標題和各種平台共用
-   支援 JSON、 二進位和組態檔

在下列各節中詳細說明 Xbox Live 的標題儲存體的主要功能：

-   [類型的儲存體](#ID4ETB)
-   [資料類型](#ID4ECF)
-   [標題儲存體 Uri](#ID4EBEAC)
-   [節流限制](#ID4ETEAC)

<a name="ID4ETB"></a>

適用於受管理的合作夥伴和ID@Xbox成員：

| 存放類型       | 配額 (管理Partners/ID@Xbox) | 配額 （Xbox Live 創作者計劃） |  用途                                                                                                                                                      | 平台                                                                                           | 使用者                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| 可信賴平台   | 每位使用者的 256 MB | 64 MB，每位使用者    | 每個使用者資料，例如儲存遊戲或遊戲播放/暫停/繼續狀態。 更安全，但其具有平台限制。 | 任何平台可讀取，但可能會寫入只有 Xbox One、 Xbox 360 或 Windows Phone。  | 可設定為 public 或只有擁有者。       |
| 通用平台 | 64 MB，每位使用者 | 64 MB，每位使用者    | 每個使用者資料，例如儲存遊戲或遊戲播放/暫停/繼續狀態。 | 可能會撰寫任何平台，但 Xbox One、 Xbox 360 或 Windows Phone 以外的平台可讀取。 | 可設定為 public 或只有擁有者。       |
| 全域             | 256 MB | 256 MB            | 所有使用者都可以讀取，例如名冊、 對應、 挑戰或美工圖案資源的資料。 | 只有透過 Xbox 開發人員入口網站或合作夥伴中心，可供寫入，任何平台可讀取。                                | 可讀取所有使用者。

### <a name="deprecated-storage-types"></a>已被取代的儲存體類型

下列儲存體類型已被取代。 它們僅支援目前使用它們的標題。 它們不適用於新的標題。

| 存放類型       | 配額  |   用途                                                                                                                                                      | 平台                                                                                           | 使用者                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 64 MB，每位使用者     | 每個使用者資料，例如儲存遊戲或遊戲播放/暫停/繼續狀態。 更安全的沒有平台限制，但需進行的資料格式的限制 (僅限 JSON)。 | 任何平台可能會讀取或寫入。                                                                     | 可設定為 public 或只有擁有者。       |
| 裝置             | 64 MB，每個裝置   | 設定或裝置喜好設定等裝置特定的資料。                                                                                            | 可以撰寫只 Xbox One、 Xbox 360 或 Windows Phone。 可讀取寫入資料的裝置。  | 可讀取所有使用者。                         |
| 工作階段存放區    | 每個工作階段的 256 MB | 任何人的資料會加入特定的多人連線遊戲工作階段。                                                                                             | 可能會加入此工作階段的任何平台。                                                             | 工作階段中的所有使用者可能會讀取或寫入。 |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>資料類型

遊戲指定要用於資料型別 **{type}** GET 或 PUT 方法的參數。 下節描述三個支援的類型：

-   [二進位資訊](#ID4ENF)
-   [JSON 資訊](#ID4EUF)
-   [組態資訊](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>二進位資訊

影像、 聲音和自訂資料使用二進位類型。 因為資料必須透過 HTTP 傳輸，二進位資料必須編碼成 HTTP 接受的字元。 比方說，您可以將資料轉換成十六進位字串，或使用 base64 編碼。 標題儲存系統不會處理編碼的資料，因此您的遊戲必須使用相同的結構描述來編碼和解碼資料讀取和寫入標題儲存體時。

<a name="ID4EUF"></a>

#### <a name="json-information"></a>JSON 資訊

結構化的資料可以使用 JSON 型別。 JSON 物件可以直接用於，JavaScript 等語言，支援它們。 擷取資料時從 JSON 檔案，可以提供遊戲*選取*參數來傳回結構內的特定項目。 例如，使用 JSON 格式的檔案，其中包含下列資訊：

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| 注意                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 基於安全考量，JSON 資料的第一個項目不能是陣列。 服務會拒絕提交根目錄陣列的 JSON 資料。 |

遊戲可以選取這個結構如下的查詢部份：

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

此查詢的回應主體是：

    {
        "name" : "poison"
    }

陣列可以使用如下的查詢來存取：

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

此查詢的回應主體是：

    {
        "quest" : "queen"
    }

JSON 資料，都會強制執行下列的長度限制：

-   數值，最大長度 = 32
-   字串值，最大長度 = 1024年
-   屬性名稱、 最大長度 = 64
-   階層架構中，最大深度 = 16
-   陣列，最大大小 = 1024年
-   子屬性，在物件中的 最大值 = 1024年

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>組態資訊

**{Type}** 可以是**config**表示資料的組態 blob。 組態 blob 是全域的標題儲存體中的資料結構。 Blob 的格式為 JSON 物件類似。

組態 blob 可包含從的可能值清單中傳回設定的虛擬節點。 虛擬節點可用於提供設定特定的情況下，例如，標題或地區設定。 虛擬節點包含數個可能的設定，以及值和條件的值選取。 在下列範例中， **defaultCardDesign**設定可以在其中一個值的虛擬節點。

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

當遊戲讀取此檔案時，系統會選取其中一個值從 **\_sourceNodes**陣列。 在此情況下，會選取項目，根據標題識別碼的遊戲。 使用者在玩遊戲**12456799**請參閱：

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

其他使用者，請參閱：

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

遊戲可以定義自訂選取器符合要求中的參數。 例如，在此組態 blob:

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

傳遞字串的遊戲**customSelector**參數來選取要傳回哪些項目。 例如，若要取得第二個選項，遊戲要求：

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

 **\_SelectBy**值表示要選取哪些類型和**\_選取器**值表示要使用選取範圍中的資料。 可能的值為：

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p><strong>_Selector</strong>符合標題識別碼中提供的宣告。</p></td>
</tr>
<tr>
<td >地區設定</td>
<td ><p><strong>_Selector</strong>符合 Accept-language 標頭的地區設定字串。</p></td>
</tr>
<tr>
<td >自訂</td>
<td ><p><strong>_Selector</strong>比對自訂字串傳入<strong>customSelector</strong>查詢參數。 <strong>CustomSelector</strong>包含一或多個以逗號分隔的查詢。 每個查詢會從名稱<strong>selectBy</strong>項目和值<strong>_selector</strong>項目。</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>標題儲存體 Uri

標題儲存體 Uri 的格式如下：

    https://titlestorage.xboxlive.com/{path}

**{Path}** URI 部分是所提出的要求類型，而且必須是 245 個字元或更少。

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>節流限制

多少個讀取或寫入每分鐘，讓標題，可以在沒有固定的限制，但它通常無法在一個以上的每分鐘平均一小時的節目。 例如，標題可以讓 60 的讀取或寫入開頭的工作階段，但沒有其他的時數的其餘部分。 標題應該強化，針對多個呼叫之後，如果 Xbox LIVE 服務需要要求進行節流。

如果您的標題有特殊的資料分割需求，例如額外的讀取或寫入，請連絡 Microsoft。

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>使用標題儲存體

若要開始使用標題儲存體，必須先判斷您要儲存的資料類型。 一些範例包括儲存的遊戲、 遊戲狀態、 每日的挑戰、 遊戲模式和圖案資源。

接下來判斷標題及平台需要存取資料。 標題儲存體支援雲端的資料存取從單一平台，單一標題和多個平台上的多個項目。

最後，使用這一節的主題來設定您的儲存體、 上傳您的資料，以及設定會根據您的選擇適當的存取權限。

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>本節內容

[讀取在 Xbox Live 的標題儲存體中的組態 Blob](reading-configuration-blobs.md)  
示範如何讀取從 Xbox Live 的組態 blob 標題儲存體。

[在 Xbox Live 的標題儲存體中儲存二進位 Blob](storing-binary-blobs.md)  
示範如何將二進位 blob 儲存在 Xbox Live 的標題儲存體中。

[讀取在 Xbox Live 的標題儲存體中的二進位 Blob](reading-binary-blobs.md)  
示範如何從 Xbox Live 的標題儲存體讀取二進位 blob。

[在 Xbox Live 的標題儲存體中儲存的 JSON Blob](storing-jsonblobs.md)  
示範如何在 Xbox Live 的標題儲存體中儲存的 JSON blob。

[讀取在 Xbox Live 的標題儲存體中的 JSON Blob](reading-jsonblobs.md)  
示範如何讀取從 Xbox Live 的 JSON blob 標題儲存體。

<a name="ID4E4FAC"></a>
