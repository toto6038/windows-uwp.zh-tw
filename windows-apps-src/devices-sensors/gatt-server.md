---
title: 藍牙 GATT 伺服器
description: 本文將概略說明適用於通用 Windows 平台 (UWP) app 的藍牙泛型屬性設定檔 (GATT) 伺服器，並提供常見使用案例的範例程式碼。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 551f8b925ffd56950ba893da7b81fefb4579f558
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635133"
---
# <a name="bluetooth-gatt-server"></a>藍牙 GATT 伺服器


**重要的 Api**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


本文示範適用於通用 Windows 平台 (UWP) app 的藍牙泛型屬性 (GATT) 伺服器 API，以及常見 GATT 伺服器工作的範例程式碼︰ 
- 定義支援的服務
- 發行伺服器，讓遠端用戶端可以找到它
- 服務的廣告支援
- 回應讀取和寫入要求
- 將通知傳送給訂閱的用戶端

## <a name="overview"></a>概觀
Windows 通常以用戶端角色操作。 不過，許多案例也都需要 Windows 作為藍牙 LE GATT 伺服器。 幾乎 IoT 裝置的所有案例，以及大部分跨平台 BLE 通訊，都需要 Windows 作為 GATT 伺服器。 此外，將通知傳送至附近的穿戴式裝置會變成也需要這項技術的常見案例。  
> 請確定清楚[GATT 用戶端文件](gatt-client.md)中的所有概念後，再繼續進行。  

伺服器的作業將圍繞著服務提供者和 GattLocalCharacteristic。 這兩個類別會提供用於宣告、 實作和公開 （expose） 至遠端裝置資料的階層架構所需的功能。

## <a name="define-the-supported-services"></a>定義支援的服務
您的應用程式可能會宣告將由 Windows 發行的一或多個服務。 每個服務都是透過 UUID 唯一識別。 

### <a name="attributes-and-uuids"></a>屬性和 UUID
每個服務、特性和描述元都是透過其專屬唯一 128 位元 UUID 所定義。
> Windows API 都會使用 GUID 這個詞彙，但藍牙標準將它們定義為 UUID。 基於我們的目的，這兩個詞彙可交換使用，因此我們將會繼續使用 UUID 這個詞彙。 

如果屬性是標準並且透過藍牙 SIG 定義所定義，則也會有對應的 16 位元簡短識別碼 (例如，電池電量 UUID 是 0000**2A19**-0000-1000-8000-00805F9B34FB，而簡短識別碼為 0x2A19)。 在 [GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx)和[GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)中可以看到這些標準 UUID。

如果您的應用程式要實作其專屬自訂服務，則必須產生自訂 UUID。 這在 Visual Studio 中透過 [工具] -> [CreateGuid] 很容易就可以完成 (使用選項 5 即可取得 "xxxxxxxx-xxxx-...xxxx" 格式的 UUID)。 這個 uuid 現在可以用來宣告新的本機服務、特性或描述元。

#### <a name="restricted-services"></a>限制服務
下列是系統所保留的服務，目前無法予以發行︰
1. 裝置資訊服務 (DIS)
2. 泛型屬性設定檔服務 (GATT)
3. 泛型存取設定檔服務 (GAP)
4. 人性化介面裝置服務 (HOGP)
5. 掃描參數服務 (SCP)

> 嘗試建立封鎖的服務，將會導致從 CreateAsync 呼叫傳回 BluetoothError.DisabledByPolicy。

#### <a name="generated-attributes"></a>產生的屬性
系統會根據建立特性期間所提供的 GattLocalCharacteristicParameters，自動產生下列描述元︰
1. 用戶端特性組態 (如果特性標示為 indicatable 或 notifiable)。
2. 特性使用者描述 (如果設定 UserDescription 屬性)。 如需詳細資訊，請參閱 GattLocalCharacteristicParameters.UserDescription 屬性。
3. 特性格式 (一個指定的簡報格式會有一個描述元)。  如需詳細資訊，請參閱 GattLocalCharacteristicParameters.PresentationFormats 屬性。
4. 特性彙總格式 (如果指定多個簡報格式)。  如需詳細資訊，請參閱 GattLocalCharacteristicParameters.See PresentationFormats 屬性。
5. 特性擴充屬性 (如果特性標示擴充的屬性位元)。

