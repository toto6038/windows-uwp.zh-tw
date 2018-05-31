---
author: stevewhims
description: 通訊端是低階資料傳輸技術，許多網路通訊協定在其上實作。 UWP 為用戶端-伺服器或對等應用程式提供 TCP 與 UDP 通訊端類別，不需要指定連線是長期或已建立的連線。
title: 通訊端
ms.assetid: 23B10A3C-E33F-4CD6-92CB-0FFB491472D6
ms.author: stwhi
ms.date: 11/27/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 70d51009fc2231900ad0eba8dfc0e706433e2338
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1690984"
---
# <a name="sockets"></a>通訊端
通訊端是低階資料傳輸技術，許多網路通訊協定在其上實作。 UWP 為用戶端-伺服器或對等應用程式提供 TCP 與 UDP 通訊端類別，不需要指定連線是長期或已建立的連線。

本主題著重於如何使用[**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets?branch=live)命名空間中的通用 Windows 平台 (UWP) 通訊端類別。 但您也可以在 UWP app 使用[Windows Sockets 2 (Winsock)](https://msdn.microsoft.com/library/windows/desktop/ms740673)。

> [!NOTE]
> 因為[網路隔離](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)的關係，所以 Windows 禁止在同一部電腦上執行的兩個 UWP app 之間建立通訊端連線 (Sockets 或 WinSock)，不論是透過本機迴路位址 (127.0.0.0) 或是透過明確指定本機 IP 位址。 如需 UWP app 通訊機制的詳細資訊，請查看[應用程式間通訊](/windows/uwp/app-to-app/index?branch=live)。

## <a name="build-a-basic-tcp-socket-client-and-server"></a>建置基本 TCP 通訊端用戶端和伺服器
TCP (傳輸控制通訊協定) 通訊端為長期連線提供雙向的低階網路資料傳輸。 TCP 通訊端是網際網路上大多數網路通訊協定所使用的基本功能。 為了示範基本 TCP 作業，以下範例程式碼示範[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live)和[**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live)透過 TCP 傳送和接收資料，形成 echo 用戶端和伺服器。

為了一開始盡可能使用較少的移動部分&mdash;和暫時避免網路隔離的問題&mdash;建立新的專案，並將以下用戶端和伺服器程式碼放在同一個專案。

您的專案中需要[宣告應用程式功能](../packaging/app-capability-declarations.md)。 開啟應用程式套件資訊清單來源檔案 (`Package.appxmanifest`檔案)，然後在 [功能] 索引標籤，勾選**\[私人網路 (用戶端 & 伺服器)\]**。 這是`Package.appxmanifest`標記中的樣子。

```xml
<Capability Name="privateNetworkClientServer" />
```

如果您透過網際網路連線，可以宣告`internetClientServer`，而不是`privateNetworkClientServer`。 **StreamSocket**和**StreamSocketListener**需要宣告其中一個應用程式功能。

### <a name="an-echo-client-and-server-using-tcp-sockets"></a>echo 用戶端和伺服器，使用 TCP 通訊端
建構[**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live)並開始接聽傳入的 TCP 連線。 每次用戶端與**StreamSocketListener**建立連線，都會引發[**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)事件。

也建構[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live)、連線到伺服器、傳送要求，及接收回應。

建立名為 `StreamSocketAndListenerPage` 的**頁面**。 將 XAML 標記放入`StreamSocketAndListenerPage.xaml`，並將命令式程式碼放入`StreamSocketAndListenerPage`類別中。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="TCP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="StreamSocket &amp; StreamSocketListener"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose an arbitrary port number.
static string PortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();

        // The ConnectionReceived event is raised when connections are received.
        streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await streamSocketListener.BindServiceNameAsync(StreamSocketAndListenerPage.PortNumber);

        this.serverListBox.Items.Add("server is listening...");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    string request;
    using (var streamReader = new StreamReader(args.Socket.InputStream.AsStreamForRead()))
    {
        request = await streamReader.ReadLineAsync();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = args.Socket.OutputStream.AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the StreamSocket and establish a connection to the echo server.
        using (var streamSocket = new Windows.Networking.Sockets.StreamSocket())
        {
            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            var hostName = new Windows.Networking.HostName("localhost");

            this.clientListBox.Items.Add("client is trying to connect...");

            await streamSocket.ConnectAsync(hostName, StreamSocketAndListenerPage.PortNumber);

            this.clientListBox.Items.Add("client connected");

            // Send a request to the echo server.
            string request = "Hello, World!";
            using (Stream outputStream = streamSocket.OutputStream.AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));

            // Read data from the echo server.
            string response;
            using (Stream inputStream = streamSocket.InputStream.AsStreamForRead())
            {
                using (StreamReader streamReader = new StreamReader(inputStream))
                {
                    response = await streamReader.ReadLineAsync();
                }
            }

            this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\" ", response));
        }

        this.clientListBox.Items.Add("client closed its socket");
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}
```

```cpp
#include <ppltasks.h>
#include <sstream>

    ...
    
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;

    ...

