---
author: mcleanbyron
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理附加元件。"
title: "使用 Windows 市集提交 API 管理附加元件"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 75548ee4689fd31d734c570f8e3eca5d33a6181f

---

# 使用 Windows 市集提交 API 管理附加元件




在 Windows 市集提交 API 中使用下列方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理附加元件 (亦稱為 App 內產品或 IAP)。 如需 Windows 市集提交 API 的簡介，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 這些方法只可用來取得、建立或刪除附加元件。 若要為附加元件建立提交，請參閱[管理附加元件提交](manage-add-on-submissions.md)中的方法。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | 為登錄到您 Windows 開發人員中心帳戶的所有 App，取得所有附加元件的資料。 如需詳細資訊，請參閱[取得所有附加元件](get-all-add-ons.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId} ``` | 取得特定附加元件的資料。 如需詳細資訊，請參閱[取得附加元件](get-an-add-on.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` | 建立新的附加元件。 如需詳細資訊，請參閱[建立附加元件](create-an-add-on.md)。  |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` | 刪除附加元件。 如需詳細資訊，請參閱[刪除附加元件](delete-an-add-on.md)。 |

## 先決條件

如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然後再嘗試使用這其中的任何方法。

## 資源

這些方法會使用下列資源來格式化資料。

<span id="add-on-object" />
### 附加元件

此資源代表附加元件。 下列範例示範此資源的格式。

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
  "id": "9NBLGGH4TNMP",
  "productId": "TestAddOn",
  "productType": "Durable",
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
  "lastPublishedInAppProductSubmission": {
    "id": "1152921504621243705",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
  }
}
```

此資源具有下列值。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applications      | 陣列  | 此陣列包含代表此附加元件相關聯之 App 的物件。 如需此物件中資料的詳細資訊，請參閱下方的[應用程式物件](#application-object)一節。 此陣列只支援一個項目。  |
| id | 字串  | 附加元件的市集識別碼。 此值由市集所提供。 市集識別碼範例為 9NBLGGH4TNMP。  |
| productId | 字串  | 附加元件的產品識別碼。 這是建立附加元件時，開發人員所提供的識別碼。 如需詳細資訊，請參閱[設定您的產品類型和產品識別碼](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。 |
| productType | 字串  | 附加元件的產品類型。 支援下列值︰**Durable** 和 **Consumable**。  |
| lastPublishedInAppProductSubmission       | 物件 | 此物件可提供附加元件最後一個發佈之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交](#submission-object)一節。                                                                                                                                                          |
| pendingInAppProductSubmission        | 物件  |  此物件可提供附加元件目前擱置中之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交](#submission-object)一節。  |   |

<span id="application-object" />
### 應用程式

此資源代表附加元件相關聯的 App。 下列範例示範此資源的格式。

```json
{
  "applications": {
    "value": [
      {
        "id": "9NBLGGH4R315",
        "resourceLocation": "applications/9NBLGGH4R315"
      }
    ],
    "totalCount": 1
  },
}
```

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|-----------|
| value            | 物件  |  此物件包含下列值： <br/><br/> <ul><li>id**。 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。</li><li>resourceLocation**。 您可以附加到基底 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 以抓取 App 完整資料的相對路徑。</li></ul>   |
| totalCount   | 整數  | 回應內文的 applications** 陣列中的 App 物件數目。                                                                                                                                                 |

<span id="submission-object" />
### 提交

此資源提供附加元件的提交相關資訊。 下列範例示範此資源的格式。

```json
{
  "pendingInAppProductSubmission": {
    "id": "1152921504621243619",
    "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
  },
}
```

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字串  | 提交的識別碼。    |
| resourceLocation   | 字串  | 您可以附加到基底 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 以抓取提交完整資料的相對路徑。                                                                                                                                               |
 
<span/>

## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 市集提交 API 管理附加元件提交](manage-add-on-submissions.md)
* [取得所有附加元件](get-all-add-ons.md)
* [取得附加元件](get-an-add-on.md)
* [建立附加元件](create-an-add-on.md)
* [刪除附加元件](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


