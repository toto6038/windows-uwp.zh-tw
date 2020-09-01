---
title: 藍牙 GATT 用戶端
description: 本文將概略說明適用於通用 Windows 平台 (UWP) app 的藍牙泛型屬性設定檔 (GATT) 用戶端，並提供常見使用案例的範例程式碼。
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd5f2b76af856dd66e2dfd0ee2b3e429199e6a19
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172282"
---
# <a name="bluetooth-gatt-client"></a>藍牙 GATT 用戶端

本文示範如何使用適用於通用 Windows 平台 (UWP) app 的藍牙泛型屬性 (GATT) 用戶端 API，以及常見 GATT 用戶端工作的範例程式碼︰

- 查詢附近的裝置
- 連接到裝置
- 列舉裝置所支援的服務和特性
- 讀取和寫入特性
- 訂閱特性值變更時的通知

> [!Important]
> 您必須在 *package.appxmanifest*中宣告「藍牙」功能。
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

> **重要 API**
>
> - [**Windows.Devices.Bluetooth**](/uwp/api/Windows.Devices.Bluetooth)
> - [**GenericAttributeProfile。**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile)

## <a name="overview"></a>概觀

開發人員可以使用 [**Windows.Devices.Bluetooth.GenericAttributeProfile**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) 命名空間中的 API 來存取藍牙 LE 裝置。 藍牙 LE 裝置透過下列一組項目公開功能：

- 服務
- 特性
- 描述項

服務定義 LE 裝置的功能協定，並且包含一組定義服務的特性。 這些特性依序包含描述特性的描述元。 這 3 個術語一般稱為裝置的屬性。

藍牙 LE GATT API 會公開物件與函式，而非原始傳輸的存取。 GATT API 也讓開發人員得以使用藍牙 LE 裝置以執行下列工作：

- 執行屬性探索
- 讀取和寫入屬性值
- 登錄 Characteristic ValueChanged 事件的回呼

為建立有用的實作和處理特定的特性值 (這樣 API 提供的二進位資料才會先轉換成有用的資料後再提供給使用者)，開發人員首先必須具備應用程式要使用之 GATT 服務與特性的知識。 藍牙 GATT API 只會公開與藍牙 LE 裝置通訊所需的基本基元。 為解譯資料，必須定義應用程式設定檔 (無論是透過藍牙 SIG 標準設定檔，或是裝置供應商實作的自訂設定檔)。 設定檔會在應用程式與裝置之間建立繫結協定，內容是關於交換的資料代表的意義以及解譯資料的方式。

為方便起見，藍牙 SIG 會保持一份可用的[公用設定檔清單](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)。

## <a name="query-for-nearby-devices"></a>查詢附近的裝置

有兩種主要方法可查詢附近的裝置︰

- DeviceWatcher in Windows.Devices.Enumeration
- Windows.Devices.Bluetooth.Advertisement 中的 AdvertisementWatcher

第 2 種方法會在[廣告](ble-beacon.md)文件中深入討論，因此在這裡不會討論太多，但基本概念是尋找附近裝置中符合特定[廣告篩選](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter)(英文) 的藍牙位址。 有了這個位址之後，您就可以呼叫[BluetoothLEDevice.FromBluetoothAddressAsync](/uwp/api/windows.devices.bluetooth.bluetoothledevice.frombluetoothaddressasync)來取得裝置的參考。

現在，回到 DeviceWatcher 方法。 藍牙 LE 裝置就像 Windows 中的任何其他裝置一樣，而且可以使用[列舉 API](/uwp/api/Windows.Devices.Enumeration) 進行查詢。 使用[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher)類別，並傳遞查詢字串，以指定要尋找的裝置︰

```csharp
// Query for extra properties you want returned
string[] requestedProperties = { "System.Devices.Aep.DeviceAddress", "System.Devices.Aep.IsConnected" };

DeviceWatcher deviceWatcher =
            DeviceInformation.CreateWatcher(
                    BluetoothLEDevice.GetDeviceSelectorFromPairingState(false),
                    requestedProperties,
                    DeviceInformationKind.AssociationEndpoint);

// Register event handlers before starting the watcher.
// Added, Updated and Removed are required to get all nearby devices
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Updated += DeviceWatcher_Updated;
deviceWatcher.Removed += DeviceWatcher_Removed;

// EnumerationCompleted and Stopped are optional to implement.
deviceWatcher.EnumerationCompleted += DeviceWatcher_EnumerationCompleted;
deviceWatcher.Stopped += DeviceWatcher_Stopped;

// Start the watcher.
deviceWatcher.Start();
```