private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();

            // The ConnectionReceived event is raised when connections are received.
            streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &StreamSocketAndListenerPage::StreamSocketListener_ConnectionReceived);

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            // Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
            // For this example, we'll choose an arbitrary port number.
            Concurrency::create_task(streamSocketListener->BindServiceNameAsync(L"1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is listening...");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        try
        {
            auto dataReader = ref new DataReader(args->Socket->InputStream);

            Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                [=](unsigned int bytesLoaded)
            {
                unsigned int stringLength = dataReader->ReadUInt32();
                Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                    [=](unsigned int bytesLoaded)
                {
                    Platform::String^ request = dataReader->ReadString(bytesLoaded);
                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                        [=]
                    {
                        std::wstringstream wstringstream;
                        wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                        this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                    }));

                    // Echo the request back as the response.
                    auto dataWriter = ref new DataWriter(args->Socket->OutputStream);
                    dataWriter->WriteUInt32(request->Length());
                    dataWriter->WriteString(request);
                    Concurrency::create_task(dataWriter->StoreAsync()).then(
                        [=](unsigned int)
                    {
                        dataWriter->DetachStream();

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                            [=]()
                        {
                            std::wstringstream wstringstream;
                            wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                        }));

                        delete this->streamSocketListener;
                        this->streamSocketListener = nullptr;

                        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message); }));
        }
    }

    void StartClient()
    {
        try
        {
            // Create the StreamSocket and establish a connection to the echo server.
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is trying to connect...");

            Concurrency::create_task(this->streamSocket->ConnectAsync(hostName, L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->clientListBox->Items->Append(L"client connected");

                // Send a request to the echo server.
                auto dataWriter = ref new DataWriter(this->streamSocket->OutputStream);
                auto request = ref new Platform::String(L"Hello, World!");
                dataWriter->WriteUInt32(request->Length());
                dataWriter->WriteString(request);

                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](Concurrency::task< unsigned int >)
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    Concurrency::create_task(dataWriter->FlushAsync()).then(
                        [=](Concurrency::task< bool >)
                    {
                        dataWriter->DetachStream();

                        // Read data from the echo server.
                        auto dataReader = ref new DataReader(this->streamSocket->InputStream);
                        Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
                            [=](unsigned int bytesLoaded)
                        {
                            unsigned int stringLength = dataReader->ReadUInt32();
                            Concurrency::create_task(dataReader->LoadAsync(stringLength)).then(
                                [=](unsigned int bytesLoaded)
                            {
                                Platform::String^ response = dataReader->ReadString(bytesLoaded);
                                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                                    [=]
                                {
                                    std::wstringstream wstringstream;
                                    wstringstream << L"client received the response: \"" << response->Data() << L"\"";
                                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                                    delete this->streamSocket;
                                    this->streamSocket = nullptr;

                                    this->clientListBox->Items->Append(L"client closed its socket");
                                }));
                            });
                        });
                    });
                });
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }
```

## <a name="references-to-streamsockets-in-c-ppl-continuations"></a>C++ PPL 接續中的 StreamSockets 參考
[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) 會維持作用，只要其輸入/輸出串流上有作用中的讀取/寫入 (例如，可以存取[**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)事件處理常式中的[**StreamSocketListenerConnectionReceivedEventArgs.Socket**](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs.Socket))。 當您呼叫[**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync?branch=live) (或`ReadAsync/WriteAsync/StoreAsync`)，它會有該通訊端的參考（透過通訊端的輸入串流），直到**LoadAsync**的完成處理常式完成執行為止。

但平行模式程式庫 (PPL) 預設不會內嵌排程工作接續。 亦即，新增接續工作 (使用`task::then()`) 並不保證接續工作會當做完成處理常式內嵌執行。

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);
    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        // Work in here isn't guaranteed to execute inline as the completion handler of the LoadAsync.
    });
}
```

