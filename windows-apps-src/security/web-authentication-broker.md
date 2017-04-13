---
title: "Web 驗證代理人"
description: "本文章說明如何將您的通用 Windows 平台 (UWP) 應用程式連線到使用授權通訊協定 (如 OpenID 或 OAuth) 的線上身分識別提供者，例如 Facebook、Twitter、Flickr、Instagram 等。"
ms.assetid: 05F06961-1768-44A7-B185-BCDB74488F85
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 0c5ca9146dd3b5bc04433ef9680af0c2d1009bf7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="web-authentication-broker"></a>Web 驗證代理人


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文章說明如何將您的通用 Windows 平台 (UWP) 應用程式連線到使用授權通訊協定 (如 OpenID 或 OAuth) 的線上身分識別提供者，例如 Facebook、Twitter、Flickr、Instagram 等。 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 方法會將要求傳送到線上身分識別提供者，然後取得說明 app 存取之提供者資源的存取權杖。

**注意** 如需完整的有效程式碼範例，請複製 [GitHub 上的 WebAuthenticationBroker 儲存機制](http://go.microsoft.com/fwlink/p/?LinkId=620622)。

 

## <a name="register-your-app-with-your-online-provider"></a>向線上提供者註冊 app


您必須在要連線的線上身分識別提供者上登錄應用程式。 您會知道如何向身分識別提供者登錄 app 的方法。 註冊之後，線上提供者通常會提供您應用程式的識別碼或祕密金鑰。

## <a name="build-the-authentication-request-uri"></a>建立驗證要求 URI


要求 URI 包含傳送驗證要求時所用的線上提供者位址以及其他所需的資訊 (例如應用程式識別碼或密碼)、驗證完成後使用者前往的重新導向 URI，以及預期的回應類型。 您可以向提供者詢問所需的參數。

要求 URI 會以 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 方法的 *requestUri* 參數形式傳送。 它必須是安全位址 (開頭必須為 https://)

下列範例顯示如何建立要求 URI。

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>&scope=<scopes>&response_type=token";
string endURL = "http://<appendpoint>";

System.Uri startURI = new System.Uri(startURL);
System.Uri endURI = new System.Uri(endURL);
```

## <a name="connect-to-the-online-provider"></a>連線到線上提供者


您呼叫 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 方法以連線到線上身分識別提供者並取得存取權杖。 這個方法使用上個步驟建構的 URI 做為 *requestUri* 參數，以及您要將使用者重新導向的 URI 做為 *callbackUri* 參數。

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI, 
        endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

除了 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212066) 以外，[**Windows.Security.Authentication.Web**](https://msdn.microsoft.com/library/windows/apps/br227044) 命名空間還包含 [**AuthenticateAndContinue**](https://msdn.microsoft.com/library/windows/apps/dn632425) 方法。 請不要呼叫此方法。 它是針對以 Windows Phone 8.1 為目標的應用程式設計的，從 Windows 10 開始即過時。

## <a name="connecting-with-single-sign-on-sso"></a>使用單一登入 (SSO) 連線。


根據預設，Web 驗證代理人不允許保留 Cookie。 基於這個原因，即使 app 使用者指出他們想要保持登入 (例如，透過在提供者登入對話方塊選取核取方塊)，仍然需要在每次想要存取該提供者的資源時進行登入。 若要使用 SSO 登入，您的線上身分識別提供者必須啟用 Web 驗證代理人的 SSO，而且 app 必須呼叫不會使用 *callbackUri* 參數的 [**AuthenticateAsync**](https://msdn.microsoft.com/library/windows/apps/br212068) 超載。

若要支援 SSO，線上提供者必須允許您以 `ms-app://`*appSID* 的形式登錄重新導向 URI，其中 *appSID* 是您 app 的 SID。 您可以在 app 的 app 開發人員頁面找到 app SID，或者呼叫 [**GetCurrentApplicationCallbackUri**](https://msdn.microsoft.com/library/windows/apps/br212069) 方法。

```cs
string result;

try
{
    var webAuthenticationResult = 
        await Windows.Security.Authentication.Web.WebAuthenticationBroker.AuthenticateAsync( 
        Windows.Security.Authentication.Web.WebAuthenticationOptions.None, 
        startURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.Success:
            // Successful authentication. 
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
        case Windows.Security.Authentication.Web.WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            result = webAuthenticationResult.ResponseErrorDetail.ToString(); 
            break;
        default:
            // Other error.
            result = webAuthenticationResult.ResponseData.ToString(); 
            break;
    } 
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and Network Unavailable errors here. 
    result = ex.Message;
}
```

## <a name="debugging"></a>偵錯


有數種方法可以疑難排解 Web 驗證代理人 API 的問題，包括使用 Fiddler 檢閱作業日誌和檢閱 Web 要求與回應。

### <a name="operational-logs"></a>作業記錄

通常您可以透過使用作業記錄來判斷哪裡出問題。 有一個專用的事件記錄檔通道 Microsoft-Windows-WebAuth\\Operational，可供網站開發人員了解 Web 驗證代理人如何處理他們的網頁。 如果要啟用作業記錄，請啟動 eventvwr.exe，然後啟用 [應用程式及服務\\Microsoft\\Windows\\WebAuth] 底下的 [作業] 記錄。 此外，Web 驗證代理人還會在使用者代理字串後面附加一個唯一字串，以便在網頁伺服器上識別自己。 這個字串是 "MSAuthHost/1.0"。 請注意，版本號碼在日後可能會有變更，因此在程式碼中不應該依據該版本號碼。 完整的使用者代理字串範例 (附有完整的偵錯步驟) 如下：

`User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; Win64; x64; Trident/6.0; MSAuthHost/1.0)`

1.  啟用作業記錄。
2.  執行 Contoso 社交應用程式。 ![顯示 WebAuth 作業記錄的事件檢視器](images/wab-event-viewer-1.png)
3.  產生的記錄項目可以用來進一步了解 Web 驗證代理人的行為。 在此情況下，這些可能包括：
    -   瀏覽開始：記錄 AuthHost 何時啟動，並且包含開始和終止 URL 的相關資訊。
    -   ![說明「瀏覽開始」的詳細資料](images/wab-event-viewer-2.png)
    -   瀏覽完成：記錄網頁載入完成。
    -   Meta 標記：記錄何時遇到 meta-tag，包含詳細資料。
    -   瀏覽終止：使用者終止瀏覽。
    -   瀏覽錯誤：AuthHost 在 URL 遇到瀏覽錯誤，包含 HttpStatusCode。
    -   瀏覽結束：遇到終止 URL。

### <a name="fiddler"></a>Fiddler

Fiddler Web 偵錯工具可以與 app 搭配使用。

1.  由於 AuthHost 是在自己的 App 容器中執行以給予私人網路功能，因此您必須設定登錄機碼：Windows Registry Editor Version 5.00

    **HKEY\_LOCAL\_MACHINE**\\**SOFTWARE**\\**Microsoft**\\**Windows NT**\\**CurrentVersion**\\**Image File Execution Options**\\**authhost.exe**\\**EnablePrivateNetwork** = 00000001

                         Data type  
                         DWORD

2.  為 AuthHost 新增一個規則，因為它負責產生輸出流量。
    ```syntax
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.a.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
    CheckNetIsolation.exe LoopbackExempt -a -n=microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
    D:\Windows\System32>CheckNetIsolation.exe LoopbackExempt -s
    List Loopback Exempted AppContainers
    [1] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.c_8wekyb3d8bbwe
        SID:  S-1-15-2-1973105767-3975693666-32999980-3747492175-1074076486-3102532000-500629349
    [2] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.sso.p_8wekyb3d8bbwe
        SID:  S-1-15-2-166260-4150837609-3669066492-3071230600-3743290616-3683681078-2492089544
    [3] -----------------------------------------------------------------
        Name: microsoft.windows.authhost.a.p_8wekyb3d8bbwe
        SID:  S-1-15-2-3506084497-1208594716-3384433646-2514033508-1838198150-1980605558-3480344935
    ```

3.  將連入流量防火牆規則新增到 Fiddler。