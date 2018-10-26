---
author: TerryWarwick
title: 開始使用服務點
description: 本文包含有關如何開始使用服務點 UWP API 的資訊
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: f3959254787ce22bea27495520805485e0ea179b
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5565478"
---
# <a name="getting-started-with-point-of-service"></a>開始使用服務點

服務點、銷售點或服務點裝置是用來協助零售交易的電腦週邊設備。 服務點裝置的範例包括電子收銀機、條碼掃描器、磁條讀取器和收據印表機。

您將從本文了解使用通用 Windows 平台 (UWP) PointOfService API 銜接服務點裝置的基本概念。 我們會討論裝置列舉、檢查裝置功能、宣告裝置和裝置共用。 我們使用條碼掃描器裝置做為範例，但幾乎這裡所有的指引都適用於任何 UWP 相容的服務點裝置。 (如需所支援裝置的清單，請參閱 [服務點裝置支援](pos-device-support.md))。

## <a name="finding-and-connecting-to-point-of-service-peripherals"></a>找出和連接至服務點週邊設備

服務點裝置必須先與執行應用程式的電腦進行配對，應用程式才能使用該裝置。 有幾個方式可以連接至服務點裝置，若不是藉由程式設計的方式，就是透過 [設定] App。

### <a name="connecting-to-devices-by-using-the-settings-app"></a>使用 [設定] App 連接至裝置
條碼掃描器等服務點裝置接上電腦時，該裝置就像任何其他裝置一樣地出現其中。 您可以在 \[設定\] App 的 **\[裝置\] > \[藍牙與其他裝置\]** 區段中找到。 您可以在那裡選取 **\[新增藍牙或其他裝置\]**，與服務點裝置進行配對。

有些服務點裝置必須使用服務點 API，以程式設計方式來列舉，否則這些裝置無法在 [設定] App 中出現。

### <a name="getting-a-single-point-of-service-device-with-getdefaultasync"></a>使用 GetDefaultAsync 取得單一服務點裝置
在簡單的使用案例中，可能只有一部服務點週邊設備連接至 App 執行所在的電腦，您想要盡快加以設定。 若要這樣做，請使用 **GetDefaultAsync** 方法來擷取預設裝置，如下所示。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

如果找到預設裝置，即可開始宣告所擷取的裝置物件。 「宣告」一部裝置會將其獨佔存取權授與應用程式，防止多個處理程序的命令發生衝突。

> [!NOTE] 
> 如果有多個服務點裝置連接至電腦，**GetDefaultAsync** 會傳回找到的第一個裝置。 因此，除非您確定應用程式只有顯示一個服務點裝置，否則請使用 **FindAllAsync**。

### <a name="enumerating-a-collection-of-devices-with-findallasync"></a>使用 FindAllAsync 列舉裝置集合

連接至多個裝置時，您必須列舉 **PointOfService** 裝置物件的集合，才能找到您要宣告的裝置。 例如，下列程式碼會建立目前連接的所有條碼掃描器的集合，然後搜尋集合中具有特定名稱的掃描器。

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;

