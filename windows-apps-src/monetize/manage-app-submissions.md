---
author: mcleanbyron
ms.assetid: C7428551-4B31-4259-93CD-EE229007C4B8
description: "在 Windows 市集提交 API 中使用這些方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理提交。"
title: "使用 Windows 市集提交 API 管理 App 提交"
translationtype: Human Translation
ms.sourcegitcommit: f52059a37194b78db2f9bb29a5e8959b2df435b4
ms.openlocfilehash: 5c19a05f51a14d9df38e64aac3b741e916fc0524

---

# <a name="manage-app-submissions-using-the-windows-store-submission-api"></a>使用 Windows 市集提交 API 管理 App 提交


在 Windows 市集提交 API 中使用下列方法，來為登錄到您 Windows 開發人員中心帳戶的 App 管理提交。 如需 Windows 市集提交 API 的簡介，包括使用此 API 的先決條件，請參閱[使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)。

>**注意**&nbsp;&nbsp;這些方法僅供已獲授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。


| 方法        | URI    | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | 取得現有的 App 提交資料。 如需詳細資訊，請參閱[這篇文章](get-an-app-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` | 取得現有的 App 提交狀態。 如需詳細資訊，請參閱[這篇文章](get-status-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions``` | 為已向您「Windows 開發人員中心」帳戶登錄的 App 建立新的提交。 如需詳細資訊，請參閱[這篇文章](create-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit``` | 將新的或更新的 App 提交認可到「Windows 開發人員中心」。 如需詳細資訊，請參閱[這篇文章](commit-an-app-submission.md)。 |
| PUT | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | 更新現有的 App 提交。 如需詳細資訊，請參閱[這篇文章](update-an-app-submission.md)。 |
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` | 刪除 App 提交。 如需詳細資訊，請參閱[這篇文章](delete-an-app-submission.md)。 |
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout``` | 取得 App 提交的漸進式推出資訊。 如需詳細資訊，請參閱[這篇文章](get-package-rollout-info-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage``` | 取得 App 提交的漸進式推出百分比。 如需詳細資訊，請參閱[這篇文章](update-the-package-rollout-percentage-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout``` | 停止 App 提交的漸進式推出。 如需詳細資訊，請參閱[這篇文章](halt-the-package-rollout-for-an-app-submission.md)。 |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout``` | 完成 App 提交的漸進式推出。 如需詳細資訊，請參閱[這篇文章](finalize-the-package-rollout-for-an-app-submission.md)。 |

<span id="create-an-app-submission">
## <a name="create-an-app-submission"></a>建立 App 提交

若要建立 App 的提交，請遵循此程序。

1. 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。

  >**注意**&nbsp;&nbsp;請確定 App 至少有一個已完成[年齡分級](https://msdn.microsoft.com/windows/uwp/publish/age-ratings)資訊的完整提交。

2. [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)。 您必須將此存取權杖傳遞至 Windows 市集提交 API 中的方法。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

3. 在「Windows 市集提交 API」中執行下列方法來[建立 App 提交](create-an-app-submission.md)。 這個方法會建立新的處理中提交，這是最後一個已發佈提交的複本。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions
  ```

  回應主體包含三個項目︰新提交的識別碼、新提交的資料 (包含所有清單和定價資訊)，以及用於上傳提交的任何 App 套件和清單影像的共用存取簽章 (SAS) URI。 如需 SAS 的詳細資訊，請參閱[共用存取簽章，第 1 部分︰了解 SAS 模型](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。

4. 如果您要新增提交的新套件或影像，請[準備 App 套件](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)和[準備 App 螢幕擷取畫面與影像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 將所有這些檔案新增到 ZIP 封存。

5. 以新提交的任何所需變更來修訂提交資料，然後執行下列方法來[更新 App 提交](update-an-app-submission.md)。

  ```
  PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}
  ```

  >**注意**&nbsp;&nbsp;如果您要為提交新增新的套件或影像，請確定將提交資料更新成參考 ZIP 封存中這些檔案的名稱和相對路徑。

4. 如果您要新增提交的新套件或影像，將 ZIP 封存上傳至您在步驟 2 中呼叫之 POST 方法回應主體中提供的 SAS URI。 如需詳細資訊，請參閱[共用存取簽章，第 2 部分︰透過 Blob 儲存體來建立與使用 SAS](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/)。

  下列程式碼片段示範如何使用「適用於 .NET 的 Azure 儲存體用戶端程式庫」中的 [CloudBlockBlob](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblockblob.aspx) 類別來上傳封存。

  ```csharp
  string sasUrl = "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl";

  Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob blockBob =
      new Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob(new System.Uri(sasUrl));
  await blockBob.UploadFromStreamAsync(stream);
  ```

5. 執行下列方法來[認可 App 提交](commit-an-app-submission.md)。 這會向「開發人員中心」發出警示，指出您已完成提交，而現在應該將更新套用至您的帳戶。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/commit
  ```

6. 執行下列方法來[取得應用程式提交狀態](get-status-for-an-app-submission.md)，以檢查認可狀態。

    ```
    GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status
    ```

    若要確認提交狀態，請檢閱回應主體中的「狀態」值。 這個值應該從 **CommitStarted** 變更為 **PreProcessing** (如果要求成功) 或 **CommitFailed** (如果要求中出現錯誤)。 如果出現錯誤，*statusDetails* 欄位會包含關於錯誤的進一步詳細資料。

7. 順利完成提交之後，即會將提交傳送到市集以供擷取。 您可以使用先前的方法或瀏覽「開發人員中心」儀表板，來繼續監視提交進度。

<span id="manage-gradual-package-rollout">
## <a name="manage-a-gradual-package-rollout-for-an-app-submission"></a>管理 App 提交的漸進式套件推出

您可以將 App 提交中的已更新套件以漸進方式推出給 Windows&nbsp;10 上的一部分 App 客戶。 這可讓您監視特定套件的意見反應和分析資料，以確保您對該項更新信心十足之後，再更廣泛地推出該項更新。 您可以變更所發佈之提交的推出百分比 (或停止更新)，而不需建立新的提交。 如需詳細資訊 (包括如何在「開發人員中心」儀表板中啟用和管理漸進式套件推出的指示)，請參閱[這篇文章](../publish/gradual-package-rollout.md)。

您也可以在「Windows 市集提交 API」中使用下列方法，以程式設計方式啟用和管理 App 提交的漸進式套件推出。

* 啟用 App 提交的漸進式套件推出：

  1. [建立 App 提交](create-an-app-submission.md)或[取得 App 提交](get-an-app-submission.md)。
  2. 在回應資料中，找出 [packageRollout](#package-rollout-object) 資源，將 *isPackageRollout* 欄位設定為 true，然後將 *packageRolloutPercentage* 欄位設定為應該取得已更新套件的 App 客戶百分比。
  3. 將已更新的 App 提交資料傳遞給[更新 App 提交](update-an-app-submission.md)方法。

<span/>

* 若要[取得 App 提交的套件推出資訊](get-package-rollout-info-for-an-app-submission.md)，請執行下列方法。

  ```
  GET https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout
  ```

<span/>

* 若要[更新 App 提交的套件推出百分比](update-the-package-rollout-percentage-for-an-app-submission.md)，請執行下列方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/updatepackagerolloutpercentage  
  ```

<span/>

* 若要[停止 App 提交的套件推出](halt-the-package-rollout-for-an-app-submission.md)，請執行下列方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout   
  ```  

<span/>

* 若要[完成 App 提交的套件推出](finalize-the-package-rollout-for-an-app-submission.md)，請執行下列方法。

  ```
  POST https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/finalizepackagerollout
  ```

## <a name="resources"></a>資源

這些方法會使用下列資源來格式化資料。

<span id="app-submission-object" />
### <a name="app-submission"></a>App 提交

此資源代表 App 的提交。 下列範例示範此資源的格式。

```json
{
  "id": "1152921504621243540",
  "applicationCategory": "BooksAndReference_EReader",
  "pricing": {
    "trialPeriod": "FifteenDays",
    "marketSpecificPricings": {},
    "sales": [],
    "priceId": "Tier2"
  },
  "visibility": "Public",
  "targetPublishMode": "Manual",
  "targetPublishDate": "1601-01-01T00:00:00Z",
  "listings": {
    "en-us": {
      "baseListing": {
        "copyrightAndTrademarkInfo": "",
        "keywords": [],
        "licenseTerms": "",
        "privacyPolicy": "",
        "supportContact": "",
        "websiteUrl": "",
        "description": "Description",
        "features": [],
        "releaseNotes": "",
        "images": [
          {
            "fileName": "contoso.png",
            "fileStatus": "Uploaded",
            "id": "1152921504672272757",
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "ApiTestApp For Devbox"
      },
      "platformOverrides": {}
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
        "packageRolloutPercentage": 0,
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
| pricing           |  物件  | 此物件包含 App 的定價資訊。 如需詳細資訊，請參閱下方的[定價資源](#pricing-object)一節。       |   
| visibility           |  字串  |  App 的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |  
| listings           |   物件  |  機碼和值組的字典，其中每個機碼都是國家/地區代碼，而每個值都是[清單資源](#listing-object)物件，其中包含 App 的清單資訊。       |   
| hardwarePreferences           |  array  |   定義 App [硬體喜好設定](https://msdn.microsoft.com/windows/uwp/publish/enter-app-properties#hardware_preferences)的字串陣列。 這可以是下列其中一個值： <ul><li>Touch</li><li>Keyboard</li><li>Mouse</li><li>Camera</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  布林值  |   指出 Windows 是否可以在自動備份至 OneDrive 時包含您 App 的資料。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  布林值  |   指出客戶是否可以將您的 App 安裝到抽取式存放裝置。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  布林值 |   指出是否已針對 App 啟用遊戲 DVR。    |   
| hasExternalInAppProducts           |     布林值          |   指出您的 App 是否允許使用者在 Windows 市集商務系統外部進行購買。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    布林值           |  指出您的 App 是否已經過測試，符合協助工具指導方針。 如需詳細資訊，請參閱[應用程式宣告](https://msdn.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字串  |   包含您 App 的[認證注意事項](https://msdn.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| status           |   字串  |  提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>      |    
| statusDetails           |   物件  |  包含其他有關提交狀態的詳細資料 (包括任何錯誤的資訊)。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-details-object)一節。       |    
| fileUploadUrl           |   字串  | 共用存取簽章 (SAS) URI，可用於上傳任何適用於提交的套件。 如果您要新增提交的新套件或影像，請將包含套件和影像的 ZIP 封存上傳至這個 URI。 如需詳細資訊，請參閱[建立 App 提交](#create-an-app-submission)。       |    
| applicationPackages           |   array  | 包含可提供關於提交中每個套件之詳細資料的物件。 如需詳細資訊，請參閱下方的[應用程式套件](#application-package-object)一節。 |    
| packageDeliveryOptions    | 物件  | 包含提交的漸進式套件推出和強制更新設定。 如需詳細資訊，請參閱下方的[套件交付選項物件](#package-delivery-options-object)一節。  |
| enterpriseLicensing           |  字串  |  其中一個[企業授權值](#enterprise-licensing)，可指出 App 適用的企業授權行為。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  布林值   |  指出是否允許 Microsoft [讓 App 可供未來的 Windows&nbsp;10 裝置系列使用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)。    |    
| allowTargetFutureDeviceFamilies           | 物件   |  機碼和值組的字典，其中每個機碼都是一個 [Windows&nbsp;10 裝置系列](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability#windows-10-device-families)，而每個值都是一個布林值，可指出您的 App 是否允許將目標設為指定的裝置系列。     |    
| friendlyName           |   字串  |  基於顯示用途而使用的 App 易記名稱。       |  


<span id="listing-object" />
### <a name="listing"></a>清單

此資源包含 App 的清單資訊。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  baseListing               |   物件      |  App 的[基本清單](#base-listing-object)資訊，這會定義適用於所有平台的預設清單資訊。   |     
|  platformOverrides               | 物件 |   機碼和值組的字典，其中每個機碼都是字串，可識別要覆寫清單資訊的平台，而每個值都是[清單](#base-listing-object)物件 (只包含從 description 到 title 的值)，可指定要針對指定平台進行覆寫的清單資訊。 機碼可以具有下列值： <ul><li>Unknown</li><li>Windows80</li><li>Windows81</li><li>WindowsPhone71</li><li>WindowsPhone80</li><li>WindowsPhone81</li></ul>     |      |     

<span id="base-listing-object" />
### <a name="base-listing"></a>基本清單

此資源包含 App 的基本清單資訊。 此資源具有下列值。

| 值           | 類型    | 描述       |
|-----------------|---------|------|
|  copyrightAndTrademarkInfo                |   字串      |  選擇性的[著作權及/或商標資訊](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#copyright-and-trademark-info)。  |
|  keywords                |  陣列       |  [關鍵字](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#keywords)陣列，可協助讓您的 App 出現在搜尋結果中。    |
|  licenseTerms                |    字串     | 適用於您 App 的選擇性[授權條款](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#additional-license-terms)。     |
|  privacyPolicy                |   字串      |   適用於您 App 的[隱私權原則](https://msdn.microsoft.com/windows/uwp/publish/privacy-policy) URL。    |
|  supportContact                |   字串      |  適用於您 App 的[支援連絡資訊](https://msdn.microsoft.com/windows/uwp/publish/support-contact-info)的 URL 或電子郵件地址。     |
|  websiteUrl                |   字串      |  適用於您 App 的[網頁](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#website) URL。    |    
|  description               |    字串     |   App 清單的[描述](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#description)。   |     
|  features               |    陣列     |  此陣列最多包含 20 個字串，可列出您 App 的[功能](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#app-features)。     |
|  releaseNotes               |  字串       |  適用於您 App 的[版本資訊](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#release-notes)。    |
|  images               |   陣列      |  App 清單的[影像和圖示](#image-object)資料陣列。  |
|  recommendedHardware               |   陣列      |  此陣列最多包含 11 個字串，可為您的 App 列出[建議的硬體設定](https://msdn.microsoft.com/windows/uwp/publish/create-app-descriptions#recommended-hardware)。     |
|  title               |     字串    |   App 清單的標題。   |  


<span id="image-object" />
### <a name="image"></a>影像

此資源包含 App 清單的影像和圖示資料。 如需可用於列出之影像和圖示的詳細資訊，請參閱 [App 螢幕擷取畫面與影像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 此資源具有下列值。

| 值           | 類型    | 描述           |
|-----------------|---------|------|
|  fileName               |    字串     |   影像檔的名稱，位於您針對提交所上傳的 ZIP封存中。    |     
|  fileStatus               |   字串      |  影像檔的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>   |
|  id  |  字串  | 由開發人員中心指定的影像識別碼。  |
|  description  |  字串  | 影像的描述。  |
|  imageType  |  字串  | 其中一個下列字串，可指出影像類型： <ul><li>Unknown</li><li>Screenshot</li><li>PromotionalArtwork414X180</li><li>PromotionalArtwork846X468</li><li>PromotionalArtwork558X756</li><li>PromotionalArtwork414X468</li><li>PromotionalArtwork558X558</li><li>PromotionalArtwork2400X1200</li><li>Icon</li><li>WideIcon358X173</li><li>BackgroundImage1000X800</li><li>SquareIcon358X358</li><li>MobileScreenshot</li><li>XboxScreenshot</li><li>SurfaceHubScreenshot</li><li>HoloLensScreenshot</li></ul>      |


<span id="pricing-object" />
### <a name="pricing"></a>定價

此資源包含 App 的定價資訊。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  trialPeriod               |    字串     |  可針對 App 指定試用期的字串。 這可以是下列其中一個值： <ul><li>NoFreeTrial</li><li>OneDay</li><li>TrialNeverExpires</li><li>SevenDays</li><li>FifteenDays</li><li>ThirtyDays</li></ul>    |
|  marketSpecificPricings               |    物件     |  機碼和值組的字典，其中每個機碼都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的 App 在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 這個字典中的任何項目都會覆寫特定市場的 *priceId* 值所指定的基本價格。      |     
|  sales               |   陣列      |  **過時**。 包含 App 銷售資訊的物件陣列。 如需詳細資訊，請參閱下方的[銷售](#sale-object)一節。    |     
|  priceId               |   字串      |  指定 App [基本價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#base-price)的[價格區間](#price-tiers)。   |


<span id="sale-object" />
### <a name="sale"></a>銷售

此資源包含 App 的銷售資訊。

>**重要**&nbsp;&nbsp;**銷售**資源不再支援，目前您無法使用 Windows 市集提交 API 取得或修改 App 提交的銷售資料︰

   > * 在呼叫 [GET 方法以取得 App 提交](get-an-app-submission.md)之後，*sales* 值將會空白。 您可以繼續使用「開發人員中心」儀表板來取得 App 提交的銷售資料。
   > * 呼叫 [PUT 方法以更新 App 提交](update-an-app-submission.md)時，會忽略 *sales* 值中的資訊。 您可以繼續使用「開發人員中心」儀表板來變更 App 提交的銷售資料。

> 我們將來會更新「Windows 市集提交 API」來導入新的方法，以程式設計方式存取 App 提交的銷售資訊。

此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  name               |    字串     |   銷售的名稱。    |     
|  basePriceId               |   字串      |  用於銷售基本價格的[價格區間](#price-tiers)。    |     
|  startDate               |   字串      |   ISO 8601 格式的銷售開始日期。  |     
|  endDate               |   字串      |  ISO 8601 格式的銷售結束日期。      |     
|  marketSpecificPricings               |   物件      |   機碼和值組的字典，其中每個機碼都是兩個字母的 ISO 3166-1 alpha-2 國家/地區代碼，而每個值都是[價格區間](#price-tiers)。 這些項目代表[您的 App 在特定市場中的自訂價格](https://msdn.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection#markets-and-custom-prices)。 這個字典中的任何項目都會覆寫特定市場的 *basePriceId* 值所指定的基本價格。    |


<span id="status-details-object" />
### <a name="status-details"></a>狀態詳細資料

此資源包含關於提交狀態的其他詳細資料。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  errors               |    物件     |   包含提交錯誤詳細資料的物件陣列。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-detail-object)一節。   |     
|  warnings               |   物件      | 包含提交警告詳細資料的物件陣列。 如需詳細資訊，請參閱下方的[狀態詳細資料](#status-detail-object)一節。     |
|  certificationReports               |     物件    |   提供提交認證報告資料存取的物件陣列。 如果認證失敗，您可以檢查這些報告來取得詳細資訊。 如需詳細資訊，請參閱下方的[認證報告](#certification-report-object)一節。   |  


<span id="status-detail-object" />
### <a name="status-detail"></a>狀態詳細資料

此資源包含有關提交的任何相關錯誤或警告的其他資訊。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|  code               |    字串     |   描述錯誤或警告類型的字串。 如需詳細資訊，請參閱下方的[提交狀態碼](#submission-status-code)一節。   |     
|  details               |     字串    |  含有更多關於問題之詳細資料的訊息。     |


<span id="application-package-object" />
### <a name="application-package"></a>應用程式套件

此資源包含有關提交的應用程式套件的相關詳細資料。 下列範例示範此資源的格式。

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

>**注意**&nbsp;&nbsp;在呼叫[更新 App 提交](update-an-app-submission.md)方法時，要求主體中只需要這個物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 及 *minimumSystemRam* 值。 其他值均是由開發人員中心所填入。

| 值           | 類型    | 描述                   |
|-----------------|---------|------|
| fileName   |   字串      |  套件的名稱。    |  
| fileStatus    | 字串    |  套件的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>PendingUpload</li><li>Uploaded</li><li>PendingDelete</li></ul>    |  
| id    |  字串   |  唯一識別套件的識別碼。 此值由開發人員中心所使用。   |     
| version    |  字串   |  App 套件的版本。 如需詳細資訊，請參閱[套件版本編號](https://msdn.microsoft.com/windows/uwp/publish/package-version-numbering)。   |   
| architecture    |  字串   |  套件的架構 (例如，ARM)。   |     
| languages    | 陣列    |  App 所支援之語言的語言代碼陣列。 如需詳細資訊，請參閱[支援的語言](https://msdn.microsoft.com/windows/uwp/publish/supported-languages)。    |     
| capabilities    |  陣列   |  套件所需的功能陣列。 如需功能的詳細資訊，請參閱[應用程式功能宣告](https://msdn.microsoft.com/windows/uwp/packaging/app-capability-declarations)。   |     
| minimumDirectXVersion    |  字串   |  App 套件所支援的最低 DirectX 版本。 這只能針對目標為 Windows 8.x 的 App 進行設定；對於目標為其他版本的 App 則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>DirectX93</li><li>DirectX100</li></ul>   |     
| minimumSystemRam    | 字串    |  App 套件所需的最小 RAM。 這只能針對目標為 Windows 8.x 的 App 進行設定；對於目標為其他版本的 App 則會加以忽略。 這可以是下列其中一個值： <ul><li>None</li><li>Memory2GB</li></ul>   |       
| targetDeviceFamilies    | 陣列    |  代表套件目標裝置系列的字串陣列。 這個值只適用於目標為 Windows&nbsp;10 的套件；對於目標為較舊版本的套件，這個值是 **None** 值。 目前針對 Windows&nbsp;10 套件支援下列的裝置系列字串，其中 *{0}* 是 Windows&nbsp;10 版本字串，例如 10.0.10240.0、10.0.10586.0 或 10.0.14393.0： <ul><li>Windows.Universal min version *{0}*</li><li>Windows.Desktop min version *{0}*</li><li>Windows.Mobile min version *{0}*</li><li>Windows.Xbox min version *{0}*</li><li>Windows.Holographic min version *{0}*</li></ul>   |    

<span/>

<span id="certification-report-object" />
### <a name="certification-report"></a>認證報告

此資源提供提交認證報告資料的存取。 此資源具有下列值。

| 值           | 類型    | 描述                                                                                                                                                                                                                          |
|-----------------|---------|------|
|     日期            |    字串     |  以 ISO 8601 格式產生報告的日期和時間。    |
|     reportUrl            |    字串     |  可供您存取報告的 URL。    |


<span id="package-delivery-options-object" />
### <a name="package-delivery-options-object"></a>套件交付選項物件

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
### <a name="package-rollout-object"></a>套件推出物件

此資源包含提交的漸進式[套件推出設定](#manage-gradual-package-rollout)。 此資源具有下列值。

| 值           | 類型    | 描述        |
|-----------------|---------|------|
| isPackageRollout   |   布林值      |  指出是否已為提交啟用漸進式套件推出。    |  
| packageRolloutPercentage    | 浮點數    |  將接收漸進式推出中套件的使用者百分比。    |  
| packageRolloutStatus    |  字串   |  下列其中一個字串，這些字串指出漸進式套件推出的狀態： <ul><li>PackageRolloutNotStarted</li><li>PackageRolloutInProgress</li><li>PackageRolloutComplete</li><li>PackageRolloutStopped</li></ul>  |  
| fallbackSubmissionId    |  字串   |  未取得漸進式推出套件的客戶將收到的提交識別碼。   |          

<span/>

## <a name="enums"></a>列舉

這些方法會使用下列列舉。


<span id="price-tiers" />
### <a name="price-tiers"></a>價格區間

下列值代表 App 提交可用的價格區間。

| 值           | 描述                                                                                                                                                                                                                          |
|-----------------|------|
|  Base               |   未設定價格區間；使用 App 的基本價格。      |     
|  NotAvailable              |   指定的區域中無法使用此 App。    |     
|  Free              |   App 是免費的。    |    
|  從 Tier2 到 Tier194               |   Tier2 代表 .99 美元的價格區間。 每一個額外的層級代表額外的遞增 (1.29 美元、1.49 美元、1.99 美元等)。    |


<span id="enterprise-licensing" />
### <a name="enterprise-licensing-values"></a>企業授權值

下列值代表 App 適用的企業授權行為。 如需這些選項的詳細資訊，請參閱[組織授權選項](https://msdn.microsoft.com/windows/uwp/publish/organizational-licensing)。

| 值           |  描述      |
|-----------------|---------------|
| None            |     請勿利用市集管理 (線上) 大量授權提供企業使用您的 App。         |     
| Online        |     利用市集管理 (線上) 大量授權提供企業使用您的 App。  |
| OnlineAndOffline | 利用市集管理 (線上) 大量授權提供企業使用您的 App，以及透過中斷連線 (離線) 授權提供企業使用您的 App。 |


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
* [使用 Windows 市集提交 API 取得 App 資料](get-app-data.md)
* [開發人員中心儀表板中的 App 提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)



<!--HONumber=Dec16_HO1-->


