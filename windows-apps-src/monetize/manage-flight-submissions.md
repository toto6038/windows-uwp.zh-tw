---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理套件正式發行前小眾測試版提交。"
title: "使用 Windows 市集提交 API 管理套件正式發行前小眾測試版提交"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 18d28495b80101cf5cfe53869b0f5cd3d61b50c9

---

# 使用 Windows 市集提交 API 管理套件正式發行前小眾測試版提交




在 Windows 市集提交 API 中使用下列方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理套件正式發行前小眾測試版提交。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的先決條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 套件正式發行前小眾測試版必須已經存在於您的開發人員中心帳戶，您才能使用這些方法來建立或管理套件正式發行前小眾測試版的提交。 您可以[使用開發人員中心儀表板](https://msdn.microsoft.com/windows/uwp/publish/package-flights)或使用[管理套件正式發行前小眾測試版](manage-flights.md)中所述的 Windows 市集提交 API 方法，來建立套件正式發行前小眾測試版。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 取得現有套件正式發行前小眾測試版提交的資料。 如需詳細資訊，請參閱[取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` | 取得現有套件正式發行前小眾測試版提交的狀態。 如需詳細資訊，請參閱[取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/applications/{applicationId}/flights/{flightId}/submissions``` | 為登錄到您 Windows 開發人員中心帳戶的 App 建立新的套件正式發行前小眾測試版提交。 如需詳細資訊，請參閱[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` | 將新的或更新的套件正式發行前小眾測試版提交認可到 Windows 開發人員中心。 如需詳細資訊，請參閱[認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 更新現有的套件正式發行前小眾測試版提交。 如需詳細資訊，請參閱[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 刪除套件正式發行前小眾測試版提交。 如需詳細資訊，請參閱[刪除套件正式發行前小眾測試版提交](delete-a-flight-submission.md)。 |

<span id="create-a-package-flight-submission">
## 建立套件正式發行前小眾測試版提交

若要建立套件正式發行前小眾測試版的提交，請遵循此程序。

1. 如果您尚未執行此動作，請先完成[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的先決條件，包括將 Azure AD 應用程式關聯至您的 Windows 開發人員中心帳戶，並取得您的用戶端識別碼和金鑰。 您只需執行此動作一次；有了用戶端識別碼和金鑰之後，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。  

2. [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Windows 市集提交 API 中的方法。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

3. 在 Windows 市集提交 API 中執行下列方法。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。 如需詳細資訊，請參閱[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  回應主體包含三個項目︰新提交的識別碼、新提交的資料 (包含所有清單和定價資訊)，以及用於上傳提交的任何套件的共用存取簽章 (SAS) URI。 如需 SAS 的詳細資訊，請參閱[共用存取簽章，第 1 部分︰了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果您要新增提交的新套件，請[準備套件](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)並將它們新增到 ZIP 封存。

5. 藉由對新的提交進行任何必要變更來更新提交資料，然後執行下列方法來更新提交。 如需詳細資訊，請參閱[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  >**注意**&nbsp;&nbsp;如果您要新增提交的新套件，確定您會更新提交資料，以參考 ZIP 封存中的名稱和這些檔案的相對路徑。

4. 如果您要新增提交的新套件，將 ZIP 封存上傳至您在步驟 2 中呼叫之 POST 方法回應主體中提供的 SAS URI。 如需詳細資訊，請參閱[共用存取簽章，第 2 部分︰透過 Blob 儲存體來建立與使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  下列程式碼片段示範如何在適用於 .NET 的 Azure 儲存體用戶端程式庫中使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 類別來上傳封存

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 執行下列方法來認可提交。 這將警示開發人員中心，您已經完成提交，而現在應該將更新套用至您的帳戶。 如需詳細資訊，請參閱[認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. 執行下列方法來檢查認可狀態。 如需詳細資訊，請參閱[取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  若要確認提交狀態，請檢閱回應主體中的「狀態」**值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到市集以供擷取。 您可以繼續使用先前的方法，或瀏覽開發人員中心儀表板來監視提交進度。

## 資源

這些方法會使用下列資料資源。

<span id="flight-submission-object" />
### 套件正式發行前小眾測試版提交

此資源代表套件正式發行前小眾測試版的提交。 下列範例示範此資源的格式。

```json
{
  "id": "1152921504621243649",
  "flightId": "cd2e368a-0da5-4026-9f34-0e7934bc6f23",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/8b389577-5d5e-4cbe-a744-1ff2e97a9eb8?sv=2014-02-14&sr=b&sig=wgMCQPjPDkuuxNLkeG35rfHaMToebCxBNMPw7WABdXU%3D&se=2016-06-17T21:29:44Z&sp=rwl",
  "targetPublishMode": "Immediate",
  "targetPublishDate": "",
  "notesForCertification": "No special steps are required for certification of this app."
}
```

此資源具有下列值。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id            | 字串  | 提交的識別碼。  |
| flightId           | 字串  |  包含要與提交產生關聯之套件正式發行前小眾測試版的識別碼。  |  
| status           | 字串  | 提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件  |  包含其他有關提交狀態的詳細資料 (包括任何錯誤的資訊)。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-details-object)一節。 |
| flightPackages           | 陣列  | 包含可提供關於提交中每個套件之詳細資料的物件。 如需詳細資訊，請參閱下方的[套件正式發行前小眾測試版](#flight-package-object)一節。  |
| fileUploadUrl           | 字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件，請將包含套件的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立套件正式發行前小眾測試版提交](#create-a-package-flight-submission)。  |
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |
| notesForCertification           | 字串  |  為認證測試人員提供其他資訊，例如測試帳戶認證，以及存取和確認功能的步驟。 如需詳細資訊，請參閱[認證注意事項](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。 |

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


<span id="flight-package-object" />
### 正式發行前小眾測試版套件

此資源提供關於提交中套件的詳細資料。 下列範例示範此資源的格式。

```json
{
  "flightPackages": [
    {
      "fileName": "newPackage.appx",
      "fileStatus": "PendingUpload",
      "id": "",
      "version": "1.0.0.0",
      "languages": ["en-us"],
      "capabilities": [],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None"
    }
  ],
}
```

此資源具有下列值。

>**注意**&nbsp;&nbsp;在呼叫[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)方法時，要求主體中只需要這個物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 及 *minimumSystemRam* 值。 其他值均是由開發人員中心所填入。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
| fileName   |   字串      |  套件的名稱。    |  
| fileStatus    | 字串    |  套件的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  唯一識別套件的識別碼。 此值由開發人員中心所使用。   |     
| version    |  字串   |  App 套件的版本。 如需詳細資訊，請參閱[套件版本編號](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字串   |  App 套件的架構 (例如，ARM)。   |     
| languages    | 陣列    |  App 所支援之語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  陣列   |  套件所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能宣告](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字串   |  App 套件所支援的最低 DirectX 版本。 這只能針對目標為 Windows 8.x 的 App 進行設定；對於目標為其他版本的 App 則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  App 套件所需的最小 RAM。 這只能針對目標為 Windows 8.x 的 App 進行設定；對於目標為其他版本的 App 則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>Memory2GB</li></ul>   |    

<span/>

## 列舉

這些方法會使用下列列舉。

<span id="submission-status-code" />
### 提交狀態碼

下列代碼代表提交的狀態。

| 代碼           |  描述      |
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
* [使用 Windows 市集提交 API 管理套件正式發行前小眾測試版](manage-flights.md)
* [取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)
* [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)
* [更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)
* [認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)
* [刪除套件正式發行前小眾測試版提交](delete-a-flight-submission.md)
* [取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)



<!--HONumber=Aug16_HO5-->


