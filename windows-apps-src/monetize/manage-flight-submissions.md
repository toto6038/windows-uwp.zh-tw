---
ms.assetid: 2A454057-FF14-40D2-8ED2-CEB5F27E0226
description: 在 Microsoft Store 提交 API 中使用這些方法，來管理已註冊至合作夥伴中心帳戶之應用程式的套件航班提交。
title: 管理封裝航班提交
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10，uwp，Microsoft Store 提交 API，航班提交
ms.localizationpriority: medium
ms.openlocfilehash: 50596fdadae2ac4a0625687e7c8acaf985ccfaa7
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690380"
---
# <a name="manage-package-flight-submissions"></a>管理封裝航班提交

Microsoft Store 提交 API 提供的方法可讓您用來管理應用程式的套件航班提交，包括逐步封裝的推出。 如需 Microsoft Store 提交 API 的簡介，包括使用 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果您使用 Microsoft Store 提交 API 來建立套件航班的提交，請務必使用 API （而不是合作夥伴中心）對提交進行進一步變更。 如果您使用儀表板來變更原本使用 API 所建立的提交，就無法再使用 API 來變更或認可該提交。 在某些情況下，提交可能處於無法在提交程式中繼續進行的錯誤狀態。 如果發生這種情況，您必須刪除提交，並建立新的提交。

<span id="methods-for-package-flight-submissions" />

## <a name="methods-for-managing-package-flight-submissions"></a>管理封裝航班提交的方法

