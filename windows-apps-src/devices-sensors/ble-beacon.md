---
author: msatranjr
title: "藍牙廣告"
description: "本節包含如何透過 AdvertisementWatcher 和 AdvertisementPublisher API 的使用者，將藍牙低功耗 (LE) 廣告整合到通用 Windows 平台 (UWP) 應用程式的文章。"
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: ff10bbc0-03a7-492c-b5fe-c5b9ce8ca32e
ms.openlocfilehash: dbd68ceb310a53932108291cad3a33ea944b4d08
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="bluetooth-le-advertisements"></a>藍牙 LE 廣告

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

這篇文章提供適用於通用 Windows 平台 (UWP) 應用程式的藍牙低功耗 (LE) 廣告指標概觀。  

## <a name="overview"></a>概觀

有兩個開發人員可以使用 LE Advertisement API 執行的主要功能：

-   [Advertisement Watcher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx)：接聽附近的指標並根據承載或鄰近性篩選出指標。  
-   [Advertisement Publisher](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx)：定義 Windows 承載，以代表開發人員宣傳。  

在 Github 上的[藍牙廣告範例](http://go.microsoft.com/fwlink/p/?LinkId=619990)中可以找到完整的範例程式碼

## <a name="basic-setup"></a>基本設定

若要在通用 Windows 平台 App 中使用基本的藍牙 LE 功能，您必須在 Package.appxmanifest 中選取藍牙功能。

1. 開啟 \[Package.appxmanifest\]
2. 移至 \[功能\] 索引標籤
3. 在左側清單中尋找 [藍牙]，並選取它旁邊的方塊。

## <a name="publishing-advertisements"></a>發佈廣告

藍牙 LE 廣告可讓您的裝置持續發出特定承載的指標 (稱為「廣告」)。 附近任何支援藍牙 LE 功能的裝置，如果有設定為接聽此特定廣告，就都能看見此廣告。

**注意：**基於使用者隱私權的理由，廣告的存留時間是與您 App 的存留時間繫結。 您可以建立 BluetoothLEAdvertisementPublisher 並且針對在背景的廣告於背景作業中呼叫 Start。 如需背景作業的詳細資訊，請參閱[啟動、繼續和背景工作](https://msdn.microsoft.com/windows/uwp/launch-resume/index)。

### <a name="basic-publishing"></a>基本發佈

將資料加入「廣告」的方法有很多種。 此範例示範建立公司專屬廣告的常見方法。 

首先，建立廣告發佈程式 (Advertisement Publisher)，以控制裝置是否發出特定廣告指標。

```csharp
BluetoothLEAdvertisementPublisher publisher = new BluetoothLEAdvertisementPublisher();
```

接著，建立自訂資料區段。 這個範例使用未指派的 **CompanyId** 值 *0xFFFE* 並且將 *Hello World* 文字加入廣告。 

```csharp
// Add custom data to the advertisement
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

var writer = new DataWriter();
writer.WriteString("Hello World");

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
manufacturerData.Data = writer.DetachBuffer();

// Add the manufacturer data to the advertisement publisher:
publisher.Advertisement.ManufacturerData.Add(manufacturerData);
```

建立並設定發佈程式之後，您可以呼叫 **Start** 來開始發出廣告。

```csharp
publisher.Start();
```

## <a name="watching-for-advertisements"></a>監看廣告

### <a name="basic-watching"></a>基本監看

以下程式碼示範如何建立藍牙 LE 廣告監看程式、設定回呼，以及監看所有 LE 廣告。

```csharp
BluetoothLEAdvertisementWatcher watcher = new BluetoothLEAdvertisementWatcher();
watcher.Received += OnAdvertisementReceived;
watcher.Start();
```    

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // Do whatever you want with the advertisement
}
```

#### <a name="active-scanning"></a>主動掃描
若同時也要接收掃描回應廣告，則在建立監看程式之後進行以下設定。 注意，此設定會造成消耗較多的電量，且無法於背景模式中使用。

```csharp
watcher.ScanningMode = BluetoothLEScanningMode.Active;
```

### <a name="watching-for-a-specific-advertisement-pattern"></a>監看特定廣告模式

有時候您會想要接聽特定的廣告。 在此案例中，接聽的廣告所含的承載包含虛構公司 (識別為 0xFFFE)，且廣告中包含 *Hello World* 字串。 這可以與「基本發佈」範例搭配，讓一部 Windows 電腦進行廣告，另一部則進行監看。 請務必在啟動監看程式之前設定此廣告篩選條件！

```csharp
var manufacturerData = new BluetoothLEManufacturerData();
manufacturerData.CompanyId = 0xFFFE;

// Make sure that the buffer length can fit within an advertisement payload (~20 bytes). 
// Otherwise you will get an exception.
var writer = new DataWriter();
writer.WriteString("Hello World");
manufacturerData.Data = writer.DetachBuffer();

watcher.AdvertisementFilter.Advertisement.ManufacturerData.Add(manufacturerData);
```

### <a name="watching-for-a-nearby-advertisement"></a>監看附近的廣告

有時候您只想要在進入裝置發出廣告的範圍時才觸發監看程式。 您可以定義自己的範圍，但請注意該值會縮減至 0 和 -128 之間。 

```csharp
// Set the in-range threshold to -70dBm. This means advertisements with RSSI >= -70dBm 
// will start to be considered "in-range" (callbacks will start in this range).
watcher.SignalStrengthFilter.InRangeThresholdInDBm = -70;

// Set the out-of-range threshold to -75dBm (give some buffer). Used in conjunction 
// with OutOfRangeTimeout to determine when an advertisement is no longer 
// considered "in-range".
watcher.SignalStrengthFilter.OutOfRangeThresholdInDBm = -75;

// Set the out-of-range timeout to be 2 seconds. Used in conjunction with 
// OutOfRangeThresholdInDBm to determine when an advertisement is no longer 
// considered "in-range"
watcher.SignalStrengthFilter.OutOfRangeTimeout = TimeSpan.FromMilliseconds(2000);
```

### <a name="gauging-distance"></a>測量距離

當藍牙 LE 監看程式的回呼被觸發時，包含 RSSI 值的 eventArgs 會告訴您收到的訊號強度 (藍牙訊號的強度)。

```csharp
private async void OnAdvertisementReceived(BluetoothLEAdvertisementWatcher watcher, BluetoothLEAdvertisementReceivedEventArgs eventArgs)
{
    // The received signal strength indicator (RSSI)
    Int16 rssi = eventArgs.RawSignalStrengthInDBm;
}
```

這個值可以約略轉譯成距離，但不應該用來測量實際距離，因為每個獨立無線電波都不同。 不同的環境因素可能使距離難以測量 (如牆壁、無線電波周圍的外殼，或甚至是空氣濕度)。

另一個用來判斷單純距離的方法是定義「值區」。 無線電波在非常接近的時候會回報 0 至 -50 DBm，在中等距離的時候為 -50 至 -90，很遠的時候則低於 -90。 若要決定適用於您 App 的值區，嘗試錯誤是最佳的方法。