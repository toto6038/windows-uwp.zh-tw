---
ms.assetid: dc632a4c-ce48-400b-8e6e-1dddbd13afff
description: 在 Microsoft Store 促銷 API 中使用此方法，管理適用於廣告行銷活動的播送行。
title: 管理廣告播送行
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 28db33951df79700c58a86d1d3189744028abcc4
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619564"
---
# <a name="manage-delivery-lines"></a>管理廣告播送行

在 Microsoft Store 促銷 API 使用這些方法，建立一個或多個 *播送行* 購買詳細目錄，並提供您的用於促銷活動的廣告。 對於每個廣告播送行，您可以設為目標，設定買方出價，以及設定預算和要使用之廣告素材的連結，決定要花多少費用。

如需有關播送行和廣告行銷活動、目標設定檔及廣告素材之間關聯性的詳細資訊，請參閱[使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

> &nbsp; 注意 &nbsp;您必須先 [使用合作夥伴中心中的 [ **ad 活動**] 頁面建立一個付費的 ad 活動](./index.md)，才能成功建立使用此 API 的廣告活動傳遞行，而且您必須在此頁面上至少新增一個付款條件。 執行此動作之後，您就可以使用此 API 成功地建立廣告行銷活動的可計費廣告播送行。 您使用 API 所建立的 ad 活動會自動為合作夥伴中心中的 [ **ad 活動** ] 頁面上選擇的預設付款條件產生帳單。

## <a name="prerequisites"></a>必要條件

若要使用這些方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 促銷交 API 的所有[先決條件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。

  > [!NOTE]
  > 作為必要條件的一部分，請務必在 [合作夥伴中心中建立至少一個付費的 ad 活動](./index.md) ，並在合作夥伴中心中新增至少一個適用于 ad 活動的付款條件。 您使用此 API 建立的傳遞行，會自動為合作夥伴中心中 [ **Ad 活動** ] 頁面上選擇的預設付款條件產生帳單。

* [取得 Azure AD 存取權杖](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這些方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這些方法具有下列 URI。

| 方法類型 | 要求 URI         |  Description  |
|--------|---------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line``` |  建立新的播送行。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  編輯 *lineId* 指定的播送行。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/line/{lineId}``` |  取得 *lineId* 指定的播送行。  |


### <a name="header"></a>標頭

| 標頭        | 類型   | 描述         |
|---------------|--------|---------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |
| 追蹤識別碼   | GUID   | 選擇性。 追蹤呼叫流程的識別碼。                                  |


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

| 欄位        | 類型   |  Description      |  唯讀  | 預設  | POST/PUT 所需 |   
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整數   |  播送行的識別碼。     |   是    |      |  否      |    
|  NAME   |  字串   |   播送行的名稱。    |    No   |      |  POST     |     
|  configuredStatus   |  字串   |  下列其中一個值，指定開發人員指定的播送行狀態︰ <ul><li>**使用中**</li><li>**非作用中**</li></ul>     |  No     |      |   POST    |       
|  effectiveStatus   |  字串   |   下列其中一個值，根據系統驗證指定有效的播送行狀態︰ <ul><li>**使用中**</li><li>**非作用中**</li><li>**Processing**</li><li>**失敗**</li></ul>    |    是   |      |  否      |      
|  effectiveStatusReasons   |  array   |  下列一或多個值，指定有效播送行狀態的原因如下︰ <ul><li>**AdCreativesInactive**</li><li>**ValidationFailed**</li></ul>      |  是     |     |    否    |           
|  startDatetime   |  字串   |  播送行的開始日期和時間，格式為 ISO 8601。 如果時間在過去，則無法變更這個值。     |    No   |      |    POST, PUT     |
|  endDatetime   |  字串   |  播送行的結束日期和時間，格式為 ISO 8601。 如果時間在過去，則無法變更這個值。     |   No    |      |  POST, PUT     |
|  createdDatetime   |  字串   |  播送行建立的日期和時間，格式為 ISO 8601。      |    是   |      |  否      |
|  bidType   |  字串   |  指定播送行的宣告類型的值。 目前唯一支援的值為 **CPM**。      |    No   |  CPM    |  No     |
|  bidAmount   |  decimal   |  用於宣告任何廣告要求的出價金額。      |    No   |  根據目標市場的平均 CPM 值 (這個值會定期修改)。    |    No    |  
|  dailyBudget   |  decimal   |  播送行的每日預算。 必須設定 *dailyBudget* 或 *lifetimeBudget*。      |    No   |      |   POST, PUT (如果 *lifetimeBudget* 未設定)       |
|  lifetimeBudget   |  decimal   |   播送行的全週期預算。 必須設定 lifetimeBudget * 或 *dailyBudget* 。      |    No   |      |   POST, PUT (如果 *dailyBudget* 未設定)    |
|  targetingProfileId   |  物件 (object)   |  在物件上，其識別[目標設定檔](manage-targeting-profiles-for-ad-campaigns.md#targeting-profile)描述使用者、地區和您想要作為播送行目標的庫存廣告類型。 這個物件由指定目標設定檔的識別碼的單一 *id* 欄位組成。     |    否   |      |  否      |  
|  廣告素材   |  array   |  一個或多個物件，其代表與播送行相關的[廣告素材](manage-creatives-for-ad-campaigns.md#creative)。 這個欄位中的每一個物件由指定廣告素材的識別碼的單一 *id* 欄位組成。      |    否   |      |   否     |  
|  campaignId   |  整數   |  父系廣告行銷活動的識別碼。      |    否   |      |   否     |  
|  minMinutesPerImp   |  整數   |  指定從這個播送行對相同使用者顯示的兩個廣告曝光數之間的最低時間間隔 (以分鐘計)。      |    No   |  4000    |  No      |  
|  pacingType   |  字串   |  下列其中一個值指定步調類型︰ <ul><li>**SpendEvenly**</li><li>**SpendAsFastAsPossible**</li></ul>      |    No   |  SpendEvenly    |  No      |
|  currencyId   |  整數   |  行銷活動的貨幣識別碼。      |    Yes   |  開發人員帳戶貨幣 (您不需要以 POST 或 PUT 呼叫指定此欄位)    |   No     |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md)
* [管理廣告行銷活動](manage-ad-campaigns.md)
* [管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)
* [取得廣告活動績效資料](get-ad-campaign-performance-data.md)