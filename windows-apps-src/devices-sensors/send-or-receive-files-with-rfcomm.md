---
author: msatranjr
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: 藍牙 RFCOMM
description: 本文將概略說明通用 Windows 平台 (UWP) app 中的藍牙 RFCOMM，並提供範例程式碼來說明如何傳送或接收檔案。
ms.author: misatran
ms.date: 07/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 27002fed78356c9a93289d1ccabbada04ea5ed30
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "4681034"
---
# <a name="bluetooth-rfcomm"></a>藍牙 RFCOMM

**重要 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529)

本文將概略說明通用 Windows 平台 (UWP) app 中的藍牙 RFCOMM，並提供範例程式碼來說明如何傳送或接收檔案。

## <a name="overview"></a>概觀

[**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) 命名空間中的 API 建構在適用於 Windows 裝置的現有模式上，包括 [**enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) 和 [**instantiation**](https://msdn.microsoft.com/library/windows/apps/BR225654)。 資料讀取和寫入是為了利用 [**established data stream patterns**](https://msdn.microsoft.com/library/windows/apps/BR208119) 和 [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/BR241791) 中的物件所設計。 服務探索通訊協定 (SDP) 屬性具有值和預期的類型。 但是一些常見裝置的 SDP 屬性實作有誤，導致值並非預期的類型。 此外，許多 RFCOMM 的用法完全不需要額外的 SDP 屬性。 基於這些理由，此 API 提供未剖析之 SDP 資料的存取，開發人員可藉此取得所需的資訊。

RFCOMM API 運用了服務識別碼的概念。 雖然服務識別碼只是 128 位元的 GUID，但也通常會指定為 16 或 32 位元的整數。 RFCOMM API 為服務識別碼提供一個包裝函式，使它們可指定和做為 128 位元的 GUID 及 32 位元的整數使用，但不提供 16 位元的整數。 這對 API 而言不是問題，因為語言將自動轉換成 32 位元的整數，而且仍然可以正確地產生識別碼。

app 可以在背景工作中執行多步驟的裝置作業，因此即使 app 移至背景且暫停，作業仍然可以完成。 這可允許可靠的裝置提供服務 (例如永續性設定或韌體的變更) 及內容同步，不需要使用者在一旁監督進度列。 使用 [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297315) 提供裝置服務，使用 [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297337) 執行內容同步。 請注意，這些背景工作會限制 app 可在背景執行的時間，且不允許無限期的作業或同步。

如需 RFCOMM 作業詳細資訊的完整程式碼範例，請參閱 Github 上的[**藍牙 Rfcomm 交談範例**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothRfcommChat) 。  

## <a name="send-a-file-as-a-client"></a>以用戶端身分傳送檔案

傳送檔案時，依據所需的服務連線至成對裝置，是最基本的案例。 這牽涉到下列步驟：

-   使用 **RfcommDeviceService.GetDeviceSelector\*** 函式協助產生 AQS 查詢，該查詢可用於所需服務的列舉成對裝置執行個體。
-   選擇一個列舉裝置、建立一個 [**RfcommDeviceService**](https://msdn.microsoft.com/library/windows/apps/Dn263463)，並視需要讀取 SDP 屬性 (使用 [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) 剖析屬性的資料)。
-   建立通訊端，並且使用[**RfcommDeviceService.ConnectionHostName**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname.aspx) 和 [**RfcommDeviceService.ConnectionServiceName**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename.aspx) 屬性以[**StreamSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701504) 至具有適當參數的遠端裝置服務。
-   遵循建立的資料串流模式以讀取檔案中的資料區塊，並在通訊端的 [**StreamSocket.OutputStream**](https://msdn.microsoft.com/library/windows/apps/BR226920) 上將它傳送至裝置。

```csharp
Windows.Devices.Bluetooth.Rfcomm.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    var services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0)
    {
        // Initialize the target Bluetooth BR device
        var service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        // Check that the service meets this App's minimum requirement
        if (SupportsProtection(service) && IsCompatibleVersion(service))
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, e.g. click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
    case SocketProtectionLevel.PlainSocket:
        if ((service.MaxProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionWithAuthentication)
            || (service.MaxProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService service)
{
    var attributes = await service.GetSdpRawAttributesAsync(
        BluetothCacheMode.Uncached);
    var attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    var reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService m_service{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Enumerate devices with the object push service.
    Windows::Devices::Enumeration::DeviceInformationCollection services{
        co_await Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::GetDeviceSelector(
                Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush())) };

    if (services.Size() > 0)
    {
        // Initialize the target Bluetooth BR device.
        Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService service{
            co_await Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService::FromIdAsync(services.GetAt(0).Id()) };

        // Check that the service meets this App's minimum
        // requirement
        if (SupportsProtection(service)
            && co_await IsCompatibleVersion(service))
        {
            m_service = service;

            // Create a socket and connect to the target
            co_await m_socket.ConnectAsync(
                m_service.ConnectionHostName(),
                m_service.ConnectionServiceName(),
                Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can
            // wait for the user to take some action, e.g. click
            // a button to send a file to the device, which could
            // invoke the Picker and then send the picked file.
            // The transfer itself would use the Sockets API and
            // not the Rfcomm API, and so is omitted here for
            //brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    switch (service.ProtectionLevel())
    {
    case Windows::Networking::Sockets::SocketProtectionLevel::PlainSocket:
        if ((service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication)
            || (service.MaxProtectionLevel() == Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This application relies on CRC32 checking available in version 2.0 of the service.
const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A }; // UINT32.
const uint32_t MINIMUM_SERVICE_VERSION{ 200 };

Windows::Foundation::IAsyncOperation<bool> IsCompatibleVersion(Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService const& service)
{
    auto attributes{
        co_await service.GetSdpRawAttributesAsync(Windows::Devices::Bluetooth::BluetoothCacheMode::Uncached) };

    auto attribute{ attributes.Lookup(SERVICE_VERSION_ATTRIBUTE_ID) };
    auto reader{ Windows::Storage::Streams::DataReader::FromBuffer(attribute) };

    // The first byte contains the attribute's type.
    byte attributeType{ reader.ReadByte() };
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint32_t version{ reader.ReadUInt32() };
        co_return (version >= MINIMUM_SERVICE_VERSION);
    }
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0)
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App's minimum
                // requirement
                if (SupportsProtection(service)
                    && IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, e.g. click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether it's authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaxProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication))
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute's type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->ReadUInt32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## <a name="receive-file-as-a-server"></a>以伺服器身分接收檔案

另一個常見的 RFCOMM app 案例是在電腦上代管服務，並向其他服務公開該服務。

-   建立 [**RfcommServiceProvider**](https://msdn.microsoft.com/library/windows/apps/Dn263511) 以通告所需的服務。
-   設定 SDP 屬性 (使用 [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) 來產生屬性的資料) 並開始通告 SDP 記錄以供其他裝置擷取。
-   為連線至用戶端裝置，建立一個通訊端接聽程式以開始接聽內送的連線要求。
-   在收到連線時，儲存連線的通訊端以稍後處理。
-   遵循建立的資料串流模式，以讀取通訊端的 InputStream 中的資料區塊，並將它儲存至檔案。

若要保留背景中的 RFCOMM 服務，請使用 [**RfcommConnectionTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.rfcommconnectiontrigger.aspx)。 連線至服務時會觸發背景工作。 開發人員會在背景工作收到通訊端的控制代碼。 背景工作是長時間執行，且只要通訊端為使用中就會持續執行。    

```csharp
Windows.Devices.Bluetooth.Rfcomm.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider = await Windows.Devices.Bluetooth.
        RfcommServiceProvider.CreateAsync(RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceived;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising(listener);
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    auto writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(SERVICE_VERSION);

    auto data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider.StopAdvertising();
    await listener.Close();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, e.g. click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cppwinrt
...
#include <winrt/Windows.Devices.Bluetooth.Rfcomm.h>
#include <winrt/Windows.Networking.Sockets.h>
#include <winrt/Windows.Storage.Streams.h>
...
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider m_provider{ nullptr };
Windows::Networking::Sockets::StreamSocket m_socket;

Windows::Foundation::IAsyncAction Initialize()
{
    // Initialize the provider for the hosted RFCOMM service.
    auto provider{ co_await Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider::CreateAsync(
        Windows::Devices::Bluetooth::Rfcomm::RfcommServiceId::ObexObjectPush()) };

    m_provider = provider;

    // Create a listener for this service and start listening.
    Windows::Networking::Sockets::StreamSocketListener listener;
    listener.ConnectionReceived({ this, &MainPage::OnConnectionReceived });

    co_await listener.BindServiceNameAsync(m_provider.ServiceId().AsString(),
        Windows::Networking::Sockets::SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes();
    m_provider.StartAdvertising(listener);
}

const uint32_t SERVICE_VERSION_ATTRIBUTE_ID{ 0x0300 };
const byte SERVICE_VERSION_ATTRIBUTE_TYPE{ 0x0A };   // UINT32.
const uint32_t SERVICE_VERSION{ 200 };

void InitializeServiceSdpAttributes()
{
    Windows::Storage::Streams::DataWriter writer;

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer.WriteUInt32(SERVICE_VERSION);

    auto data{ writer.DetachBuffer() };
    m_provider.SdpRawAttributes().Insert(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    Windows::Networking::Sockets::StreamSocketListener const& listener,
    Windows::Networking::Sockets::StreamSocketListenerConnectionReceivedEventArgs const& args)
{
    // Stop advertising/listening so that we're only serving one client
    m_provider.StopAdvertising();
    listener.Close();
    m_socket = args.Socket();

    // The client socket is connected. At this point the application can wait for
    // the user to take some action, e.g. click a button to receive a
    // file from the device, which could invoke the Picker and then save
    // the received file to the picked location. The transfer itself
    // would use the Sockets API and not the Rfcomm API, and so is
    // omitted here for brevity.
}
...
```

```cpp
Windows::Devices::Bluetooth::Rfcomm::RfcommServiceProvider^ _provider;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising(listener);
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE);
    // Then write the data
    writer->WriteUInt32(SERVICE_VERSION);

    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, e.g. click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```
