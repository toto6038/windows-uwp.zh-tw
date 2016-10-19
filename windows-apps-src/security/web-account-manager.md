---
title: "使用 Web 帳戶管理員連線到身分識別提供者"
description: "本文章說明如何利用新的 Windows 10 Web 帳戶管理員 API，使用 AccountsSettingsPane 將您的通用 Windows 平台 (UWP) App 連線到外部身份識別提供者 (例如 Microsoft 或 Facebook)。"
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: f3cdb187ec4056d4c7db6acde471b0bc91c78390
ms.openlocfilehash: 093ca8906853121bbf33a729c523717d26cb7b0d

---
# 使用 Web 帳戶管理員連線到身分識別提供者

本文章說明如何使用新的 Windows 10 Web 帳戶管理員 API 顯示 AccountsSettingsPane，並將您的通用 Windows 平台 (UWP) App 連接到外部身份識別提供者 (例如 Microsoft 或 Facebook)。 您將了解如何要求使用者的權限以使用其 Microsoft 帳戶，取得存取權杖，並利用它來執行基本操作 (例如取得個人檔案資料，或上傳檔案到他們的 OneDrive)。 取得使用者權限的步驟，與透過任何支援 Web 帳戶管理員的身分識別提供者存取類似。

> 注意：如需完整的程式碼範例，請參閱 [GitHub 上的 WebAccountManagement 範例](http://go.microsoft.com/fwlink/p/?LinkId=620621)。

## 開始設定

首先，在 Visual Studio 中建立新的空白 App。 

其次，為了要連線到身分識別提供者，您必須將 App 與市集建立關聯。 若要這麼做，請以滑鼠右鍵按一下專案，選擇 [市集]****  >  [將 App 與市集建立關聯]****，然後遵循精靈的指示進行。 

接下來，建立非常基本的 UI，並在其中包含一個簡單的 XAML 按鈕以及兩個文字方塊。

```XML
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
    <Button x:Name="LoginButton" Content="Log in" Click="LoginButton_Click" />
    <TextBlock x:Name="UserIdTextBlock"/>
    <TextBlock x:Name="UserNameTextBlock"/>
</StackPanel>
```

接著在程式碼後置中將事件處理常式附加至該按鈕：

```C#
private void LoginButton_Click(object sender, RoutedEventArgs e)
{   
}
```

最後，新增下列命名空間，如此您稍後就無須擔心任何參照問題： 

```C#
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

## 顯示 AccountSettingsPane

系統提供稱為 AccountSettingsPane 的內建使用者介面，用來管理身分識別提供者和 Web 帳戶。 您可以透過下列方式顯示它：

```C#
private void LoginButton_Click(object sender, RoutedEventArgs e)
{
    AccountsSettingsPane.Show(); 
}
```

當您執行 App 並按一下 [登入] 按鈕，它應該會顯示空白視窗。 

![帳戶設定窗格](images/tb-1.png)

窗格之所以為空白，是因為系統僅提供 UI 殼層 - 開發人員必須以程式設計方式將身分識別提供者填入窗格。 

## 註冊 AccountCommandsRequested

若要將命令加入到窗格中，我們可以從註冊 AccountCommandsRequested 事件處理常式開始。 當使用者要求查看窗格時 (例如按一下 XAML 按鈕)，這會要求系統執行我們的建置邏輯。 

在程式碼後置中，覆寫 OnNavigatedTo 和 OnNavigatedFrom 事件，然後將下列程式碼新增到它們︰ 

```C#
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested += BuildPaneAsync; 
}
```

```C#
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
    AccountsSettingsPane.GetForCurrentView().AccountCommandsRequested -= BuildPaneAsync; 
}
```

使用者不會非常頻繁地與帳戶互動，因此以這種方式註冊和解除註冊事件處理常式有助於防止記憶體流失。 如此一來，當使用者有很大機率會要求查看您的自訂窗格時 (因為他們正位於 [設定] 或 [登入] 頁面)，該窗格只會存在於記憶體中。 

## 建置帳戶設定窗格

每當顯示 AccountSettingsPane 時，便會呼叫 BuildPaneAsync 方法。 這是我們將放置程式碼以自訂在窗格中顯示之命令的位置。 

從取得延遲開始。 這會告訴系統延遲顯示 AccountsSettingsPane，直到我們完成建置為止。

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    deferral.Complete(); 
}
```

