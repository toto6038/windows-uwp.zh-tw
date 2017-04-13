---
author: shawjohn
Description: "除了為您的 app 建立將在 Windows app 中執行的廣告行銷活動之外，您也可以使用其他管道促銷您的 app。"
title: "建立自訂應用程式促銷活動"
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 自訂, 應用程式, 促銷, 行銷活動"
ms.openlocfilehash: 1e56b1df4b5501ded5db799c3ad67f97c0e1cfcd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-a-custom-app-promotion-campaign"></a>建立自訂應用程式促銷活動

除了為您的 app 建立將在 Windows app 中執行的[廣告活動](create-an-ad-campaign-for-your-app.md)之外，您也可以使用其他管道促銷您的 app。 例如，您可以使用第三方 app 行銷提供者促銷您的 app，或可以在社交媒體網站上張貼 app 的連結。 這些活動稱為*自訂行銷活動*。

如果為您的 app 執行自訂行銷活動，可以追蹤每個行銷活動相關的效能，方法是為每個自訂行銷活動建立不同的 Windows 市集 app URL，其中每個 URL 包含不同的行銷活動識別碼。 當執行 Windows 10 的使用者按一下包含行銷活動識別碼的 URL 時，Microsoft 會將按一下與對應的自訂行銷活動產生關聯並讓您使用此資料。

