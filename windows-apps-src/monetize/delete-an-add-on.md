---
author: mcleanbyron
ms.assetid: 16D4C3B9-FC9B-46ED-9F87-1517E1B549FA
description: 使用 Microsoft Store 提交 API 中的這個方法為註冊到您 Windows 開發人員中心帳戶的 App 刪除附加元件。
title: 刪除附加元件
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store 提交 API, 附加元件, 刪除, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 95743f600b2f688a09f3fe96bd7e58acc93da5d2
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2018
ms.locfileid: "1661988"
---
# <a name="delete-an-add-on"></a>刪除附加元件

使用 Microsoft Store 提交 API 中的這個方法為註冊到您 Windows 開發人員中心帳戶的 App 刪除附加元件 (也稱為應用程式內產品或 IAP)。

## <a name="prerequisites"></a>先決條件

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
| id | 字串 | 必要。 要刪除的附加元件 Store 識別碼。  Store 識別碼可在開發人員中心儀表板上取得。  |


### <a name="request-body"></a>要求本文

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
| 409  | 找到指定的附加元件，但無法以其目前的狀態刪除，或附加元件使用 [Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得所有附加元件](get-all-add-ons.md)
* [取得附加元件](get-an-add-on.md)
* [建立附加元件](create-an-add-on.md)
