---
author: Xansky
ms.assetid: 66400066-24BF-4AF2-B52A-577F5C3CA474
description: 在 Microsoft Store 提交 API 中使用這些方法，來管理附加元件提交，針對已登錄到您的合作夥伴中心帳戶的應用程式。
title: 管理附加元件提交
ms.author: mhopkins
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 附加元件提交, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0e07b588415094281683ff762c02ed5242654
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2018
ms.locfileid: "7427244"
---
# <a name="manage-add-on-submissions"></a>管理附加元件提交

Microsoft Store 提交 API 提供方法讓您使用於管理附加元件 (也稱為應用程式內產品或 IAP) 提交的應用程式。 如需 Microsoft Store 提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果您使用 Microsoft Store 提交 API 建立附加元件提交，請務必透過進一步變更提交僅使用 API，而不是在合作夥伴中心中進行變更。 如果您使用合作夥伴中心來變更最初使用 API 所建立的提交，您將不再能夠變更或是認可該提交使用 API。 有時候提交可能會處於錯誤狀態，而無法繼續提交過程。 若發生這種情形，您必須刪除提交並建立新的提交。

<span id="methods-for-add-on-submissions" />

## <a name="methods-for-managing-add-on-submissions"></a>管理附加元件提交的方法

使用下列方法取得、建立、更新、認可或刪除附加元件提交。 您可以使用這些方法之前，附加元件必須已經存在於您的合作夥伴中心帳戶。 藉由[定義其產品類型和產品識別碼](../publish/set-your-add-on-product-id.md)，或使用 Microsoft Store 提交 API 方法中所述的[管理附加元件](manage-add-ons.md)中，您可以在合作夥伴中心建立附加元件。

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">方法</th>
<th align="left">URI</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-add-on-submission.md">取得現有的附加元件提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-add-on-submission.md">取得現有附加元件提交的狀態</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions</td>
<td align="left"><a href="create-an-add-on-submission.md">建立新的附加元件提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-add-on-submission.md">更新現有的附加元件提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-add-on-submission.md">認可全新或更新的附加元件提交</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-add-on-submission.md">刪除附加元件提交</a></td>
</tr>
</tbody>
</table>

<span id="create-an-add-on-submission">

## <a name="create-an-add-on-submission"></a>建立附加元件提交

若要建立附加元件的提交，請遵循此程序。

1. 如果您尚未執行此動作，完成必要條件中所述[建立和管理提交使用 Microsoft Store 服務](create-and-manage-submissions-using-windows-store-services.md)，包括 Azure AD 應用程式關聯至您的合作夥伴中心帳戶，並取得您的用戶端識別碼和金鑰。 您只需執行此動作一次；有了用戶端識別碼和金鑰之後，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。  