從**StreamSocket**的角度，在執行接續主體之前完成處理常式會完成執行（而且通訊端可供處置）。 因此，如果您想要在接續中使用通訊端，不要處置通訊端，您需要直接（透過 lambda 擷取）或間接 (透過在接續中繼續存取`args->Socket`) 參考通訊端並使用它，或強制內嵌接續工作。 您可以在[StreamSocket 範例](http://go.microsoft.com/fwlink/p/?LinkId=620609)中看到第一項技巧（lambda 擷取）的運作情形。 [建置基本 TCP 通訊端用戶端和伺服器](#build-a-basic-tcp-socket-client-and-server)上一節中的 C++ 程式碼使用第二項技巧&mdash;它將要求當做回應 echo 回來，並從其中一個最內層的接續存取`args->Socket`。

當您不要 echo 回應時適用第三項技巧。 您可以使用`task_continuation_context::use_synchronous_execution()`選項強制 PPL 內嵌執行接續主體。 以下程式碼範例示範如何做。

```cpp
void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    auto dataReader = ref new DataReader(args->Socket->InputStream);

    Concurrency::create_task(dataReader->LoadAsync(sizeof(unsigned int))).then(
        [=](unsigned int bytesLoaded)
    {
        unsigned int messageLength = dataReader->ReadUInt32();
        Concurrency::create_task(dataReader->LoadAsync(messageLength)).then(
            [=](unsigned int bytesLoaded)
        {
            Platform::String^ request = dataReader->ReadString(bytesLoaded);
            this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                [=]
            {
                std::wstringstream wstringstream;
                wstringstream << L"server received the request: \"" << request->Data() << L"\"";
                this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
            }));
        });
    }, Concurrency::task_continuation_context::use_synchronous_execution());
}
```

這個行為套用至[**Windows.Networking.Sockets**](/uwp/api/Windows.Networking.Sockets?branch=live)命名空間中的所有通訊端和 WebSockets 類別。 但用戶端案例通常將通訊端儲存在成員變數中，所以該問題最適用於[**StreamSocketListener.ConnectionReceived**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)案例中，如上述。

## <a name="build-a-basic-udp-socket-client-and-server"></a>建置基本 UDP 通訊端用戶端和伺服器
，UDP（使用者資料包通訊協定）通訊端與 TCP 通訊端類似，也提供雙向的低階網路資料傳輸。 但是，TCP 通訊端適用於長期連線，UDP 通訊端適用於不需要建立連線的應用程式。 因為 UDP 通訊端不會維持兩端點的連線，所以是遠端電腦之間網路功能快速又簡單的解決方案。 不過，UDP 通訊端不會確保網路封包的完整性，甚至不會確認封包是否到達遠端目的地。 所以您的應用程式需要設計成可容許這點。 使用 UDP 通訊端的應用程式範例包括區域網路探索和區域聊天用戶端。

為了示範基本 UDP 作業，以下範例程式碼示範[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live)類別用來透過 UDP 傳送和接收資料，形成 echo 用戶端和伺服器。 建立新的專案，並將以下用戶端和伺服器程式碼放在同一個專案。 如同 TCP 通訊端，您將需要宣告**\[私人網路 (用戶端 & 伺服器)\]** 應用程式功能。

### <a name="an-echo-client-and-server-using-udp-sockets"></a>echo 用戶端和伺服器，使用 UDP 通訊端
建構[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live)扮演 echo 伺服器的角色、繫結至特定連接埠號碼、接聽連入的 UDP 訊息，然後將訊息 echo 回來。 通訊端上收到訊息時，會引發[**DatagramSocket.MessageReceived**](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived)事件。

建構另一個**DatagramSocket**扮演 echo 用戶端的角色、繫結至特定連接埠號碼、傳送 UDP 訊息，然後接收回應。

將 XAML 標記放入`MainPage.xaml`，並將命令式程式碼放入`MainPage`類別中。

建立名為 `DatagramSocketPage` 的**頁面**。 將 XAML 標記放入`DatagramSocketPage.xaml`，並將命令式程式碼放入`DatagramSocketPage`類別中。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel>
        <TextBlock Margin="9.6,0" Style="{StaticResource TitleTextBlockStyle}" Text="UDP socket example"/>
        <TextBlock Margin="7.2,0,0,0" Style="{StaticResource HeaderTextBlockStyle}" Text="DatagramSocket"/>
    </StackPanel>

    <Grid Grid.Row="1">
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition/>
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="*"/>
        </Grid.ColumnDefinitions>
        <TextBlock Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="client"/>
        <ListBox x:Name="clientListBox" Grid.Row="1" Margin="9.6"/>
        <TextBlock Grid.Column="1" Margin="9.6" Style="{StaticResource SubtitleTextBlockStyle}" Text="server"/>
        <ListBox x:Name="serverListBox" Grid.Column="1" Grid.Row="1" Margin="9.6"/>
    </Grid>
</Grid>
```

```csharp
// Every protocol typically has a standard port number. For example, HTTP is typically 80, FTP is 20 and 21, etc.
// For this example, we'll choose different arbitrary port numbers for client and server, since both will be running on the same machine.
static string ClientPortNumber = "1336";
static string ServerPortNumber = "1337";

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    this.StartServer();
    this.StartClient();
}

private async void StartServer()
{
    try
    {
        var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        // The ConnectionReceived event is raised when connections are received.
        serverDatagramSocket.MessageReceived += ServerDatagramSocket_MessageReceived;

        this.serverListBox.Items.Add("server is about to bind...");

        // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
        await serverDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ServerPortNumber);

        this.serverListBox.Items.Add(string.Format("server is bound to port number {0}", DatagramSocketPage.ServerPortNumber));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.serverListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ServerDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string request;
    using (DataReader dataReader = args.GetDataReader())
    {
        request = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server received the request: \"{0}\"", request)));

    // Echo the request back as the response.
    using (Stream outputStream = (await sender.GetOutputStreamAsync(args.RemoteAddress, DatagramSocketPage.ClientPortNumber)).AsStreamForWrite())
    {
        using (var streamWriter = new StreamWriter(outputStream))
        {
            await streamWriter.WriteLineAsync(request);
            await streamWriter.FlushAsync();
        }
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add(string.Format("server sent back the response: \"{0}\"", request)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.serverListBox.Items.Add("server closed its socket"));
}

