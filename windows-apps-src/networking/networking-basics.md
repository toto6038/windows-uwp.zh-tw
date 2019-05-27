---
description: 您對於任何具備網路功能的 app 所需執行的動作。
title: 網路功能基本知識
ms.assetid: 1F47D33B-6F00-4F74-A52D-538851FD38BE
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8ed003fbae285f003724e5f540612d86902ee2d4
ms.sourcegitcommit: f282c906cddf0d57217484e61a5cbd2fe8469421
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2019
ms.locfileid: "65852249"
---
# <a name="networking-basics"></a>網路功能基本知識
您對於任何具備網路功能的 app 所需執行的動作。

## <a name="capabilities"></a>功能
若要使用網路，您必須在 app 資訊清單中新增適當的功能元素。 如果未在您的 app 資訊清單中指定任何網路功能，您的 app 將沒有網路功能，並連接到網路的任何嘗試都將失敗。

以下是最常用的網路功能。

| 功能 | 描述 |
|------------|-------------|
| **internetClient** | 提供公共場所 (例如機場和咖啡廳) 的網際網路與網路的對外存取。 大部分需要網際網路存取的應用程式都應使用此功能。 |
| **internetClientServer** | 透過公共場所 (例如機場和咖啡廳) 的網際網路和網路提供 app 對內及對外網路存取。 |
| **privateNetworkClientServer** | 在使用者信任的場所提供 app 的對內及對外網路存取，例如家中與工作場所。 |

您的 app 在某些情況下有可能需要其他功能。

| 功能 | 描述 |
|------------|-------------|
| **enterpriseAuthentication** | 允許 app 連線至需要網域認證的網路資源。 例如，應用程式從私人內部網路上的 SharePoint 伺服器擷取資料。 透過此功能，您的認證可用來在需要認證的網路上存取網路資源。 具有此功能的應用程式可在網路上模擬您。 您不需要這項功能，以便在您的應用程式透過驗證的 proxy 存取網際網路。<br/><br/>如需詳細資訊，請參閱文件*企業*中的功能案例[限制功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。 |
| **proximity** | 與非常靠近電腦的裝置進行近距離鄰近性通訊時所需。 近距離鄰近性可用來傳送或與鄰近裝置上的應用程式連線。 <br/><br/> 這個功能可讓 app 存取網路以連線至非常靠近的裝置，只要使用者同意傳送邀請或是接受邀請即可。 |
| **sharedUserCertificates** | 這個功能可讓 app 存取軟體和硬體憑證，例如智慧卡憑證。 在執行階段叫用這個功能時，使用者必須採取行動，例如插入卡片或是選取憑證。 <br/><br/> 透過這個功能，您的軟體與硬體憑證或智慧卡可供應用程式識別身分。 您的員工、銀行或政府服務單位可使用這個功能來識別身分。 |

## <a name="communicating-when-your-app-is-not-in-the-foreground"></a>App 不在前景時進行通訊
[使用背景工作支援應用程式](https://msdn.microsoft.com/library/windows/apps/mt299103)包含當 app 不在前景時，使用背景工作執行工作的一般資訊。 具體而言，如果 app 不是目前的前景 app，您的程式碼必須執行特殊步驟，才可在資料透過網路送達時接收通知。 控制通道的觸發程序用於此目的，在 Windows 8 和 Windows 10 中仍然支援它們。 如需使用控制通道觸發程序的完整資訊，請參閱 [**here**](https://msdn.microsoft.com/library/windows/apps/hh701032)。 Windows 10 的新技術提供更佳的功能使用，請在某些情況下，例如啟用推播的資料流通訊端的額外負荷較低： 通訊端訊息代理程式和通訊端活動觸發程序。

如果您的 app 使用 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)、[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 或 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906)，則您的 app 可以將開啟之通訊端的擁有權轉換給系統所提供的通訊端代理程式，然後離開前景，或甚至終止。 當轉換的通訊端建立連線，或流量到達該通訊端時，表示您的 app 或其指定的背景工作已啟用。 如果您的 app 未執行，將在此時啟動。 接著，通訊端代理程式會使用 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009) 通知您的 app 有新流量到達。 您的 app 會從通訊端代理程式回收通訊端，並處理通訊端上的流量。 這表示當您的 app 未主動處理網路流量時，所耗用的系統資源會大幅降低。

