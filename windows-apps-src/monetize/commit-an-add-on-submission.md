---
author: mcleanbyron
ms.assetid: AC74B4FA-5554-4C03-9683-86EE48546C05
description: "使用 Windows 市集提交 API 中的這個方法，將新的或或更新的附加元件提交認可到 Windows 開發人員中心。"
title: "使用 Windows 市集提交 API 認可附加元件提交"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: f9b9e5801f94101156850086c16311cf567b1e7d

---

# 使用 Windows 市集提交 API 認可附加元件提交




使用 Windows 市集提交 API 中的這個方法，將新的或或更新的附加元件 (也稱為應用程式內產品或 IAP) 提交認可到 Windows 開發人員中心。 認可動作會警示開發人員中心提交資料已上傳 (包括任何相關的圖示)。 在回應中，開發人員中心會認可對提交資料所做的變更以供擷取和發佈。 認可作業成功之後，提交的變更會顯示在開發人員中心儀表板中。

如需認可作業如何在使用 Windows 市集提交 API 提交附加元件的程序中進行的詳細資訊，請參閱[管理附加元件提交](manage-add-on-submissions.md)。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。
* [建立附加元件提交](create-an-add-on-submission.md)，然後[更新提交](update-an-add-on-submission.md)，對提交資料進行任何必要的變更。

>**注意**&nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| POST    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}/commit``` |

<span/>
 

### 要求的標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | 字串 | 必要。 包含您想要認可提交之附加元件的市集識別碼。 市集識別碼可在開發人員中心儀表板上取得，並且包含在[取得所有附加元件](get-all-add-ons.md)和[建立附加元件](create-an-add-on.md)要求的回應資料中。 |
| submissionId | 字串 | 必要。 您想要認可之提交的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立附加元件提交](create-an-add-on-submission.md)要求的回應資料中。  |

<span/>

### 要求本文

不提供此方法的要求本文。

### 要求範例

下列範例示範如何認可附加元件提交。

```
POST https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023/commit HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

下列範例示範成功呼叫這個方法的 JSON 回應本文。 如需回應本文中各個值的詳細資訊，請參閱下列各節。

```json
{
  "status": "CommitStarted"
}
```

### 回應本文

| 值      | 類型   | 描述                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| status           | 字串  | 提交的狀態。 這可以是下列其中一個值： <ul><li>None</li><li>Canceled</li><li>PendingCommit</li><li>CommitStarted</li><li>CommitFailed</li><li>PendingPublication</li><li>Publishing</li><li>Published</li><li>PublishFailed</li><li>PreProcessing</li><li>PreProcessingFailed</li><li>Certification</li><li>CertificationFailed</li><li>Release</li><li>ReleaseFailed</li></ul>  |

<span/>

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 要求參數無效。 |
| 404  | 找不到指定的提交。 |
| 409  | 找到指定的提交，但無法以其目前的狀態認可，或附加元件使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |

<span/>


## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得附加元件提交](get-an-add-on-submission.md)
* [建立附加元件提交](create-an-add-on-submission.md)
* [更新附加元件提交](update-an-add-on-submission.md)
* [刪除附加元件提交](delete-an-add-on-submission.md)
* [取得附加元件提交的狀態](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->