接下來，使用 WebAuthenticationCoreManager.FindAccountProviderAsync 方法取得提供者。 提供者的 URL 會隨著提供者而有所不同，您可以在提供者的文件中找到。 針對 Microsoft 帳戶和 Azure Active Directory 而言，URL 為「https://login.microsoft.com」。 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s,
    AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    var deferral = e.GetDeferral();
        
    var msaProvider = await WebAuthenticationCoreManager.FindAccountProviderAsync(
        "https://login.microsoft.com", "consumers"); 
        
    deferral.Complete(); 
}
```

請注意，我們也會傳送字串「consumers」到選擇性的 *authority* 參數。 這是因為 Microsoft 提供兩種不同類型的驗證 - 針對「consumers (消費者)」的 Microsoft 帳戶 (MSA)，針對「organizations (組織)」的 Azure Active Directory (AAD)。 「consumers」授權可讓提供者知道我們對於前者選項有興趣。

如果您正在開發企業 App，您可能想要改用 AAD Graph 端點。 如需如何執行這項操作的詳細資訊，請參閱 [GitHub 上完整的 WebAccountManagement 範例](http://go.microsoft.com/fwlink/p/?LinkId=620621)和 Azure 文件。 

最後，建立新的 WebAccountProviderCommand，將提供者新增到 AccountsSettingsPane，如下所示： 

```C#
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

請注意，我們傳送到新的 WebAccountProviderCommand 的 GetMsaToken 方法還不存在 (我們將在後續步驟中建置)，因此目前請放心地將它新增為空白方法。

執行上述的程式碼，您的窗格看起來應該如下： 

![帳戶設定窗格](images/tb-2.png)

### 要求權杖

一旦我們在 AccountsSettingsPane 中顯示 Microsoft 帳戶選項，就必須處理使用者選取它時所發生的情況。 我們已註冊當使用者選擇使用他們的 Microsoft 帳戶登入時要引發的 GetMsaToken 方法，因此我們將在該處取得權杖。 

若要取得權杖，請使用 RequestTokenAsync 方法，如下所示： 

```C#
private async void GetMsaTokenAsync(WebAccountProviderCommand command)
{
    WebTokenRequest request = new WebTokenRequest(command.WebAccountProvider, "wl.basic");
    WebTokenRequestResult = await WebAuthenticationCoreManager.RequestTokenAsync(request);
}
```

在此範例中，我們傳送「wl.basic」字串到範圍參數。 範圍代表您從特定使用者提供的服務要求所要求的資訊類型。 某些範圍僅提供存取使用者的基本資訊，例如名稱和電子郵件地址。 其他範圍可能授與存取機密資訊的權限，例如使用者的相片或電子郵件收件匣。 一般而言，您的 App 應該使用最低的範圍，除非我們的 App 明確要求額外的權限 - 例如，請不要在您的 App 沒有絕對需要機密資訊的情況下要求存取它。 

服務提供者會提供文件，說明需要指定哪些範圍以取得權杖來使用他們的服務。 

針對 Office 365 和 Outlook.com 範圍，請參閱(使用 v2.0 驗證端點驗證 Office 365 和 Outlook.com API)[https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2]。 

