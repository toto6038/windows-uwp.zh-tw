---
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: 如果您有應用程式及附加元件的型錄，就能使用「Microsoft Store 集合 API」及「Microsoft Store 購買 API」，從您的服務存取這些產品的擁有權資訊。
title: 管理服務的產品權利
ms.date: 01/21/2021
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 集合 API, Microsoft Store 購買 API, 檢視產品, 授與產品
ms.localizationpriority: medium
ms.openlocfilehash: 7674a9b966510d914850e1fc8b2c8ca531f64a20
ms.sourcegitcommit: 069f5ab4be85a7d638fc2a426afaed824e5dfeae
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2021
ms.locfileid: "98668735"
---
# <a name="manage-product-entitlements-from-a-service"></a>管理服務的產品權利

如果您有應用程式及附加元件的型錄，就能使用「*Microsoft Store 集合 API*」及「*Microsoft Store 購買 API*」，從您的服務存取這些產品的權利資訊。 *「權利」* 表示客戶使用透過 Microsoft Store 所發行 app 或附加元件的權利。

這些 API 是由幾個 REST 方法所構成的，而這些方法是專供開發人員用來與跨平台服務所支援的附加元件型錄搭配使用。 這些 API 可讓您進行下列行動：

-   Microsoft Store 集合 API：[查詢由使用者所擁有的產品](query-for-products.md)，以及[將某個消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)。
-   Microsoft Store 購買 API：[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，以及[變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

> [!NOTE]
> Microsoft Store 集合 API 及購買 API 會使用 Azure Active Directory (Azure AD) 驗證來存取客戶的擁有權資訊。 若要使用這些 API，您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域管理員](/azure/active-directory/users-groups-roles/directory-assign-admin-roles)權限。 如果您已經使用 Microsoft 365 或 Microsoft 的其他商務服務，則您已經有 Azure AD 目錄。

## <a name="overview"></a>概觀

下列步驟描述使用 Microsoft Store 集合 API 和購買 API 的端對端程序：

1.  [在 Azure AD 中設定應用程式](#step-1)。
2.  [在合作夥伴中心中，將您的 Azure AD 應用程式識別碼與應用](#step-2)程式產生關聯。
3.  在您的服務中，[建立代表您發行者身分識別的 Azure AD 存取權杖](#step-3)。
4.  在您的用戶端 Windows 應用程式中， [建立 MICROSOFT STORE 識別碼金鑰](#step-4) 以代表目前使用者的身分識別，並將此金鑰傳遞回您的服務。

此端對端程式牽涉到兩個執行不同工作的軟體元件：

* **您的服務**。 這是在您商務環境環境中安全執行的應用程式，而且可以使用您選擇的任何開發平臺來執行。 您的服務會負責建立案例所需的 Azure AD 存取權杖，以及呼叫 Microsoft Store 收集 API 和購買 API 的 REST Uri。
* **您的用戶端 Windows 應用程式**。 這是您要用來存取和管理客戶權利資訊 (包括應用程式) 附加元件的應用程式。 此應用程式負責建立 Microsoft Store 識別碼金鑰，您必須從您的服務呼叫 Microsoft Store 收集 API 和購買 API。

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>步驟1：在 Azure AD 中設定應用程式

您必須先建立 Azure AD Web 應用程式、取出應用程式的租使用者識別碼和應用程式識別碼，並產生金鑰，才能使用 Microsoft Store 收集 API 或購買 API。 Azure AD Web 應用程式代表您要從中呼叫 Microsoft Store 收集 API 或購買 API 的服務。 您需要租使用者識別碼、應用程式識別碼和金鑰，才能產生您需要呼叫 API Azure AD 存取權杖。

1.  如果您尚未這麼做，請遵循 [整合應用程式與 Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications) 中的指示，向 Azure AD 註冊 **Web 應用程式/API** 應用程式。
    > [!NOTE]
    > 當您註冊應用程式時，您必須選擇 [ **Web 應用程式/API** ] 作為 [應用程式類型]，讓您可以取出金鑰 (也稱為應用程式的 *用戶端密碼*) 。 為了呼叫 Microsoft Store 集合 API 或購買 API，當您在稍後的步驟中向 Azure AD 要求存取權杖時，必須提供用戶端密碼。

2.  在 [Azure 管理入口網站](https://portal.azure.com/)中，流覽至 **Azure Active Directory**。 選取您的目錄，按一下左方流覽窗格中的 [ **應用程式註冊** ]，然後選取您的應用程式。
3.  您會進入應用程式的主要註冊頁面。 在此頁面上，複製 [ **應用程式識別碼** ] 值以供稍後使用。
4.  建立稍後將需要的金鑰 (這全都稱為 *用戶端密碼*) 。 在左窗格中，按一下 [ **設定** ]，然後按一下 [機 **碼**]。 在此頁面上，完成 [建立金鑰](/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis)的步驟。 複製此金鑰以供稍後使用。

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-partner-center"></a>步驟2：將您的 Azure AD 應用程式識別碼與合作夥伴中心中的用戶端應用程式產生關聯

在您可以使用 Microsoft Store 收集 API 或購買 API 來設定應用程式或附加元件的擁有權和購買之前，您必須將 Azure AD 應用程式識別碼與應用程式建立關聯 (或合作夥伴中心中包含附加元件) 的應用程式。

> [!NOTE]
> 您只需要執行此工作一次。 當您擁有租使用者識別碼、應用程式識別碼和用戶端密碼之後，您可以在需要建立新的 Azure AD 存取權杖時重複使用這些值。

1.  登入 [合作夥伴中心](https://partner.microsoft.com/dashboard) ，然後選取您的應用程式。
2.  移至 [ **服務** &gt; **產品集合和購買** ] 頁面，然後在其中一個可用的 [ **用戶端識別碼** ] 欄位中輸入您的 Azure AD 應用程式識別碼。

<span id="step-3"/>

## <a name="step-3-create-azure-ad-access-tokens"></a>步驟 3：建立 Azure AD 存取權杖

您的服務必須先建立數個不同的 Azure AD 存取權杖來代表您的發行者身分識別，才能擷取 Microsoft Store 識別碼金鑰，或是呼叫 Microsoft Store 集合 API 或購買 API。 每個權杖與不同的 API 搭配使用。 每個權杖的存留期是 60 分鐘，且您可以在權杖到期後更新權杖。

> [!IMPORTANT]
> 請僅在服務內 (而非應用程式中) 建立 Azure AD 存取權杖。 如果該存取權杖被傳送到您的應用程式，您的用戶端密碼就可能遭竊。

<span id="access-tokens" />

### <a name="understanding-the-different-tokens-and-audience-uris"></a>了解不同的權杖和對象 URI

根據您想要在 Microsoft Store 集合 API 或購買 API 中呼叫的方法，您必須建立兩個或三個不同權杖。 每個存取權杖都會與不同的物件 URI 相關聯。

  * 在所有情況中，您都必須使用 `https://onestore.microsoft.com` 對象 URI 建立權杖。 在稍後的步驟，您將這個權杖傳遞到 Microsoft Store 集合 API 或購買 API 中方法的 **Authorization** 標頭。
      > [!IMPORTANT]
      > 請只搭配安全地儲存在您服務中的存取權杖來使用 `https://onestore.microsoft.com` 對象。 在您服務以外的地方公開與此對象搭配的存取權杖，可能會讓您的服務容易遭受重新執行攻擊。

  * 如果您想要呼叫 Microsoft Store 集合 API 中的方法，以[查詢由使用者所擁有的產品](query-for-products.md)或[將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)，您必須也要使用 `https://onestore.microsoft.com/b2b/keys/create/collections` 對象 URI 建立權杖。 在稍後的步驟中，您將這個權杖傳遞到 Windows SDK 中的用戶端方法，來要求可搭配 Microsoft Store 集合 API 使用的 Microsoft Store 識別碼金鑰。

  * 如果您想要呼叫 Microsoft Store 購買 API 的方法，[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，或[變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)，您必須也要使用 `https://onestore.microsoft.com/b2b/keys/create/purchase` 對象 URI 建立權杖 在稍後的步驟中，您將這個權杖傳遞到 Windows SDK 中的用戶端方法，來要求可搭配 Microsoft Store 購買 API 使用的 Microsoft Store 識別碼金鑰。

<span />

### <a name="create-the-tokens"></a>建立權杖

若要建立存取權杖，請在您的服務中使用 OAuth 2.0 API，方法是依照[使用用戶端認證的服務對服務呼叫](/azure/active-directory/azuread-dev/v1-oauth2-client-creds-grant-flow)中的指引將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

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

* 若為 *用戶端 \_ 識別碼* 和 *用戶端 \_ 秘密* 參數，請為您從 [Azure 管理入口網站](https://portal.azure.com/)取出的應用程式指定應用程式識別碼和用戶端密碼。 為了要建立 Microsoft Store 集合 API 或購買 API 所需驗證層級的存取權杖，這兩個參數都是必要的。

* 對於 *resource* 參數，指定 [上一節](#access-tokens)中列出的其中一個對象 URI，視您要建立的存取權杖類型而定。

存取權杖到期之後，您可以按照[這裡](/azure/active-directory/azuread-dev/v1-protocols-oauth-code#refreshing-the-access-tokens)的指示，重新整理權杖。 如需有關存取權杖結構的詳細資訊，請參閱[支援的權杖和宣告類型](/azure/active-directory/develop/id-tokens)。

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

  * 如果您的 app 使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空間中的 [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) 類別來管理在應用程式內購買，請使用 [StoreContext.GetCustomerCollectionsIdAsync](/uwp/api/windows.services.store.storecontext.getcustomercollectionsidasync) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空間中的 [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerCollectionsIdAsync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomercollectionsidasync) 方法。

    將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 如果您在做為目前應用程式發行者的服務內容中維護匿名使用者識別碼，您也可以將使用者識別碼傳遞給 *publisherUserId* 參數，以將目前使用者與新的 Microsoft Store 識別碼金鑰建立關聯 (使用者識別碼將內嵌在金鑰) 中。 否則，如果您不需要將使用者識別碼與 Microsoft Store 識別碼索引鍵建立關聯，您可以將任何字串值傳遞給 *publisherUserId* 參數。

3.  在您的應用程式成功建立「Microsoft Store 識別碼」金鑰之後，將金鑰傳遞回您的服務。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>若要建立 Microsoft Store 購買 API 的 Microsoft Store 識別碼金鑰

請依照下列步驟來建立可搭配 Microsoft Store 購買集合 API 使用的 Microsoft Store 識別碼金鑰，以[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，[或變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

1.  將擁有對象 URI 值 `https://onestore.microsoft.com/b2b/keys/create/purchase` 的 Azure AD 存取權杖從您的服務傳遞到用戶端 App。 這是您[先前在步驟 3](#step-3) 中建立的權杖。

2.  在您的應用程式程式碼中，呼叫其中一個方法來擷取 Microsoft Store 識別碼金鑰：

  * 如果您的 app 使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空間中的 [StoreContext](/uwp/api/Windows.Services.Store.StoreContext) 類別來管理 App 內購買，請使用 [StoreContext.GetCustomerPurchaseIdAsync](/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空間中的 [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerPurchaseIdAsync](/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) 方法。

    將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 如果您在做為目前應用程式發行者的服務內容中維護匿名使用者識別碼，您也可以將使用者識別碼傳遞給 *publisherUserId* 參數，以將目前使用者與新的 Microsoft Store 識別碼金鑰建立關聯 (使用者識別碼將內嵌在金鑰) 中。 否則，如果您不需要將使用者識別碼與 Microsoft Store 識別碼索引鍵建立關聯，您可以將任何字串值傳遞給 *publisherUserId* 參數。

3.  在您的應用程式成功建立「Microsoft Store 識別碼」金鑰之後，將金鑰傳遞回您的服務。

### <a name="diagram"></a>圖表

下圖說明建立 Microsoft Store 識別碼金鑰的程式。

  ![建立 Windows Store 識別碼機碼](images/b2b-1.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Microsoft Store 識別碼金鑰中的宣告

Microsoft Store 識別碼金鑰就是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 當您使用 Base64 來解碼時，Microsoft Store 識別碼金鑰會包含下列宣告。

* `iat`： &nbsp; &nbsp; &nbsp; 識別發出金鑰的時間。 此宣告可以用來判斷權杖的存在時間。 此值會顯示為 Epoch 時間。
* `iss`： &nbsp; &nbsp; &nbsp; 識別簽發者。 其值與 `aud` 宣告的值相同。
* `aud`： &nbsp; &nbsp; &nbsp; 識別物件。 必須為下列其中一個值：`https://collections.mp.microsoft.com/v6.0/keys` 或 `https://purchase.mp.microsoft.com/v6.0/keys`。
* `exp`： &nbsp; &nbsp; &nbsp; 識別到期時間或之後，在這段時間之後，將不再接受金鑰的處理，除了更新金鑰以外。 此宣告的值會顯示為 Epoch 時間。
* `nbf`： &nbsp; &nbsp; &nbsp; 識別將接受標記以進行處理的時間。 此宣告的值會顯示為 Epoch 時間。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`： &nbsp; &nbsp; &nbsp; 識別開發人員的用戶端識別碼。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`： &nbsp; &nbsp; &nbsp; 透明承載 (加密和 Base64 編碼的) ，其中包含僅供 Microsoft Store 服務使用的資訊。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`： &nbsp; &nbsp; &nbsp; 在您的服務內容中識別目前使用者的使用者識別碼。 此值與您傳遞給 [用來建立金鑰的方法](#step-4)之選擇性 *publisherUserId* 參數的值相同。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`： &nbsp; &nbsp; &nbsp; 您可以用來更新金鑰的 URI。

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

* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)
* [取得使用者訂閱](get-subscriptions-for-a-user.md)
* [變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)
* [更新 Microsoft Store 識別碼金鑰](renew-a-windows-store-id-key.md)
* [整合應用程式與 Azure Active Directory](/azure/active-directory/develop/quickstart-register-app)
* [了解 Azure Active Directory 應用程式資訊清單]( https://go.microsoft.com/fwlink/?LinkId=722500)
* [支援的權杖和宣告類型](/azure/active-directory/develop/id-tokens)