與自訂行銷活動關聯的資料類型主要有兩個：您應用程式的頁面檢視和「轉換」**。 「轉換」即是指客戶透過按下您藉由自訂行銷活動促銷的 URL，連結到您應用程式的 Windows 市集頁面，並且取得您的應用程式。 如需轉換的詳細資訊，請參閱本主題中的[了解應用程式下載數如何符合轉換的資格](#understanding-how-app-acquisitions-qualify-as-conversions)。

您可以使用下列方式擷取 App 的自訂行銷活動成效資料：

* 您可以從開發人員中心儀表板上的[管道和轉換報告](channels-and-conversions-report.md)，檢視有關您 App 或附加元件頁面檢視和轉換的資料。
* 如果您的 App 是通用 Windows 平台 (UWP) app，它可以使用 Windows SDK 中的 API，以程式設計方式擷取導致轉換的自訂行銷活動識別碼。

> **重要**&nbsp;&nbsp;這項資料只能追蹤執行 Windows 10 的客戶。 使用其他作業系統的客戶仍可以透過連結連到您的 app 清單，但不包含有關這些客戶的活動相關資料。

## <a name="example-custom-campaign-scenario"></a>自訂行銷活動案例的範例

請考慮已經完成建置新遊戲並想要對為其現有的遊戲玩家促銷的遊戲開發人員。 她在她的 Facebook 頁面上張貼新遊戲發行的通知，包含遊戲的 Windows 市集頁面連結。 她的許多玩家也在 Twitter 上關注她，所以她也推文公告遊戲的 Windows 市集頁面連結。

為了追蹤每個這些促銷活動管道的成功度，開發人員會為遊戲的 Windows 市集頁面建立兩個 URL 變數：

* 她將張貼到 Facebook 網頁的 URL 包含自訂行銷活動識別碼 `my-facebook-campaign`。
* 她將張貼至 Twitter 的 URL 包含自訂行銷活動識別碼 `my-twitter-campaign`。

當她的 Facebook 和 Twitter 追隨者按一下 URL 時，Microsoft 會追蹤與對應的自訂行銷活動關聯的每個點按。 後續符合資格的遊戲下載數和任何附加元件購買會與自訂行銷活動產生關聯，並且回報為轉換。

<span id="conversions" />
## <a name="understanding-how-app-acquisitions-qualify-as-conversions"></a>了解應用程式下載數如何符合轉換的資格

「轉換」**即是指客戶透過按下您藉由自訂行銷活動促銷的 URL，連結到您應用程式的 Windows 市集頁面，並且取得您的應用程式。 透過開發人員中心儀表板上的[管道和轉換報告](channels-and-conversions-report.md)，及[以程式設計方式擷取行銷活動識別碼](#programmatically)符合轉換的資格，兩者皆有不同的條件。

### <a name="qualifying-conversions-for-the-dashboard-report"></a>儀表板報告合格轉換

透過[管道和轉換報告](channels-and-conversions-report.md)，若要符合轉換的資格，必須符合下列條件：

* 客戶 *(具備或不具備已辨識的 Microsoft 帳戶)* 按一下包含自訂行銷活動識別碼的 App URL，並重新導向至 App 的 Windows 市集頁面。 同一個客戶在首次按下包含自訂行銷活動識別碼的 Windows 市集 URL 的 24 小時內取得應用程式。

* 若客戶在不同的電腦，或是在與他們用來按下包含自訂行銷活動識別碼之 Windows 市集 URL 的裝置不一樣的裝置上取得 App，只有在該客戶擁有 Microsoft 帳戶時，才會計算轉換。

> **注意**&nbsp;&nbsp;針對已計入為透過自訂行銷活動轉換的 App 下載數，該客戶在該 App 中任何附加內容的購買，也都會計為透過同一個自訂行銷活動進行的轉換。

### <a name="qualifying-conversions-for-programmatically-retrieving-the-campaign-id"></a>以程式設計方式擷取行銷活動識別碼的合格轉換

若要在以程式設計方式擷取與 App 相關聯的行銷活動識別碼時符合轉換的資格，必須符合下列條件：

* 在執行 **Windows 10 1607 版或更新版本**的裝置上：客戶 *(具備或不具備已辨識的 Microsoft 帳戶)* 按一下包含自訂行銷活動識別碼的 App URL，並已重新導向至 App 的 Windows 市集頁面。 客戶在按一下 URL 之後，於相同的 Windows 市集頁面檢視中取得 App。

* 在執行 **Windows 10 1511 版或更早版本**的裝置上：客戶 *(具備已辨識的 Microsoft 帳戶)* 按一下包含自訂行銷活動識別碼的 App URL，並已重新導向至該 App 的 Windows 市集頁面。 客戶在按一下 URL 之後，於相同的 Windows 市集頁面檢視中取得 App。 在這些版本的 Windows 10 中，使用者必須擁有已辨識的 Microsoft 帳戶，才能在以程式設計方式擷取行銷活動識別碼時讓下載符合轉換資格。

>**注意**&nbsp;&nbsp;若客戶離開頁面，然後在 24 小時之內又重新返回該頁面 (在同一部電腦或裝置上，或在具備了可辨識 Microsoft 帳戶的前提下，使用不同的電腦或裝置)，並且取得 App，這將符合對[通道與轉換報告](channels-and-conversions-report.md)的轉換資格。 然而，這不會計為您透過程式設計方式擷取行銷活動識別碼所達成之轉換。

## <a name="embed-a-custom-campaign-id-to-your-apps-windows-store-page-url"></a>內嵌自訂行銷活動識別碼到您的 app 的 Windows 市集頁面 URL

若要使用自訂行銷活動識別碼為您的 app 建立 Windows 市集頁面 URL：

1.  為您的自訂行銷活動建立識別碼字串。 此字串可以包含最多 100 個字元，但建議您定義容易識別的簡短行銷活動識別碼。

  >**注意**：通道與轉換報告中的其他開發人員可能會看到行銷活動識別碼字串。 這可能會導致客戶雖然透過按下您的行銷活動識別碼進入到市集之中，但最後卻在同一個工作階段中購買了另一位開發人員的應用程式。 即使另一個開發人員可能會在這種情況下看到您的行銷活動識別碼名稱，該開發人員只會看到他們自己的應用程式當中，有多少轉換是來自於初次按下您的行銷活動識別碼。 他們不會看到有多少使用者是透過您的行銷活動識別碼而購買到您應用程式的相關資料。

2.  以 HTML 或通訊協定格式取得 App 的 Windows 市集頁面 URL。

    * 如果您想要客戶在瀏覽器中瀏覽到您的 app 的 Windows 市集頁面 (如果已安裝 Windows 市集 app，這個 URL 也將啟動 Windows 市集 app 到您的 app 清單)，請使用 HTTP 格式。 此 URL 的格式為 **`https://www.microsoft.com/store/apps/*your app name*/*your app ID*`**。 例如，Skype 的 HTTP URL 為 `https://www.microsoft.com/store/apps/skype/9wzdncrfj364`。 HTML 格式的 URL 可在[開發人員中心儀表板中的 **\[App 身分識別\]** 頁面](link-to-your-app.md)取得。 請注意，HTTP 格式 URL 可以在執行 Windows 7 和更新版本的電腦和平板，以及執行 Windows Phone 8 和更新版本的電話上的瀏覽器中用來瀏覽 Windows 市集。

    * 若您想要從已安裝 Windows 市集應用程式的裝置或電腦上執行的其他 Windows 應用程式進行促銷，並且您想要客戶在 Windows 市集應用程式中開啟您的應用程式頁面，請使用通訊協定格式。 此 URL 的格式為 **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**。 例如，Skype 的通訊協定 URL 為 `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`。

3.  附加以下字串到您的 app 的 URL 的結尾：

    * 針對 HTTP 格式 URL，附加 **`?cid=*my custom campaign ID*`**。 例如，如果 Skype 推出值為 **custom\_campaign** 的行銷活動識別碼，則包含行銷活動識別碼的新 HTTP URL 會是：`https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`。

    * 針對通訊協定格式 URL，附加 **`&cid=*my custom campaign ID*`**。 例如，如果 Skype 推出值為 **custom\_campaign** 的行銷活動識別碼，則包含行銷活動識別碼的新通訊協定 URL 會是：`ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`。

<span id="programmatically" />
## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>以程式設計方式擷取 app 的自訂行銷活動識別碼

如果您的 App 是 UWP app，您可以使用 Windows SDK 中的 API 以程式設計方式擷取與 App 相關聯的自訂行銷活動識別碼。 這些 API 使許多分析和創造營收案例得以實現。 例如，您可以了解目前的使用者在透過您的 Facebook 行銷活動發現您的 app 後是否取得它，然後據此自訂 app 經驗。 或者，如果您使用第三方 App 行銷提供者，您可以將資料傳回至提供者。

只有在客戶按一下包含行銷活動識別碼的 URL，重新導向至 App 的 Windows 市集頁面，接著取得您的 App 但未離開這個頁面時，這些 API 才會傳回行銷活動識別碼字串 如果使用者離開頁面，但稍後又返回並取得 App，那麼使用這些 API 時就[不符合轉換資格](#conversions)。

您要使用的 API 會有所不同，取決於 App 針對的 Windows 10 目標版本：

* Windows 10 1607 版或更新版本︰使用 **Windows.Services.Store** 命名空間中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext)。 使用此 API 時，您可以擷取任何[合格下載](#conversions)的自訂行銷活動識別碼，不論使用者是否擁有已辨識的 Microsoft 帳戶。
* Windows 10 1511 版或更早版本︰使用 **Windows.ApplicationModel.Store**命名空間中的  [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx)。 使用此 API 時，您只能在使用者擁有已辨識的 Microsoft 帳戶情況下擷取[合格下載](#conversions)的自訂行銷活動識別碼。

>**注意**&nbsp;&nbsp;雖然 **Windows.ApplicationModel.Store** 命名空間適用於所有 Windows 10 版本，如果您的 App 以 Windows 10 1607 版或更新版本為目標，還是建議您使用 **Windows.Services.Store** 命名空間中的 API。 如需有關這些命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md#choose-namespace)。 下列程式碼範例說明如何建構程式碼，在相同專案中使用這兩個 API。

### <a name="code-example"></a>程式碼範例

下列程式碼範例說明如何擷取自訂行銷活動識別碼。 這個範例透過[版本調適型程式碼](../debug-test-perf/version-adaptive-code.md)使用 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空間中的這兩組 API。 您的程式碼可以依照此程序，在任何版本的 Windows 10 上執行。 若要使用此代碼，專案的目標作業系統版本必須是 **Windows 10 Anniversary Edition (10.0 版；組建 14394)** 或更新版本，雖然最小作業系統版本可以是較早的版本。

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

此程式碼會進行下列作業：

1. 首先，檢查 **Windows.Services.Store** 命名空間中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別是否適用於目前的裝置 (這表示裝置正在執行 Windows 10 1607 版或更新版本)。 如果是這樣，請繼續使用此類別。

2. 接下來，在目前的使用者擁有已辨識的 Microsoft 帳戶情況下嘗試取得自訂行銷活動識別碼。 為此，程式碼會取得表示目前 App SKU 的 [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) 物件，然後存取 [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata#Windows_Services_Store_StoreCollectionData_CampaignId) 屬性以擷取行銷活動識別碼 (如果有的話)。

2. 程式碼接著在目前使用者沒有已辨識的 Microsoft 帳戶情況下嘗試擷取行銷活動識別碼。 在此情況下，行銷活動識別碼是內嵌在 App 授權中。 程式碼會使用 [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppLicenseAsync) 方法擷取授權，然後對名稱為 *customPolicyField1* 的索引鍵值進行授權的 JSON 內容剖析。 這個值包含行銷活動識別碼。

3. 如果 **Windows.Services.Store** 命名空間中的 [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) 類別無法使用，程式碼就會回復為使用 **Windows.ApplicationModel.Store** 命名空間中的 [**GetAppPurchaseCampaignIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getapppurchasecampaignidasync.aspx) 方法，以擷取自訂行銷活動識別碼 (這個命名空間適用於所有 Windows 10 版本，包括 1511 版和更早版本)。 請注意，使用此方法時，您只能在使用者擁有已辨識的 Microsoft 帳戶情況下擷取[合格下載](#conversions)的自訂行銷活動識別碼。

  >**注意**&nbsp;&nbsp;**Windows.ApplicationModel.Store** 命名空間包含 [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator)，這是為了在提交 App 至市集之前測試程式碼而模擬市集作業的特殊類別。 此類別會從[名為 Windows.StoreProxy.xml 的本機檔案](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator)擷取資料。 上一個程式碼範例說明如何在專案的偵錯及非偵錯程式碼中同時使用 **CurrentApp** 和 **CurrentAppSimulator**。 若要在偵錯環境中測試此程式碼，請將 **AppPurchaseCampaignId** 項目新增至開發電腦上的 WindowsStoreProxy.xml 檔案，如下列範例所示。 執行 app 時，[**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator#Windows_ApplicationModel_Store_CurrentAppSimulator_GetAppPurchaseCampaignIdAsync) 方法永遠傳回這個值。

  ``` xml
  <CurrentApp>
      ...
      <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
  </CurrentApp>
  ```

  >**注意**&nbsp;&nbsp;**Windows.Services.Store** 命名空間沒有提供可在測試期間用來模擬授權資訊的類別。 您必須改為將 App 發行至市集，並將該 App 下載到您的開發裝置，才能使用它的授權進行測試。 如需詳細資訊，請參閱 [App 內購買和試用版](../monetize/in-app-purchases-and-trials.md#testing)。

## <a name="test-your-custom-campaign"></a>測試您的自訂行銷活動

促銷自訂行銷活動 URL 之前，建議您執行下列動作來測試自訂行銷活動：

1.  在您要用於測試的電腦或裝置上登入您的 Microsoft 帳戶。
2.  按一下您的自訂行銷活動 URL。 請確定 Windows 市集可正確載入您的 app 的頁面，然後關閉 Windows 市集 app 或瀏覽器頁面。
3.  按一下 URL 數次，在每次瀏覽到您的應用程式頁面後關閉 Windows 市集應用程式或瀏覽器頁面。 在對 App 頁面的其中一次瀏覽中，取得您的 App 以產生轉換。 計算您點選 URL 的總次數。
4. 確認預期的頁面檢視及轉換是否在[通道與轉換報告](channels-and-conversions-report.md)中的出現，然後測試 App 程式碼以確認其是否可以順利擷取行銷活動識別碼。
