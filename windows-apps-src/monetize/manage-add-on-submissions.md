---
author: mcleanbyron
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理附加元件提交。"
title: "使用 Windows 市集提交 API 管理附加元件提交"
translationtype: Human Translation
ms.sourcegitcommit: 4a1ea50d72e0f754658d8ee99755b873619e1969
ms.openlocfilehash: 9d19ecae9d5c43c28e887627372aabb58bf0aab2

---

# 使用 Windows 市集提交 API 管理附加元件提交



在 Windows 市集提交 API 中使用下列方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理附加元件 (亦稱為 App 內產品或 IAP) 提交。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的先決條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>
            **注意**
            &nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 附加元件必須已經存在於您的開發人員中心帳戶，您才能使用這些方法來建立或管理附加元件的提交。 您可以[使用開發人員中心儀表板](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)或使用[管理附加元件](manage-add-ons.md)中所述的「Windows 市集提交 API」方法來建立附加元件。

>
            **重要**
            &nbsp;&nbsp;在不久的將來，Microsoft 將會變更「Windows 開發人員中心」中附加元件提交的定價資料模型。 實作這項變更之後，將不再支援「定價」資源，而您將暫時無法使用「Windows 市集提交 API」來取得或修改附加元件提交的定價和銷售資料。 將來，我們會更新此 API 來導入新的方法，以程式設計方式存取附加元件提交的定價資訊。 如需詳細資訊，請參閱[定價資源](#pricing-object)一節。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | 取得現有附加元件提交的資料。 如需詳細資訊，請參閱[取得附加元件提交](get-an-add-on-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status``` | 取得現有附加元件提交的狀態。 如需詳細資訊，請參閱[取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions``` | 為登錄到您 Windows 開發人員中心帳戶的 App 建立新的附加元件提交。 如需詳細資訊，請參閱[建立附加元件提交](create-an-add-on-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit``` | 將新的或更新的附加元件提交認可到 Windows 開發人員中心。 如需詳細資訊，請參閱[認可附加元件提交](commit-an-add-on-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | 更新現有的附加元件提交。 如需詳細資訊，請參閱[更新附加元件提交](update-an-add-on-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}``` | 刪除附加元件提交。 如需詳細資訊，請參閱[刪除附加元件提交](delete-an-add-on-submission.md)。 |

<span id="create-an-add-on-submission">
## 建立附加元件提交

若要建立附加元件的提交，請遵循此程序。

1. 如果您尚未執行此動作，請先完成[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的先決條件，包括將 Azure AD 應用程式關聯至您的 Windows 開發人員中心帳戶，並取得您的用戶端識別碼和金鑰。 您只需執行此動作一次；有了用戶端識別碼和金鑰之後，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。  

2. 
            [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Windows 市集提交 API 中的方法。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

3. 在 Windows 市集提交 API 中執行下列方法。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。 如需詳細資訊，請參閱[建立附加元件提交](create-an-add-on-submission.md)。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
  ```

  回應主體包含三個項目︰新提交的識別碼、新提交的資料 (包含所有清單和定價資訊)，以及用於上傳提交的任何附加元件圖示的共用存取簽章 (SAS) URI。 如需 SAS 的詳細資訊，請參閱[共用存取簽章，第 1 部分︰了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果您要新增提交的新圖示，請[準備圖示](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)並將它們新增到 ZIP 封存。

5. 藉由對新的提交進行任何必要變更來更新提交資料，然後執行下列方法來更新提交。 如需詳細資訊，請參閱[更新附加元件提交](update-an-add-on-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
  ```

  >
            **注意**
            &nbsp;&nbsp;如果您要新增提交的新圖示，確定您會更新提交資料，以參考 ZIP 封存中的名稱和這些檔案的相對路徑。

4. 如果您要新增提交的新圖示，將 ZIP 封存上傳至您在步驟 2 中呼叫之 POST 方法回應主體中提供的 SAS URI。 如需詳細資訊，請參閱[共用存取簽章，第 2 部分︰透過 Blob 儲存體來建立與使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  下列程式碼片段示範如何在適用於 .NET 的 Azure 儲存體用戶端程式庫中使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 類別來上傳封存

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 執行下列方法來認可提交。 這將警示開發人員中心，您已經完成提交，而現在應該將更新套用至您的帳戶。 如需詳細資訊，請參閱[認可附加元件提交](commit-an-add-on-submission.md)。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
  ```

6. 執行下列方法來檢查認可狀態。 如需詳細資訊，請參閱[取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
  ```

  若要確認提交狀態，請檢閱回應主體中的「狀態」值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到市集以供擷取。 您可以繼續使用先前的方法，或瀏覽開發人員中心儀表板來監視提交進度。

## 資源

這些方法會使用下列資源來格式化資料。

<span id="add-on-submission-object" />
### 附加元件提交

此資源代表附加元件的提交。 下列範例示範此資源的格式。

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

此資源具有下列值。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字串  | 提交的識別碼。  |
| contentType           | 字串  |  附加元件中提供的[內容類型](../publish/enter-add-on-properties.md#content-type)。 這可以是下列其中一個值： <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | 這個字串陣列可針對附加元件包含最多 10 個[關鍵字](../publish/enter-add-on-properties.md#keywords)。 您的 App 可以使用這些關鍵字查詢附加元件。   |
| lifetime           | 字串  |  附加元件的存留期。 這可以是下列其中一個值： <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | 物件  |  機碼和值組的字典，其中每個機碼都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[清單資源](#listing-object)物件，其中包含附加元件的清單資訊。  |
| pricing           | 物件  | 此物件包含附加元件的定價資訊。 如需詳細資訊，請參閱下方的[定價資源](#pricing-object)一節。  |
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |
| tag           | 字串  |  附加元件的[自訂開發人員資料](../publish/enter-add-on-properties.md#custom-developer-data) (此資訊以前稱為「標記」)。   |
| visibility  | 字串  |  附加元件的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | 字串  |  提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件  |  包含其他有關提交狀態的詳細資料 (包括任何錯誤的資訊)。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-details-object)一節。 |
| fileUploadUrl           | 字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件，請將包含套件的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立附加元件提交](#create-an-add-on-submission)。  |
| friendlyName  | 字串  |  基於顯示用途而使用的附加元件易記名稱。  |

<span id="listing-object" />
### 清單

此資源包含附加元件的清單資訊。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  description               |    字串     |   附加元件清單的描述。   |     
|  icon               |   物件      |  包含附加元件清單圖示的資料。 如需詳細資訊，請參閱下方的[圖示](#icon-object)一節。   |
|  title               |     字串    |   附加元件清單的標題。   |  

<span id="icon-object" />
### 圖示

此資源包含附加元件清單的圖示資料。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  fileName               |    字串     |   圖示檔案的名稱，位於您針對提交所上傳的 ZIP封存中。    |     
|  fileStatus               |   字串      |  圖示檔案的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />
### 定價

此資源包含附加元件的定價資訊。

>
            **重要**
            &nbsp;&nbsp;在不久的將來，Microsoft 將會變更「Windows 開發人員中心」中附加元件提交的定價資料模型。 實作這項變更之後，將不再支援「定價」資源，而您將暫時無法使用「Windows 市集提交 API」來取得或修改附加元件提交的定價和銷售資料。 您將會發現下列行為變更：

   > * 在呼叫 [GET 方法以取得附加元件提交](get-an-add-on-submission.md)之後，「定價」資源將會空白。 您可以繼續使用「開發人員中心」儀表板來取得附加元件提交的定價資料。
   > * 呼叫 [PUT 方法以更新附加元件提交](update-an-add-on-submission.md)時，會忽略「定價」資源中的資訊。 您可以繼續使用「開發人員中心」儀表板來變更附加元件提交的定價資料。

> 將來，我們會更新「Windows 市集提交 API」來導入新的方法，以程式設計方式取得及更新附加元件提交的定價資訊。

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  marketSpecificPricings               |    物件     |  機碼和值組的字典，其中每個機碼都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的附加元件在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices)。 這個字典中的任何項目都會覆寫特定市場的 *priceId* 值所指定的基本價格。     |     
|  sales               |   陣列      |  包含附加元件銷售資訊的物件陣列。 如需詳細資訊，請參閱下方的[銷售](#sale-object)一節。    |     
|  priceId               |   字串      |  指定附加元件[基本價格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price)的[價格區間](#price-tiers)。    |


<span id="sale-object" />
### 銷售

此資源包含附加元件的銷售資訊。

>
            **重要**
            &nbsp;&nbsp;在不久的將來，Microsoft 將會變更「Windows 開發人員中心」中附加元件提交的定價資料模型。 實作這項變更之後，將不再支援「銷售」資源，而您將暫時無法使用「Windows 市集提交 API」來取得或修改附加元件提交的銷售資料。 將來，我們會更新此 API 來導入新的方法，以程式設計方式存取附加元件提交的銷售資訊。 如需詳細資訊，請參閱[定價資源](#pricing-object)一節。

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    字串     |   銷售的名稱。    |     
|  basePriceId               |   字串      |  用於銷售基本價格的[價格區間](#price-tiers)。    |     
|  startDate               |   字串      |   ISO 8601 格式的銷售開始日期。  |     
|  endDate               |   字串      |  ISO 8601 格式的銷售結束日期。      |     
|  marketSpecificPricings               |   物件      |   機碼和值組的字典，其中每個機碼都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的附加元件在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess)。 這個字典中的任何項目都會覆寫特定市場的 *basePriceId* 值所指定的基本價格。    |



<span id="status-details-object" />
### 狀態詳細資料

此資源包含關於提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    物件     |   包含提交錯誤詳細資料的物件陣列。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-detail-object)一節。   |     
|  warnings               |   物件      | 包含提交警告詳細資料的物件陣列。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-detail-object)一節。     |
|  certificationReports               |     物件    |   提供提交認證報告資料存取的物件陣列。 如果認證失敗，您可以檢查這些報告來取得詳細資訊。 如需詳細資訊，請參閱下方的[認證報告](#certification-report-object)一節。   |  


<span id="status-detail-object" />
### 狀態詳細資料

此資源包含有關提交的任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的字串。 如需詳細資訊，請參閱下方的[提交狀態碼](#submission-status-code)一節。   |     
|  details               |     字串    |  含有更多關於問題之詳細資料的訊息。     |


<span id="certification-report-object" />
### 認證報告

此資源提供提交認證報告資料的存取。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     日期            |    字串     |  以 ISO 8601 格式產生報告的日期和時間。    |
|     reportUrl            |    字串     |  您可以存取報告的 URL。    |



## 列舉

這些方法會使用下列列舉。


<span id="price-tiers" />
### 價格區間

下列值代表附加元件提交可用的價格區間。

| 值           | 描述                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   未設定價格區間；使用附加元件的基本價格。      |     
|  NotAvailable              |   特定區域中無法使用此附加元件。    |     
|  Free              |   附加元件是免費的。    |    
|  從 Tier2 到 Tier194               |   Tier2 代表 .99 美元的價格區間。 每一個額外的層級代表額外的遞增 (1.29 美元、1.49 美元、1.99 美元等)。    |


<span id="submission-status-code" />
### 提交狀態碼

下列值代表提交的狀態碼。

| 值           |  描述      |
|-----------------|---------------|
|  None            |     未指定任何代碼。         |     
|      InvalidArchive        |     包含該套件的 ZIP 封存無效，或具有無法辨識的封存格式。  |
| MissingFiles | ZIP 封存沒有您提交資料中列出的所有檔案，或者它們位於封存中的錯誤位置。 |
| PackageValidationFailed | 提交中有一或多個套件無法驗證。 |
| InvalidParameterValue | 要求主體中有一個參數不正確。 |
| InvalidOperation | 您嘗試執行的操作無效。 |
| InvalidState | 您嘗試針對套件正式發行前小眾測試版的目前狀態執行的操作不正確。 |
| ResourceNotFound | 找不到指定的套件正式發行前小眾測試版。 |
| ServiceError | 內部服務錯誤已防止要求成功。 請重試要求。 |
| ListingOptOutWarning | 開發人員已從先前提交中移除清單，或者未包含該套件支援的清單資訊。 |
| ListingOptInWarning  | 開發人員已新增清單。 |
| UpdateOnlyWarning | 開發人員正嘗試插入某些只有更新支援的項目。 |
| Other  | 提交處於無法辨識或未分類的狀態。 |
| PackageValidationWarning | 套件驗證程序產生了警告。 |

<span/>

## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 市集提交 API 管理附加元件](manage-add-ons.md)
* [開發人員中心儀表板中的附加元件提交](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)



<!--HONumber=Nov16_HO1-->


