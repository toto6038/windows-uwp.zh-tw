---
ms.assetid: F94AF8F6-0742-4A3F-938E-177472F96C00
description: 認可全新或更新的套件正式發行前小眾到合作夥伴中心，Microsoft Store 提交 API 中使用這個方法。
title: 認可套件正式發行前小眾測試版提交
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 認可正式發行前小眾測試版提交
ms.localizationpriority: medium
ms.openlocfilehash: 820e10695cce2d6242a51b0017d2fe3981cf77b1
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8345772"
---
# <a name="commit-a-package-flight-submission"></a>認可套件正式發行前小眾測試版提交

認可全新或更新的套件正式發行前小眾到合作夥伴中心，Microsoft Store 提交 API 中使用這個方法。 認可動作會警示合作夥伴中心，對提交資料已上傳 （包括任何相關的套件）。 在回應中，合作夥伴中心會針對擷取和發佈對提交資料認可變更。 認可作業成功之後，提交，變更就會顯示在合作夥伴中心。

如需認可作業如何在使用 Microsoft Store 提交 API 建立套件正式發行前小眾測試版提交的程序中進行的詳細資訊，請參閱[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)，然後[更新提交](update-a-flight-submission.md)，對提交資料進行任何必要的變更。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想要認可套件正式發行前小眾測試版提交之 App 的 Store 識別碼。 應用程式的市集識別碼是可在合作夥伴中心。  |
| flightId | 字串 | 必要。 包含要認可提交之套件正式發行前小眾測試版的識別碼。 識別碼可從[建立套件正式發行前小眾測試版](create-a-flight.md)和[取得 App 套件正式發行前小眾測試版](get-flights-for-an-app.md)要求的回應資料中取得。 飛行合作夥伴中心中所建立，這個 ID 也是適用於合作夥伴中心中飛行頁面的 URL。  |
| submissionId | 字串 | 必要。 要認可之提交的識別碼。 在[建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)要求的回應資料中有提供此識別碼。 對於在合作夥伴中心中建立的提交，這個 ID 也是適用於在合作夥伴中心提交頁面的 URL。  |


### <a name="request-body"></a>要求主體

不提供此方法的要求主體。

### <a name="request-example"></a>要求範例

下列範例示範如何認可套件正式發行前小眾測試版提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/flights/43e448df-97c9-4a43-a0bc-2a445e736bcd/submissions/1152921504621243649/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>回應本文

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 字串  | 提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 要求參數無效。 |
| 404  | 找不到指定的提交。 |
| 409  | 找到指定的提交，但無法認可以其目前的狀態，或 app 使用[Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。 |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
* [取得套件正式發行前小眾測試版提交](get-a-flight-submission.md)
* [建立套件正式發行前小眾測試版提交](create-a-flight-submission.md)
* [更新套件正式發行前小眾測試版提交](update-a-flight-submission.md)
* [刪除套件正式發行前小眾測試版提交](delete-a-flight-submission.md)
* [取得套件正式發行前小眾測試版提交的狀態](get-status-for-a-flight-submission.md)
