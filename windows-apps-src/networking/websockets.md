---
author: DelfCo
description: "WebSocket 提供了一項機制，可讓用戶端與伺服器之間透過使用 HTTP(S) 的 Web 快速且安全地進行雙向通訊。"
title: WebSocket
ms.assetid: EAA9CB3E-6A3A-4C13-9636-CCD3DE46E7E2
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ff2429e1e9ea56c414978c126497551b1e1864b8

---

# WebSocket

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)
-   [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923)

WebSocket 提供了一項機制，可讓用戶端與伺服器之間透過使用 HTTP(S) 的 Web 快速且安全地進行雙向通訊。

在 [WebSocket 通訊協定](http://tools.ietf.org/html/rfc6455)下，資料會立即透過全雙工單一通訊端連線來傳輸，因此能即時從這兩個端點傳送與接收訊息。 WebSockets 十分適用於立即社交網路通知和最新資訊顯示 (遊戲統計資料) 需具備安全性，且需要快速資料傳輸的即時遊戲。 通用 Windows 平台 (UWP) 開發人員可以使用 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 和 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 類別與支援 Websocket 通訊協定的伺服器連接。

| MessageWebSocket                                                         | StreamWebSocket                                                                               |
|--------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------|
| 適用於訊息不是非常大的一般案例。   | 適用於傳輸大型檔案 (例如相片或視訊) 的案例。 |
| 啟用已經收到整個 WebSocket 訊息的通知。 | 允許使用每個讀取操作來讀取訊息區段。                             |
| 支援 UTF-8 和二進位兩種訊息。                                 | 僅支援二進位訊息。                                                                |
| 類似於 UDP 或資料包通訊端。                                     | 類似於 TCP 或資料流通訊端。                                                            |

在大部分情況下，您會想使用安全的 WebSocket 連線為傳送和接收的資料加密。 這也將增加連線成功的機率，因為許多 Proxy 都會拒絕未加密的 WebSocket 連線。 WebSocket 通訊協定定義下列兩種 URI 配置。

-   ws: - 用於未加密的連線。
-   wss - 用於應加密的安全連線。

若要為 WebSocket 連線加密，請使用 wss: URI 配置，例如，`wss://www.contoso.com/mywebservice`。

## 使用 MessageWebSocket

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 允許使用每個讀取作業來讀取訊息區段。 **MessageWebSocket** 通常用於訊息不是非常大的案例中。 同時支援 UTF-8 與二進位檔案。

本節中的程式碼會建立新的 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842)、連線到 WebSocket 伺服器並傳送資料到伺服器。 建立成功的連線後，應用程式會等候觸發 [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) 事件，指出已收到資料。

