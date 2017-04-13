---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的應用程式管理提交。"
title: "管理 App 提交"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, Windows 市集提交 API, 應用程式提交"
ms.openlocfilehash: f6f0342619f91190bf021842c3ac3b0c61964d25
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-app-submissions"></a>管理 App 提交

Windows 市集提交 API 提供方法讓您使用於管理應用程式的提交，包括漸進式套件推出。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的必要條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 此權限是在各個階段中針對開發人員帳戶啟用，並非所有帳戶目前都啟用此權限。 若要要求早一點存取，請登入開發人員中心儀表板，按一下儀表板下方的 **\[意見反應\]**，選取意見反應區域的 **\[提交 API\]**，並提交您的要求。 當您的帳戶啟用此權限時，您會收到電子郵件。

>**重要**&nbsp;&nbsp;如果您使用 Windows 市集提交 API 以建立應用程式的提交，請確定僅使用 API 變更提交，而不是開發人員中心儀表板。 如果您使用儀表板變更最初使用 API 所建立的提交，您將無法再使用 API 變更或是認可該提交。 有時候提交可能會處於錯誤狀態，而無法繼續提交過程。 若發生這種情形，您必須刪除提交並建立新的提交。

<span id="methods-for-app-submissions" />
## <a name="methods-for-managing-app-submissions"></a>管理應用程式提交的方法

