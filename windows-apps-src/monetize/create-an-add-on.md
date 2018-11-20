---
author: Xansky
ms.assetid: 5BD650D2-AA26-4DE9-8243-374FDB7D932B
description: 在 Microsoft Store 提交 API 中使用這個方法，來建立附加元件，已登錄到您 PartnerCenter 帳戶的應用程式。
title: 建立附加元件
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 建立附加元件, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: d262a86c4a177095015c3f1391b19f1a7719d0a4
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7419802"
---
# <a name="create-an-add-on"></a>建立附加元件

在 Microsoft Store 提交 API 中使用這個方法，來建立附加元件 （也稱為應用程式內產品或 IAP） 針對已登錄到您的合作夥伴中心帳戶的 app。

> [!NOTE]
> 這個方法會建立一個附加元件但不含任何提交。 若要為附加元件建立提交，請參閱[管理附加元件提交](manage-add-on-submissions.md)中的方法。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-body"></a>要求本文

要求本文包含下列參數。

|  參數  |  類型  |  描述  |  必要  |
|------|------|------|------|
|  applicationIds  |  陣列  |  此陣列包含此附加元件相關聯之 App 的 Store 識別碼。 此陣列只支援一個項目。   |  是  |
|  productId  |  字串  |  附加元件的產品識別碼。 這是可在程式碼中用來參考附加元件的識別碼。 如需詳細資訊，請參閱[設定您的產品類型和產品識別碼](https://msdn.microsoft.com/windows/uwp/publish/set-your-iap-product-id)。  |  是  |
|  productType  |  字串  |  附加元件的產品類型。 支援下列值︰**Durable** 和 **Consumable**。  |  是  |


### <a name="request-example"></a>要求範例

下列範例示範如何為 App 建立新的消耗性附加元件。

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
    "applicationIds": [  "9NBLGGH4R315"  ],
    "productId": "my-new-add-on",
    "productType": "Consumable",
}
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 如需回應本文中各個值的詳細資訊，請參閱[附加元件資源](manage-add-ons.md#add-on-object)。

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
  "productId": "my-new-add-on",
  "productType": "Consumable",
}
```

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 要求無效。 |
| 409  | 無法建立附加元件，因為其目前的狀態，或附加元件使用[「 Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。 |   


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [取得所有附加元件](get-all-add-ons.md)
* [取得附加元件](get-an-add-on.md)
* [刪除附加元件](delete-an-add-on.md)
