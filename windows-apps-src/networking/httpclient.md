---
description: 使用 HttpClient 和其餘 Windows.Web.Http 命名空間 API，透過 HTTP 2.0 與 HTTP 1.1 通訊協定傳送和接收資訊。
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 6/5/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bb098aae346c7a81771262793f5f6a042d62d5a3
ms.sourcegitcommit: 1f39b67f2711b96c6b4e7ed7107a9a47127d4e8f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2019
ms.locfileid: "66721605"
---
# <a name="httpclient"></a>HttpClient

**重要的 Api**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

使用 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 和其餘 [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空間 API，透過 HTTP 2.0 與 HTTP 1.1 通訊協定傳送和接收資訊。

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient 和 Windows.Web.Http 命名空間的概觀

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空間和相關 [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 與 [**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters)命名空間中的類別提供了適用於通用 Windows 平台 (UWP) app 的程式設計介面，可做為 HTTP 用戶端來執行基本的 GET 要求或實作下列更進階的 HTTP 功能。

-   適用於常見動詞 (**DELETE**、**GET**、**PUT** 及 **POST**) 的方法。 每一個要求在傳送時，會以非同步作業的方式進行。

-   常見驗證設定與模式的支援。

-   存取傳輸相關的安全通訊端層 (SSL) 詳細資料。

-   在進階 app 中包含自訂篩選。

-   取得、設定及刪除 Cookie。

-   非同步方法中的 HTTP 要求進度資訊。

[  **Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) 類別代表 [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 所傳送的 HTTP 要求訊息。 [  **Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) 類別代表從 HTTP 要求收到的 HTTP 回應訊息。 HTTP 訊息是由 IETF 定義在 [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) 中。

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空間代表做為 HTTP 實體內容和標頭 (包含 Cookie) 的 HTTP 內容。 HTTP 內容可與 HTTP 要求或 HTTP 回應產生關聯。 **Windows.Web.Http** 命名空間提供一些不同的類別來代表 HTTP 內容。

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent)。 緩衝區形式的內容
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent)。 形式為以 **application/x-www-form-urlencoded** MIME 類型編碼之名稱/值 Tuple 的內容
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent)。 內容的形式**multipart /\***  MIME 類型。
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent)。 以 **multipart/form-data** MIME 類型編碼的內容。
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent)。 串流形式的內容 (HTTP GET 方法用來接收資料以及 HTTP POST 方法用來上傳資料的內部類型)
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent)。 字串形式的內容
-   [**IHttpContent** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) -開發人員建立自己的內容物件的基底介面

在 [透過 HTTP 傳送簡單 GET 要求] 一節的程式碼片段中，會使用 [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent) 類別，以字串形式表示來自 HTTP GET 要求的 HTTP 回應。

[  **Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) 命名空間支援建立 HTTP 標頭與 Cookie，接著將它們當成屬性來與 [**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) 和 [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) 物件產生關聯。

## <a name="send-a-simple-get-request-over-http"></a>透過 HTTP 傳送簡單 GET 要求

如本文先前所述，[**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 命名空間可讓 UWP app 傳送 GET 要求。 下列程式碼片段示範如何將傳送 GET 要求 http://www.contoso.com使用[ **Windows.Web.Http.HttpClient** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)類別並[ **Windows.Web.Http.HttpResponseMessage** ](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)類別以讀取自 GET 要求的回應。

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>透過 HTTP POST 二進位資料

[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis)下列程式碼範例說明使用表單資料和 POST 要求傳送至 web 伺服器，檔案上傳為少量的二進位資料。 程式碼會使用[ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent)類別來代表二進位資料，而[ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent)類別代表多部分表單資料。

> [!NOTE]
> 呼叫**取得**（如下列程式碼範例所示） 並不適合用於 UI 執行緒。 若要在此情況下使用正確的技術，請參閱[並行和非同步作業C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

若要張貼的內容，實際的二進位檔案 （而不是上述範例中使用明確的二進位資料），您會發現這使用的工作變得更容易[HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent)物件。 建構，並為其建構函式的引數，傳遞從呼叫傳回的值[StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync)。 該方法會傳回資料流以供您的二進位檔內的資料。

此外，如果您上傳大型檔案 （大於大約 10 MB），則我們建議您使用 Windows 執行階段[背景傳送](/uwp/api/windows.networking.backgroundtransfer)Api。

## <a name="post-json-data-over-http"></a>透過 HTTP POST JSON 資料

下列範例會張貼到端點，某些 JSON，然後寫出回應主體。

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http 中的例外狀況

如果傳送到 [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 物件建構函式的統一資源識別項 (URI) 字串無效時，即會擲回例外狀況。

**.NET:**    [ **Windows.Foundation.Uri** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri)類型會顯示為[ **System.Uri** ](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN)中C#和VB。

在 C# 和 Visual Basic 中，可在建構 URI 之前，於 .NET 4.5 中使用 [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) 類別和其中一個 [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads) 方法來測試接收自使用者的字串，以避免發生這個錯誤。

在 C++ 中，沒有可以嘗試將字串剖析為 URI 的方法。 如果應用程式取得使用者為 [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) 輸入的值，則建構函式應在 try/catch 區塊中。 如果發生例外狀況，app 可通知使用者並要求新的主機名稱。

[  **Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) 缺少便利的函式。 所以使用 [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 的 app 及此命名空間中的其他類別需要使用 **HRESULT** 值。

使用.NET Framework 4.5 中的應用程式C#、 VB.NET [System.Exception](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN)例外狀況發生時，表示應用程式執行期間的錯誤。 [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult) 屬性會傳回指派給特定例外狀況的 **HRESULT**。 [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message) 屬性會傳回描述例外狀況的訊息。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 app 可以篩選特定 **HRESULT** 值，依據例外狀況的發生原因來修改 app 行為。

在使用 Managed C++ 的 app 中，[Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) 代表例外狀況發生時 app 執行期間的錯誤。 [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) 屬性會傳回指派給特定例外狀況的 **HRESULT**。 [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) 屬性會傳回與 **HRESULT** 值關聯的系統提供字串。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 app 可以篩選特定 **HRESULT** 值，依據例外狀況的發生原因來修改 app 行為。

對於大多數的參數驗證錯誤， **HRESULT**傳回**E\_INVALIDARG**。 有些不合法的方法呼叫，如**HRESULT**傳回**E\_不合法\_方法\_呼叫**。

