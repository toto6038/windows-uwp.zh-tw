---
description: 若要繼續網路通訊 (不是在背景中)，應用程式可以使用背景工作與通訊端代理程式或控制通道觸發程序。
title: 背景網路通訊
ms.assetid: 537F8E16-9972-435D-85A5-56D5764D3AC2
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4fcf2e1fa39ae1e40edc07c9ca3df81386c17823
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319927"
---
# <a name="network-communications-in-the-background"></a>背景網路通訊
若要繼續網路通訊 (雖然它不在前景)，您的應用程式可以使用背景工作及這兩個選項之一。
- 通訊端代理程式。 如果您的應用程式使用通訊端進行長期連線，當它離開前景時，即可將通訊端的擁有權委派給系統通訊端代理程式。 當流量抵達通訊端時，代理程式就會啟動應用程式，將擁有權移轉回應用程式，接著應用程式會處理抵達的流量。
- 控制通道觸發程序。 

## <a name="performing-network-operations-in-background-tasks"></a>在背景工作中執行網路作業
- 收到封包且需要執行短期工作時，可使用 [SocketActivityTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.socketactivitytrigger) 來啟動背景工作。 執行完工作後，背景工作應該會終止以節省電源。
- 收到封包且需要執行長期工作時，可使用 [ControlChannelTrigger](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 來啟動背景工作。

**網路相關條件和旗標**

- 新增 **InternetAvailable** 條件至您的背景工作 [BackgroundTaskBuilder.AddCondition](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 可延遲觸發背景工作，直到網路堆疊執行。 這個條件可以節省電源，因為直到網路連線前，背景工作都不會執行。 這個條件不提供即時啟動。

無論使用哪種觸發程序，在背景工作上設定 [IsNetworkRequested](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder) 可確保在背景工作執行時保持網路連線。 這會告訴背景工作基礎結構在工作執行時隨時保持網路連線，即使裝置已進入 [連線待命] 模式。 如果您的背景工作不使用 **IsNetworkRequested**，則處於 [連線待命] 模式時 (例如，當手機螢幕關閉時) 背景工作將無法存取網路。

## <a name="socket-broker-and-the-socketactivitytrigger"></a>通訊端代理程式和 SocketActivityTrigger
如果 app 使用 [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)、[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 或 [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 連線，則應使用 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) 和通訊端代理程式，這樣 app 的流量於 app 不在前景時抵達就會收到通知。

為了讓 App 在非使用狀態時可以接收並處理通訊端上接收的資料，App 必須在啟動時執行一些一次性設定，然後在其轉為非使用狀態時將通訊端擁有權移轉給通訊端代理程式。

一次設定步驟是建立觸發程序、登錄觸發程序的背景工作，並啟用通訊端代理程式的通訊端：
  - 建立 **SocketActivityTrigger** 並登錄觸發程序的背景工作，並在您的程式碼中設定 TaskEntryPoint 參數以處理收到的封包。
```csharp
            var socketTaskBuilder = new BackgroundTaskBuilder();
            socketTaskBuilder.Name = _backgroundTaskName;
            socketTaskBuilder.TaskEntryPoint = _backgroundTaskEntryPoint;
            var trigger = new SocketActivityTrigger();
            socketTaskBuilder.SetTrigger(trigger);
            _task = socketTaskBuilder.Register();
```
  - 繫結通訊端之前，先在通訊端上呼叫 **EnableTransferOwnership**。
```csharp
           _tcpListener = new StreamSocketListener();

           // Note that EnableTransferOwnership() should be called before bind,
           // so that tcpip keeps required state for the socket to enable connected
           // standby action. Background task Id is taken as a parameter to tie wake pattern
           // to a specific background task.  
           _tcpListener. EnableTransferOwnership(_task.TaskId,SocketActivityConnectedStandbyAction.Wake);
           _tcpListener.ConnectionReceived += OnConnectionReceived;
           await _tcpListener.BindServiceNameAsync("my-service-name");
```

正確設定您的通訊端之後，當您的 App 即將暫停時，就會在通訊端上呼叫 **TransferOwnership** 來將 App 傳輸到通訊端代理程式。 代理程式會監視通訊端，並在收到資料時啟動背景工作。 以下範例包含執行 **StreamSocketListener** 通訊端移轉的公用程式 **TransferOwnership** 函式。 (請注意，不同類型的通訊端有各自的 **TransferOwnership** 方法，所以您必須針對要移轉擁有權的通訊端呼叫適當的方法。 程式碼可能會包含一個多載的 **TransferOwnership** 協助程式，每個所使用之通訊端類型會有一個實作，這樣 **OnSuspending** 程式碼會更易於閱讀。)

App 會將通訊端的擁有權移轉給通訊端代理程式，並使用下列其中一個適當的方法傳遞背景作業的識別碼。
-   [  **DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket) 上的其中一個 [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.datagramsocket.transferownership) 方法。
-   [  **StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 上的其中一個 [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocket.transferownership) 方法。
-   [  **StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 上的其中一個 [**TransferOwnership**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamsocketlistener.transferownership) 方法。

```csharp

// declare int _transferOwnershipCount as a field.

private void TransferOwnership(StreamSocketListener tcpListener)
{
    await tcpListener.CancelIOAsync();

    var dataWriter = new DataWriter();
    ++_transferOwnershipCount;
    dataWriter.WriteInt32(transferOwnershipCount);
    var context = new SocketActivityContext(dataWriter.DetachBuffer());
    tcpListener.TransferOwnership(_socketId, context);
}

private void OnSuspending(object sender, SuspendingEventArgs e)
{
    var deferral = e.SuspendingOperation.GetDeferral();

    TransferOwnership(_tcpListener);
    deferral.Complete();
}
```
在背景工作的事件處理常式中：
   -  首先，取得背景工作延遲以使用非同步方法處理事件。
```csharp
var deferral = taskInstance.GetDeferral();
```
   -  接下來，從事件引數擷取 SocketActivityTriggerDetails，並尋找引發事件的原因：
```csharp
var details = taskInstance.TriggerDetails as SocketActivityTriggerDetails;
    var socketInformation = details.SocketInformation;
    switch (details.Reason)
```
   -   如果事件是因為通訊端活動而引發，請在通訊端建立 DataReader，並以非同步方式載入讀取器，然後按照 app 的設計使用資料。 請注意，您必須將擁有權移轉回通訊端代理程式，之後通訊端再次活動時才會收到通知。

   以下範例中，在通訊端上接收的文字會以快顯通顯示。

```csharp
case SocketActivityTriggerReason.SocketActivity:
            var socket = socketInformation.StreamSocket;
            DataReader reader = new DataReader(socket.InputStream);
            reader.InputStreamOptions = InputStreamOptions.Partial;
            await reader.LoadAsync(250);
            var dataString = reader.ReadString(reader.UnconsumedBufferLength);
            ShowToast(dataString);
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   如果事件是因為保持運作計時器到期而引發，則您的程式碼應透過通訊端傳送一些資料，以保持通訊端運作並重新啟動保持運作計時器。 同樣地，務必將擁有權移轉回通訊端代理程式以接收後來的事件通知：

```csharp
case SocketActivityTriggerReason.KeepAliveTimerExpired:
            socket = socketInformation.StreamSocket;
            DataWriter writer = new DataWriter(socket.OutputStream);
            writer.WriteBytes(Encoding.UTF8.GetBytes("Keep alive"));
            await writer.StoreAsync();
            writer.DetachStream();
            writer.Dispose();
            socket.TransferOwnership(socketInformation.Id); /* Important! */
            break;
```

   -   如果事件因為通訊端關閉而引發，請確認在建立新的通訊端之後，將擁有權移轉給通訊端代理程式。 在此範例中，主機名稱和連接埠會儲存在本機設定中，以供用來建立新的通訊端連線：

```csharp
case SocketActivityTriggerReason.SocketClosed:
            socket = new StreamSocket();
            socket.EnableTransferOwnership(taskInstance.Task.TaskId, SocketActivityConnectedStandbyAction.Wake);
            if (ApplicationData.Current.LocalSettings.Values["hostname"] == null)
            {
                break;
            }
            var hostname = (String)ApplicationData.Current.LocalSettings.Values["hostname"];
            var port = (String)ApplicationData.Current.LocalSettings.Values["port"];
            await socket.ConnectAsync(new HostName(hostname), port);
            socket.TransferOwnership(socketId);
            break;
```

   -   一旦事件通知處理完成，請記得將延遲完成：

```csharp
  deferral.Complete();
```

如需示範使用 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger) 和通訊端代理程式的範例，請參閱 [SocketActivityStreamSocket 範例](https://go.microsoft.com/fwlink/p/?LinkId=620606)。 通訊端初始化在 Scenario1\_Connect.xaml.cs 中執行，而背景作業實作是在 SocketActivityTask.cs 中。

您可能會注意到範例一旦建立新的通訊端或取得現有通訊端就會呼叫 **TransferOwnership**，而不是像本主題所描述使用 **OnSuspending** 事件處理常式。 這是因為範例著重在示範 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)，且在執行時沒有任何其他活動使用通訊端。 您的 app 可能更複雜，且應使用 **OnSuspending** 來決定呼叫 **TransferOwnership** 的時機。

## <a name="control-channel-triggers"></a>控制通道觸發程序
首先，請確認正確地使用控制通道 (CCT) 觸發程序。 如果您使用 [**DatagramSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.DatagramSocket)、[**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 或 [**StreamSocketListener**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocketListener) 連線，建議使用 [**SocketActivityTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)。 您可以為 **StreamSocket** 使用 CCT，但它們使用更多資源，且可能無法在連線待命模式中運作。

如果使用 WebSockets、[**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2)、[**System.Net.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) 或 [**Windows.Web.Http.HttpClient**](/uwp/api/windows.web.http.httpclient)，您必須使用 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)。

## <a name="controlchanneltrigger-with-websockets"></a>ontrolChannelTrigger 搭配 WebSockets
使用 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，有一些特殊考量。 在使用 **MessageWebSocket** 或 **StreamWebSocket** 搭配 **ControlChannelTrigger** 時，應遵循某些傳輸專屬的使用模式與最佳做法。 此外，這些考量也會影響在 **StreamWebSocket** 接收封包要求的處理方式。 在 **MessageWebSocket** 接收封包的要求不會受到影響。

在使用 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，應遵循下列使用模式與最佳做法：

-   必須隨時通知未處理的通訊端接收。 這樣才能夠發生推播通知工作。
-   WebSocket 通訊協定會定義持續連線訊息的標準模型。 [  **WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) 類別可將用戶端起始的 WebSocket 通訊協定持續連線訊息傳送至伺服器。 應用程式應該為 KeepAliveTrigger 將 **WebSocketKeepAlive** 類別登錄為 TaskEntryPoint。

有些特殊考量會影響在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 接收封包要求的處理方式。 特別是在使用 **StreamWebSocket** 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，您的 app 必須使用原始非同步模式來處理讀取，而非 C# 與 VB.NET 中的 **await** 模型或 C++ 中的工作。 稍後在本節的程式碼範例中會說明原始非同步模式。

使用原始非同步模式可讓 Windows 同步處理 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 背景工作上的 [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法與傳回的接收完成回呼。 當傳回完成回呼之後，就會叫用 **Run** 方法。 這可確保 app 可以在叫用 **Run** 方法之前收到資料/錯誤。

請注意，app 必須發出另一個讀取，才能從完成回呼傳回控制項，這一點很重要。 請注意，還有一點也很重要，就是 [**DataReader**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) 不能直接搭配 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 傳輸使用，因為那會中斷上述的同步處理。 不支援直接在傳輸上使用 [**DataReader.LoadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.loadasync) 方法。 相反地，[**StreamWebSocket.InputStream**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.streamwebsocket.inputstream) 屬性上 [**IInputStream.ReadAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.iinputstream.readasync) 方法傳回的 [**IBuffer**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IBuffer) 可在稍後傳送至 [**DataReader.FromBuffer**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader.frombuffer) 方法，以便進一步處理。

下列範例顯示如何使用原始非同步模式在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上處理讀取。

```csharp
void PostSocketRead(int length)
{
    try
    {
        var readBuf = new Windows.Storage.Streams.Buffer((uint)length);
        var readOp = socket.InputStream.ReadAsync(readBuf, (uint)length, InputStreamOptions.Partial);
        readOp.Completed = (IAsyncOperationWithProgress<IBuffer, uint>
            asyncAction, AsyncStatus asyncStatus) =>
        {
            switch (asyncStatus)
            {
                case AsyncStatus.Completed:
                case AsyncStatus.Error:
                    try
                    {
                        // GetResults in AsyncStatus::Error is called as it throws a user friendly error string.
                        IBuffer localBuf = asyncAction.GetResults();
                        uint bytesRead = localBuf.Length;
                        readPacket = DataReader.FromBuffer(localBuf);
                        OnDataReadCompletion(bytesRead, readPacket);
                    }
                    catch (Exception exp)
                    {
                        Diag.DebugPrint("Read operation failed:  " + exp.Message);
                    }
                    break;
                case AsyncStatus.Canceled:

                    // Read is not cancelled in this sample.
                    break;
           }
       };
   }
   catch (Exception exp)
   {
       Diag.DebugPrint("failed to post a read failed with error:  " + exp.Message);
   }
}
```

讀取完成處理常式一定會在叫用 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 背景工作的 [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法之前觸發。 Windows 的內部同步處理要等候從讀取完成回呼傳回 app。 App 通常會在讀取完成回呼快速處理從 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 收到的資料或錯誤。 訊息本身會在 **IBackgroundTask.Run** 方法的內容中處理。 在下列範例中，會使用讀取完成處理常式插入訊息且背景工作稍後要處理的訊息佇列來說明這一點。

下列範例顯示搭配原始非同步模式在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上處理讀取時要使用的讀取完成處理常式。

```csharp
public void OnDataReadCompletion(uint bytesRead, DataReader readPacket)
{
    if (readPacket == null)
    {
        Diag.DebugPrint("DataReader is null");

        // Ideally when read completion returns error,
        // apps should be resilient and try to
        // recover if there is an error by posting another recv
        // after creating a new transport, if required.
        return;
    }
    uint buffLen = readPacket.UnconsumedBufferLength;
    Diag.DebugPrint("bytesRead: " + bytesRead + ", unconsumedbufflength: " + buffLen);

    // check if buffLen is 0 and treat that as fatal error.
    if (buffLen == 0)
    {
        Diag.DebugPrint("Received zero bytes from the socket. Server must have closed the connection.");
        Diag.DebugPrint("Try disconnecting and reconnecting to the server");
        return;
    }

    // Perform minimal processing in the completion
    string message = readPacket.ReadString(buffLen);
    Diag.DebugPrint("Received Buffer : " + message);

    // Enqueue the message received to a queue that the push notify
    // task will pick up.
    AppContext.messageQueue.Enqueue(message);

    // Post another receive to ensure future push notifications.
    PostSocketRead(MAX_BUFFER_LENGTH);
}
```

WebSocket 的額外細節是持續連線處理常式。 WebSocket 通訊協定會定義持續連線訊息的標準模型。

使用 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 時，請針對 KeepAliveTrigger 將 [**WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) 類別登錄為 [**TaskEntryPoint**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint)，以允許取消暫停應用程式並定期將持續連線訊息傳送至伺服器 (遠端端點)。 這應該在背景登錄應用程式程式碼以及套件資訊清單中完成。

[  **Windows.Sockets.WebSocketKeepAlive**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.WebSocketKeepAlive) 的這個工作進入點必須在下列兩個地方指定：

-   在來源程式碼中建立 KeepAliveTrigger 觸發程序時 (請參閱下列範例)。
-   在持續連線背景工作宣告的應用程式套件資訊清單。

下列範例會在應用程式資訊清單的 &lt;Application&gt; 元素下，新增網路觸發程序通知與持續連線觸發程序。

```xml
  <Extensions>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Background.PushNotifyTask">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
    <Extension Category="windows.backgroundTasks"
         Executable="$targetnametoken$.exe"
         EntryPoint="Windows.Networking.Sockets.WebSocketKeepAlive">
      <BackgroundTasks>
        <Task Type="controlChannel" />
      </BackgroundTasks>
    </Extension>
  </Extensions>
```

在 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 內容中使用 **await** 陳述式，以及在 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket)、[**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket) 上使用非同步操作時，app 要非常小心。 **Task&lt;bool&gt;** 物件可用來登錄推播通知的 **ControlChannelTrigger**，以及 **StreamWebSocket** 上的 WebSocket 持續連線，然後連線傳輸。 在登錄程序中，**StreamWebSocket** 傳輸會設為 **ControlChannelTrigger** 的傳輸，然後發佈讀取。 **Task.Result** 會封鎖目前的執行緒，直到執行完工作的所有步驟，且陳述式傳回訊息本文後才停止。 必須等到方法傳回 True 或 False 後，才能解析工作。 這樣可以確保整個方法都已執行。 **Task** 可以包含多個 **await** 陳述式，這些陳述式由 **Task** 保護。 使用 **StreamWebSocket** 或 **MessageWebSocket** 做為傳輸時，這個模式應與 **ControlChannelTrigger** 物件搭配使用。 對於那些需要長時間完成的操作 (例如，一般非同步讀取操作)，app 應使用上述的原始非同步模式。

下列範例會登錄推播通知的 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 與 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 上的 WebSocket 持續連線。

```csharp
private bool RegisterWithControlChannelTrigger(string serverUri)
{
    // Make sure the objects are created in a system thread
    // Demonstrate the core registration path
    // Wait for the entire operation to complete before returning from this method.
    // The transport setup routine can be triggered by user control, by network state change
    // or by keepalive task
    Task<bool> registerTask = RegisterWithCCTHelper(serverUri);
    return registerTask.Result;
}

async Task<bool> RegisterWithCCTHelper(string serverUri)
{
    bool result = false;
    socket = new StreamWebSocket();

    // Specify the keepalive interval expected by the server for this app
    // in order of minutes.
    const int serverKeepAliveInterval = 30;

    // Specify the channelId string to differentiate this
    // channel instance from any other channel instance.
    // When background task fires, the channel object is provided
    // as context and the channel id can be used to adapt the behavior
    // of the app as required.
    const string channelId = "channelOne";

    // For websockets, the system does the keepalive on behalf of the app
    // But the app still needs to specify this well known keepalive task.
    // This should be done here in the background registration as well
    // as in the package manifest.
    const string WebSocketKeepAliveTask = "Windows.Networking.Sockets.WebSocketKeepAlive";

    // Try creating the controlchanneltrigger if this has not been already
    // created and stored in the property bag.
    ControlChannelTriggerStatus status;

    // Create the ControlChannelTrigger object and request a hardware slot for this app.
    // If the app is not on LockScreen, then the ControlChannelTrigger constructor will
    // fail right away.
    try
    {
        channel = new ControlChannelTrigger(channelId, serverKeepAliveInterval,
                                   ControlChannelTriggerResourceType.RequestHardwareSlot);
    }
    catch (UnauthorizedAccessException exp)
    {
        Diag.DebugPrint("Is the app on lockscreen? " + exp.Message);
        return result;
    }

    Uri serverUriInstance;
    try
    {
        serverUriInstance = new Uri(serverUri);
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Error creating URI: " + exp.Message);
        return result;
    }

    // Register the apps background task with the trigger for keepalive.
    var keepAliveBuilder = new BackgroundTaskBuilder();
    keepAliveBuilder.Name = "KeepaliveTaskForChannelOne";
    keepAliveBuilder.TaskEntryPoint = WebSocketKeepAliveTask;
    keepAliveBuilder.SetTrigger(channel.KeepAliveTrigger);
    keepAliveBuilder.Register();

    // Register the apps background task with the trigger for push notification task.
    var pushNotifyBuilder = new BackgroundTaskBuilder();
    pushNotifyBuilder.Name = "PushNotificationTaskForChannelOne";
    pushNotifyBuilder.TaskEntryPoint = "Background.PushNotifyTask";
    pushNotifyBuilder.SetTrigger(channel.PushNotificationTrigger);
    pushNotifyBuilder.Register();

    // Tie the transport method to the ControlChannelTrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the ControlChannelTrigger object can be reused to plug in a new transport by
    // calling UsingTransport API again.
    try
    {
        channel.UsingTransport(socket);

        // Connect the socket
        //
        // If connect fails or times out it will throw exception.
        // ConnectAsync can also fail if hardware slot was requested
        // but none are available
        await socket.ConnectAsync(serverUriInstance);

        // Call WaitForPushEnabled API to make sure the TCP connection has
        // been established, which will mean that the OS will have allocated
        // any hardware slot for this TCP connection.
        //
        // In this sample, the ControlChannelTrigger object was created by
        // explicitly requesting a hardware slot.
        //
        // On systems that without connected standby, if app requests hardware slot as above,
        // the system will fallback to a software slot automatically.
        //
        // On systems that support connected standby,, if no hardware slot is available, then app
        // can request a software slot by re-creating the ControlChannelTrigger object.
        status = channel.WaitForPushEnabled();
        if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
            && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
        {
            throw new Exception(string.Format("Neither hardware nor software slot could be allocated. ChannelStatus is {0}", status.ToString()));
        }

        // Store the objects created in the property bag for later use.
        CoreApplication.Properties.Remove(channel.ControlChannelTriggerId);

        var appContext = new AppContext(this, socket, channel, channel.ControlChannelTriggerId);
        ((IDictionary<string, object>)CoreApplication.Properties).Add(channel.ControlChannelTriggerId, appContext);
        result = true;

        // Almost done. Post a read since we are using streamwebsocket
        // to allow push notifications to be received.
        PostSocketRead(MAX_BUFFER_LENGTH);
    }
    catch (Exception exp)
    {
         Diag.DebugPrint("RegisterWithCCTHelper Task failed with: " + exp.Message);

         // Exceptions may be thrown for example if the application has not
         // registered the background task class id for using real time communications
         // broker in the package manifest.
    }
    return result
}
```

如需使用 [**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的詳細資訊，請參閱 [ControlChannelTrigger StreamWebSocket 範例](https://go.microsoft.com/fwlink/p/?linkid=251232)。

## <a name="controlchanneltrigger-with-httpclient"></a>ControlChannelTrigger 搭配 HttpClient
使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，有一些特殊考量。 在使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 搭配 **ControlChannelTrigger** 時，應遵循某些傳輸專屬的使用模式與最佳做法。 此外，這些考量也會影響在 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 接收封包要求的處理方式。

**注意** 目前不支援使用 SSL 的   [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 使用網路觸發程序功能和 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)。
 
在使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，應遵循下列使用模式與最佳做法：

-   App 可能需要先在 [System.Net.Http](https://go.microsoft.com/fwlink/p/?linkid=227894) 命名空間中的 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 或 [HttpClientHandler](https://go.microsoft.com/fwlink/p/?linkid=241638) 物件上設定各種屬性及標頭，然後才能將要求傳送到特定 URI。
-   App 需要先執行一個起始要求來測試並正確地設定傳輸，然後再建立與 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 搭配使用的 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 傳輸。 一旦 app 判斷傳輸已正確設定之後，就可以將 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 物件設定為與 **ControlChannelTrigger** 物件搭配使用的傳輸物件。 設計這個處理程序的目的是為了避免中斷透過傳輸所建立的連線。 使用 SSL 搭配憑證時，應用程式可能需要顯示一個對話方塊來輸入 PIN 或顯示可以從多個憑證進行選擇的選項。 可能需要 Proxy 驗證和伺服器驗證。 如果 Proxy 或伺服器驗證已到期，就可能會關閉連線。 應用程式因應這些驗證到期的其中一個方法是設定計時器。 需要使用 HTTP 重新導向時，無法保證能夠建立穩定的第二個連線。 起始測試要求將會確保 app 可以先使用最新的重新導向 URL，然後才使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 物件當作傳輸與 **ControlChannelTrigger** 物件搭配使用。

與其他網路傳輸不同的是，[HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 物件無法直接傳送至 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 物件的 [**UsingTransport**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.controlchanneltrigger.usingtransport) 方法。 相反地，必須特別建構 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 物件，以搭配 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 物件和 **ControlChannelTrigger** 使用。 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 物件是使用 [RtcRequestFactory.Create](https://go.microsoft.com/fwlink/p/?linkid=259154) 方法建立的。 然後，所建立的 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 物件會傳送到 **UsingTransport** 方法。

下列範例顯示如何建構 [HttpRequestMessage](https://go.microsoft.com/fwlink/p/?linkid=259153) 物件以便搭配 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 物件與 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 使用。

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

public HttpRequestMessage httpRequest;
public HttpClient httpClient;
public HttpRequestMessage httpRequest;
public ControlChannelTrigger channel;
public Uri serverUri;

private void SetupHttpRequestAndSendToHttpServer()
{
    try
    {
        // For HTTP based transports that use the RTC broker, whenever we send next request, we will abort the earlier
        // outstanding http request and start new one.
        // For example in case when http server is taking longer to reply, and keep alive trigger is fired in-between
        // then keep alive task will abort outstanding http request and start a new request which should be finished
        // before next keep alive task is triggered.
        if (httpRequest != null)
        {
            httpRequest.Dispose();
        }

        httpRequest = RtcRequestFactory.Create(HttpMethod.Get, serverUri);

        SendHttpRequest();
    }
        catch (Exception e)
    {
        Diag.DebugPrint("Connect failed with: " + e.ToString());
        throw;
    }
}
```

有些特殊考量會影響在 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 上要求傳送 HTTP 要求以起始接收已處理的回應的方式。 特別是當使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，您的 app 必須使用 Task 來處理傳送，而非使用 **await** 模型。

使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 時，針對 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 背景工作上的 [**IBackgroundTask.Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) 方法與接收完成回呼的傳回並沒有同步。 基於這個原因，app 只能使用 **Run** 方法中的封鎖 HttpResponseMessage 技術，並等候接收整個回應。

使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 顯然與 [**StreamSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamSocket)、[**MessageWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.MessageWebSocket) 或 [**StreamWebSocket**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.StreamWebSocket) 傳輸不同。 因為 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 程式碼，[HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 接收回呼是透過 Task 傳遞至 app。 這表示當資料或錯誤分派至 app 時，會立即觸發 **ControlChannelTrigger** 推播通知工作。 在下列範例中，程式碼會將 [HttpClient.SendAsync](https://go.microsoft.com/fwlink/p/?linkid=241637) 方法傳回的 responseTask 儲存到全域儲存區，推播通知工作會提取並內嵌處理這個傳回值。

下列範例顯示當搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 使用時，如何處理 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 上的傳送要求。

```csharp
using System;
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
using Windows.Networking.Sockets;

private void SendHttpRequest()
{
    if (httpRequest == null)
    {
        throw new Exception("HttpRequest object is null");
    }

    // Tie the transport method to the controlchanneltrigger object to push enable it.
    // Note that if the transport' s TCP connection is broken at a later point of time,
    // the controlchanneltrigger object can be reused to plugin a new transport by
    // calling UsingTransport API again.
    channel.UsingTransport(httpRequest);

    // Call the SendAsync function to kick start the TCP connection establishment
    // process for this http request.
    Task<HttpResponseMessage> httpResponseTask = httpClient.SendAsync(httpRequest);

    // Call WaitForPushEnabled API to make sure the TCP connection has been established,
    // which will mean that the OS will have allocated any hardware slot for this TCP connection.
    ControlChannelTriggerStatus status = channel.WaitForPushEnabled();
    Diag.DebugPrint("WaitForPushEnabled() completed with status: " + status);
    if (status != ControlChannelTriggerStatus.HardwareSlotAllocated
        && status != ControlChannelTriggerStatus.SoftwareSlotAllocated)
    {
        throw new Exception("Hardware/Software slot not allocated");
    }

    // The HttpClient receive callback is delivered via a Task to the app.
    // The notification task will fire as soon as the data or error is dispatched
    // Enqueue the responseTask returned by httpClient.sendAsync
    // into a queue that the push notify task will pick up and process inline.
    AppContext.messageQueue.Enqueue(httpResponseTask);
}
```

下列範例顯示當搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 時，如何讀取接收到的回應。

```csharp
using System.Net;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

public string ReadResponse(Task<HttpResponseMessage> httpResponseTask)
{
    string message = null;
    try
    {
        if (httpResponseTask.IsCanceled || httpResponseTask.IsFaulted)
        {
            Diag.DebugPrint("Task is cancelled or has failed");
            return message;
        }
        // We' ll wait until we got the whole response.
        // This is the only supported scenario for HttpClient for ControlChannelTrigger.
        HttpResponseMessage httpResponse = httpResponseTask.Result;
        if (httpResponse == null || httpResponse.Content == null)
        {
            Diag.DebugPrint("Cannot read from httpresponse, as either httpResponse or its content is null. try to reset connection.");
        }
        else
        {
            // This is likely being processed in the context of a background task and so
            // synchronously read the Content' s results inline so that the Toast can be shown.
            // before we exit the Run method.
            message = httpResponse.Content.ReadAsStringAsync().Result;
        }
    }
    catch (Exception exp)
    {
        Diag.DebugPrint("Failed to read from httpresponse with error:  " + exp.ToString());
    }
    return message;
}
```

如需使用 [HttpClient](https://go.microsoft.com/fwlink/p/?linkid=241637) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的詳細資訊，請參閱 [ControlChannelTrigger HttpClient 範例](https://go.microsoft.com/fwlink/p/?linkid=258323)。

## <a name="controlchanneltrigger-with-ixmlhttprequest2"></a>ControlChannelTrigger 搭配 IXMLHttpRequest2
使用 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時，有一些特殊考量。 在使用 **IXMLHTTPRequest2** 搭配 **ControlChannelTrigger** 時，應遵循某些傳輸專屬的使用模式與最佳做法。 使用 **ControlChannelTrigger** 不會影響在 **IXMLHTTPRequest2** 要求傳送或接收 HTTP 要求的處理方式。

使用 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 時的使用模式與最佳做法

-   [  **IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 物件當作傳輸時的存留期只包含一個要求/回應。 與 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 物件搭配使用時，可以只建立和設定 **ControlChannelTrigger** 物件一次，然後重複呼叫 [**UsingTransport**](https://docs.microsoft.com/uwp/api/windows.networking.sockets.controlchanneltrigger.usingtransport) 方法，而每次呼叫關聯一個新的 **IXMLHTTPRequest2** 物件。 App 應該在提供新的 **IXMLHTTPRequest2** 物件之前先刪除之前的 **IXMLHTTPRequest2** 物件，以確保 app 不會超用配置的資源限制。
-   App 可能需在呼叫 [**Send**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-send) 方法之前，先呼叫 [**SetProperty**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-setproperty) 和 [**SetRequestHeader**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-setrequestheader) 方法來設定 HTTP 傳輸。
-   App 需要先執行一個起始 [**Send**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nf-msxml6-ixmlhttprequest2-send) 要求來測試並正確地設定傳輸，然後再建立與 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 搭配使用的傳輸。 一旦 app 判斷傳輸已正確設定之後，就可以將 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 物件設定為與 **ControlChannelTrigger** 搭配使用的傳輸物件。 設計這個處理程序的目的是為了避免中斷透過傳輸所建立的連線。 使用 SSL 搭配憑證時，應用程式可能需要顯示一個對話方塊來輸入 PIN 或顯示可以從多個憑證進行選擇的選項。 可能需要 Proxy 驗證和伺服器驗證。 如果 Proxy 或伺服器驗證已到期，就可能會關閉連線。 應用程式因應這些驗證到期的其中一個方法是設定計時器。 需要使用 HTTP 重新導向時，無法保證能夠建立穩定的第二個連線。 起始測試要求將會確保 app 可以先使用最新的重新導向 URL，然後才使用 **IXMLHTTPRequest2** 物件當作傳輸與 **ControlChannelTrigger** 物件搭配使用。

如需使用 [**IXMLHTTPRequest2**](https://docs.microsoft.com/previous-versions/windows/desktop/api/msxml6/nn-msxml6-ixmlhttprequest2) 搭配 [**ControlChannelTrigger**](https://docs.microsoft.com/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger) 的詳細資訊，請參閱 [ControlChannelTrigger 搭配 IXMLHTTPRequest2 範例](https://go.microsoft.com/fwlink/p/?linkid=258538)。

## <a name="important-apis"></a>重要 API
* [SocketActivityTrigger](/uwp/api/Windows.ApplicationModel.Background.SocketActivityTrigger)
* [ControlChannelTrigger](/uwp/api/Windows.Networking.Sockets.ControlChannelTrigger)