string selector = BarcodeScanner.GetDeviceSelector();       
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
    if (devInfo.Name.Contains("1200G"))
    {
        Debug.WriteLine(" Found one");
    }
}
```

### <a name="scoping-the-device-selection"></a>限定裝置選取範圍
連接至裝置時，您可能會想要將搜尋限制在 App 可存取之服務點週邊設備的子集。 您可以使用 **GetDeviceSelector** 方法，將選取範圍限定為僅擷取透過特定方法 (藍牙、USB 等) 連接的裝置。 您可以建立選取器，搜尋透過 **\[藍牙\]**、**\[IP\]**、**\[本機\]** 或 **\[所有連接類型\]** 連接的裝置。 這相當實用，因為相較於本機 (有線) 探索，無線裝置探索需要很長的時間。 您可以將 **FindAllAsync** 限制為 **\[本機\]** 連接類型，確保本機裝置連線的等待時間可確定。 例如，此程式碼會擷取所有可透過本機連線存取的條碼掃描器。 

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

### <a name="reacting-to-device-connection-changes-with-devicewatcher"></a>使用 DeviceWatcher 回應裝置連線變更

當您的 App 執行時，裝置有時會中斷或更新，或是需要加入新的裝置。 您可以使用 **DeviceWatcher** 類別來存取裝置相關事件，讓您的 App 可以適當回應。 以下是 **DeviceWatcher** 使用方式的範例，如果新增、移除或更新裝置，就會呼叫其中的方法虛設常式。

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

## <a name="checking-the-capabilities-of-a-point-of-service-device"></a>檢查服務點裝置的功能
即使在同一裝置類別 (例如條碼掃描器)，每個裝置的屬性可能在各型號之間都會有相當大的差別。 如果應用程式需要特定裝置屬性，您可能需要檢查每個連接的裝置物件，以判斷是否支援屬性。 例如，您的企業或許需要使用特定條碼列印模式來建立標籤。 以下是您可檢查以判斷連接的條碼掃描器是否支援特定符號學的方式。 

> [!NOTE]
> 符號學是條碼用來編碼訊息的語言對應。

```Csharp
try
{
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceId);
    if (await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32))
    {
        Debug.WriteLine("Has symbology");
    }
}
catch (Exception ex)
{
    Debug.WriteLine("FromIdAsync() - " + ex.Message);
}
```

### <a name="using-the-devicecapabilities-class"></a>使用 Device.Capabilities 類別
**Device.Capabilities** 類別是所有服務點裝置類別的屬性，可用來取得關於每個裝置的一般資訊。 例如，此範例會判斷裝置是否支援統計資料報告，若不支援，則擷取任何受支援類型的統計資料。

```Csharp
try
{
    if (barcodeScanner.Capabilities.IsStatisticsReportingSupported)
    {
        Debug.WriteLine("Statistics reporting is supported");

        string[] statTypes = new string[] {""};
        IBuffer ibuffer = await barcodeScanner.RetrieveStatisticsAsync(statTypes);
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: RetrieveStatisticsAsync() - " + ex.Message);
}
```

## <a name="claiming-a-point-of-service-device"></a>宣告服務點裝置
在使用服務點裝置可供作用中輸入或輸出之前，您必須宣告該裝置，並授與應用程式對其許多功能的獨佔存取權。 此範例示範如何在您使用上述其中一個方法找到裝置後，宣告條碼掃描器裝置。

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}
```

### <a name="retaining-the-device"></a>保留裝置
透過網路或藍牙連線使用服務點裝置時，您可能希望與網路上的其他應用程式共用該裝置  (如需詳細資訊，請參閱[共用裝置](#sharing-a-device-between-apps))。有時候，您可能想要佔住裝置長時間使用。 此範例示範如何在其他應用程式要求釋放已宣告的條碼掃描器之後保留該裝置。

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner e)
{
    e.RetainDevice();  // Retain exclusive access to the device
}
```

## <a name="input-and-output"></a>輸入和輸出

當您宣告裝置之後，裝置幾乎已可供使用。 若要接收裝置的輸入，您必須設定並啟用委派才能收到資料。 在以下範例中，我們會宣告條碼掃描器裝置、設定其解碼屬性，然後呼叫 **EnableAsync** 以啟用裝置的解碼輸入。 此程序在不同裝置類別中各有不同，如需有關如何設定非條碼裝置委派的指引，請參閱 [UWP app 範例](https://github.com/Microsoft/Windows-universal-samples#devices-and-sensors)。

```Csharp
try
{
    claimedBarcodeScanner = await barcodeScanner.ClaimScannerAsync();
    if (claimedBarcodeScanner != null)
    {
        claimedBarcodeScanner.DataReceived += claimedBarcodeScanner_DataReceived;
        claimedBarcodeScanner.IsDecodeDataEnabled = true;
        await claimedBarcodeScanner.EnableAsync();
    }
}
catch (Exception ex)
{
    Debug.WriteLine("EX: ClaimScannerAsync() - " + ex.Message);
}


void claimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    string symbologyName = BarcodeSymbologies.GetName(args.Report.ScanDataType);
    var scanDataLabelReader = DataReader.FromBuffer(args.Report.ScanDataLabel);
    string barcode = scanDataLabelReader.ReadString(args.Report.ScanDataLabel.Length);
}
```

## <a name="sharing-a-device-between-apps"></a>在應用程式之間共用裝置

服務點裝置經常用於多個 App 必須在一小段時間內存取這些裝置的案例。  在本機 (USB 或其他有線連線) 或透過藍牙或 IP 網路連接至多個 App 時，可以共用裝置。 視每個 App 的需求而定，某一個處理程序可能需要在裝置上處置其宣告。 這個程式碼會處置我們宣告的條碼掃描器裝置，並允許其他 App 宣告和使用該裝置。

```Csharp
if (claimedBarcodeScanner != null)
{
    claimedBarcodeScanner.Dispose();
    claimedBarcodeScanner = null;
}
```

> [!NOTE]
> 已宣告和未宣告的服務點裝置類別都會實作 [IClosable 介面](https://docs.microsoft.com/uwp/api/windows.foundation.iclosable)。 如果裝置透過網路或藍牙應用程式連接至 App，則必須先處置已宣告及未宣告的物件，其他 App 才能進行連接。

## <a name="see-also"></a>請參閱
+ [條碼掃描器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [收銀機範例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行顯示範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁條讀取器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POS 印表機範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