通訊端代理程式將在適用的環境中取代控制通道觸發程序，因為它可提供相同的功能，但限制較少，所需使用的記憶體也較少。 通訊端代理程式可供不是鎖定畫面 app 的 app 使用，且其在手機與其他裝置上的使用方式是相同的。 app 在流量到達時不需要執行，即可由通訊端代理程式啟用。 通訊端代理程式支援接聽 TCP 通訊端，控制通道觸發程序則不支援。

### <a name="choosing-a-network-trigger"></a>選擇網路觸發程序
在某些情況下，任一觸發程序均適用。 當您選擇要在您的 app 中使用的觸發程序時，請考量下列建議。

-   如果您使用 [**IXMLHTTPRequest2**](https://msdn.microsoft.com/library/windows/desktop/hh831151)、[**System.Net.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 或 [System.Net.Http.HttpClientHandler](https://go.microsoft.com/fwlink/p/?linkid=241638)，您必須使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)。
-   如果使用啟用推播的 **StreamSockets**，您可以使用控制通道觸發程序，但應優先選擇使用 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)。 後者可讓系統在連線未積極使用時釋出記憶體並降低電源需求。
-   如果您想要讓 app 在不主動處理網路要求時將其記憶體使用量降到最低，您應盡可能使用 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)。
-   如果您想要讓應用程式能夠在系統處於「連線待命」模式時接收資料、請使用 [**SocketActivityTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806009)。

如需如何使用通訊端代理程式的詳細資訊和範例，請參閱[背景網路通訊](network-communications-in-the-background.md)。

## <a name="secured-connections"></a>安全連線
安全通訊端層 (SSL) 與較新的傳輸層安全性 (TLS) 是密碼編譯通訊協定，其設計目的在於提供網路通訊的驗證與加密。 這些通訊協定的設計目的在於防止傳送和接收網路資料時遭到竊取和竄改。 這些通訊協定使用用戶端伺服器模型以進行通訊協定交換。 這些通訊協定也使用數位憑證與憑證授權單位，以驗證該伺服器是否為其本身所宣稱的伺服器。

