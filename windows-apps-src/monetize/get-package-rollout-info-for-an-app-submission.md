---
description: 使用「Microsoft Store 提交 API」中的這個方法，來取得 App 提交的套件推出資訊。
title: 取得 App 提交的推出資訊
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 套件推出, 應用程式提交
ms.assetid: 9ada5ac3-a86e-4bb6-8ebc-915ba9649e3c
ms.localizationpriority: medium
ms.openlocfilehash: 3ca8fc759d123a25a58c8126426fb41066d5f1b4
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358883"
---
# <a name="get-rollout-info-for-an-app-submission"></a>取得 App 提交的推出資訊


使用「Microsoft Store 提交 API」中的這個方法，來取得套件正式發行前小眾測試版提交的[套件推出](../publish/gradual-package-rollout.md)資訊。 如需使用 Microsoft Store 提交 API 建立 App 提交的程序的詳細資訊，請參閱[管理 App 提交](manage-app-submissions.md)。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* 建立您的應用程式的其中一個的送出。 您可以在合作夥伴中心，或您可以使用[建立應用程式提交](create-an-app-submission.md)方法。

## <a name="request"></a>要求

這個方法的語法如下。 如需使用方式範例及標頭和要求參數的描述，請參閱下列各節。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| GET   | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}/packagerollout   ``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 App 的「市集識別碼」，此 App 包含您想要取得其套件推出資訊的提交。 如需有關市集識別碼的詳細資訊，請參閱[檢視應用程式身分識別詳細資料](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字串 | 必要。 您想要取得其套件推出資訊之提交的識別碼。 在[建立 App 提交](create-an-app-submission.md)要求的回應資料中有提供此識別碼。 提交在合作夥伴中心所建立，此識別碼也會提供在合作夥伴中心內的 [提交] 頁面的 url。  |


### <a name="request-body"></a>要求本文

不提供此方法的要求本文。

### <a name="request-example"></a>要求範例

下列範例示範如何取得 App 提交的套件推出資訊。

```
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243649/packagerollout HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例針對已啟用漸進式套件推出功能的 App 提交，示範成功呼叫這個方法時的 JSON 回應主體。 如需有關回應主體中各個值的詳細資訊，請參閱[套件推出資源](manage-app-submissions.md#package-rollout-object)。

```json
{
    "isPackageRollout": true,
    "packageRolloutPercentage": 25.0,
    "packageRolloutStatus": "PackageRolloutInProgress",
    "fallbackSubmissionId": "1212922684621243058"
}
```

如果 App 提交未啟用漸進式套件推出功能，則會傳回以下回應主體。

```json
{
    "isPackageRollout": false,
    "packageRolloutPercentage": 0.0,
    "packageRolloutStatus": "PackageRolloutNotStarted",
    "fallbackSubmissionId": "0"
}
```

## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 404  | 找不到提交。 |
| 409  | 提交不屬於指定的應用程式，或應用程式使用的合作夥伴中心功能[目前不支援 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |   


## <a name="related-topics"></a>相關主題

* [漸進式封裝首度發行](../publish/gradual-package-rollout.md)
* [管理使用 Microsoft Store 提交 API 提交的應用程式](manage-app-submissions.md)
* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
