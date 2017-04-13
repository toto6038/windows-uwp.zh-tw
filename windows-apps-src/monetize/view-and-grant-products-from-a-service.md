---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "如果您有應用程式及附加元件的型錄，就能使用「Windows 市集集合 API」及「Windows 市集購買 API」，來從您的服務存取這些產品的擁有權資訊。"
title: "管理服務的產品權利"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, Windows 市集集合 API, Windows 市集購買 API, 檢視產品, 授與產品"
ms.openlocfilehash: 1f5930a9917933937a1a0103fe118a2ccdf2d47f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-product-entitlements-from-a-service"></a>管理服務的產品權利

如果您有應用程式及附加元件 (又稱為應用程式內產品或 IAP) 的型錄，就能使用*「Windows 市集集合 API」*及*「Windows 市集購買 API」*，來從您的服務存取這些產品的權利資訊。 *「權利」*表示客戶使用透過 Windows 市集所發行 app 或附加元件的權利。

這些 API 是由幾個 REST 方法所構成的，而這些方法是專供開發人員用來與跨平台服務所支援的附加元件型錄搭配使用。 這些 API 可讓您進行下列行動：

-   Windows 市集集合 API：[查詢由使用者所擁有的產品](query-for-products.md)，以及[將某個消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)。
-   Windows 市集購買 API：[將免費產品授與使用者](grant-free-products.md)。

