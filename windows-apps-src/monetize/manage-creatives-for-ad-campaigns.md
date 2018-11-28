---
ms.assetid: c5246681-82c7-44df-87e1-a84a926e6496
description: 在 Microsoft Store 促銷 API 中使用此方法，管理適用於廣告行銷活動的廣告素材。
title: 管理廣告素材
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 促銷 API, 廣告行銷活動
ms.localizationpriority: medium
ms.openlocfilehash: 41c11ee9c5decffff57a2d443e1385398ce40d89
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7832178"
---
# <a name="manage-creatives"></a>管理廣告素材

在 Microsoft Store 促銷 API 使用這些方法，上傳您自己的自訂廣告素以用於廣告行銷活動，或取得現有的廣告素材。 廣告素材可能會與一個或多個廣告播送行相關聯，甚至跨廣告行銷活動，但前提是它永遠代表相同 app。

如需有關廣告素材廣告行銷活動、播送行和目標設定檔之間關聯性的詳細資訊，請參閱[使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

> [!NOTE]
> 使用此 API 上傳您自己的廣告素材時，允許的廣告素材大小上限為 40 KB。 如果您提交的廣告素材檔案大於此上限，這個 API 將不會傳回錯誤，但不會成功建立行銷活動。

## <a name="prerequisites"></a>必要條件

若要使用這些方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 促銷交 API 的所有[先決條件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這些方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。


## <a name="request"></a>要求

這些方法具有下列 URI。

| 方法類型 | 要求 URI     |  描述  |
|--------|-----------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative``` |  建立新廣告素材。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/{creativeId}``` |  取得 *creativeId* 指定的廣告素材。  |

> [!NOTE]
> 此 API 目前不支援 PUT 方法。


### <a name="header"></a>標頭

| 標頭        | 類型   | 描述         |
|---------------|--------|---------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |
| 追蹤識別碼   | GUID   | 選用。 追蹤呼叫流程的識別碼。                                  |


### <a name="request-body"></a>要求本文

POST 方法需要 JSON 要求本文，以及[廣告素材](#creative)物件之必要欄位。


### <a name="request-examples"></a>要求範例

以下範例示範如何呼叫 POST 方法，以建立廣告素材。 在此範例中，為了簡短而縮短 *content* 值。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative HTTP/1.1
Authorization: Bearer <your access token>

{
  "name": "Contoso App Campaign - Creative 1",
  "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
  "height": 80,
  "width": 480,
  "imageAttributes":
  {
    "imageExtension": "PNG"
  }
}
```

以下範例示範如何呼叫 GET 方法，以擷取廣告素材。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/creative/106851  HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>回應

下列方法傳回 JSON 回應本文及[廣告素材](#creative)物件，其包含所建立或擷取之廣告素材的相關資訊。 以下為範例示範這些方法的回應本文。 在此範例中，為了簡短而縮短 *content* 值。

```json
{
    "Data": {
        "id": 106126,
        "name": "Contoso App Campaign - Creative 2",
        "content": "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQEAAQABAAD/2wBDAAgGB...other base64 data shortened for brevity...",
        "height": 50,
        "width": 300,
        "format": "Banner",
        "imageAttributes":
        {
          "imageExtension": "PNG"
        },
        "storeProductId": "9nblggh42cfd"
    }
}
```


<span id="creative"/>

## <a name="creative-object"></a>廣告素材物件

下列方法的要求和回應本文包含下列欄位。 下表顯示的欄位為唯讀 (亦即們無法在 PUT 方法中變更)，而且僅 POST 方法之要求本文所需的欄位。

| 欄位        | 類型   |  描述      |  唯讀  | 預設值  |  POST 所需 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  整數   |  廣告素材的識別碼。     |   是    |      |    否   |       
|  name   |  字串   |   廣告素材的名稱。    |    否   |      |  是     |       
|  內容   |  字串   |  廣告素材影像的內容，含 Base64 編碼格式。<br/><br/>**注意**&nbsp;&nbsp;允許的廣告素材大小上限為 40 KB。 如果您提交的廣告素材檔案大於此上限，這個 API 將不會傳回錯誤，但不會成功建立行銷活動。     |  否     |      |   是    |       
|  height   |  整數   |   廣告素材的高度。    |    否    |      |   是    |       
|  width   |  整數   |  廣告素材的寬度。     |  否    |     |    是   |       
|  landingUrl   |  字串   |  如果您使用行銷活動追蹤服務例如 Kochava、AppsFlyer 或 Tune，來衡量 App 安裝分析，當您呼叫 POST 方法時，在這個欄位中指派追蹤 URL（如果指定，這個值必須是有效 URI）。 如果您未使用行銷活動追蹤服務，當您呼叫 POST 方法時略過此值（在這個情況下，就會自動建立此 URL）。   |  否    |     |   是    |       
|  format   |  字串   |   廣告格式。 目前唯一支援的值為 **Banner**。    |   否    |  橫幅   |  否     |       
|  imageAttributes   | [ImageAttributes](#image-attributes)    |   提供廣告素材的屬性。     |   否    |      |   是    |       
|  storeProductId   |  字串   |   此廣告行銷活動所關聯之應用程式的[ Store 識別碼](in-app-purchases-and-trials.md#store-ids)。 例如產品 Store 識別碼為 9nblggh42cfd。    |   否    |    |  否     |   |  


<span id="image-attributes"/>

## <a name="imageattributes-object"></a>ImageAttributes 物件

| 欄位        | 類型   |  描述      |  唯讀  | 預設值  | POST 所需 |  
|--------------|--------|---------------|------|-------------|------------|
|  imageExtension   |   字串  |   下列其中一個值：**PNG** 或 **JPG**。    |    否   |      |   是    |       |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務執行廣告行銷活動](run-ad-campaigns-using-windows-store-services.md)
* [管理廣告行銷活動](manage-ad-campaigns.md)
* [管理廣告行銷活動的廣告播送行](manage-delivery-lines-for-ad-campaigns.md)
* [管理廣告行銷活動的目標設定檔](manage-targeting-profiles-for-ad-campaigns.md)
* [取得廣告行銷活動績效資料](get-ad-campaign-performance-data.md)
