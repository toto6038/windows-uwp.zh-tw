---
author: mcleanbyron
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: "使用「Windows 市集分析 API」，以程式設計方式擷取登錄到您或您組織的 Windows 開發人員中心帳戶的 app 分析資料。"
title: "使用 Windows 市集服務存取分析資料"
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 1293bb5beb927425928d832f887129263db5a895

---

# 使用 Windows 市集服務存取分析資料

使用「Windows 市集分析 API」**，以程式設計方式擷取登錄到您或您組織的 Windows 開發人員中心帳戶的 App 分析資料。 這個 API 可讓您擷取 App 和附加元件 (也稱為應用程式內產品或 IAP) 下載數、錯誤、App 評分與評論的資料。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

下列步驟說明端對端的程序：

1.  請確定您已經完成所有[先決條件](#prerequisites)。
2.  在呼叫 Windows 市集分析 API 中的方法之前，請先[取得 Azure AD 存取權杖](#obtain-an-azure-ad-access-token)。 取得權杖之後，在權杖到期之前，您有 60 分鐘的時間可以使用這個權杖呼叫 Windows 市集分析 API。 權杖到期之後，您可以產生新的權杖。
3.  [呼叫 Windows 市集分析 API](#call-the-windows-store-analytics-api)。

<span id="prerequisites" />
## 步驟 1：完成使用 Windows 市集分析 API 的先決條件

開始撰寫程式碼以呼叫 Windows 市集分析 API 之前，請先確定您已完成下列先決條件。

* 您 (或您的組織) 必須擁有 Azure AD 目錄，而且您必須具備目錄的[全域系統管理員](http://go.microsoft.com/fwlink/?LinkId=746654)權限。 如果您已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經擁有 Azure AD 目錄。 如果沒有，可以免費[在開發人員中心建立新的 Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

* 您必須將 Azure AD 應用程式與開發人員中心帳戶相關聯、擷取應用程式的租用戶識別碼和用戶端識別碼，並產生金鑰。 Azure AD 應用程式代表您要呼叫 Windows 市集分析 API 的 App 或服務。 您需要租用戶識別碼、用戶端識別碼和金鑰，才能取得傳遞給 API 的 Azure AD 存取權杖。

  >**注意**&nbsp;&nbsp;您只需要執行此工作一次。 有了租用戶識別碼、用戶端識別碼和金鑰，每當您必須建立新的 Azure AD 存取權杖時，就可以重複使用它們。

將 Azure AD 應用程式與您的 Windows 開發人員中心帳戶產生關聯並擷取需要的值：

1.  在開發人員中心，移至您的 [帳戶設定]****，按一下 [管理使用者]****，將您組織的開發人員中心帳戶與您組織的 Azure AD 目錄產生關聯。 如需詳細指示，請參閱[管理帳戶使用者](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

2.  在 [管理使用者]**** 頁面中，按一下 [新增 Azure AD 應用程式]****，新增代表您要用來存取開發人員中心帳戶分析資料之 App 或服務的 Azure AD 應用程式，並指派 [管理員]**** 角色給它。 如果這個應用程式已經存在於您的 Azure AD 目錄中，則您可以在 [新增 Azure AD 應用程式]**** 頁面中選取它，以將其新增至您的開發人員中心帳戶。 如果不是，可以在 [新增 Azure AD 應用程式]**** 頁面建立新的 Azure AD 應用程式。 如需詳細資訊，請參閱[新增和管理 Azure AD 應用程式](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)。

3.  返回 [管理使用者]**** 頁面，按一下您 Azure AD 應用程式的名稱來移至應用程式設定，然後複製 [租用戶識別碼]**** 和 [用戶端識別碼]**** 的值。

4. 按一下 [加入新的金鑰]****。 在下列畫面中，複製 [金鑰]**** 的值。 您離開這個頁面之後就無法再存取此資訊。 如需詳細資訊，請參閱[新增和管理 Azure AD 應用程式](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)中管理金鑰的相關資訊。

<span id="obtain-an-azure-ad-access-token" />
## 步驟 2：取得 Azure AD 存取權杖

在 Windows 市集分析 API 中呼叫任何方法之前，您必須先取得傳遞至 API 中每個方法的 **Authorization** 標頭的 Azure AD 存取權杖。 在您取得存取權杖之後，您在權杖到期之前有 60 分鐘的時間可以使用權杖。 權杖到期之後，您可以重新整理權杖以便在後續呼叫 API 時繼續使用。

若要取得存取權杖，請按照[使用用戶端認證的服務對服務呼叫](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的指示，將 HTTP POST 傳送至 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 端點。 以下是範例要求。

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

對於 *tenant\_id*、*client\_id* 和 *client\_secret* 參數，指定您在上一節從開發人員中心為應用程式擷取的租用戶識別碼、用戶端識別碼和金鑰。 對於 *resource* 參數，您必須指定 ```https://manage.devcenter.microsoft.com``` URI。

存取權杖到期之後，您可以按照[這裡](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的指示，重新整理權杖。

<span id="call-the-windows-store-analytics-api" />
## 步驟 3：呼叫 Windows 市集分析 API

有了 Azure AD 存取權杖之後，就可以呼叫 Windows 市集分析 API。 如需每個方法之語法的相關資訊，請參閱下列文章。 您必須將存取權杖傳送給每個方法的 **Authorization** 標頭。

-   [取得應用程式下載數](get-app-acquisitions.md)
-   [取得附加元件下載數](get-in-app-acquisitions.md)
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

            // This is your app's Store ID. This ID is available on
            // the App identity page of the Dev Center dashboard.
            string appID = "<your app's Store ID>";

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

            //// Get add-on acquisitions
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

* [取得應用程式下載數](get-app-acquisitions.md)
* [取得附加元件下載數](get-in-app-acquisitions.md)
* [取得錯誤報告資料](get-error-reporting-data.md)
* [取得 app 評分](get-app-ratings.md)
* [取得 app 評論](get-app-reviews.md)
 



<!--HONumber=Sep16_HO1-->


