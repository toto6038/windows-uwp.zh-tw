---
author: mcleanbyron
ms.assetid: 8C63D33B-557D-436E-9DDA-11F7A5BFA2D7
description: "使用 Windows 市集提交 API 中的這個方法，更新現有的附加元件提交。"
title: "使用 Windows 市集提交 API 更新附加元件提交"
translationtype: Human Translation
ms.sourcegitcommit: f52059a37194b78db2f9bb29a5e8959b2df435b4
ms.openlocfilehash: ac126d8e8cf8301399a3248a1d65e19805e70255

---

# <a name="update-an-add-on-submission-using-the-windows-store-submission-api"></a>使用 Windows 市集提交 API 更新附加元件提交


使用 Windows 市集提交 API 中的這個方法，更新現有的附加元件 (也稱為應用程式內產品或 IAP) 提交。 使用這個方法成功更新提交之後，您必須針對擷取和發佈[認可提交](commit-an-add-on-submission.md)。

如需這個方法如何在使用 Windows 市集提交 API 建立附加元件提交的程序中進行的詳細資訊，請參閱[管理附加元件提交](manage-add-on-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 針對您開發人員中心帳戶中的 App 建立附加元件提交。 您可以在開發人員中心儀表板中進行，或者可以使用[建立附加元件提交](create-an-add-on-submission.md)方法進行。

>**注意**  這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |

<span/>
 

### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字串 | 必要。 您想要更新提交之附加元件的市集識別碼。 市集識別碼可在開發人員中心儀表板上取得，並且包含在[建立附加元件](create-an-add-on.md)或[取得附加元件詳細資料](get-all-add-ons.md)要求的回應資料中。  |
| submissionId | 字串 | 必要。 要更新之提交的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立附加元件提交](create-an-add-on-submission.md)要求的回應資料中。  |

<span/>

### <a name="request-body"></a>要求主體

要求主體包含下列參數。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contentType           | 字串  |  附加元件中提供的[內容類型](../publish/enter-add-on-properties.md#content-type)。 這可以是下列其中一個值： <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | 這個字串陣列可針對附加元件包含最多 10 個[關鍵字](../publish/enter-add-on-properties.md#keywords)。 您的 App 可以使用這些關鍵字查詢附加元件。   |
| lifetime           | 字串  |  附加元件的存留期。 這可以是下列其中一個值： <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | 物件  | 此物件包含附加元件的清單資訊。 如需詳細資訊，請參閱[清單資源](manage-add-on-submissions.md#listing-object)。  |
| pricing           | 物件  | 此物件包含附加元件的定價資訊。 如需詳細資訊，請參閱[定價資源](manage-add-on-submissions.md#pricing-object)。  |
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |
| tag           | 字串  |  附加元件的[自訂開發人員資料](../publish/enter-add-on-properties.md#custom-developer-data) (此資訊以前稱為「標記」)。   |
| visibility  | 字串  |  附加元件的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |

<span/>

### <a name="request-example"></a>要求範例

下列範例示範如何更新附加元件提交。

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
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
    "sales": [],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
}
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應主體。 回應主體包含更新提交的相關資訊。 如需回應主體中各個值的詳細資訊，請參閱[附加元件提交資源](manage-add-on-submissions.md#add-on-submission-object)。

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
    "sales": [],
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

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 無法更新提交，因為要求無效。 |
| 409  | 無法更新提交，因為附加元件的目前狀態，或附加元件使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   

<span/>


## <a name="related-topics"></a>相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [取得附加元件提交](get-an-add-on-submission.md)
* [建立附加元件提交](create-an-add-on-submission.md)
* [認可附加元件提交](commit-an-add-on-submission.md)
* [刪除附加元件提交](delete-an-add-on-submission.md)
* [取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)



<!--HONumber=Dec16_HO1-->


