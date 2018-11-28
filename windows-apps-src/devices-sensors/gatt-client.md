---
title: 藍牙 GATT 用戶端
description: 本文提供針對通用 Windows 平台 (UWP) 應用程式，以及針對常見使用案例的範例程式碼的藍牙泛型屬性設定檔 (GATT) 用戶端的概觀。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3ae656b473a4dd5999588057b0ec970645703eec
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7842423"
---
# <a name="bluetooth-gatt-client"></a>藍牙 GATT 用戶端


**重要 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

這篇文章示範適用於通用 Windows 平台 (UWP) app，常見的 GATT 用戶端工作的範例程式碼的藍牙泛型屬性 (GATT) 用戶端 api 的使用方式：
- 查詢附近的裝置
- 連線到裝置
- 列舉支援的服務及裝置的特性
- 讀取和寫入特性
- 訂閱的通知時特性值的變更

## <a name="overview"></a>概觀
開發人員可以使用[**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)命名空間中的 Api 來存取藍牙 LE 裝置。 藍牙 LE 裝置透過下列一組項目公開功能：

-   服務
-   特性
-   描述元

服務定義 LE 裝置的功能協定，並且包含一組定義服務的特性。 這些特性依序包含描述特性的描述元。 這些 3 個詞彙代稱為裝置的屬性。

藍牙 LE GATT Api 會公開物件與函式，，而不是原始傳輸的存取。 GATT Api 也讓開發人員得以使用藍牙 LE 裝置能夠執行下列工作：

-   執行屬性探索
-   讀取和寫入屬性值
-   登錄 Characteristic ValueChanged 事件的回呼

若要建立有用的實作開發人員必須有背景知識的 GATT 服務與應用程式將以取用的情況下的特性和處理特定的特性值，這類 API 所提供的二進位資料會先轉換成有用的資料後再提供給使用者。 藍牙 GATT API 只會公開與藍牙 LE 裝置通訊所需的基本基元。 為解譯資料，必須定義應用程式設定檔 (無論是透過藍牙 SIG 標準設定檔，或是裝置供應商實作的自訂設定檔)。 設定檔會在應用程式與裝置之間建立繫結協定，內容是關於交換的資料代表的意義以及解譯資料的方式。

為方便起見，藍牙 SIG 會保持一份可用的[公用設定檔清單](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)。

## <a name="query-for-nearby-devices"></a>查詢附近的裝置
有兩種主要的方法來查詢附近的裝置：
- DeviceWatcher Windows.Devices.Enumeration 中
- 在 Windows.Devices.Bluetooth.Advertisement AdvertisementWatcher

第 2 個方法討論旨在[廣告](ble-beacon.md)文件中讓它將不會討論的基本概念，但更這裡會找到附近的裝置滿足特定[廣告篩選條件](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)的藍芽位址。 一旦您有地址，您可以呼叫[BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx)來取得裝置的參考。 

現在，回到 DeviceWatcher 方法。 藍牙 LE 裝置，就如同在 Windows 中的任何其他裝置，並可使用[列舉 Api](https://msdn.microsoft.com/library/windows/apps/BR225459)查詢。 使用[DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher)類別，並傳遞查詢字串，指定要尋找的裝置： 

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
一旦您已經開始 DeviceWatcher，您會收到[DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393)滿足有問題的裝置[新增](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added)事件處理常式中的查詢的每個裝置。 如需更詳細查看 DeviceWatcher 查看完整[Github 上](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)的範例。 

## <a name="connecting-to-the-device"></a>連線到裝置
一旦探索所需的裝置，使用[DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id)取得裝置的藍牙 LE 裝置物件有問題： 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
相反地，處置 BluetoothLEDevice 的所有參照裝置的物件 （和系統上的沒有其他應用程式如果有裝置的參照） 將會觸發自動在小型的逾時期間之後中斷連線。 

```csharp
bluetoothLeDevice.Dispose();
```
如果應用程式需要再次存取裝置，只要重新建立裝置物件，並存取特性 （下一節中討論） 將會觸發重新連線必要時 OS。 如果裝置在附近，您將取得的裝置的存取權否則它將會傳回與 DeviceUnreachable 錯誤。  

## <a name="enumerating-supported-services-and-characteristics"></a>列舉支援的服務與特性
現在，您已經有 BluetoothLEDevice 物件下, 一個步驟是探索裝置會公開哪些資料。 若要這樣做的第一個步驟是要查詢的服務： 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
識別感興趣的服務之後, 的下一個步驟是查詢的特性。 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
作業系統會傳回 GattCharacteristic 的唯讀清單物件，然後您可以執行的作業上。

## <a name="perform-readwrite-operations-on-a-characteristic"></a>讀/寫上執行操作特性

特性是 GATT 的基本單位為基礎的通訊。 它包含值，表示裝置上的資料的不同部分。 例如，電池層級特性都有值，代表裝置的電池電量。

讀取特性的屬性，以判斷支援哪些作業：
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

如果支援讀取，您可以讀取值： 
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
寫入特性，按照類似的模式： 
```csharp
var writer = new DataWriter();
// WriteByte used for simplicity. Other commmon functions - WriteInt16 and WriteSingle
writer.WriteByte(0x01);

GattReadResult result = await selectedCharacteristic.WriteValueAsync(writer.DetachBuffer());
if (result.Status == GattCommunicationStatus.Success)
{
    // Successfully wrote to device
}
```
> **提示**： 取得熟悉使用[DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx)和[DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)。 使用您從許多藍牙 Api 取得的原始緩衝區時，其功能將會不可或缺。 
## <a name="subscribing-for-notifications"></a>訂閱的通知

請確定特性支援指出或通知 （檢查以確定特性的屬性）。 

> **預留**： 表示可視為更可靠，因為每個值變更事件搭配從用戶端裝置的通知。 通知是更普遍，因為大部分的 GATT 交易會而節省電源，而非非常可靠。 在任何情況下，所有的會處理在控制器層級所以應用程式不會不會取得涉及。 我們將通稱為它們只是 「 通知 」，但現在您知道。 

有負責處理的通知之前的兩件事：
- 寫入至用戶端特性組態描述元 (CCCD)
- 處理 Characteristic.ValueChanged 事件

CCCD 寫入會告訴伺服器裝置，此用戶端想要知道每次該特定的特性值的變更。 做法如下： 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
現在，GattCharacteristic ValueChanged 事件將會取得每次呼叫取得遠端裝置上變更的值。 剩下的就是實作處理常式： 

```csharp
characteristic.ValueChanged += Characteristic_ValueChanged;
// ... 

void Characteristic_ValueChanged(GattCharacteristic sender, 
                                    GattValueChangedEventArgs args)
{
    // An Indicate or Notify reported that the value has changed.
    var reader = DataReader.FromBuffer(args.CharacteristicValue)
    // Parse the data however required.
}
```