### <a name="creating-secure-socket-connections"></a>建立安全的通訊端連線
[  **StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 物件可用來設定在用戶端與伺服器之間使用 SSL/TLS 進行通訊。 對於 SSL/TLS 的支援，受限於使用 **StreamSocket** 物件做為 SSL/TLS 交涉中的用戶端。 您無法將 SSL/TLS 用於由 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 在收到連入通訊時建立的 **StreamSocket**，因為 **StreamSocket** 類別沒有實作做為伺服器的 SSL/TLS 交涉。

有兩種方式可使用 SSL/TLS 保護 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 連線：

-   [**ConnectAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701504) -進行初始連線至網路服務，然後立即交涉要使用 SSL/TLS 的所有通訊。
-   [**UpgradeToSslAsync** ](https://msdn.microsoft.com/library/windows/apps/br226922) -一開始連接到未加密的網路服務。 應用程式可能會傳送或接收資料。 然後為所有進一步的通訊將連線升級成使用 SSL/TLS。

SocketProtectionLevel 指定應用程式想用來建立或升級連線的所需通訊端保護層級。 不過，已建立的連線的最終保護層級取決於連線的這兩個端點之間的交涉程序。 如果其他端點要求較低的層級，結果可能是比您指定的要更低的保護層級。 

 順利完成非同步作業後，您可以透過 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 屬性擷取 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 或 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 呼叫中使用的要求保護層級。 不過，這不會反映連線所使用的實際保護層級。

> [!NOTE]
> 您的程式碼不應該以隱含方式依賴使用特定的保護層級，或是依預設使用提供的安全性層級的假設。 安全性概況經常變更，為避免使用含有已知弱點的通訊協定，通訊協定和預設保護層級會隨著時間變更。 依據個別的電腦設定或安裝的軟體及套用的修補程式而定，預設值可能會有所不同。 如果您的應用程式需要使用特定的安全性層級，那麼您必須明確地指定層級，並確定它實際上已在建立的連線中使用。

### <a name="use-connectasync"></a>使用 ConnectAsync
[**ConnectAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701504)可用來建立初始連接以網路服務，然後再立即使用 SSL/TLS 的所有通訊。 有兩種 **ConnectAsync** 方法可支援傳遞 *protectionLevel* 參數：

-   [**（EndpointPair、 SocketProtectionLevel） ConnectAsync** ](https://msdn.microsoft.com/library/windows/apps/hh701511) -上啟動非同步作業[ **StreamSocket** ](https://msdn.microsoft.com/library/windows/apps/br226882)物件來連接至遠端網路目的地指定為[ **EndpointPair** ](https://msdn.microsoft.com/library/windows/apps/hh700953)物件並[ **SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880)。
-   [**(主機名稱、 字串、 SocketProtectionLevel) ConnectAsync** ](https://msdn.microsoft.com/library/windows/apps/br226916) -上啟動非同步作業[ **StreamSocket** ](https://msdn.microsoft.com/library/windows/apps/br226882)物件來連接至遠端目的地指定遠端主機名稱、 遠端服務名稱，以及[ **SocketProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/br226880)。

如果在呼叫上面的 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 方法時，將 *protectionLevel* 參數設定為 **Windows.Networking.Sockets.SocketProtectionLevel.Ssl**，則必須建立 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 以使用 SSL/TLS 來加密。 這個值需要加密而且絕不允許使用 NULL 密碼。

與其中一個 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 方法搭配使用的一般順序是相同的。

-   建立一個 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)。
-   如果需要通訊端上的進階選項，請使用 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) 屬性取得與 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 物件關聯的 [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) 執行個體。 設定 **StreamSocketControl** 上的屬性。
-   呼叫上面其中一個 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 方法以啟動作業來連線至遠端目的地，並立即交涉以使用 SSL/TLS。
-   順利完成非同步作業後，實際上使用 [**ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/hh701504) 交涉的 SSL 強度可以透過取得 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 屬性來決定。

下列範例會建立 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 並嘗試建立網路服務的連線，然後立即交涉以使用 SSL/TLS。 如果交涉成功，將會加密在用戶端與網路伺服器之間使用 **StreamSocket** 的所有網路通訊。

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
     
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "https";
    
    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 
    
    // Try to connect to contoso using HTTPS (port 443)
    try {

        // Call ConnectAsync method with SSL
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.Ssl);

        NotifyUser("Connected");
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }
        
        NotifyUser("Connect failed with error: " + exception.Message);
        // Could retry the connection, but for this simple example
        // just close the socket.
        
        clientSocket.Dispose();
        clientSocket = null; 
    }
           
    // Add code to send and receive data using the clientSocket
    // and then close the clientSocket
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>

using namespace winrt;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages.

    // Try to connect to the server using HTTPS and SSL (port 443).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::Tls12);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }
    // Add code to send and receive data using the clientSocket,
    // then set the clientSocket to nullptr when done to close it.
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    HostName^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "https";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to the server using HTTPS and SSL (port 443)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::SSL)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
            
            clientSocket.Close();
            clientSocket = null;
        }
    });
    // Add code to send and receive data using the clientSocket
    // Then close the clientSocket when done
