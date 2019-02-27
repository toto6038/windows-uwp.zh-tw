---
title: 遊戲的網路功能
description: 了解如何在您的 DirectX 遊戲中開發與納入網路功能。
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 遊戲, 網路功能, directx
ms.localizationpriority: medium
ms.openlocfilehash: e3dc77b48feb0c7ceba9fa3cede82c1a44687d0d
ms.sourcegitcommit: 175d0fc32db60017705ab58136552aee31407412
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9114624"
---
# <a name="networking-for-games"></a>遊戲的網路功能



了解如何在您的 DirectX 遊戲中開發與納入網路功能。

## <a name="concepts-at-a-glance"></a>概念簡介


不論您的 DirectX 遊戲是簡單的獨立遊戲或大型的多人遊戲，都可在遊戲中使用多種不同的網路功能。 最簡單的網路運用就是將使用者名稱與遊戲分數儲存在中央網路伺服器。

使用基礎結構 (主從式架構或網際網路點對點) 模型的多人遊戲以及臨機操作 (本機點對點) 遊戲皆需要網路 API。 對於伺服器型的多人遊戲，通常中央遊戲伺服器會處理大部分遊戲作業，而用戶端遊戲應用程式則用於輸入、顯示圖形、播放音訊與其他功能。 網路傳輸的速度與延遲是獲得滿意的遊戲體驗的一個關鍵點。

對於點對點遊戲，每個玩家的應用程式會處理輸入與圖形。 在大多數情況下，遊戲玩家位於鄰近位置，因此網路延遲的狀況較輕微，但仍然值得關注。 如何探索其他玩家，並建立連線成為一個重要問題。

對於單人遊戲，通常會使用中央 Web 伺服器或服務來儲存使用者名稱、遊戲分數與其他資訊。 在這些遊戲中，因為網路傳輸的速度與延遲不會直接影響遊戲運作，因此不是太重要。

網路狀況會隨時改變，因此所有使用網路功能 API 的遊戲都必須能處理可能發生的網路例外狀況。 若要深入了解如何處理網路例外狀況，請參閱[網路功能基本知識](https://msdn.microsoft.com/library/windows/apps/mt280233)。

防火牆與 Web Proxy 很常見，且可能影響網路功能的使用。 使用網路的遊戲必須預備好能正確處理防火牆與 Proxy。

對於行動裝置，在漫遊或資料成本昂貴的計量付費網路中的，監控可用的網路資源及操作行為是很重要的。

網路隔離是 Windows 使用的應用程式安全性模型的一部分。 Windows 會主動探索網路界限並為網路隔離強制網路存取限制。 應用程式必須宣告網路隔離功能，才能定義網路存取的範圍。 若未宣告這些功能，應用程式將無法存取網路資源。 若要 深入了解 Windows 如何強制執行應用程式的網路隔離，請參閱[如何設定網路隔離功能](https://msdn.microsoft.com/library/windows/apps/hh770532)。

## <a name="design-considerations"></a>設計考量


DirectX 遊戲可使用多種不同的網路 API。 因此，挑選正確的 API 是很重要的。 Windows 支援各種不同的網路 API，讓您的 應用程式可以透過網際網路或私人網路與其他電腦或裝置通訊。 您首先要做的是找出應用程式所需的網路功能。

適用於遊戲的熱門網路 API 包含：

-   TCP 與通訊端 - 提供可靠的連線。 在不需安全性的遊戲中使用 TCP。 TCP 能輕鬆擴充伺服器，因此常用於使用基礎結構 (主從式架構或網際網路點對點) 模型的遊戲。 Wi-Fi Direct 與藍牙上的臨機 (本機點對點) 遊戲也可使用 TCP。 TCP 常用於遊戲物件動作、角色互動、文字交談與其他作業。 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)類別提供 TCP 通訊端，可以使用 Microsoft Store 遊戲中。 **StreamSocket** 類別與 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間中的相關類別搭配使用。
-   使用 SSL 的 TCP 與通訊端 - 提供能防竊聽的可靠連線。 針對需要安全性的遊戲使用 TCP 連線搭配 SSL。 SSL 的加密與額外負荷會增加延遲並影響效能，請只在需要安全性時使用。 TCP 搭配 SSL 常用於登入、購買與交易資產、遊戲角色建立與管理。 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 類別提供支援 SSL 的 TCP 通訊端。
-   UDP 與通訊端 - 提供不可靠的網路傳輸，但額外負荷小。 UDP 用於要求低延遲且可容許某些封包遺失的遊戲作業。 它常用於搏鬥遊戲、射擊與追蹤、網路音訊與視訊聊天。 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)類別提供 UDP 通訊端，可以使用 Microsoft Store 遊戲中。 **DatagramSocket** 類別與 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間中的相關類別搭配使用。
-   HTTP 用戶端 - 提供可靠的 HTTP 伺服器連線。 最常見的網路案例是存取網站以擷取或儲存資訊。 使用網站來儲存使用者資訊與遊戲分數的遊戲就是一個簡單的例子。 為求安全性搭配 SSL 使用時，可使用 HTTP 用戶端來登入、購買、交易資產、遊戲角色建立與管理。 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)類別提供的現代化 HTTP 用戶端 API 使用 Microsoft Store 遊戲中。 **HttpClient** 類別與 [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) 命名空間中的相關類別搭配使用。

