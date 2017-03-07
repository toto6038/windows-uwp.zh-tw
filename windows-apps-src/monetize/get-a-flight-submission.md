---
author: mcleanbyron
ms.assetid: A0DFF26B-FE06-459B-ABDC-3EA4FEB7A21E
description: "在 Windows 市集提交 API 中使用這個方法，取得現有套件正式發行前小眾測試版提交的資料。"
title: "使用 Windows 市集提交 API 取得套件正式發行前小眾測試版提交"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, Windows 市集提交 API, 正式發行前小眾測試版提交"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a989e9d081ec9cb709b59e1ff4ceff2d517ae48f
ms.lasthandoff: 02/07/2017

---

# <a name="get-a-package-flight-submission-using-the-windows-store-submission-api"></a>使用 Windows 市集提交 API 取得套件正式發行前小眾測試版提交




在 Windows 市集提交 API 中使用這個方法，取得現有套件正式發行前小眾測試版提交的資料。 如需使用 Windows 市集提交 API 建立套件正式發行前小眾測試版提交程序的詳細資訊，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 針對您開發人員中心帳戶中的 App 建立套件正式發行前小眾測試版提交。 您可以在開發人員中心儀表板中進行，或者可以使用[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)方法進行。

>**注意**&nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions{submissionId}``` |

<span/>
 

### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想要取得套件正式發行前小眾測試版提交之 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| flightId | 字串 | 必要。 包含您要取得提交之套件正式發行前小眾測試版的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中。  |
| submissionId | 字串 | 必要。 要取得之提交的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中。  |

<span/>

### <a name="request-body"></a>要求本文

不提供此方法的要求本文。

### <a name="request-example"></a>要求範例

下列範例示範如何為市集識別碼為 9WZDNCRD91MD 的 App 取得套件正式發行前小眾測試版提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 回應本文包含指定提交的相關資訊。 如需回應本文中各個值的詳細資訊，請參閱[套件正式發行前小眾測試版提交資源](manage-flight-submissions.md#flight-submission-object)。

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

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到套件正式發行前小眾測試版。 |
| 409  | 套件正式發行前小眾測試版提交不屬於指定的套件正式發行前小眾測試版，或 App 使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |   

<span/>


## <a name="related-topics"></a>相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
* [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)
* [認可套件正式發行前小眾測試版提交](commit-a-flight-submission.md)
* [更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)
* [刪除套件正式發行前小眾測試版提交](delete-a-flight-submission.md)

