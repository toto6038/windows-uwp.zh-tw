---
author: mcleanbyron
ms.assetid: 96C090C1-88F8-42E7-AED1-AFA9031E952B
description: "使用 Windows 市集提交 API 中的這個方法，刪除現有的 App 提交。"
title: "使用 Windows 市集提交 API 刪除 App 提交"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 4109414e684fc38f461f9e97af38839cb83330fc

---

# 使用 Windows 市集提交 API 刪除 App 提交




使用 Windows 市集提交 API 中的這個方法，刪除現有的 App 提交。

## 先決條件

若要使用這個方法，您必須先進行下列動作：

* 如果您尚未完成，請先完成 Windows 市集提交 API 的所有[先決條件](create-and-manage-submissions-using-windows-store-services.md#prerequisites)。
* [取得 Azure AD 存取權杖](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)以便用於這個方法的要求標頭。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以取得新的權杖。

>**注意**&nbsp;&nbsp;這個方法僅供已被授權使用 Windows 市集提交 API 的 Windows 開發人員中心帳戶使用。 並非所有的帳戶都已啟用此權限。

## 要求

這個方法的語法如下。 請參閱下列各小節了解標頭和要求本文的使用範例和描述。

| 方法 | 要求 URI                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/submissions/{submissionId}``` |

<span/>
 

### 要求標頭

| 標頭        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | 字串 | 必要。 Azure AD 存取權杖，形式為 **Bearer** &lt;*token*&gt;。 |

<span/>

### 要求參數

| 名稱        | 類型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| applicationId | 字串 | 必要。 包含要刪除提交之 App 的市集識別碼。 如需有關市集識別碼的詳細資訊，請參閱[檢視 App 身分識別詳細資料](https://msdn.microsoft.com/windows/uwp/publish/view-app-identity-details)。  |
| submissionId | 字串 | 必要。 要刪除之提交的識別碼。 識別碼可在開發人員中心儀表板上取得，並且包含在[建立 App 提交](create-an-app-submission.md)要求的回應資料中。  |

<span/>

### 要求本文

不提供此方法的要求本文。

<span/>

### 要求範例

下列範例示範如何刪除 App 提交。

```
DELETE https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/submissions/1152921504621243610 HTTP/1.1
Authorization: Bearer <your access token>
```

## 回應

如果成功，此方法會傳回空白回應主體。

## 錯誤碼

如果要求無法順利完成，則回應會包含下列其中一個 HTTP 錯誤碼。

| 錯誤碼 |  描述   |
|--------|------------------|
| 400  | 要求參數無效。 |
| 404  | 找不到指定的提交。 |
| 409  | 找到指定的提交，但無法以其目前的狀態刪除，或 App 使用 [Windows 市集提交 API 目前不支援](create-and-manage-submissions-using-windows-store-services.md#not_supported)的開發人員中心儀表板功能。 |

<span/>


## 相關主題

* [使用 Windows 市集服務建立和管理提交](create-and-manage-submissions-using-windows-store-services.md)
* [取得 App 提交](get-an-app-submission.md)
* [建立 App 提交](create-an-app-submission.md)
* [認可 App 提交](commit-an-app-submission.md)
* [更新 App 提交](update-an-app-submission.md)
* [取得 App 提交的狀態](get-status-for-an-app-submission.md)



<!--HONumber=Aug16_HO5-->


