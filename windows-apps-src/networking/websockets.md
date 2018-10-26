---
author: stevewhims
description: WebSocket 提供了一項機制，可讓用戶端與伺服器之間透過使用 HTTP(S) 的 Web 快速且安全地進行雙向通訊，並支援 UTF-8 和二進位訊息。
title: WebSocket
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
ms.author: stwhi
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, 網路功能, websocket, messagewebsocket, streamwebsocket
ms.localizationpriority: medium
ms.openlocfilehash: eaba8d7b87d9e2762d13bd86629ee4a996e2d5e8
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562137"
---
# <a name="websockets"></a>WebSocket
WebSocket 提供了一項機制，可讓用戶端與伺服器之間透過使用 HTTP(S) 的 Web 快速且安全地進行雙向通訊，並支援 UTF-8 和二進位訊息。

在 [WebSocket 通訊協定](http://tools.ietf.org/html/rfc6455)下，資料會立即透過全雙工單一通訊端連線來傳輸，因此能即時從這兩個端點傳送與接收訊息。 WebSocket 十分適用於多人遊戲 (即時和回合制)、立即社交網路通知、最新股價或天氣資訊顯示，以及需要安全和快速資料傳輸的其他應用程式。

要建立 WebSocket 連線，用戶端和伺服器之間會交換以 HTTP 為基礎的特定交握。 如果成功，應用程式層通訊協定會從 HTTP「升級」為 WebSockets，使用先前建立的 TCP 連線。 當發生這種情形時，HTTP 會完全退場；端點使用 WebSocket 通訊協定傳送或接收資料，直到 WebSocket 連線關閉為止。

**注意** 用戶端無法使用 WebSockets 傳輸資料，除非伺服器也使用 WebSocket 通訊協定。 如果伺服器不支援 WebSockets，您必須使用另一個資料傳輸方法。

通用 Windows 平台 (UWP) 支援用戶端和伺服器支援 WebSockets。 [**Windows.Networking.Sockets**](/uwp/api/windows.networking.sockets)命名空間定義兩個 WebSocket 類別供用戶端使用：&mdash;[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket)及[**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket)。 以下是這兩個 WebSocket 類別比較。

| [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket) | [StreamWebSocket](/uwp/api/windows.networking.sockets.streamwebsocket) |
| - | - |
| 整個 WebSocket 訊息在單一作業中讀取/寫入。 | 可使用每個讀取作業讀取訊息區段。 |
| 適合訊息不是非常大時。 | 適用於傳輸非常大的檔案 (例如相片或影片) 時。 |
| 支援 UTF-8 和二進位兩種訊息。 | 僅支援二進位訊息。 |
| 類似於[UDP 或資料包通訊端](sockets.md#build-a-basic-udp-socket-client-and-server)（在適用於常用的小訊息的意義上），但具有 TCP 的可靠性、封包順序保證和壅塞控制項。 | 類似於 [TCP 或資料流通訊端](sockets.md#build-a-basic-tcp-socket-client-and-server)。 |

## <a name="secure-your-connection-with-tlsssl"></a>使用 TLS/SSL 保護您的連線
在大部分情況下，您會想使用安全的 WebSocket 連線，讓傳送和接收的資料加密。 這也將增加連線成功的機率，因為許多中繼服務 (如防火牆和 Proxy) 都會拒絕未加密的 WebSocket 連線。 [WebSocket 通訊協定](https://tools.ietf.org/html/rfc6455#section-3)定義這兩種 URI 配置。

| URI 配置 | 用途 |
| - | - |
| wss： | 用於應加密的安全連線。 |
| ws： | 用於未加密的連線。 |

若要為 WebSocket 連線加密，請使用 `wss:` URI 配置。 範例如下。

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var webSocket = new Windows.Networking.Sockets.MessageWebSocket();
    await webSocket.ConnectAsync(new Uri("wss://www.contoso.com/mywebservice"));
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
{
    Windows::Networking::Sockets::MessageWebSocket webSocket;
    co_await webSocket.ConnectAsync(Uri{ L"wss://www.contoso.com/mywebservice" });
}
```

## <a name="use-messagewebsocket-to-connect"></a>使用 MessageWebSocket 以連線
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) 允許整個 WebSocket 訊息在單一作業中讀取/寫入。 因此，適合訊息不是非常大時。 此類別支援 UTF-8 和二進位兩種訊息。

以下的範例程式碼使用 WebSocket.org echo 伺服器，此服務會將傳送給它的任何訊息 echo 給傳送者。

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
    this.messageWebSocket.Control.MessageType = Windows.Networking.Sockets.SocketMessageType.Utf8;

    this.messageWebSocket.MessageReceived += WebSocket_MessageReceived;
    this.messageWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();
        connectTask.ContinueWith(_ => this.SendMessageUsingMessageWebSocketAsync("Hello, World!"));
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private async Task SendMessageUsingMessageWebSocketAsync(string message)
{
    using (var dataWriter = new DataWriter(this.messageWebSocket.OutputStream))
    {
        dataWriter.WriteString(message);
        await dataWriter.StoreAsync();
        dataWriter.DetachStream();
    }
    Debug.WriteLine("Sending message using MessageWebSocket: " + message);
}

private void WebSocket_MessageReceived(Windows.Networking.Sockets.MessageWebSocket sender, Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs args)
{
    try
    {
        using (DataReader dataReader = args.GetDataReader())
        {
            dataReader.UnicodeEncoding = Windows.Storage.Streams.UnicodeEncoding.Utf8;
            string message = dataReader.ReadString(dataReader.UnconsumedBufferLength);
            Debug.WriteLine("Message received from MessageWebSocket: " + message);
            this.messageWebSocket.Dispose();
        }
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;
    winrt::event_token m_messageReceivedEventToken;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        m_messageWebSocket.Control().MessageType(Windows::Networking::Sockets::SocketMessageType::Utf8);

        m_messageReceivedEventToken = m_messageWebSocket.MessageReceived({ this, &MessageWebSocketPage::OnWebSocketMessageReceived });
        m_closedEventToken = m_messageWebSocket.Closed({ this, &MessageWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            SendMessageUsingMessageWebSocketAsync(L"Hello, World!");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingMessageWebSocketAsync(std::wstring message)
    {
        DataWriter dataWriter{ m_messageWebSocket.OutputStream() };
        dataWriter.WriteString(message);

        co_await dataWriter.StoreAsync();
        dataWriter.DetachStream();
        std::wstringstream wstringstream;
        wstringstream << L"Sending message using MessageWebSocket: " << message.c_str() << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
    }

    void OnWebSocketMessageReceived(Windows::Networking::Sockets::MessageWebSocket const& /* sender */, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs const& args)
    {
        try
        {
            DataReader dataReader{ args.GetDataReader() };

            dataReader.UnicodeEncoding(Windows::Storage::Streams::UnicodeEncoding::Utf8);
            auto message = dataReader.ReadString(dataReader.UnconsumedBufferLength());
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message.c_str() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_messageWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const& /* sender */, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        // In this example, we send/receive a string, so we need to set the MessageType to Utf8.
        this->messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;

        this->messageWebSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::MessageWebSocket^, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^>(this, &MessageWebSocketPage::WebSocket_MessageReceived);
        this->messageWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &MessageWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
            connectTask.then([this] { this->SendMessageUsingMessageWebSocketAsync(L"Hello, World!"); });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingMessageWebSocketAsync(Platform::String^ message)
    {
        auto dataWriter = ref new DataWriter(this->messageWebSocket->OutputStream);
        dataWriter->WriteString(message);

        Concurrency::create_task(dataWriter->StoreAsync()).then(
            [=](unsigned int)
        {
            dataWriter->DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending message using MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        });
    }

    void WebSocket_MessageReceived(Windows::Networking::Sockets::MessageWebSocket^ sender, Windows::Networking::Sockets::MessageWebSocketMessageReceivedEventArgs^ args)
    {
        try
        {
            DataReader^ dataReader = args->GetDataReader();

            dataReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
            Platform::String^ message = dataReader->ReadString(dataReader->UnconsumedBufferLength);
            std::wstringstream wstringstream;
            wstringstream << L"Message received from MessageWebSocket: " << message->Data() << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            this->messageWebSocket->Close(1000, L"");
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-messagewebsocketmessagereceived-and-messagewebsocketclosed-events"></a>處理 MessageWebSocket.MessageReceived 和 MessageWebSocket.Closed 事件
如上述範例中所示，使用**MessageWebSocket**連線並傳送資料之前，您應該訂閱[**MessageWebSocket.MessageReceived**](/uwp/api/windows.networking.sockets.messagewebsocket.MessageReceived)和[**MessageWebSocket.Closed**](/uwp/api/windows.networking.sockets.messagewebsocket.Closed)事件。
 
收到資料時會引發**MessageReceived**。 可以透過[**MessageWebSocketMessageReceivedEventArgs**](/uwp/api/windows.networking.sockets.messagewebsocketmessagereceivedeventargs)存取資料。 關閉當用戶端或伺服器關閉通訊端時會引發 **Closed**。
 
### <a name="send-data-on-a-messagewebsocket"></a>在 MessageWebSocket 上傳送資料
建立連線後，您可將資料傳送至伺服器。 可以藉由使用[**MessageWebSocket.OutputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream)屬性執行此動作，並使用[**DataWriter**](/uwp/api/windows.storage.streams.datawriter)來將資料寫入。 

**注意** **DataWriter**擁有輸出資料流。 當**DataWriter**超出範圍時，如果它有附加的輸出資料流，**DataWriter**會解除配置輸出資料流。 之後，使用輸出資料流的後續嘗試皆失敗，並出現 HRESULT 值 0x80000013。 但您可以呼叫[**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream)中斷輸出資料流與**DataWriter**的連結，並將資料流擁有權傳回給**MessageWebSocket**。

## <a name="use-streamwebsocket-to-connect"></a>使用 StreamWebSocket 以連線
[**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) 允許使用每個讀取作業來讀取訊息區段。 因此，適用於傳輸非常大的檔案 (例如相片或影片) 時。 此類別僅支援二進位訊息。

以下的範例程式碼使用 WebSocket.org echo 伺服器，此服務會將傳送給它的任何訊息 echo 給傳送者。

```csharp
private Windows.Networking.Sockets.StreamWebSocket streamWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

    this.streamWebSocket.Closed += WebSocket_Closed;

    try
    {
        Task connectTask = this.streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask();

        connectTask.ContinueWith(_ =>
        {
            Task.Run(() => this.ReceiveMessageUsingStreamWebSocket());
            Task.Run(() => this.SendMessageUsingStreamWebSocket(new byte[] { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 }));
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void ReceiveMessageUsingStreamWebSocket()
{
    try
    {
        using (var dataReader = new DataReader(this.streamWebSocket.InputStream))
        {
            dataReader.InputStreamOptions = InputStreamOptions.Partial;
            await dataReader.LoadAsync(256);
            byte[] message = new byte[dataReader.UnconsumedBufferLength];
            dataReader.ReadBytes(message);
            Debug.WriteLine("Data received from StreamWebSocket: " + message.Length + " bytes");
        }
        this.streamWebSocket.Dispose();
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private async void SendMessageUsingStreamWebSocket(byte[] message)
{
    try
    {
        using (var dataWriter = new DataWriter(this.streamWebSocket.OutputStream))
        {
            dataWriter.WriteBytes(message);
            await dataWriter.StoreAsync();
            dataWriter.DetachStream();
        }
        Debug.WriteLine("Sending data using StreamWebSocket: " + message.Length.ToString() + " bytes");
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add code here to handle exceptions.
    }
}

private void WebSocket_Closed(Windows.Networking.Sockets.IWebSocket sender, Windows.Networking.Sockets.WebSocketClosedEventArgs args)
{
    Debug.WriteLine("WebSocket_Closed; Code: " + args.Code + ", Reason: \"" + args.Reason + "\"");
    // Add additional code here to handle the WebSocket being closed.
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket m_streamWebSocket;
    winrt::event_token m_closedEventToken;

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        m_closedEventToken = m_streamWebSocket.Closed({ this, &StreamWebSocketPage::OnWebSocketClosed });

        try
        {
            co_await m_streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
            ReceiveMessageUsingStreamWebSocket();
            SendMessageUsingStreamWebSocket({ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

private:
    IAsyncAction SendMessageUsingStreamWebSocket(std::vector< byte > message)
    {
        try
        {
            DataWriter dataWriter{ m_streamWebSocket.OutputStream() };
            dataWriter.WriteBytes(message);

            co_await dataWriter.StoreAsync();
            dataWriter.DetachStream();
            std::wstringstream wstringstream;
            wstringstream << L"Sending data using StreamWebSocket: " << message.size() << L" bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    IAsyncAction ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader dataReader{ m_streamWebSocket.InputStream() };
            dataReader.InputStreamOptions(InputStreamOptions::Partial);

            unsigned int bytesLoaded = co_await dataReader.LoadAsync(256);
            std::vector< byte > message(bytesLoaded);
            dataReader.ReadBytes(message);
            std::wstringstream wstringstream;
            wstringstream << L"Data received from StreamWebSocket: " << message.size() << " bytes" << std::endl;
            ::OutputDebugString(wstringstream.str().c_str());
            m_streamWebSocket.Close(1000, L"");
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }

    void OnWebSocketClosed(Windows::Networking::Sockets::IWebSocket const&, Windows::Networking::Sockets::WebSocketClosedEventArgs const& args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args.Code() << ", Reason: \"" << args.Reason().c_str() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

```cpp
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::StreamWebSocket^ streamWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

        this->streamWebSocket->Closed += ref new TypedEventHandler<Windows::Networking::Sockets::IWebSocket^, Windows::Networking::Sockets::WebSocketClosedEventArgs^>(this, &StreamWebSocketPage::WebSocket_Closed);

        try
        {
            auto connectTask = Concurrency::create_task(this->streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));

            connectTask.then(
                [=]
            {
                this->ReceiveMessageUsingStreamWebSocket();
                this->SendMessageUsingStreamWebSocket(ref new Platform::Array< byte >{ 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09 });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

private:
    void SendMessageUsingStreamWebSocket(const Platform::Array< byte >^ message)
    {
        try
        {
            auto dataWriter = ref new DataWriter(this->streamWebSocket->OutputStream);
            dataWriter->WriteBytes(message);

            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](Concurrency::task< unsigned int >) // task< unsigned int > instead of unsigned int in order to handle any exceptions thrown in StoreAsync().
            {
                dataWriter->DetachStream();
                std::wstringstream wstringstream;
                wstringstream << L"Sending data using StreamWebSocket: " << message->Length << L" bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void ReceiveMessageUsingStreamWebSocket()
    {
        try
        {
            DataReader^ dataReader = ref new DataReader(this->streamWebSocket->InputStream);
            dataReader->InputStreamOptions = InputStreamOptions::Partial;

            Concurrency::create_task(dataReader->LoadAsync(256)).then(
                [=](unsigned int bytesLoaded)
            {
                auto message = ref new Platform::Array< byte >(bytesLoaded);
                dataReader->ReadBytes(message);
                std::wstringstream wstringstream;
                wstringstream << L"Data received from StreamWebSocket: " << message->Length << " bytes" << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
                this->streamWebSocket->Close(1000, L"");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }

    void WebSocket_Closed(Windows::Networking::Sockets::IWebSocket^ sender, Windows::Networking::Sockets::WebSocketClosedEventArgs^ args)
    {
        std::wstringstream wstringstream;
        wstringstream << L"WebSocket_Closed; Code: " << args->Code << ", Reason: \"" << args->Reason->Data() << "\"" << std::endl;
        ::OutputDebugString(wstringstream.str().c_str());
        // Add additional code here to handle the WebSocket being closed.
    }
```

### <a name="handle-the-streamwebsocketclosed-event"></a>處理 StreamWebSocket.Closed 事件
使用**StreamWebSocket**連線並傳送資料之前，您應該訂閱[**StreamWebSocket.Closed**](/uwp/api/windows.networking.sockets.streamwebsocket.Closed)事件。 關閉當用戶端或伺服器關閉通訊端時會引發 **Closed**。
 
### <a name="send-data-on-a-streamwebsocket"></a>在 StreamWebSocket 上傳送資料
建立連線後，您可將資料傳送至伺服器。 可以藉由使用[**StreamWebSocket.OutputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream)屬性執行此動作，並使用[**DataWriter**](/uwp/api/windows.storage.streams.datawriter)來將資料寫入。

**注意**如果您想要在相同的通訊端上寫入更多資料，請務必在**DataWriter**超出範圍之前，呼叫[**DataWriter.DetachStream**](/uwp/api/windows.storage.streams.datawriter.DetachStream)中斷輸出資料流與**DataWriter**的連結。 這會將資料流的擁有權傳回給**MessageWebSocket**。

### <a name="receive-data-on-a-streamwebsocket"></a>在 StreamWebSocket 上接收資料
使用[**StreamWebSocket.InputStream**](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream)屬性，並使用[**DataReader**](/uwp/api/windows.storage.streams.datareader)來讀取資料。

## <a name="advanced-options-for-messagewebsocket-and-streamwebsocket"></a>MessageWebSocket 和 StreamWebSocket 的進階選項
連線之前，您可以在[**MessageWebSocketControl**](/uwp/api/windows.networking.sockets.messagewebsocketcontrol)或[**StreamWebSocketControl**](/uwp/api/windows.networking.sockets.streamwebsocketcontrol)上設定屬性，設定通訊端的進階選項。 從通訊端物件本身存取這些類別的執行個體 (視需要透過其[**MessageWebSocket.Control**](/uwp/api/windows.networking.sockets.messagewebsocket.control)屬性或[**StreamWebSocket.Control**](/uwp/api/windows.networking.sockets.streamwebsocket.control)屬性)。

這裡提供使用**StreamWebSocket**的範例。 相同模式適用於**MessageWebSocket**。

```csharp
var streamWebSocket = new Windows.Networking.Sockets.StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control.NoDelay = false;

await streamWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org"));
```

```cppwinrt
Windows::Networking::Sockets::StreamWebSocket streamWebSocket;

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket.Control().NoDelay(false);

auto connectAsyncAction = streamWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });
```

```cpp
auto streamWebSocket = ref new Windows::Networking::Sockets::StreamWebSocket();

// By default, the Nagle algorithm is not used. This overrides that, and causes it to be used.
streamWebSocket->Control->NoDelay = false;

auto connectTask = Concurrency::create_task(streamWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")));
```

**注意** 請不要嘗試在呼叫**ConnectAsync** *之後*更改控制項屬性。 該規則的唯一例外是[MessageWebSocketControl.MessageType](/uwp/api/windows.networking.sockets.messagewebsocketcontrol.MessageType)。

## <a name="websocket-information-classes"></a>WebSocket 資訊類別
[**MessageWebSocket**](/uwp/api/windows.networking.sockets.messagewebsocket) 和 [**StreamWebSocket**](/uwp/api/windows.networking.sockets.streamwebsocket) 分別有一個對應的類別，可提供關於物件的其他資訊。

[**MessageWebSocketInformation**](/uwp/api/windows.networking.sockets.messagewebsocketinformation) 會提供 **MessageWebSocket** 的相關資訊，您可以使用 [**MessageWebSocket.Information**](/uwp/api/windows.networking.sockets.messagewebsocket.Information) 屬性擷取其執行個體。

[**StreamWebSocketInformation**](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation) 會提供 **StreamWebSocket** 的相關資訊，您可以使用 [**StreamWebSocket.Information**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Information) 屬性擷取其執行個體。

請注意，這些資訊類別的所有屬性都是唯讀的，但是在 Web 通訊端物件的存留期間，您可以隨時擷取資訊。

## <a name="handling-exceptions"></a>處理例外狀況
[**MessageWebSocket**](/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 作業上發生的錯誤會傳回為 **HRESULT** 值。 您可以將該**HRESULT**值傳送至[**WebSocketError.GetStatus**](/uwp/api/windows.networking.sockets.websocketerror.getstatus)方法，以將它轉換為[**WebErrorStatus**](/uwp/api/Windows.Web.WebErrorStatus)列舉值。

大多數 **WebErrorStatus** 列舉值對應原始 HTTP 用戶端作業傳回的錯誤。 您的應用程式可以切換開啟 **WebErrorStatus** 列舉值，依據例外狀況的發生原因來修改應用程式行為。

針對參數驗證錯誤，您可以使用來自例外狀況的 **HRESULT**，深入了解更多關於錯誤的詳細資訊。 可能的**HRESULT**值列在`Winerror.h`中，可以在 SDK 安裝程式中找到 (例如，在資料夾`C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared`中)。 針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E_INVALIDARG**。

## <a name="setting-timeouts-on-websocket-operations"></a>設定 WebSocket 作業的逾時
**MessageWebSocket** 和 **StreamWebSocket** 會使用內部系統服務，來傳送 WebSocket 用戶端要求和接收來自伺服器的回應。 用於 WebSocket 連線作業的預設逾時值為 60 秒。 如果支援 WebSocket 的 HTTP 伺服器未回應或無法回應 WebSocket 連線要求 (暫時關閉，或因網路中斷而遭到封鎖)，內部系統服務會先等候預設的 60 秒，再傳回錯誤。 該錯誤導致對 WebSocket **ConnectAsync** 方法擲回例外狀況。 建立 WebSocket 連線之後，傳送和接收作業的預設逾時為 30 秒。

如果對 URI 中的 HTTP 伺服器名稱進行的名稱查詢針對該名稱傳回多個 IP 位址，內部系統服務會先針對該網站嘗試最多 5 個 IP 位址 (每個都使用預設的 60 秒逾時)，然後才會失敗。 因此，您的應用程式可能會先等候數分鐘，嘗試連線到多個 IP 位址，然後才會處理例外狀況。 此行為對使用者看起來就像是應用程式停止運作。 

若要讓應用程式更具回應性，並將這些問題減至最低，應用程式可設定較短的連線要求逾時。 您可以為 **MessageWebSocket** 和 **StreamWebSocket** 以類似的方式設定逾時。

```csharp
private Windows.Networking.Sockets.MessageWebSocket messageWebSocket;

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.messageWebSocket = new Windows.Networking.Sockets.MessageWebSocket();

    try
    {
        var cancellationTokenSource = new CancellationTokenSource();
        var connectTask = this.messageWebSocket.ConnectAsync(new Uri("wss://echo.websocket.org")).AsTask(cancellationTokenSource.Token);

        // Cancel connectTask after 5 seconds.
        cancellationTokenSource.CancelAfter(TimeSpan.FromMilliseconds(5000));

        connectTask.ContinueWith((antecedent) =>
        {
            if (antecedent.Status == TaskStatus.RanToCompletion)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask timed out, or faulted.
            }
        });
    }
    catch (Exception ex)
    {
        Windows.Web.WebErrorStatus webErrorStatus = Windows.Networking.Sockets.WebSocketError.GetStatus(ex.GetBaseException().HResult);
        // Add additional code here to handle exceptions.
    }
}
```

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.UI.Xaml.Navigation.h>
#include <sstream>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket m_messageWebSocket;

    IAsyncAction TimeoutAsync()
    {
        // Return control to the caller, and resume again to complete the async action after the timeout period.
        // 5 seconds, in this example.
        co_await(std::chrono::seconds{ 5 });
    }

public:
    IAsyncAction OnNavigatedTo(NavigationEventArgs /* e */)
    {
        try
        {
            // Return control to the caller, and then immediately resume on a thread pool thread.
            co_await winrt::resume_background();

            auto connectAsyncAction = m_messageWebSocket.ConnectAsync(Uri{ L"wss://echo.websocket.org" });

            TimeoutAsync().Completed([connectAsyncAction](IAsyncAction const& sender, AsyncStatus const)
            {
                // TimeoutAsync completes after the timeout period. After that period, it's safe
                // to cancel the ConnectAsync action even if it has already completed.
                connectAsyncAction.Cancel();
            });

            try
            {
                // Block until the ConnectAsync action completes or is canceled.
                connectAsyncAction.get();
            }
            catch (winrt::hresult_error const& ex)
            {
                std::wstringstream wstringstream;
                wstringstream << L"ConnectAsync threw an exception: " << ex.message().c_str() << std::endl;
                ::OutputDebugString(wstringstream.str().c_str());
            }

            if (connectAsyncAction.Status() == AsyncStatus::Completed)
            {
                // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                // Add additional code here to use the MessageWebSocket.
            }
            else
            {
                // connectTask did not run to completion.
            }
        }
        catch (winrt::hresult_error const& ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus{ Windows::Networking::Sockets::WebSocketError::GetStatus(ex.to_abi()) };
            // Add additional code here to handle exceptions.
        }
    }
```

```cpp
#include <agents.h>
#include <ppltasks.h>
#include <sstream>
...
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Xaml::Navigation;
...
private:
    Windows::Networking::Sockets::MessageWebSocket^ messageWebSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->messageWebSocket = ref new Windows::Networking::Sockets::MessageWebSocket();

        try
        {
            Concurrency::cancellation_token_source cancellationTokenSource;
            Concurrency::cancellation_token cancellationToken = cancellationTokenSource.get_token();

            auto connectTask = Concurrency::create_task(this->messageWebSocket->ConnectAsync(ref new Uri(L"wss://echo.websocket.org")), cancellationToken);

            // This continuation task returns true should connectTask run to completion.
            Concurrency::task< bool > taskRanToCompletion = connectTask.then([](void)
            {
                return true;
            });

            // This task returns false after the specified timeout. 5 seconds, in this example.
            Concurrency::task< bool > taskTimedout = Concurrency::create_task([]() -> bool
            {
                Concurrency::task_completion_event< void > taskCompletionEvent;

                // A call object that sets the task completion event.
                auto call = std::make_shared< Concurrency::call< int > >([taskCompletionEvent](int)
                {
                    taskCompletionEvent.set();
                });

                // A non-repeating timer that calls the call object when the timer fires.
                auto nonRepeatingTimer = std::make_shared< Concurrency::timer < int > >(5000, 0, call.get(), false);
                nonRepeatingTimer->start();

                // A task that completes after the completion event is set.
                Concurrency::task< void > taskWaitForCompletionEvent(taskCompletionEvent);

                return taskWaitForCompletionEvent.then([]() {return false; }).get();
            });

            (taskRanToCompletion || taskTimedout).then([this, cancellationTokenSource](bool connectTaskRanToCompletion)
            {
                if (connectTaskRanToCompletion)
                {
                    // connectTask ran to completion, so we know that the MessageWebSocket is connected.
                    // Add additional code here to use the MessageWebSocket.
                }
                else
                {
                    // taskTimedout ran to completion, so we should cancel connectTask via the cancellation_token_source.
                    cancellationTokenSource.cancel();
                }
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Web::WebErrorStatus webErrorStatus = Windows::Networking::Sockets::WebSocketError::GetStatus(ex->HResult);
            // Add additional code here to handle exceptions.
        }
    }
```

## <a name="important-apis"></a>重要 API
* [DataReader](/uwp/api/Windows.Storage.Streams.DataReader)
* [DataWriter](/uwp/api/Windows.Storage.Streams.DataWriter)
* [DataWriter.DetachStream](/uwp/api/windows.storage.streams.datawriter.DetachStream)
* [MessageWebSocket](/uwp/api/windows.networking.sockets.messagewebsocket)
* [MessageWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Closed)
* [MessageWebSocket.ConnectAsync](/uwp/api/windows.networking.sockets.messagewebsocket.connectasync)
* [MessageWebSocket.Control](/uwp/api/windows.networking.sockets.messagewebsocket.control)
* [MessageWebSocket.Information](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.Information)
* [MessageWebSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.MessageWebSocket.MessageReceived)
* [MessageWebSocket.OutputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.MessageWebSocket.OutputStream)
* [MessageWebSocketControl](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl)
* [MessageWebSocketControl.MessageType](/uwp/api/Windows.Networking.Sockets.MessageWebSocketControl.MessageType)
* [MessageWebSocketInformation](/uwp/api/Windows.Networking.Sockets.MessageWebSocketInformation)
* [MessageWebSocketMessageReceivedEventArgs](/uwp/api/Windows.Networking.Sockets.MessageWebSocketMessageReceivedEventArgs)
* [SocketMessageType](/uwp/api/windows.networking.sockets.socketmessagetype)
* [StreamWebSocket](/uwp/api/Windows.Networking.Sockets.StreamWebSocket)
* [StreamWebSocket.Closed](/uwp/api/Windows.Networking.Sockets.StreamWebSocket.Closed)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync)
* [StreamWebSocket.Control](/uwp/api/windows.networking.sockets.streamwebsocket.control)
* [StreamWebSocket.Information](/uwp/api/windows.networking.sockets.streamwebsocket.Information)
* [StreamWebSocket.InputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.InputStream)
* [StreamWebSocket.OutputStream](https://docs.microsoft.com/en-us/uwp/api/Windows.Networking.Sockets.StreamWebSocket.OutputStream)
* [StreamWebSocketControl](/uwp/api/Windows.Networking.Sockets.StreamWebSocketControl)
* [StreamWebSocketInformation](/uwp/api/Windows.Networking.Sockets.StreamWebSocketInformation)
* [WebErrorStatus](/uwp/api/Windows.Web.WebErrorStatus) 
* [WebSocketError.GetStatus](/uwp/api/windows.networking.sockets.websocketerror.getstatus)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets)

## <a name="related-topics"></a>相關主題
* [WebSocket 通訊協定](http://tools.ietf.org/html/rfc6455)
* [通訊端](sockets.md)

## <a name="samples"></a>範例
* [WebSocket 範例](http://go.microsoft.com/fwlink/p/?LinkId=620623)
