---
ms.assetid: E8751EBF-AE0F-4107-80A1-23C186453B1C
description: 使用 Microsoft Store 提交 API 中的這個方法，更新現有的 App 提交。
title: 更新應用程式提交
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 應用程式提交, 更新
ms.localizationpriority: medium
ms.openlocfilehash: 77c033ff09d56448f42d1f8084265ac0aa5d5212
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371434"
---
# <a name="update-an-app-submission"></a>更新應用程式提交

使用 Microsoft Store 提交 API 中的這個方法，更新現有的 App 提交。 使用這個方法成功更新提交之後，您必須針對擷取和發佈[認可提交](commit-an-app-submission.md)。

如需這個方法如何在使用 Microsoft Store 提交 API 建立 App 提交的程序中進行的詳細資訊，請參閱[管理 App 提交](manage-app-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 建立您的應用程式的其中一個的送出。 您可以在合作夥伴中心，或您可以使用[建立應用程式提交](create-an-app-submission.md)方法。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| PUT   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}  ``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 您想要更新提交之 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視應用程式身分識別詳細資料](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字串 | 必要。 要更新之提交的識別碼。 在[建立 App 提交](create-an-app-submission.md)要求的回應資料中有提供此識別碼。 提交在合作夥伴中心所建立，此識別碼也會提供在合作夥伴中心內的 [提交] 頁面的 url。  |


### <a name="request-body"></a>要求本文

要求本文包含下列參數。

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| applicationCategory           | 字串  |   指定 App [類別和/或子類別](https://docs.microsoft.com/windows/uwp/publish/category-and-subcategory-table)的字串。 類別與子類別會使用底線 '_' 字元結合為單一字串，例如 **BooksAndReference_EReader**。      |  
| 定價           |  物件  | 此物件包含 App 的定價資訊。 如需詳細資訊，請參閱[定價資源](manage-app-submissions.md#pricing-object)一節。       |   
| 可見度           |  字串  |  應用程式的可見度。 這可以是下列其中一個值： <ul><li>Hidden</li><li>Public</li><li>Private</li><li>NotSet</li></ul>       |   
| targetPublishMode           | 字串  | 提交的發佈模式。 這可以是下列其中一個值： <ul><li>Immediate</li><li>Manual</li><li>SpecificDate</li></ul> |
| targetPublishDate           | 字串  | 如果將 *targetPublishMode* 設為 SpecificDate，則為 ISO 8601 格式的提交發佈日期。  |  
| listings           |   物件  |  機碼和值組的字典，其中每個機碼都是國家/地區代碼，而每個值都是[清單資源](manage-app-submissions.md#listing-object)物件，其中包含 App 的清單資訊。       |   
| hardwarePreferences           |  陣列  |   定義 App [硬體喜好設定](https://docs.microsoft.com/windows/uwp/publish/enter-app-properties)的字串陣列。 這可以是下列其中一個值： <ul><li>觸控</li><li>鍵盤</li><li>滑鼠</li><li>相機</li><li>NfcHce</li><li>Nfc</li><li>BluetoothLE</li><li>Telephony</li></ul>     |   
| automaticBackupEnabled           |  boolean  |   指出 Windows 是否可以在自動備份至 OneDrive 時包含您應用程式的資料。 如需詳細資訊，請參閱[應用程式宣告](https://docs.microsoft.com/windows/uwp/publish/app-declarations)。   |   
| canInstallOnRemovableMedia           |  boolean  |   指出客戶是否可以將您的應用程式安裝到抽取式存放裝置。 如需詳細資訊，請參閱[應用程式宣告](https://docs.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| isGameDvrEnabled           |  boolean |   指出是否已針對應用程式啟用遊戲 DVR。    |   
| gamingOptions           |  物件 |   包含一個[遊戲選項資源](manage-app-submissions.md#gaming-options-object)的陣列，其定義此 App 的遊戲相關設定。     |   
| hasExternalInAppProducts           |     boolean          |   指出您的 App 是否允許使用者在 Microsoft Store 商務系統外部進行購買。 如需詳細資訊，請參閱[應用程式宣告](https://docs.microsoft.com/windows/uwp/publish/app-declarations)。     |   
| meetAccessibilityGuidelines           |    boolean           |  指出您的應用程式是否已經過測試，符合協助工具指導方針。 如需詳細資訊，請參閱[應用程式宣告](https://docs.microsoft.com/windows/uwp/publish/app-declarations)。      |   
| notesForCertification           |  字串  |   包含您 App 的[認證注意事項](https://docs.microsoft.com/windows/uwp/publish/notes-for-certification)。    |    
| applicationPackages           |   陣列  | 包含可提供關於提交中每個套件之詳細資料的物件。 如需詳細資訊，請參閱[應用程式套件](manage-app-submissions.md#application-package-object)一節。 在呼叫這個方法以更新 App 提交時，要求主體中只需要這些物件的 *fileName*、*fileStatus*、*minimumDirectXVersion* 和 *minimumSystemRam* 值。 合作夥伴中心會填入其他值。   |    
| packageDeliveryOptions    | 物件  | 包含提交的漸進式套件推出和強制更新設定。 如需詳細資訊，請參閱[套件交付選項物件](manage-app-submissions.md#package-delivery-options-object)。  |
| enterpriseLicensing           |  字串  |  其中一個[企業授權值](manage-app-submissions.md#enterprise-licensing)，可指出 App 適用的企業授權行為。  |    
| allowMicrosftDecideAppAvailabilityToFutureDeviceFamilies           |  boolean   |  指出是否允許 Microsoft [讓 App 可供未來的 Windows 10 裝置系列使用](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)。    |    
| allowTargetFutureDeviceFamilies           | boolean   |  指出是否允許您的 App [以未來的 Windows 10 裝置系列為目標](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)。     |   
| trailers           |  陣列 |   包含最多 15 個[預告片資源](manage-app-submissions.md#trailer-object)的陣列，代表 App 清單的預告影片。   |   


### <a name="request-example"></a>要求範例

下列範例示範如何更新 App 提交。

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
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
  "applicationPackages": [
    {
      "fileName": "contoso_app.appx",
      "fileStatus": "PendingUpload",
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
  "enterpriseLicensing": "Online",
  "allowMicrosoftDecideAppAvailabilityToFutureDeviceFamilies": true,
  "allowTargetFutureDeviceFamilies": {
    "Desktop": false,
    "Mobile": true,
    "Holographic": true,
    "Xbox": false,
    "Team": true
  },
  "trailers": []
}
```

## <a name="response"></a>回應

下列範例示範成功呼叫此方法時的 JSON 回應主體。 回應主體包含更新提交的相關資訊。 如需回應本文中各個值的詳細資訊，請參閱 [App 提交資源](manage-app-submissions.md#app-submission-object)。

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
            "imageType": "Screenshot"
          }
        ],
        "recommendedHardware": [],
        "title": "Contoso ebook reader"
      },
      "platformOverrides": {
        "Windows81": {
          "description": "Ebook reader for Windows 8.1",
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
      "fileStatus": "PendingUpload",
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

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 無法更新提交，因為要求無效。 |
| 409  | 無法更新送出作業，因為應用程式中的目前狀態，或應用程式使用的合作夥伴中心功能[目前不支援 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相關主題

* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
* [取得應用程式提交](get-an-app-submission.md)
* [建立應用程式提交](create-an-app-submission.md)
* [認可應用程式提交](commit-an-app-submission.md)
* [刪除應用程式提交](delete-an-app-submission.md)
* [取得應用程式提交的狀態](get-status-for-an-app-submission.md)
