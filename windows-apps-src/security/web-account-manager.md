---
title: Web 帳戶管理員
description: 本文章說明如何利用 Windows10 Web 帳戶管理員 API，使用 AccountsSettingsPane 將您的通用 Windows 平台 (UWP) 應用程式連線到外部身份識別提供者 (例如 Microsoft 或 Facebook)。
author: PatrickFarley
ms.author: pafarley
ms.date: 12/6/2017
ms.topic: article
keywords: windows 10，uwp 安全性
ms.assetid: ec9293a1-237d-47b4-bcde-18112586241a
ms.localizationpriority: medium
ms.openlocfilehash: 71a5cddcd5ccb5185cda422c3df16797f5765688
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459808"
---
# <a name="web-account-manager"></a>Web 帳戶管理員

本文章說明如何利用 Windows10 Web 帳戶管理員 API，使用 **[AccountsSettingsPane](https://docs.microsoft.com/uwp/api/Windows.UI.ApplicationSettings.AccountsSettingsPane)** 將您的通用 Windows 平台 (UWP) 應用程式連線到外部身分識別提供者 (例如 Microsoft 或 Facebook)。 您將了解如何要求使用者的權限以使用其 Microsoft 帳戶，取得存取權杖，並利用它來執行基本操作 (例如取得個人檔案資料，或上傳檔案到他們的 OneDrive 帳戶)。 取得使用者權限的步驟，與透過任何支援 Web 帳戶管理員的身分識別提供者存取類似。

> [!NOTE]
> 如需完整的程式碼範例，請參閱 [GitHub 上的 WebAccountManagement 範例](http://go.microsoft.com/fwlink/p/?LinkId=620621)。

## <a name="get-set-up"></a>開始設定

首先，在 Visual Studio 中建立新的空白應用程式。 

其次，為了要連線到身分識別提供者，您必須將應用程式與市集建立關聯。 若要這麼做，請以滑鼠右鍵按一下專案，選擇 **\[市集\]** >  **\[將應用程式與市集建立關聯\]**，然後遵循精靈的指示進行。 

接下來，建立非常基本的 UI，並在其中包含一個簡單的 XAML 按鈕以及兩個文字方塊。

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

接著在程式碼後置中將事件處理常式附加至該按鈕：

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

最後，新增下列命名空間，如此您稍後就無須擔心任何參照問題： 

```csharp
using System;
using Windows.Security.Authentication.Web.Core;
using Windows.System;
using Windows.UI.ApplicationSettings;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Data.Json;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Http;
```

## <a name="show-the-accounts-settings-pane"></a>顯示帳戶設定窗格

系統提供稱為 **AccountsSettingsPane** 的內建使用者介面，用來管理身分識別提供者和 Web 帳戶。 您可以透過下列方式顯示它：

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

當您執行應用程式並按一下 [登入] 按鈕，它應該會顯示空白視窗。 

![帳戶設定窗格](images/tb-1.png)

窗格之所以為空白，是因為系統僅提供 UI 殼層 - 開發人員必須以程式設計方式將身分識別提供者填入窗格。 

## <a name="register-for-accountcommandsrequested"></a>註冊 AccountCommandsRequested

若要將命令加入到窗格中，我們可以從註冊 AccountCommandsRequested 事件處理常式開始。 當使用者要求查看窗格時 (例如按一下 XAML 按鈕)，這會要求系統執行我們的建置邏輯。 

在程式碼後置中，覆寫 OnNavigatedTo 和 OnNavigatedFrom 事件，然後將下列程式碼新增到它們︰ 

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

使用者不會非常頻繁地與帳戶互動，因此以這種方式註冊和解除註冊事件處理常式有助於防止記憶體流失。 如此一來，當使用者有很大機率會要求查看您的自訂窗格時 (因為他們正位於 [設定] 或 [登入] 頁面)，該窗格只會存在於記憶體中。 

## <a name="build-the-account-settings-pane"></a>建置帳戶設定窗格

每當顯示 **AccountsSettingsPane** 時，便會呼叫 BuildPaneAsync 方法。 這是我們將放置程式碼以自訂在窗格中顯示之命令的位置。 

從取得延遲開始。 這會告訴系統延遲顯示 **AccountsSettingsPane**，直到我們完成建置為止。

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

接下來，使用 WebAuthenticationCoreManager.FindAccountProviderAsync 方法取得提供者。 提供者的 URL 會隨著提供者而有所不同，您可以在提供者的文件中找到。 若是 Microsoft 帳戶和 Azure Active Directory，會是 "https://login.microsoft.com"。 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

請注意，我們也會傳送字串「consumers」到選擇性的 *authority* 參數。 這是因為 Microsoft 提供兩種不同類型的驗證 - 針對「consumers (消費者)」的 Microsoft 帳戶 (MSA)，針對「organizations (組織)」的 Azure Active Directory (AAD)。 「consumers (消費者)」授權表示我們想要 MSA 選項。 如果您是在開發企業應用程式，請改為使用「organizations (組織)」字串。

最後，建立新的 **[WebAccountProviderCommand](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings.webaccountprovidercommand)**，將提供者新增到 **AccountsSettingsPane**，如下所示： 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();

    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers");

    var command = new WebAccountProviderCommand(msaProvider, GetMsaTokenAsync);  

    e.WebAccountProviderCommands.Add(command);

    deferral.Complete(); 
}
```

我們傳送到新的 **WebAccountProviderCommand** 的 GetMsaToken 方法還不存在 (我們將在後續步驟中建置)，因此目前請放心地將它新增為空白方法。

執行上述的程式碼，您的窗格看起來應該如下： 

![帳戶設定窗格](images/tb-2.png)

### <a name="request-a-token"></a>要求權杖

一旦我們在 **AccountsSettingsPane** 中顯示 Microsoft 帳戶選項，就必須處理使用者選取它時所發生的情況。 我們已註冊當使用者選擇使用他們的 Microsoft 帳戶登入時要引發的 GetMsaToken 方法，因此我們將在該處取得權杖。 

若要取得權杖，請使用 RequestTokenAsync 方法，如下所示： 

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

在此範例中，我們傳送「wl.basic」字串到 _scope_ 參數。 範圍代表您從特定使用者提供的服務要求所要求的資訊類型。 某些範圍僅提供使用者基本資訊的存取，例如姓名和電子郵件地址，有些範圍可能授與敏感資訊的存取權，像是使用者的相片或電子郵件收件匣。 一般而言，您的 app 應該使用達成其功能所需的最低權限範圍。 服務提供者會提供文件，說明需要哪些範圍以取得權杖來使用他們的服務。 

* 針對 Office 365 和 Outlook.com 範圍，請參閱[使用 v2.0 驗證端點驗證 Office 365 和 Outlook.com API](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)。 
* 若為 OneDrive 範圍，請參閱 [OneDrive 驗證與登入](https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes)。 

> [!TIP]
> 或者，如果您的應用程式使用 （若要填入使用者欄位使用預設的電子郵件地址） 登入提示或其他特殊的登入體驗的相關的屬性，列出它**[WebTokenRequest.AppProperties](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webtokenrequest.appproperties#Windows_Security_Authentication_Web_Core_WebTokenRequest_AppProperties)** 屬性中。 這會導致系統當快取 web 帳戶，這會防止在快取帳戶不符會忽略此屬性。

如果您是在開發企業應用程式，您可以連線 Azure Active Directory (AAD) 執行個體，並使用 Microsoft Graph API，而不是使用一般的 MSA 服務。 在這個案例中，請改為使用以下程式碼： 

```csharp
private async void GetAadTokenAsync(WebAccountProviderCommand command)
{
    string clientId = "your_guid_here"; // Obtain your clientId from the Azure Portal
    WebTokenRequest request = new WebTokenRequest(provider, "User.Read", clientId);
    request.Properties.Add("resource", "https://graph.microsoft.com");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

本文其餘部分會繼續說明 MSA 案例，但 AAD 的程式碼是非常類似的。 如需 AAD/Graph 的詳細資訊，包括 GitHub 的完整範例，請參閱 [Microsoft Graph 文件](https://graph.microsoft.io/docs/platform/get-started)。

## <a name="use-the-token"></a>使用權杖

RequestTokenAsync 方法會傳回 WebTokenRequestResult 物件，它包含您要求的結果。 如果要求成功，它會包含權杖。  

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
    }
}
```

> [!NOTE]
> 如果您在要求權杖時收到錯誤，請確定您已將應用程式與 Store 產生關聯，如步驟一所述。 如果您略過此步驟，您的應用程式將無法取得權杖。 

一旦您擁有權杖之後，就可以使用它來呼叫您提供者的 API。 在下列程式碼中，我們將呼叫 [使用者資訊 Microsoft Live API](https://msdn.microsoft.com/library/hh826533.aspx) 以取得關於使用者的基本資訊，然後將資訊顯示在我們的 UI 中。 不過請注意，大多數情況下建議您在取得權杖後立即將它儲存，然後以不同的方法使用它。

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        string token = result.ResponseData[0].Token; 
        
        var restApi = new Uri(@"https://apis.live.net/v5.0/me?access_token=" + token);

        using (var client = new HttpClient())
        {
            var infoResult = await client.GetAsync(restApi);
            string content = await infoResult.Content.ReadAsStringAsync();

            var jsonObject = JsonObject.Parse(content);
            string id = jsonObject["id"].GetString();
            string name = jsonObject["name"].GetString();

            UserIdTextBlock.Text = "Id: " + id; 
            UserNameTextBlock.Text = "Name: " + name;
        }
    }
}
```

呼叫 REST API 的方式依提供者而有所不同；如需如何使用權杖的詳細資訊，請參閱提供者的 API 文件。 

## <a name="store-the-account-for-future-use"></a>儲存帳戶供未來使用

權杖對於立即取得使用者的相關資訊非常有用，但它們通常具有不同的壽命 - 例如 MSA 權杖僅在幾個小時內有效。 幸運的是，您不需要每次在權杖到期時都重新顯示 **AccountsSettingsPane**。 使用者只要授權您的應用程式一次，您就可以儲存使用者的帳戶資訊供以後使用。 

若要這樣做，請使用 **[WebAccount](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount)** 類別。 **WebAccount**會透過您用來要求權杖的相同方法傳回：

```csharp
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult result = await WebAuthenticationCoreManager.RequestTokenAsync(request);
    
    if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        WebAccount account = result.ResponseData[0].WebAccount; 
    }
}
```

一旦擁有 **WebAccount** 執行個體之後，您就可以輕鬆地儲存它。 在下列範例中，我們使用 LocalSettings。 如需有關使用 LocalSettings 和其他方法儲存使用者資料的詳細資訊，請參閱[儲存和擷取應用程式設定和資料](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

```csharp
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

然後，我們可以使用如下所示的非同步方法，嘗試在背景使用儲存的 **WebAccount** 取得權杖。

```csharp
private async Task<string> GetTokenSilentlyAsync()
{
    string providerId = ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"]?.ToString();
    string accountId = ApplicationData.Current.LocalSettings.Values["CurrentUserId"]?.ToString();

    if (null == providerId || null == accountId)
    {
        return null; 
    }

    WebAccountProvider provider = await WebAuthenticationCoreManager.FindAccountProviderAsync(providerId);
    WebAccount account = await WebAuthenticationCoreManager.FindAccountAsync(provider, accountId);

    WebTokenRequest request = new WebTokenRequest(provider, "wl.basic");

    WebTokenRequestResult result = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(request, account);
    if (result.ResponseStatus == WebTokenRequestStatus.UserInteractionRequired)
    {
        // Unable to get a token silently - you'll need to show the UI
        return null; 
    }
    else if (result.ResponseStatus == WebTokenRequestStatus.Success)
    {
        // Success
        return result.ResponseData[0].Token;
    }
    else
    {
        // Other error 
        return null; 
    }
}
```

將上述方法放在建置 **AccountsSettingsPane** 的程式碼正前方。 如果權杖是在背景中取得，則不需要顯示窗格。 

```csharp
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    string silentToken = await GetMsaTokenSilentlyAsync();

    if (silentToken != null)
    {
        // the token was obtained. store a reference to it or do something with it here.
    }
    else
    {
        // the token could not be obtained silently. Show the AccountsSettingsPane
        AccountsSettingsPane.Show();
    }
}
```

因為以無訊息方式取得權杖非常簡單，您應該使用此程序在工作階段之間重新整理權杖，而不是快取現有權杖 (因為該權杖隨時可能到期)。

> [!NOTE]
> 上述範例僅涵蓋基本的成功與失敗情況。 您的應用程式應該也要考量特殊案例 (例如使用者撤銷應用程式的權限，或從 Windows 移除帳戶) 並妥善處理。  

## <a name="remove-a-stored-account"></a>移除儲存的帳戶

如果您保留 Web 帳戶，建議您讓使用者能夠將自己的帳戶與您的 app 解除關聯。 如此一來，它們可以有效地 「 登出 」 的應用程式： 他們的帳戶資訊將不會再會自動在啟動時載入。 若要這樣做，請先移除儲存空間中的任何儲存的帳戶和提供者資訊。 接著呼叫 **[SignOutAsync](https://docs.microsoft.com/uwp/api/windows.security.credentials.webaccount.SignOutAsync)** 以清除快取，並使任何應用程式可能擁有的現有權杖無效。 

```csharp
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## <a name="add-providers-that-dont-support-webaccountmanager"></a>新增不支援 WebAccountManager 的提供者

如果您想要整合服務的驗證到您的應用程式，但該服務不支援 WebAccountManager (例如 Google+ 或 Twitter)，您仍然可以將該提供者手動新增到 **AccountsSettingsPane**。 若要這樣做，請建立新的 WebAccountProvider 物件，並提供您自己的名稱及 .png 圖示，然後將它新增到 WebAccountProviderCommands 清單。 以下是一些虛設常式程式碼︰ 

 ```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 

    var twitterProvider = new WebAccountProvider("twitter", "Twitter", new Uri(@"ms-appx:///Assets/twitter-auth-icon.png")); 
    var twitterCmd = new WebAccountProviderCommand(twitterProvider, GetTwitterTokenAsync);
    e.WebAccountProviderCommands.Add(twitterCmd);   
    
    // other code here
}

private async void GetTwitterTokenAsync(WebAccountProviderCommand command)
{
    // Manually handle Twitter login here
}

```

> [!NOTE] 
> 這僅會將圖示加入到 **AccountsSettingsPane**，並在按一下圖示時執行您指定的方法 (在此案例中為 GetTwitterTokenAsync)。 您必須提供會處理實際驗證的程式碼。 如需詳細資訊，請參閱 (Web 驗證代理人)[web-authentication-broker]，它會提供協助程式方法，利用 REST 服務來進行驗證。 

## <a name="add-a-custom-header"></a>新增自訂標頭

您可以使用 HeaderText 屬性來自訂帳戶設定窗格，如下所示： 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![帳戶設定窗格](images/tb-3.png)

請勿過度使用標頭文字；請將它維持簡短易記。 如果您的登入程序很複雜，而您需要顯示更多資訊，請使用自訂連結將使用者連結到另一個頁面。 

## <a name="add-custom-links"></a>新增自訂連結

您可以將自訂命令加入到 AccountsSettingsPane，該命令在支援的 WebAccountProviders 底下會顯示為連結。 自訂命令非常適合用於與使用者關連的簡單工作，例如顯示隱私權原則，或針對遇到問題的使用者啟動支援頁面。 

以下是範例： 

```csharp
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    var settingsCmd = new SettingsCommand(
        "settings_privacy", 
        "Privacy policy", 
        async (x) => await Launcher.LaunchUriAsync(new Uri(@"https://privacy.microsoft.com/en-US/"))); 

    e.Commands.Add(settingsCmd); 
    
    // other code here
}
```

![帳戶設定窗格](images/tb-4.png)

理論上，您可以使用 settings 命令來做任何事情。 不過，我們建議僅將它們使用在直覺、與帳戶相關的案例上，例如上述的案例。 

## <a name="see-also"></a>另請參閱

[Windows.Security.Authentication.Web.Core 命名空間](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials 命名空間](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Web 驗證代理人](web-authentication-broker.md)

[Web 帳戶管理範例](http://go.microsoft.com/fwlink/p/?LinkId=620621)

[Lunch Scheduler (午餐排程器) app](https://github.com/Microsoft/Windows-appsample-lunch-scheduler)