```

### <a name="use-upgradetosslasync"></a>使用 UpgradeToSslAsync
當您的程式碼使用 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 時，它會先在不加密的情況下建立網路服務的連線。 應用程式可以傳送或接收部分資料，然後為所有進一步的通訊將連線升級成使用 SSL/TLS。

[  **UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 方法需要兩個參數。 *protectionLevel* 參數指示所需的保護層級。 *validationHostName* 參數是在升級成 SSL 時用於驗證的遠端網路目的地的主機名稱。 通常 *validationHostName* 是 app 用來初次建立連線的相同主機名稱。 如果在呼叫 **UpgradeToSslAsync** 時將 *protectionLevel* 參數設定為 **Windows.System.Socket.SocketProtectionLevel.Ssl**，則 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 必須使用 SSL/TLS 針對透過通訊端的進一步通訊加密。 這個值需要加密而且絕不允許使用 NULL 密碼。

與 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 方法搭配使用的一般順序如下：

-   建立一個 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)。
-   如果需要通訊端上的進階選項，請使用 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226917) 屬性取得與 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 物件關聯的 [**StreamSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226893) 執行個體。 設定 **StreamSocketControl** 上的屬性。
-   如果需要以未加密的方式傳送和接收任何資料，請立即傳送。
-   呼叫 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 方法以啟動作業，將連線升級成使用 SSL/TLS。
-   順利完成非同步作業後，實際上使用 [**UpgradeToSslAsync**](https://msdn.microsoft.com/library/windows/apps/br226922) 交涉的 SSL 強度可以透過取得 [**StreamSocketinformation.ProtectionLevel**](https://msdn.microsoft.com/library/windows/apps/hh967868) 屬性來決定。

下列範例會建立 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)、嘗試建立網路服務的連線、傳送部分初始資料，然後交涉使用 SSL/TLS。 如果交涉成功，會加密在用戶端與網路伺服器之間使用 **StreamSocket** 的所有網路通訊。

```csharp
using Windows.Networking;
using Windows.Networking.Sockets;
using Windows.Storage.Streams;

    // Define some variables and set values
    StreamSocket clientSocket = new StreamSocket();
 
    HostName serverHost = new HostName("www.contoso.com");
    string serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    try {
        // Call ConnectAsync method with a plain socket
        await clientSocket.ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel.PlainSocket);

        NotifyUser("Connected");

    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Connect failed with error: " + exception.Message, NotifyType.ErrorMessage);
        // Could retry the connection, but for this simple example
        // just close the socket.

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }

    // Now try to send some data
    DataWriter writer = new DataWriter(clientSocket.OutputStream);
    string hello = "Hello, World! ☺ ";
    Int32 len = (int) writer.MeasureString(hello); // Gets the UTF-8 string length.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser("Client: sending hello");

    try {
        // Call StoreAsync method to store the hello message
        await writer.StoreAsync();

        NotifyUser("Client: sent data");

        writer.DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
    }
    catch (Exception exception) {
        NotifyUser("Store failed with error: " + exception.Message);
        // Could retry the store, but for this simple example
            // just close the socket.

            clientSocket.Dispose();
            clientSocket = null; 
            return;
    }

    // Now upgrade the client to use SSL
    try {
        // Try to upgrade to SSL
        await clientSocket.UpgradeToSslAsync(SocketProtectionLevel.Ssl, serverHost);

        NotifyUser("Client: upgrade to SSL completed");
           
        // Add code to send and receive data 
        // The close clientSocket when done
    }
    catch (Exception exception) {
        // If this is an unknown status it means that the error is fatal and retry will likely fail.
        if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
            throw;
        }

        NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

        clientSocket.Dispose();
        clientSocket = null; 
        return;
    }
