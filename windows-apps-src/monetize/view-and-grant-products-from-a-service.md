---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "如果您有 App 及附加元件的型錄，就能使用「Windows 市集集合 API」及「Windows 市集購買 API」，來從您的服務存取這些產品的擁有權資訊。"
title: "從服務檢視及授與產品"
translationtype: Human Translation
ms.sourcegitcommit: 819843c8ba1e4a073f70f7de36fe98dd4087cdc6
ms.openlocfilehash: 86d7383a7288434ec613244e4c2ba610889a45fb

---

# 從服務檢視及授與產品

如果您有 App 及附加元件 (又稱為應用程式內產品或 IAP) 的型錄，就能使用「Windows 市集集合 API」及「Windows 市集購買 API」，來從您的服務存取這些產品的擁有權資訊。

這些 API 是由幾個 REST 方法所構成的，而這些方法是專供開發人員用來與跨平台服務所支援的附加元件型錄搭配使用。 這些 API 可讓您進行下列行動：

-   Windows 市集集合 API：查詢由特定使用者所擁有的 App 和附加元件，或是將某個消費性產品回報為已完成。
-   Windows 市集購買 API：將免費的 App 或附加元件授與給特定的使用者。

## 使用 Windows 市集集合 API 與 Windows 市集購買 API


Windows 市集集合 API 及購買 API 會使用 Azure Active Directory (Azure AD) 驗證來存取客戶的擁有權資訊。 您必須先在 Windows 開發人員中心儀表板中，把 Azure AD 中繼資料套用到您的應用程式上，並產生數個必要的存取權杖和金鑰，才能夠呼叫這些 API。 下列步驟說明端對端的程序：

