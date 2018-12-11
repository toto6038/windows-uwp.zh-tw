---
title: 藍牙 GATT 伺服器
description: 本文提供針對通用 Windows 平台 (UWP) 應用程式，以及針對常見使用案例的範例程式碼的藍牙泛型屬性設定檔 (GATT) 伺服器的概觀。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 551f8b925ffd56950ba893da7b81fefb4579f558
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8930719"
---
# <a name="bluetooth-gatt-server"></a>藍牙 GATT 伺服器


**重要 API**
- [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
- [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)


這篇文章示範適用於通用 Windows 平台 (UWP) app，常見的 GATT 伺服器工作的範例程式碼的藍牙泛型屬性 (GATT) Server Api: 
- 定義支援的服務
- 發佈伺服器，因此它可以藉由遠端用戶端
- 通告服務的支援
- 讀取和寫入要求回應
- 將通知傳送到訂閱的用戶端

## <a name="overview"></a>概觀
Windows 通常會以用戶端角色運作。 不過，許多案例發生，這樣做需要以做為藍牙 LE GATT 伺服器以及 Windows。 適用於 IoT 裝置，以及大部分的跨平台 BLE 通訊幾乎所有的案例需要 Windows GATT 伺服器。 此外，將通知傳送到附近穿戴式裝置的裝置已成為需要這項技術的常見案例。  
> 請確定[GATT 用戶端文件](gatt-client.md)中的所有概念都都會清除再繼續。  

伺服器作業將會為中心服務提供者和 GattLocalCharacteristic。 這兩個類別都可提供宣告、 實作和公開到遠端裝置的資料階層所需的功能。

## <a name="define-the-supported-services"></a>定義支援的服務
您的應用程式可能會宣告要發佈的 Windows 的一或多個服務。 每個服務是以 UUID 唯一識別。 

### <a name="attributes-and-uuids"></a>屬性和 Uuid
每個服務、 特性和描述元定義自己的唯一 128 位元 UUID 是透過。
> 所有 Windows Api 都使用一詞的 GUID，但藍芽標準定義這些可依 Uuid。 目的，因此我們將會繼續使用長期 UUID，是互換這些兩個詞彙。 

如果屬性是標準和藍牙 SIG 定義，由定義，它也會有一個對應的 16 位元簡短的識別碼 (例如電池層級 UUID 已 0000**2A19**-0000-1000年-8000-00805F9B34FB 和簡短的識別碼是 0x2A19)。 這些標準 Uuid 可以看到[GattServiceUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattserviceuuids.aspx)和[GattCharacteristicUuids](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.genericattributeprofile.gattcharacteristicuuids.aspx)中。

如果您的應用程式實作它是自己自訂的服務，自訂 UUID 會產生。 這是輕鬆地完成在 Visual Studio 中透過工具-> CreateGuid （使用選項 5 來取得它，「 儲存-xxxx-...xxxx 」 格式）。 這個 uuid 現在可用來宣告新的本機服務、 特性或描述元。

#### <a name="restricted-services"></a>受限制的服務
下列服務由系統保留，並在這個階段無法發行：
1. 裝置資訊服務 (DIS)
2. 泛型屬性設定檔服務 (GATT)
3. 一般的存取權設定檔服務 （間隔）
4. 人性化介面裝置服務 (HOGP)
5. 掃描參數服務 (SCP)

> 嘗試建立已封鎖的服務，將導致 BluetoothError.DisabledByPolicy 從 CreateAsync 呼叫傳回。

#### <a name="generated-attributes"></a>產生的屬性
下列的描述元是特性的由系統，根據所提供建立期間 GattLocalCharacteristicParameters 自動產生：
1. 用戶端的特性設定 （如果特性會標示為 indicatable 或 notifiable）。
2. 特性使用者說明 （如果已設定 UserDescription 屬性）。 請參閱 GattLocalCharacteristicParameters.UserDescription 屬性，如需詳細資訊。
3. 特性格式 （一個描述元指定每個簡報格式）。  請參閱 GattLocalCharacteristicParameters.PresentationFormats 屬性，如需詳細資訊。
4. 特性彙總格式 （如果未指定多個簡報格式）。  如需詳細資訊的 GattLocalCharacteristicParameters.See PresentationFormats 屬性。
5. 特性延伸屬性 （如果特性標記延伸的屬性位元）。

> 擴充屬性描述元的值是透過 ReliableWrites 和 WritableAuxiliaries 特性屬性決定。

> 嘗試建立保留描述元，將會導致例外狀況。

> 在此階段不支援廣播的注意。  指定廣播 GattCharacteristicProperty 將導致例外狀況。

### <a name="build-up-the-hierarchy-of-services-and-characteristics"></a>建置服務與特性的階層
GattServiceProvider 用來建立及通告根主要服務定義。  每個服務需要它是在 GUID 中採用自己 ServiceProvider 物件： 

```csharp
GattServiceProviderResult result = await GattServiceProvider.CreateAsync(uuid);

if (result.Error == BluetoothError.Success)
{
    serviceProvider = result.ServiceProvider;
    // 
}
```
> 主要服務是最上層的 GATT 樹狀結構。 主要服務包含特性，以及其他服務 （稱為 「 包含 」 或次要服務）。 

現在，填入必要的特性和描述元服務：

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
如以上所示，這也是適合用來宣告每個特性支援的作業的事件處理常式。  若要正確地回應的要求，應用程式必須定義並設定每個要求類型屬性支援的事件處理常式。  失敗的登錄處理常式，將導致立即使用*UnlikelyError*完成，系統會要求。

### <a name="constant-characteristics"></a>常數的特性
有時會有應用程式的存留期的過程中不會變更的特性值。 在此情況下，建議您宣告的常數的特性，以避免不必要的應用程式啟用： 

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
一旦完整定義服務下, 一個步驟是發佈支援服務。 這會通知 OS 當遠端裝置執行服務探索時，會傳回該服務。  您將會設定兩個屬性-IsDiscoverable 和 IsConnectable:  

```csharp
GattServiceProviderAdvertisingParameters advParameters = new GattServiceProviderAdvertisingParameters
{
    IsDiscoverable = true,
    IsConnectable = true
};
serviceProvider.StartAdvertising(advParameters);
```
- **IsDiscoverable**： 通告到遠端裝置的廣告，讓裝置可搜尋的易記名稱。
- **IsConnectable**： 公告可連接的廣告，以用於周邊角色。

> 服務時可搜尋和 Connectable，則系統會將服務 Uuid 新增廣告封包。  廣告封包中有只有 31 位元組和 128 位元 UUID 佔用 16 它們 ！

> 請注意，服務發行時在前景中，應用程式必須呼叫 StopAdvertising 應用程式暫停時。

## <a name="respond-to-read-and-write-requests"></a>讀取和寫入要求回應
如我們所看到的上方時宣告的必要的特性，GattLocalCharacteristics 都有 3 種類型的事件-ReadRequested、 WriteRequested 和 SubscribedClientsChanged。

### <a name="read"></a>讀取
當遠端裝置會嘗試讀取特性值 （和它不是常數的值） 時，會呼叫 ReadRequested 事件。 讀取已呼叫以及引數 （包含在遠端裝置的相關資訊） 的特性會傳遞至委派： 

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

### <a name="write"></a>撰寫
當遠端裝置會嘗試將值寫入特性時，WriteRequested 事件會呼叫含有在遠端裝置相關詳細資料寫入至哪些特性和本身的值： 

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
有 2 種寫入-含/不含回應。 您可以使用 GattWriteOption （GattWriteRequest 物件上的屬性） 來找出遠端裝置正在執行哪種類型的寫入。 

## <a name="send-notifications-to-subscribed-clients"></a>將通知傳送到訂閱的用戶端
最常見的 GATT 伺服器作業，通知執行重要的函式，將資料推送到遠端裝置。 有時候，您會想要通知所有訂閱的用戶端，但您可能想要挑選哪些裝置来傳送到新的值 othertimes: 

```csharp
async void NotifyValue()
{
    var writer = new DataWriter();
    // Populate writer with data
    // ...
    
    await notifyCharacteristic.NotifyValueAsync(writer.DetachBuffer());
}
```

當新的裝置訂閱的通知時，取得呼叫 SubscribedClientsChanged 事件： 

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
> 請注意應用程式可以針對特定的用戶端與 MaxNotificationSize 屬性要取得最大通知大小。  系統會截斷大於最大大小的任何資料。
