---
ms.assetid: C78176D6-47BB-4C63-92F8-426719A70F04
description: 在 Microsoft Store 提交 API 中使用這個方法，取得套件正式發行前小眾測試版提交的狀態。
title: 取得套件正式發行前小眾測試版提交的狀態
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 正式發行前小眾測試版提交, 狀態
ms.localizationpriority: medium
ms.openlocfilehash: 28eb08ff5f1dd6a5c7887c100edfe2f38780f6fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175012"
---
# <a name="get-the-status-of-a-package-flight-submission"></a>取得套件正式發行前小眾測試版提交的狀態

在 Microsoft Store 提交 API 中使用這個方法，取得套件正式發行前小眾測試版提交的狀態。 如需使用 Microsoft Store 提交 API 建立套件正式發行前小眾測試版提交程序的詳細資訊，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。
* 為您的其中一個應用程式建立套件航班提交。 您可以在合作夥伴中心中進行這項作業，也可以使用 [建立封裝航班提交](create-a-flight-submission.md) 方法來達到此目的。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/status` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 說明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想取得狀態之套件正式發行前小眾測試版提交的 App 的 Store 識別碼。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](../publish/view-app-identity-details.md)。  |
| flightId | 字串 | 必要。 包含您想取得狀態之提交的套件正式發行前小眾測試版的識別碼。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 針對在合作夥伴中心中建立的航班，此識別碼也可在合作夥伴中心中航班頁面的 URL 中取得。  |
| submissionId | 字串 | 必要。 您想取得狀態之提交的識別碼。 在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中有提供此識別碼。 針對合作夥伴中心中建立的提交，此識別碼也可在 [提交] 頁面的 URL 中找到合作夥伴中心。  |


### <a name="request-body"></a>Request body

不提供此方法的要求主體。

### <a name="request-example"></a>要求範例

下列範例示範如何取得套件正式發行前小眾測試版提交狀態。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 回應本文包含指定提交的相關資訊。 如需回應主體中各個值的詳細資訊，請參閱下列各節。

```json
{
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [],
    "warnings": [],
    "certificationReports": []
  },
}
```

### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 字串  | 提交的狀態。 這個值可以是下列其中一個值： <ul><li>無</li><li>已取消</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>發佈</li><li>已發行</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>認證</li><li>CertificationFailed</li><li>版本</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件 (object)  |  包含其他有關提交狀態的詳細資料 (包括任何錯誤的資訊)。 如需詳細資訊，請參閱[狀態詳細資料資源](manage-flight-submissions.md#status-details-object)。 |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 應用程式會使用 [目前 Microsoft Store 提交 API 不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。  |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
* [取得應用程式提交](get-an-app-submission.md)
* [建立應用程式提交](create-an-app-submission.md)
* [認可應用程式提交](commit-an-app-submission.md)
* [更新應用程式提交](update-an-app-submission.md)
* [刪除應用程式提交](delete-an-app-submission.md)