此範例使用 WebSocket.org 回應伺服器，此服務會直接將傳送給它的任何字串回傳給傳送者。 使範例使用 "wss:" 通訊協定規範，透過安全連線來傳送和接收訊息。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> void Game::InitWebSockets()
> {
>     // Create a new web socket
>     m_messageWebSocket = ref new MessageWebSocket();
> 
>     // Set the message type to UTF-8
>     m_messageWebSocket->Control->MessageType = Windows::Networking::Sockets::SocketMessageType::Utf8;
> 
>     // Register callbacks for notifications of interest
>     m_messageWebSocket->MessageReceived += 
>        ref new TypedEventHandler<MessageWebSocket^, MessageWebSocketMessageReceivedEventArgs^>(this, &Game::WebSocketMessageReceived);
>     m_messageWebSocket->Closed += ref new TypedEventHandler<IWebSocket^, WebSocketClosedEventArgs^>(this, &Game::WebSocketClosed);
> 
>     // This test code uses the websocket.org echo service to illustrate sending a string and receiving the echoed string back
>     // Note that wss: makes this an encrypted connection.
>     m_serverUri = ref new Uri("wss://echo.websocket.org");
> 
>     // Establish the connection, and set m_socketConnected on success
>     create_task(m_messageWebSocket->ConnectAsync(m_serverUri)).then([this] (task<void> previousTask)
>     {
>         try
>         {
>             // Try getting all exceptions from the continuation chain above this point.
>             previousTask.get();
> 
>             // websocket connected. update state variable
>             m_socketConnected = true;
>             OutputDebugString(L"Successfully initialized websockets\n");
>         }
>         catch (Platform::COMException^ exception)
>         {
>             // Add code here to handle any exceptions
>             // HandleException(exception);
> 
>         }
>     });
> }
> ```
> ```cs
> MessageWebSocket webSock = new MessageWebSocket();
> 
> //In this case we will be sending/receiving a string so we need to set the MessageType to Utf8.
> webSock.Control.MessageType = SocketMessageType.Utf8;
> 
> //Add the MessageReceived event handler.
> webSock.MessageReceived += WebSock_MessageReceived;
> 
> //Add the Closed event handler.
> webSock.Closed += WebSock_Closed;
> 
> Uri serverUri = new Uri("wss://echo.websocket.org");
> 
> try
> {
>     //Connect to the server.
>     await webSock.ConnectAsync(serverUri);
> 
>     //Send a message to the server.
>     await WebSock_SendMessage(webSock, "Hello, world!");
> }
> catch (Exception ex)
> {
>     //Add code here to handle any exceptions
> }
> ```

初始化 WebSocket 連線之後，您的程式碼必須執行下列活動，才能正常傳送和接收資料。

### 實作 MessageWebSocket.MessageReceived 事件的回呼

在使用 WebSocket 建立連線和傳送資料之前，您的應用程式必須登錄事件回呼，以在接收資料時接收通知。 發生 [**MessageWebSocket.MessageReceived**](https://msdn.microsoft.com/library/windows/apps/br241358) 事件時，會呼叫登錄回呼並從 [**MessageWebSocketMessageReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br226852) 接收資料。 此範例的撰寫假設是，訊息是以 UTF-8 格式傳送的。

下列範例函式會從連線的 WebSocket 伺服器接收字串，並將字串列印至偵錯工具的輸出視窗。

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketMessageReceived(MessageWebSocket^ sender, MessageWebSocketMessageReceivedEventArgs^ args)
>{
>    DataReader^ messageReader = args->GetDataReader();
>    messageReader->UnicodeEncoding = Windows::Storage::Streams::UnicodeEncoding::Utf8;
>
>    String^ readString = messageReader->ReadString(messageReader->UnconsumedBufferLength);
>    // Data has been read and is now available from the readString variable.
>    swprintf(m_debugBuffer, 511, L"WebSocket Message received: %s\n", readString->Data());
>    OutputDebugString(m_debugBuffer);
>}
>```
>```csharp
>//The MessageReceived event handler.
>private void WebSock_MessageReceived(MessageWebSocket sender, MessageWebSocketMessageReceivedEventArgs args)
>{
>    DataReader messageReader = args.GetDataReader();
>    messageReader.UnicodeEncoding = UnicodeEncoding.Utf8;
>    string messageString = messageReader.ReadString(messageReader.UnconsumedBufferLength);
>
>    //Add code here to do something with the string that is received.
>}
>```

###  實作 MessageWebSocket.Closed 事件的回呼