使用下列方法來取得、建立、更新、認可或刪除封裝航班提交。 在您可以使用這些方法之前，套件飛行必須已經存在於合作夥伴中心。 您可以[在合作夥伴中心](https://docs.microsoft.com/windows/uwp/publish/package-flights)建立套件飛行，或使用 [[管理封裝航班](manage-flights.md)] 中所述的 Microsoft Store 提交 API 方法。

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
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="get-a-flight-submission.md">取得現有的套件航班提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-a-flight-submission.md">取得現有的套件航班提交狀態</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions</td>
<td align="left"><a href="create-a-flight-submission.md">建立新的套件航班提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="update-a-flight-submission.md">更新現有的套件航班提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-a-flight-submission.md">認可新的或更新的套件航班提交</a></td>
</tr>
<tr>
<td align="left">刪除</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-a-flight-submission.md">刪除套件航班提交</a></td>
</tr>
</tbody>
</table>

<span id="create-a-package-flight-submission">

## <a name="create-a-package-flight-submission"></a>建立套件航班提交

若要建立封裝航班的提交，請遵循此程式。

1. 如果您尚未這麼做，請完成[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)中所述的必要條件，包括將 Azure AD 應用程式與您的合作夥伴中心帳戶產生關聯，以及取得您的用戶端識別碼和金鑰。 您只需要執行此動作一次，擁有用戶端識別碼和金鑰之後，您就可以在需要建立新的 Azure AD 存取權杖時重複使用它們。  

2. [取得 Azure AD 的存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞給 Microsoft Store 提交 API 中的方法。 取得存取權杖之後，您會有60分鐘的時間，可以在到期前使用。 權杖到期之後，您可以取得新的權杖。

3. 在 Microsoft Store 提交 API 中執行下列方法，以[建立套件航班提交](create-a-flight-submission.md)。 這個方法會建立新的進行中提交，這是您上次發行提交的複本。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions
    ```

    回應主體包含[航班提交](#flight-submission-object)資源，其中包含新提交的識別碼、共用存取簽章（SAS） URI，用於上傳提交至 Azure Blob 儲存體的任何套件，以及新提交的資料（包括全部清單和定價資訊）。

    > [!NOTE]
    > SAS URI 可讓您存取 Azure 儲存體中的安全資源，而不需要帳戶金鑰。 如需有關 SAS Uri 及其搭配 Azure Blob 儲存體使用的背景資訊，請參閱[共用存取簽章，第1部分：瞭解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)和[共用存取簽章，第2部分：建立和使用 sas 與 Blob 儲存體](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果您要為提交新增新的封裝，請[準備封裝](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements)，並將它們新增到 ZIP 封存。

5. 使用新提交的任何必要變更修訂[航班提交](#flight-submission-object)資料，並執行下列方法來[更新套件航班提交](update-a-flight-submission.md)。

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果您要新增提交的新套件，請務必更新提交資料，以參照 ZIP 封存中這些檔案的名稱和相對路徑。

4. 如果您要為提交新增套件，請使用您稍早呼叫的 POST 方法回應本文中提供的 SAS URI，將 ZIP 封存上傳至[Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)。 您可以使用不同的 Azure 程式庫在各種平臺上執行這項操作，包括：

    * [適用於 .NET 的 Azure 儲存體用戶端程式庫](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK for Java](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
    * [Azure Storage SDK for Python](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

    下列C#程式碼範例示範如何使用適用于 .net 的 Azure 儲存體用戶端程式庫中的[CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob)類別，將 ZIP 封存上傳至 Azure Blob 儲存體。 這個範例假設 ZIP 封存已寫入資料流程物件。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 藉由執行下列方法來[認可封裝航班提交](commit-a-flight-submission.md)。 這會通知合作夥伴中心您已完成提交，且您的更新現在應套用至您的帳戶。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit
    ```

6. 藉由執行下列方法來檢查認可狀態，以[取得封裝航班提交的狀態](get-status-for-a-flight-submission.md)。

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status
    ```

    若要確認提交狀態，請檢查回應主體中的*狀態值*。 如果要求成功，此值應從**CommitStarted**變更為前置處理，或在要求中有錯誤時 **，才會**進行**CommitFailed** 。 如果發生錯誤， *statusDetails*欄位會包含有關錯誤的進一步詳細資料。

7. 認可順利完成之後，提交會傳送至存放區進行內嵌。 您可以使用先前的方法或造訪合作夥伴中心，繼續監視提交進度。

<span/>

## <a name="code-examples"></a>程式碼範例

下列文章提供詳細的程式碼範例，示範如何以數種不同的程式設計語言建立封裝航班提交：

* [C#程式碼範例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [JAVA 程式碼範例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 程式碼範例](python-code-examples-for-the-windows-store-submission-api.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模組

除了直接呼叫 Microsoft Store 提交 API 之外，我們也提供開放原始碼 PowerShell 模組，以在 API 之上執行命令列介面。 此模組稱為[StoreBroker](https://aka.ms/storebroker)。 您可以使用此模組從命令列管理您的應用程式、航班和附加元件提交，而不是直接呼叫 Microsoft Store 提交 API，或者只要流覽來源，即可查看更多如何呼叫此 API 的範例。 StoreBroker 模組會在 Microsoft 中主動使用，做為許多第一方應用程式提交至存放區的主要方式。

如需詳細資訊，請參閱[GitHub 上的 StoreBroker 頁面](https://aka.ms/storebroker)。

<span id="manage-gradual-package-rollout">

## <a name="manage-a-gradual-package-rollout-for-a-package-flight-submission"></a>管理封裝航班提交的逐步封裝推出

您可以將套件航班提交中已更新的套件逐漸推出至 Windows 10 上的應用程式客戶百分比。 這可讓您監視特定套件的意見反應和分析資料，以確保您在更廣泛地推出更新之前，先安心。 您可以變更已發行提交的推出百分比（或終止更新），而不需要建立新的提交。 如需更多詳細資料，包括如何在合作夥伴中心啟用和管理逐步封裝推出的指示，請參閱[這篇文章](../publish/gradual-package-rollout.md)。

若要以程式設計方式啟用封裝航班提交的逐步封裝推出，請遵循此程式，使用 Microsoft Store 提交 API 中的方法：

  1. [建立套件航班提交](create-a-flight-submission.md)或[取得套件航班提交](get-a-flight-submission.md)。
  2. 在回應資料中，找出[packageRollout](#package-rollout-object)資源，將*isPackageRollout*欄位設定為 True，並將*packageRolloutPercentage*欄位設定為應用程式客戶的百分比，應取得更新的套件。
  3. 將更新的套件航班提交資料傳遞至[更新套件航班提交](update-a-flight-submission.md)方法。

在啟用封裝航班提交的漸進式封裝推出之後，您可以使用下列方法，以程式設計方式取得、更新、停止或完成逐步推出。

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
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-a-flight-submission.md">取得套件航班提交的逐步推出資訊</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-a-flight-submission.md">更新封裝航班提交的逐步推出百分比</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-a-flight-submission.md">暫停封裝航班提交的逐步推出</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-a-flight-submission.md">完成封裝航班提交的逐步推出</a></td>
</tr>
</tbody>
</table>

<span/>

## <a name="data-resources"></a>資料資源

用於管理封裝航班提交的 Microsoft Store 提交 API 方法會使用下列 JSON 資料資源。

<span id="flight-submission-object" />

### <a name="flight-submission-resource"></a>航班提交資源

此資源描述套件航班提交。

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
        "packageRolloutPercentage": 0.0,
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

| 值      | 類型   | 說明              |
|------------|--------|------------------------------|
| id            | 字串  | 提交的識別碼。  |
| flightId           | 字串  |  與提交相關聯之封裝航班的識別碼。  |  
| status           | 字串  | 提交的狀態。 此屬性可為下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>發佈</li><li>Published</li><li>PublishFailed</li><li>預處理</li><li>PreProcessingFailed</li><li>認證</li><li>CertificationFailed</li><li>版本</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件  |  [狀態詳細資料資源](#status-details-object)，其中包含有關提交狀態的其他詳細資料，包括任何錯誤的相關資訊。  |
| flightPackages           | array  | 包含[航班封裝資源](#flight-package-object)，可提供提交中每個套件的詳細資料。   |
| packageDeliveryOptions    | 物件  | [封裝傳遞選項資源](#package-delivery-options-object)，包含逐步封裝推出和提交的強制更新設定。   |
| fileUploadUrl           | 字串  | 用於上傳提交之任何封裝的共用存取簽章（SAS） URI。 如果您要為提交新增封裝，請將包含封裝的 ZIP 封存上傳至此 URI。 如需詳細資訊，請參閱[建立套件航班提交](#create-a-package-flight-submission)。  |
| targetPublishMode           | 字串  | 提交的發佈模式。 此屬性可為下列其中一個值： <ul><li>立即</li><li>手動</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果*targetPublishMode*設定為 SpecificDate，則為 ISO 8601 格式提交的發行日期。  |
| notesForCertification           | 字串  |  為憑證測試人員提供其他資訊，例如測試帳號憑證和存取和驗證功能的步驟。 如需詳細資訊，請參閱[認證的注意事項](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)。 |

<span id="status-details-object" />

### <a name="status-details-resource"></a>狀態詳細資料資源

此資源包含提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | 說明                   |
|-----------------|---------|------|
|  錯誤               |    物件     |   [狀態詳細資料資源](#status-detail-object)的陣列，其中包含提交的錯誤詳細資料。   |     
|  消息               |   物件      | [狀態詳細資料資源](#status-detail-object)的陣列，其中包含提交的警告詳細資料。     |
|  certificationReports               |     物件    |   [認證報告資源](#certification-report-object)的陣列，可讓您存取認證報告資料以進行提交。 如果認證失敗，您可以檢查這些報告以取得詳細資訊。    |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>狀態詳細資料資源

此資源包含有關提交之任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 說明       |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的[提交狀態碼](#submission-status-code)。 |  
|  詳細資料               |     字串    |  包含有關問題詳細資料的訊息。     |


<span id="certification-report-object" />

### <a name="certification-report-resource"></a>認證報表資源

此資源可讓您存取認證報告資料以進行提交。 此資源具有下列值。

| 值           | 類型    | 說明         |
|-----------------|---------|------|
|     日期            |    字串     |  產生報告的日期和時間，格式為 ISO 8601。    |
|     reportUrl            |    字串     |  您可以存取報表的 URL。    |


<span id="flight-package-object" />

### <a name="flight-package-resource"></a>航班封裝資源

此資源會提供有關提交中套件的詳細資料。

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

> [!NOTE]
> 呼叫[更新套件航班提交](update-a-flight-submission.md)方法時，要求主體中只需要這個物件的*fileName*、 *fileStatus*、 *minimumDirectXVersion*和*minimumSystemRam*值。 其他值會由合作夥伴中心填入。

| 值           | 類型    | 說明              |
|-----------------|---------|------|
| fileName   |   字串      |  封裝的名稱。    |  
| fileStatus    | 字串    |  封裝的狀態。 此屬性可為下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>已上傳</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  可唯一識別封裝的識別碼。 合作夥伴中心會使用此值。   |     
| 版本    |  字串   |  應用程式套件的版本。 如需詳細資訊，請參閱[套件版本編號](https://docs.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| 架構    |  字串   |  應用程式套件（例如 ARM）的架構。   |     
| 語言    | array    |  應用程式所支援語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](https://docs.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  array   |  封裝所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能聲明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字串   |  應用程式套件支援的最低 DirectX 版本。 這只能針對以 Windows 8.x 為目標的應用程式進行設定;以其他版本為目標的應用程式會忽略它。 此屬性可為下列其中一個值： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  應用程式套件所需的最小 RAM。 這只能針對以 Windows 8.x 為目標的應用程式進行設定;以其他版本為目標的應用程式會忽略它。 此屬性可為下列其中一個值： <ul><li>None</li><li>Memory2GB</li></ul>   |    


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>封裝傳遞選項資源

此資源包含逐步封裝推出和提交的強制更新設定。

```json
{
  "packageDeliveryOptions": {
    "packageRollout": {
        "isPackageRollout": false,
        "packageRolloutPercentage": 0.0,
        "packageRolloutStatus": "PackageRolloutNotStarted",
        "fallbackSubmissionId": "0"
    },
    "isMandatoryUpdate": false,
    "mandatoryUpdateEffectiveDate": "1601-01-01T00:00:00.0000000Z"
  },
}
```

此資源具有下列值。

| 值           | 類型    | 說明        |
|-----------------|---------|------|
| packageRollout   |   物件      |   [封裝首](#package-rollout-object)度發行資源，包含提交的逐步封裝推出設定。    |  
| isMandatoryUpdate    | 布林值    |  指出您是否要將此提交中的套件視為自動安裝應用程式更新的必要元件。 如需有關自行安裝應用程式更新之強制套件的詳細資訊，請參閱[下載並安裝應用程式的套件更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  日期   |  這項提交中的封裝變成必要時的日期和時間，採用 ISO 8601 格式和 UTC 時區。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>套件首度發行資源

此資源包含提交的逐步[封裝推出設定](#manage-gradual-package-rollout)。 此資源具有下列值。

| 值           | 類型    | 說明        |
|-----------------|---------|------|
| isPackageRollout   |   布林值      |  指出是否已啟用逐步封裝推出以進行提交。    |  
| packageRolloutPercentage    | float    |  會在逐步推出中接收封裝的使用者百分比。    |  
| packageRolloutStatus    |  字串   |  下列其中一個字串，表示漸進式封裝推出的狀態： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字串   |  未取得逐步推出套件的客戶所收到的提交識別碼。   |          

> [!NOTE]
> *PackageRolloutStatus*和*fallbackSubmissionId*值是由合作夥伴中心指派，並不適合由開發人員設定。 如果您在要求主體中包含這些值，將會忽略這些值。

<span/>

## <a name="enums"></a>列舉

這些方法會使用下列列舉。

<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交狀態碼

下列代碼代表提交的狀態。

| 代碼           |  說明      |
|-----------------|---------------|
|  None            |     未指定任何程式碼。         |     
|      InvalidArchive        |     包含封裝的 ZIP 封存無效，或具有無法辨識的封存格式。  |
| MissingFiles | ZIP 封存並未包含提交資料中列出的所有檔案，或其位於封存檔案錯誤的位置。 |
| PackageValidationFailed | 提交中的一或多個套件無法驗證。 |
| InvalidParameterValue | 要求主體中的其中一個參數無效。 |
| InvalidOperation | 您嘗試的操作無效。 |
| InvalidState | 您嘗試的作業對封裝航班的目前狀態無效。 |
| ResourceNotFound | 找不到指定的封裝航班。 |
| ServiceError | 發生內部服務錯誤，導致要求無法成功。 請重試一次要求。 |
| ListingOptOutWarning | 開發人員已從先前的提交中移除清單，或未包含套件所支援的清單資訊。 |
| ListingOptInWarning  | 開發人員已加入清單。 |
| UpdateOnlyWarning | 開發人員嘗試插入只有更新支援的東西。 |
| 其他  | 提交處於無法辨識或未分類的狀態。 |
| PackageValidationWarning | 封裝驗證程式造成警告。 |

<span/>

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務來建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 來管理套件航班](manage-flights.md)
* [取得套件航班提交](get-a-flight-submission.md)
* [建立套件航班提交](create-a-flight-submission.md)
* [更新套件航班提交](update-a-flight-submission.md)
* [認可封裝航班提交](commit-a-flight-submission.md)
* [刪除套件航班提交](delete-a-flight-submission.md)
* [取得套件航班提交的狀態](get-status-for-a-flight-submission.md)