>**注意**&nbsp;&nbsp;Windows 市集集合 API 及購買 API 會使用 Azure Active Directory (Azure AD) 驗證來存取客戶的擁有權資訊。 若要使用這些 API，您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域管理員](http://go.microsoft.com/fwlink/?LinkId=746654)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。

## <a name="overview"></a>概觀

下列步驟描述使用 Windows 市集集合 API 和購買 API 的端對端程序：

1.  [在 Azure AD 中設定 Web 應用程式](#step-1)。
2.  [在 Windows 開發人員中心儀表板中，讓您的 Azure AD 用戶端識別碼與應用程式建立關聯](#step-2)。
3.  在您的服務中，[建立代表您發行者身分識別的 Azure AD 存取權杖](#step-3)。
4.  在您 Windows 應用程式的用戶端程式碼中，[建立代表目前使用者身分識別的 Windows 市集識別碼金鑰](#step-4)，然後把該 Windows 市集識別碼金鑰傳回您的服務。
5.  在您擁有必要的 Azure AD 存取權杖和 Windows 市集識別碼金鑰之後，[從您的服務呼叫 Windows 市集集合 API 或購買 API](#step-5)。

下列各節提供以上每個步驟的詳細資料。

<span id="step-1"/>
## <a name="step-1-configure-a-web-application-in-azure-ad"></a>步驟 1：在 Azure AD 中設定 Web 應用程式

在使用 Windows 市集集合 API 或購買 API 之前，您必須建立 Azure AD Web 應用程式、擷取應用程式的租用戶識別碼與用戶端識別碼，以及產生金鑰。 Azure AD 應用程式代表您要從中呼叫 Windows 市集集合 API 或購買 API 的 App 或服務。 您需要租用戶識別碼、用戶端識別碼和金鑰，才能取得傳遞給 API 的 Azure AD 存取權杖。

>**注意**&nbsp;&nbsp;您只需要執行本節中的工作一次。 當您更新 Azure AD 應用程式資訊清單，並擁有您的租用戶識別碼、用戶端識別碼和用戶端密碼之後，需要建立新 Azure AD 存取權杖時，可以隨時重複使用這些值。

1.  依照[整合應用程式與 Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) 中的指示，將 Web 應用程式新增到 Azure AD。

    > **注意**&nbsp;&nbsp;在 **\[告訴我們您的應用程式\]** 頁面上，請確認您選擇的是 **\[Web 應用程式和/或 Web API\]**。 您必須這麼做，才能為您的應用程式擷取金鑰 (也稱為*「用戶端密碼」*)。 為了呼叫 Windows 市集集合 API 或購買 API，當您在稍後的步驟中向 Azure AD 要求存取權杖時，必須提供用戶端密碼。

2.  在 [Azure 管理入口網站](http://manage.windowsazure.com/)中，瀏覽到 **\[Active Directory\]**。 選取您的目錄，按一下頂端的 **\[應用程式\]** 索引標籤，然後選取您的應用程式。
3.  按一下 **\[設定\]** 索引標籤。 在此索引標籤上，取得您應用程式的用戶端識別碼並要求一個金鑰 (在後續步驟中稱為*「用戶端密碼」*)。
4.  按一下螢幕底部的 **\[管理資訊清單\]**。 下載您的 Azure AD 應用程式資訊清單，並以下列文字取代 `"identifierUris"` 區段。

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    這些字串代表您應用程式所支援的對象。 在後續的步驟中，您將會建立與這些對象值中每個相關聯的 Azure AD 存取權杖。 如需有關如何下載您應用程式資訊清單的詳細資訊，請參閱[了解 Azure Active Directory 應用程式資訊清單](http://go.microsoft.com/fwlink/?LinkId=722500)。

5.  儲存您的應用程式資訊清單，並將它上傳到 [Azure 管理入口網站](http://manage.windowsazure.com/)上您的應用程式中。

<span id="step-2"/>
## <a name="step-2-associate-your-azure-ad-client-id-with-your-app-in-windows-dev-center"></a>步驟 2：在 Windows 開發人員中心中，將您的 Azure AD 用戶端識別碼與您的 app 建立關聯

在使用 Windows 市集集合 API 或購買 API 運作的 app 或附加元件之前，必須在 Windows 開發人員中心儀表板中，將您的 Azure AD 用戶端識別碼與 app (或包含附加元件的 app) 建立關聯。

>**注意**&nbsp;&nbsp;您只需要執行此工作一次。

1.  登入[開發人員中心儀表板](https://dev.windows.com/overview)，然後選取您的應用程式。
2.  前往 **\[服務\]** &gt; **\[Product collections and purchases\]** (產品系列和購買) 頁面，並將您的 Azure AD 用戶端識別碼輸入其中一個可用欄位中。

<span id="step-3"/>
## <a name="step-3-create-azure-ad-access-tokens"></a>步驟 3：建立 Azure AD 存取權杖

您的服務必須先建立數個不同的 Azure AD 存取權杖來代表您的發行者身分識別，才能擷取 Windows 市集識別碼金鑰，或是呼叫 Windows 市集集合 API 或購買 API。 每個權杖與不同的 API 搭配使用。 每個權杖的存留期是 60 分鐘，且您可以在權杖到期後更新權杖。

<span id="access-tokens" />
### <a name="understanding-the-different-tokens-and-audience-uris"></a>了解不同的權杖和對象 URI

根據您想要在 Windows 市集集合 API 或購買 API 中呼叫的方法，您必須建立兩個或三個不同權杖。 每個存取權杖與不同的對象 URI 相關聯 (這些是先前加入到 Azure AD 應用程式資訊清單 `"identifierUris"` 區段的相同 URI)。

  * 在所有情況中，您都必須使用 `https://onestore.microsoft.com` 對象 URI 建立權杖。 在稍後的步驟，您將這個權杖傳遞到 Windows 市集集合 API 或購買 API 中方法的 **Authorization** 標頭。

  > **重要**&nbsp;&nbsp;僅搭配安全儲存於您服務中的存取權杖使用 `https://onestore.microsoft.com` 受眾。 在您服務以外的地方公開與此對象搭配的存取權杖，可能會讓您的服務容易遭受重新執行攻擊。

  * 如果您想要呼叫 Windows 市集集合 API 中的方法，以[查詢由使用者所擁有的產品](query-for-products.md)或[將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)，您必須也要使用 `https://onestore.microsoft.com/b2b/keys/create/collections` 對象 URI 建立權杖。 在稍後的步驟中，您將這個權杖傳遞到 Windows SDK 中的用戶端方法，來要求可搭配 Windows 市集集合 API 使用的 Windows 市集識別碼金鑰。

  * 如果您想要呼叫 Windows 市集購買 API 的方法，[將免費產品授與使用者](grant-free-products.md)，您必須也要使用 `https://onestore.microsoft.com/b2b/keys/create/purchase` 對象 URI 建立權杖。 在稍後的步驟中，您將這個權杖傳遞到 Windows SDK 中的用戶端方法，來要求可搭配 Windows 市集購買 API 使用的 Windows 市集識別碼金鑰。

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

* 對於 *client\_id* 和 *client\_secret* 參數，請指定您從 [Azure 管理入口網站](http://manage.windowsazure.com)擷取的應用程式用戶端識別碼和用戶端密碼。 為了要建立 Windows 市集集合 API 或購買 API 所需驗證層級的存取權杖，這兩個參數都是必要的。

* 對於 *resource* 參數，指定[上一節](#access-tokens)中列出的其中一個對象 URI，視您要建立的存取權杖類型而定。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。 如需有關存取權杖結構的詳細資訊，請參閱[支援的權杖和宣告類型](http://go.microsoft.com/fwlink/?LinkId=722501)。

> **重要**&nbsp;&nbsp;您僅應在服務內而非應用程式中建立 Azure AD 存取權杖。 如果該存取權杖被傳送到您的應用程式，您的用戶端密碼就可能遭竊。

<span id="step-4"/>
## <a name="step-4-create-a-windows-store-id-key"></a>步驟 4：建立 Windows 市集識別碼金鑰

您的 App 必須先建立 Windows 市集識別碼金鑰並傳送至您的服務，才能讓您呼叫 Windows 市集集合 API 或購買 API 中的方法。 此金鑰是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 如需該金鑰中宣告的詳細資訊，請參閱 [Windows 市集識別碼金鑰中的宣告](#claims)。

目前，建立 Windows 市集識別碼金鑰的唯一方式是。從 app 的用戶端程式碼呼叫通用 Windows 平台 (UWP) API。 產生的金鑰代表目前在裝置上已登入 Windows 市集的使用者的身分識別。

> **備註**&nbsp;&nbsp;每個 Windows 市集識別碼金鑰的有效期皆為 90 天。 金鑰到期之後，您可以[更新金鑰](renew-a-windows-store-id-key.md)。 我們建議您更新自己的 Windows 市集識別碼金鑰，而不是建立新的。

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-collection-api"></a>若要建立 Windows 市集集合 API 的 Windows 市集識別碼金鑰

請依照下列步驟來建立可搭配 Windows 市集集合 API 使用的 Windows 市集識別碼金鑰，以[查詢由使用者所擁有的產品](query-for-products.md)或[將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)。

1.  將您使用 `https://onestore.microsoft.com/b2b/keys/create/collections` 對象 URI 建立的 Azure AD 存取權杖從您的服務傳遞到用戶端 App。

2.  在您的應用程式程式碼中，呼叫其中一個方法來擷取 Windows 市集識別碼金鑰：

  * 如果您的 app 使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別來管理在應用程式內購買，請使用 [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間中的 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) 方法。

  將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 您可以視需要把識別碼傳遞給可識別您服務內容中目前使用者的 *publisherUserId* 參數。 如果您負責維護服務的使用者識別碼，您便可以使用此參數，將這些使用者識別碼與您對 Windows 市集集合 API 發出的呼叫相互關聯。

3.  在您的應用程式成功建立「Windows 市集識別碼」金鑰之後，將金鑰傳遞回您的服務。

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-purchase-api"></a>若要建立 Windows 市集購買 API 的 Windows 市集識別碼金鑰

請依照下列步驟來建立可搭配 Windows 購買集合 API 使用的 Windows 市集識別碼金鑰，以[將免費產品授與使用者](grant-free-products.md)。

1.  將您使用 `https://onestore.microsoft.com/b2b/keys/create/purchase` 對象 URI 建立的 Azure AD 存取權杖從您的服務傳遞到用戶端 App。

2.  在您的應用程式程式碼中，呼叫其中一個方法來擷取 Windows 市集識別碼金鑰：

  * 如果您的 app 使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別來管理在應用程式內購買，請使用 [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) 方法。

  * 如果您的 app 使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間中的 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別來管理在應用程式內購買，請使用 [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法。

  將您的 Azure AD 存取權杖傳遞給方法的 *serviceTicket* 參數。 您可以視需要把識別碼傳遞給可識別您服務內容中目前使用者的 *publisherUserId* 參數。 如果您負責維護服務的使用者識別碼，您便可以使用此參數，將這些使用者識別碼與您對 Windows 市集購買 API 發出的呼叫相互關聯。

3.  在您的應用程式成功建立「Windows 市集識別碼」金鑰之後，將金鑰傳遞回您的服務。

<span id="step-5"/>
## <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>步驟 5：從您的服務呼叫 Windows 市集集合 API 或購買 API

在您的服務擁有可存取特定使用者的產品擁有權資訊的 Windows 市集金鑰之後，依照下列指示讓您的服務呼叫 Windows 市集集合 API 或購買 API。

* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)

請針對每個案例，將下列資訊傳遞給 API：

-   您之前使用 `https://onestore.microsoft.com` 對象 URI 所建立的 Azure AD 存取權杖。 此權杖代表您的發行者身分識別。 請在要求標頭中傳遞此權杖。
-   您從應用程式的用戶端程式碼擷取的「Windows 市集識別碼」金鑰。 此金鑰代表您想要存取其產品擁有權資訊之使用者的身分識別。

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Windows 市集識別碼金鑰中的宣告

Windows 市集識別碼金鑰就是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 當您使用 Base64 來解碼時，Windows 市集識別碼金鑰會包含下列宣告。

* `iat`：&nbsp;&nbsp;&nbsp;能識別金鑰發出的時間。 此宣告可以用來判斷權杖的存在時間。 此值會顯示為 Epoch 時間。
* `iss`：&nbsp;&nbsp;&nbsp;能識別簽發者。 其值與 `aud` 宣告的值相同。
* `aud`：&nbsp;&nbsp;&nbsp;能識別對象。 必須為下列其中一個值：`https://collections.mp.microsoft.com/v6.0/keys` 或 `https://purchase.mp.microsoft.com/v6.0/keys`。
* `exp`：&nbsp;&nbsp;&nbsp;能識別金鑰的到期時間；金鑰在到期當天或之後，除了更新金鑰之外，系統就無法再接受該金鑰來處理任何工作。 此宣告的值會顯示為 Epoch 時間。
* `nbf`：&nbsp;&nbsp;&nbsp;能識別系統將接受要處理之權杖的時間。 此宣告的值會顯示為 Epoch 時間。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`：&nbsp;&nbsp;&nbsp;能識別開發人員的用戶端識別碼。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`：&nbsp;&nbsp;&nbsp;不透明的承載 (已加密，且已使用 Base64 編碼)，包含僅供 Windows 市集服務使用的資訊。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`：&nbsp;&nbsp;&nbsp;能識別您服務內容中目前使用者的使用者識別碼。 此值與您傳遞給[用來建立金鑰的方法](#step-4)之選擇性 *publisherUserId* 參數的值相同。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`：&nbsp;&nbsp;&nbsp;可用來更新金鑰的 URI。

以下是已解碼的 Windows 市集識別碼金鑰標頭範例。

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

以下是已解碼的 Windows 市集識別碼金鑰宣告組範例。

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
* [更新 Windows 市集識別碼索引鍵](renew-a-windows-store-id-key.md)
* [整合應用程式與 Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [了解 Azure Active Directory 應用程式資訊清單]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [支援的權杖和宣告類型](http://go.microsoft.com/fwlink/?LinkId=722501)
