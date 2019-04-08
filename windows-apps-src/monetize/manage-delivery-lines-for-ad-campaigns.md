---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: 在 Microsoft Store 促銷 API 中使用此方法，管理適用於廣告行銷活動的播送行。
title: 管理廣告播送行
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 363f7034d7e353d9ee110637971e7b848dbca1bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625633"
---
# <a name="manage-delivery-lines"></a>管理廣告播送行

在 Microsoft Store 促銷 API 使用這些方法，建立一個或多個*播送行*購買詳細目錄，並提供您的用於促銷活動的廣告。 對於每個廣告播送行，您可以設為目標，設定買方出價，以及設定預算和要使用之廣告素材的連結，決定要花多少費用。

如需有關播送行和廣告行銷活動、目標設定檔及廣告素材之間關聯性的詳細資訊，請參閱[使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

>**附註**&nbsp;&nbsp;成功，您就可以建立使用此 API 的廣告活動的傳遞行之前，您必須先[建立另一種付費的廣告活動使用**廣告活動**頁面合作夥伴中心](../publish/create-an-ad-campaign-for-your-app.md)，而且您必須加入至少一個付款方式，此頁面上。 執行此動作之後，您就可以使用此 API 成功地建立廣告行銷活動的可計費廣告播送行。 使用 API 建立的廣告活動將會自動計費上所選擇的預設付款方式**廣告活動**在合作夥伴中心內的頁面。

## <a name="prerequisites"></a>必要條件

若要使用這些方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 促銷交 API 的所有[先決條件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。

  > [!NOTE]
  > 隨著必要條件的詳細資訊，請確定您[在合作夥伴中心中建立至少一個付費的廣告活動](../publish/create-an-ad-campaign-for-your-app.md)並在合作夥伴中心內，新增 ad 行銷活動的至少一個付款方式。 您建立使用此 API 的傳遞行會自動向上所選擇的預設付款方式**廣告活動**在合作夥伴中心內的頁面。

* [取得 Azure AD 存取權杖](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這些方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這些方法具有下列 URI。

| 方法類型 | 要求 URI         |  描述  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  建立新的播送行。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  編輯 *lineId* 指定的播送行。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  取得 *lineId* 指定的播送行。  |


### <a name="header"></a>標頭

| 標頭        | 類型   | 描述         |
|---------------|--------|---------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |
| 追蹤識別碼   | GUID   | 選用。 追蹤呼叫流程的識別碼。                                  |


### <a name="request-body"></a>要求本文

POST 和 PUT 方法需要 JSON 要求本文及所需欄位[播送行](#line)物件，以及您想要設定或變更的任何其他欄位。


### <a name="request-examples"></a>要求範例

以下範例示範如何呼叫 POST 方法，以建立播送行。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/line HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Paid Line",
    "configuredStatus": "Active",
    "startDateTime": "2017-01-19T12:09:34Z",
    "endDateTime": "2017-01-31T23:59:59Z",
    "bidAmount": 0.4,
    "dailyBudget": 20,
    "targetProfileId": {
        "id": 310021746
    },
    "creatives": [
        {
            "id": 106851
        }
    ],
    "campaignId": 31043481,
    "minMinutesPerImp ": 1
}
```

以下範例示範如何呼叫 GET 方法，以擷取播送行。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/31019990  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列方法傳回 JSON 回應本文及[播送行](#line)物件，其包含所建立、更新或擷取的播送行相關資訊。 以下為範例示範這些方法的回應本文。

```json
{
    "Data": {
        "id": 31043476,
        "name": "Contoso App Campaign - Paid Line",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "startDateTime": "2017-01-19T12:09:34Z",
        "endDateTime": "2017-01-31T23:59:59Z",
        "createdDateTime": "2017-01-17T10:28:34Z",
        "bidType": "CPM",
        "bidAmount": 0.4,
        "dailyBudget": 20,
        "targetProfileId": {
            "id": 310021746
        },
        "creatives": [
            {
                "id": 106126
            }
        ],
        "campaignId": 31043481,
        "minMinutesPerImp ": 1,
        "pacingType ": "SpendEvenly",
        "currencyId ": 732
    }
}

```


<span id="line"/>

## <a name="delivery-line-object"></a>播送行物件

下列方法的要求和回應本文包含下列欄位。 下表顯示的欄位為唯讀 (亦即們無法在 PUT 方法中變更)，而且僅 POST 或 PUT 方法之要求主體所需的欄位。

| 欄位        | 類型   |  描述      |  唯讀  | 預設值  | POST/PUT 所需 |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整數   |  播送行的識別碼。     |   是    |      |  否      |    
|  name   |  字串   |   播送行的名稱。    |    否   |      |  POST     |     
|  configuredStatus   |  字串   |  下列其中一個值，指定開發人員指定的播送行狀態︰ <ul><li>**使用中**</li><li>**非使用中**</li></ul>     |  否     |      |   POST    |       
|  effectiveStatus   |  字串   |   下列其中一個值，根據系統驗證指定有效的播送行狀態︰ <ul><li>**使用中**</li><li>**非使用中**</li><li>**處理**</li><li>**失敗**</li></ul>    |    是   |      |  否      |      
|  effectiveStatusReasons   |  陣列   |  下列一或多個值，指定有效播送行狀態的原因如下︰ <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  是     |     |    否    |           
|  startDatetime   |  字串   |  播送行的開始日期和時間，格式為 ISO 8601。 如果時間在過去，則無法變更這個值。     |    否   |      |    POST, PUT     |
|  endDatetime   |  字串   |  播送行的結束日期和時間，格式為 ISO 8601。 如果時間在過去，則無法變更這個值。     |   否    |      |  POST, PUT     |
|  createdDatetime   |  字串   |  播送行建立的日期和時間，格式為 ISO 8601。      |    是   |      |  否      |
|  bidType   |  字串   |  指定播送行的宣告類型的值。 目前唯一支援的值為 **CPM**。      |    否   |  CPM    |  否     |
|  bidAmount   |  十進位   |  用於宣告任何廣告要求的出價金額。      |    否   |  根據目標市場的平均 CPM 值 (這個值會定期修改)。    |    否    |  
|  dailyBudget   |  十進位   |  播送行的每日預算。 必須設定 *dailyBudget* 或 *lifetimeBudget*。      |    否   |      |   POST, PUT (如果 *lifetimeBudget* 未設定)       |
|  lifetimeBudget   |  十進位   |   播送行的全週期預算。 必須設定 lifetimeBudget* 或 *dailyBudget*。      |    否   |      |   POST, PUT (如果 *dailyBudget* 未設定)    |
|  targetingProfileId   |  物件   |  在物件上，其識別[目標設定檔](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile)描述使用者、地區和您想要作為播送行目標的庫存廣告類型。 這個物件由指定目標設定檔的識別碼的單一 *id* 欄位組成。     |    否   |      |  否      |  
|  廣告素材   |  陣列   |  一個或多個物件，其代表與播送行相關的[廣告素材](manage-creatives-for-ad-campaigns.md#creative)。 這個欄位中的每一個物件由指定廣告素材的識別碼的單一 *id* 欄位組成。      |    否   |      |   否     |  
|  campaignId   |  整數   |  父系廣告行銷活動的識別碼。      |    否   |      |   否     |  
|  minMinutesPerImp   |  整數   |  指定從這個播送行對相同使用者顯示的兩個廣告曝光數之間的最低時間間隔 (以分鐘計)。      |    否   |  4000    |  否      |  
|  pacingType   |  字串   |  下列其中一個值指定步調類型︰ <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    否   |  SpendEvenly    |  否      |
|  currencyId   |  整數   |  行銷活動的貨幣識別碼。      |    是   |  開發人員帳戶貨幣 (您不需要以 POST 或 PUT 呼叫指定此欄位)    |   否     |      |


## <a name="related-topics"></a>相關主題

* [執行使用 Microsoft Store 服務的廣告活動](run-ad-campaigns-using-windows-store-services.md)
* [管理 ad 行銷活動](manage-ad-campaigns.md)
* [管理 ad 行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [管理 ad 行銷活動的素材](manage-creatives-for-ad-campaigns.md)
* [取得 ad 行銷活動效能資料](get-ad-campaign-performance-data.md)