private async void StartClient()
{
    try
    {
        // Create the DatagramSocket and establish a connection to the echo server.
        var clientDatagramSocket = new Windows.Networking.Sockets.DatagramSocket();

        clientDatagramSocket.MessageReceived += ClientDatagramSocket_MessageReceived;

        // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
        var hostName = new Windows.Networking.HostName("localhost");

        this.clientListBox.Items.Add("client is about to bind...");

        await clientDatagramSocket.BindServiceNameAsync(DatagramSocketPage.ClientPortNumber);

        this.clientListBox.Items.Add(string.Format("client is bound to port number {0}", DatagramSocketPage.ClientPortNumber));

        // Send a request to the echo server.
        string request = "Hello, World!";
        using (var serverDatagramSocket = new Windows.Networking.Sockets.DatagramSocket())
        {
            using (Stream outputStream = (await serverDatagramSocket.GetOutputStreamAsync(hostName, DatagramSocketPage.ServerPortNumber)).AsStreamForWrite())
            {
                using (var streamWriter = new StreamWriter(outputStream))
                {
                    await streamWriter.WriteLineAsync(request);
                    await streamWriter.FlushAsync();
                }
            }
        }

        this.clientListBox.Items.Add(string.Format("client sent the request: \"{0}\"", request));
    }
    catch (Exception ex)
    {
        Windows.Networking.Sockets.SocketErrorStatus webErrorStatus = Windows.Networking.Sockets.SocketError.GetStatus(ex.GetBaseException().HResult);
        this.clientListBox.Items.Add(webErrorStatus.ToString() != "Unknown" ? webErrorStatus.ToString() : ex.Message);
    }
}

private async void ClientDatagramSocket_MessageReceived(Windows.Networking.Sockets.DatagramSocket sender, Windows.Networking.Sockets.DatagramSocketMessageReceivedEventArgs args)
{
    string response;
    using (DataReader dataReader = args.GetDataReader())
    {
        response = dataReader.ReadString(dataReader.UnconsumedBufferLength).Trim();
    }

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add(string.Format("client received the response: \"{0}\"", response)));

    sender.Dispose();

    await this.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => this.clientListBox.Items.Add("client closed its socket"));
}
```

```cpp
#include <ppltasks.h>
#include <sstream>

    ...

using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;

    ...

private:
    Windows::Networking::Sockets::DatagramSocket^ clientDatagramSocket;
    Windows::Networking::Sockets::DatagramSocket^ serverDatagramSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->StartServer();
        this->StartClient();
    }

