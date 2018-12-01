---
ms.assetid: 039B8810-5C9E-4DB9-A6AF-33E7401311FF
description: 在 Microsoft Store 提交 API 中使用這個方法，取得 App 提交的狀態。
title: 取得 App 提交的狀態
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 應用程式提交, 狀態
ms.localizationpriority: medium
ms.openlocfilehash: 97070136f9e1f926a275185d2b4c222513aa49d7
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8339516"
---
# <a name="get-the-status-of-an-app-submission"></a>取得 App 提交的狀態

在 Microsoft Store 提交 API 中使用這個方法，取得 App 提交的狀態。 如需使用 Microsoft Store 提交 API 建立 App 提交的程序的詳細資訊，請參閱[管理 App 提交](manage-app-submissions.md)。

## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求主體的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/status``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含您想取得狀態之提交的 App 的 Store 識別碼。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字串 | 必要。 您想取得狀態之提交的識別碼。 在[建立 App 提交](create-an-app-submission.md)要求的回應資料中有提供此識別碼。 對於在合作夥伴中心中建立的提交，這個 ID 也是適用於在合作夥伴中心提交頁面的 URL。  |


### <a name="request-body"></a>要求主體

不提供此方法的要求主體。

### <a name="request-example"></a>要求範例

下列範例示範如何取得 App 提交狀態。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610/status HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 回應本文包含指定提交的相關資訊。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

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
| status           | 字串  | 提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>   |
| statusDetails           | 物件  |  包含其他有關提交狀態的詳細資料 (包括任何錯誤的資訊)。 如需詳細資訊，請參閱[狀態詳細資料資源](manage-app-submissions.md#status-details-object)。 |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 應用程式會使用[Microsoft Store 提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。  |


## <a name="related-topics"></a>相關主題

* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得 App 提交](get-an-app-submission.md)
* [建立 App 提交](create-an-app-submission.md)
* [認可 App 提交](commit-an-app-submission.md)
* [更新 App 提交](update-an-app-submission.md)
* [刪除 App 提交](delete-an-app-submission.md)
