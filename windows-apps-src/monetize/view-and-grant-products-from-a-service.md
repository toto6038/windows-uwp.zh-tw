---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: 如果您有應用程式及附加元件的型錄，就能使用「Microsoft Store 集合 API」及「Microsoft Store 購買 API」，從您的服務存取這些產品的擁有權資訊。
title: 管理服務的產品權利
ms.author: mcleans
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, Microsoft Store 集合 API, Microsoft Store 購買 API, 檢視產品, 授與產品
ms.localizationpriority: medium
ms.openlocfilehash: 3a0766830bc2110dffcf5baf886e8ccb98ac6446
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2018
ms.locfileid: "4209830"
---
# <a name="manage-product-entitlements-from-a-service"></a>管理服務的產品權利

如果您有應用程式及附加元件的型錄，就能使用「*Microsoft Store 集合 API*」及「*Microsoft Store 購買 API*」，從您的服務存取這些產品的權利資訊。 *「權利」* 表示客戶使用透過 Microsoft Store 所發行 app 或附加元件的權利。

這些 API 是由幾個 REST 方法所構成的，而這些方法是專供開發人員用來與跨平台服務所支援的附加元件型錄搭配使用。 這些 API 可讓您進行下列行動：

-   Microsoft Store 集合 API：[查詢由使用者所擁有的產品](query-for-products.md)，以及[將某個消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)。
-   Microsoft Store 購買 API：[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，以及[變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

> [!NOTE]
> Microsoft Store 集合 API 及購買 API 會使用 Azure Active Directory (Azure AD) 驗證來存取客戶的擁有權資訊。 若要使用這些 API，您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域管理員](http://go.microsoft.com/fwlink/?LinkId=746654)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。

## <a name="overview"></a>概觀

下列步驟描述使用 Microsoft Store 集合 API 和購買 API 的端對端程序：

1.  [設定 Azure AD 中的應用程式](#step-1)。
2.  [將您的 Azure AD 應用程式識別碼與您的 app 在 Windows 開發人員中心儀表板中建立關聯](#step-2)。
3.  在您的服務中，[建立代表您發行者身分識別的 Azure AD 存取權杖](#step-3)。
4.  在您用戶端的 Windows 應用程式，[建立 Microsoft Store 識別碼金鑰](#step-4)，代表目前的使用者，並傳遞此金鑰的身分識別回您的服務。
5.  在您擁有必要的 Azure AD 存取權杖和 Microsoft Store 識別碼金鑰之後，[從您的服務呼叫 Microsoft Store 集合 API 或購買 API](#step-5)。

這個端對端程序涉及執行不同的工作的兩個軟體元件：

* **您的服務**。 這是安全地執行您的企業環境的內容中的應用程式，而且可以實作使用您選擇的任何開發平台。 您的服務負責建立 Azure AD 存取權杖所需的案例和適用於 REST Uri 呼叫 Microsoft Store 集合 API 及購買 API。
* **您用戶端的 Windows 應用程式**。 這是您要存取和管理客戶的權利資訊 （包括應用程式的附加元件） 的 app。 此應用程式會負責建立您要呼叫 Microsoft Store 集合 API 及購買 API，從您的服務的 Microsoft Store 識別碼金鑰。

<span id="step-1"/>

## <a name="step-1-configure-an-application-in-azure-ad"></a>步驟 1： 在 Azure AD 中設定應用程式

您可以使用 「 Microsoft Store 集合 API 或購買 API 之前，您必須建立 Azure AD Web 應用程式、 擷取的租用戶識別碼和應用程式識別碼，讓應用程式，並產生金鑰。 Azure AD Web 應用程式代表您想要呼叫 Microsoft Store 集合 API 或購買 API 的服務。 您需要租用戶識別碼、 應用程式識別碼和金鑰，以產生您需要呼叫 API 的 Azure AD 存取權杖。

> [!NOTE]
> 您只需要執行本節中的工作一次。 更新您的 Azure AD 應用程式資訊清單，而且您有您的租用戶識別碼、 應用程式識別碼和用戶端密碼之後，您可以重複使用這些值的每當您必須建立新的 Azure AD 存取權杖。

1.  如果您還沒有這麼做，請依照[整合應用程式與 Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)中的指示來登錄**Web 應用程式 / API**與 Azure AD 應用程式。
    > [!NOTE]
    > 當您註冊您的應用程式時，您必須選擇**Web 應用程式 / API**在應用程式輸入，讓您可以擷取您的應用程式的金鑰 （也稱為 「 用*戶端密碼*」）。 為了呼叫 Microsoft Store 集合 API 或購買 API，當您在稍後的步驟中向 Azure AD 要求存取權杖時，必須提供用戶端密碼。

2.  在[Azure 管理入口網站](https://portal.azure.com/)中，瀏覽至**Azure Active Directory**。 選取您的目錄，在左側瀏覽窗格中，按一下 [**應用程式註冊**，然後選取您的應用程式。
3.  您被引導至應用程式的主要註冊頁面。 在此頁面上，將複製**應用程式識別碼**值，以供稍後使用。
4.  建立之後，您將需要的金鑰 （這呼叫所有用*戶端密碼*）。 在左窗格中，按一下 [**設定**]，然後按一下 [**機碼**。 在此頁面上，完成步驟，以[建立金鑰](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-application-credentials-or-permissions-to-access-web-apis)。 複製這個金鑰，以供稍後使用。
5.  將數個必要的對象 Uri 新增到您的[應用程式資訊清單](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)。 在左窗格中，按一下 [**資訊清單**。 按一下 [**編輯**、 取代`"identifierUris"`以下列文字，區段，然後再按一下 [**儲存**。

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    這些字串代表您應用程式所支援的對象。 在後續的步驟中，您將會建立與這些對象值中每個相關聯的 Azure AD 存取權杖。

<span id="step-2"/>

## <a name="step-2-associate-your-azure-ad-application-id-with-your-client-app-in-windows-dev-center"></a>步驟 2： 您的 Azure AD 應用程式識別碼關聯至您的用戶端應用程式，在 Windows 開發人員中心

您可以使用 「 Microsoft Store 集合 API 或購買 API 」 來設定的擁有權和您的應用程式或附加元件購買之前，您必須產生您的 Azure AD 應用程式識別碼與應用程式 （或包含附加元件的應用程式） 關聯在開發人員中心儀表板。

> [!NOTE]
> 您只需要執行此工作一次。

1.  登入[開發人員中心儀表板](https://dev.windows.com/overview)，然後選取您的應用程式。
2.  移至**服務** &gt; **產品集合與購買**頁面，並在其中一個可用的**用戶端識別碼**欄位中輸入您的 Azure AD 應用程式識別碼。

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

若要建立存取權杖，請在您的服務中使用 OAuth 2.0 API，方法是依照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service)中的指引將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

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

* *Client\_id*與*client\_secret*參數，指定應用程式識別碼和用戶端密碼為您從[Azure 管理入口網站](http://manage.windowsazure.com)中擷取的應用程式。 為了要建立 Microsoft Store 集合 API 或購買 API 所需驗證層級的存取權杖，這兩個參數都是必要的。

* 對於 *resource* 參數，指定[上一節](#access-tokens)中列出的其中一個對象 URI，視您要建立的存取權杖類型而定。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。 如需有關存取權杖結構的詳細資訊，請參閱[支援的權杖和宣告類型](http://go.microsoft.com/fwlink/?LinkId=722501)。

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

    將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 如果您負責維護服務，您管理與目前應用程式的 「 發行者 」 的內容中的匿名使用者識別碼，您也可以傳遞的使用者識別碼來*publisherUserId*參數，以將目前的使用者與新的 Microsoft Store 識別碼金鑰 （使用者識別碼將會 em 產生關聯索引鍵中 bedded)。 否則，如果您不需要的使用者識別碼關聯至 Microsoft Store 識別碼金鑰，您可以傳遞任何字串值來*publisherUserId*參數。

3.  在您的應用程式成功建立「Microsoft Store 識別碼」金鑰之後，將金鑰傳遞回您的服務。

<span />

### <a name="to-create-a-microsoft-store-id-key-for-the-microsoft-store-purchase-api"></a>若要建立 Microsoft Store 購買 API 的 Microsoft Store 識別碼金鑰

請依照下列步驟來建立可搭配 Microsoft Store 購買集合 API 使用的 Microsoft Store 識別碼金鑰，以[將免費產品授與使用者](grant-free-products.md)、[取得使用者的訂閱](get-subscriptions-for-a-user.md)，[或變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

1.  將擁有對象 URI 值 `https://onestore.microsoft.com/b2b/keys/create/purchase` 的 Azure AD 存取權杖從您的服務傳遞到用戶端 App。 這是您[先前在步驟 3](#step-3) 中建立的權杖。

2.  在您的應用程式程式碼中，呼叫其中一個方法來擷取 Microsoft Store 識別碼金鑰：

  * 如果您的 app 使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 [StoreContext](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 類別來管理 App 內購買，請使用 [StoreContext.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getcustomerpurchaseidasync) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 命名空間中的 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerPurchaseIdAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.getcustomerpurchaseidasync) 方法。

    將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 如果您負責維護服務，您管理與目前應用程式的 「 發行者 」 的內容中的匿名使用者識別碼，您也可以傳遞的使用者識別碼來*publisherUserId*參數，以將目前的使用者與新的 Microsoft Store 識別碼金鑰 （使用者識別碼將會 em 產生關聯索引鍵中 bedded)。 否則，如果您不需要的使用者識別碼關聯至 Microsoft Store 識別碼金鑰，您可以傳遞任何字串值來*publisherUserId*參數。

3.  在您的應用程式成功建立「Microsoft Store 識別碼」金鑰之後，將金鑰傳遞回您的服務。

### <a name="diagram"></a>圖表

下圖說明建立 Microsoft Store 識別碼金鑰的程序。

  ![建立 Windows 市集識別碼金鑰](images/b2b-1.png)

<span id="step-5"/>

## <a name="step-5-call-the-microsoft-store-collection-api-or-purchase-api-from-your-service"></a>步驟 5：從您的服務呼叫 Microsoft Store 集合 API 或購買 API

在您的服務擁有可存取特定使用者的產品擁有權資訊的 Microsoft Store 識別碼之後，依照下列指示讓您的服務呼叫 Microsoft Store 集合 API 或購買 API。

* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)
* [取得使用者訂閱](get-subscriptions-for-a-user.md)
* [變更使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)

請針對每個案例，將下列資訊傳遞給 API：

-   在要求標頭中，傳遞具有對象 URI 值 `https://onestore.microsoft.com` 的 Azure AD 存取權杖。 這是您[先前在步驟 3](#step-3) 中建立的權杖。 此權杖代表您的發行者身分識別。
-   在要求本文中，傳遞您[先前在步驟 4](#step-4) 中，從應用程式的用戶端程式碼擷取的 Microsoft Store 識別碼金鑰。 此金鑰代表您想要存取其產品擁有權資訊之使用者的身分識別。

### <a name="diagram"></a>圖表

下圖說明 Microsoft Store 集合 API 或購買 API 中呼叫的方法，從您的服務程的序。

  ![呼叫集合或購買 API](images/b2b-2.png)

## <a name="claims-in-a-microsoft-store-id-key"></a>Microsoft Store 識別碼金鑰中的宣告

Microsoft Store 識別碼金鑰就是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 當您使用 Base64 來解碼時，Microsoft Store 識別碼金鑰會包含下列宣告。

* `iat`：&nbsp;&nbsp;&nbsp;能識別金鑰發出的時間。 此宣告可以用來判斷權杖的存在時間。 此值會顯示為 Epoch 時間。
* `iss`：&nbsp;&nbsp;&nbsp;能識別簽發者。 其值與 `aud` 宣告的值相同。
* `aud`：&nbsp;&nbsp;&nbsp;能識別對象。 必須為下列其中一個值：`https://collections.mp.microsoft.com/v6.0/keys` 或 `https://purchase.mp.microsoft.com/v6.0/keys`。
* `exp`：&nbsp;&nbsp;&nbsp;能識別金鑰的到期時間；金鑰在到期當天或之後，除了更新金鑰之外，系統就無法再接受該金鑰來處理任何工作。 此宣告的值會顯示為 Epoch 時間。
* `nbf`：&nbsp;&nbsp;&nbsp;能識別系統將接受要處理之權杖的時間。 此宣告的值會顯示為 Epoch 時間。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`：&nbsp;&nbsp;&nbsp;能識別開發人員的用戶端識別碼。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`：&nbsp;&nbsp;&nbsp;不透明的承載 (已加密，且已使用 Base64 編碼)，包含僅供 Microsoft Store 服務使用的資訊。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`：&nbsp;&nbsp;&nbsp;能識別您服務內容中目前使用者的使用者識別碼。 此值與您傳遞給[用來建立金鑰的方法](#step-4)之選擇性 *publisherUserId* 參數的值相同。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`：&nbsp;&nbsp;&nbsp;可用來更新金鑰的 URI。

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
* [整合應用程式與 Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [了解 Azure Active Directory 應用程式資訊清單]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [支援的權杖和宣告類型](http://go.microsoft.com/fwlink/?LinkId=722501)