2. [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Microsoft Store 提交 API 中的方法。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

3. 在 Microsoft Store 提交 API 中執行下列方法。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。 如需詳細資訊，請參閱[建立附加元件提交](create-an-add-on-submission.md)。

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions
    ```

    回應主體包含[附加元件提交](#add-on-submission-object)資源，其中包括新提交的識別碼、用於上傳提交至 Azure Blob 儲存體的任何附加元件圖示的共用存取簽章 (SAS) URI，以及新提交的所有資料 (例如清單和定價資訊)。

    > [!NOTE]
    > SAS URI 提供 Azure 儲存體中安全資源的存取權，完全不需要帳戶金鑰。 如需有關 SAS URI 及使用 Azure Blob 儲存體的背景資訊，請參閱[共用存取簽章，第 1 部分︰了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1)和[共用存取簽章，第 2 部分︰透過 Blob 儲存體來建立與使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果您要新增提交的新圖示，請[準備圖示](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)並將它們新增到 ZIP 封存。

5. 藉由對新的提交進行任何必要變更來更新[附加元件提交](#add-on-submission-object)資料，然後執行下列方法來更新提交。 如需詳細資訊，請參閱[更新附加元件提交](update-an-add-on-submission.md)。

    ```
    PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果您要新增提交的新圖示，確定您會更新提交資料，以參考 ZIP 封存中的名稱和這些檔案的相對路徑。

4. 如果您要新增提交的新圖示，請使用您稍早呼叫之 POST 方法回應主體中提供的 SAS URI，將 ZIP 封存上傳至 [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)。 您可在各種不同的平台上使用不同的 Azure Libraries 來執行，包括：

    * [.NET 適用的 Azure 儲存體用戶端程式庫](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Java 適用的 Azure 儲存體 SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Python 適用的 Azure 儲存體 SDK](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    下列 C# 程式碼範例示範如何在適用於 .NET 的 Azure 儲存體用戶端程式庫中，使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 類別上傳 ZIP 封存。 此範例假設已將 ZIP 封存寫入串流物件。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 執行下列方法來認可提交。 這會警示合作夥伴中心，您已完成您的提交，現在應該會將更新套用到您的帳戶。 如需詳細資訊，請參閱[認可附加元件提交](commit-an-add-on-submission.md)。

    ```
    POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/commit
    ```

6. 執行下列方法來檢查認可狀態。 如需詳細資訊，請參閱[取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)。

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{id}/submissions/{submissionId}/status
    ```

    若要確認提交狀態，請檢閱回應主體中的 *「狀態」* 值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到 Microsoft Store 以供擷取。 您可以繼續使用先前的方法，或造訪合作夥伴中心監視提交進度。

<span/>

## <a name="code-examples"></a>程式碼範例

下列文章提供詳細的程式碼範例，示範如何以多個不同的程式設計語言來建立附加元件提交：

* [C# 程式碼範例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 程式碼範例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 程式碼範例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模組

另一種直接呼叫 Microsoft Store 提交 API 的方法，我們也提供了開放原始碼 PowerShell 模組，該模組在 API 上方實作命令列介面。 這個模組稱為 [StoreBroker](https://aka.ms/storebroker)。 您可以使用此模組從命令列來管理您的應用程式、正式發行前小眾測試版和附加元件提交，而不是直接直接呼叫 Microsoft Store 提交 API，也可以簡單瀏覽來源以檢視有關如何呼叫此 API 的更多範例。 StoreBroker 模組在 Microsoft 中積極地被用作為將眾多第一方應用程式提交至 Microsoft Store 的主要方式。

如需詳細資訊，請查看我們[在 GitHub 上的 StoreBroker 頁面](https://aka.ms/storebroker)。

<span/>

## <a name="data-resources"></a>資料資源

用於管理加元件提交的 Microsoft Store 提交 API 方法，使用下列 JSON 資料資源。

<span id="add-on-submission-object" />

### <a name="add-on-submission-resource"></a>附加元件提交資源

此資源描述附加元件提交。

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
    "priceId": "Free",
    "isAdvancedPricingModel": true
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

| 值      | 類型   | 描述        |
|------------|--------|----------------------|
| id            | 字串  | 提交的識別碼。 在回應資料中可將此識別碼用於要求，以[建立附加元件提交](create-an-add-on-submission.md)、[取得所有的附加元件](get-all-add-ons.md)，以及[取得附加元件](get-an-add-on.md)。 對於在合作夥伴中心中建立的提交，這個 ID 也是適用於在合作夥伴中心提交頁面的 URL。  |
| contentType           | 字串  |  附加元件中提供的[內容類型](../publish/enter-add-on-properties.md#content-type)。 這可以是下列其中一個值： <ul><li>NotSet</li><li>BookDownload</li><li>EMagazine</li><li>ENewspaper</li><li>MusicDownload</li><li>MusicStream</li><li>OnlineDataStorage</li><li>VideoDownload</li><li>VideoStream</li><li>Asp</li><li>OnlineDownload</li></ul> |  
| keywords           | array  | 這個字串陣列可針對附加元件包含最多 10 個[關鍵字](../publish/enter-add-on-properties.md#keywords)。 您的 App 可以使用這些關鍵字查詢附加元件。   |
| lifetime           | 字串  |  附加元件的存留期。 這可以是下列其中一個值： <ul><li>Forever</li><li>OneDay</li><li>ThreeDays</li><li>FiveDays</li><li>OneWeek</li><li>TwoWeeks</li><li>OneMonth</li><li>TwoMonths</li><li>ThreeMonths</li><li>SixMonths</li><li>OneYear</li></ul> |
| listings           | 物件  |  索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[清單資源](#listing-object)，其中包含附加元件的清單資訊。  |
| pricing           | 物件  | [定價資源](#pricing-object)包含附加元件的定價資訊。   |
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |
| tag           | 字串  |  附加元件的[自訂開發人員資料](../publish/enter-add-on-properties.md#custom-developer-data) (此資訊以前稱為 *「標記」*)。   |
| visibility  | 字串  |  附加元件的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>  |
| status  | 字串  |  提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件  |  [狀態詳細資料資源](#status-details-object)包含其他有關提交狀態的詳細資料，包括任何錯誤的資訊。 |
| fileUploadUrl           | 字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件，請將包含套件的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立附加元件提交](#create-an-add-on-submission)。  |
| friendlyName  | 字串  |  合作夥伴中心中所示之提交的易記名稱。 當您建立提交時，也會為您產生此值。  |

<span id="listing-object" />

### <a name="listing-resource"></a>清單資源

此資源包含[附加元件的清單資訊](../publish/create-add-on-store-listings.md)。 此資源具有下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|------|
|  description               |    字串     |   附加元件清單的描述。   |     
|  icon               |   物件      |[圖示資源](#icon-object)包含附加元件清單圖示的資料。    |
|  title               |     字串    |   附加元件清單的標題。   |  

<span id="icon-object" />

### <a name="icon-resource"></a>圖示資源

此資源包含附加元件清單的圖示資料。 此資源具有下列值。

| 值           | 類型    | 描述     |
|-----------------|---------|------|
|  fileName               |    字串     |   圖示檔案的名稱，位於您針對提交所上傳的 ZIP封存中。 圖示必須是 .png 檔案，大小必須是 300 x 300 像素。   |     
|  fileStatus               |   字串      |  圖示檔案的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |

<span id="pricing-object" />

### <a name="pricing-resource"></a>定價資源

此資源包含附加元件的定價資訊。 此資源具有下列值。

| 值           | 類型    | 描述    |
|-----------------|---------|------|
|  marketSpecificPricings               |    物件     |  索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的附加元件在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-prices)。 這個字典中的任何項目都會覆寫特定市場的 *priceId* 值所指定的基本價格。     |     
|  sales               |   陣列      |  **過時**。 包含附加元件的銷售資訊的[銷售資源](#sale-object)陣列。     |     
|  priceId               |   字串      |  指定附加元件[基本價格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#base-price)的[價格區間](#price-tiers)。    |    
|  isAdvancedPricingModel               |   布林值      |  若為 **true**，您的開發人員帳戶可以存取從 .99 美元到 1999.99 美元的展開價格區間。 若為 **false**，您的開發人員帳戶可以存取從 .99 美元到 999.99 美元的原始價格區間。 如需不同區間的詳細資訊，請參閱[價格區間](#price-tiers)。<br/><br/>**注意**&nbsp;&nbsp;此欄位為唯讀。   |


<span id="sale-object" />

### <a name="sale-resource"></a>銷售資源

此資源包含附加元件的銷售資訊。

> [!IMPORTANT]
> **「銷售」** 資源不再支援，目前您無法使用 Microsoft Store 提交 API 取得或修改附加元件提交的銷售資料。 我們將來會更新「Microsoft Store 提交 API」來導入新的方法，以程式設計方式存取附加元件提交的銷售資訊。
>    * 在呼叫 [GET 方法以取得附加元件提交](get-an-add-on-submission.md)之後，*sales* 值將會空白。 您可以繼續使用合作夥伴中心取得您的附加元件提交的銷售資料。
>    * 呼叫 [PUT 方法以更新附加元件提交](update-an-add-on-submission.md)時，會忽略 *sales* 值中的資訊。 您可以繼續使用合作夥伴中心來變更您的附加元件提交的銷售資料。

此資源具有下列值。

| 值           | 類型    | 描述           |
|-----------------|---------|------|
|  name               |    字串     |   銷售的名稱。    |     
|  basePriceId               |   字串      |  用於銷售基本價格的[價格區間](#price-tiers)。    |     
|  startDate               |   字串      |   ISO 8601 格式的銷售開始日期。  |     
|  endDate               |   字串      |  ISO 8601 格式的銷售結束日期。      |     
|  marketSpecificPricings               |   物件      |   索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的附加元件在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/set-iap-pricing-and-availability#markets-and-custom-pricess)。 這個字典中的任何項目都會覆寫特定市場的 *basePriceId* 值所指定的基本價格。    |

<span id="status-details-object" />

### <a name="status-details-resource"></a>狀態詳細資料資源

此資源包含關於提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|------|
|  errors               |    物件     |   包含提交的錯誤詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。   |     
|  warnings               |   物件      | 包含提交的警告詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。     |
|  certificationReports               |     物件    |   提供提交認證報告資料存取的[認證報告資源](#certification-report-object)陣列。 如果認證失敗，您可以檢查這些報告來取得詳細資訊。    |  

<span id="status-detail-object" />

### <a name="status-detail-resource"></a>狀態詳細資料資源

此資源包含有關提交的任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 描述    |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的[提交狀態碼](#submission-status-code)。   |     
|  details               |     字串    |  含有更多關於問題之詳細資料的訊息。     |

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>認證報告資源

此資源提供提交認證報告資料的存取。 此資源具有下列值。

| 值           | 類型    | 描述               |
|-----------------|---------|------|
|     日期            |    字串     |  日期和時間產生報告，格式為 ISO 8601。    |
|     reportUrl            |    字串     |  您可以存取報告的 URL。    |

## <a name="enums"></a>列舉

這些方法會使用下列列舉。

<span id="price-tiers" />

### <a name="price-tiers"></a>價格區間

下列值代表附加元件提交的[價格資源](#pricing-object)資源的可用價格區間。

| 值           | 描述       |
|-----------------|------|
|  Base               |   未設定價格區間；使用附加元件的基本價格。      |     
|  NotAvailable              |   特定區域中無法使用此附加元件。    |     
|  Free              |   附加元件是免費的。    |    
|  層*xxxx*               |   指定附加元件的價格區間的字串，格式為**第 <em>xxxx</em> 層**。 目前支援下列價格區間範圍︰<br/><br/><ul><li>如果[價格資源](#pricing-object) 的 *isAdvancedPricingModel* 值為**true**，您帳戶的可用價格區間值是 **Tier1012** - **Tier1424**。</li><li>如果[價格資源](#pricing-object) 的 *isAdvancedPricingModel* 值為**false**，您帳戶的可用價格區間值是 **Tier2** - **Tier96**。</li></ul>若要查看完整的價格表可供您的開發人員帳戶，包括與每個層，相關聯的特定市場的價格區間，請移至任何您在合作夥伴中心的應用程式提交的**定價和可用性**頁面和按一下 [**市場和自訂價格**] 區段中的**檢視表**連結 （對於某些開發人員帳戶，此連結是**價格**] 區段中）。     |

<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交狀態碼

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

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 管理附加元件](manage-add-ons.md)
* [在合作夥伴中心的附加元件提交](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)
