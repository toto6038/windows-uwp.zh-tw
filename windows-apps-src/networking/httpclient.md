---
author: stevewhims
description: 使用 HttpClient 和其餘 Windows.Web.Http 命名空間 API，透過 HTTP 2.0 與 HTTP 1.1 通訊協定傳送和接收資訊。
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c874c690826dfa74b8dcb2312204cd549db3db2b
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6192463"
---
# <a name="httpclient"></a>HttpClient


**重要 API**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 和其餘 [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間 API，透過 HTTP 2.0 與 HTTP 1.1 通訊協定傳送和接收資訊。

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>HttpClient 和 Windows.Web.Http 命名空間的概觀

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間和相關 [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) 與 [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623)命名空間中的類別提供了適用於通用 Windows 平台 (UWP) app 的程式設計介面，可做為 HTTP 用戶端來執行基本的 GET 要求或實作下列更進階的 HTTP 功能。

-   適用於常見動詞 (**DELETE**、**GET**、**PUT** 及 **POST**) 的方法。 每一個要求在傳送時，會以非同步作業的方式進行。

-   常見驗證設定與模式的支援。

-   存取傳輸相關的安全通訊端層 (SSL) 詳細資料。

-   在進階 app 中包含自訂篩選。

-   取得、設定及刪除 Cookie。

-   非同步方法中的 HTTP 要求進度資訊。

[**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) 類別代表 [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 所傳送的 HTTP 要求訊息。 [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 類別代表從 HTTP 要求收到的 HTTP 回應訊息。 HTTP 訊息是由 IETF 定義在 [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642) 中。

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間代表做為 HTTP 實體內容和標頭 (包含 Cookie) 的 HTTP 內容。 HTTP 內容可與 HTTP 要求或 HTTP 回應產生關聯。 **Windows.Web.Http** 命名空間提供一些不同的類別來代表 HTTP 內容。

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625)。 緩衝區形式的內容
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685)。 形式為以 **application/x-www-form-urlencoded** MIME 類型編碼之名稱/值 Tuple 的內容
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708)。 格式為 **multipart/\*** MIME 類型的內容。
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596)。 以 **multipart/form-data** MIME 類型編碼的內容。
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649)。 串流形式的內容 (HTTP GET 方法用來接收資料以及 HTTP POST 方法用來上傳資料的內部類型)
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661)。 字串形式的內容
-   [**IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684) - 開發人員用來建立他們自己的內容物件的基底介面

在 [透過 HTTP 傳送簡單 GET 要求] 一節的程式碼片段中，會使用 [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) 類別，以字串形式表示來自 HTTP GET 要求的 HTTP 回應。

[**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) 命名空間支援建立 HTTP 標頭與 Cookie，接著將它們當成屬性來與 [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) 和 [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) 物件產生關聯。

## <a name="send-a-simple-get-request-over-http"></a>透過 HTTP 傳送簡單 GET 要求

如本文先前所述，[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間可讓 UWP app 傳送 GET 要求。 下列程式碼片段示範如何傳送 GET 要求http://www.contoso.com使用[**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)類別和[**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)類別讀取 GET 要求的回應。

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

## <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http 中的例外狀況

如果傳送到 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 物件建構函式的統一資源識別項 (URI) 字串無效時，即會擲回例外狀況。

**.NET:** [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998)類型會顯示為 C# 和 VB 中[**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx)

在 C# 和 Visual Basic 中，可在建構 URI 之前，於 .NET 4.5 中使用 [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) 類別和其中一個 [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) 方法來測試接收自使用者的字串，以避免發生這個錯誤。

在 C++ 中，沒有可以嘗試將字串剖析為 URI 的方法。 如果應用程式取得使用者為 [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 輸入的值，則建構函式應在 try/catch 區塊中。 如果發生例外狀況，app 可通知使用者並要求新的主機名稱。

[**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 缺少便利的函式。 所以使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 的 app 及此命名空間中的其他類別需要使用 **HRESULT** 值。

在應用程式中使用 C#、 VB.NET、 [System.Exception](http://msdn.microsoft.com/library/system.exception.aspx) .NET Framework4.5 代表 app 執行期間時的錯誤發生例外狀況。 [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx) 屬性會傳回指派給特定例外狀況的 **HRESULT**。 [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx) 屬性會傳回描述例外狀況的訊息。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 app 可以篩選特定 **HRESULT** 值，依據例外狀況的發生原因來修改 app 行為。

在使用 Managed C++ 的 app 中，[Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx) 代表例外狀況發生時 app 執行期間的錯誤。 [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx) 屬性會傳回指派給特定例外狀況的 **HRESULT**。 [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx) 屬性會傳回與 **HRESULT** 值關聯的系統提供字串。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 app 可以篩選特定 **HRESULT** 值，依據例外狀況的發生原因來修改 app 行為。

針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E\_INVALIDARG**。 針對部分不正確的方法呼叫，傳回的 **HRESULT** 是 **E\_ILLEGAL\_METHOD\_CALL**。

