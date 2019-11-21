---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: 如果您有應用程式及附加元件的型錄，就能使用「Microsoft Store 集合 API」及「Microsoft Store 購買 API」，從您的服務存取這些產品的擁有權資訊。
title: 管理服務的產品權利
ms.date: 08/01/2018
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 集合 API, Microsoft Store 購買 API, 檢視產品, 授與產品
ms.localizationpriority: medium
ms.openlocfilehash: 2d0df7780943717d8e01f0efcf1583efe05608af
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259221"
---
# <a name="manage-product-entitlements-from-a-service"></a>管理服務的產品權利

如果您有應用程式及附加元件的型錄，就能使用「*Microsoft Store 集合 API*」及「*Microsoft Store 購買 API*」，從您的服務存取這些產品的權利資訊。 *「權利」* 表示客戶使用透過 Microsoft Store 所發行 app 或附加元件的權利。

這些 API 是由幾個 REST 方法所構成的，而這些方法是專供開發人員用來與跨平台服務所支援的附加元件型錄搭配使用。 這些 API 可讓您進行下列行動：

-   Microsoft Store 集合 API：[查詢由使用者所擁有的產品](query-for-products.md)，以及[將某個消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)。
-   Microsoft Store 購買 API：[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，以及[變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

> [!NOTE]
> Microsoft Store 集合 API 及購買 API 會使用 Azure Active Directory (Azure AD) 驗證來存取客戶的擁有權資訊。 若要使用這些 API，您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域管理員](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。

## <a name="overview"></a>概觀

下列步驟描述使用 Microsoft Store 集合 API 和購買 API 的端對端程序：

1.  [Configure an application in Azure AD](#step-1).
2.  [Associate your Azure AD application ID with your app in Partner Center](#step-2).
3.  在您的服務中，[建立代表您發行者身分識別的 Azure AD 存取權杖](#step-3)。
4.  In your client Windows app, [create a Microsoft Store ID key](#step-4) that represents the identity of the current user, and pass this key back to your service.
5.  在您擁有必要的 Azure AD 存取權杖和 Microsoft Store 識別碼金鑰之後，[從您的服務呼叫 Microsoft Store 集合 API 或購買 API](#step-5)。

This end-to-end process involves two software components that perform different tasks:

* **Your service**. This is an application that runs securely in the context of your business environment, and it can be implemented using any development platform you choose. Your service is responsible for creating the Azure AD access tokens needed for the scenario and for calling the REST URIs for the Microsoft Store collection API and purchase API.
* **Your client Windows app**. This is the app for which you want to access and manage customer entitlement information (including add-ons for the app). This app is responsible for creating the Microsoft Store ID keys you need to call the Microsoft Store collection API and purchase API from your service.

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>Step 1: Configure an application in Azure AD

Before you can use the Microsoft Store collection API or purchase API, you must create an Azure AD Web application, retrieve the tenant ID and application ID for the application, and generate a key. The Azure AD Web application represents the service from which you want to call the Microsoft Store collection API or purchase API. You need the tenant ID, application ID and key to generate Azure AD access tokens that you need to call the API.

> [!NOTE]
> 您只需要執行本節中的工作一次。 After you update your Azure AD application manifest and you have your tenant ID, application ID and client secret, you can reuse these values any time you need to create a new Azure AD access token.

1.  If you haven't done so already, follow the instructions in [Integrating Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) to register a **Web app / API** application with Azure AD.
    > [!NOTE]
    > When you register your application, you must choose **Web app / API** as the application type so that you can retrieve a key (also called a *client secret*) for your application. 為了呼叫 Microsoft Store 集合 API 或購買 API，當您在稍後的步驟中向 Azure AD 要求存取權杖時，必須提供用戶端密碼。

2.  In the [Azure Management Portal](https://portal.azure.com/), navigate to **Azure Active Directory**. Select your directory, click **App registrations** in the left navigation pane, and then select your application.
3.  You are taken to the application's main registration page. On this page, copy the **Application ID** value for use later.
4.  Create a key that you will need later (this is all called a *client secret*). In the left pane, click **Settings** and then **Keys**. On this page, complete the steps to [create a key](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis). Copy this key for later use.
5.  Add several required audience URIs to your [application manifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest). In the left pane, click **Manifest**. Click **Edit**, replace the `"identifierUris"` section with the following text, and then click **Save**.

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    這些字串代表您應用程式所支援的對象。 在後續的步驟中，您將會建立與這些對象值中每個相關聯的 Azure AD 存取權杖。

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>Step 2: Associate your Azure AD application ID with your client app in Partner Center

Before you can use the Microsoft Store collection API or purchase API to configure the ownership and purchases for your app or add-on, you must associate your Azure AD application ID with the app (or the app that contains the add-on) in Partner Center.

> [!NOTE]
> 您只需要執行此工作一次。

1.  Sign in to [Partner Center](https://partner.microsoft.com/dashboard) and select your app.
2.  Go to the **Services** &gt; **Product collections and purchases** page and enter your Azure AD application ID into one of the available **Client ID** fields.

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>步驟 3：建立 Azure AD 存取權杖

您的服務必須先建立數個不同的 Azure AD 存取權杖來代表您的發行者身分識別，才能擷取 Microsoft Store 識別碼金鑰，或是呼叫 Microsoft Store 集合 API 或購買 API。 每個權杖與不同的 API 搭配使用。 每個權杖的存留期是 60 分鐘，且您可以在權杖到期後更新權杖。

> [!IMPORTANT]
> 請僅在服務內 (而非應用程式中) 建立 Azure AD 存取權杖。 如果該存取權杖被傳送到您的應用程式，您的用戶端密碼就可能遭竊。

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>了解不同的權杖和對象 URI

根據您想要在 Microsoft Store 集合 API 或購買 API 中呼叫的方法，您必須建立兩個或三個不同權杖。 每個存取權杖與不同的對象 URI 相關聯 (這些是先前加入到 Azure AD 應用程式資訊清單 `"identifierUris"` 區段的相同 URI)。

  * 在所有情況中，您都必須使用 `https://onestore.microsoft.com` 對象 URI 建立權杖。 在稍後的步驟，您將這個權杖傳遞到 Microsoft Store 集合 API 或購買 API 中方法的 **Authorization** 標頭。
      > [!IMPORTANT]
      > 請只搭配安全地儲存在您服務中的存取權杖來使用 `https://onestore.microsoft.com` 對象。 在您服務以外的地方公開與此對象搭配的存取權杖，可能會讓您的服務容易遭受重新執行攻擊。

  * 如果您想要呼叫 Microsoft Store 集合 API 中的方法，以[查詢由使用者所擁有的產品](query-for-products.md)或[將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)，您必須也要使用 `https://onestore.microsoft.com/b2b/keys/create/collections` 對象 URI 建立權杖。 在稍後的步驟中，您將這個權杖傳遞到 Windows SDK 中的用戶端方法，來要求可搭配 Microsoft Store 集合 API 使用的 Microsoft Store 識別碼金鑰。

  * 如果您想要呼叫 Microsoft Store 購買 API 的方法，[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，或[變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)，您必須也要使用 `https://onestore.microsoft.com/b2b/keys/create/purchase` 對象 URI 建立權杖 在稍後的步驟中，您將這個權杖傳遞到 Windows SDK 中的用戶端方法，來要求可搭配 Microsoft Store 購買 API 使用的 Microsoft Store 識別碼金鑰。

<span />

### <a name="create-the-tokens"></a>建立權杖

若要建立存取權杖，請在您的服務中使用 OAuth 2.0 API，方法是依照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指引將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

請針對每個權杖指定下列參數資料：

* For the *client\_id* and *client\_secret* parameters, specify the application ID and the client secret for your application that you retrieved from the [Azure Management Portal](https://portal.azure.com/). 為了要建立 Microsoft Store 集合 API 或購買 API 所需驗證層級的存取權杖，這兩個參數都是必要的。

* 對於 *resource* 參數，指定[上一節](#access-tokens)中列出的其中一個對象 URI，視您要建立的存取權杖類型而定。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。 如需有關存取權杖結構的詳細資訊，請參閱[支援的權杖和宣告類型](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)。

<span id="step-4"/>

## <a name="step-4-create-a-microsoft-store-id-key"></a>步驟 4：建立 Microsoft Store 識別碼金鑰

您的 App 必須先建立 Microsoft Store 識別碼金鑰並傳送至您的服務，才能讓您呼叫 Microsoft Store 集合 API 或購買 API 中的方法。 此金鑰是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 如需該金鑰中宣告的詳細資訊，請參閱 [Microsoft Store 識別碼金鑰中的宣告](#claims-in-a-microsoft-store-id-key)。

目前，建立 Microsoft Store 識別碼金鑰的唯一方式是從 app 的用戶端程式碼呼叫通用 Windows 平台 (UWP) API。 產生的金鑰代表目前在裝置上已登入 Microsoft Store 的使用者的身分識別。

> [!NOTE]
> 每個 Microsoft Store 識別碼金鑰的有效期皆為 90 天。 金鑰到期之後，您可以[更新金鑰](renew-a-windows-store-id-key.md)。 我們建議您更新自己的 Microsoft Store 識別碼金鑰，而不是建立新的。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-collection-api"></a>若要建立 Microsoft Store 集合 API 的 Microsoft Store 識別碼金鑰

請依照下列步驟來建立可搭配 Microsoft Store 集合 API 使用的 Microsoft Store 識別碼金鑰，以[查詢由使用者所擁有的產品](query-for-products.md)或[將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)。

1.  將擁有對象 URI 值 `https://onestore.microsoft.com/b2b/keys/create/collections` 的 Azure AD 存取權杖從您的服務傳遞到用戶端 App。 這是您[先前在步驟 3](#step-3) 中建立的權杖。

2.  在您的應用程式程式碼中，呼叫其中一個方法來擷取 Microsoft Store 識別碼金鑰：

  * 如果您的 app 使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 類別來管理在應用程式內購買，請使用 [StoreContext.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 命名空間中的 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerCollectionsIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) 方法。

    將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 If you maintain anonymous user IDs in the context of services that you manage as the publisher of the current app, you can also pass a user ID to the *publisherUserId* parameter to associate the current user with the new Microsoft Store ID key (the user ID will be embedded in the key). Otherwise, if you don't need to associate a user ID with the Microsoft Store ID key, you can pass any string value to the *publisherUserId* parameter.

3.  在您的應用程式成功建立「Microsoft Store 識別碼」金鑰之後，將金鑰傳遞回您的服務。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>若要建立 Microsoft Store 購買 API 的 Microsoft Store 識別碼金鑰

請依照下列步驟來建立可搭配 Microsoft Store 購買集合 API 使用的 Microsoft Store 識別碼金鑰，以[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，[或變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

1.  將擁有對象 URI 值 `https://onestore.microsoft.com/b2b/keys/create/purchase` 的 Azure AD 存取權杖從您的服務傳遞到用戶端 App。 這是您[先前在步驟 3](#step-3) 中建立的權杖。

2.  在您的應用程式程式碼中，呼叫其中一個方法來擷取 Microsoft Store 識別碼金鑰：

  * 如果您的 app 使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 類別來管理 App 內購買，請使用 [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 命名空間中的 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) 方法。

    將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 If you maintain anonymous user IDs in the context of services that you manage as the publisher of the current app, you can also pass a user ID to the *publisherUserId* parameter to associate the current user with the new Microsoft Store ID key (the user ID will be embedded in the key). Otherwise, if you don't need to associate a user ID with the Microsoft Store ID key, you can pass any string value to the *publisherUserId* parameter.

3.  在您的應用程式成功建立「Microsoft Store 識別碼」金鑰之後，將金鑰傳遞回您的服務。

### <a name="diagram"></a>圖表

The following diagram illustrates the process of creating a Microsoft Store ID key.

  ![Create Windows Store ID key](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>步驟 5：從您的服務呼叫 Microsoft Store 集合 API 或購買 API

在您的服務擁有可存取特定使用者的產品擁有權資訊的 Microsoft Store 識別碼之後，依照下列指示讓您的服務呼叫 Microsoft Store 集合 API 或購買 API。

* [Query for products](query-for-products.md)
* [Report consumable products as fulfilled](report-consumable-products-as-fulfilled.md)
* [Grant free products](grant-free-products.md)
* [Get subscriptions for a user](get-subscriptions-for-a-user.md)
* [Change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md)

請針對每個案例，將下列資訊傳遞給 API：

-   在要求標頭中，傳遞具有對象 URI 值 `https://onestore.microsoft.com` 的 Azure AD 存取權杖。 這是您[先前在步驟 3](#step-3) 中建立的權杖。 此權杖代表您的發行者身分識別。
-   在要求本文中，傳遞您[先前在步驟 4](#step-4) 中，從應用程式的用戶端程式碼擷取的 Microsoft Store 識別碼金鑰。 此金鑰代表您想要存取其產品擁有權資訊之使用者的身分識別。

### <a name="diagram"></a>圖表

The following diagram describes the process of calling a method in the Microsoft Store collection API or purchase API from your service.

  ![Call collections or puchase API](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Microsoft Store 識別碼金鑰中的宣告

Microsoft Store 識別碼金鑰就是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 當您使用 Base64 來解碼時，Microsoft Store 識別碼金鑰會包含下列宣告。

* `iat`:&nbsp;&nbsp;&nbsp;Identifies the time at which the key was issued. 此宣告可以用來判斷權杖的存在時間。 此值會顯示為 Epoch 時間。
* `iss`:&nbsp;&nbsp;&nbsp;Identifies the issuer. 其值與 `aud` 宣告的值相同。
* `aud`:&nbsp;&nbsp;&nbsp;Identifies the audience. 必須為下列其中一個值：`https://collections.mp.microsoft.com/v6.0/keys` 或 `https://purchase.mp.microsoft.com/v6.0/keys`。
* `exp`:&nbsp;&nbsp;&nbsp;Identifies the expiration time on or after which the key will no longer be accepted for processing anything except for renewing keys. 此宣告的值會顯示為 Epoch 時間。
* `nbf`:&nbsp;&nbsp;&nbsp;Identifies the time at which the token will be accepted for processing. 此宣告的值會顯示為 Epoch 時間。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`:&nbsp;&nbsp;&nbsp;The client ID that identifies the developer.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`:&nbsp;&nbsp;&nbsp;An opaque payload (encrypted and Base64 encoded) that contains information that is intended only for use by Microsoft Store services.
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`:&nbsp;&nbsp;&nbsp;A user ID that identifies the current user in the context of your services. 此值與您傳遞給[用來建立金鑰的方法](#step-4)之選擇性 *publisherUserId* 參數的值相同。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`:&nbsp;&nbsp;&nbsp;The URI that you can use to renew the key.

以下是已解碼的 Microsoft Store 識別碼金鑰標頭範例。

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

以下是已解碼的 Microsoft Store 識別碼金鑰宣告組範例。

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>相關主題

* [Query for products](query-for-products.md)
* [Report consumable products as fulfilled](report-consumable-products-as-fulfilled.md)
* [Grant free products](grant-free-products.md)
* [Get subscriptions for a user](get-subscriptions-for-a-user.md)
* [Change the billing state of a subscription for a user](change-the-billing-state-of-a-subscription-for-a-user.md)
* [Renew a Microsoft Store ID key](renew-a-windows-store-id-key.md)
* [Integrating Applications with Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app)
* [Understanding the Azure Active Directory application manifest]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [Supported Token and Claim Types](https://docs.microsoft.com/azure/active-directory/develop/id-tokens)
