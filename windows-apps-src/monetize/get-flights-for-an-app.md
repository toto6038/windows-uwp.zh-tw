---
author: mcleanbyron
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: "在 Windows 市集提交 API 中使用這個方法，針對已登錄到您 Windows 開發人員中心帳戶的 App 擷取套件正式發行前小眾測試版資訊。"
title: "使用 Windows 市集提交 API 取得 App 的套件正式發行前小眾測試版"
translationtype: Human Translation
ms.sourcegitcommit: ef90390fcf7d4aa2e040eae65119ac7959f3423f
ms.openlocfilehash: eddac4b37f6f00bad33f543f0e55415a5dcea887

---

# 使用 Windows 市集提交 API 取得 App 的套件正式發行前小眾測試版




在 Windows 市集提交 API 中使用這個方法，針對已登錄到您 Windows 開發人員中心帳戶的 App 列出套件正式發行前小眾測試版。 如需有關套件正式發行前小眾測試版的詳細資訊，請參閱[套件正式發行前小眾測試版](https://msdn.microsoft.com/windows/uwp/publish/package-flights)。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

>**注意**&nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights``` |

<span/>
 
### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

|  名稱  |  類型  |  描述  |  必要  |
|------|------|------|------|
|  applicationId  |  字串  |  您想要擷取其套件正式發行前小眾測試版之 App 的「市集識別碼」。 如需有關市集識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |  是  |
|  top  |  整數  |  要在要求中傳回的項目數目 (也就是要傳回的套件正式發行前小眾測試版數目)。 如果您帳戶擁有的套件正式發行前小眾測試版數目超出您在查詢中指定的值，回應主體中就會包含一個相對 URI 路徑，您可以將此路徑附加到方法 URI 來要求下一頁資料。  |  否  |
|  skip  |  整數  |  在傳回剩餘項目之前要略過的項目數目。 使用此參數來瀏覽資料集。 例如，top=10 且 skip=0 會擷取 1 到 10 的項目，top=10 且 skip=10 會擷取 11 到 20 的項目，依此類推。  |  否  |

<span/>

### 要求主體

不提供此方法的要求主體。

### 要求範例

下列範例示範如何列出 App 的所有套件正式發行前小眾測試版。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

下列範例示範如何列出 App 的第一個套件正式發行前小眾測試版。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

下列範例說明針對含有總共 3 個套件正式發行前小眾測試版的 App 成功要求第一個套件正式發行前小眾測試版所傳回的 JSON 回應主體。 如需回應主體中各個值的詳細資訊，請參閱下列各節。

```json
{
  "value": [
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
  ],
  "totalCount": 3
}
```

### 回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含您可以附加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 的相對路徑以要求下一頁資料。 例如，如果初始要求內文的 *top* 參數設為 2，但是 App 有 4 個套件正式發行前小眾測試版，回應主體會包含 ```applications/{applicationid}/listflights/?skip=2&top=2``` 的 @nextLink 值，這指出您可以呼叫 ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2``` 來要求接下來的 2 個套件正式發行前小眾測試版。 |
| value      | 陣列  | 提供指定 App 之套件正式發行前小眾測試版相關資訊的物件陣列。 如需有關每個物件中資料的詳細資訊，請參閱[正式發行前小眾測試版資源](get-app-data.md#flight-object)。                                                                                                                           |
| totalCount | 整數    | 查詢的資料結果中的列數總計 (也就是指定 App 的套件正式發行前小眾測試版總數目)。                                                                                                                                                                                                                             |

<span/>

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到任何套件正式發行前小眾測試版。 |
| 409  | App 使用[目前 Windows 市集提交 API 不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。  |

<span/>

## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得所有 App](get-all-apps.md)
* [取得 App](get-an-app.md)
* [取得 App 的附加元件](get-add-ons-for-an-app.md)



<!--HONumber=Nov16_HO1-->