private:
    void StartServer()
    {
        try
        {
            this->serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            // The ConnectionReceived event is raised when connections are received.
            this->serverDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ServerDatagramSocket_MessageReceived);

            this->serverListBox->Items->Append(L"server is about to bind...");

            // Start listening for incoming TCP connections on the specified port. You can specify any port that's not currently in use.
            Concurrency::create_task(this->serverDatagramSocket->BindServiceNameAsync("1337")).then(
                [=]
            {
                this->serverListBox->Items->Append(L"server is bound to port number 1337");
            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ServerDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ request = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"server received the request: \"" << request->Data() << L"\"";
            this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        // Echo the request back as the response.
        Concurrency::create_task(sender->GetOutputStreamAsync(args->RemoteAddress, "1336")).then(
            [=](IOutputStream^ outputStream)
        {
            auto dataWriter = ref new DataWriter(outputStream);
            dataWriter->WriteString(request);
            Concurrency::create_task(dataWriter->StoreAsync()).then(
                [=](unsigned int)
            {
                dataWriter->DetachStream();

                this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
                    [=]()
                {
                    std::wstringstream wstringstream;
                    wstringstream << L"server sent back the response: \"" << request->Data() << L"\"";
                    this->serverListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));

                    delete this->serverDatagramSocket;
                    this->serverDatagramSocket = nullptr;

                    this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->serverListBox->Items->Append(L"server closed its socket"); }));
                }));
            });
        });
    }

    void StartClient()
    {
        try
        {
            // Create the DatagramSocket and establish a connection to the echo server.
            this->clientDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();

            this->clientDatagramSocket->MessageReceived += ref new TypedEventHandler<Windows::Networking::Sockets::DatagramSocket^, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^>(this, &DatagramSocketPage::ClientDatagramSocket_MessageReceived);

            // The server hostname that we will be establishing a connection to. In this example, the server and client are in the same process.
            auto hostName = ref new Windows::Networking::HostName(L"localhost");

            this->clientListBox->Items->Append(L"client is about to bind...");

            Concurrency::create_task(this->clientDatagramSocket->BindServiceNameAsync("1336")).then(
                [=]
            {
                this->clientListBox->Items->Append(L"client is bound to port number 1336");
            });

            // Send a request to the echo server.
            auto serverDatagramSocket = ref new Windows::Networking::Sockets::DatagramSocket();
            Concurrency::create_task(serverDatagramSocket->GetOutputStreamAsync(hostName, "1337")).then(
                [=](IOutputStream^ outputStream)
            {
                auto request = ref new Platform::String(L"Hello, World!");
                auto dataWriter = ref new DataWriter(outputStream);
                dataWriter->WriteString(request);
                Concurrency::create_task(dataWriter->StoreAsync()).then(
                    [=](unsigned int)
                {
                    dataWriter->DetachStream();
                    std::wstringstream wstringstream;
                    wstringstream << L"client sent the request: \"" << request->Data() << L"\"";
                    this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
                });

            });
        }
        catch (Platform::Exception^ ex)
        {
            Windows::Networking::Sockets::SocketErrorStatus webErrorStatus = Windows::Networking::Sockets::SocketError::GetStatus(ex->HResult);
            this->serverListBox->Items->Append(webErrorStatus.ToString() != L"Unknown" ? webErrorStatus.ToString() : ex->Message);
        }
    }

    void ClientDatagramSocket_MessageReceived(Windows::Networking::Sockets::DatagramSocket^ sender, Windows::Networking::Sockets::DatagramSocketMessageReceivedEventArgs^ args)
    {
        DataReader^ dataReader = args->GetDataReader();
        Platform::String^ response = dataReader->ReadString(dataReader->UnconsumedBufferLength);
        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler(
            [=]
        {
            std::wstringstream wstringstream;
            wstringstream << L"client received the response: \"" << response->Data() << L"\"";
            this->clientListBox->Items->Append(ref new Platform::String(wstringstream.str().c_str()));
        }));

        delete this->clientDatagramSocket;
        this->clientDatagramSocket = nullptr;

        this->Dispatcher->RunAsync(CoreDispatcherPriority::Normal, ref new DispatchedHandler([=]() {this->clientListBox->Items->Append(L"client closed its socket"); }));
    }
```

## <a name="background-operations-and-the-socket-broker"></a>背景作業和通訊端代理程式
您可以使用通訊端代理程式，並控制通道觸發程序，以確保您的應用程式不在前景時於通訊端上正常收到連線或資料。 如需詳細資訊，請參閱[背景網路通訊](network-communications-in-the-background.md)。

## <a name="batched-sends"></a>批次傳送
每當您寫入與通訊端相關的資料流，會發生從使用者模式（您的程式碼）到核心模式（網路堆疊所在）的轉換。 如果您一次寫入許多緩衝區，這些重複的轉換會造成重大負荷。 批次傳送是一種同時傳送多個資料緩衝區的方式，可避免該負荷。 如果您的 app 正在進行 VoIP、VPN 或其他涉及盡可能有效移動大量資料的工作，這會特別有用。

本節示範兩個可搭配[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live)或已連接[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live) 的批次傳送技巧。

若要取得基準，讓我們了解如何透過有效率的方式傳送大量緩衝區。 以下是最少示範，使用**StreamSocket**。

```csharp
protected override async void OnNavigatedTo(NavigationEventArgs e)
{
    var streamSocketListener = new Windows.Networking.Sockets.StreamSocketListener();
    streamSocketListener.ConnectionReceived += this.StreamSocketListener_ConnectionReceived;
    await streamSocketListener.BindServiceNameAsync("1337");

    var streamSocket = new Windows.Networking.Sockets.StreamSocket();
    await streamSocket.ConnectAsync(new Windows.Networking.HostName("localhost"), "1337");
    this.SendMultipleBuffersInefficiently(streamSocket, "Hello, World!");
    //this.BatchedSendsCSharpOnly(streamSocket, "Hello, World!");
    //this.BatchedSendsAnyUWPLanguage(streamSocket, "Hello, World!");
}

private async void StreamSocketListener_ConnectionReceived(Windows.Networking.Sockets.StreamSocketListener sender, Windows.Networking.Sockets.StreamSocketListenerConnectionReceivedEventArgs args)
{
    using (var dataReader = new DataReader(args.Socket.InputStream))
    {
        dataReader.InputStreamOptions = InputStreamOptions.Partial;
        while (true)
        {
            await dataReader.LoadAsync(256);
            if (dataReader.UnconsumedBufferLength == 0) break;
            IBuffer requestBuffer = dataReader.ReadBuffer(dataReader.UnconsumedBufferLength);
            string request = Windows.Security.Cryptography.CryptographicBuffer.ConvertBinaryToString(Windows.Security.Cryptography.BinaryStringEncoding.Utf8, requestBuffer);
            Debug.WriteLine(string.Format("server received the request: \"{0}\"", request));
        }
    }
}