在使用 WebSocket 建立連線和傳送資料之前，您的應用程式必須登錄事件回呼，以在 WebSocket 伺服器關閉 WebSocket 時接收通知。 [**MessageWebSocket.Closed**](https://msdn.microsoft.com/library/windows/apps/hh701364) 事件發生時會呼叫已登錄的回呼，以指出 WebSocket 伺服器已關閉連線。

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::WebSocketClosed(IWebSocket^ sender, WebSocketClosedEventArgs^ args)
>{
>    // The method may be triggered remotely by the server sending unsolicited close frame or locally by Close()/delete operator.
>    // This method assumes we saved the connected WebSocket to a variable called m_messageWebSocket
>    if (m_messageWebSocket != nullptr)
>    {
>        delete m_messageWebSocket;
>        m_messageWebSocket = nullptr;
>        OutputDebugString(L"Socket was closed\n");
>    }
>    m_socketConnected = false;
> }
>```
>```csharp
>//The Closed event handler
>private void WebSock_Closed(IWebSocket sender, WebSocketClosedEventArgs args)
>{
>    //Add code here to do something when the connection is closed locally or by the server
>}
>```

###  在 WebSocket 上傳送訊息

建立連線後，WebSocket 用戶端即可將資料傳送至伺服器。 [**DataWriter.StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) 方法會傳回對應到不帶正負號之整數的參數。 與建立連線的工作相較，這改變了我們定義傳送訊息之工作的方式。

**注意** 當您使用 MessageWebSocket 的 OutputStream 建立新的 DataWriter 物件時，DataWriter 會取得 OutputStream 的擁有權，且會在 DataWriter 超出範圍時取消 Outputstream 的配置。 這會導致任何使用 OutputStream 的後續嘗試皆失敗，並出現 HRESULT 值 0x80000013。 為避免取消配置 OutputStream，這段程式碼會呼叫 DataWriter 的 DetachStream 方法，此方法會將資料流的擁有權傳回給 WebSocket 物件。

下列函式會將指定的字串傳送到連線的 WebSocket，並在偵錯工具的輸出視窗中列印驗證訊息。

> [!div class="tabbedCodeSnippets"]
>```cpp
>void Game::SendWebSocketMessage(Windows::Networking::Sockets::MessageWebSocket^ sendingSocket, Platform::String^ message)
>{
>    if (m_socketConnected)
>    {
>        // WebSocket is connected, so send a message
>        m_messageWriter = ref new DataWriter(sendingSocket->OutputStream);
>
>        m_messageWriter->WriteString(message);
>
>        // Send the data as one complete message
>        create_task(m_messageWriter->StoreAsync()).then([this] (unsigned int)
>        {
>            // Send Completed
>            m_messageWriter->DetachStream();    // give the stream back to m_messageWebSocket
>            OutputDebugString(L"Sent websocket message\n");
>        })
>            .then([this] (task<void>> previousTask)
>        {
>            try
>            {
>                // Try getting all exceptions from the continuation chain above this point.
>                previousTask.get();
>            }
>            catch (Platform::COMException ^ex)
>            {
>                // Add code to handle the exception
>                // HandleException(exception);
>            }
>        });
>    }
>}
>```
>```csharp
>//Send a message to the server.
>private async Task WebSock_SendMessage(MessageWebSocket webSock, string message)
>{
>    DataWriter messageWriter = new DataWriter(webSock.OutputStream);
>    messageWriter.WriteString(message);
>    await messageWriter.StoreAsync();
>}
>```

## 對 Websocket 使用進階控制項

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 和 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 都遵循使用進階控制項的相同模型。 對應到上述每一個主要類別的是可以存取進階控制項的相關類別。

[**MessageWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226843) 提供 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 物件的通訊端控制項資料。
[**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 提供 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 物件的通訊端控制項資料。
兩種類型的 WebSocket 使用的進階控制項基本模型都是相同的。 以下討論使用 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 做為範例，但是相同程序也可與 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 搭配使用。

1.  建立 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 物件。
2.  使用 [**StreamWebSocket.Control**](https://msdn.microsoft.com/library/windows/apps/br226934) 屬性來擷取與此 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 物件關聯的 [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 執行個體。
3.  取得或設定 [**StreamWebSocketControl**](https://msdn.microsoft.com/library/windows/apps/br226924) 執行個體的屬性，以取得或設定特定的進階控制項。

[**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 和 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 對於何時可設定進階控制項都有其限制。

-   對於 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 的所有進階控制項，應用程式都必須一律先設定屬性，再發出連線作業。 由於有此需求，因此最好在建立 **StreamWebSocket** 物件之後立即設定所有控制項屬性。 請不要嘗試在呼叫 [**StreamWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226933) 方法之後設定控制項屬性。
-   對於訊息類型以外的所有 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 進階控制項，您必須一律先設定屬性，再發出連線作業。 最好在建立 **MessageWebSocket** 物件之後立即設定所有控制項屬性。 除了訊息類型以外，請不要嘗試在呼叫 [**MessageWebSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/br226859) 之後變更控制項屬性。

## WebSocket 資訊類別

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 和 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 分別有一個對應的類別，可提供關於 WebSocket 執行個體的其他資訊。

[**MessageWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226849) 會提供 [**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 的相關資訊，您可以使用 [**MessageWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226861) 屬性擷取資訊類別的執行個體。

[**StreamWebSocketInformation**](https://msdn.microsoft.com/library/windows/apps/br226929) 會提供 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 的相關資訊，您可以使用 [**StreamWebSocket.Information**](https://msdn.microsoft.com/library/windows/apps/br226935) 屬性擷取資訊類別的執行個體。

請注意，這兩個資訊類別的所有屬性都是唯讀的，在 Web 通訊端物件的存留期間，您可以隨時擷取目前資訊。

## 處理網路例外狀況

[**MessageWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226842) 或 [**StreamWebSocket**](https://msdn.microsoft.com/library/windows/apps/br226923) 作業上發生的錯誤會傳回為 **HRESULT** 值。 使用 [**WebSocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701529) 方法，將 WebSocket 作業的網路錯誤轉換為 [**WebErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh747818) 列舉值。 大多數 **WebErrorStatus** 列舉值對應原始 HTTP 用戶端作業傳回的錯誤。 您的應用程式可以篩選特定 **WebErrorStatus** 列舉值，依據例外狀況的發生原因來修改應用程式行為。

針對參數驗證錯誤，應用程式也可以使用來自例外狀況的 **HRESULT**，深入了解更多關於導致例外狀況的錯誤詳細資訊。 可能的 **HRESULT** 值列在 *Winerror.h* 標頭檔中。 針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E\_INVALIDARG**。

## 設定 WebSocket 作業的逾時

MessageWebSocket 和 StreamWebSocket 類別會使用內部系統服務，來傳送 WebSocket 用戶端要求和接收來自伺服器的回應。 用於 WebSocket 連線作業的預設逾時值為 60 秒。 如果支援 WebSocket 的 HTTP 伺服器因網路中斷而暫時關閉或遭到封鎖，且伺服器沒有或無法回應 WebSocket 連線要求，內部系統服務會先等候預設的 60 秒，再傳回導致對 WebSocket ConnectAsync 方法擲回例外狀況的錯誤。 如果對 URI 中的 HTTP 伺服器名稱進行的名稱查詢針對該名稱傳回多個 IP 位址，內部系統服務會先針對該網站嘗試最多 5 個 IP 位址 (每個都使用預設的 60 秒逾時)，然後才會失敗。 提出 WebSocket 連線要求的應用程式可先等候數分鐘程式連接到多個 IP 位址，然後才會傳回錯誤並擲回例外狀況。 當應用程式停止運作時，使用者即會看見此行為。 建立 WebSocket 連線之後，傳送和接收作業所使用的預設逾時為 30 秒。

若要讓應用程式更具回應性，並將這些問題減至最低，應用程式可設定較短的連線要求逾時，讓作業儘速在該逾時 (而非預設設定) 之後失敗。

您可以為 StreamWebSockets 和 MessageWebSockets 設定類似的逾時。 下列範例說明如何設定 StreamWebSocket 的逾時，但其程序與 MessageWebSocket 相類似。

1.  建立使用計時器在指定的延遲之後完成的工作。
2.  使用 cancellation\_token\_source 建立 WebSocket 作業的工作，以支援取消動作。
3.  如果具有指定逾時延遲的工作在 WebSocket 連線作業之前完成，請取消 WebSocket 作業的工作。

下列範例會建立一個在指定的延遲之後完成的工作，並建立另一個在指定的延遲之後取消的工作。 在嘗試建立連線時可以將這些類別與 StreamWebSocket 和 MessageWebSocket 搭配使用，以設定特定逾時。 範例用法會在具有 cancellation\_token\_source 的工作中呼叫 StreamWebSocket.ConnectAsync 方法，以支援取消。 如果先到達逾時，則會使用 cancellation\_token\_source 來取消 WebSocket 連線作業的工作。

```cpp
    #include <agents.h>
    #include <ppl.h>
    #include <ppltasks.h>

    using namespace concurrency;
    using namespace std;

    // Creates a task that completes after the specified delay.
    task<void> complete_after(unsigned int timeout)
    {
        // A task completion event that is set when a timer fires.
        task_completion_event<void> tce;

        // Create a non-repeating timer.
        shared_ptr<timer<int>> fire_once(new timer<int>(timeout, 0, nullptr, false));
        
        // Create a call object that sets the completion event after the timer fires.
        shared_ptr<call<int>> callback(new call<int>([tce](int)
        {
            tce.set();
        }));

        // Connect the timer to the callback and start the timer.
        fire_once->link_target(callback.get());
        fire_once->start();

        // Create a task that completes after the completion event is set.
        task<void> event_set(tce);

        // Create a continuation task that cleans up resources and
        // and return that continuation task.
        return event_set.then([callback, fire_once]()
        {
        });
    }

    // Cancels the provided task after the specifed delay, if the task
    // did not complete.
    template<typename T>
    task<T> cancel_after_timeout(task<T> t, cancellation_token_source cts, unsigned int timeout)
    {
        // Create a task that returns true after the specified task completes.
        task<bool> success_task = t.then([](T)
        {
            return true;
        });
        // Create a task that returns false after the specified timeout.
        task<bool> failure_task = complete_after(timeout).then([]
        {
            return false;
        });

        // Create a continuation task that cancels the overall task  
        // if the timeout task finishes first. 
        return (failure_task || success_task).then([t, cts](bool success)
        {
            if (!success)
            {
                // Set the cancellation token. The task that is passed as the 
                // t parameter should respond to the cancellation and stop 
                // as soon as it can.
                cts.cancel();
            }
 
            // Return the original task.
            return t;
        });
    }
```




<!--HONumber=Aug16_HO3-->


