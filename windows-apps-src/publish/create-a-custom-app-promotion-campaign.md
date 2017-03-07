---
author: shawjohn
Description: "除了為您的應用程式建立將在 Windows 應用程式中執行的廣告活動之外，您也可以使用其他管道促銷您的應用程式。"
title: "建立自訂應用程式促銷活動"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 自訂, 應用程式, 促銷, 行銷活動"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7920e2ba2fd4222ba012a98751e35133b36ac334
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-custom-app-promotion-campaign"></a>建立自訂應用程式促銷活動



除了為您的應用程式建立將在 Windows 應用程式中執行的[廣告活動](create-an-ad-campaign-for-your-app.md)之外，您也可以使用其他管道促銷您的應用程式。 例如，您可以使用第三方 app 行銷提供者促銷您的 app，或可以在社交媒體網站上張貼 app 的連結。 這些活動稱為*自訂行銷活動*。

如果為您的 app 執行自訂行銷活動，可以追蹤每個行銷活動相關的效能，方法是為每個自訂行銷活動建立不同的 Windows 市集 app URL，其中每個 URL 包含不同的行銷活動識別碼。 當執行 Windows 10 的使用者按一下包含行銷活動識別碼的 URL 時，Microsoft 會將按一下與對應的自訂行銷活動產生關聯並讓您使用此資料。