啟動 DeviceWatcher 之後，就會收到每個符合查詢之裝置的[DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)，而查詢位於有問題裝置中 [Added](/uwp/api/windows.devices.enumeration.devicewatcher.added)事件的處理常式。 若要更詳細地查看 DeviceWatcher，請參閱 [Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing) 上的完整範例。

## <a name="connecting-to-the-device"></a>連接至裝置

探索到想要的裝置之後，請使用[DeviceInformation.Id](/uwp/api/windows.devices.enumeration.deviceinformation.id)取得有問題裝置的藍牙 LE 裝置物件︰

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```

另一方面，處置裝置之 BluetoothLEDevice 物件的所有參考 (而且，如果系統上沒有其他應用程式具有裝置的參考) 都會在一小段逾時期間之後觸發自動中斷連接。

```csharp
bluetoothLeDevice.Dispose();
```

如果應用程式需要再次存取裝置，則只有重新建立裝置物件以及存取特性 (在下一節討論) 才會觸發作業系統在必要時重新連接。 如果裝置在附近，您會取得裝置的存取權，否則會傳回 DeviceUnreachable 錯誤。  

## <a name="enumerating-supported-services-and-characteristics"></a>列舉支援的服務和特性

既然您已經有 BluetoothLEDevice 物件, 下一步就是找出裝置所公開的資料。 要這樣做的第一個步驟是查詢服務︰

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```

找到感興趣的服務之後, 下一步就是查詢特性。

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();

if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```

作業系統會傳回接著可對其執行作業之 GattCharacteristic 物件的 ReadOnly 清單。

## <a name="perform-readwrite-operations-on-a-characteristic"></a>對特性執行讀取/寫入作業

特性是以 GATT 為基礎之通訊的基本單位。 它包含的值代表裝置上資料的不同部分。 例如，電池電量特性的值代表裝置的電池電量。

讀取特性屬性，以判斷支援的作業︰

```csharp
GattCharacteristicProperties properties = characteristic.CharacteristicProperties

if(properties.HasFlag(GattCharacteristicProperties.Read))
{
    // This characteristic supports reading from it.
}
if(properties.HasFlag(GattCharacteristicProperties.Write))
{
    // This characteristic supports writing to it.
}
if(properties.HasFlag(GattCharacteristicProperties.Notify))
{
    // This characteristic supports subscribing to notifications.
}
```

如果支援讀取，則可以讀取值︰

```csharp
GattReadResult result = await selectedCharacteristic.ReadValueAsync();
if (result.Status == GattCommunicationStatus.Success)
{
    var reader = DataReader.FromBuffer(result.Value);
    byte[] input = new byte[reader.UnconsumedBufferLength];
    reader.ReadBytes(input);
    // Utilize the data as needed
}
```

寫入特性時遵循類似的模式︰

```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other common functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattCommunicationStatus result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```

> **秘訣**：當使用您從許多藍牙 api 取得的原始緩衝區時，會不可或缺 [DataReader](/uwp/api/windows.storage.streams.datareader) 和 [>datawriter](/uwp/api/windows.storage.streams.datawriter) 。

## <a name="subscribing-for-notifications"></a>訂閱通知

請確定特性支援 Indicate 或 Notify (檢查特性屬性予以確定)。

> **此外**︰Indicate 視為更可靠，因為每個值 changed 事件都會搭配來自用戶端裝置的通知。 Notify 較為普遍，因為大部分 GATT 交易都會想要節省電源，而不是額外可靠。 在任何情況下，所有這些作業都是在控制器層級處理，應用程式並不會涉入。 我們將它們通稱為「通知」，而現在您也知道了。

取得通知之前，需要注意兩個事項︰

- 寫入至用戶端特性組態描述元 (CCCD)
- 處理 Characteristic.ValueChanged 事件

寫入至 CCCD 是要告訴「伺服器」裝置，每次特定特性值變更時，這個用戶端都想要知道。 若要這樣做：

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```

現在，每次遠端裝置上的值變更時，都會呼叫 GattCharacteristic ValueChanged 事件。 現在只剩下實作處理常式︰

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;

...

void Characteristic_ValueChanged(GattCharacteristic sender,
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```