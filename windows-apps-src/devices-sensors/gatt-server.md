---
author: msatranjr
title: Bluetooth GATT 伺服器
description: 本文概述 Bluetooth 泛用屬性的設定檔 (GATT) 伺服器的通用 Windows 平台 (UWP) 應用程式，以及一般使用案例的範例程式碼。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 27154fbb535b76995fba97702e65a9c0b2a8291c
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "610763"
---
# <a name="bluetooth-gatt-server"></a>Bluetooth GATT 伺服器


**重要 API**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


本文示範 Bluetooth 一般屬性 (GATT) 伺服器 api （英文） 通用 Windows 平台 (UWP) 應用程式，以及一般 GATT 伺服器工作的範例程式碼： 
- 定義支援的服務
- 因此可以依遠端用戶端探索到發佈伺服器
- Advertise 服務支援
- 回應中讀取及寫入要求
- 將通知傳送給已訂閱的用戶端

## <a name="overview"></a>概觀
Windows 通常會作業之用戶端角色。 不過，許多案例發生 Windows 作為 Bluetooth LE GATT 伺服器也需要的。 IoT 裝置，大多數跨平台 ble.s 使用通訊幾乎所有的案例需要 Windows GATT 伺服器。 此外，將通知傳送給附近 wearable 裝置已經成為需要此技術，以及在常用案例。  
> 請務必[GATT 用戶端文件](gatt-client.md)中的所有概念都的清除再繼續進行。  

伺服器作業將會以服務提供者和 GattLocalCharacteristic 為中心。 這兩種類別會提供宣告、 實作和公開資料的遠端裝置的合作夥伴階層所需的功能。

## <a name="define-the-supported-services"></a>定義支援的服務
您的應用程式可能會宣告會由 Windows 所發佈的一或多個服務。 UUID 唯一地識別每個服務。 

### <a name="attributes-and-uuids"></a>Attributes 及 Uuid
每個服務、 特性和描述元定義自己的唯一 128 位元 UUID 是依照。
> Windows api （英文） 所有使用此術語的 GUID，但 Bluetooth 標準定義這些為 Uuid。 我們基於下列兩個字詞可以互換讓我們將會繼續使用 UUID 的字詞。 

如果屬性是標準和由 Bluetooth 簽章定義已定義，它同時也會相對應的 16 位元簡短識別碼 (例如電池層級 UUID 是 0000**2A19**-0000-1000年位 8000 位 00805F9B34FB 和簡短識別碼是 0x2A19)。 這些標準 Uuid 可以看到[GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx)和[GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)中。

