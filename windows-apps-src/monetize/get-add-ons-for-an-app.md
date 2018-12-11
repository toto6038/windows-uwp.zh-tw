---
ms.assetid: E59FB6FE-5318-46DF-B050-73F599C3972A
description: 在 Microsoft Store 提交 API 中使用這個方法，來擷取已登錄到您的合作夥伴中心的應用程式的應用程式內購買相關資訊。
title: 取得 App 的附加元件
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 附加元件, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: ec065eef5d411e35515837b169fd57d71e4ef6ac
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922745"
---
# <a name="get-add-ons-for-an-app"></a>取得 App 的附加元件

在 Microsoft Store 提交 API 中使用這個方法，來列出附加元件，針對已登錄到您的合作夥伴中心帳戶的 app。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listinappproducts``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數


|  名稱  |  類型  |  描述  |  必要  |
|------|------|------|------|
|  applicationId  |  字串  |  您想要擷取其附加元件之 App 的「Store 識別碼」。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |  是  |
|  top  |  整數  |  要求中要傳回的項目數目 (也就是要傳回的附加元件數目)。 如果 App 擁有的附加元件超過您在查詢中指定的值，回應主體會包含您可以附加到方法 URI 的相對 URI 路徑以要求下一個頁面的資料。  |  否  |
|  skip |  整數  | 在傳回剩餘項目之前要略過的項目數目。 使用此參數來瀏覽資料集。 例如，top=10 且 skip=0 會擷取 1 到 10 的項目，top=10 且 skip=10 會擷取 11 到 20 的項目，依此類推。   |  否  |


### <a name="request-body"></a>要求主體

不提供此方法的要求主體。

### <a name="request-examples"></a>要求範例

下列範例示範如何列出 App 的所有附加元件。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

下列範例示範如何列出 App 的前 10 個附加元件。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listinappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範針對含有總共 53 個附加元件的 App 成功要求前 10 個附加元件所傳回的 JSON 回應主體。 為求簡潔，這個範例僅會顯示由要求所傳回之前 3 個附加元件的資料。 如需回應主體中各個值的詳細資訊，請參閱下列各節。

```json
{
  "@nextLink": "applications/9NBLGGH4R315/listinappproducts/?skip=10&top=10",
  "value": [
    {
      "inAppProductId": "9NBLGGH4TNMP"
    },
    {
      "inAppProductId": "9NBLGGH4TNMN"
    },
    {
      "inAppProductId": "9NBLGGH4TNNR"
    },
    // Next 7 add-ons  are omitted for brevity ...
  ],
  "totalCount": 53
}
```

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含您可以附加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 的相對路徑以要求下一頁資料。 例如，如果初始要求主體的 *top* 參數設為 10，但是 App 有 50 個附加元件，回應主體會包含 ```applications/{applicationid}/listinappproducts/?skip=10&top=10``` 的 @nextLink 值，這指出您可以呼叫 ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listinappproducts/?skip=10&top=10``` 來要求接下來的 10 個附加元件。 |
| value      | 陣列  | 列出指定 App 每個附加元件之 Store 識別碼的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱[附加元件資源](get-app-data.md#add-on-object)。                                                                                                                           |
| totalCount | 整數    | 查詢的資料結果中的列數總計 (也就是指定 App 的附加元件總數目)。    |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到任何附加元件。 |
| 409  | 附加元件使用[「 Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。  |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得所有應用程式](get-all-apps.md)
* [取得 App](get-an-app.md)
* [取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)
