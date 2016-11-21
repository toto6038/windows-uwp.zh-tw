---
author: mcleanbyron
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理套件正式發行前小眾測試版提交。"
title: "使用 Windows 市集提交 API 管理套件正式發行前小眾測試版提交"
translationtype: Human Translation
ms.sourcegitcommit: 9b76a11adfab838b21713cb384cdf31eada3286e
ms.openlocfilehash: 7b59bb255774c8050232831e7f0d7a78a921ec6d

---

# 使用 Windows 市集提交 API 管理套件正式發行前小眾測試版提交




在 Windows 市集提交 API 中使用下列方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理套件正式發行前小眾測試版提交。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的先決條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>
            **注意**
            &nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。 套件正式發行前小眾測試版必須已經存在於您的開發人員中心帳戶，您才能使用這些方法來建立或管理套件正式發行前小眾測試版的提交。 您可以[使用開發人員中心儀表板](https://msdn.microsoft.com/windows/uwp/publish/package-flights)或使用[管理套件正式發行前小眾測試版](manage-flights.md)中所述的 Windows 市集提交 API 方法，來建立套件正式發行前小眾測試版。

| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 取得現有套件正式發行前小眾測試版提交的資料。 如需詳細資訊，請參閱[這篇文章](get-a-flight-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status``` | 取得現有套件正式發行前小眾測試版提交的狀態。 如需詳細資訊，請參閱[這篇文章](get-status-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions``` | 為已向您「Windows 開發人員中心」帳戶登錄的 App 建立新的套件正式發行前小眾測試版提交。 如需詳細資訊，請參閱[這篇文章](create-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` | 將新的或更新的套件正式發行前小眾測試版提交認可到「Windows 開發人員中心」。 如需詳細資訊，請參閱[這篇文章](commit-a-flight-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 更新現有的套件正式發行前小眾測試版提交。 如需詳細資訊，請參閱[這篇文章](update-a-flight-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}``` | 刪除套件正式發行前小眾測試版提交。 如需詳細資訊，請參閱[這篇文章](delete-a-flight-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout``` | 取得套件正式發行前小眾測試版提交的漸進式推出資訊。 如需詳細資訊，請參閱[這篇文章](get-package-rollout-info-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage``` | 更新套件正式發行前小眾測試版提交的漸進式推出百分比。 如需詳細資訊，請參閱[這篇文章](update-the-package-rollout-percentage-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout``` | 停止套件正式發行前小眾測試版提交的漸進式推出。 如需詳細資訊，請參閱[這篇文章](halt-the-package-rollout-for-a-flight-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout``` | 完成套件正式發行前小眾測試版提交的漸進式推出。 如需詳細資訊，請參閱[這篇文章](finalize-the-package-rollout-for-a-flight-submission.md)。 |

<span id="create-a-package-flight-submission">
## 建立套件正式發行前小眾測試版提交

若要建立套件正式發行前小眾測試版的提交，請遵循此程序。

1. 如果您尚未執行此動作，請先完成[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的先決條件，包括將 Azure AD 應用程式關聯至您的 Windows 開發人員中心帳戶，並取得您的用戶端識別碼和金鑰。 您只需執行此動作一次；有了用戶端識別碼和金鑰之後，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。  

2. 
            [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Windows 市集提交 API 中的方法。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

3. 在「Windows 市集提交 API」中執行下列方法來[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications{applicationId}/flights/{flightId}/submissions
  ```

  回應主體包含三個項目︰新提交的識別碼、新提交的資料 (包含所有清單和定價資訊)，以及用於上傳提交的任何套件的共用存取簽章 (SAS) URI。 如需 SAS 的詳細資訊，請參閱[共用存取簽章，第 1 部分︰了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果您要為提交新增新的套件，請[準備套件](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)並將它們新增到 ZIP 封存。

5. 以新提交的任何所需變更來修訂提交資料，然後執行下列方法來[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
  ```

  >
            **注意**
            &nbsp;&nbsp;如果您要為提交新增新的套件，請確定將提交資料更新成參考 ZIP 封存中這些檔案的名稱和相對路徑。

4. 如果您要新增提交的新套件，將 ZIP 封存上傳至您在步驟 2 中呼叫之 POST 方法回應主體中提供的 SAS URI。 如需詳細資訊，請參閱[共用存取簽章，第 2 部分︰透過 Blob 儲存體來建立與使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  下列程式碼片段示範如何使用「適用於 .NET 的 Azure 儲存體用戶端程式庫」中的 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 類別來上傳封存。

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 執行下列方法來[認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)。 這會向「開發人員中心」發出警示，指出您已完成提交，而現在應該將更新套用至您的帳戶。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
  ```

6. 執行下列方法來[取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)，以檢查認可狀態。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
  ```

  若要確認提交狀態，請檢閱回應主體中的「狀態」值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到市集以供擷取。 您可以使用先前的方法或瀏覽「開發人員中心」儀表板，來繼續監視提交進度。

<span id="manage-gradual-package-rollout">
## 管理套件正式發行前小眾測試版提交的漸進式套件推出

您可以將套件正式發行前小眾測試版提交中的已更新套件，以漸進方式推出給 Windows10 上的一部分 App 客戶。 這可讓您監視特定套件的意見反應和分析資料，以確保您對該項更新信心十足之後，再更廣泛地推出該項更新。 您可以變更所發佈之提交的推出百分比 (或停止更新)，而不需建立新的提交。 如需詳細資訊 (包括如何在「開發人員中心」儀表板中啟用和管理漸進式套件推出的指示)，請參閱[這篇文章](../publish/gradual-package-rollout.md)。

您也可以在「Windows 市集提交 API」中使用下列方法，以程式設計方式啟用和管理套件正式發行前小眾測試版提交的漸進式套件推出。

* 啟用套件正式發行前小眾測試版提交的漸進式套件推出：

  1. 
            [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)或[取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)。
  2. 在回應資料中，找出 [packageRollout](#package-rollout-object) 資源，將 *isPackageRollout* 欄位設定為 true，然後將 *packageRolloutPercentage* 欄位設定為應該取得已更新套件的 App 客戶百分比。
  3. 將已更新的套件正式發行前小眾測試版提交資料傳遞給[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)方法。

<span/>

* 若要[取得套件正式發行前小眾測試版提交的套件推出資訊](get-package-rollout-info-for-a-flight-submission.md)，請執行下列方法。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout
  ```

<span/>

* 若要[更新套件正式發行前小眾測試版提交的套件推出百分比](update-the-package-rollout-percentage-for-a-flight-submission.md)，請執行下列方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage  
  ```

<span/>

* 若要[停止套件正式發行前小眾測試版提交的套件推出](halt-the-package-rollout-for-a-flight-submission.md)，請執行下列方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout   
  ```  

<span/>

* 若要[完成套件正式發行前小眾測試版提交的套件推出](finalize-the-package-rollout-for-a-flight-submission.md)，請執行下列方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout
  ```

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
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
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
| packageDeliveryOptions    | 物件  | 包含提交的漸進式套件推出和強制更新設定。 如需詳細資訊，請參閱下方的[套件交付選項物件](#package-delivery-options-object)一節。  |
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

>
            **注意**
            &nbsp;&nbsp;在呼叫[更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)方法時，要求主體中只需要這個物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 及 *minimumSystemRam* 值。 其他值均是由開發人員中心所填入。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
| fileName   |   字串      |  套件的名稱。    |  
| fileStatus    | 字串    |  套件的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  唯一識別套件的識別碼。 此值由開發人員中心所使用。   |     
| version    |  字串   |  App 套件的版本。 如需詳細資訊，請參閱[套件版本編號](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字串   |  App 套件的架構 (例如，ARM)。   |     
| languages    | 陣列    |  App 所支援之語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  陣列   |  套件所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能宣告](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字串   |  App 套件所支援的最低 DirectX 版本。 這只能針對目標為 Windows8.x 的 App 進行設定；對於目標為其他版本的 App 則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  App 套件所需的最小 RAM。 這只能針對目標為 Windows8.x 的 App 進行設定；對於目標為其他版本的 App 則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />
### 套件交付選項物件

此資源包含提交的漸進式套件推出和強制更新設定。 下列範例示範此資源的格式。

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| packageRollout   |   物件      |  包含提交的漸進式套件推出設定。 如需詳細資訊，請參閱下方的[套件推出物件](#package-rollout-object)一節。    |  
| isMandatoryUpdate    | 布林值    |  指出您是否要將這項提交中的套件視為自我安裝 App 更新的強制項目。 如需有關自我安裝 App 更新的強制套件詳細資訊，請參閱[下載與安裝 App 的套件更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  日期   |  這項提交中的套件變成強制項目的日期和時間，採用 ISO 8601 格式和 UTC 時區。   |        

<span id="package-rollout-object" />
### 套件推出物件

此資源包含提交的漸進式[套件推出設定](#manage-gradual-package-rollout)。 此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| isPackageRollout   |   布林值      |  指出是否已為提交啟用漸進式套件推出。    |  
| packageRolloutPercentage    | 浮點數    |  將接收漸進式推出中套件的使用者百分比。    |  
| packageRolloutStatus    |  字串   |  下列其中一個字串，這些字串指出漸進式套件推出的狀態： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字串   |  未取得漸進式推出套件的客戶將收到的提交識別碼。   |          

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



<!--HONumber=Nov16_HO1-->