## <a name="handling-network-exceptions-in-your-directx-game"></a>處理您的 DirectX 遊戲中的網路例外狀況


當您的 DirectX 遊戲中發生網路例外狀況時，表示有嚴重的問題或失敗。 使用網路 API 時，有許多原因會導致例外狀況發生。 例外狀況通常是因為網路連線變更，或是遠端主機或伺服器發生其他網路問題。

使用網路 API 時，發生例外狀況的一些原因包括下列項目：

-   使用者輸入的主機名稱或 URI 包含錯誤和無效。
-   查詢主機名稱或 URI 時名稱解析失敗。
-   網路連線中斷或變更。
-   使用通訊端或 HTTP 用戶端 API 的網路連線失敗。
-   網路伺服器或遠端端點錯誤。
-   其他網路錯誤。

網路錯誤 (例如，連線中斷或變更、連線失敗、伺服器故障) 的例外狀況隨時都有可能發生。 這些錯誤會造成擲出例外狀況。 如果您的應用程式不處理例外狀況，它可能會造成您整個應用程式被執行階段終止。

您必須撰寫程式碼，以處理在呼叫大多數非同步網路方法時的例外狀況。 有時候，在例外狀況發生時，會以重試網路方法的方式來解決問題。 其他時候，您的 app 需要規劃，在沒有網路連線的情況下，使用之前快取的資料繼續工作。

通用 Windows 平台 (UWP) app 通常會擲回單一例外狀況。 您的例外狀況處理常式可以擷取例外狀況發生原因的詳細資訊，更清楚地了解失敗的情況並作出適當的決定。

當 UWP app 的 DirectX 遊戲發生例外狀況時，會擷取導致錯誤的 **HRESULT** 值。 *Winerror.h* 包含的檔案含有一個大型的可能 **HRESULT** 值清單，其中包含網路錯誤。

網路 API 支援不同的方法來抓取例外狀況發生原因的更詳細資訊。