與自訂行銷活動關聯的資料類型主要有兩個：您應用程式的頁面檢視和「轉換」**。 「轉換」即是指客戶透過按下您藉由自訂行銷活動促銷的 URL，連結到您應用程式的 Windows 市集頁面，並且取得您的應用程式。 如需轉換的詳細資訊，請參閱本主題中的[了解應用程式下載數如何符合轉換的資格](#understanding-how-app-acquisitions-qualify-as-conversions)。

您可以使用下列方式擷取您應用程式的自訂行銷活動效能資料：

-   如果您的 App 是通用 Windows 平台 (UWP) app，它可以使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法，以程式設計方式擷取導致轉換的自訂行銷活動識別碼。
-   您可以從開發人員中心儀表板上的[管道和轉換報告](channels-and-conversions-report.md)，檢視有關您 App 或附加元件頁面檢視和轉換的資料。

> **重要：**這項資料只能追蹤執行 Windows 10 的客戶。 使用其他作業系統的客戶仍可以透過連結連到您的 app 清單，但不包含有關這些客戶的活動相關資料。

 

## <a name="example-custom-campaign-scenario"></a>自訂行銷活動案例的範例


請考慮已經完成建置新遊戲並想要對為其現有的遊戲玩家促銷的遊戲開發人員。 她在她的 Facebook 頁面上張貼新遊戲發行的通知，包含遊戲的 Windows 市集頁面連結。 她的許多玩家也在 Twitter 上關注她，所以她也推文公告遊戲的 Windows 市集頁面連結。

為了追蹤每個這些促銷活動管道的成功度，開發人員會為遊戲的 Windows 市集頁面建立兩個 URL 變數：

-   她將張貼到 Facebook 網頁的 URL 包含自訂行銷活動識別碼 `my-facebook-campaign`。
-   她將張貼至 Twitter 的 URL 包含自訂行銷活動識別碼 `my-twitter-campaign`。

當她的 Facebook 和 Twitter 追隨者按一下 URL 時，Microsoft 會追蹤與對應的自訂行銷活動關聯的每個點按。 後續符合資格的遊戲下載數和任何附加元件購買會與自訂行銷活動產生關聯，並且回報為轉換。

## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>了解應用程式下載數如何符合轉換的資格


「轉換」**即是指客戶透過按下您藉由自訂行銷活動促銷的 URL，連結到您應用程式的 Windows 市集頁面，並且取得您的應用程式。 透過開發人員中心儀表板上的[管道和轉換報告](channels-and-conversions-report.md)，及[以程式設計方式擷取行銷活動識別碼](#programmatically)符合轉換的資格，兩者皆有不同的條件。

透過[管道和轉換報告](channels-and-conversions-report.md)，若要符合轉換的資格，必須符合下列條件：

-   客戶 (具備或不具備可辨識 Microsoft 帳戶) 按一下包含自訂行銷活動識別碼的應用程式 URL，並重新導向至應用程式的 Windows 市集頁面。 同一個客戶在首次按下包含自訂行銷活動識別碼的 Windows 市集 URL 的 24 小時內取得應用程式。
-   若客戶在不同的電腦，或是在與他們用來按下包含自訂行銷活動識別碼之 Windows 市集 URL 的裝置不一樣的裝置上取得應用程式，只有在當該客戶具備了可辨識的 Microsoft 帳戶時，才會計算轉換。

> **注意：**針對已計入為透過自訂行銷活動轉換的應用程式下載數，該客戶在該應用程式中任何附加內容的購買，也都會計為透過同一個自訂行銷活動進行的轉換。
     

若要在以程式設計方式擷取與應用程式相關聯的行銷活動識別碼時符合轉換的資格，必須符合下列條件：

-   客戶 (具備或不具備可辨識 Microsoft 帳戶) 按一下包含自訂行銷活動識別碼的應用程式 URL，並重新導向至應用程式的 Windows 市集頁面。 客戶在按一下 URL 之後，於相同的 Windows 市集網頁檢視中取得應用程式。
-   若客戶離開頁面，然後在 24 小時之內又重新返回該頁面 (在同一部電腦或裝置上，或在具備了可辨識 Microsoft 帳戶的前提下，使用不同的電腦或裝置)，並且取得應用程式，這將計為透過[管道和轉換報告](channels-and-conversions-report.md)完成之轉換。 然而，這不會計為您透過程式設計方式擷取行銷活動識別碼所達成之轉換。

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>內嵌自訂行銷活動識別碼到您的 app 的 Windows 市集頁面 URL


若要使用自訂行銷活動識別碼為您的 app 建立 Windows 市集頁面 URL：

1.  為您的自訂行銷活動建立識別碼字串。 此字串可包含最多 100 個字元，但建議您定義容易識別的簡短行銷活動識別碼。 請注意：其他位於管道和轉換報告中的開發人員，可能會看到您的行銷活動識別碼字串。 這可能會導致客戶雖然透過按下您的行銷活動識別碼進入到市集之中，但最後卻在同一個工作階段中購買了另一位開發人員的應用程式。 即使另一個開發人員可能會在這種情況下看到您的行銷活動識別碼名稱，該開發人員只會看到他們自己的應用程式當中，有多少轉換是來自於初次按下您的行銷活動識別碼。 他們不會看到有多少使用者是透過您的行銷活動識別碼而購買到您應用程式的相關資料。
2.  取得您應用程式的 HTML 或通訊協定格式的 Windows 市集頁面 URL。 HTML 格式的 URL 可在[開發人員中心儀表板中的 [App 身分識別]**** 頁面](link-to-your-app.md)取得。
    -   如果您想要客戶在瀏覽器中瀏覽到您的 app 的 Windows 市集頁面 (如果已安裝 Windows 市集 app，這個 URL 也將啟動 Windows 市集 app 到您的 app 清單)，請使用 HTTP 格式。 此 URL 的格式為 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**。 例如，Skype 的 HTTP URL 為 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`。 請注意：HTTP 格式 URL 可以在執行 Windows 7 和更新版本的電腦和平板，以及執行 Windows Phone 8 和更新版本的電話上的瀏覽器中用來瀏覽 Windows 市集。
    -   若您想要從已安裝 Windows 市集應用程式的裝置或電腦上執行的其他 Windows 應用程式進行促銷，並且您想要客戶在 Windows 市集應用程式中開啟您的應用程式頁面，請使用通訊協定格式。 此 URL 的格式為 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的通訊協定 URL 為 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。
3.  附加以下字串到您的 app 的 URL 的結尾：
    -   針對 HTTP 格式 URL，附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 推出值為 **custom\_campaign** 的行銷活動識別碼，則包含行銷活動識別碼的新 HTTP URL 會是：`https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。
    -   針對通訊協定格式 URL，附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 推出值為 **custom\_campaign** 的行銷活動識別碼，則包含行銷活動識別碼的新通訊協定 URL 會是：`ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>以程式設計方式擷取 app 的自訂行銷活動識別碼


如果您的 app 是 UWP app，您可以使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法以程式設計方式擷取與 app 相關聯的自訂行銷活動識別碼。 這個方法能讓您實現許多分析以及獲利案例。 例如，您可以了解目前的使用者在透過您的 Facebook 行銷活動發現您的 app 後是否取得它，然後據此自訂 app 經驗。 或者，如果您使用第三方 app 行銷提供者，您可以將資料傳回提供者。

> **注意：**只有當客戶按下您內嵌了行銷活動識別碼的 URL，重新導向至您應用程式的 Windows 市集頁面，並且在未離開該頁面前取得您的應用程式時，[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法才會傳回行銷活動識別碼字串。 即使使用者離開頁面，並在稍後即返回取得應用程式，使用 **GetAppPurchaseCampaignIdAsync** 時仍將不符合轉換的資格。 如需詳細資訊，請參閱本主題中的[了解轉換](#conversions)。

 

下列範例示範如何使用 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法來擷取應用程式的自訂行銷活動識別碼。 若應用程式並未作為成功轉換的一部分由使用者取得，這個方法會傳回空字串。

``` csharp
string campaignId = await CurrentApp.GetAppPurchaseCampaignIdAsync();
```

``` cpp
HString campaignId;
HRESULT hr = CurrentApp::GetAppPurchaseCampaignIdAsync(campaignId.GetAddressOf());
```

[**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt186445) 方法會從 Windows 市集存取資料。 使用這個方法時請遵循這些指導方針：

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

## <a name="test-your-custom-campaign"></a>測試您的自訂行銷活動


促銷自訂行銷活動 URL 之前，建議您執行下列動作來測試自訂行銷活動：

1.  在您要用於測試的電腦或裝置上登入您的 Microsoft 帳戶。
2.  按一下您的自訂行銷活動 URL。 請確定 Windows 市集可正確載入您的 app 的頁面，然後關閉 Windows 市集 app 或瀏覽器頁面。
3.  按一下 URL 數次，在每次瀏覽到您的應用程式頁面後關閉 Windows 市集應用程式或瀏覽器頁面。 在其中一次到您應用程式頁面的瀏覽中，取得您的應用程式以產生轉換。 計算您按一下 URL 的總次數。
4.  如果您是以程式設計方式擷取您的 app 中的自訂行銷活動識別碼，請使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別 (而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別) 的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/mt187034) 方法測試這個作業。

 

 