使用下列方法取得、建立、更新、認可或刪除應用程式提交。 使用這些方法之前，應用程式必須存在於您的開發人員中心帳戶，而且您必須先在儀表板中建立一個應用程式提交。 如需詳細資訊，請參閱[必要條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。

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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[取得現有的應用程式提交](get-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status```</td>
<td align="left">[取得現有的應用程式提交狀態](get-status-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions```</td>
<td align="left">[建立新的應用程式提交](create-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">PUT</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[更新現有的應用程式提交](update-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit```</td>
<td align="left">[認可全新或更新的應用程式提交](commit-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}```</td>
<td align="left">[刪除應用程式提交](delete-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span id="create-an-app-submission">
### 建立應用程式提交

若要建立應用程式的提交，請遵循此程序。

1. 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。

  >**注意**&nbsp;&nbsp;請確定應用程式至少有一個已完成[年齡分級](https://msdn.microsoft.com/windows/uwp/publish/age-ratings)資訊的完整提交。

2. [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Windows 市集提交 API 中的方法。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

3. 在「Windows 市集提交 API」中執行下列方法來[建立應用程式提交](create-an-app-submission.md)。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  回應主體包含三個項目︰新提交的識別碼、新提交的資料 (包含所有清單和定價資訊)，以及用於上傳提交至 Azure Blob 儲存體的任何應用程式套件和清單影像的共用存取簽章 (SAS) URI。

  >**注意**&nbsp;&nbsp;SAS URI 提供 Azure 儲存體中安全資源的存取權，完全不需要帳戶金鑰。 如需有關 SAS URI 及使用 Azure Blob 儲存體的背景資訊，請參閱[共用存取簽章，第 1 部分︰了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1)和[共用存取簽章，第 2 部分︰透過 Blob 儲存體來建立與使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

4. 如果您要新增提交的新套件或影像，請[準備應用程式套件](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)和[準備應用程式螢幕擷取畫面與影像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 將所有這些檔案新增到 ZIP 封存。

5. 以新提交的任何所需變更來修訂提交資料，然後執行下列方法來[更新應用程式提交](update-an-app-submission.md)。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  <span/>
  >**注意**&nbsp;&nbsp;如果您要為提交新增新的套件或影像，請確定將提交資料更新成參考 ZIP 封存中這些檔案的名稱和相對路徑。

4. 如果您要新增提交的新套件或影像，請使用您稍早呼叫之 POST 方法回應主體中提供的 SAS URI，將 ZIP 封存上傳至 [Azure Blob 儲存體](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage)。 您可在各種不同的平台上使用不同的 Azure Libraries 來執行，包括：

  * [.NET 適用的 Azure 儲存體用戶端程式庫](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-blobs)
  * [Java 適用的 Azure 儲存體 SDK](https://docs.microsoft.com/azure/storage/storage-java-how-to-use-blob-storage)
  * [Python 適用的 Azure 儲存體 SDK](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-blob-storage)

  <span/>

  下列 C# 程式碼範例示範如何在適用於 .NET 的 Azure 儲存體用戶端程式庫中，使用 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 類別上傳 ZIP 封存。 此範例假設已將 ZIP 封存寫入串流物件。

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";
  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 執行下列方法來[認可應用程式提交](commit-an-app-submission.md)。 這會向「開發人員中心」發出警示，指出您已完成提交，而現在應該將更新套用至您的帳戶。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. 執行下列方法來[取得應用程式提交狀態](get-status-for-an-app-submission.md)，以檢查認可狀態。

  > [!div class="tabbedCodeSnippets"]
  ``` syntax
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
  ```

  若要確認提交狀態，請檢閱回應主體中的*「狀態」*值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到市集以供擷取。 您可以繼續使用先前的方法，或瀏覽開發人員中心儀表板來監視提交進度。

<span/>
### <a name="code-examples-for-managing-app-submissions"></a>管理應用程式提交的程式碼範例

下列文章提供詳細的程式碼範例，示範如何以多個不同的程式設計語言來建立應用程式提交：

* [C# 程式碼範例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 程式碼範例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 程式碼範例](python-code-examples-for-the-windows-store-submission-api.md)

>**注意**&nbsp;&nbsp;：除了列示於上方的程式碼範例，我們也提供在 Windows 市集中提交 API 上方實作命令列介面的開放原始碼 PowerShell 模組。 這個模組稱為 [StoreBroker](https://aka.ms/storebroker)。 您可以從命令列使用此模組管理您的應用程式、正式發行前小眾測試版和附加元件提交，而無須直接呼叫 Windows 市集提交 API，或是您只需瀏覽來源即可查看更多的範例，了解如何呼叫此 API。 StoreBroker 模組在 Microsoft 中積極地被用作為將眾多第一方應用程式提交至市集的主要方式。 如需詳細資訊，請查看我們[在 GitHub 上的 StoreBroker 頁面](https://aka.ms/storebroker)。

<span id="manage-gradual-package-rollout">
## <a name="methods-for-managing-a-gradual-package-rollout"></a>管理漸進式套件推出的方法

您可以將應用程式提交中的已更新套件以漸進方式推出給 Windows10 上的一部分應用程式客戶。 這可讓您監視特定套件的意見反應和分析資料，以確保您對該項更新信心十足之後，再更廣泛地推出該項更新。 您可以變更所發佈之提交的推出百分比 (或停止更新)，而不需建立新的提交。 如需詳細資訊 (包括如何在「開發人員中心」儀表板中啟用和管理漸進式套件推出的指示)，請參閱[這篇文章](../publish/gradual-package-rollout.md)。

若要以程式設計方式啟用和管理應用程式提交的漸進式套件推出，請使用 Windows 市集提交 API 中的方法，遵照此程序執行。

  1. [建立應用程式提交](create-an-app-submission.md)或[取得現有的應用程式提交](get-an-app-submission.md)。
  2. 在回應資料中，找出 [packageRollout](#package-rollout-object) 資源，將 *isPackageRollout* 欄位設定為 **true**，然後將 *packageRolloutPercentage* 欄位設定為應該取得已更新套件的應用程式客戶百分比。
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
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout```</td>
<td align="left">[取得應用程式提交的漸進式推出資訊](get-package-rollout-info-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage```</td>
<td align="left">[更新應用程式提交的漸進式推出百分比](update-the-package-rollout-percentage-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout```</td>
<td align="left">[中斷應用程式提交的漸進式推出](halt-the-package-rollout-for-an-app-submission.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout```</td>
<td align="left">[完成應用程式提交的漸進式推出](finalize-the-package-rollout-for-an-app-submission.md)</td>
</tr>
</tbody>
</table>

<span/>
## 資料資源用於管理應用程式提交的 Windows 市集提交 API 方法，使用下列 JSON 資料資源。

<span id="app-submission-object" />
### 應用程式提交資源

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
  "friendlyName": "Submission 2"
}
```

此資源具有下列值。

| 值      | 類型   | 描述      |
|------------|--------|-------------------|
| id            | 字串  | 提交的識別碼。  |
| applicationCategory           | 字串  |   指定 App [類別和/或子類別](https://msdn.microsoft.com/windows/uwp/publish/category-and-subcategory-table)的字串。 類別與子類別會使用底線 '_' 字元結合為單一字串，例如 **BooksAndReference_EReader**。      |  
| pricing           |  物件  | [定價資源](#pricing-object)包含應用程式的定價資訊。        |   
| 可見度           |  字串  |  App 的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |  
| listings           |   物件  |  索引鍵/值組的字典，其中每個索引鍵都是國家/地區代碼，而每個值都是[清單資源](#listing-object)，其中包含應用程式的清單資訊。       |   
| hardwarePreferences           |  array  |   定義 App [硬體喜好設定](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)的字串陣列。 這可以是下列其中一個值： <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  布林值  |   指出 Windows 是否可以在自動備份至 OneDrive 時包含您 App 的資料。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  布林值  |   指出客戶是否可以將您的 App 安裝到抽取式存放裝置。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  布林值 |   指出是否已針對 App 啟用遊戲 DVR。    |   
| hasExternalInAppProducts           |     布林值          |   指出您的 App 是否允許使用者在 Windows 市集商務系統外部進行購買。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    布林值           |  指出您的 App 是否已經過測試，符合協助工具指導方針。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字串  |   包含您應用程式的[認證注意事項](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| status           |   字串  |  提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   物件  | [狀態詳細資料資源](#status-details-object)包含其他有關提交狀態的詳細資料，包括任何錯誤的資訊。       |    
| fileUploadUrl           |   字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件或影像，請將包含套件和影像的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立應用程式提交](#create-an-app-submission)。       |    
| applicationPackages           |   陣列  | [應用程式套件資源](#application-package-object)的陣列，其提供關於提交中每個套件的詳細資料。 |    
| packageDeliveryOptions    | 物件  | [套件交付選項資源](#package-delivery-options-object)包含提交的漸進式套件推出和強制更新設定。  |
| enterpriseLicensing           |  字串  |  其中一個[企業授權值](#enterprise-licensing)，可指出 App 適用的企業授權行為。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  布林值   |  指出是否允許 Microsoft [讓 App 可供未來的 Windows10 裝置系列使用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。    |    
| allowTargetFutureDeviceFamilies           | 物件   |  索引鍵/值組的字典，其中每個索引鍵都是一個 [Windows10 裝置系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)，而每個值都是一個布林值，可指出您的應用程式是否允許將目標設為指定的裝置系列。     |    
| friendlyName           |   字串  |  提交的易記名稱，如開發人員中心儀表板所示。 當您建立提交時，也會為您產生此值。       |  


<span id="listing-object" />
### <a name="listing-resource"></a>清單資源

此資源包含應用程式的清單資訊。 此資源具有下列值。

| 值           | 類型    | 描述                  |
|-----------------|---------|------|
|  baseListing               |   物件      |  應用程式的[基本清單](#base-listing-object)資訊，這會定義適用於所有平台的預設清單資訊。   |     
|  platformOverrides               | 物件 |   索引鍵/值組的字典，其中每個索引鍵都是字串，可識別要覆寫清單資訊的平台，而每個值都是[清單](#base-listing-object)資源 (只包含從 description 到 title 的值)，可指定要針對指定平台進行覆寫的清單資訊。 索引鍵可以具有下列值： <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### <a name="base-listing-resource"></a>基本清單資源

此資源包含應用程式的基本清單資訊。 此資源具有下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   字串      |  選擇性的[著作權及/或商標資訊](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info)。  |
|  keywords                |  陣列       |  [關鍵字](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords)陣列，可協助讓您的應用程式出現在搜尋結果中。    |
|  licenseTerms                |    字串     | 適用於您應用程式的選擇性[授權條款](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms)。     |
|  privacyPolicy                |   字串      |   適用於您應用程式的[隱私權原則](../publish/create-app-store-listings.md#privacy-policy) URL。    |
|  supportContact                |   字串      |  適用於您應用程式的[支援連絡資訊](../publish/create-app-store-listings.md#support-contact-info)的 URL 或電子郵件地址。     |
|  websiteUrl                |   字串      |  適用於您應用程式的[網頁](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) URL。    |    
|  description               |    字串     |   應用程式清單的[描述](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description)。   |     
|  features               |    陣列     |  此陣列最多包含 20 個字串，可列出您應用程式的[功能](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features)。     |
|  releaseNotes               |  字串       |  適用於您應用程式的[版本資訊](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes)。    |
|  images               |   陣列      |  應用程式清單的[影像和圖示](#image-object)資源陣列。  |
|  recommendedHardware               |   陣列      |  此陣列最多包含 11 個字串，可為您的應用程式列出[建議的硬體設定](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware)。     |
|  title               |     字串    |   應用程式清單的標題。   |  

<span id="image-object" />
### <a name="image-resource"></a>影像資源

此資源包含應用程式清單的影像和圖示資料。 如需可用於列出之影像和圖示的詳細資訊，請參閱 [應用程式螢幕擷取畫面與影像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 此資源具有下列值。

| 值           | 類型    | 描述           |
|-----------------|---------|------|
|  fileName               |    字串     |   影像檔的名稱，位於您針對提交所上傳的 ZIP封存中。    |     
|  fileStatus               |   字串      |  影像檔的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  字串  | 由開發人員中心指定的影像識別碼。  |
|  description  |  字串  | 影像的描述。  |
|  imageType  |  字串  | 其中一個下列字串，可指出影像類型： <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Icon</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### <a name="pricing-resource"></a>定價資源

此資源包含應用程式的定價資訊。 此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
|  trialPeriod               |    字串     |  可針對應用程式指定試用期的字串。 這可以是下列其中一個值： <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    物件     |  索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的應用程式在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 這個字典中的任何項目都會覆寫特定市場的 *priceId* 值所指定的基本價格。      |     
|  sales               |   陣列      |  **過時**。 包含應用程式的銷售資訊的[銷售資源](#sale-object)陣列。   |     
|  priceId               |   字串      |  指定應用程式[基本價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price)的[價格區間](#price-tiers)。   |     
|  isAdvancedPricingModel               |   布林值      |  若為 **true**，您的開發人員帳戶可以存取從 .99 美元到 1999.99 美元的展開價格區間。 若為 **false**，您的開發人員帳戶可以存取從 .99 美元到 999.99 美元的原始價格區間。 如需不同區間的詳細資訊，請參閱[價格區間](#price-tiers)。<br/><br/>**注意**&nbsp;&nbsp;此欄位為唯讀。   |


<span id="sale-object" />
### <a name="sale-resource"></a>銷售資源

此資源包含應用程式的銷售資訊。

>**重要**&nbsp;&nbsp;**銷售**資源不再支援，目前您無法使用 Windows 市集提交 API 取得或修改應用程式提交的銷售資料︰

   > * 在呼叫 [GET 方法以取得應用程式提交](get-an-app-submission.md)之後，*sales* 值將會空白。 您可以繼續使用「開發人員中心」儀表板來取得應用程式提交的銷售資料。
   > * 呼叫 [PUT 方法以更新應用程式提交](update-an-app-submission.md)時，會忽略 *sales* 值中的資訊。 您可以繼續使用「開發人員中心」儀表板來變更應用程式提交的銷售資料。

> 我們將來會更新「Windows 市集提交 API」來導入新的方法，以程式設計方式存取應用程式提交的銷售資訊。

此資源具有下列值。

| 值           | 類型    | 描述    |
|-----------------|---------|------|
|  name               |    字串     |   銷售的名稱。    |     
|  basePriceId               |   字串      |  用於銷售基本價格的[價格區間](#price-tiers)。    |     
|  startDate               |   字串      |   ISO 8601 格式的銷售開始日期。  |     
|  endDate               |   字串      |  ISO 8601 格式的銷售結束日期。      |     
|  marketSpecificPricings               |   物件      |   索引鍵/值組的字典，其中每個索引鍵都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的應用程式在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 這個字典中的任何項目都會覆寫特定市場的 *basePriceId* 值所指定的基本價格。    |


<span id="status-details-object" />
### <a name="status-details-resource"></a>狀態詳細資料資源

此資源包含關於提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | 描述         |
|-----------------|---------|------|
|  errors               |    物件     |   包含提交的錯誤詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。    |     
|  warnings               |   物件      | 包含提交的警告詳細資料的[狀態詳細資料資源](#status-detail-object)陣列。      |
|  certificationReports               |     物件    |   提供提交認證報告資料存取的[認證報告資源](#certification-report-object)陣列。 如果認證失敗，您可以檢查這些報告來取得詳細資訊。   |  


<span id="status-detail-object" />
### <a name="status-detail-resource"></a>狀態詳細資料資源

此資源包含有關提交的任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的[提交狀態碼](#submission-status-code)。   |     
|  details               |     字串    |  含有更多關於問題之詳細資料的訊息。     |


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

>**注意**&nbsp;&nbsp;在呼叫[更新應用程式提交](update-an-app-submission.md)方法時，要求主體中只需要這個物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 及 *minimumSystemRam* 值。 其他值均是由開發人員中心所填入。

| 值           | 類型    | 描述                   |
|-----------------|---------|------|
| fileName   |   字串      |  套件的名稱。    |  
| fileStatus    | 字串    |  套件的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  唯一識別套件的識別碼。 此值由開發人員中心所使用。   |     
| version    |  字串   |  應用程式套件的版本。 如需詳細資訊，請參閱[套件版本編號](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字串   |  套件的架構 (例如，ARM)。   |     
| languages    | 陣列    |  應用程式所支援之語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  陣列   |  套件所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能宣告](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字串   |  應用程式套件所支援的最低 DirectX 版本。 這只能針對目標為 Windows 8.x 的應用程式進行設定；對於目標為其他版本的應用程式則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  應用程式套件所需的最小 RAM。 這只能針對目標為 Windows 8.x 的應用程式進行設定；對於目標為其他版本的應用程式則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | 陣列    |  代表套件目標裝置系列的字串陣列。 這個值只適用於目標為 Windows10 的套件；對於目標為較舊版本的套件，這個值是 **None** 值。 目前針對 Windows10 套件支援下列的裝置系列字串，其中 *{0}* 是 Windows10 版本字串，例如 10.0.10240.0、10.0.10586.0 或 10.0.14393.0： <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### <a name="certification-report-resource"></a>認證報告資源

此資源提供提交認證報告資料的存取。 此資源具有下列值。

| 值           | 類型    | 描述             |
|-----------------|---------|------|
|     日期            |    字串     |  以 ISO 8601 格式產生報告的日期和時間。    |
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

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| packageRollout   |   物件      |  [套件推出資源](#package-rollout-object)包含用於提交的漸進式套件推出設定。   |  
| isMandatoryUpdate    | 布林值    |  指出您是否要將這項提交中的套件視為自我安裝應用程式更新的強制項目。 如需有關自我安裝應用程式更新的強制套件詳細資訊，請參閱[下載與安裝應用程式的套件更新](../packaging/self-install-package-updates.md)。    |  
| mandatoryUpdateEffectiveDate    |  日期   |  這項提交中的套件變成強制項目的日期和時間，採用 ISO 8601 格式和 UTC 時區。   |        

<span id="package-rollout-object" />
### <a name="package-rollout-resource"></a>套件推出資源

此資源包含提交的漸進式[套件推出設定](#manage-gradual-package-rollout)。 此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| isPackageRollout   |   布林值      |  指出是否已為提交啟用漸進式套件推出。    |  
| packageRolloutPercentage    | 浮點數    |  將接收漸進式推出中套件的使用者百分比。    |  
| packageRolloutStatus    |  字串   |  下列其中一個字串，這些字串指出漸進式套件推出的狀態： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字串   |  未取得漸進式推出套件的客戶將收到的提交識別碼。   |          

>**注意**&nbsp;&nbsp;*packageRolloutStatus* 和 *fallbackSubmissionId* 值是由開發人員中心指派，並不是供開發人員設定。 如果您將這些包含在要求主體中，將會忽略這些值。 

<span/>

## <a name="enums"></a>列舉

這些方法會使用下列列舉。

<span id="price-tiers" />
### <a name="price-tiers"></a>價格區間

下列值代表應用程式提交的[價格資源](#pricing-object)資源的可用價格區間。

| 值           | 描述        |
|-----------------|------|
|  Base               |   未設定價格區間；使用應用程式的基本價格。      |     
|  NotAvailable              |   指定的區域中無法使用此應用程式。    |     
|  Free              |   應用程式是免費的。    |    
|  第 *xxx* 層               |   指定應用程式的價格區間的字串，格式為**第 <em>xxx</em> 層**。 目前支援下列價格區間範圍︰<br/><br/><ul><li>如果[價格資源](#pricing-object) 的 *isAdvancedPricingModel* 值為**true**，您帳戶的可用價格區間值是 **Tier1012** - **Tier1424**。</li><li>如果[價格資源](#pricing-object) 的 *isAdvancedPricingModel* 值為**false**，您帳戶的可用價格區間值是 **Tier2** - **Tier96**。</li></ul>若要查看您的開發人員帳戶可用的完整價格區表，包括與每一層相關聯的市場特定價格，請移至適用於開發人員中心儀表板中的應用程式提交的任何**價格與可用性**網頁，並按一下 **\[市場和自訂價格\]** 區段的**檢視表格**連結 (對於某些開發人員帳戶，此連結為**價格**區段)。    |


<span id="enterprise-licensing" />
### <a name="enterprise-licensing-values"></a>企業授權值

下列值代表應用程式適用的企業授權行為。 如需這些選項的詳細資訊，請參閱[組織授權選項](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing)。

| 值           |  描述      |
|-----------------|---------------|
| None            |     請勿利用市集管理 (線上) 大量授權提供企業使用您的應用程式。         |     
| Online        |     利用市集管理 (線上) 大量授權提供企業使用您的應用程式。  |
| OnlineAndOffline | 利用市集管理 (線上) 大量授權提供企業使用您的應用程式，以及透過中斷連線 (離線) 授權提供企業使用您的應用程式。 |


<span id="submission-status-code" />
### <a name="submission-status-code"></a>提交狀態碼

下列值代表提交的狀態碼。

| 值           |  描述      |
|-----------------|---------------|
| None            |     未指定任何代碼。         |     
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
| Other  | 提交處於無法辨識或未分類的狀態。 |
| PackageValidationWarning | 套件驗證程序產生了警告。 |

<span/>

## <a name="related-topics"></a>相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [使用 Windows 市集提交 API 取得應用程式資料](get-app-data.md)
* [開發人員中心儀表板中的應用程式提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)
