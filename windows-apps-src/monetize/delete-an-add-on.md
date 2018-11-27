---
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: 在 Microsoft Store 提交 API 中使用這個方法，刪除附加元件，已登錄到您的合作夥伴中心帳戶的應用程式。
title: 刪除附加元件
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 附加元件, 刪除, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 837cbc19268a88be986068f4a5e60002a1eb55e2
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2018
ms.locfileid: "7705120"
---
# <a name="delete-an-add-on"></a>刪除附加元件

在 Microsoft Store 提交 API 中使用這個方法，刪除附加元件 （也稱為應用程式內產品或 IAP） 針對已登錄到您的合作夥伴中心帳戶的 app。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}``` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| id | 字串 | 必要。 要刪除的附加元件 Store 識別碼。 「 市集識別碼 」 是可在合作夥伴中心。  |


### <a name="request-body"></a>要求主體

不提供此方法的要求主體。


### <a name="request-example"></a>要求範例

下列範例示範如何刪除附加元件。

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

如果成功，此方法會傳回空白回應主體。

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述                                                                                                                                                                           |
|--------|------------------|
| 400  | 要求無效。 |
| 404  | 找不到指定的附加元件。  |
| 409  | 找到指定的附加元件，但它無法以其目前的狀態刪除，或附加元件使用[「 Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。 |   


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得所有附加元件](get-all-add-ons.md)
* [取得附加元件](get-an-add-on.md)
* [建立附加元件](create-an-add-on.md)
