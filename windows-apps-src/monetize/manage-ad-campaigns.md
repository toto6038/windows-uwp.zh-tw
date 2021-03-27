---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: 在 Microsoft Store 中使用此方法促銷 API，建立、編輯及獲得促銷廣告活動。
title: 管理廣告行銷活動
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 0c985c9a53c233ae0c433567fcd0f88da1428542
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619624"
---
# <a name="manage-ad-campaigns"></a>管理廣告行銷活動

使用這些方法在 [Microsoft Store 促銷 API](run-ad-campaigns-using-windows-store-services.md)，為您的應用程式建立、編輯及取得促銷廣告活動。 您使用此方法建立的每個活動，只能和一個應用程式相關聯。

> &nbsp; 注意 &nbsp;您也可以使用合作夥伴中心建立和管理 ad 活動，您可以在合作夥伴中心中存取以程式設計方式建立的行銷活動。 如需在合作夥伴中心中管理 ad 活動的詳細資訊，請參閱為 [您的應用程式建立 ad 行銷活動](./index.md)。

當您使用這些方法建立或更新活動時，通常也呼叫下列一或多個管理方法 *播送行*、*目標設定檔* 及 *廣告素材* 相關的活動。 如需有關播送行、目標設定檔及廣告素材之間關聯性的詳細資訊，請參閱[使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

* [管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)
* [管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>必要條件

若要使用這些方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 促銷交 API 的所有[先決條件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。

  > &nbsp; 注意 &nbsp;作為必要條件的一部分，請務必在[合作夥伴中心中建立至少一個付費的 ad 活動](./index.md)，並在合作夥伴中心中新增至少一個適用于 ad 活動的付款條件。 您使用此 API 建立的 ad 活動傳遞行，會自動為合作夥伴中心中的 [ **ad 活動** ] 頁面上選擇的預設付款條件產生帳單。

* [取得 Azure AD 存取權杖](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這些方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。


## <a name="request"></a>要求

這些方法具有下列 URI。

| 方法類型 | 要求 URI                                                      |  Description  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  建立新的廣告行銷活動。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  編輯 *campaignId* 指定的廣告行銷活動。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  取得 *campaignId* 指定的廣告行銷活動。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  廣告行銷活動查詢。 請參閱查詢參數支援的[參數](#parameters)一節。  |


### <a name="header"></a>標頭

| 標頭        | 類型   | 描述         |
|---------------|--------|---------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |
| 追蹤識別碼   | GUID   | 選擇性。 追蹤呼叫流程的識別碼。                                  |


<span id="parameters"/> 

### <a name="parameters"></a>參數

用於廣告活動查詢的 GET 方法，支援下列選擇性查詢參數。

| 名稱        | 類型   |  Description      |    
|-------------|--------|---------------|
| skip  |  int   | 在查詢中要略過的資料列數目。 使用此參數來瀏覽資料集。 例如，fetch=10 且 skip=0 將擷取前 10 個資料列的資料，top=10 且 skip=10 將擷取下 10 個資料列的資料，以此類推。    |       
| fetch  |  int   | 在要求中傳回的資料列數目。    |       
| campaignSetSortColumn  |  字串   | 回應主體中指定欄位的訂購[活動](#campaign)物件。 語法為 <em>CampaignSetSortColumn=field</em>，其中 <em>field</em> 參數可以是下列其中一個字串︰</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>預設值為 **createdDateTime**。     |     
| isDescending  |  Boolean   | 回應主體中以遞減或增遞順序排序[行銷活動](#campaign)物件。   |         
| storeProductId  |  字串   | 使用這個值只傳回廣告活動相關的應用程式，含指定的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 例如產品 Store 識別碼為 9nblggh42cfd。   |         
| label  |  字串   | 使用這個值只傳回廣告活動，其在 [行銷活動](#campaign)物件中包含指定的 *標籤*。    |


### <a name="request-body"></a>要求本文

POST 和 PUT 方法需要 JSON 要求本文及所需欄位[行銷活動](#campaign)物件，以及您想要設定或變更的任何其他欄位。


### <a name="request-examples"></a>要求範例

以下範例示範如何呼叫 POST 方法建立廣告行銷活動。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

以下範例示範如何呼叫 GET 方法擷取特定廣告行銷活動。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

以下範例示範如何呼叫 GET 方法查詢一組廣告行銷活動，以建立的日期排序。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>回應

下列方法傳回 JSON 回應主體中的一或多[行銷活動](#campaign)物件，視您所呼叫的方法而定。 以下為範例示範特定行銷活動的 GET 方法的回應主體。

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>行銷活動物件

下列方法的要求和回應本文包含下列欄位。 下表顯示的欄位為唯讀 (亦即們無法在 PUT 方法中變更)，而且僅 POST 方法之要求本文所需的欄位。

| 欄位        | 類型   |  Description      |  唯讀  | 預設  | POST 所需 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整數   |  廣告行銷活動的識別碼。     |   是    |      |  否     |       
|  NAME   |  字串   |   廣告行銷活動的名稱。    |    否   |      |  是     |       
|  configuredStatus   |  字串   |  下列其中一個值，指定開發人員指定的廣告行銷活動狀態︰ <ul><li>**使用中**</li><li>**非作用中**</li></ul>     |  No     |  使用中    |   Yes    |       
|  effectiveStatus   |  字串   |   下列其中一個值，根據系統驗證指定有效的廣告行銷活動狀態︰ <ul><li>**使用中**</li><li>**非作用中**</li><li>**Processing**</li></ul>    |    是   |      |   否      |       
|  effectiveStatusReasons   |  array   |  下列一或多個值，指定有效廣告行銷活動狀態的原因如下︰ <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**失敗**</li></ul>      |  是     |     |    否     |       
|  storeProductId   |  字串   |  此廣告行銷活動所關聯之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 例如產品 Store 識別碼為 9nblggh42cfd。     |   是    |      |  是     |       
|  標籤   |  array   |   一或多個字串，表示自訂活動的標籤。 使用下列的標籤來搜尋及標記活動。    |   No    |  null    |    否     |       
|  type   | 字串    |  下列其中一個值，指定活動類型︰ <ul><li>**已支付**</li><li>**門牌**</li><li>**社群**</li></ul>      |   是    |      |   是    |       
|  objective   |  字串   |  下列其中一個值，指定行銷活動的目標︰ <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   No    |  DriveInstall    |   Yes    |       
|  lines   |  array   |   一或多個物件，識別與行銷活動相關[播送行](manage-delivery-lines-for-ad-campaigns.md#line)。 每個這個欄位中的物件由 *識別碼* 和 *名稱* 欄位組成，指定廣告播送行的識別碼和名稱。     |   否    |      |    否     |       
|  createdDate   |  字串   |  廣告行銷活動的建立日期和時間，採用 ISO 8601 格式。     |  是     |      |     否    |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md)
* [管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)
* [管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [管理廣告行銷活動的廣告素材](manage-creatives-for-ad-campaigns.md)
* [取得廣告活動績效資料](get-ad-campaign-performance-data.md)