// This implementation incurs kernel transition overhead for each packet written.
private async void SendMultipleBuffersInefficiently(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    foreach (IBuffer packet in packetsToSend)
    {
        await streamSocket.OutputStream.WriteAsync(packet);
    }
}
```

```cpp
#include <ppltasks.h>
#include <sstream>

    ...

using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml::Navigation;

    ...

private:
    Windows::Networking::Sockets::StreamSocketListener^ streamSocketListener;
    Windows::Networking::Sockets::StreamSocket^ streamSocket;

protected:
    virtual void OnNavigatedTo(NavigationEventArgs^ e) override
    {
        this->streamSocketListener = ref new Windows::Networking::Sockets::StreamSocketListener();
        streamSocketListener->ConnectionReceived += ref new TypedEventHandler<Windows::Networking::Sockets::StreamSocketListener^, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^>(this, &BatchedSendsPage::StreamSocketListener_ConnectionReceived);
        Concurrency::create_task(this->streamSocketListener->BindServiceNameAsync(L"1337")).then(
            [=]
        {
            this->streamSocket = ref new Windows::Networking::Sockets::StreamSocket();
            Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337")).then(
                [=](Concurrency::task< void >)
            {
                this->SendMultipleBuffersInefficiently(L"Hello, World!");
                // this->BatchedSendsAnyUWPLanguage(L"Hello, World!");
            }, Concurrency::task_continuation_context::use_synchronous_execution());
        });
    }

private:
    void StreamSocketListener_ConnectionReceived(Windows::Networking::Sockets::StreamSocketListener^ sender, Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs^ args)
    {
        auto dataReader = ref new DataReader(args->Socket->InputStream);
        dataReader->InputStreamOptions = Windows::Storage::Streams::InputStreamOptions::Partial;
        this->ReceiveStringRecurse(dataReader, args->Socket);
    }

    void ReceiveStringRecurse(DataReader^ dataReader, Windows::Networking::Sockets::StreamSocket^ streamSocket)
    {
        Concurrency::create_task(dataReader->LoadAsync(256)).then(
            [this, dataReader, streamSocket](unsigned int bytesLoaded)
        {
            if (bytesLoaded == 0) return;
            Platform::String^ message = dataReader->ReadString(bytesLoaded);
            ::OutputDebugString(message->Data());
            this->ReceiveStringRecurse(dataReader, streamSocket);
        });
    }

    // This implementation incurs kernel transition overhead for each packet written.
    void SendMultipleBuffersInefficiently(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            Concurrency::create_task(this->streamSocket->OutputStream->WriteAsync(element)).wait();
        }
    }
```

第一個範例示範更有效率的技巧，只有在使用 C# 時適用。 變更`OnNavigatedTo`呼叫`BatchedSendsCSharpOnly`，而不呼叫`SendMultipleBuffersInefficiently`。

```csharp
// A C#-only technique for batched sends.
private async void BatchedSendsCSharpOnly(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingTasks = new System.Threading.Tasks.Task[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingTasks[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]).AsTask();
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete.
    System.Threading.Tasks.Task.WaitAll(pendingTasks);
}
```

下一個範例適用於任何 UWP 語言，不只適用於 C#（但在此以 C# 示範）。 它依賴[**StreamSocket.OutputStream**](/uwp/api/windows.networking.sockets.streamsocket.OutputStream)和[**DatagramSocket.OutputStream**](/uwp/api/windows.networking.sockets.datagramsocket.OutputStream)中一起批次傳送的行為。 此技巧在該輸出資料流上呼叫[**FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync)，在 Windows 10，這保證只在輸出資料流上的所有作業都完成之後傳回。

```csharp
// An implementation of batched sends suitable for any UWP language.
private async void BatchedSendsAnyUWPLanguage(Windows.Networking.Sockets.StreamSocket streamSocket, string message)
{
    var packetsToSend = new List<IBuffer>();
    for (int count = 0; count < 5; ++count) { packetsToSend.Add(Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(message, Windows.Security.Cryptography.BinaryStringEncoding.Utf8)); }

    var pendingWrites = new IAsyncOperationWithProgress<uint, uint>[packetsToSend.Count];

    for (int index = 0; index < packetsToSend.Count; ++index)
    {
        // track all pending writes as tasks, but don't wait on one before beginning the next.
        pendingWrites[index] = streamSocket.OutputStream.WriteAsync(packetsToSend[index]);
        // Don't modify any buffer's contents until the pending writes are complete.
    }

    // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
    await streamSocket.OutputStream.FlushAsync();
}
```

```cpp
private:
    // An implementation of batched sends suitable for any UWP language.
    void BatchedSendsAnyUWPLanguage(Platform::String^ message)
    {
        std::vector< IBuffer^ > packetsToSend{};
        std::vector< IAsyncOperationWithProgress< unsigned int, unsigned int >^ >pendingWrites{};
        for (unsigned int count = 0; count < 5; ++count)
        {
            packetsToSend.push_back(Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(message, Windows::Security::Cryptography::BinaryStringEncoding::Utf8));
        }

        for (auto element : packetsToSend)
        {
            // track all pending writes as tasks, but don't wait on one before beginning the next.
            pendingWrites.push_back(this->streamSocket->OutputStream->WriteAsync(element));
            // Don't modify any buffer's contents until the pending writes are complete.
        }

        // Wait for all of the pending writes to complete. This step enables batched sends on the output stream.
        Concurrency::create_task(this->streamSocket->OutputStream->FlushAsync());
    }
