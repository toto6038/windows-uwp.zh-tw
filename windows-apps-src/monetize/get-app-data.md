---
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: 請在 Microsoft Store 提交 API 中使用這些方法，來抓取已向合作夥伴中心帳戶註冊的應用程式資料。
title: 取得 App 資料
ms.date: 02/28/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 應用程式資料
ms.localizationpriority: medium
ms.openlocfilehash: cfbe8df46f51b41ccdd840f609caf2c593735e1f
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210974"
---
# <a name="get-app-data"></a>取得 App 資料

在 Microsoft Store 提交 API 中使用下列方法，以取得合作夥伴中心帳戶中現有應用程式的資料。 如需 Microsoft Store 提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

在您可以使用這些方法之前，應用程式必須已經存在於您的合作夥伴中心帳戶中。 若要為應用程式建立或管理提交，請參閱[管理應用程式提交](manage-app-submissions.md)中的方法。

| 方法 | URI                                                                                             | 描述                                                 |
|------- |------------------------------------------------------------------------------------------------ |------------------------------------------------------------ |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications`                                   | [取得所有應用程式的資料](get-all-apps.md)               |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}`                   | [取得特定應用程式的資料](get-an-app.md)                |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts` | [取得應用程式的附加元件](get-add-ons-for-an-app.md)         |
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights`       | [取得應用程式的套件航班](get-flights-for-an-app.md) |

## <a name="prerequisites"></a>必要條件

如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[必要條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然後再嘗試使用這其中的任何方法。

## <a name="data-resources"></a>資料資源

Microsoft Store 提交 API 方法，其使用下列 JSON 資料資源取得應用程式資料。

<span id="application_object" />

### <a name="application-resource"></a>應用程式資源

此資源代表已登錄到您帳戶的應用程式。

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
  },
  "hasAdvancedListingPermission": true
}
```

此資源具有下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|---------------------|
| id            | string  | 應用程式的 Store 識別碼。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)。   |
| primaryName   | string  | 應用程式的主要名稱。      |
| packageFamilyName | string  | 應用程式的套件系列名稱      |
| packageIdentityName          | string  | 應用程式的套件識別資料名稱。                       |
| publisherName       | string  | 與應用程式相關聯的 Windows 發行者識別碼。 這會對應至 [合作夥伴中心] 中應用程式的 [[應用程式識別](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)] 頁面上顯示的 [**套件/身分識別/發行者]** 值。       |
| firstPublishedDate      | string  | 第一次發佈應用程式的日期 (格式為 ISO 8601)。   |
| lastPublishedApplicationSubmission       | object | [提交資源](#submission_object)，其提供應用程式最新發行提交的資訊。    |
| pendingApplicationSubmission        | object  |  [提交資源](#submission_object)，其提供應用程式目前擱置提交的資訊。   |   
| hasAdvancedListingPermission        | boolean  |  指出您是否可以為應用程式的提交設定 [gamingOptions](manage-app-submissions.md#gaming-options-object) 或 [trailers](manage-app-submissions.md#trailer-object)。 這個值對 2017 年 5 月後建立的提交，設為提交。 |  |


<span id="add-on-object" />

### <a name="add-on-resource"></a>附加元件資源

此資源提供附加元件的相關資訊。

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

此資源具有下列值。

| 值           | 類型    | 描述         |
|-----------------|---------|----------------------|
| inAppProductId            | string  | 附加元件的 Store 識別碼。 此值由 Microsoft Store 所提供。 Store 識別碼範例為 9NBLGGH4TNMP。   |


<span id="flight-object" />

### <a name="flight-resource"></a>正式發行前小眾測試版資源

此資源提供應用程式的套件正式發行前小眾測試版相關資訊。

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

| 值           | 類型    | 描述           |
|-----------------|---------|------------------------|
| flightId            | string  | 套件正式發行前小眾測試版的識別碼。 此值是由合作夥伴中心提供。  |
| friendlyName           | string  | 開發人員指定的套件正式發行前小眾測試版名稱。   |
| lastPublishedFlightSubmission       | object | [提交資源](#submission_object)，其提供套件正式發行前小眾測試版最新發行提交的資訊。   |
| pendingFlightSubmission        | object  |  [提交資源](#submission_object)，其提供套件正式發行前小眾測試版目前擱置提交的資訊。  |    
| groupIds           | 陣列  | 此字串陣列包含與套件正式發行前小眾測試版相關聯的正式發行前小眾測試版群組的識別碼。 如需有關正式發行前小眾測試版群組的詳細資訊，請參閱[套件正式發行前小眾測試版](https://docs.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | string  | 排名位於目前套件正式發行前小眾測試版之下的套件正式發行前小眾測試版易記名稱。 如需有關正式發行前小眾測試版群組排名的詳細資訊，請參閱[套件正式發行前小眾測試版](https://docs.microsoft.com/windows/uwp/publish/package-flights)。  |


<span id="submission_object" />

### <a name="submission-resource"></a>提交資源

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

| 值              | 類型   | 描述               |
|--------------------|--------|---------------------------|
| id                 | string | 提交的識別碼。 |
| resourceLocation   | string | 您可以附加到基底 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 以抓取提交完整資料的相對路徑。 |

 
## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務來建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 來管理應用程式提交](manage-app-submissions.md)
* [取得所有應用程式](get-all-apps.md)
* [取得應用程式](get-an-app.md)
* [取得應用程式的附加元件](get-add-ons-for-an-app.md)
* [取得應用程式的套件航班](get-flights-for-an-app.md)
