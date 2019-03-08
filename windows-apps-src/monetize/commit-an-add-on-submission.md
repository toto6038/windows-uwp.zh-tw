---
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: 在 Microsoft Store 提交 API 中使用這個方法，以認可新的或更新的附加元件提交到合作夥伴中心。
title: 認可附加元件提交
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store 提交 API, 認可附加元件提交, 應用程式內產品, IAP
ms.localizationpriority: medium
ms.openlocfilehash: efab4412486566ae817eb66e78f5407533a30d5b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608213"
---
# <a name="commit-an-add-on-submission"></a>認可附加元件提交

在 Microsoft Store 提交 API 中使用這個方法，以認可新的或更新附加元件 （也稱為應用程式內產品或 IAP） 提交至合作夥伴中心。 認可動作警示合作夥伴中心，提交資料已上傳 （包括任何相關的圖示）。 在回應中，合作夥伴中心會認可的變更，提交資料的擷取及發行。 認可作業成功後，提交的變更會顯示在合作夥伴中心。

如需認可作業如何在使用 Microsoft Store 提交 API 提交附加元件的程序中進行的詳細資訊，請參閱[管理附加元件提交](manage-add-on-submissions.md)。

## <a name="prerequisites"></a>必要條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Microsoft Store 提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* [建立附加元件提交](create-an-add-on-submission.md)，然後[更新提交](update-an-add-on-submission.md)，對提交資料進行任何必要的變更。

## <a name="request"></a>要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |


### <a name="request-header"></a>要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 在表單中的 Azure AD 存取權杖**持有人** &lt;*語彙基元*&gt;。 |


### <a name="request-parameters"></a>要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字串 | 必要。 包含您想要認可提交之附加元件的市集識別碼。 存放區的識別碼在合作夥伴中心，而且它包含在要求的回應資料[取得所有附加元件](get-all-add-ons.md)並[建立附加元件](create-an-add-on.md)。 |
| submissionId | 字串 | 必要。 您想要認可之提交的識別碼。 在[建立附加元件提交](create-an-add-on-submission.md)要求的回應資料中有提供此識別碼。 提交在合作夥伴中心所建立，此識別碼也會提供在合作夥伴中心內的 [提交] 頁面的 url。  |


### <a name="request-body"></a>要求本文

不提供此方法的要求本文。

### <a name="request-example"></a>要求範例

下列範例示範如何認可附加元件提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>回應

下列範例示範成功呼叫此方法時的 JSON 回應主體。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "status": "CommitStarted"
}
```

### <a name="response-body"></a>回應主體

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 字串  | 提交的狀態。 這可以是下列其中一個值： <ul><li>無</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>發行</li><li>ReleaseFailed</li></ul>  |


## <a name="error-codes"></a>錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 要求參數無效。 |
| 404  | 找不到指定的提交。 |
| 409  | 找不到指定的提交，但無法認可目前的狀態，或附加元件會使用合作夥伴中心功能[目前不支援 Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md#not_supported)。 |


## <a name="related-topics"></a>相關主題

* [建立和管理使用 Microsoft Store 服務的提交內容](create-and-manage-submissions-using-windows-store-services.md)
* [取得附加元件提交](get-an-add-on-submission.md)
* [建立附加元件提交](create-an-add-on-submission.md)
* [更新附加元件提交](update-an-add-on-submission.md)
* [刪除附加元件提交](delete-an-add-on-submission.md)
* [取得附加元件送出的狀態](get-status-for-an-add-on-submission.md)
