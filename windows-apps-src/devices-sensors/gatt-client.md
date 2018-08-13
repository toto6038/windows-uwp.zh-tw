---
author: msatranjr
title: Bluetooth GATT 用戶端
description: 本文概述 Bluetooth 泛用屬性的設定檔 (GATT) 用戶端對於萬用 Windows 平台 (UWP) 應用程式，以及一般使用案例的範例程式碼。
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 555fec6d534c07898acd911f9cd84a11ac66dcd8
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "304664"
---
# <a name="bluetooth-gatt-client"></a>Bluetooth GATT 用戶端


**重要 API**

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

本文示範如何使用通用 Windows 平台 (UWP) 應用程式，以及一般 GATT 用戶端工作的範例程式碼的 Bluetooth 一般屬性 (GATT) 用戶端 api （英文）：
- 附近裝置的查詢
- 連線至裝置
- 列舉支援的服務和裝置的特性
- 讀取及寫入特性
- 訂閱的通知時特性值的變更

## <a name="overview"></a>概觀
開發人員可以[**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)命名空間中使用 Api 來存取 Bluetooth LE 裝置。 藍牙 LE 裝置透過下列一組項目公開功能：

-   服務
-   特性
-   描述元

服務定義 LE 裝置的正常運作的合約，包含一組定義服務的特性。 這些特性依序包含描述特性的描述元。 將這些 3 字詞代稱為裝置的屬性。

Bluetooth LE GATT Api 公開物件和函數，而不是原始傳輸的存取。 GATT Api 也讓開發人員可使用 Bluetooth LE 裝置與能夠執行下列工作：

-   執行屬性探索
-   讀取和寫入屬性值
-   註冊回呼特性 ValueChanged 事件

若要建立有用的實作開發人員必須先備知識 GATT 服務及應用程式要使用的特性和程序特定特性值如此 API 所提供的二進位資料轉換成之前要向使用者顯示的有用資料。 藍牙 GATT API 只會公開與藍牙 LE 裝置通訊所需的基本基元。 為解譯資料，必須定義應用程式設定檔 (無論是透過藍牙 SIG 標準設定檔，或是裝置供應商實作的自訂設定檔)。 設定檔會在應用程式與裝置之間建立繫結協定，內容是關於交換的資料代表的意義以及解譯資料的方式。

為方便起見，藍牙 SIG 會保持一份可用的[公用設定檔清單](https://www.bluetooth.com/specifications/adopted-specifications#gattspec)。

## <a name="query-for-nearby-devices"></a>附近裝置的查詢
以下兩種主要方法鄰近裝置的查詢：
- 在 Windows.Devices.Enumeration DeviceWatcher
- 在 Windows.Devices.Bluetooth.Advertisement AdvertisementWatcher

討論 2nd 方法旨在[通告](ble-beacon.md)文件中讓它不會討論太多這裡但的基本概念是要找出滿足特定[通告篩選](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.advertisementfilter.aspx)的附近裝置的 Bluetooth 地址。 一旦您有地址，您可以呼叫[BluetoothLEDevice.FromBluetoothAddressAsync](https://msdn.microsoft.com/en-us/library/windows/apps/mt608819.aspx)若要取得之裝置的參照。 

現在，回 DeviceWatcher 方法。 Bluetooth LE 裝置就如同在 Windows 中的任何其他裝置，並可以使用[列舉 api （英文)](https://msdn.microsoft.com/library/windows/apps/BR225459)查詢。 使用[DeviceWatcher](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher)類別並傳遞查詢字串，指定要尋找的裝置。 

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
一旦您已啟動 DeviceWatcher，您會收到[DeviceInformation](https://msdn.microsoft.com/library/windows/apps/br225393)每個裝置滿足上述裝置[已新增](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.devicewatcher.added)事件處理常式中的查詢。 如需更多詳細深入 DeviceWatcher 查看完整的[Github](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceEnumerationAndPairing)的範例。 

## <a name="connecting-to-the-device"></a>連線至裝置
一旦發現所需的裝置時，使用[DeviceInformation.Id](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.enumeration.deviceinformation.id)來取得 Bluetooth LE 裝置物件的裝置有問題： 

```csharp
async void ConnectDevice(DeviceInformation deviceInfo)
{
    // Note: BluetoothLEDevice.FromIdAsync must be called from a UI thread because it may prompt for consent.
    BluetoothLEDevice bluetoothLeDevice = await BluetoothLEDevice.FromIdAsync(deviceInfo.Id);
    // ...
}
```
另一方面，處置 BluetoothLEDevice 的所有參考裝置的物件 （以及在系統上的任何其他應用程式是否具有裝置參照） 會觸發自動小逾時期間之後中斷連線。 

```csharp
bluetoothLeDevice.Dispose();
```
如果應用程式需要再次存取裝置，只要重新建立裝置物件並存取特性 （在下一節中討論） 就會觸發重新連線時所需作業系統。 如果鄰近裝置，您將取得的裝置的存取權否則它會傳回具有 DeviceUnreachable 錯誤。  

## <a name="enumerating-supported-services-and-characteristics"></a>支援的服務和特性列舉
現在您有 BluetoothLEDevice 物件下, 一步就是探索何種裝置會公開的資料。 為達成此目的第一個步驟是查詢服務： 

```csharp
GattDeviceServicesResult result = await bluetoothLeDevice.GetGattServicesAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var services = result.Services;
    // ...
}
```
已識別感興趣的服務，下一步是以查詢的特性。 

```csharp
GattCharacteristicsResult result = await service.GetCharacteristicsAsync();
                
if (result.Status == GattCommunicationStatus.Success)
{
    var characteristics = result.Characteristics;
    // ...
}
```  
OS 會傳回 GattCharacteristic ReadOnly 清單物件您可以再上執行作業。

## <a name="perform-readwrite-operations-on-a-characteristic"></a>執行讀取/寫入作業對特性

特性為基本單位 GATT 架構通訊。 包含代表不同裝置上的資料的一部分的值。 例如，電池層級特性具有代表裝置的電池層級的值。

讀取特性屬性來判斷支援何種作業：
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

如果支援讀取，您可以閱讀值： 
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
寫入特性遵循類似的模式： 
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
> **提示**： 取得自信能使用[DataReader](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datareader.aspx)和[DataWriter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.streams.datawriter.aspx)。 當使用您要從許多 Bluetooth Api 取得原始緩衝區都會不可或缺其功能。 
## <a name="subscribing-for-notifications"></a>訂閱的通知

請確定此特性支援指出或通知 （檢查特性屬性來確定）。 

> **預留**： 指出因為每個值變更事件搭配用戶端裝置認可會視為更可靠。 通知會更普遍因為大部分 GATT 交易嗎而節省電源而不是極可靠。 在任何情況下的所有的處理方式控制器層級讓應用程式不會不會取得相關。 我們將分別表示參照其為只是 「 通知 」，但現在您知道。 

有兩個之前取得通知需要注意的事項：
- 寫入至用戶端特性設定描述元 (CCCD)
- 處理 Characteristic.ValueChanged 事件

寫入 CCCD 會告知伺服器裝置此用戶端想要知道每次該特定的特性值有所變更。 做法如下： 

```csharp
GattCommunicationStatus status = await selectedCharacteristic.WriteClientCharacteristicConfigurationDescriptorAsync(
                        GattClientCharacteristicConfigurationDescriptorValue.Notify);
if(status == GattCommunicationStatus.Success)
{
    // Server has been informed of clients interest.
}
```
現在，GattCharacteristic ValueChanged 事件將會取得每次呼叫此值會取得變更遠端裝置上。 所有會保持為實作處理常式： 

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