針對 OneDrive，請參閱 (OneDrive 驗證與登入)[https://dev.onedrive.com/auth/msa_oauth.htm#authentication-scopes]。 

## 使用權杖

RequestTokenAsync 方法會傳回 WebTokenRequestResult 物件，它包含您要求的結果。 如果要求成功，它會包含權杖。  

```C#
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

> 注意︰如果您在要求權杖時收到錯誤，請確定您已將 App 與 [市集] 產生關聯，如步驟一所述。 如果您略過此步驟，您的 App 將無法取得權杖。 

一旦您擁有權杖之後，就可以使用它來呼叫您提供者的 API。 在下列程式碼中，我們將呼叫 Microsoft Live API 以取得關於使用者的基本資訊，然後將資訊顯示在我們的 UI 中。 

```C#
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

## 儲存帳戶狀態

權杖對於立即取得使用者的相關資訊非常有用，但它們通常具有不同的壽命 - 例如 MSA 權杖僅在幾個小時內有效。 幸運的是，您不需要每次在權杖到期時都重新顯示 AccountsSettingsPane。 使用者只要授權您的 App 一次，您就可以儲存使用者的帳戶資訊供以後使用。 

若要這樣做，請使用 WebAccount 類別。 WebAccount 會隨著要求權杖一起傳回：

```C#
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

一旦擁有 WebAccount 之後，您就可以輕鬆地儲存它。 在下列範例中，我們使用 LocalSettings： 

```C#
private async void StoreWebAccount(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values["CurrentUserProviderId"] = account.WebAccountProvider.Id;
    ApplicationData.Current.LocalSettings.Values["CurrentUserId"] = account.Id; 
}
```

下一次使用者啟動您的 App 時，您可以嘗試以無訊息方式 (在背景中) 取得權杖，如下所示： 

```C#
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

因為以無訊息方式取得權杖非常簡單，您應該使用此程序在工作階段之間重新整理權杖，而不是快取現有權杖 (因為該權杖隨時可能到期)。

請注意，上述範例僅涵蓋基本的成功與失敗情況。 您的 App 應該也要考量特殊案例 (例如使用者撤銷 App 的權限，或從 Windows 移除帳戶) 並妥善處理。  

## 登出帳戶 

如果您保留 WebAccount，您可能想要提供 [登出] 功能給使用者，以便他們可以切換帳戶，或簡單地將他們的帳戶與 App 取消關聯。 若要這樣做，請先移除任何儲存的帳戶和提供者資訊。 接著呼叫 WebAccount.SignOutAsync() 以清除快取，並使任何 App 可能擁有的現有權杖無效。 

```C#
private async Task SignOutAccountAsync(WebAccount account)
{
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserProviderId");
    ApplicationData.Current.LocalSettings.Values.Remove("CurrentUserId"); 
    account.SignOutAsync(); 
}
```

## 新增不支援 WebAccountManager 的提供者

如果您想要整合服務的驗證到您的 App，但該服務不支援 WebAccountManager (例如 Google+ 或 Twitter)，您仍然可以將該提供者手動新增到 AccountsSettingsPane。 若要這樣做，請建立新的 WebAccountProvider 物件，並提供您自己的名稱及 .png 圖示，然後將它新增到 WebAccountProviderCommands。 以下是一些虛設常式程式碼︰ 

 ```C#
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

請注意，這僅會將圖示加入到 AccountsSettingsPane，並在按一下圖示時執行您指定的方法 (在此案例中為 GetTwitterTokenAsync)。 您必須提供會處理實際驗證的程式碼。 如需詳細資訊，請參閱 (Web 驗證代理人)[web-authentication-broker]，它會提供協助程式方法，利用 REST 服務來進行驗證。 

## 新增自訂標頭

您可以使用 HeaderText 屬性來自訂帳戶設定窗格，如下所示： 

```C#
private async void BuildPaneAsync(AccountsSettingsPane s, AccountsSettingsPaneCommandsRequestedEventArgs e)
{
    // other code here 
    
    args.HeaderText = "MyAwesomeApp works best if you're signed in.";   
    
    // other code here
}
```

![帳戶設定窗格](images/tb-3.png)

請勿過度使用標頭文字；請將它維持簡短易記。 如果您的登入程序很複雜，而您需要顯示更多資訊，請使用自訂連結將使用者連結到另一個頁面。 

## 新增自訂連結

您可以將自訂命令加入到 AccountsSettingsPane，該命令在支援的 WebAccountProviders 底下會顯示為連結。 自訂命令非常適合用於與使用者關連的簡單工作，例如顯示隱私權原則，或針對遇到問題的使用者啟動支援頁面。 

以下是範例： 

```C#
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

## 另請參閱

[Windows.Security.Authentication.Web.Core 命名空間](https://msdn.microsoft.com/library/windows/apps/windows.security.authentication.web.core.aspx)

[Windows.Security.Credentials 命名空間](https://msdn.microsoft.com/library/windows/apps/windows.security.credentials.aspx)

[AccountsSettingsPane](https://msdn.microsoft.com/library/windows/apps/windows.ui.applicationsettings.accountssettingspane)

[Web 驗證代理人](web-authentication-broker.md)

[WebAccountManagement 範例](http://go.microsoft.com/fwlink/p/?LinkId=620621)



<!--HONumber=Aug16_HO3-->


