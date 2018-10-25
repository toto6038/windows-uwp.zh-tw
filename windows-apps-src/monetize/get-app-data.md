---
author: Xansky
ms.assetid: 8D4AE532-22EF-4743-9555-A828B24B8F16
description: 在 Microsoft Store 提交 API 中使用這些方法，針對已登錄到您 Windows 開發人員中心帳戶的應用程式擷取資料。
title: 取得 App 資料
ms.author: mhopkins
ms.date: 02/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 提交 API, 應用程式資料
ms.localizationpriority: medium
ms.openlocfilehash: 6940c1079c7973bc4fd639345c5d5e3f33b0221f
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5520920"
---
# <a name="get-app-data"></a>取得 App 資料

在 Microsoft Store 提交 API 中使用下列方法，取得您開發人員中心帳戶中現有應用程式的資料。 如需 Microsoft Store 提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

使用這些方法之前，應用程式必須已經存在於開發人員中心帳戶中。 若要為應用程式建立或管理提交，請參閱[管理應用程式提交](manage-app-submissions.md)中的方法。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications</td>
<td align="left"><a href="get-all-apps.md">取得您所有應用程式的資料</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}</td>
<td align="left"><a href="get-an-app.md">取得特定應用程式的資料</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts</td>
<td align="left"><a href="get-add-ons-for-an-app.md">取得應用程式的附加元件</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights</td>
<td align="left"><a href="get-flights-for-an-app.md">取得應用程式套件正式發行前小眾測試版</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="prerequisites"></a>先決條件

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
| id            | 字串  | 應用程式的 Store 識別碼。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視應用程式身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。   |
| primaryName   | 字串  | 應用程式的主要名稱。      |
| packageFamilyName | 字串  | 應用程式的套件系列名稱      |
| packageIdentityName          | 字串  | 應用程式的套件識別資料名稱。                       |
| publisherName       | 字串  | 與應用程式相關聯的 Windows 發行者識別碼。 這會對應到 Windows 開發人員中心儀表板中應用程式的[應用程式身分識別](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details) 頁面上顯示的 **Package/Identity/Publisher** 值。       |
| firstPublishedDate      | 字串  | 第一次發佈應用程式的日期 (格式為 ISO 8601)。   |
| lastPublishedApplicationSubmission       | 物件 | [提交資源](#submission_object)，其提供應用程式最新發行提交的資訊。    |
| pendingApplicationSubmission        | 物件  |  [提交資源](#submission_object)，其提供應用程式目前擱置提交的資訊。   |   
| hasAdvancedListingPermission        | 布林值  |  指出您是否可以為應用程式的提交設定 [gamingOptions](manage-app-submissions.md#gaming-options-object) 或 [trailers](manage-app-submissions.md#trailer-object)。 這個值對 2017 年 5 月後建立的提交，設為提交。 |  |


<span id="add-on-object" />

### <a name="add-on-resouce"></a>附加元件資源

此資源提供附加元件的相關資訊。

```json
{
    "inAppProductId": "9WZDNCRD7DLK"
}
```

此資源具有下列值。

| 值           | 類型    | 描述         |
|-----------------|---------|----------------------|
| inAppProductId            | 字串  | 附加元件的 Store 識別碼。 此值由 Microsoft Store 所提供。  Store 識別碼範例為 9NBLGGH4TNMP。   |


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
| flightId            | 字串  | 套件正式發行前小眾測試版的識別碼。 此值由開發人員中心提供。  |
| friendlyName           | 字串  | 開發人員指定的套件正式發行前小眾測試版名稱。   |
| lastPublishedFlightSubmission       | 物件 | [提交資源](#submission_object)，其提供套件正式發行前小眾測試版最新發行提交的資訊。   |
| pendingFlightSubmission        | 物件  |  [提交資源](#submission_object)，其提供套件正式發行前小眾測試版目前擱置提交的資訊。  |    
| groupIds           | 陣列  | 此字串陣列包含與套件正式發行前小眾測試版相關聯的正式發行前小眾測試版群組的識別碼。 如需有關正式發行前小眾測試版群組的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | 字串  | 排名位於目前套件正式發行前小眾測試版之下的套件正式發行前小眾測試版易記名稱。 如需有關正式發行前小眾測試版群組排名的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |


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

| 值           | 類型    | 描述                 |
|-----------------|---------|------------------------------|
| id            | 字串  | 提交的識別碼。    |
| resourceLocation   | 字串  | 您可以附加到基底 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 以抓取提交完整資料的相對路徑。            |
 
<span/>

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 管理 App 提交](manage-app-submissions.md)
* [取得所有應用程式](get-all-apps.md)
* [取得 App](get-an-app.md)
* [取得應用程式的附加元件](get-add-ons-for-an-app.md)
* [取得應用程式套件正式發行前小眾測試版](get-flights-for-an-app.md)