```

```cppwinrt
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt;
using namespace Windows::Storage::Streams;
...
    // Define some variables, and set values.
    Windows::Networking::Sockets::StreamSocket clientSocket;

    Windows::Networking::HostName serverHost{ L"www.contoso.com" };
    winrt::hstring serverServiceName{ L"https" };

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages. 

    // Try to connect to the server using HTTP (port 80).
    try
    {
        co_await clientSocket.ConnectAsync(serverHost, serverServiceName, Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket);
        NotifyUser(L"Connected");
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Connect failed with error: " + exception.message());
        clientSocket = nullptr;
    }

    // Now, try to send some data.
    DataWriter writer{ clientSocket.OutputStream() };
    winrt::hstring hello{ L"Hello, World! ☺ " };
    uint32_t len{ writer.MeasureString(hello) }; // Gets the size of the string, in bytes.
    writer.WriteInt32(len);
    writer.WriteString(hello);
    NotifyUser(L"Client: sending hello");

    try
    {
        co_await writer.StoreAsync();
        NotifyUser(L"Client: sent hello");

        writer.DetachStream(); // Detach the stream when you want to continue using it; otherwise, the DataWriter destructor closes it.
    }
    catch (winrt::hresult_error const& exception)
    {
        NotifyUser(L"Store failed with error: " + exception.message());
        // We could retry the store operation. But, for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }

    // Now, upgrade the client to use SSL.
    try
    {
        co_await clientSocket.UpgradeToSslAsync(Windows::Networking::Sockets::SocketProtectionLevel::Tls12, serverHost);
        NotifyUser(L"Client: upgrade to SSL completed");

        // Add code to send and receive data using the clientSocket,
        // then set the clientSocket to nullptr when done to close it.
    }
    catch (winrt::hresult_error const& exception)
    {
        // If this is an unknown status, then the error is fatal and retry will likely fail.
        Windows::Networking::Sockets::SocketErrorStatus socketErrorStatus{ Windows::Networking::Sockets::SocketError::GetStatus(exception.to_abi()) };
        if (socketErrorStatus == Windows::Networking::Sockets::SocketErrorStatus::Unknown)
        {
            throw;
        }

        NotifyUser(L"Upgrade to SSL failed with error: " + exception.message());
        // We could retry the store operation. But for this simple example, just close the socket by setting it to nullptr.
        clientSocket = nullptr;
        co_return;
    }
```

```cpp
using Windows::Networking;
using Windows::Networking::Sockets;
using Windows::Storage::Streams;

    // Define some variables and set values
    StreamSocket^ clientSocket = new ref StreamSocket();
 
    Hostname^ serverHost = new ref HostName("www.contoso.com");
    String serverServiceName = "http";

    // For simplicity, the sample omits implementation of the
    // NotifyUser method used to display status and error messages 

    // Try to connect to contoso using HTTP (port 80)
    task<void>(clientSocket->ConnectAsync(serverHost, serverServiceName, SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask) {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();
            NotifyUser("Connected");
        }
        catch (Exception^ exception)
        {
            NotifyUser("Connect failed with error: " + exception->Message);
 
            clientSocket->Close();
            clientSocket = null;
        }
    });
       
    // Now try to send some data
    DataWriter^ writer = new ref DataWriter(clientSocket.OutputStream);
    String hello = "Hello, World! ☺ ";
    Int32 len = (int) writer->MeasureString(hello); // Gets the UTF-8 string length.
    writer->writeInt32(len);
    writer->writeString(hello);
    NotifyUser("Client: sending hello");

    task<void>(writer->StoreAsync()).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

            NotifyUser("Client: sent hello");

            writer->DetachStream(); // Detach stream, if not, DataWriter destructor will close it.
       }
       catch (Exception^ exception) {
               NotifyUser("Store failed with error: " + exception->Message);
               // Could retry the store, but for this simple example
               // just close the socket.
 
               clientSocket->Close();
               clientSocket = null;
               return
       }
    });

    // Now upgrade the client to use SSL
    task<void>(clientSocket->UpgradeToSslAsync(clientSocket.SocketProtectionLevel.Ssl, serverHost)).then([this] (task<void> previousTask) {
        try {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.Get();

           NotifyUser("Client: upgrade to SSL completed");
           
           // Add code to send and receive data 
           // Then close clientSocket when done
        }
        catch (Exception^ exception) {
            // If this is an unknown status it means that the error is fatal and retry will likely fail.
            if (SocketError.GetStatus(exception.HResult) == SocketErrorStatus.Unknown) {
                throw;
            }

            NotifyUser("Upgrade to SSL failed with error: " + exception.Message);

            clientSocket->Close();
            clientSocket = null; 
            return;
        }
    });
