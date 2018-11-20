---
author: Xansky
ms.assetid: 4F9657E5-1AF8-45E0-9617-45AF64E144FC
description: 在 Microsoft Store 提交 API 中使用這些方法，來管理附加元件，來為登錄到您的合作夥伴中心帳戶的應用程式。
title: 管理附加元件
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 附加元件, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: c3861d5a135e0ee7a688a93a57cce801a05cf9a5
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7288071"
---
# <a name="manage-add-ons"></a>管理附加元件

使用 Microsoft Store 提交 API 中的下列方法，管理應用程式的附加元件。 如需 Microsoft Store 提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

這些方法只可用來取得、建立或刪除附加元件。 若要為附加元件建立提交，請參閱[管理附加元件提交](manage-add-on-submissions.md)中的方法。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="get-all-add-ons.md">取得您所有應用程式的附加元件</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="get-an-add-on.md">取得特定的附加元件</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts</td>
<td align="left"><a href="create-an-add-on.md">建立附加元件</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}</td>
<td align="left"><a href="delete-an-add-on.md">刪除附加元件</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>必要條件

如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[必要條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)，然後再嘗試使用這其中的任何方法。

## <a name="data-resources"></a>資料資源

用於管理加元件的 Microsoft Store 提交 API 方法，使用下列 JSON 資料資源。

<span id="add-on-object" />

### <a name="add-on-resource"></a>附加元件資源

此資源描述附加元件。

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

| 值      | 類型   | 描述        |
|------------|--------|--------------|
| applications      | 陣列  | 包含一個[應用程式資源](#application-object)的陣列，其代表與此附加元件相關聯之應用程式。 此陣列只支援一個項目。  |
| id | 字串  | 附加元件的 Store 識別碼。 此值由 Microsoft Store 所提供。  Store 識別碼範例為 9NBLGGH4TNMP。  |
| productId | 字串  | 附加元件的產品識別碼。 這是建立附加元件時，開發人員所提供的識別碼。 如需詳細資訊，請參閱[設定您的產品類型和產品識別碼](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。 |
| productType | 字串  | 附加元件的產品類型。 支援下列值︰**Durable** 和 **Consumable**。  |
| lastPublishedInAppProductSubmission       | 物件 | [提交資源](#submission-object)，其提供附加元件最新發行的提交相關資訊。         |
| pendingInAppProductSubmission        | 物件  |  [提交資源](#submission-object)，其提供附加元件目前擱置提交的資訊。  |   |

<span id="application-object" />

### <a name="application-resource"></a>應用程式資源

此資源描述與附加元件相關聯的應用程式。 下列範例示範此資源的格式。

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

| 值           | 類型    | 描述        |
|-----------------|---------|-----------|
| value            | 物件  |  此物件包含下列值： <br/><br/> <ul><li>*id*。應用程式的 Store 識別碼。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視應用程式身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。</li><li>*resourceLocation*。 您可以附加到基底 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 以抓取應用程式完整資料的相對路徑。</li></ul>   |
| totalCount   | 整數  | 回應內文的 applications** 陣列中的應用程式物件數目。                                                                                                                                                 |

<span id="submission-object" />

### <a name="submission-resource"></a>提交資源

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

| 值           | 類型    | 描述     |
|-----------------|---------|------------------|
| id            | 字串  | 提交的識別碼。    |
| resourceLocation   | 字串  | 您可以附加到基底 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 以抓取提交完整資料的相對路徑。     |
 
<span/>

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 管理附加元件提交](manage-add-on-submissions.md)
* [取得所有附加元件](get-all-add-ons.md)
* [取得附加元件](get-an-add-on.md)
* [建立附加元件](create-an-add-on.md)
* [刪除附加元件](delete-an-add-on.md)
