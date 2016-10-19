---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "使用 Windows 市集提交 API 以程式設計方式為登錄到您 Windows 開發人員中心帳戶的 App 建立和管理提交。"
title: "使用 Windows 市集服務建立和管理提交"
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 0a566dfee8f7fe08c06ce4963435a70c30b1650d

---

# 使用 Windows 市集服務建立和管理提交


使用 *Windows 市集提交 API* 以程式設計的方式查詢和建立您或您組織的 Windows 開發人員中心帳戶內 App、附加元件 (也稱為應用程式內產品或 IAP) 以及套件正式發行前小眾測試版的提交。 如果您的帳戶管理多個 App 或附加元件，而且您想要自動化與最佳化這些資產的提交程序，這個 API 非常有用。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

下列步驟說明使用 Windows 市集提交 API 的端對端處理程序︰

1.  請確定您已經完成所有[先決條件](#prerequisites)。
3.  在呼叫 Windows 市集提交 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Windows 市集提交 API。 權杖到期之後，您可以產生新的權杖。
4.  [呼叫 Windows 市集提交 API](#call-the-windows-store-submission-api)。



<span id="not_supported" />
>**重要**

> * 這個 API 僅供已被授權使用 API 的 Windows 開發人員中心帳戶使用。 此權限是在各個階段中針對開發人員帳戶啟用，並非所有帳戶目前都啟用此權限。 若要要求早一點存取，請登入開發人員中心儀表板，按一下儀表板下方的 [意見反應]****，選取意見反應區域的 [提交 API]****，並提交您的要求。 當您的帳戶啟用此權限時，您會收到電子郵件。

> * 這個 API 無法用於使用 2016 年 8 月開發人員中心儀表板引入的某些功能的 App 或附加元件，包括 (但不限於) 強制性的 App 更新和市集管理的消耗性附加元件。 如果您將 Windows 市集提交 API 用於使用上述其中一個功能的 App 或附加元件，API 將會傳回 409 錯誤碼。 在此情況下，您必須使用儀表板管理 App 或附加元件的提交。

<span id="prerequisites" />
## 步驟 1︰完成使用 Windows 市集提交 API 的先決條件

開始撰寫程式碼以呼叫 Windows 市集提交 API 之前，請先確定您已完成下列先決條件。

* 您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域系統管理員](http://go.microsoft.com/fwlink/?LinkId=746654)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。 如果沒有，可以免費[在開發人員中心建立新的 Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

* 您必須[將 Azure AD 應用程式與您的 Windows 開發人員中心帳戶相關聯](#associate-an-azure-ad-application-with-your-windows-dev-center-account)並取得您的租用戶識別碼、用戶端識別碼和金鑰。 您需要這些值才能取得 Azure AD 存取權杖，您將會在呼叫 Windows 市集提交 API 時使用權杖。

* 準備您的 App 與 Windows 市集提交 API 搭配使用︰

  * 如果您的 App 還不在開發人員中心中，請[在開發人員中心儀表板中建立您的 App](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)。 您無法使用 Windows 市集提交 API 在開發人員中心建立 App。您必須使用儀表板建立您的 App，然後就可以使用 API 存取 App，並以程式設計方式建立提交。 不過，您可以使用 API 以程式設計方式建立附加元件和套件正式發行前小眾測試版，再建立提交。

  * 使用這個 API 建立特定 App 的提交之前，您必須先[在開發人員中心儀表板中建立 App 的一個提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)，包括回答[年齡分級](https://msdn.microsoft.com/windows/uwp/publish/age-ratings)等問題。 執行這個動作之後，您就能夠使用 API 以程式設計方式建立此 App 的新提交。 您不需要先建立附加元件提交或套件正式發行前小眾測試版提交，才能使用 API 進行這些類型的提交。

  * 如果您要建立或更新 App 提交，而且必須包含 App 套件，請[準備 App 套件](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)。

  * 如果您要建立或更新 App 提交，而且必須包含市集清單的螢幕擷取畫面或影像，請[準備 App 螢幕擷取畫面與影像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。

  * 如果您要建立或更新附加元件提交，而且必須包含圖示，請[準備圖示](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)。

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### 如何將 Azure AD 應用程式與您的 Windows 開發人員中心帳戶產生關聯

在您使用 Windows 市集提交 API 之前，您必須將 Azure AD 應用程式與開發人員中心帳戶相關聯、擷取應用程式的租用戶識別碼和用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Windows 市集提交 API 的 App 或服務。 您需要租用戶識別碼、用戶端識別碼和金鑰，才能取得傳遞給 API 的 Azure AD 存取權杖。

>**注意**&nbsp;&nbsp;您只需要執行此工作一次。 有了租用戶識別碼、用戶端識別碼和金鑰，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。

1.  在開發人員中心，移至您的 [帳戶設定]****，按一下 [管理使用者]****，將您組織的開發人員中心帳戶與您組織的 Azure AD 目錄產生關聯。 如需詳細指示，請參閱[管理帳戶使用者](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

2.  在 [管理使用者]**** 頁面中，按一下 [新增 Azure AD 應用程式]****，新增代表您要用來存取開發人員中心帳戶提交之 App 或服務的 Azure AD 應用程式，並指派 [管理員]**** 角色給它。 如果這個應用程式已經在您的 Azure AD 目錄中，則您可以在 [新增 Azure AD 應用程式]**** 頁面中選取它，以將其新增至您的開發人員中心帳戶。 如果不是，可以在 [新增 Azure AD 應用程式]**** 頁面建立新的 Azure AD 應用程式。 如需詳細資訊，請參閱[新增和管理 Azure AD 應用程式](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)。

3.  返回 [管理使用者]**** 頁面，按一下您 Azure AD 應用程式的名稱來移至應用程式設定，然後複製 [租用戶識別碼]**** 和 [用戶端識別碼]**** 的值。

4. 按一下 [加入新的金鑰]****。 在下列畫面中，複製 [金鑰]**** 的值。 您離開這個頁面之後就無法再存取此資訊。 如需詳細資訊，請參閱[新增和管理 Azure AD 應用程式](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)中管理金鑰的相關資訊。

<span id="obtain-an-azure-ad-access-token" />
## 步驟 2：取得 Azure AD 存取權杖

在 Windows 市集提交 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指示，將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

對於 *tenant\_id*、*client\_id* 和 *client\_secret* 參數，指定您在上一節從開發人員中心為應用程式擷取的租用戶識別碼、用戶端識別碼和金鑰。 對於 *resource* 參數，您必須指定 ```https://manage.devcenter.microsoft.com``` URI。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。

<span id="call-the-windows-store-submission-api">
## 步驟 3：使用 Windows 市集提交 API

取得 Azure AD 存取權杖之後，您就可以呼叫 Windows 市集提交 API 中的方法。 API 包含許多方法，可以分成適用於 App、附加元件與套件正式發行前小眾測試版的案例。 若要建立或更新提交，您通常要以特定順序呼叫 Windows 市集提交 API 中的多個方法。 如需每個案例及每個方法之語法的相關資訊，請參閱下表中的文章。

>**注意**&nbsp;&nbsp;取得存取權杖之後，在權杖到期之前，您有 60 分鐘的時間可以呼叫 Windows 市集提交 API 中的方法。

| 案例       | 描述                                                                 |
|---------------|----------------------------------------------------------------------|
| App |  為登錄到您 Windows 開發人員中心帳戶的所有 App 擷取資料，並建立 App 的提交。 如需這些方法的詳細資訊，請參閱下列文章： <ul><li>[取得 App 資料](get-app-data.md)</li><li>[管理 App 提交](manage-app-submissions.md)</li></ul> |
| 附加元件 | 取得、建立或刪除您 App 的附加元件，然後取得、建立或刪除附加元件的提交。 如需這些方法的詳細資訊，請參閱下列文章： <ul><li>[管理附加元件](manage-add-ons.md)</li><li>[管理附加元件提交](manage-add-on-submissions.md)</li></ul> |
| 套件正式發行前小眾測試版 | 取得、建立或刪除您 App 的套件正式發行前小眾測試版，然後取得、建立或刪除套件正式發行前小眾測試版的提交。 如需這些方法的詳細資訊，請參閱下列文章： <ul><li>[管理套件正式發行前小眾測試版](manage-flights.md)</li><li>[管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)</li></ul> |

<span />

## 程式碼範例

下列文章提供詳細的程式碼範例，示範如何以多個不同的程式設計語言使用 Windows 市集提交 API：

* [C# 程式碼範例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 程式碼範例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 程式碼範例](python-code-examples-for-the-windows-store-submission-api.md)

## 疑難排解

| 問題      | 解決方法                                          |
|---------------|---------------------------------------------|
| 從 PowerShell 呼叫 Windows 市集提交 API 之後，如果您使用 [ConvertFrom-Json](https://technet.microsoft.com/en-us/library/hh849898.aspx) Cmdlet 從 JSON 格式轉換為 PowerShell 物件，然後使用 [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) Cmdlet 轉換回 JSON 格式，API 的回應資料會損毀。 |  [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) Cmdlet 的 *-Depth* 參數預設是設為 2 層物件，這對於 Windows 市集提交 API 傳回的大部分 JSON 物件來說太淺。 當您呼叫 [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) Cmdlet 時，請將 *-Depth* 參數設為較大的數字，例如 20。 |

## 其他說明

如果您有 Windows 市集提交 API 的相關問題，或需要使用這個 API 管理提交的協助，請使用下列資源︰

* 在我們的[論壇](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpsubmit)發問。
* 造訪我們的[支援頁面](https://developer.microsoft.com/windows/support)並提出開發人員中心儀表板其中一個協助支援選項。 如果系統提示您選擇問題類型以及類別，請分別選擇 **App 提交和認證**以及**提交 App**。  

## 相關主題

* [取得 App 資料](get-app-data.md)
* [管理 App 提交](manage-app-submissions.md)
* [管理附加元件](manage-add-ons.md)
* [管理附加元件提交](manage-add-on-submissions.md)
* [管理套件正式發行前小眾測試版](manage-flights.md)
* [管理套件正式發行前小眾測試版提交](manage-flight-submissions.md)
 



<!--HONumber=Sep16_HO1-->

