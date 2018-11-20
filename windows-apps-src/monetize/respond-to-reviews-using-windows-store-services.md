---
author: Xansky
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: 使用 Microsoft Store 評論 API，以程式設計方式在 Microsoft Store 中提交對於您的應用程式評論的回應。
title: 使用Microsoft Store 服務回應評論
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store reviews API, respond to reviews, Microsoft Store 評論 API, 回應評論
ms.localizationpriority: medium
ms.openlocfilehash: 063c228a9a2fcfde9350af4872aabba44f9bb8a5
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7292426"
---
# <a name="respond-to-reviews-using-store-services"></a>使用Microsoft Store 服務回應評論

使用 *Microsoft Store 評論 API*，以程式設計方式在 Microsoft Store 中回應您 app 的評論。 開發人員想要大量回應許多評論不需要使用合作夥伴中心的此 API 會特別有用。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您應用程式或服務的呼叫。

下列步驟說明端對端的程序：

1.  請確定您已經完成所有[先決條件](#prerequisites)。
2.  在呼叫 Microsoft Store 評論 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Microsoft Store 評論 API。 權杖到期之後，您可以產生新的權杖。
3.  [呼叫 Microsoft Store 評論 API](#call-the-windows-store-reviews-api)。

> [!NOTE]
> 除了使用 Microsoft Store 評論 API 以程式設計方式回應評論外，您可以回應評論[使用合作夥伴中心](../publish/respond-to-customer-reviews.md)。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>步驟 1：完成使用 Microsoft Store 評論 API 的先決條件

開始撰寫程式碼以呼叫 Microsoft Store 評論 API 之前，請先確定您已完成下列先決條件。

* 您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域系統管理員](http://go.microsoft.com/fwlink/?LinkId=746654)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。 否則，您可以[建立新的 Azure AD，在合作夥伴中心中](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)沒有額外費用。

* 您必須將 Azure AD 應用程式與您的合作夥伴中心帳戶產生關聯，擷取的租用戶識別碼和應用程式的用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Microsoft Store 評論 API 的應用程式或服務。 您需要租用戶識別碼、用戶端識別碼和金鑰，才能取得傳遞給 API 的 Azure AD 存取權杖。
    > [!NOTE]
    > 您只需要執行此工作一次。 有了租用戶識別碼、用戶端識別碼和金鑰，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。

若要將 Azure AD 應用程式與您的合作夥伴中心帳戶產生關聯並擷取需要的值：

1.  在合作夥伴中心，[您組織的合作夥伴中心帳戶與組織的 Azure AD 目錄產生關聯](../publish/associate-azure-ad-with-dev-center.md)。

2.  接下來，在合作夥伴中心的**帳戶設定**\] 區段中**的使用者**頁面上，從[加入 Azure AD 應用程式](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)，代表應用程式或服務，您將會用來回應評論。 請確定您指派此應用程式 **[管理員]** 角色。 如果尚未應用程式不會存在於您的 Azure AD 目錄，您可以[建立新的 Azure AD 應用程式，在合作夥伴中心](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)。 

3.  返回 **\[使用者\]** 頁面，按一下您 Azure AD 應用程式的名稱來移至應用程式設定，然後複製 **\[租用戶識別碼\]** 和 **\[用戶端識別碼\]** 的值。

4. 按一下 **\[加入新的金鑰\]**。 在下列畫面中，複製 **\[金鑰\]** 的值。 您離開這個頁面之後就無法再存取此資訊。 如需詳細資訊，請參閱[管理 Azure AD 應用程式的金鑰](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步驟 2：取得 Azure AD 存取權杖

在 Microsoft Store 評論 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指示，將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

對於 POST URI 和*client\_id*和*client\_secret*參數中*tenant\_id*值，指定租用戶識別碼、 用戶端識別碼和您從上一節中的合作夥伴中心中擷取您應用程式的索引鍵。 對於 *resource* 參數，您必須指定 ```https://manage.devcenter.microsoft.com```。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>步驟 3：呼叫 Microsoft Store 評論 API

有了 Azure AD 存取權杖之後，就可以呼叫 Microsoft Store 評論 API。 您必須將存取權杖傳送給每個方法的 **Authorization** 標頭。

Microsoft Store 評論 API 包含數種方法您可以用來判斷，是否允許您對特定評論回應和提交一個或多個評論的回應。 請依照此程序，以使用此 API：

1. 取得您想要回應之評論的識別碼。 評論識別碼是在 Microsoft Store 分析 API [取得 app 評論](get-app-reviews.md)方法的回應資料中，以及[評論報告](../publish/reviews-report.md)的[離線下載](../publish/download-analytic-reports.md)中。
2. 呼叫[取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)方法，以判斷是否允許您回應評論。 當客戶提交評論時，他們可以選擇不接收評論的回應。 您無法回應已選擇不要接收評論回應的客戶所提交的評論。
3. 呼叫[提交應用程式評論的回應](submit-responses-to-app-reviews.md)方法，以程式設計方式回應評論。


## <a name="related-topics"></a>相關主題

* [取得應用程式評論](get-app-reviews.md)
* [取得應用程式評論的回應資訊](get-response-info-for-app-reviews.md)
* [提交應用程式評論的回應](submit-responses-to-app-reviews.md)

 
