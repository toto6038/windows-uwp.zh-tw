---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: 在 Microsoft Store 提交 API 中使用這個方法，來為登錄到您的合作夥伴中心帳戶的所有 app 都擷取相關資訊。
title: 取得所有 App
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 5c909e707d25e4add534ce89319abe71c2557b59
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8348269"
---
# <a name="get-all-apps"></a>取得所有 App


在 Microsoft Store 提交 API 中使用這個方法，擷取已登錄到您的合作夥伴中心帳戶的所有應用程式資料。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

對於此方法而言，所有的要求參數都是選用的。 如果您呼叫這個不含參數的方法，回應會包含已登錄到您帳戶之所有 App 的資料。

|  參數  |  類型  |  描述  |  必要  |
|------|------|------|------|
|  top  |  整數  |  要求中要傳回的項目數目 (也就是要傳回的 App 數目)。 如果您的帳戶擁有的 App 超過您在查詢中指定的值，回應本文會包含您可以附加到方法 URI 的相對 URI 路徑以要求下一個頁面的資料。  |  否  |
|  skip  |  整數  |  在傳回剩餘項目之前要略過的項目數目。 使用此參數來瀏覽資料集。 例如，top=10 且 skip=0 會擷取 1 到 10 的項目，top=10 且 skip=10 會擷取 11 到 20 的項目，依此類推。  |  否  |


### <a name="request-body"></a>要求主體

不提供此方法的要求主體。

### <a name="request-examples"></a>要求範例

下列範例示範如何擷取已登錄到您帳戶之所有 App 的相關資訊。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

下列範例示範如何擷取已登錄到您帳戶的前 10 個 App。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例說明成功要求前 10 個 App (已登錄到含有總共 21 個 App 的開發人員帳戶) 所傳回的 JSON 回應本文。 為求簡潔，這個範例僅會顯示由要求所傳回之前 2 個 App 的資料。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | 陣列  | 包含已登錄到您帳戶之每個 App 相關資訊的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱[應用程式資源](get-app-data.md#application_object)。                                                                                                                           |
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含您可以附加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 的相對路徑以要求下一頁資料。 例如，如果初始要求主體的 *top* 參數設為 10，但是 App 有 20 個登錄到您帳戶的 App，回應本文會包含 ```applications?skip=10&top=10``` 的 @nextLink 值，這指出您可以呼叫 ```https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10``` 來要求接下來的 10 個 App。 |
| totalCount | 整數    | 查詢的資料結果中的列數總計 (也就是已登錄到您帳戶的 App 總數目)。                                                |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到任何 App。 |
| 409  | 應用程式使用合作夥伴中心功能， [Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)。  |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得 App](get-an-app.md)
* [取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)
* [取得 App 的附加元件](get-add-ons-for-an-app.md)
