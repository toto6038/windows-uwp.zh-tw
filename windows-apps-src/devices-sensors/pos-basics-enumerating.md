---
title: 列舉 PointOfService 裝置
description: 瞭解使用裝置選取器來查詢和列舉系統可用之 PointOfService 裝置的四種方法。
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8fc869ab16fd745da32572a54354aeeca8abb14c
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970296"
---
# <a name="enumerating-point-of-service-devices"></a>列舉服務點裝置
本節中，您會了解如何 [定義裝置選取器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)，用來查詢提供給系統的裝置並用此選取器使用其中一項下列方法列舉服務點裝置：

**方法1：** [使用裝置選擇器](#method-1-use-a-device-picker)
<br/>
顯示裝置選擇器 UI，並讓使用者選擇連線的裝置。 當附加和移除裝置時，這個方法會處理清單的更新，而且比其他方法更簡單且更安全。

**方法2：** [取得第一個可用的裝置](#method-2-get-first-available-device)<br />使用 [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) 來存取特定點服務裝置類別中的第一個可用裝置。

**方法3：** [裝置的快照](#method-3-snapshot-of-devices)集<br />列舉在給定時間點出現在系統上的服務裝置點的快照集。 您想要建置自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置時，這非常實用。 [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 會保留結果，直到整個列舉完成為止。

**方法4：** [列舉和監看](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 是一種功能更強大且更有彈性的列舉模型，可讓您列舉目前存在的裝置，並在從系統新增或移除裝置時，也會收到通知。  當您想要保留背景中目前的裝置清單，可在您的 UI 中顯示，而不是等待發生快照時，這非常實用。

## <a name="define-a-device-selector"></a>定義裝置選取器
裝置選取器可讓您在列舉裝置時限制要搜尋的裝置。  這可讓您只取得相關的結果，並減少列舉所需裝置所需的時間。

您可以針對您要尋找的裝置類型使用 **GetDeviceSelector** 方法，以取得該類型的裝置選取器。 例如，使用 PosPrinter 時， [GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 會提供您一個選取器來列舉附加至系統的所有 [POSPRINTERS](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter) ，包括 USB、網路和藍牙 POS 印表機。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

不同裝置類型的 **GetDeviceSelector** 方法如下：

* [BarcodeScanner. GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

使用採用[PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)值作為參數的**GetDeviceSelector**方法，您可以限制您的選取器來列舉本機、網路或連接藍牙的 POS 裝置，減少查詢完成所需的時間。  下列範例示範如何使用這個方法來定義僅支援本機連接之 POS 印表機的選取器。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 如需建置進階選取器的詳細資訊，請參閱 [建置裝置選取器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)。

## <a name="method-1-use-a-device-picker"></a>方法1：使用裝置選擇器

[DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker)類別可讓您顯示選擇器飛出視窗，其中包含可供使用者選擇的裝置清單。 您可以使用 [ [篩選](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter) ] 屬性來選擇要在選擇器中顯示的裝置類型。 這個屬性的型別為 [DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)。 您可以使用 [SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) 或 [SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) 屬性，將裝置類型新增至篩選準則。

當您準備好顯示裝置選擇器時，您可以呼叫 [PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) 方法，此方法會顯示選擇器 UI 並傳回選取的裝置。 您將需要指定一個 [矩形](https://docs.microsoft.com/uwp/api/windows.foundation.rect) ，以決定飛出視窗的出現位置。 這個方法會傳回 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 物件，因此若要將它與服務 api 點搭配使用，您必須針對您想要的特定裝置類別使用 **FromIdAsync** 方法。 您將 [DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 屬性傳遞為方法的 *deviceId* 參數，並取得裝置類別的實例做為傳回值。

下列程式碼片段會建立 **DevicePicker**、將條碼掃描器篩選器新增至其中，然後使用者挑選裝置，然後根據裝置識別碼建立 **BarcodeScanner** 物件：

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>方法2：取得第一個可用的裝置

取得服務點的最簡單方式是使用 **GetDefaultAsync** 取得服務裝置類別中的第一個可用裝置。 

下列範例說明如何使用 [GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 進行 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)。 所有服務裝置類別的程式碼撰寫模式都很類似。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync** 必須謹慎使用，因為它可能會將不同的裝置從某個會話傳回至下一個會話。 許多事件可能影響這個列舉，導致第一個可用裝置不同，包括： 
> - 變更連接到您電腦的相機 
> - 連接到您電腦的服務裝置變更點
> - 網路上可用的服務裝置網路連接點變更
> - 在您的電腦範圍內變更藍牙點的服務裝置 
> - 變更服務點設定 
> - 安裝驅動程式或 OPOS 服務物件
> - 安裝點的服務延伸模組
> - 更新 Windows 作業系統

## <a name="method-3-snapshot-of-devices"></a>方法3：裝置的快照集

在某些情況下，或許您想要組建自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置。  在這些情況下，您可能會列舉目前連接或使用 [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 與系統配對的裝置快照。  這個方法在整個列舉完成之前，將不會顯示任何結果。

> [!TIP]
> 建議您在使用**FindAllAsync**將查詢限制為所需的連線類型時，使用**GetDeviceSelector**方法搭配**PosConnectionTypes**參數。  網路和藍牙連線可能會延遲結果，因為它們的列舉必須在傳回 **FindAllAsync** 結果之前完成。

> [!CAUTION] 
> **FindAllAsync** 會傳回裝置的陣列。  這個陣列的順序可以在工作階段間變更，因此不建議透過在陣列中使用硬式編碼索引依賴特定順序。  使用 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 屬性來篩選您的結果，或提供 UI 供使用者選擇。

此範例使用上面定義的選取器來取得使用 **FindAllAsync** 的裝置快照集，然後列舉集合所傳回的每個專案，然後將裝置名稱和識別碼寫入至 debug 輸出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 時，您通常需要使用 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 物件，以取得特定裝置的相關資訊。 例如， [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 屬性可用來復原和重複使用相同的裝置（如果未來的會話中有提供），而且 [DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 屬性可用於應用程式中的顯示用途。  如需有關其他可用屬性的資訊，請參閱 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 參考頁面。

## <a name="method-4-enumerate-and-watch"></a>方法4：列舉和監看

一個功能更強大且彈性的列舉裝置方法是建立一個 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  裝置監看員動態列舉裝置，如此一來，如果在初始列舉完成之後加入、移除或變更裝置，應用程式就會收到通知。  **DeviceWatcher**可讓您偵測網路連線裝置何時上線、藍牙裝置在範圍內，以及是否插上本機連線的裝置，讓您可以在應用程式中採取適當的動作。

這個範例會使用上面定義的選取器來建立 **DeviceWatcher** ，以及定義 [已新增](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)、 [移除](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)和 [更新之](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated) 通知的事件處理常式。 您必須填寫希望對每個通知採取動作的詳細資訊。

```Csharp
using Windows.Devices.Enumeration;

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

> [!TIP]
> 如需有關使用**DeviceWatcher**的詳細資訊，請參閱[列舉和觀賞裝置]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)。

## <a name="see-also"></a>另請參閱
* [開始使用服務點](pos-basics.md)
* [DeviceInformation 類別](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 列舉](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 類別](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]