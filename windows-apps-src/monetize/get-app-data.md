---
author: mcleanbyron
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: "在 Windows 市集提交 API 中使用這些方法，針對已登錄到您 Windows 開發人員中心帳戶的 App 擷取資料。"
title: "使用 Windows 市集提交 API 取得 App 資料"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99d609decc8c38952961deac5bb8ec6926d91c88

---

# 使用 Windows 市集提交 API 取得 App 資料

在 Windows 市集提交 API 中使用下列方法，針對已登錄到您 Windows 開發人員中心帳戶的 App 取得資料。 如需 Windows 市集提交 API 的簡介，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**  這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 這些方法僅能用來取得 App 的資料。 若要為附加元件建立或管理提交，請參閱[管理 App 提交](manage-app-submissions.md)中的方法。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` | 針對已登錄到您 Windows 開發人員中心帳戶的所有 App 擷取資料。 如需詳細資訊，請參閱[取得所有 App](get-all-apps.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}``` | 針對已登錄到您 Windows 開發人員中心帳戶的特定 App 擷取相關資料。 如需詳細資訊，請參閱[取得 App](get-an-app.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` | 針對登錄到您 Windows 開發人員中心帳戶的 App 列出附加元件 (也稱為應用程式內產品或 IAP)。 如需詳細資訊，請參閱[取得 App 的附加元件](get-add-ons-for-an-app.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` | 針對已登錄到您 Windows 開發人員中心帳戶的 App 列出套件正式發行前小眾測試版。 如需詳細資訊，請參閱[取得 App 的套件正式發行前小眾測試版](get-flights-for-an-app.md)。 |

<span/>

## 先決條件

如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然後再嘗試使用這其中的任何方法。

## 資源

這些方法會使用下列資源來格式化資料。

<span id="application_object" />
### 應用程式

此資源代表已登錄到您帳戶的應用程式。 下列範例示範此資源的格式。

```json
{
  "id": "9NBLGGH4R315",
  "primaryName": "ApiTestApp",
  "packageFamilyName": "30481DevCenterAPITester.ApiTestAppForDevbox_ng6try80pwt52",
  "packageIdentityName": "30481DevCenterAPITester.ApiTestAppForDevbox",
  "publisherName": "CN=…",
  "firstPublishedDate": "1601-01-01T00:00:00Z",
  "lastPublishedApplicationSubmission": {
    "id": "1152921504621086517",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621086517"
  },
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621243487"
  }
}
```

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字串  | App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。   |
| primaryName   | 字串  | App 的主要名稱。                                                                                                                                                   |
| packageFamilyName | 字串  | App 的套件系列名稱                                                                                                                                                                                                         |
| packageIdentityName          | 字串  | App 的套件識別資料名稱。                                                                                                                                                              |
| publisherName       | 字串  | 與 App 相關聯的 Windows 發行者識別碼。 這會對應到 Windows 開發人員中心儀表板中 App 的 [App 身分識別](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) 頁面上顯示的 **Package/Identity/Publisher** 值。                                                                                             |
| firstPublishedDate      | 字串  | 第一次發佈 App 的日期 (格式為 ISO 8601)。                                                                                         |
| lastPublishedApplicationSubmission       | object | 此物件可提供 App 最後一個發佈之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交](#submission_object)一節。                                                                                                                                                          |
| pendingApplicationSubmission        | object  |  此物件可提供 App 目前擱置中之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交](#submission_object)一節。  |   |


<span id="add-on-object" />
### 附加元件

此資源提供附加元件的相關資訊。 下列範例示範此資源的格式。

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| inAppProductId            | 字串  | 附加元件的市集識別碼。 此值由市集所提供。 市集識別碼範例為 9NBLGGH4TNMP。   |


<span id="flight-object" />
### 正式發行前小眾測試版

此資源提供 App 的套件正式發行前小眾測試版相關資訊。 下列範例示範此資源的格式。

```json
{
    "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
    "friendlyName": "myflight",
    "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
    },
    "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
    },
    "groupIds": [
        "1152921504606962205"
    ],
    "rankHigherThan": "Non-flighted submission"
}
```

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 字串  | 套件正式發行前小眾測試版的識別碼。 此值由開發人員中心提供。  |
| friendlyName           | 字串  | 開發人員指定的套件正式發行前小眾測試版名稱。   |
| lastPublishedFlightSubmission       | object | 此物件可提供套件正式發行前小眾測試版最後一個發佈之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交](#submission_object)一節。  |
| pendingFlightSubmission        | object  |  此物件可提供套件正式發行前小眾測試版目前擱置中之提交的相關資訊。 如需詳細資訊，請參閱下方的[提交](#submission_object)一節。  |    
| groupIds           | 陣列  | 此字串陣列包含與套件正式發行前小眾測試版相關聯的正式發行前小眾測試版群組的識別碼。 如需有關正式發行前小眾測試版群組的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | 字串  | 排名位於目前套件正式發行前小眾測試版之下的套件正式發行前小眾測試版易記名稱。 如需有關正式發行前小眾測試版群組排名的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |


<span id="submission_object" />
### 提交

此資源提供提交的相關資訊。 下列範例示範此資源的格式。

```json
{
  "pendingApplicationSubmission": {
    "id": "1152921504621243487",
    "resourceLocation": "applications/9WZDNCRD9MMD/submissions/1152921504621243487"
  }
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
* [使用 Windows 市集提交 API 管理 App 提交](manage-app-submissions.md)
* [取得所有 App](get-all-apps.md)
* [取得 App](get-an-app.md)
* [取得 App 的附加元件](get-add-ons-for-an-app.md)
* [取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)



<!--HONumber=Aug16_HO5-->