```

### <a name="creating-secure-websocket-connections"></a>建立安全的 WebSocket 連線
如同傳統型的通訊端連線，在使用 UWP app 中的 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 功能時，也可以使用傳輸層安全性 (TLS)/安全通訊端層 (SSL) 加密 WebSocket 連線。 在大部分情況下，您會想使用安全的 WebSocket 連線。 這將會增加連線成功的機率，因為許多 Proxy 都會拒絕未加密的 WebSocket 連線。

如需如何建立 (或升級至) 連線到網路服務的安全通訊端連線的範例，請參閱[如何使用 TLS/SSL 保護 WebSocket 連線](https://msdn.microsoft.com/library/windows/apps/xaml/hh994399)。

除了 TLS/SSL 加密，伺服器可能還需要一個 **Sec-WebSocket-Protocol** 標頭值才能完成初始交握。 由 [**StreamWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701514) 和 [**MessageWebSocketInformation.Protocol**](https://msdn.microsoft.com/library/windows/apps/hh701358) 屬性代表的這個值，指出連線的通訊協定版本，並可讓伺服器正確解譯正在開啟的交握和以後交換的資料。 使用這個通訊協定資訊，如果伺服器在任何時間無法以安全的方式解譯連入資料，就可以關閉連線。

如果來自用戶端的起始要求不包含這個值，或提供的值不符合伺服器所期待，當發生 WebSocket 交握錯誤時，預期的值就會從伺服器傳送到用戶端。

## <a name="authentication"></a>驗證
如何在透過網路連線時提供驗證認證。

### <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>提供具有 StreamSocket 類別的用戶端憑證
[  **Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 類別支援使用 SSL/TLS 來驗證與 app 交談的伺服器。 在某些情況下，app 也必須使用 TLS 的用戶端憑證向伺服器驗證本身。 在 Windows 10 中，您可以在提供用戶端憑證[ **StreamSocket.Control** ](https://msdn.microsoft.com/library/windows/apps/br226893) （這必須設定啟動 TLS 交握之前） 的物件。 如果伺服器要求用戶端憑證，Windows 會使用提供的憑證來回應。

以下程式碼片段說明其實作方式：

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

### <a name="providing-authentication-credentials-to-a-web-service"></a>提供驗證認證到 Web 服務
能讓 app 與安全 Web 服務互動的網路 API，每一個都提供自己的方法來初始化用戶端，或是使用伺服器和 Proxy 驗證認證來設定要求標頭。 使用指出使用者名稱、密碼以及使用這些認證之資源的 [**PasswordCredential**](https://msdn.microsoft.com/library/windows/apps/br227061) 物件設定每個方法。 下表提供這些 API 的對應：

| **WebSocket** | [**MessageWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226848) |
|-------------------------|----------------------------------------------------------------------------------------------------------|
|  | [**MessageWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226847) |
|  | [**StreamWebSocketControl.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br226928) |
|  | [**StreamWebSocketControl.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br226927) |
| **背景傳送** | [**BackgroundDownloader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701076) |
|  | [**BackgroundDownloader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701068) |
|  | [**BackgroundUploader.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/hh701184) |
|  | [**BackgroundUploader.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/hh701178) |
| **Syndication** | [**SyndicationClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702355) |
|  | [**SyndicationClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) |
|  | [**SyndicationClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) |
| **AtomPub** | [**AtomPubClient(PasswordCredential)**](https://msdn.microsoft.com/library/windows/apps/hh702262) |
|  | [**AtomPubClient.ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243428) |
|  | [**AtomPubClient.ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243423) |

## <a name="handling-network-exceptions"></a>處理網路例外狀況
在程式設計大部分的領域中，例外狀況意味著程式中有某些缺失導致嚴重的問題或失敗。 網路程式設計還有其他因素會導致例外狀況：網路本身，以及網路通訊的本質。 網路通訊原本就不穩定，而容易發生未預期的失敗。 針對您每個使用網路的 app，您必須維護某些狀態資訊；且您的 app 程式碼必須藉由更新該狀態資訊，並為 app 初始化適當的邏輯以重新建立或重試通訊失敗，以處理網路例外狀況。

當通用 Windows app 擲回例外狀況時，您的例外狀況處理常式可以擷取例外狀況發生原因的更詳細資訊，更清楚地了解失敗的情況並作出適當的決定。

每個語言投影支援存取這個更詳細資訊的方法。 例外狀況會在通用 Windows app 中投影為 **HRESULT** 值。 *Winerror.h* 包含的檔案包含一個非常大的可能 **HRESULT** 值清單，其中包含網路錯誤。

網路 API 支援不同的方法來抓取例外狀況發生原因的更詳細資訊。

-   有些 API 會提供協助程式方法，將例外狀況的 **HRESULT** 值轉換為例舉值。
-   其他 API 則提供擷取實際 **HRESULT** 值的方法。

## <a name="related-topics"></a>相關主題
* [Windows 10 中的網路功能 API 增強功能](https://blogs.windows.com/buildingapps/2015/07/02/networking-api-improvements-in-windows-10/)