```

在您的程式碼中使用批次傳送時有一些重要的限制。

-   在非同步寫入完成之前，您無法對正在寫入的 **IBuffer** 執行個體修改內容。
-   **FlushAsync** 模式只適用於 **StreamSocket.OutputStream** 和 **DatagramSocket.OutputStream**。
-   **FlushAsync** 模式只適用於 Windows 10 和後續版本。
-   在其他情況下，請改用 [**Task.WaitAll**](https://docs.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.waitall?view=netcore-2.0#System_Threading_Tasks_Task_WaitAll_System_Threading_Tasks_Task___)，而不要使用 **FlushAsync** 模式。

## <a name="port-sharing-for-datagramsocket"></a>DatagramSocket 的連接埠共用
您可以設定[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live)與繫結至相同地址/連接埠的其他 Win32 或 UWP 多點傳送通訊端並存。 在繫結或連接通訊端之前將[**DatagramSocketControl.MulticastOnly**](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly)設定為`true`，執行此動作。 您可以從**DatagramSocket**物件本身存取**DatagramSocketControl**執行個體 (透過其[**DatagramSocket.Control**](/uwp/api/windows.networking.sockets.datagramsocket.Control)屬性)。

## <a name="providing-a-client-certificate-with-the-streamsocket-class"></a>提供具有 StreamSocket 類別的用戶端憑證
[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) 支援使用 SSL/TLS 來驗證與用戶端 app 交談的伺服器。 在某些情況下，用戶端 app 必須使用 SSL/TLS 的用戶端憑證向伺服器驗證本身。 您可以在繫結或連接通訊端（SSL/TLS 交握開始前必須先設定通訊端）之前，使用[**StreamSocketControl.ClientCertificate**](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate)屬性提供用戶端憑證。 您可以從**StreamSocket**物件本身存取**StreamSocketControl**執行個體 (透過其[**StreamSocket.Control**](/uwp/api/windows.networking.sockets.streamsocket.Control)屬性)。 如果伺服器要求用戶端憑證，Windows 會使用提供的用戶端憑證來回應。

使用採用[**SocketProtectionLevel**](/uwp/api/windows.networking.sockets.socketprotectionlevel?branch=live)的[**StreamSocket.ConnectAsync**](/uwp/api/windows.networking.sockets.streamsocket.connectasync?branch=live)覆寫，如這個最少程式碼範例所示。

```csharp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
var certificateQuery = new Windows.Security.Cryptography.Certificates.CertificateQuery();
certificateQuery.StoreName = "MY";
IReadOnlyList<Windows.Security.Cryptography.Certificates.Certificate> certificates = await Windows.Security.Cryptography.Certificates.CertificateStores.FindAllAsync(certificateQuery);
if (certificates.Count > 0)
{
    streamSocket.Control.ClientCertificate = certificates[0];
    await streamSocket.ConnectAsync(hostName, "1337", Windows.Networking.Sockets.SocketProtectionLevel.Tls12);
}
```

```cpp
// For this code to work, you need at least one certificate to be present in the user MY certificate store.
// Plugging a smartcard into a smartcard reader connected to your PC will achieve that.
// Also, your project needs to declare the sharedUserCertificates app capability.
auto certificateQuery = ref new Windows::Security::Cryptography::Certificates::CertificateQuery();
certificateQuery->StoreName = L"MY";
Concurrency::create_task(Windows::Security::Cryptography::Certificates::CertificateStores::FindAllAsync(certificateQuery)).then(
    [=](IVectorView< Windows::Security::Cryptography::Certificates::Certificate^ >^ certificates)
{
    if (certificates->Size > 0)
    {
        this->streamSocket->Control->ClientCertificate = certificates->GetAt(0);
        Concurrency::create_task(this->streamSocket->ConnectAsync(ref new Windows::Networking::HostName(L"localhost"), L"1337", Windows::Networking::Sockets::SocketProtectionLevel::Tls12)).then(
            [=]
        {
            ...
        });
    }
});
```

## <a name="handling-exceptions"></a>處理例外狀況
[**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live)、[**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live) 或 [**StreamSocketListener**](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live) 作業上發生的錯誤會傳回為 **HRESULT** 值。 您可以將該**HRESULT**值傳送至[**SocketError.GetStatus**](/uwp/api/windows.networking.sockets.socketerror.getstatus?branch=live)方法，以將它轉換為[**SocketErrorStatus**](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus?branch=live)列舉值。

大多數 **SocketErrorStatus** 列舉值對應於原生 Windows 通訊端作業傳回的錯誤。 您的應用程式可以切換開啟 **SocketErrorStatus** 列舉值，依據例外狀況的發生原因來修改應用程式行為。

針對參數驗證錯誤，您可以使用來自例外狀況的 **HRESULT**，深入了解更多關於錯誤的詳細資訊。 可能的**HRESULT**值列在`Winerror.h`中，可以在 SDK 安裝程式中找到 (例如，在資料夾`C:\Program Files (x86)\Windows Kits\10\Include\<VERSION>\shared`中)。 針對大多數的參數驗證錯誤，傳回的 **HRESULT** 是 **E_INVALIDARG**。

如果傳遞的字串不是有效的主機名稱，[**HostName**](/uwp/api/Windows.Networking.HostName?branch=live)建構函式會擲回例外狀況。 例如，它包含不允許的字元，如果使用者在您的應用程式中輸入主機名稱，可能發生此狀況。 在 try/catch 區塊內建構**HostName**。 如此一來，如果發生例外狀況，app 可通知使用者並要求新的主機名稱。

## <a name="important-apis"></a>重要 API
* [CertificateQuery](/uwp/api/windows.security.cryptography.certificates.certificatequery?branch=live)
* [CertificateStores.FindAllAsync](/uwp/api/windows.security.cryptography.certificates.certificatestores.findallasync?branch=live)
* [DatagramSocket](/uwp/api/Windows.Networking.Sockets.DatagramSocket?branch=live)
* [DatagramSocket.BindServiceNameAsync](/uwp/api/windows.networking.sockets.datagramsocket.bindservicenameasync?branch=live)
* [DatagramSocket.Control](/uwp/api/windows.networking.sockets.datagramsocket.Control)
* [DatagramSocket.GetOutputStreamAsync](/uwp/api/windows.networking.sockets.datagramsocket.getoutputstreamasync?branch=live)
* [DatagramSocket.MessageReceived](/uwp/api/Windows.Networking.Sockets.DatagramSocket.MessageReceived)
* [DatagramSocketControl.MulticastOnly](/uwp/api/Windows.Networking.Sockets.DatagramSocketControl.MulticastOnly)
* [DatagramSocketMessageReceivedEventArgs](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs?branch=live)
* [DatagramSocketMessageReceivedEventArgs.GetDataReader](/uwp/api/windows.networking.sockets.datagramsocketmessagereceivedeventargs.GetDataReader)
* [DataReader.LoadAsync](/uwp/api/windows.storage.streams.datareader.loadasync?branch=live)
* [IOutputStream.FlushAsync](/uwp/api/windows.storage.streams.ioutputstream.FlushAsync)
* [SocketError.GetStatus](/uwp/api/windows.networking.sockets.socketerror.getstatus?branch=live)
* [SocketErrorStatus](/uwp/api/Windows.Networking.Sockets.SocketErrorStatus?branch=live)
* [SocketProtectionLevel](/uwp/api/windows.networking.sockets.socketprotectionlevel?branch=live)
* [StreamSocket](/uwp/api/Windows.Networking.Sockets.StreamSocket?branch=live)
* [StreamSocketControl.ClientCertificate](/uwp/api/windows.networking.sockets.streamsocketcontrol.ClientCertificate)
* [StreamSocket.ConnectAsync](/uwp/api/windows.networking.sockets.streamsocket.connectasync?branch=live)
* [StreamSocket.InputStream](/uwp/api/windows.networking.sockets.streamsocket.InputStream)
* [StreamSocket.OutputStream](/uwp/api/windows.networking.sockets.streamsocket.OutputStream)
* [StreamSocketListener](/uwp/api/Windows.Networking.Sockets.StreamSocketListener?branch=live)
* [StreamSocketListener.BindServiceNameAsync](/uwp/api/windows.networking.sockets.streamsocketlistener.bindservicenameasync?branch=live)
* [StreamSocketListener.ConnectionReceived](/uwp/api/Windows.Networking.Sockets.StreamSocketListener.ConnectionReceived)
* [StreamSocketListenerConnectionReceivedEventArgs](/uwp/api/windows.networking.sockets.streamsocketlistenerconnectionreceivedeventargs?branch=live)
* [Windows.Networking.Sockets](/uwp/api/Windows.Networking.Sockets?branch=live)

## <a name="related-topics"></a>相關主題
* [Windows Sockets 2 (Winsock)](https://msdn.microsoft.com/library/windows/desktop/ms740673)
* [如何設定網路功能](https://msdn.microsoft.com/library/windows/apps/hh770532.aspx)
* [應用程式間通訊](/windows/uwp/app-to-app/index?branch=live)

## <a name="samples"></a>範例
* [StreamSocket 範例](http://go.microsoft.com/fwlink/p/?LinkId=620609)