> 透過 ReliableWrites 和 WritableAuxiliaries 特性屬性判斷擴充屬性描述元的值。

> 嘗試建立保留描述元，會導致例外狀況。

> 請注意，目前不支援廣播。  指定廣播 GattCharacteristicProperty，將會導致例外狀況。

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>服務和特性的階層中往上建置
GattServiceProvider 是用來建立和廣告根主要服務定義。  每個服務都需要其在 GUID 中接受的專屬 ServiceProvider 物件︰ 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 主要服務是 GATT 樹狀結構的最上層。 主要服務包含特性，以及其他服務 (稱為「已包含」或次要服務)。 

現在，將必要特性和描述元填入服務中︰

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
如上所述，這也是針對每個特性所支援的作業宣告事件處理常式的不錯位置。  為了正確地回應要求，應用程式必須定義和設定屬性所支援之每個要求類型的事件處理常式。  無法登錄處理常式時，將會導致系統使用 *UnlikelyError* 立即完成要求。

### <a name="constant-characteristics"></a>常數特性
有時候，有些特性值不會在應用程式存留期間變更。 在該情況下，建議宣告常數特性，避免不必要的應用程式啟用︰ 

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
## <a name="publish-the-service"></a>發行服務
完全定義服務之後，下一步就是發行服務支援。 這會通知作業系統，應該在遠端裝置執行服務探索時傳回服務。  您需要設定兩個屬性 - IsDiscoverable 和 IsConnectable：  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **Feedviewerbasic IsDiscoverable**:通告公告，讓裝置變成可搜尋中的遠端裝置的易記名稱。
- **IsConnectable**:通告週邊設備的角色中使用可連接的公告。

> 服務為 Discoverable 和 Connectable 時，系統會將服務 Uuid 新增至廣告封包。  廣告封包中只有 31 個位元組，而 128 位元 UUID 佔用其中的 16 個！

> 請注意，如果在前景發行服務，則應用程式必須在暫停時呼叫 StopAdvertising。

## <a name="respond-to-read-and-write-requests"></a>回應讀取和寫入要求
如上所述，宣告必要特性時，GattLocalCharacteristics 有 3 種類型的事件-ReadRequested、WriteRequested 和 SubscribedClientsChanged。

### <a name="read"></a>Read
當遠端裝置嘗試從特性讀取值 (且不是常數值) 時，會呼叫 ReadRequested 事件。 在其上呼叫讀取的特性以及引數 (包含遠端裝置的相關資訊) 會傳遞給委派︰ 

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
當遠端裝置嘗試將值寫入特性時，會使用遠端裝置、要寫入的特性和值本身的詳細資料來呼叫 WriteRequested 事件︰ 

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
有 2 種類型的「寫入」- 有回應和無回應。 使用 GattWriteOption (GattWriteRequest 物件的屬性) 找出遠端裝置所要執行的寫入類型。 

## <a name="send-notifications-to-subscribed-clients"></a>將通知傳送給訂閱的用戶端
通知是最常見的 GATT 伺服器作業，可執行將資料推送至遠端裝置的重大功能。 有時候，您會想要通知所有訂閱的用戶端，但有時候，您可能會想要選擇接收新值的裝置︰ 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

當新的裝置訂閱通知時，會呼叫 SubscribedClientsChanged 事件︰ 

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
> 請注意，應用程式可以使用 MaxNotificationSize 屬性取得特定用戶端的最大通知大小。  系統會截斷任何大於大小上限的資料。
