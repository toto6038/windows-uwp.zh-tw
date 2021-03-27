---
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: 在 Microsoft Store 提交 API 中使用這些方法來管理向合作夥伴中心帳戶註冊的應用程式提交。
title: 管理應用程式提交
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10、uwp、Microsoft Store 提交 API、App 提交
ms.localizationpriority: medium
ms.openlocfilehash: 45f4dd26920d0e323bd1efc13a378aec6eb52468
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619334"
---
# <a name="manage-app-submissions"></a>管理應用程式提交

Microsoft Store 提交 API 提供方法讓您使用於管理應用程式的提交，包括漸進式套件推出。 如需 Microsoft Store 提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

> [!IMPORTANT]
> 如果您使用 Microsoft Store 提交 API 來建立應用程式的提交，請務必使用 API 對提交進行進一步變更，而不是合作夥伴中心。 如果您使用合作夥伴中心來變更最初使用 API 所建立的提交，您將無法再使用 API 變更或認可該提交。 有時候提交可能會處於錯誤狀態，而無法繼續提交過程。 若發生這種情形，您必須刪除提交並建立新的提交。

> [!IMPORTANT]
> 您無法使用此 API 來發佈 [透過商務用 Microsoft Store 和教育用 Microsoft Store 大量購買](../publish/organizational-licensing.md) 的提交或直接向企業發佈 [LOB 應用程式](../publish/distribute-lob-apps-to-enterprises.md) 的提交。 針對這兩種情況，您必須使用合作夥伴中心來發佈提交。


<span id="methods-for-app-submissions" />

## <a name="methods-for-managing-app-submissions"></a>管理應用程式提交的方法

使用下列方法取得、建立、更新、認可或刪除應用程式提交。 在您可以使用這些方法之前，應用程式必須已經存在於您的合作夥伴中心帳戶中，而且您必須先在合作夥伴中心中為應用程式建立一個提交。 如需詳細資訊，請參閱 [必要條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="get-an-app-submission.md">取得現有的應用程式提交</a></td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status</td>
<td align="left"><a href="get-status-for-an-app-submission.md">取得現有的應用程式提交狀態</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions</td>
<td align="left"><a href="create-an-app-submission.md">建立新的應用程式提交</a></td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="update-an-app-submission.md">更新現有的應用程式提交</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit</td>
<td align="left"><a href="commit-an-app-submission.md">認可全新或更新的應用程式提交</a></td>
</tr>
<tr>
<td align="left">刪除</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}</td>
<td align="left"><a href="delete-an-app-submission.md">刪除應用程式提交</a></td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">

### <a name="create-an-app-submission"></a>建立應用程式提交

若要建立應用程式的提交，請遵循此程序。

1. 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
    > [!NOTE]
    > 請確定應用程式至少有一個已完成[年齡分級](../publish/age-ratings.md)資訊的完整提交。