如果您的應用程式已實作自己的自訂服務，自訂 UUID 必須對要產生。 這是輕鬆地完成在 Visual Studio 中透過工具]-> [CreateGuid （使用選項 5 取得"xxxxxxxx-是-...是"格式）。 此 uuid 現在可用於宣告新的本機服務、 特性或描述元。

#### <a name="restricted-services"></a>受限制的服務
下列服務系統所保留並無法在此階段發佈：
1. 裝置資訊服務 (DIS)
2. 一般屬性設定檔服務 (GATT)
3. 一般存取設定檔服務 （間距）
4. 人性化介面裝置服務 (HOGP)
5. 掃描參數服務 (SCP)

> 嘗試建立封鎖的 service 會導致 BluetoothError.DisabledByPolicy CreateAsync 呼叫所傳回。

#### <a name="generated-attributes"></a>產生的屬性
下列描述元會自動產生之系統根據期間建立的特性提供 GattLocalCharacteristicParameters：
1. 用戶端的特性設定 （如果此特性標示為 indicatable 或 notifiable）。
2. Characteristic 使用者描述 （如果 UserDescription 屬性已設定）。 請參閱 GattLocalCharacteristicParameters.UserDescription 屬性的詳細資訊。
3. Characteristic 格式 （每個指定的簡報格式的一個描述元）。  請參閱 GattLocalCharacteristicParameters.PresentationFormats 屬性的詳細資訊。
4. Characteristic 彙總格式 （如果指定一個以上的簡報格式）。  如需詳細資訊的 GattLocalCharacteristicParameters.See PresentationFormats 屬性。
5. Characteristic 擴充屬性 （如果此特性已標示的延伸的內容位元）。

> 擴充屬性描述元的值會決定透過 ReliableWrites 和 WritableAuxiliaries 特性屬性。

> 試圖建立保留描述元會導致發生例外狀況。

> 請注意，廣播不支援這一次。  指定廣播 GattCharacteristicProperty 會產生例外狀況。

### <a name="build-up-the-heirarchy-of-services-and-characteristics"></a>設定服務以及特性合作夥伴階層建立
GattServiceProvider 用來建立和 advertise 根主要服務定義。  每個服務需要很自己 ServiceProvider 物件會以 GUID： 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 主要服務是 GATT 樹狀目錄的最上層網站。 主要服務包含特性以及其他服務 （稱為 「 包含' 或次要服務）。 

現在，填入必要的特性及描述元服務：

```csharp
GattLocalCharacteristicResult characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid1, ReadParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_readCharacteristic = characteristicResult.Characteristic;
_readCharacteristic.ReadRequested += ReadCharacteristic_ReadRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid2, WriteParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_writeCharacteristic = characteristicResult.Characteristic;
_writeCharacteristic.WriteRequested += WriteCharacteristic_WriteRequested;

characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid3, NotifyParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
_notifyCharacteristic = characteristicResult.Characteristic;
_notifyCharacteristic.SubscribedClientsChanged += SubscribedClientsChanged;
```
如上述，這也是宣告的每個特性支援的作業的事件處理常式的理想位置。  應用程式必須正確回應要求的定義和設定事件處理常式的屬性支援每個要求類型。  失敗的註冊處理常式會導致立即使用*UnlikelyError*完成，系統的要求。

### <a name="constant-characteristics"></a>常特性
有時，有不會變更應用程式的存留期間的特性值。 在此情況下，建議您先宣告以避免不必要的應用程式啟用常數特性： 

```csharp
byte[] value = new byte[] {0x21};
var constantParameters = new GattLocalCharacteristicParameters
{
    CharacteristicProperties = (GattCharacteristicProperties.Read),
    StaticValue = value.AsBuffer(),
    ReadProtectionLevel = GattProtectionLevel.Plain,
};

var characteristicResult = await serviceProvider.Service.CreateCharacteristicAsync(uuid4, constantParameters);
if (characteristicResult.Error != BluetoothError.Success)
{
    // An error occurred.
    return;
}
```
## <a name="publish-the-service"></a>發佈該服務
此服務已完全定義下一步是將發佈的服務支援。 這會告知 OS 遠端裝置執行服務探索時應傳回該服務。  您必須設定兩個屬性-IsDiscoverable 和 IsConnectable：  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**： 通告通告進行裝置可供搜尋中的遠端裝置的易記名稱。
- **IsConnectable**： 通告用於周邊角色可連接通告。

> 當服務時可搜尋和 Connectable，系統會將服務 Uuid 新增通告封包。  通告封包中有僅 31 位元組與 128 位元 UUID 佔用這些 16 ！

> 請注意當服務發佈在前景中，應用程式必須先呼叫 StopAdvertising 時之應用程式暫停。

## <a name="respond-to-read-and-write-requests"></a>回應中讀取及寫入要求
我們正上方時宣告所需的特性，GattLocalCharacteristics 必須事件-ReadRequested、 WriteRequested 及 SubscribedClientsChanged 3 類型。

### <a name="read"></a>讀取
如果遠端裝置嘗試從特性讀取值 （不常數值），會呼叫 ReadRequested 事件。 讀取在呼叫 args （包含遠端裝置的相關資訊） 以及此特性會傳遞給代理人： 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ... 

async void ReadCharacteristic_ReadRequested(GattLocalCharacteristic sender, GattReadRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    // Our familiar friend - DataWriter.
    var writer = new DataWriter();
    // populate writer w/ some data. 
    // ... 

    var request = await args.GetRequestAsync();
    request.RespondWithValue(writer.DetachBuffer());
    
    deferral.Complete();
}
``` 

### <a name="write"></a>寫入
當遠端裝置嘗試將值寫入特性時、 WriteRequested 事件會在呼叫有關的詳細資料的遠端裝置，將寫入的特性與本身的值： 

```csharp
characteristic.ReadRequested += Characteristic_ReadRequested;
// ...

async void WriteCharacteristic_WriteRequested(GattLocalCharacteristic sender, GattWriteRequestedEventArgs args)
{
    var deferral = args.GetDeferral();
    
    var request = await args.GetRequestAsync();
    var reader = DataReader.FromBuffer(request.Value);
    // Parse data as necessary. 

    if (request.Option == GattWriteOption.WriteWithResponse)
    {
        request.Respond();
    }
    
    deferral.Complete();
}
```
有 2 類型的寫入-和未回應。 若要了解遠端裝置執行的寫入類型使用 GattWriteOption （GattWriteRequest 物件的屬性）。 

## <a name="send-notifications-to-subscribed-clients"></a>將通知傳送給已訂閱的用戶端
最常見的 GATT Server 操作通知執行資料發佈至遠端裝置的重要函數。 有時，您將想要通知所有已訂閱的用戶端但 othertimes 要選擇哪個裝置傳送的新值： 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

當新的裝置所訂閱的通知時，取得呼叫 SubscribedClientsChanged 事件： 

```csharp
characteristic.SubscribedClientsChanged += SubscribedClientsChanged;
// ...

void _notifyCharacteristic_SubscribedClientsChanged(GattLocalCharacteristic sender, object args)
{
    List<GattSubscribedClient> clients = sender.SubscribedClients;
    // Diff the new list of clients from a previously saved one 
    // to get which device has subscribed for notifications. 

    // You can also just validate that the list of clients is expected for this app.  
}

```
> 應用程式可以取得的最大通知大小與 MaxNotificationSize 屬性的特定用戶端的附註。  任何大於最大大小的資料會截斷系統。
