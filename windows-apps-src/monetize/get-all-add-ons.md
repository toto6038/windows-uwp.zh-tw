---
author: mcleanbyron
ms.assetid: 7B6A99C6-AC86-41A1-85D0-3EB39A7211B6
description: "在 Windows 市集提交 API 中使用這個方法，針對已登錄到您 Windows 開發人員中心帳戶的所有 App 擷取所有附加元件資料。"
title: "使用 Windows 市集提交 API 取得所有附加元件"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 64d40badaa4664e4f516900e82d290f60a499149

---

# 使用 Windows 市集提交 API 取得所有附加元件




在 Windows 市集提交 API 中使用這個方法，針對已登錄到您 Windows 開發人員中心帳戶的所有 App 擷取所有附加元件 (也稱為應用程式內產品或 IAP) 資料。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

>**注意**  這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts``` |

<span/>
 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

對於此方法而言，所有的要求參數都是選用的。 如果您呼叫這個不含參數的方法，回應會包含已登錄到您帳戶之所有 App 的所有附加元件的資料。
 
|  參數  |  類型  |  描述  |  必要  |
|------|------|------|------|
|  top  |  整數  |  要求中要傳回的項目數目 (也就是要傳回的附加元件數目)。 如果您的帳戶擁有的附加元件超過您在查詢中指定的值，回應本文會包含您可以附加到方法 URI 的相對 URI 路徑以要求下一個頁面的資料。  |  否  |
|  skip  |  整數  |  在傳回剩餘項目之前要略過的項目數目。 使用此參數來瀏覽資料集。 例如，top=10 且 skip=0 會擷取 1 到 10 的項目，top=10 且 skip=10 會擷取 11 到 20 的項目，依此類推。  |  否  |

<span/>

### 要求本文

不提供此方法的要求本文。

### 要求範例

下列範例示範如何擷取已登錄到您帳戶之所有 App 的所有附加元件資料。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts HTTP/1.1
Authorization: Bearer <your access token>
```

下列範例示範如何僅擷取前 10 個附加元件。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

下列範例說明成功要求前 5 個附加元件 (已登錄到含有總共 1072 個附加元件的開發人員帳戶) 所傳回的 JSON 回應本文。 為求簡潔，這個範例僅會顯示由要求所傳回之前 2 個附加元件的資料。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "@nextLink": "inappproducts/?skip=5&top=5",
  "value": [
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
      "productId": "a8b8310b-fa8d-4da0-aca0-577ef6dce4dd",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243619",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243619"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243705",
        "resourceLocation": "inappproducts/9NBLGGH4TNMP/submissions/1152921504621243705"
      }
    },
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
      "id": "9NBLGGH4TNMN",
      "productId": "6a3c9788-a350-448a-bd32-16160a13018a",
      "productType": "Consumable",
      "pendingInAppProductSubmission": {
        "id": "1152921504621243538",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243538"
      },
      "lastPublishedInAppProductSubmission": {
        "id": "1152921504621243106",
        "resourceLocation": "inappproducts/9NBLGGH4TNMN/submissions/1152921504621243106"
      }
    },

  // Other add-ons omitted for brevity...
  ],
  "totalCount": 1072
}
```

### 回應本文

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| @nextLink  | 字串 | 如果還有其他資料頁面，此字串包含您可以附加到基本 ```https://manage.devcenter.microsoft.com/v1.0/my/``` 要求 URI 的相對路徑以要求下一頁資料。 例如，如果初始要求內文的 *top* 參數設為 10，但是 App 有 100 個登錄到您帳戶的附加元件，回應本文會包含 ```inappproducts?skip=10&top=10``` 的 @nextLink 值，這指出您可以呼叫 ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts?skip=10&top=10``` 來要求接下來的 10 個附加元件。 |
| value            | 陣列  |  包含提供每個附加元件相關資訊之物件的陣列。 如需詳細資訊，請參閱[附加元件資源](manage-add-ons.md#add-on-object)。   |
| totalCount   | 整數  | 回應內文的 *value* 陣列中的 App 物件數目。                                                                                                                                                 |



## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到任何附加元件。 |
| 409  | App 或附加元件使用[目前 Windows 市集提交 API 不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。  |

<span/>

## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [取得附加元件](get-an-add-on.md)
* [建立附加元件](create-an-add-on.md)
* [刪除附加元件](delete-an-add-on.md)



<!--HONumber=Aug16_HO5-->


