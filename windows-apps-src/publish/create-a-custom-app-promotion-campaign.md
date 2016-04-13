---
Description: 除了為您的 app 建立將在 Windows app 中執行的廣告行銷活動之外，您也可以使用其他管道促銷您的 app。
title: 建立自訂 app 促銷活動
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
---

# 建立自訂 app 促銷活動



除了為您的 app 建立將在 Windows app 中執行的[廣告活動](create-an-ad-campaign-for-your-app.md)之外，您也可以使用其他管道促銷您的 app。 例如，您可以使用第三方 app 行銷提供者促銷您的 app，或可以在社交媒體網站上張貼 app 的連結。 這些活動稱為*自訂行銷活動*。

如果為您的 app 執行自訂行銷活動，可以追蹤每個行銷活動相關的效能，方法是為每個自訂行銷活動建立不同的 Windows 市集 app URL，其中每個 URL 包含不同的行銷活動識別碼。 當執行 Windows 10 的使用者按一下包含行銷活動識別碼的 URL 時，Microsoft 會將按一下與對應的自訂行銷活動產生關聯並讓您使用此資料。

與自訂行銷活動關聯的資料類型主要有兩個：您的 app 的頁面檢視和*轉換*。 轉換是 app 安裝，源自客戶透過自訂行銷活動所促銷的 URL 按一下 app 的 Windows 市集頁面。 如需轉換的詳細資訊，請參閱本主題中的[了解 app 安裝如何符合轉換的資格](#understanding-how-app-installs-qualify-as-conversions)。

您可以以下列方式擷取您的 app 的自訂行銷活動效能資料：

-   如果您的 app 是通用 Windows 平台 (UWP) app，它可以使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法，以程式設計方式擷取導致轉換的自訂行銷活動識別碼。
-   您可以從開發人員中心儀表板上的[通道和轉換報告](channels-and-conversions-report.md)，看到有關您 app 或 IAP 頁面檢視和轉換的資料。

> **重要** 這個資料只能追蹤執行 Windows 10 的客戶。 使用其他作業系統的客戶仍可以透過連結連到您的 app 清單，但不包含有關這些客戶的活動相關資料。

 

## 自訂行銷活動案例的範例


請考慮已經完成建置新遊戲並想要對為其現有的遊戲玩家促銷的遊戲開發人員。 她在她的 Facebook 頁面上張貼新遊戲發行的通知，包含遊戲的 Windows 市集頁面連結。 她的許多玩家也在 Twitter 上關注她，所以她也推文公告遊戲的 Windows 市集頁面連結。

為了追蹤每個這些促銷活動管道的成功度，開發人員會為遊戲的 Windows 市集頁面建立兩個 URL 變數：

-   她將張貼到 Facebook 網頁的 URL 包含自訂行銷活動識別碼 `my-facebook-campaign`。
-   她將張貼至 Twitter 的 URL 包含自訂行銷活動識別碼 `my-twitter-campaign`。

當她的 Facebook 和 Twitter 追隨者按一下 URL 時，Microsoft 會追蹤與對應的自訂行銷活動關聯的每個點按。 後續符合資格的遊戲購置和任何 app 內產品 (IAP) 購買會與自訂行銷活動產生關聯，並且回報為轉換。

## 了解 app 安裝如何符合轉換的資格


[轉換]** 是 app 安裝，源自客戶透過自訂行銷活動促銷的 URL 按一下 app 的 Windows 市集頁面。 針對開發人員中心儀表板上的[管道和轉換報告](channels-and-conversions-report.md)，符合轉換的資格與[以程式設計方式擷取行銷活動識別碼](#programmatically)符合轉換的資格有不同的條件。

針對[通道和轉換報告](channels-and-conversions-report.md)，若要符合轉換的資格，必須符合下列條件：

-   具備可識別 Microsoft 帳戶的客戶按一下包含自訂行銷活動識別碼的 app URL，並被重新導向至 app 的 Windows 市集頁面。
-   相同客戶 (依相同的 Microsoft 帳戶識別) 在第一次按一下具有自訂行銷活動識別碼的 Windows 市集 URL 之後的 24 小時內安裝 app。 這會符合轉換的資格，即使客戶在與他們按一下具有自訂行銷活動識別碼的 Windows 市集 URL 不同的電腦或裝置上安裝 app。
    > **注意** 針對計入為自訂行銷活動轉換的 app 安裝，在該 app 中的任何 IAP 購買也會計算為相同的自訂行銷活動的轉換。

     

若要在以程式設計方式擷取與 app 關聯的行銷活動識別碼時符合轉換的資格，必須符合下列條件：

-   具備可識別 Microsoft 帳戶的客戶按一下包含自訂行銷活動識別碼的 app URL，並被重新導向至 app 的 Windows 市集頁面。
-   客戶在按一下 URL 之後，於相同的 Windows 市集頁面檢視中安裝 app。 如果使用者離開頁面，並在 24 小時內返回頁面並安裝 app (無論是在同一部電腦或裝置或不同電腦或裝置上)，這就會符合[管道和轉換報告](channels-and-conversions-report.md)上的轉換資格，但如果您以程式設計方式擷取行銷活動識別碼，則不符合轉換的資格。

## 內嵌自訂行銷活動識別碼到您的 app 的 Windows 市集頁面 URL


若要使用自訂行銷活動識別碼為您的 app 建立 Windows 市集頁面 URL：

1.  為您的自訂行銷活動建立識別碼字串。 此字串可以包含最多 100 個字元，不過我們建議您定義容易識別的簡短行銷活動識別碼。
2.  取得您 app 的 HTML 或通訊協定格式的 Windows 市集頁面 URL。 HTML 格式的 URL 可在[開發人員中心儀表板中的 [App 身分識別]**** 頁面](link-to-your-app.md)取得。
    -   如果您想要客戶在瀏覽器中瀏覽到您的 app 的 Windows 市集頁面 (如果已安裝 Windows 市集 app，這個 URL 也將啟動 Windows 市集 app 到您的 app 清單)，請使用 HTTP 格式。 此 URL 的格式為 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**。 例如，Skype 的 HTTP URL 為 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`。
        > **注意** HTTP 格式 URL 可以用來在執行 Windows 7 和更新版本的電腦和平板和執行 Windows Phone 8 和更新版本的電話上的瀏覽器中瀏覽到 Windows 市集。
    - 如果您要從已安裝 Windows 市集 app 的裝置或電腦上執行的其他 Windows app 進行促銷，並且您想要客戶在 Windows 市集 app 中開啟您的 app 頁面，請使用通訊協定格式。 此 URL 的格式為 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的通訊協定 URL 為 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。
3.  附加以下字串到您的 app 的 URL 的結尾：
    -   針對 HTTP 格式 URL，附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 推出值為 **custom\_campaign** 的行銷活動識別碼，則包含行銷活動識別碼的新 HTTP URL 會是：`https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。
    -   針對通訊協定格式 URL，附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 推出值為 **custom\_campaign** 的行銷活動識別碼，則包含行銷活動識別碼的新通訊協定 URL 會是：`ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

## 以程式設計方式擷取 app 的自訂行銷活動識別碼


如果您的 app 是 UWP app，您可以使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法以程式設計方式擷取與 app 相關聯的自訂行銷活動識別碼。 這個方法能讓您實現許多分析以及獲利案例。 例如，您可以了解目前的使用者在透過您的 Facebook 行銷活動發現您的 app 後是否取得它，然後據此自訂 app 經驗。 或者，如果您使用第三方 app 行銷提供者，您可以將資料傳回提供者。

> **注意** 只有當客戶使用內嵌的行銷活動識別碼按一下您的 URL、被重新導向到您的 app 的 Windows 市集頁面，然後安裝您的 app 而未離開這個頁面時，[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法才會傳回行銷活動識別碼字串。 如果使用者離開頁面，並稍後回來並安裝 app，則使用 **GetAppPurchaseCampaignIdAsync** 時將不符合轉換的資格。 如需詳細資訊，請參閱本主題中的[了解轉換](#conversions)。

 

下列範例示範如何使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法來擷取 app 的自訂行銷活動識別碼。 如果 app 未安裝為成功轉換的一部分，這個方法會傳回空字串。

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` ManagedCPlusPlus
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

[
            **GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法會從 Windows 市集存取資料。 使用這個方法時請遵循這些指導方針：

-   在非同步作業中包裝這個方法可讓呼叫完成。
-   如果您的 app 尚未發佈到 Windows 市集，而您在測試您自訂行銷活動，請使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法，而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別。 依照下列指導方針執行：
    -   新增 **AppPurchaseCampaignId** 元素到 WindowsStoreProxy.xml 檔案，如下列範例所示。 將元素的值指派給自訂行銷活動識別碼。 執行 app 時，[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法一律會傳回這個值。 如需有關 WindowsStoreProxy.xml 檔案的詳細資訊，請參閱 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 的說明文件。

    ```        XML
    <CurrentApp>
        ....
        <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
    </CurrentApp>
    ```
    
    -   若要輕鬆地在使用 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 和 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 之間切換，我們建議您將下列陳述式新增到您的程式碼來定義 **Store** 別名，然後使用 **Store** 別名取得 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 呼叫的資格。

    ```        CSharp
    #if DEBUG
            using Store = Windows.ApplicationMode.Store.CurrentAppSimulator;
            #else
            using Store = Windows.ApplicationMode.Store.CurrentApp;
            #endif   
    ```

## 測試您的自訂行銷活動


促銷自訂行銷活動 URL 之前，建議您執行下列動作來測試自訂行銷活動：

1.  在您要用於測試的電腦或裝置上登入您的 Microsoft 帳戶。
2.  按一下您的自訂行銷活動 URL。 請確定 Windows 市集可正確載入您的 app 的頁面，然後關閉 Windows 市集 app 或瀏覽器頁面。
3.  按一下 URL 數次，在每次瀏覽到您的 app 頁面後關閉 Windows 市集 app 或瀏覽器頁面。 在其中一個到您的 app 頁面的瀏覽中，安裝您的 app 以產生轉換。 計算您按一下 URL 的總次數。
4.  如果您是以程式設計方式擷取您的 app 中的自訂行銷活動識別碼，請使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別 (而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別) 的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法測試這個作業。

 

 






<!--HONumber=Mar16_HO1-->