1.  [在 Azure AD 中設定 Web 應用程式](#step-1)。 這個應用程式代表您在 Azure AD 內容中的跨平台服務。
2.  [在 Windows 開發人員中心儀表板中，讓您的 Azure AD 用戶端識別碼與應用程式建立關聯](#step-2)。
3.  在您的服務中，[產生代表您發行者身分識別的 Azure AD 存取權杖](#step-3)。
4.  在您 Windows 應用程式的用戶端程式碼中，[產生代表目前使用者身分識別的 Windows 市集識別碼金鑰](#step-4)，然後把該 Windows 市集識別碼金鑰傳回您的服務。
5.  在您擁有必要的 Azure AD 存取權杖和 Windows 市集識別碼金鑰之後，[從您的服務呼叫 Windows 市集集合 API 或購買 API](#step-5)。

下列各節提供以上每個步驟的詳細資料。

<span id="step-1"/>
### 步驟 1：在 Azure AD 中設定 Web 應用程式。

1.  依照[整合應用程式與 Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502) 中的指示，將 Web 應用程式新增到 Azure AD。

    > **注意**
            &nbsp;&nbsp;在 \[告訴我們您的應用程式\] 頁面上，請務必選擇 \[Web 應用程式和/或 Web API\]。 您必須這麼做，才能為您的應用程式取得金鑰 (也稱為「用戶端密碼」)。 為了呼叫 Windows 市集集合 API 或購買 API，當您在稍後的步驟中向 Azure AD 要求存取權杖時，必須提供用戶端密碼。

2.  在 [Azure 管理入口網站](http://manage.windowsazure.com/)中，瀏覽到 \[Active Directory\]。 選取您的目錄，按一下頂端的 \[應用程式\] 索引標籤，然後選取您的應用程式。
3.  按一下 \[設定\] 索引標籤。 在此索引標籤上，取得您應用程式的用戶端識別碼並要求一個金鑰 (在後續步驟中稱為「用戶端密碼」)。
4.  按一下螢幕底部的 \[管理資訊清單\]。 下載您的 Azure AD 應用程式資訊清單，並以下列文字取代 `"identifierUris"` 區段。

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    這些字串代表您應用程式所支援的對象。 在後續的步驟中，您將會建立與這些對象值中每個相關聯的 Azure AD 存取權杖。 如需有關如何下載您應用程式資訊清單的詳細資訊，請參閱[了解 Azure Active Directory 應用程式資訊清單]( http://go.microsoft.com/fwlink/?LinkId=722500)。

5.  儲存您的應用程式資訊清單，並將它上傳到 [Azure 管理入口網站](http://manage.windowsazure.com/)上您的應用程式中。

<span id="step-2"/>
### 步驟 2：在 Windows 開發人員中心儀表板中，將您的 Azure AD 用戶端識別碼與應用程式建立關聯

Windows 市集集合 API 及購買 API 只會針對已與您 Azure AD 用戶端識別碼建立關聯的 App 和附加元件，提供其使用者擁有權資訊的存取權。

1.  登入 [Windows 開發人員中心儀表板](https://dev.windows.com/overview)，然後選取您的 App
2.  依序前往 \[服務\]&gt; \[產品系列和購買\] 頁面，然後在其中一個可用欄位中輸入您的 Azure AD 用戶端識別碼。

<span id="step-3"/>
### 步驟 3：從 Azure AD 擷取存取權杖

您的服務必須先要求三個代表您發行者身分識別的 Azure AD 存取權杖，才能擷取 Windows 市集金鑰，或是呼叫 Windows 市集集合 API 或購買 API。 這些存取權杖中的每個都與不同的對象 URI 相關聯，且每個權杖都將搭配不同的 API 呼叫來使用。 每個權杖的存留期是 60 分鐘，且您可以在權杖到期後更新權杖。

如要建立存取權杖，請依照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指示，在您的服務中使用 OAuth 2.0 API。 請針對每個權杖指定下列參數資料：

-   對於 *client\_id* 和 *client\_secret* 參數，請指定您從 [Azure 管理入口網站](http://manage.windowsazure.com/)取得的應用程式用戶端識別碼和用戶端密碼。 為了要產生 Windows 市集集合 API 或購買 API 所需驗證層級的存取權杖，這兩個參數都是必要的。
-   對於 *resource* 參數，請指定下列其中一個應用程式識別碼 URI (這些 URI 與您先前新增到應用程式資訊清單之 `"identifierUris"` 區段的 URI 相同)。 在此程序結束時，您應該會有三個存取權杖，且每個都與下列其中一個應用程式識別碼 URI 相關聯：
    -   `https://onestore.microsoft.com/b2b/keys/create/collections`：在稍後的步驟中，您將使用以此 URI 建立的存取權杖，來要求 Windows 市集識別碼金鑰，讓您能搭配 Windows 市集集合 API 來使用。
    -   `https://onestore.microsoft.com/b2b/keys/create/purchase`：在稍後的步驟中，您將使用以此 URI 建立的存取權杖，來要求 Windows 市集識別碼金鑰，讓您能搭配 Windows 市集購買 API 來使用。
    -   `https://onestore.microsoft.com`：在稍後的步驟中，您將使用以此 URI 建立的存取權杖，來直接呼叫「Windows 市集集合 API」或「購買 API」。

    > **重要：**
            &nbsp;&nbsp;請只搭配安全地儲存在您服務中的存取權杖來使用 `https://onestore.microsoft.com` 對象。 在您服務以外的地方公開與此對象搭配的存取權杖，可能會讓您的服務容易遭受重新執行攻擊。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。 如需有關存取權杖結構的詳細資訊，請參閱[支援的權杖和宣告類型](http://go.microsoft.com/fwlink/?LinkId=722501)。

> **重要**
            &nbsp;&nbsp;您應該只在您的服務內容中 (而不是在您的 App 中) 建立 Azure AD 存取權杖。 如果該存取權杖被傳送到您的 App，您的用戶端密碼就可能遭竊。

<span id="step-4"/>
### 步驟 4：從您應用程式中的用戶端程式碼產生 Windows 市集識別碼金鑰

您的服務必須先取得 Windows 市集識別碼金鑰，才能讓您呼叫 Windows 市集集合 API 或購買 API。 這是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 如需該金鑰中宣告的詳細資訊，請參閱＜[Windows 市集識別碼金鑰中的宣告](#claims)＞一節。

目前，能取得 Windows 市集金鑰唯一的方法，就是從您應用程式的用戶端程式碼呼叫通用 Windows 平台 (UWP) API，來擷取目前登入 Windows 市集的使用者身分識別。 如何產生 Windows 市集識別碼金鑰：

1.  將下列其中一個存取權杖，從您的服務傳遞給您的用戶端應用程式：

  * 如要取得可搭配 Windows 市集集合 API 使用的 Windows 市集識別碼金鑰，請傳遞您之前利用 `https://onestore.microsoft.com/b2b/keys/create/collections` 對象 URI 所建立的 Azure AD 存取權杖。

  * 若要取得可搭配「Windows 市集購買 API」使用的「Windows 市集識別碼」金鑰，請傳遞您以 `https://onestore.microsoft.com/b2b/keys/create/purchase` 對象 URI 建立的 Azure AD 存取權杖。

2.  在您的應用程式程式碼中，呼叫下列其中一個方法來擷取「Windows 市集識別碼」金鑰。

  * 當您的 App 使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別來管理 App 內購買時，如果您想要使用「Windows 市集集合 API」，請使用 [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) 方法，或是如果您想要使用「Windows 市集購買 API」，則請使用 [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) 方法。

  * 當您的 App 使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間中的 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別來管理 App 內購買時，如果您想要使用「Windows 市集集合 API」，請使用 [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) 方法，或是如果您想要使用「Windows 市集購買 API」，則請使用 [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法。

    針對每一種方法，都請將您的 Azure AD 存取權杖傳遞給 *serviceTicket* 參數。 您可以視需要把識別碼傳遞給可識別您服務內容中目前使用者的 *publisherUserId* 參數。 如果您負責維護服務的使用者識別碼，您便可以使用此參數，將這些使用者識別碼與您對「Windows 市集集合 API」或「購買 API」發出的呼叫相互關聯。

    >**注意**
            &nbsp;&nbsp;如需有關 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

3.  在您的 App 成功擷取「Windows 市集識別碼」金鑰之後，將金鑰傳遞回您的服務。

> **注意**
            &nbsp;&nbsp;每個「Windows 市集識別碼」金鑰的有效期是 90 天。 金鑰到期之後，您可以[更新金鑰](renew-a-windows-store-id-key.md)。 我們建議您更新自己的 Windows 市集識別碼金鑰，而不是建立新的。

<span id="step-5"/>
### 步驟 5：從您的服務呼叫 Windows 市集集合 API 或購買 API

在您的服務擁有可存取特定使用者的產品擁有權資訊的 Windows 市集金鑰之後，您的服務便可呼叫 Windows 市集集合 API 或購買 API。 請使用適用於您案例的指示：

-   [查詢產品](query-for-products.md)
-   [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
-   [授與免費產品](grant-free-products.md)

請針對每個案例，將下列資訊傳遞給 API：

-   您之前使用 `https://onestore.microsoft.com` 對象 URI 所建立的 Azure AD 存取權杖。 此權杖代表您的發行者身分識別。 請在要求標頭中傳遞此權杖。
-   您從 App 的用戶端程式碼擷取的「Windows 市集識別碼」金鑰。 此金鑰代表您想要存取其產品擁有權資訊之使用者的身分識別。

<span id="claims"/>
## Windows 市集識別碼金鑰中的宣告

Windows 市集識別碼金鑰就是 JSON Web 權杖 (JWT)，代表您想要存取其產品擁有權資訊之使用者的身分識別。 當您使用 Base64 來解碼時，Windows 市集識別碼金鑰會包含下列宣告。

| 宣告名稱            | 說明          |
|---------------|-------------|
| iat                 | 能識別金鑰發出的時間。 此宣告可以用來判斷權杖的存在時間。 此值會顯示為 Epoch 時間。           |
| iss                    | 能識別簽發者。 其值與 *aud* 宣告的值相同。      |
| aud           | 能識別對象。 必須為下列其中一個值：`https://collections.mp.microsoft.com/v6.0/keys` 或 `https://purchase.mp.microsoft.com/v6.0/keys`。  |
| exp        | 能識別金鑰的到期時間；金鑰在到期當天或之後，除了更新金鑰之外，系統就無法再接受該金鑰來處理任何工作，。 此宣告的值會顯示為 Epoch 時間。       |
| nbf                | 能識別系統將接受權杖來處理工作的時間。 此宣告的值會顯示為 Epoch 時間。 |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`   | 能識別開發人員的用戶端識別碼。     |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`    | 不透明的裝載 (已加密，且已使用 Base64 編碼)，包含僅供 Windows 市集服務使用的資訊。   |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`     | 能識別您服務內容中目前使用者的使用者識別碼。 此值與您傳遞給[用來產生金鑰的方法](view-and-grant-products-from-a-service.md#step-4)之選擇性 *publisherUserId* 參數的值相同。 |
| `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri` | 可用來更新金鑰的 URI。                                                                                                                 
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

## 相關主題

* [查詢產品](query-for-products.md)
* [將消費性產品回報為已完成](report-consumable-products-as-fulfilled.md)
* [授與免費產品](grant-free-products.md)
* [更新 Windows 市集識別碼金鑰](renew-a-windows-store-id-key.md)
* [整合應用程式與 Azure Active Directory](http://go.microsoft.com/fwlink/?LinkId=722502)
* [了解 Azure Active Directory 應用程式資訊清單]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [支援的權杖和宣告類型](http://go.microsoft.com/fwlink/?LinkId=722501)
 

 



<!--HONumber=Nov16_HO1-->