2. [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Microsoft Store 提交 API 中的方法。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。

3. 在「Microsoft Store 提交 API」中執行下列方法來[建立應用程式提交](create-an-app-submission.md)。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
    ```

    回應主體包含提交新提交識別碼的 [應用程式提交](#app-submission-object) 資源、共用存取簽章 (SAS) URI，以便上傳至 Azure Blob 儲存體 (例如應用程式套件、列出影像和結尾檔案，以及新提交) 的所有資料，例如清單和定價資訊 (。

    > [!NOTE]
    > SAS URI 提供 Azure 儲存體中安全資源的存取權，完全不需要帳戶金鑰。 如需有關 SAS Uri 及其搭配 Azure Blob 儲存體使用的背景資訊，請參閱 [共用存取簽章，第1部分：瞭解 sas 模型](/azure/storage/common/storage-sas-overview) 和 [共用存取簽章，第2部分：透過 Blob 儲存體建立和使用 sas](/azure/storage/common/storage-sas-overview)。

4. 如果您要新增提交的新套件、清單影像或預告片檔案，請[準備應用程式套件](../publish/app-package-requirements.md)並[準備應用程式螢幕擷取畫面、影像與預告片](../publish/app-screenshots-and-images.md)。 將所有這些檔案新增到 ZIP 封存。

5. 以新提交的任何所需變更來修訂[應用程式提交](#app-submission-object)資料，然後執行下列方法來[更新應用程式提交](update-an-app-submission.md)。

    ```json
    PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
    ```
      > [!NOTE]
      > 如果您要新增提交的新檔案，確定您會更新提交資料，以參考 ZIP 封存中的名稱和這些檔案的相對路徑。

4. 如果您要新增套件、列出影像或結尾檔案以進行提交，請使用您稍早呼叫之 POST 方法的回應主體中提供的 SAS URI，將 ZIP 封存檔上傳至 [Azure Blob 儲存體](/azure/storage/storage-introduction#blob-storage) 。 您可在各種不同的平台上使用不同的 Azure Libraries 來執行，包括：

    * [適用於 .NET 的 Azure 儲存體用戶端程式庫](/azure/storage/storage-dotnet-how-to-use-blobs)
    * [Azure Storage SDK for Java](/azure/storage/storage-java-how-to-use-blob-storage)
    * [適用于 Python 的 Azure 儲存體 SDK](/azure/storage/storage-python-how-to-use-blob-storage)

    下列 c # 程式碼範例示範如何使用適用于 .NET 的 Azure 儲存體用戶端程式庫中的 [>cloudblockblob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob) 類別，將 ZIP 封存上傳至 Azure Blob 儲存體。 此範例假設已將 ZIP 封存寫入串流物件。

    ```csharp
    string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
    Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
        new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
    await blockBob.UploadFromStreamAsync(stream);
    ```

5. 執行下列方法來[認可應用程式提交](commit-an-app-submission.md)。 這會警示合作夥伴中心您已完成提交，而且您的更新現在應該套用至您的帳戶。

    ```json
    POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
    ```

6. 執行下列方法來[取得應用程式提交狀態](get-status-for-an-app-submission.md)，以檢查認可狀態。

    ```json
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    若要確認提交狀態，請檢閱回應主體中的 *「狀態」* 值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到 Microsoft Store 以供擷取。 您可以使用先前的方法或造訪合作夥伴中心，繼續監視提交進度。

<span id="manage-gradual-package-rollout">

## <a name="methods-for-managing-a-gradual-package-rollout"></a>管理漸進式套件推出的方法

您可以將應用程式提交中的已更新套件以漸進方式推出給 Windows 10 上的一部分應用程式客戶。 這可讓您監視特定套件的意見反應和分析資料，以確保您在更廣泛地推出更新之前，就已經信心滿滿。 您可以變更所發佈之提交的推出百分比 (或停止更新)，而不需建立新的提交。 如需詳細資訊，包括如何在合作夥伴中心中啟用和管理漸進式套件推出的指示，請參閱 [這篇文章](../publish/gradual-package-rollout.md)。

若要以程式設計方式啟用和管理應用程式提交的漸進式套件推出，請使用 Microsoft Store 提交 API 中的方法，遵照此程序執行。

  1. [建立應用程式提交](create-an-app-submission.md)或[取得現有的應用程式提交](get-an-app-submission.md)。
  2. 在回應資料中，找出 [packageRollout](#package-rollout-object) 資源、將 [ *isPackageRollout* ] 欄位設定為 [ **true**]，然後將 [ *packageRolloutPercentage* ] 欄位設定為應取得更新套件的應用程式客戶百分比。
  3. 將已更新的應用程式提交資料傳遞給[更新應用程式提交](update-an-app-submission.md)方法。

在為應用程式提交啟用漸進式套件推出之後，您可使用下列方法，以程式設計方式取得、更新、中斷或完成漸進式推出。

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout</td>
<td align="left"><a href="get-package-rollout-info-for-an-app-submission.md">取得應用程式提交的漸進式推出資訊</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage</td>
<td align="left"><a href="update-the-package-rollout-percentage-for-an-app-submission.md">更新應用程式提交的漸進式推出百分比</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout</td>
<td align="left"><a href="halt-the-package-rollout-for-an-app-submission.md">中斷應用程式提交的漸進式推出</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout</td>
<td align="left"><a href="finalize-the-package-rollout-for-an-app-submission.md">完成應用程式提交的漸進式推出</a></td>
</tr>
</tbody>
</table>


## <a name="code-examples-for-managing-app-submissions"></a>管理應用程式提交的程式碼範例

下列文章提供詳細的程式碼範例，示範如何以多個不同的程式設計語言來建立應用程式提交：

* [C# 範例：提交應用程式、附加元件與正式發行前小眾測試版](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C# 範例：提交含遊戲選項與預告片的應用程式](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Java 範例：提交應用程式、附加元件與正式發行前小眾測試版](java-code-examples-for-the-windows-store-submission-api.md)
* [Java 範例：提交含遊戲選項與預告片的應用程式](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Python 範例：提交應用程式、附加元件與正式發行前小眾測試版](python-code-examples-for-the-windows-store-submission-api.md)
* [Python 範例：提交含遊戲選項與預告片的應用程式](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>StoreBroker PowerShell 模組

另一種直接呼叫 Microsoft Store 提交 API 的方法，我們也提供了開放原始碼 PowerShell 模組，該模組在 API 上方實作命令列介面。 這個模組稱為 [StoreBroker](https://github.com/Microsoft/StoreBroker)。 您可以使用此模組從命令列來管理您的應用程式、正式發行前小眾測試版和附加元件提交，而不是直接直接呼叫 Microsoft Store 提交 API，也可以簡單瀏覽來源以檢視有關如何呼叫此 API 的更多範例。 StoreBroker 模組在 Microsoft 中積極地被用作為將眾多第一方應用程式提交至 Microsoft Store 的主要方式。

如需詳細資訊，請查看我們[在 GitHub 上的 StoreBroker 頁面](https://github.com/Microsoft/StoreBroker)。

<span/>

## <a name="data-resources"></a>資料資源
用於管理 App 提交的 Microsoft Store 提交 API 方法，使用下列 JSON 資料資源。

<span id="app-submission-object" />

### <a name="app-submission-resource"></a>應用程式提交資源

此資源描述應用程式提交。

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2",
    "isAdvancedPricingModel": true
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [
          "epub"
        ],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [
          "Free ebook reader"
        ],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "description": "Main page",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1"
        }
      }
    }
  },
  "hardwarePreferences": [
    "Touch"
  ],
  "automaticBackupEnabled": false,
  "canInstallOnRemovableMedia": true,
  "isGameDvrEnabled": false,
  "gamingOptions": [],
  "hasExternalInAppProducts": false,
  "meetAccessibilityGuidelines": true,
  "notesForCertification": "",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/387a9ea8-a412-43a9-8fb3-a38d03eb483d?sv=2014-02-14&sr=b&sig=sdd12JmoaT6BhvC%2BZUrwRweA%2Fkvj%2BEBCY09C2SZZowg%3D&se=2016-06-17T18:32:26Z&sp=rwl",
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "friendlyName": "Submission 2",
  "trailers": []
}
```

此資源具有下列值。

| 值      | 類型   | 描述      |
|------------|--------|-------------------|
| id            | 字串  | 提交的識別碼。 此識別碼可用於要求 [建立 App 提交](create-an-app-submission.md)、 [取得所有 App](get-all-apps.md) 和 [取得 App](get-an-app.md) 的回應資料中。 針對合作夥伴中心中建立的提交，此識別碼也可在 [提交] 頁面的 URL 中找到合作夥伴中心。  |
| applicationCategory           | 字串  |   指定 App [類別和/或子類別](../publish/category-and-subcategory-table.md)的字串。 類別與子類別會使用底線 '_' 字元結合為單一字串，例如 **BooksAndReference_EReader**。      |  
| 定價           |  物件 (object)  | [定價資源](#pricing-object)包含應用程式的定價資訊。        |   
| 可見性           |  字串  |  App 的可見度。 這個值可以是下列其中一個值： <ul><li>Hidden</li><li>公開</li><li>私人</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字串  | 提交的發佈模式。 這個值可以是下列其中一個值： <ul><li>立即</li><li>手動</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |  
| listings           |   物件 (object)  |  索引鍵/值組的字典，其中每個索引鍵都是國家/地區代碼，而每個值都是[清單資源](#listing-object)，其中包含應用程式的清單資訊。       |   
| hardwarePreferences           |  array  |   定義 App [硬體喜好設定](../publish/enter-app-properties.md)的字串陣列。 這個值可以是下列其中一個值： <ul><li>觸控</li><li>鍵盤</li><li>滑鼠</li><li>相機</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   指出 Windows 是否可以在自動備份至 OneDrive 時包含您 App 的資料。 如需詳細資訊，請參閱[應用程式宣告](../publish/product-declarations.md)。   |   
| canInstallOnRemovableMedia           |  boolean  |   指出客戶是否可以將您的 App 安裝到抽取式存放裝置。 如需詳細資訊，請參閱[應用程式宣告](../publish/product-declarations.md)。     |   
| isGameDvrEnabled           |  boolean |   指出是否已針對 App 啟用遊戲 DVR。    |   
| gamingOptions           |  array |   包含一個[遊戲選項資源](#gaming-options-object)的陣列，其定義此 App 的遊戲相關設定。     |   
| hasExternalInAppProducts           |     boolean          |   指出您的 App 是否允許使用者在 Microsoft Store 商務系統外部進行購買。 如需詳細資訊，請參閱[應用程式宣告](../publish/product-declarations.md)。     |   
| meetAccessibilityGuidelines           |    boolean           |  指出您的 App 是否已經過測試，符合協助工具指導方針。 如需詳細資訊，請參閱[應用程式宣告](../publish/product-declarations.md)。      |   
| notesForCertification           |  字串  |   包含您 App 的[認證注意事項](../publish/notes-for-certification.md)。    |    
| status           |   字串  |  提交的狀態。 這個值可以是下列其中一個值： <ul><li>無</li><li>已取消</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>發佈</li><li>已發行</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>認證</li><li>CertificationFailed</li><li>版本</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   物件 (object)  | [狀態詳細資料資源](#status-details-object)包含其他有關提交狀態的詳細資料，包括任何錯誤的資訊。       |    
| fileUploadUrl           |   字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件、清單影像或預告片檔案，請將包含套件和影像的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立應用程式提交](#create-an-app-submission)。       |    
| applicationPackages           |   array  | [應用程式套件資源](#application-package-object)的陣列，其提供關於提交中每個套件的詳細資料。 |    
| packageDeliveryOptions    | 物件 (object)  | [套件交付選項資源](#package-delivery-options-object)包含提交的漸進式套件推出和強制更新設定。  |
| enterpriseLicensing           |  字串  |  其中一個[企業授權值](#enterprise-licensing)，可指出應用程式適用的企業授權行為。  |    
| allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  指出是否允許 Microsoft [讓 App 可供未來的 Windows 10 裝置系列使用](../publish/set-app-pricing-and-availability.md)。    |    
| allowTargetFutureDeviceFamilies           | 物件 (object)   |  索引鍵/值組的字典，其中每個索引鍵都是一個 [Windows 10 裝置系列](../publish/set-app-pricing-and-availability.md)，而每個值都是一個布林值，可指出您的應用程式是否允許將目標設為指定的裝置系列。     |    
| friendlyName           |   字串  |  提交的易記名稱，如合作夥伴中心所示。 當您建立提交時，也會為您產生此值。       |  
| trailers           |  array |   包含高達 15 個 [預告片資源](#trailer-object) 陣列，代表應用程式清單的視訊預告片。<br/><br/>   |  


<span id="pricing-object" />

### <a name="pricing-resource"></a>定價資源

此資源包含應用程式的定價資訊。 此資源具有下列值。

| 值           | 類型    | Description        |
|-----------------|---------|------|
|  trialPeriod               |    字串     |  可針對應用程式指定試用期的字串。 這個值可以是下列其中一個值： <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    物件 (object)     |  索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的應用程式在特定市場中的自訂價格](../publish/define-market-selection.md)。 這個字典中的任何項目都會覆寫特定市場的 *priceId* 值所指定的基本價格。      |     
|  sales               |   array      |  已 **淘汰**。 包含應用程式的銷售資訊的[銷售資源](#sale-object)陣列。   |     
|  priceId               |   字串      |  指定應用程式[基本價格](../publish/define-market-selection.md)的[價格區間](#price-tiers)。   |     
|  isAdvancedPricingModel               |   boolean      |  若為 **true**，您的開發人員帳戶可以存取從 .99 美元到 1999.99 美元的展開價格區間。 若為 **false**，您的開發人員帳戶可以存取從 .99 美元到 999.99 美元的原始價格區間。 如需不同區間的詳細資訊，請參閱[價格區間](#price-tiers)。<br/><br/> &nbsp; 注意 &nbsp;此欄位是唯讀的。   |


<span id="sale-object" />

### <a name="sale-resource"></a>銷售資源

此資源包含應用程式的銷售資訊。

> [!IMPORTANT]
> **銷售** 資源不再支援，目前您無法使用 Microsoft Store 提交 API 取得或修改應用程式提交的銷售資料。 我們將來會更新「Microsoft Store 提交 API」來導入新的方法，以程式設計方式存取應用程式提交的銷售資訊。
>    * 在呼叫 [GET 方法以取得應用程式提交](get-an-app-submission.md)之後，*sales* 值將會空白。 您可以繼續使用合作夥伴中心取得提交應用程式的銷售資料。
>    * 呼叫 [PUT 方法以更新應用程式提交](update-an-app-submission.md)時，會忽略 *sales* 值中的資訊。 您可以繼續使用合作夥伴中心來變更提交應用程式的銷售資料。

此資源具有下列值。

| 值           | 類型    | 描述    |
|-----------------|---------|------|
|  NAME               |    字串     |   銷售的名稱。    |     
|  basePriceId               |   字串      |  用於銷售基本價格的[價格區間](#price-tiers)。    |     
|  startDate               |   字串      |   ISO 8601 格式的銷售開始日期。  |     
|  endDate               |   字串      |  ISO 8601 格式的銷售結束日期。      |     
|  marketSpecificPricings               |   物件 (object)      |   索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的應用程式在特定市場中的自訂價格](../publish/define-market-selection.md)。 這個字典中的任何項目都會覆寫特定市場的 *basePriceId* 值所指定的基本價格。    |


<span id="listing-object" />

### <a name="listing-resource"></a>清單資源

此資源包含應用程式的清單資訊。 此資源具有下列值。

| 值           | 類型    | Description                  |
|-----------------|---------|------|
|  baseListing               |   物件 (object)      |  應用程式的[基本清單](#base-listing-object)資訊，這會定義適用於所有平台的預設清單資訊。   |     
|  platformOverrides               | 物件 (object) |   索引鍵/值組的字典，其中每個索引鍵都是字串，可識別要覆寫清單資訊的平台，而每個值都是[清單](#base-listing-object)資源 (只包含從 description 到 title 的值)，可指定要針對指定平台進行覆寫的清單資訊。 索引鍵可以具有下列值： <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |

<span id="base-listing-object" />

### <a name="base-listing-resource"></a>基本清單資源

此資源包含應用程式的基本清單資訊。 此資源具有下列值。

| 值           | 類型    | Description       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   字串      |  選擇性的[著作權及/或商標資訊](../publish/create-app-store-listings.md)。  |
|  關鍵字                |  array       |  [關鍵字](../publish/create-app-store-listings.md)陣列，可協助讓您的應用程式出現在搜尋結果中。    |
|  licenseTerms                |    字串     | 適用於您應用程式的選擇性[授權條款](../publish/create-app-store-listings.md)。     |
|  privacyPolicy                |   字串      |   這個值已經過時。 若要設定或變更應用程式的隱私權原則 URL，您必須在合作夥伴中心的 [ [屬性](../publish/enter-app-properties.md#privacy-policy-url) ] 頁面上進行這項操作。 您可以在呼叫提交 API 時省略這個值。 如果您設定這個值，將會忽略它。       |
|  supportContact                |   字串      |  這個值已經過時。 若要設定或變更您的應用程式的支援連絡人 URL 或電子郵件地址，您必須在合作夥伴中心的 [  [屬性](../publish/enter-app-properties.md#support-contact-info) ] 頁面上進行此動作。 您可以在呼叫提交 API 時省略這個值。 如果您設定這個值，將會忽略它。        |
|  websiteUrl                |   字串      |  這個值已經過時。 若要設定或變更應用程式網頁的 URL，您必須在合作夥伴中心的 [  [屬性](../publish/enter-app-properties.md#website) ] 頁面上進行這項操作。 您可以在呼叫提交 API 時省略這個值。 如果您設定這個值，將會忽略它。      |    
|  description               |    字串     |   應用程式清單的[描述](../publish/create-app-store-listings.md)。   |     
|  特性               |    array     |  此陣列最多包含 20 個字串，可列出您應用程式的[功能](../publish/create-app-store-listings.md)。     |
|  releaseNotes               |  字串       |  適用於您應用程式的[版本資訊](../publish/create-app-store-listings.md)。    |
|  images               |   array      |  應用程式清單的[影像和圖示](#image-object)資源陣列。  |
|  recommendedHardware               |   array      |  此陣列最多包含 11 個字串，可為您的應用程式列出[建議的硬體設定](../publish/create-app-store-listings.md#additional-information)。     |
|  minimumHardware               |     字串    |  此陣列最多包含 11 個字串，可為您的應用程式列出[最低硬體設定](../publish/create-app-store-listings.md#additional-information)。    |  
|  title               |     字串    |   應用程式清單的標題。   |  
|  shortDescription               |     字串    |  僅用於遊戲。 此描述會出現在遊戲中樞 Xbox One 的 [ **資訊** ] 區段中，並協助客戶深入瞭解您的遊戲。   |  
|  shortTitle               |     字串    |  產品名稱的縮寫版本。 若有提供，這個較短名稱可能會出現在 Xbox One 上的各種位置 (在安裝期間、在成就中等) 來取代您產品的完整標題。    |  
|  sortTitle               |     字串    |   如果您的產品可能以不同的方式依字母順序排列，則可以在此處輸入另一個版本。 這可協助客戶在尋找時能更快地找到產品。    |  
|  voiceTitle               |     字串    |   您產品的替代名稱，若有提供，在 Xbox One 上使用 Kinect 或頭戴式裝置時，此名稱會用於音訊體驗。    |  
|  devStudio               |     字串    |   如果您想要在清單中包含 **Developed by** 欄位，請指定這個值。 (**Published by** 欄位會列出與您帳戶相關聯的發行者顯示名稱，無論您是否提供 *devStudio* 值。)    |  

<span id="image-object" />

### <a name="image-resource"></a>影像資源

此資源包含應用程式清單的影像和圖示資料。 如需可用於應用程式列出之影像和圖示的詳細資訊，請參閱 [應用程式螢幕擷取畫面與影像](../publish/app-screenshots-and-images.md)。 此資源具有下列值。

| 值           | 類型    | Description           |
|-----------------|---------|------|
|  fileName               |    字串     |   影像檔的名稱，位於您針對提交所上傳的 ZIP封存中。    |     
|  fileStatus               |   字串      |  影像檔的狀態。 這個值可以是下列其中一個值： <ul><li>無</li><li>PendingUpload</li><li>已上傳</li><li>PendingDelete</li></ul>   |
|  id  |  字串  | 影像的識別碼。 此值是由合作夥伴中心提供。  |
|  description  |  字串  | 影像的描述。  |
|  imageType  |  字串  | 指出影像的類型。 以下是目前支援的字串。 <p/>[螢幕擷取畫面影像](../publish/app-screenshots-and-images.md#screenshots)： <ul><li>Screenshot (此值適用於桌面螢幕擷取畫面)</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul><p/>[商店標誌](../publish/app-screenshots-and-images.md#store-logos)：<ul><li>StoreLogo9x16 </li><li>StoreLogoSquare</li><li>Icon (此值適用於 1:1 300 x 300 像素標誌)</li></ul><p/>[促銷映射](../publish/app-screenshots-and-images.md#promotional-images)： <ul><li>PromotionalArt16x9</li><li>PromotionalArtwork2400X1200</li></ul><p/>[Xbox 映射](../publish/app-screenshots-and-images.md#xbox-images)： <ul><li>XboxBrandedKeyArt</li><li>XboxTitledHeroArt</li><li>XboxFeaturedPromotionalArt</li></ul><p/>[選用宣傳影像](../publish/app-screenshots-and-images.md#optional-promotional-images)： <ul><li>SquareIcon358X358</li><li>BackgroundImage1000X800</li><li>PromotionalArtwork414X180</li></ul><p/> <!-- The following strings are also recognized for this field, but they correspond to image types that are no longer for listings in the Store.<ul><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>WideIcon358X173</li><li>Unknown</li></ul> -->   |


<span id="gaming-options-object" />

### <a name="gaming-options-resource"></a>遊戲選項資源

此資源包含應用程式的遊戲相關設定。 此資源中的值會對應至合作夥伴中心中提交的 [遊戲設定](../publish/enter-app-properties.md#game-settings) 。

```json
{
  "gamingOptions": [
    {
      "genres": [
        "Games_ActionAndAdventure",
        "Games_Casino"
      ],
      "isLocalMultiplayer": true,
      "isLocalCooperative": true,
      "isOnlineMultiplayer": false,
      "isOnlineCooperative": false,
      "localMultiplayerMinPlayers": 2,
      "localMultiplayerMaxPlayers": 12,
      "localCooperativeMinPlayers": 2,
      "localCooperativeMaxPlayers": 12,
      "isBroadcastingPrivilegeGranted": true,
      "isCrossPlayEnabled": false,
      "kinectDataForExternal": "Enabled"
    }
  ],
}
```

此資源具有下列值。

| 值           | 類型    | Description        |
|-----------------|---------|------|
|  內容類型               |    array     |  一或多個下列字串的陣列，描述遊戲的類型： <ul><li>Games_ActionAndAdventure</li><li>Games_CardAndBoard</li><li>Games_Casino</li><li>Games_Educational</li><li>Games_FamilyAndKids</li><li>Games_Fighting</li><li>Games_Music</li><li>Games_Platformer</li><li>Games_PuzzleAndTrivia</li><li>Games_RacingAndFlying</li><li>Games_RolePlaying</li><li>Games_Shooter</li><li>Games_Simulation</li><li>Games_Sports</li><li>Games_Strategy</li><li>Games_Word</li></ul>    |
|  isLocalMultiplayer               |    boolean     |  指出遊戲是否支援本機多人遊戲。      |     
|  isLocalCooperative               |   boolean      |  指出遊戲是否支援本機合作。    |     
|  isOnlineMultiplayer               |   boolean      |  指出遊戲是否支援線上多人遊戲。    |     
|  isOnlineCooperative               |   boolean      |  指出遊戲是否支援線上合作。    |     
|  localMultiplayerMinPlayers               |   int      |   指定遊戲支援本機多人遊戲的最少玩家數。   |     
|  localMultiplayerMaxPlayers               |   int      |   指定遊戲支援本機多人遊戲的最多玩家數。  |     
|  localCooperativeMinPlayers               |   int      |   指定遊戲支援本機合作的最少玩家數。  |     
|  localCooperativeMaxPlayers               |   int      |   指定遊戲支援本機合作的最多玩家數。  |     
|  isBroadcastingPrivilegeGranted               |   boolean      |  指出遊戲是否支援廣播。   |     
|  isCrossPlayEnabled               |   boolean      |   指出遊戲是否支援在 Windows 10 電腦和 Xbox 之間的多人工作階段。  |     
|  kinectDataForExternal               |   字串      |  下列其中一個值，指出遊戲是否可以收集 Kinect 資料並將其傳送到外部服務： <ul><li>NotSet</li><li>Unknown</li><li>啟用</li><li>停用</li></ul>   |

> [!NOTE]
> 在 Microsoft Store 提交 API 首次向開發人員發佈後，2017 年 5 月新增 *gamingOptions* 資源。 如果您在引進此資源前透過提交 API 為應用程式建立了提交，並且此提交仍在進行中，則在確認成功提交或刪除提交之前，此資源將不適用於應用程式的提交。 如果 *gamingOptions* 資源不適用於應用程式的提交，則由 [取得應用程式](get-an-app.md) 方法傳回的 [應用程式資源](get-app-data.md#application_object) 的 *hasAdvancedListingPermission* 欄位為 false。

<span id="status-details-object" />

### <a name="status-details-resource"></a>狀態詳細資料資源

此資源包含關於提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | Description         |
|-----------------|---------|------|
|  錯誤               |    物件 (object)     |   包含提交的錯誤詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。    |     
|  warnings               |   物件 (object)      | 包含提交的警告詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。      |
|  certificationReports               |     物件 (object)    |   提供提交認證報告資料存取的[認證報告資源](#certification-report-object)陣列。 如果認證失敗，您可以檢查這些報告來取得詳細資訊。   |  


<span id="status-detail-object" />

### <a name="status-detail-resource"></a>狀態詳細資料資源

此資源包含有關提交的任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 說明        |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的[提交狀態碼](#submission-status-code)。   |     
|  詳細資料               |     字串    |  含有更多關於問題之詳細資料的訊息。     |


<span id="application-package-object" />

### <a name="application-package-resource"></a>應用程式套件資源

此資源包含有關提交的應用程式套件的相關詳細資料。

```json
{
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "Uploaded",
      "id": "1152921504620138797",
      "version": "1.0.0.0",
      "architecture": "ARM",
      "languages": [
        "en-US"
      ],
      "capabilities": [
        "ID_RESOLUTION_HD720P",
        "ID_RESOLUTION_WVGA",
        "ID_RESOLUTION_WXGA"
      ],
      "minimumDirectXVersion": "None",
      "minimumSystemRam": "None",
      "targetDeviceFamilies": [
        "Windows.Mobile min version 10.0.10240.0"
      ]
    }
  ],
}
```

此資源具有下列值。  

> [!NOTE]
> 在呼叫 [更新應用程式提交](update-an-app-submission.md)方法時，要求主體中只需要這個物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 及 *minimumSystemRam* 值。 其他值則由合作夥伴中心填入。

| 值           | 類型    | Description                   |
|-----------------|---------|------|
| fileName   |   字串      |  封裝的名稱。    |  
| fileStatus    | 字串    |  套件的狀態。 這個值可以是下列其中一個值： <ul><li>無</li><li>PendingUpload</li><li>已上傳</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  唯一識別套件的識別碼。 此值是由合作夥伴中心提供。   |     
| version    |  字串   |  應用程式套件的版本。 如需詳細資訊，請參閱[套件版本編號](../publish/package-version-numbering.md)。   |   
| 架構    |  字串   |  套件的架構 (例如，ARM)。   |     
| 語言    | array    |  應用程式所支援之語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](../publish/supported-languages.md)。    |     
| capabilities    |  array   |  套件所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能宣告](../packaging/app-capability-declarations.md)。   |     
| minimumDirectXVersion    |  字串   |  應用程式套件所支援的最低 DirectX 版本。 只能針對 Windows 8.x 的應用程式作設定。 對於以其他 OS 版本為目標的應用程式，在呼叫[更新的應用程式提交](update-an-app-submission.md) 方法時，這個值是必要的，但您指定的值將被忽略。 這個值可以是下列其中一個值： <ul><li>無</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  應用程式套件所需的最小 RAM。 只能針對 Windows 8.x 的應用程式作設定。 對於以其他 OS 版本為目標的應用程式，在呼叫[更新的應用程式提交](update-an-app-submission.md) 方法時，這個值是必要的，但您指定的值將被忽略。 這個值可以是下列其中一個值： <ul><li>無</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | array    |  代表套件目標裝置系列的字串陣列。 這個值只適用於目標為 Windows 10 的套件；對於目標為較舊版本的套件，這個值是 **None** 值。 以下是 Windows 10 套件目前支援的裝置系列字串，其中 *{0}* 是 Windows 10 版本字串，例如10.0.10240.0、10.0.10586.0 或10.0.14393.0： <ul><li>Windows 通用最小版本 *{0}*</li><li>Windows 桌面最小版本 *{0}*</li><li>Windows Mobile 最小版本 *{0}*</li><li>Windows. Xbox min 版本 *{0}*</li><li>Windows. 全像版本的最小值 *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />

### <a name="certification-report-resource"></a>認證報告資源

此資源提供提交認證報告資料的存取。 此資源具有下列值。

| 值           | 類型    | Description             |
|-----------------|---------|------|
|     date            |    字串     |  產生報表的日期和時間，格式為 ISO 8601。    |
|     reportUrl            |    字串     |  您可以存取報告的 URL。    |


<span id="package-delivery-options-object" />

### <a name="package-delivery-options-resource"></a>套件交付選項資源

此資源包含提交的漸進式套件推出和強制更新設定。

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

| 值           | 類型    | Description        |
|-----------------|---------|------|
| packageRollout   |   物件 (object)      |  [套件推出資源](#package-rollout-object)包含用於提交的漸進式套件推出設定。   |  
| isMandatoryUpdate    | boolean    |  指出您是否要將這項提交中的套件視為自我安裝應用程式更新的強制項目。 如需有關自我安裝應用程式更新的強制套件詳細資訊，請參閱[下載與安裝應用程式的套件更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  date   |  這項提交中的套件變成強制項目的日期和時間，採用 ISO 8601 格式和 UTC 時區。   |        

<span id="package-rollout-object" />

### <a name="package-rollout-resource"></a>套件推出資源

此資源包含提交的漸進式[套件推出設定](#manage-gradual-package-rollout)。 此資源具有下列值。

| 值           | 類型    | Description        |
|-----------------|---------|------|
| isPackageRollout   |   boolean      |  指出是否已為提交啟用漸進式套件推出。    |  
| packageRolloutPercentage    | FLOAT    |  將接收漸進式推出中套件的使用者百分比。    |  
| packageRolloutStatus    |  字串   |  下列其中一個字串，這些字串指出漸進式套件推出的狀態： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字串   |  未取得漸進式推出套件的客戶將收到的提交識別碼。   |          

> [!NOTE]
> *PackageRolloutStatus* 和 *fallbackSubmissionId* 值是由合作夥伴中心指派，且不適合由開發人員設定。 如果您將這些值包含在要求本文中，則會忽略這些值。

<span id="trailer-object" />

### <a name="trailers-resource"></a>預告片資源

此資源代表應用程式清單的視訊預告片。 此資源中的值會對應至合作夥伴中心中提交的 [尾端](../publish/app-screenshots-and-images.md#trailers) 選項。

您可新增高達 15 個預告片資源至 [App 提交資源](#app-submission-object)中的 *trailers* 陣列。 若要上傳提交的預告片影片檔案和縮圖影像，請將這些檔案新增至包含提交的套件和清單影像的同一個 ZIP 封存中，然後將此 ZIP 封存上傳到提交的共用存取簽章 (SAS) URI。 如需有關將 ZIP 封存上傳到 SAS URI 的詳細資訊，請參閱 [建立應用程式提交](#create-an-app-submission)。

```json
{
  "trailers": [
    {
      "id": "1158943556954955699",
      "videoFileName": "Trailers\\ContosoGameTrailer.mp4",
      "videoFileId": "1159761554639123258",
      "trailerAssets": {
        "en-us": {
          "title": "Contoso Game",
          "imageList": [
            {
              "fileName": "Images\\ContosoGame-Thumbnail.png",
              "id": "1155546904097346923",
              "description": "This is a still image from the video."
            }
          ]
        }
      }
    }
  ]
}
```

此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
|  id               |    字串     |   預告片的識別碼。 此值是由合作夥伴中心提供。   |
|  videoFileName               |    字串     |    ZIP 封存中預告片影片檔的名稱，包含提交的檔案。    |     
|  videoFileId               |   字串      |  預告片影片檔的識別碼。 此值是由合作夥伴中心提供。   |     
|  trailerAssets               |   物件 (object)      |  機碼和值組的字典，其中每個機碼都是語言代碼，而每個值都是[預告片資產資源](#trailer-assets-object)，其中包含預告片的其他地區設定資產。 如需有關支援的語言代碼的詳細資訊，請參閱 [支援的語言](../publish/supported-languages.md)。    |     

> [!NOTE]
> 在 Microsoft Store 提交 API 首次向開發人員發佈後，2017 年 5 月新增 *預告片* 資源。 如果您在引進此資源前透過提交 API 為應用程式建立了提交，並且此提交仍在進行中，則在確認成功提交或刪除提交之前，此資源將不適用於應用程式的提交。 如果 *預告片* 資源不適用於應用程式的提交，則由 [取得應用程式](get-an-app.md) 方法傳回的 [應用程式資源](get-app-data.md#application_object) 的 *hasAdvancedListingPermission* 欄位為 false。

<span id="trailer-assets-object" />

### <a name="trailer-assets-resource"></a>預告片資產資源

此資源包含[預告片資源](#trailer-object)中所定義之預告片的其他地區設定資產。 此資源具有下列值。

| 值           | 類型    | Description        |
|-----------------|---------|------|
| title   |   字串      |  當地語系化的預告片標題。 當使用者以全螢幕模式播放預告片時，會顯示此標題。     |  
| imageList    | array    |   陣列，其中包含提供預告片縮圖影像的[影像](#image-for-trailer-object)資源。 此陣列中只能包含一個[影像](#image-for-trailer-object)資源。  |   


<span id="image-for-trailer-object" />

### <a name="image-resource-for-a-trailer"></a>影像資源 (適用於預告片)

此資源描述預告片的縮圖影像。 此資源具有下列值。

| 值           | 類型    | Description           |
|-----------------|---------|------|
|  fileName               |    字串     |   縮圖影像檔的名稱，位於您針對提交所上傳的 ZIP 封存中。    |     
|  id  |  字串  | 縮圖影像的識別碼。 此值是由合作夥伴中心提供。  |
|  description  |  字串  | 縮圖影像的描述。 這個值僅是中繼資料，並不會對使用者顯示。   |

<span/>

## <a name="enums"></a>列舉

這些方法會使用下列列舉。

<span id="price-tiers" />

### <a name="price-tiers"></a>價格區間

下列值代表應用程式提交的[價格資源](#pricing-object)資源的可用價格區間。

| 值           | 描述        |
|-----------------|------|
|  基本               |   未設定價格區間；使用應用程式的基本價格。      |     
|  NotAvailable              |   指定的區域中無法使用此應用程式。    |     
|  免費              |   這個應用程式是免費的。    |    
|  第 *xxx* 層               |   指定應用程式的價格區間的字串，格式為 **第 <em>xxx</em> 層**。 目前支援下列價格區間範圍︰<br/><br/><ul><li>如果 [價格資源](#pricing-object) 的 *isAdvancedPricingModel* 值為 **true**，您帳戶的可用價格區間值是 **Tier1012** - **Tier1424**。</li><li>如果 [價格資源](#pricing-object) 的 *isAdvancedPricingModel* 值為 **false**，您帳戶的可用價格區間值是 **Tier2** - **Tier96**。</li></ul>若要查看可供您的開發人員帳戶使用的完整定價層資料表，包括與每個階層相關聯的市場特定價格，請移至合作夥伴中心中任何提交應用程式的 [**定價和可用性**] 頁面，然後按一下 [**市場] 和 [自訂價格**] 區段中的 [ **view table** ] 連結 (針對某些開發人員帳戶，此連結位於 [**定價**]) 區段中    |


<span id="enterprise-licensing" />

### <a name="enterprise-licensing-values"></a>企業授權值

下列值代表應用程式適用的組織授權行為。 如需這些選項的詳細資訊，請參閱[組織授權選項](../publish/organizational-licensing.md)。

> [!NOTE]
> 雖然您可以透過提交 API 為應用程式提交來設定組織授權選項，但您無法使用此 API 來發佈[透過商務用 Microsoft Store 和教育用 Microsoft Store 大量購買](../publish/organizational-licensing.md)提交。 若要將提交發佈到商務用 Microsoft Store 和教育用 Microsoft Store，您必須使用合作夥伴中心。


| 值           |  描述      |
|-----------------|---------------|
| 無            |     請勿利用 Microsoft Store 管理 (線上) 大量授權提供企業使用您的應用程式。         |     
| 線上        |     利用 Microsoft Store 管理 (線上) 大量授權提供企業使用您的應用程式。  |
| OnlineAndOffline | 利用 Microsoft Store 管理 (線上) 大量授權提供企業使用您的應用程式，以及透過中斷連線 (離線) 授權提供企業使用您的應用程式。 |


<span id="submission-status-code" />

### <a name="submission-status-code"></a>提交狀態碼

下列值代表提交的狀態碼。

| 值           |  描述      |
|-----------------|---------------|
| 無            |     未指定任何代碼。         |     
| InvalidArchive        |     包含該套件的 ZIP 封存無效，或具有無法辨識的封存格式。  |
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
| 其他  | 提交處於無法辨識或未分類的狀態。 |
| PackageValidationWarning | 套件驗證程序產生了警告。 |

<span/>

## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Microsoft Store 提交 API 取得應用程式資料](get-app-data.md)
* [合作夥伴中心中的應用程式提交](../publish/app-submissions.md)