-   擷取導致例外狀況的錯誤 **HRESULT** 值的方法。 可能的 **HRESULT** 值的可能性清單過大且未指定。 使用任何網路 API 都可擷取 **HRESULT** 值。
-   將 **HRESULT** 值轉換為列舉值的協助程式方法。 可能的列舉值清單已指定，而且相對過小。 [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 的通訊端類別可使用協助程式方法。

### <a name="exceptions-in-windowsnetworkingsockets"></a>Windows.Networking.Sockets 中的例外狀況

如果傳送的字串不是有效的主機名稱 (包含不允許在主機名稱中使用的字元)，與通訊端一起使用的 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 類別的建構函式會發生例外狀況。 如果 app 取得使用者在遊戲的對等連線為 **HostName** 輸入的值，則建構函式應在 try/catch 區塊中。 如果發生例外狀況，app 可通知使用者並要求新的主機名稱。

新增程式碼以驗證使用者輸入的主機名稱字串

```cpp

    // Define some variables at the class level.
    Windows::Networking::HostName^ remoteHost;

    bool isHostnameFromUser = false;
    bool isHostnameValid = false;

    ///...

    // If the value of 'remoteHostname' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^hostString = remoteHostname;

    try 
    {
        remoteHost = ref new Windows::Networking:Host(hostString);
        isHostnameValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
        return;
    }

    isHostnameFromUser = true;


    // ... Continue with code to execute with a valid hostname.
```

[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間有便利的協助程式方法及列舉，在使用通訊端時用來處理錯誤。 這對於在您的應用程式中以不同的方式處理特定網路例外狀況時很有用。

[**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)、[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 或 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 作業中若發生錯誤，會導致例外狀況。 例外狀況的原因是以 **HRESULT** 值表示的錯誤值。 使用 [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) 方法，將通訊端作業的網路錯誤轉換為 [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457) 列舉值。 大多數 **SocketErrorStatus** 列舉值對應原始 Windows 通訊端作業傳回的錯誤。 app 可以篩選特定 **SocketErrorStatus** 列舉值，依據例外狀況的發生原因來修改 app 行為。

針對參數驗證錯誤，app 也可以使用來自例外狀況的 **HRESULT**，深入了解更多關於導致例外狀況的錯誤詳細資訊。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E\_INVALIDARG**。

新增程式碼以處理嘗試建立串流通訊端連線時發生的例外狀況

```cpp
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;

    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Windows.Web.Http 中的例外狀況

如果傳送的字串不是有效的 URI (包含不允許在 URI 中使用的字元)，與 [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 一起使用的 [**Windows::Foundation::Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) 類別的建構函式會發生例外狀況。 在 C++ 中，沒有可以嘗試將字串剖析為 URI 的方法。 如果 app 取得使用者為 **Windows::Foundation::Uri** 輸入的值，則建構函式應在 try/catch 區塊中。 如果發生例外狀況，app 可通知使用者並要求新的 URI。

您的應用程式也應檢查 URI 中的配置是 HTTP 或 HTTPS，因為 [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 僅支援這些配置。

新增程式碼以驗證使用者輸入的 URI 字串

```cpp

    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

[**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/windows.web.http.aspx) 命名空間缺少便利的函式。 所以使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 的 app 及此命名空間中的其他類別需要使用 **HRESULT** 值。

在使用 C++ 的 app 中，[**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) 代表 app 執行期間發生例外狀況時的錯誤。 [**Platform::Exception::HResult**](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) 屬性會傳回指派給特定例外狀況的 **HRESULT**。 [**Platform::Exception::Message**](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) 屬性會傳回與 **HRESULT** 值關聯的系統提供字串。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 app 可以篩選特定 **HRESULT** 值，依據例外狀況的發生原因來修改 app 行為。

針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E\_INVALIDARG**。 針對部分不正確的方法呼叫，傳回的 **HRESULT** 是 **E\_ILLEGAL\_METHOD\_CALL**。

新增程式碼以處理嘗試使用 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 連線至 HTTP 伺服器時發生的例外狀況

```cpp
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
    

```

## <a name="related-topics"></a>相關主題


**其他資源**

* [使用資料包通訊端進行連線](https://msdn.microsoft.com/library/windows/apps/xaml/jj635238)
* [利用資料流通訊端連線到網路資源](https://msdn.microsoft.com/library/windows/apps/xaml/jj150599)
* [連線到網路服務](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
* [連線到 Web 服務](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
* [網路功能基本知識](https://msdn.microsoft.com/library/windows/apps/mt280233)
* [如何設定網路隔離功能](https://msdn.microsoft.com/library/windows/apps/hh770532)
* [如何啟用回送以及偵錯網路隔離](https://msdn.microsoft.com/library/windows/apps/hh780593)

**參考資料**

* [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)
* [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
* [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)
* [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
* [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

**範例**

* [DatagramSocket 範例](https://go.microsoft.com/fwlink/p/?LinkID=243037)
* [HttpClient 範例]( https://go.microsoft.com/fwlink/p/?linkid=242550)
* [鄰近性範例](https://go.microsoft.com/fwlink/p/?linkid=245082)
* [StreamSocket 範例](https://go.microsoft.com/fwlink/p/?linkid=243037)
