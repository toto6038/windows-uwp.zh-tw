---
description: 使用「Microsoft Store 提交 API」中的這個方法，來停止 App 提交的套件推出。
title: 中斷應用程式提交的推出
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 套件推出, 應用程式提交, 停止
ms.assetid: 4ce79fe3-deda-4d31-b938-d672c3869051
ms.localizationpriority: medium
ms.openlocfilehash: ba6c32bcf61f16c5b734ad58c591a9575a3440a2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158542"
---
# <a name="halt-the-rollout-for-an-app-submission"></a>中斷應用程式提交的推出


在 Microsoft Store 提交 API 中使用這個方法，可讓應用程式提交的 [套件推出](../publish/gradual-package-rollout.md#completing-the-rollout) 。 如需使用 Microsoft Store 提交 API 建立 App 提交的程序的詳細資訊，請參閱[管理 App 提交](manage-app-submissions.md)。

> [!NOTE]
> 如果您停止 App 提交的推出，然後[建立新的 App 提交](create-an-app-submission.md)，新提交是暫止提交的複本。


## <a name="prerequisites"></a>先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您有 60 分鐘的使用時間，之後其便會到期。 權杖到期之後，您可以取得新的權杖。
* 為您的其中一個應用程式建立提交。 您可以在合作夥伴中心中進行這項作業，也可以使用 [ [建立應用程式] 提交](create-an-app-submission.md) 方法來完成這項作業。
* 啟用提交的漸進式套件推出。 您可以 [在合作夥伴中心中](../publish/gradual-package-rollout.md)這麼做，也可以 [使用 Microsoft Store 提交 API](manage-app-submissions.md#manage-gradual-package-rollout)來完成這項作業。

## <a name="request"></a>要求

這個方法的語法如下。 如需使用方式範例及標頭和要求參數的描述，請參閱下列各節。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST   | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/haltpackagerollout` |


### <a name="request-header"></a>要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授權 | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 說明                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 App 的「Store 識別碼」，此 App 包含您想要停止其套件推出的提交。 如需有關 Store 識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](../publish/view-app-identity-details.md)。  |
| submissionId | 字串 | 必要。 您想要停止其套件推出之提交的識別碼。 在[建立 App 提交](create-an-app-submission.md)要求的回應資料中有提供此識別碼。 針對合作夥伴中心中建立的提交，此識別碼也可在 [提交] 頁面的 URL 中找到合作夥伴中心。  |


### <a name="request-body"></a>Request body

不提供此方法的要求主體。

### <a name="request-example"></a>要求範例

下列範例示範如何停止 App 提交的套件推出。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243680/haltpackagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 如需有關回應主體中各個值的詳細資訊，請參閱[套件推出資源](manage-app-submissions.md#package-rollout-object)。

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutStopped",
    "fallbackSubmissionId": "1212922684621243058"
}
```

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 此代碼指出下列其中一個錯誤：<br/><br/><ul><li>提交狀態不是漸進式推出作業的有效狀態 (呼叫此方法之前，必須先發佈提交且 [packageRolloutStatus](manage-app-submissions.md#package-rollout-object) 值必須設定為 **PackageRolloutInProgress**)。</li><li>提交不屬於指定的 App。</li><li>應用程式會使用 [目前 Microsoft Store 提交 API 不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的合作夥伴中心功能。</li></ul> |   


## <a name="related-topics"></a>相關主題

* [漸進式套件推出](../publish/gradual-package-rollout.md)
* [使用 Microsoft Store 提交 API 管理 App 提交](manage-app-submissions.md)
* [使用 Microsoft Store 服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)