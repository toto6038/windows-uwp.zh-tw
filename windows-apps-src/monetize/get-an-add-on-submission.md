---
author: mcleanbyron
ms.assetid: E3DF5D11-8791-4CFC-8131-4F59B928A228
description: "在 Windows 市集提交 API 中使用這個方法，取得現有附加元件提交的資料。"
title: "使用 Windows 市集提交 API 取得附加元件提交"
translationtype: Human Translation
ms.sourcegitcommit: 03942eb9015487cfd5690e4b1933e4febd705971
ms.openlocfilehash: ecdd4292c7980a647075c55abf7d14edd39d23d6

---

# 使用 Windows 市集提交 API 取得附加元件提交




在 Windows 市集提交 API 中使用這個方法，取得現有附加元件 (也稱為應用程式內產品或 IAP) 提交的資料。 如需有關使用「Windows 市集提交 API」來建立附加元件提交的程序詳細資訊，請參閱[管理附加元件提交](manage-add-on-submissions.md)。

>
            **重要**
            &nbsp;&nbsp;在不久的將來，Microsoft 將會變更「Windows 開發人員中心」中附加元件提交的定價資料模型。 實作這項變更之後，此方法之回應資料中的「定價」資源將會空白，而您將暫時無法使用此方法來取得附加元件提交的定價和銷售資料。 將來，我們會更新「Windows 市集提交 API」來導入新的方法，以程式設計方式存取附加元件提交的定價資訊。 如需詳細資訊，請參閱[定價資源](manage-add-on-submissions.md#pricing-object)。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* 
            [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 針對您開發人員中心帳戶中的 App 建立附加元件提交。 您可以在開發人員中心儀表板中進行，或者可以使用[建立附加元件提交](create-an-add-on-submission.md)方法進行。

>
            **注意**
            &nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |

<span/>
 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer**&lt;*token*&gt;。 |

<span/>

### 要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字串 | 必要。 包含您想要取得提交之附加元件的市集識別碼。 市集識別碼可在開發人員中心儀表板上取得，並且包含在[建立附加元件](create-an-add-on.md)或[取得附加元件詳細資料](get-all-add-ons.md)要求的回應資料中。  |
| submissionId | 字串 | 必要。 要取得之提交的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立附加元件提交](create-an-add-on-submission.md)要求的回應資料中。  |

<span/>

### 要求主體

不提供此方法的要求主體。

### 要求範例

下列範例示範如何取得附加元件提交。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621243680 HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

下列範例示範成功呼叫這個方法的 JSON 回應主體。 回應主體包含指定提交的相關資訊。 如需回應主體中各個值的詳細資訊，請參閱[附加元件提交資源](manage-add-on-submissions.md#add-on-submission-object)。

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },

    "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
    "friendlyName": "Submission 2"
}
```

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 提交不屬於指定的附加元件，或附加元件使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   

<span/>


## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [建立附加元件提交](create-an-add-on-submission.md)
* [認可附加元件提交](commit-an-add-on-submission.md)
* [更新附加元件提交](update-an-add-on-submission.md)
* [刪除附加元件提交](delete-an-add-on-submission.md)
* [取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)



<!--HONumber=Nov16_HO1-->


