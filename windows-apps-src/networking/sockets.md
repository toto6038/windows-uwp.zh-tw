---
author: DelfCo
description: "您可以使用 Windows.Networking.Sockets 和 Winsock，以通用 Windows 平台 (UWP) 的 app 開發人員的身分與其他裝置通訊。"
title: "通訊端"
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0e9121dfc590a1a7f67be69b7dbce475e438dd08
ms.lasthandoff: 02/07/2017

---

# <a name="sockets"></a>通訊端

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)

您可以使用 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 和 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms737523)，以通用 Windows 平台 (UWP) 的 app 開發人員的身分與其他裝置通訊。 本主題提供使用 **Windows.Networking.Sockets** 命名空間執行網路功能作業的詳細指導方針。

>**注意：**因為是[網路隔離](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)的一部分，所以系統禁止在同一部電腦上執行的兩個 UWP app 之間建立通訊端連線 (Sockets 或 WinSock)，不論是透過本機迴路位址 (127.0.0.0) 或明確指定本機 IP 位址。 這表示您無法使用通訊來在 UWP app 之間通訊。 UWP 提供其他 app 間的通訊機制。 如需詳細資訊，請參閱 [App 間通訊](https://msdn.microsoft.com/windows/uwp/app-to-app/index)。

## <a name="basic-tcp-socket-operations"></a>基本 TCP 通訊端作業

TCP 通訊端為長期連線提供雙向的低階網路資料傳輸。 TCP 通訊端是網際網路上大多數網路通訊協定所使用的基本功能。 本節說明如何透過使用 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 和 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 類別當作 [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間的一部分，讓 UWP app 以 TCP 資料流通訊端來傳送和接收資料。 在本節中，我們會建立一個非常簡單的 app 當作回應伺服器和用戶端，以示範基本 TCP 操作。

**建立 TCP echo server**

以下程式碼範例示範如何建立 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 物件並啟動接聽 TCP 連入連線。

```csharp
try
{
    //Create a StreamSocketListener to start listening for TCP connections.
    Windows.Networking.Sockets.StreamSocketListener socketListener = new Windows.Networking.Sockets.StreamSocketListener();

    //Hook up an event handler to call when connections are received.
    socketListener.ConnectionReceived += SocketListener_ConnectionReceived;

    //Start listening for incoming TCP connections on the specified port. You can specify any port that' s not currently in use.
    await socketListener.BindServiceNameAsync("1337");
}
catch (Exception e)
{
    //Handle exception.
}
```

以下程式碼範例實作上述範例中附加至 [**StreamSocketListener.ConnectionReceived**](https://msdn.microsoft.com/library/windows/apps/hh701494) 事件的 SocketListener\_ConnectionReceived 事件處理常式。 每當遠端用戶端與回應伺服器建立連線時，[**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 類別就會呼叫此事件處理常式。

```csharp
private async void SocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, 
    Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    //Read line from the remote client.
    Stream inStream = args.Socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(inStream);
    string request = await reader.ReadLineAsync();
    
    //Send the line back to the remote client.
    Stream outStream = args.Socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(outStream);
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();
}
```

**建立 TCP echo 用戶端**

以下程式碼範例示範如何建立 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 物件、建立連線至遠端伺服器、傳送要求及接收回應。

```csharp
try
{
    //Create the StreamSocket and establish a connection to the echo server.
    Windows.Networking.Sockets.StreamSocket socket = new Windows.Networking.Sockets.StreamSocket();
    
    //The server hostname that we will be establishing a connection to. We will be running the server and client locally,
    //so we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Every protocol typically has a standard port number. For example HTTP is typically 80, FTP is 20 and 21, etc.
    //For the echo server/client application we will use a random port 1337.
    string serverPort = "1337";
    await socket.ConnectAsync(serverHost, serverPort);

    //Write data to the echo server.
    Stream streamOut = socket.OutputStream.AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string request = "test";
    await writer.WriteLineAsync(request);
    await writer.FlushAsync();

    //Read data from the echo server.
    Stream streamIn = socket.InputStream.AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string response = await reader.ReadLineAsync();
}
catch (Exception e)
{
    //Handle exception here.            
}
```

## <a name="basic-udp-socket-operations"></a>基本 UDP 通訊端作業

UDP 提供雙向低階網路資料傳輸，可用於不需要建立連線的網路通訊。 因為 UDP 通訊端不會維持兩端點的連線，它們可提供遠端電腦之間網路功能快速又簡單的解決方案。 不過，UDP 通訊端不會確保網路封包的完整性，或確認封包是否到達遠端目的地。 使用 UDP 通訊端的 app 包括區域網路探索和區域聊天用戶端。 本節透過建立簡單的回應伺服器和用戶端，來示範使用 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 類別傳送和接收 UDP 訊息。

**建立 UDP echo server**

以下程式碼範例示範如何建立 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 物件並將它繫結至特定連接埠，以接聽連入的 UDP 訊息。

```csharp
Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();

socket.MessageReceived += Socket_MessageReceived;

//You can use any port that is not currently in use already on the machine.
string serverPort = "1337";

//Bind the socket to the serverPort so that we can start listening for UDP messages from the UDP echo client.
await socket.BindServiceNameAsync(serverPort);
```

以下程式碼範例實作 **Socket\_MessageReceived** 事件處理常式，以讀取從用戶端接收的訊息並將同樣的訊息傳回。

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo client.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();

    //Create a new socket to send the same message back to the UDP echo client.
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    //Use a separate port number for the UDP echo client because both will be unning on the same machine.
    string clientPort = "1338"
    Stream streamOut = (await socket.GetOutputStreamAsync(args.RemoteAddress, clientPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

**建立 UDP echo 用戶端**

以下程式碼範例示範如何建立 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 物件並將它繫結至特定連接埠，以接聽連入的 UDP 訊息並傳送 UDP 訊息至 UDP 回應伺服器。

```csharp
private async void testUdpSocketServer()
{
    Windows.Networking.Sockets.DatagramSocket socket = new Windows.Networking.Sockets.DatagramSocket();
    
    socket.MessageReceived += Socket_MessageReceived;
    
    //You can use any port that is not currently in use already on the machine. We will be using two separate and random 
    //ports for the client and server because both the will be running on the same machine.
    string serverPort = "1337";
    string clientPort = "1338";
    
    //Because we will be running the client and server on the same machine, we will use localhost as the hostname.
    Windows.Networking.HostName serverHost = new Windows.Networking.HostName("localhost");
    
    //Bind the socket to the clientPort so that we can start listening for UDP messages from the UDP echo server.
    await socket.BindServiceNameAsync(clientPort);
                
    //Write a message to the UDP echo server.
    Stream streamOut = (await socket.GetOutputStreamAsync(serverHost, serverPort)).AsStreamForWrite();
    StreamWriter writer = new StreamWriter(streamOut);
    string message = "Hello, world!";
    await writer.WriteLineAsync(message);
    await writer.FlushAsync();
}
```

以下程式碼範例實作 **Socket\_MessageReceived** 事件處理常式，以讀取從 UDP 回應伺服器接收的訊息。

```csharp
private async void Socket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, 
    Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    //Read the message that was received from the UDP echo server.
    Stream streamIn = args.GetDataStream().AsStreamForRead();
    StreamReader reader = new StreamReader(streamIn);
    string message = await reader.ReadLineAsync();
}
```

## <a name="background-operations-and-the-socket-broker"></a>背景作業和通訊端代理程式

如果您的 app 會在通訊端上接收連線或資料，則您必須做好準備，以便在您的 app 不在前景時適當執行這些作業。 若要這樣做，您可以使用通訊端代理程式。 如需如何使用通訊端代理程式的詳細資訊，請參閱[背景網路通訊](network-communications-in-the-background.md)。

## <a name="batched-sends"></a>批次傳送

從 Windows 10 開始，Windows.Networking.Sockets 可支援批次傳送，此方式可讓您同時傳送多個資料緩衝區，且內容切換的額外負荷會比個別傳送每個緩衝區時低得多。 如果您的 app 正在進行 VoIP、VPN 或其他涉及盡可能有效移動大量資料的工作，這會特別有用。

每次在通訊端上呼叫 WriteAsync 時，都會觸發核心轉換以聯繫網路堆疊。 當 app 同時寫入許多緩衝區時，每次寫入將會引發個別的核心轉換，因而產生許多額外負荷。 新的批次傳送模式可最佳化核心轉換的頻率。 這項功能目前限用於 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 與連線的 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 執行個體。

以下是 app 在非最佳的方式中如何傳送大量緩衝區的範例。

```csharp
// Send a set of packets inefficiently, one packet at a time.
// This is not recommended if you have many packets to send
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

foreach (IBuffer packet in packetsToSend)
{
    // Incurs kernel transition overhead for each packet
    await outputStream.WriteAsync(packet);
}
```

此範例說明如何以更有效的方式傳送大量緩衝區。 因為這項技術使用 C# 語言專用的功能，因此只適用於 C# 程式設計師。 藉由同時間傳送多個封包，此範例讓系統可執行批次傳送，進而將核心轉換最佳化以改善效能。

```csharp
// More efficient way to send packets.
// This way enables the system to do batched sends
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = stream.OutputStream;

int i = 0;
Task[] pendingTasks = new Tast[packetsToSend.Count];
foreach (IBuffer packet in packetsToSend)
{
    // track all pending writes as tasks, but do not wait on one before peforming the next
    pendingTasks[i++] = outputStream.WriteAsync(packet).AsTask();
    // Do not modify any buffer' s contents until the pending writes are complete.
}
// Now, wait for all of the pending writes to complete
await Task.WaitAll(pendingTasks);
```

此範例說明如何以另一種與批次傳送相容的方式，傳送大量的緩衝區。 此方式不會使用任何 C# 的特定功能，因此適用於所有語言 (在此以 C# 示範)。 在 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 和 [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 類別的 **OutputStream** 成員中，它會使用 Windows 10 中新增的已變更行為

```csharp
// More efficient way to send packets in Windows 10, using the new behavior of OutputStream.FlushAsync().
int i = 0;
IList<IBuffer> packetsToSend = PreparePackets();
var outputStream = socket.OutputStream;

var pendingWrites = new IAsyncOperationWithProgress<uint,uint> [packetsToSend.Count];

foreach (IBuffer packet in packetsToSend)
{
    pendingWrites[i++] = outputStream.WriteAsync(packet);
    // Do not modify any buffer' s contents until the pending writes are complete.
}

// Wait for all pending writes to complete. This step enables batched sends on the output stream.
await outputStream.FlushAsync();
```

在舊版 Windows 中，**FlushAsync** 會立即傳回，且不保證資料流的所有作業都已完成。 在 Windows 10 中，此行為已變更。 現在，**FlushAsync** 一定會在輸出資料流的所有作業皆完成後才傳回。

在您的程式碼中使用批次寫入時有一些重要的限制。

-   在同步寫入完成之前，您無法對正在寫入的 **IBuffer** 執行個體修改內容。
-   **FlushAsync** 模式只適用於 **StreamSocket.OutputStream** 和 **DatagramSocket.OutputStream**。
-   **FlushAsync** 模式只適用於 Windows 10 和後續版本。
-   在其他情況下，請改用 **Task.WaitAll**，而不要使用 **FlushAsync** 模式。

## <a name="port-sharing-for-datagramsocket"></a>DatagramSocket 的連接埠共用

Windows 10 導入了新的 [**DatagramSocketControl**](https://msdn.microsoft.com/library/windows/apps/hh701190) 屬性 ([**MulticastOnly**](https://msdn.microsoft.com/library/windows/apps/dn895368))，可讓您指定相關的 **DatagramSocket** 能夠與其他繫結至相同地址/連接埠的 Win32 或 WinRT 多點傳送通訊端並存。

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>提供具有 StreamSocket 類別的用戶端憑證

[**Windows.Networking.StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 類別支援使用 SSL/TLS 來驗證與 app 交談的伺服器。 在某些情況下，app 也必須使用 TLS 的用戶端憑證向伺服器驗證本身。 在 Windows 10 中，您可以在 [**StreamSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226893) 物件上提供用戶端憑證 (這必須在 TLS 交握啟動之前設定)。 如果伺服器要求用戶端憑證，Windows 會使用提供的憑證來回應。

以下程式碼片段說明其實作方式：

```csharp
var socket = new StreamSocket();
Windows.Security.Cryptography.Certificates.Certificate certificate = await GetClientCert();
socket.Control.ClientCertificate = certificate;
await socket.ConnectAsync(destination, SocketProtectionLevel.Tls12);
```

## <a name="exceptions-in-windowsnetworkingsockets"></a>Windows.Networking.Sockets 中的例外狀況

如果傳送的字串不是有效的主機名稱 (包含不允許在主機名稱中使用的字元)，與通訊端一起使用的 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 類別的建構函式會發生例外狀況。 如果 app 取得使用者為 **HostName** 輸入的值，則建構函式應在 try/catch 區塊中。 如果發生例外狀況，app 可通知使用者並要求新的主機名稱。

[**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間有便利的協助程式方法及列舉，在使用通訊端和 WebSocket 時用來處理錯誤。 這對於在您的應用程式中以不同的方式處理特定網路例外狀況時很有用。

[**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)、[**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 或 [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) 作業上發生的錯誤會傳回為 **HRESULT** 值。 使用 [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) 方法，將通訊端作業的網路錯誤轉換為 [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457) 列舉值。 大多數 **SocketErrorStatus** 列舉值對應原始 Windows 通訊端作業傳回的錯誤。 app 可以篩選特定 **SocketErrorStatus** 列舉值，依據例外狀況的發生原因來修改 app 行為。

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 或 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 作業上發生的錯誤會傳回為 **HRESULT** 值。 使用 [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) 方法，將 WebSocket 作業的網路錯誤轉換為 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 列舉值。 大多數 **WebErrorStatus** 列舉值對應原始 HTTP 用戶端作業傳回的錯誤。 app 可以篩選特定 **WebErrorStatus** 列舉值，依據例外狀況的發生原因來修改 app 行為。

針對參數驗證錯誤，app 也可以使用來自例外狀況的 **HRESULT**，深入了解更多關於導致例外狀況的錯誤詳細資訊。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E\_INVALIDARG**。

## <a name="the-winsock-api"></a>Winsock API

您也可以在您的 UWP app 中使用 [Winsock](https://msdn.microsoft.com/library/windows/desktop/ms740673)。 支援的 Winsock API 以 Windows Phone 8.1Microsoft Silverlight 的 API 為基礎，且會繼續支援大部分的類型、屬性和方法 (已移除一些被視為過時的 API)。 您可以在[這裡](https://msdn.microsoft.com/library/windows/desktop/ms740673)找到更多關於 Winsock 程式設計的資訊。



