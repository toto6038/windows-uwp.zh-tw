---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: 使用「Windows 市集分析 API」，以程式設計方式擷取登錄到您或您組織的 Windows 開發人員中心帳戶的 app 分析資料。
title: 使用 Windows 市集服務存取分析資料
---

# 使用 Windows 市集服務存取分析資料


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


使用「Windows 市集分析 API」**，以程式設計方式擷取登錄到您或您組織的 Windows 開發人員中心帳戶的 app 分析資料。 這個 API 可讓您擷取 app 的資料和 IAP 的下載數、錯誤、app 評分與評論。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 app 或服務的呼叫。

## 使用 Windows 市集分析 API 的先決條件


-   您 (或您的組織) 必須有一個 Azure AD 目錄。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD 目錄。 否則，您可以[免費取得一個](http://go.microsoft.com/fwlink/p/?LinkId=703757)。
-   在您要與 Windows 開發人員中心帳戶相關聯的 Azure AD 目錄中，必須要有[使用者帳戶](https://azure.microsoft.com/documentation/articles/active-directory-create-users/)。

## 使用 Windows 市集分析 API


在您可以使用 Windows 市集分析 API 之前，必須先將 Azure AD 應用程式與開發人員中心帳戶產生關聯，並取得 Azure AD 存取權杖。 Azure AD 應用程式代表您要呼叫 Windows 市集分析 API 的 app 或服務。 有了存取權杖之後，您即可從您的 app 或服務呼叫 Windows 市集分析 API。

下列步驟說明端對端的程序：

1.  [將 Azure AD應用程式與您的 Windows 開發人員中心帳戶產生關聯](#associate-an-azure-ad-application-with-your-windows-dev-center-account)。
2.  [取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。
3.  [呼叫 Windows 市集分析 API](#call-the-windows-store-analytics-api)。


### 將 Azure AD 應用程式與您的 Windows 開發人員中心帳戶產生關聯

1.  在開發人員中心，移至您的 [帳戶設定]****，按一下 [管理使用者]****，將您組織的開發人員中心帳戶與您組織的 Azure AD 目錄產生關聯。 如需詳細指示，請參閱[管理帳戶使用者](https://msdn.microsoft.com/library/windows/apps/mt489008)。 您也可以選擇性地從組織的 Azure AD 目錄加入使用者，這樣他們也能存取開發人員帳戶。

    **注意** 一個 Azure Active Directory 只能與一個開發人員中心帳戶關聯。 同樣地，一個開發人員中心帳戶只能與一個 Azure Active Directory 關聯。 建立此關聯之後，您必須連絡支援人員才能將它移除。

     

2.  在 [管理使用者]**** 頁面中，按一下 [新增 Azure AD 應用程式]****，新增您要用來存取開發人員中心帳戶分析資料的 Azure AD 應用程式，並指派它為 [Manager]**** 角色。 如果這個應用程式已經存在於您的 Azure AD 目錄中，則您可以在 [新增 Azure AD 應用程式]**** 中選取它，以將其新增至您的開發人員中心帳戶。 否則，您可以在 [新增 Azure AD 應用程式]**** 頁面建立新的 Azure AD 應用程式。 如需詳細資訊，請參閱[管理帳戶使用者](https://msdn.microsoft.com/library/windows/apps/mt489008)中，管理 Azure AD 應用程式一節。

3.  返回 [管理使用者]**** 頁面，按一下您 Azure AD 應用程式的名稱來移至應用程式設定，然後按一下 [新增金鑰]****。 在下列畫面中，複製 [用戶端識別碼]**** 和 [金鑰]**** 的值。 如需詳細資訊，請參閱[管理帳戶使用者](https://msdn.microsoft.com/library/windows/apps/mt489008)中，管理 Azure AD 應用程式一節。 您需要此用戶端識別碼和金鑰來取得 Azure AD 存取權杖，以在呼叫 Windows 市集分析 API 時使用。 您離開這個頁面之後就無法再存取此資訊。


### 取得 Azure AD 存取權杖

當您將 Azure AD 應用程式與開發人員中心帳戶產生關聯，且已經擷取用戶端識別碼和金鑰之後，即可使用這些資訊來取得 Azure AD 存取權杖。 您需要存取權杖才能呼叫 Windows 市集分析 API 中的任何方法。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](https://msdn.microsoft.com/library/azure/dn645543.aspx)中的指示，來將 HTTP POST 傳送至下列的 Azure AD 端點。

```syntax
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

-   若要取得您的租用戶識別碼，請登入到 [Azure 管理入口網站](http://manage.windowsazure.com/)，瀏覽至 [Active Directory]****，然後按一下您要連結到開發人員中心帳戶的目錄。 此目錄的租用戶識別碼會內嵌在此頁面的 URL 中，如以下範例中所示的 *your\_tenant\_ID* 字串。

  ```syntax
  https://manage.windowsazure.com/@<your_tenant_name>#Workspaces/ActiveDirectoryExtension/Directory/<your_tenant_ID>/directoryQuickStart
  ```

-   針對 *client_id* 和 *client_secret* 參數，指定您稍早從開發人員中心抓取的應用程式用戶端識別碼和金鑰。
-   對於 *resource* 參數，請指定以下 URI：```https://manage.devcenter.microsoft.com```。


### 呼叫 Windows 市集分析 API

有了 Azure AD 存取權杖之後，您即可呼叫 Windows 市集分析 API。 如需每個方法之語法的相關資訊，請參閱下列文章。 您必須將存取權杖傳送給每個方法的 **Authorization** 標頭。

-   [取得 app 下載數](get-app-acquisitions.md)
-   [取得 IAP 下載數](get-in-app-acquisitions.md)
-   [取得錯誤報告資料](get-error-reporting-data.md)
-   [取得 app 評分](get-app-ratings.md)
-   [取得 app 評論](get-app-reviews.md)

## 程式碼範例


下列程式碼範例示範如何取得 Azure AD 存取權杖，並從 C# 主控台應用程式呼叫 Windows 市集分析 API。 若要使用此程式碼範例，請針對您的案例將 *tenantId*、*clientId*、*clientSecret* 和 *appID* 變數指定為適當的值。 此範例需要 Newtonsoft 的 [Json.NET package](http://www.newtonsoft.com/json)，。以還原序列化 Windows 市集分析 API 傳回的 JSON 資料。

```CSharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Threading.Tasks;

namespace TestAnalyticsAPI
{
    class Program
    {
        static void Main(string[] args)
        {
            string tenantId = "<your tenant ID>";
            string clientId = "<your client ID>";
            string clientSecret = "<your secret>";

            string scope = "https://manage.devcenter.microsoft.com";

            // Retrieve an Azure AD access token
            string accessToken = GetClientCredentialAccessToken(
                    tenantId,
                    clientId,
                    clientSecret,
                    scope).Result;

            // This is your app's product ID. This ID is embedded in the app's listing link
            // on the App identity page of the Dev Center dashboard.
            string appID = "<your app's product ID>";

            DateTime startDate = DateTime.Parse("08-01-2015");
            DateTime endDate = DateTime.Parse("11-01-2015");
            int pageSize = 1000;
            int startPageIndex = 0;

            // Call the Windows Store analytics API
            CallAnalyticsAPI(accessToken, appID, startDate, endDate, pageSize, startPageIndex);

            Console.Read();
        }

        private static void CallAnalyticsAPI(string accessToken, string appID, DateTime startDate, DateTime endDate, int top, int skip)
        {
            string requestURI;

            // Get app acquisitions
            requestURI = string.Format(
                "https://manage.devcenter.microsoft.com/v1.0/my/analytics/appacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
                appID, startDate, endDate, top, skip);

            //// Get IAP acquisitions
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/inappacquisitions?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app failures
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/failurehits?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app ratings
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/ratings?applicationId={0}&startDate={1}&endDate={2}top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            //// Get app reviews
            //requestURI = string.Format(
            //    "https://manage.devcenter.microsoft.com/v1.0/my/analytics/reviews?applicationId={0}&startDate={1}&endDate={2}&top={3}&skip={4}",
            //    appID, startDate, endDate, top, skip);

            HttpRequestMessage requestMessage = new HttpRequestMessage(HttpMethod.Get, requestURI);
            requestMessage.Headers.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);

            WebRequestHandler handler = new WebRequestHandler();
            HttpClient httpClient = new HttpClient(handler);

            HttpResponseMessage response = httpClient.SendAsync(requestMessage).Result;

            Console.WriteLine(response);
            Console.WriteLine(response.Content.ReadAsStringAsync().Result);

            response.Dispose();
        }

        public static async Task<string> GetClientCredentialAccessToken(string tenantId, string clientId, string clientSecret, string scope)
        {
            string tokenEndpointFormat = "https://login.microsoftonline.com/{0}/oauth2/token";
            string tokenEndpoint = string.Format(tokenEndpointFormat, tenantId);

            dynamic result;
            using (HttpClient client = new HttpClient())
            {
                string tokenUrl = tokenEndpoint;
                using (
                    HttpRequestMessage request = new HttpRequestMessage(
                        HttpMethod.Post,
                        tokenUrl))
                {
                    string content =
                        string.Format(
                            "grant_type=client_credentials&client_id={0}&client_secret={1}&resource={2}",
                            clientId,
                            clientSecret,
                            scope);

                    request.Content = new StringContent(content, Encoding.UTF8, "application/x-www-form-urlencoded");

                    using (HttpResponseMessage response = await client.SendAsync(request))
                    {
                        string responseContent = await response.Content.ReadAsStringAsync();
                        result = JsonConvert.DeserializeObject(responseContent);
                    }
                }
            }

            return result.access_token;
        }
    }
}
```

## 錯誤回應


Windows 市集分析 API 會以包含錯誤碼和訊息的 JSON 物件，傳回錯誤回應。 下列範例示範由無效的參數造成的錯誤回應。

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```

## 相關主題

* [取得 app 下載數](get-app-acquisitions.md)
* [取得 IAP 下載數](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得 app 評分](get-app-ratings.md)
* [取得 app 評論](get-app-reviews.md)
 


<!--HONumber=May16_HO2-->


