---
ms.assetid: 8C1E9E36-13AF-4386-9D0F-F9CB320F02F5
description: 在 Microsoft Store 提交 API 中使用這個方法，來建立封裝航班已向您的合作夥伴中心帳戶的應用程式。
title: 建立套件正式發行前小眾測試版
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 建立正式發行前小眾測試版
ms.localizationpriority: medium
ms.openlocfilehash: af5ffe0dd72f0c3aae21a2dc522b469358626bab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603453"
---
# <a name="create-a-package-flight"></a>建立套件正式發行前小眾測試版

在 Microsoft Store 提交 API 中使用這個方法，來建立封裝航班已向您的合作夥伴中心帳戶的應用程式。

> [!NOTE]
> 這個方法會建立一個套件正式發行前小眾測試版但不含任何提交。 若要為套件正式發行前小眾測試版建立提交，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)中的方法。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 您想要建立套件正式發行前小眾測試版之 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視應用程式身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |


### <a name="request-body"></a>要求本文

要求本文包含下列參數。

|  參數  |  類型  |  描述  |  必要  |
|------|------|------|------|
|  friendlyName  |  字串  |  開發人員指定的套件正式發行前小眾測試版名稱。  |  否  |
|  groupIds  |  陣列  |  此字串陣列包含與套件正式發行前小眾測試版相關聯的正式發行前小眾測試版群組的識別碼。 如需有關正式發行前小眾測試版群組的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |  否  |
|  rankHigherThan  |  字串  |  排名位於目前套件正式發行前小眾測試版之下的套件正式發行前小眾測試版易記名稱。 如果您未設定此參數，新的套件正式發行前小眾測試版在所有套件正式發行前小眾測試版當中會排在最高排名。 如需有關正式發行前小眾測試版群組排名的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。    |  否  |


### <a name="request-example"></a>要求範例

下列範例示範如何為市集識別碼為 9WZDNCRD911W 的 App 建立新的套件正式發行前小眾測試版。

```syntax
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1Q...
Content-Type: application/json
{
  "friendlyName": "myflight",
  "groupIds": [
    0
  ],
  "rankHigherThan": null
}

```

## <a name="response"></a>回應

下列範例示範成功呼叫此方法時的 JSON 回應主體。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "flightId": "43e448df-97c9-4a43-a0bc-2a445e736bcd",
  "friendlyName": "myflight",
  "groupIds": [
    "0"
  ],
  "rankHigherThan": "671c2857-725e-4faf-9e9e-ea1191ef879c"
}
```

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| flightId            | 字串  | 套件正式發行前小眾測試版的識別碼。 這個值是由合作夥伴中心提供。  |
| friendlyName           | 字串  | 要求中指定的套件正式發行前小眾測試版名稱。   |  
| groupIds           | 陣列  | 此字串陣列包含要求中指定，與套件正式發行前小眾測試版相關聯的正式發行前小眾測試版群組的識別碼。 如需有關正式發行前小眾測試版群組的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。   |
| rankHigherThan           | 字串  | 要求中指定，排名位於目前套件正式發行前小眾測試版之下的套件正式發行前小眾測試版易記名稱。 如需有關正式發行前小眾測試版群組排名的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。  |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 要求無效。 |
| 409  | 無法建立封裝飛行，因為其目前的狀態，或應用程式使用的合作夥伴中心功能[目前不支援 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相關主題

* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
* [取得封裝的航班](get-a-flight.md)
* [刪除套件班機](delete-a-flight.md)
