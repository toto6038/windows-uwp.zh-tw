---
author: TerryWarwick
title: 列舉 PointOfService 裝置
description: 了解如何列舉 PointOfService 裝置
ms.author: jken
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 10804b006cb7ab542c74e363af5134634b7651e3
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2018
ms.locfileid: "6156262"
---
# <a name="enumerating-point-of-service-devices"></a>列舉服務點裝置
本節中，您會了解如何 [定義裝置選取器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)，用來查詢提供給系統的裝置並用此選取器使用其中一項下列方法列舉服務點裝置：

**方法 1:**[使用裝置選擇器](#method-1:-use-a-device-picker)
<br/>
顯示裝置選擇器 UI，讓使用者選擇連接的裝置。 這個方法會處理更新清單，當裝置連接並移除，並會更簡單、 更安全，比其他方法。

**方法 2:**[取得第一個可用裝置](#Method-1:-get-first-available-device)<br />使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync)存取特定服務點裝置類別中的第一個可用裝置。

**方法 3:**[裝置的快照](#Method-2:-Snapshot-of-devices)<br />列舉的時間出現在特定點系統的服務點裝置的快照。 您想要建置自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置時，這非常實用。 [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)會保留回結果直到整個列舉完成。

**方法 4:**[列舉及監看](#Method-3:-Enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)是更強大且彈性的列舉模式，可讓您列舉目前存在，裝置，並新增或從系統移除裝置時也會收到通知。  當您想要保留背景中目前的裝置清單，可在您的 UI 中顯示，而不是等待發生快照時，這非常實用。

## <a name="define-a-device-selector"></a>定義裝置選取器
裝置選取器可讓您在列舉裝置時限制要搜尋的裝置。  這可讓您只獲得相關結果，並減少列舉所需的裝置所花費的時間。

您可以使用**GetDeviceSelector**方法適用於您正在尋找的裝置選取器，取得該類型的裝置類型。 例如，使用[PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector)會提供您選取器以列舉所有[PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)附加至系統，包括 USB、 網路及藍牙 POS 印表機。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

針對不同的裝置類型**GetDeviceSelector**方法如下：

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

使用**GetDeviceSelector**方法來接受[PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)值做為參數，您可以限制您的選取器列舉本機、 網路或附加的藍牙 POS 裝置，減少完成查詢所需的時間。  下列範例顯示使用的這個方法，以定義選取器支援僅在本機連接的 POS 印表機。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 如需建置進階選取器的詳細資訊，請參閱 [建置裝置選取器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)。

## <a name="method-1-use-a-device-picker"></a>方法 1： 使用的裝置選擇器

[DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker)類別可讓您顯示包含一份使用者可從中選擇的裝置選擇器飛出視窗。 您可以使用[篩選器](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter)屬性以選擇要顯示在選擇器中的裝置類型。 這個屬性是類型[DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)。 您可以新增裝置類型篩選器使用[SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses)或[SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors)屬性。

當您準備好以顯示裝置選擇器時，您可以呼叫[PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync)方法，這將會顯示在選擇器 UI 並傳回所選取的裝置。 您將需要指定飛出視窗的顯示位置會決定[Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) 。 這個方法會傳回[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)物件，因此若要使用它的服務點 Api，您將需要使用您想要的特定裝置類別**FromIdAsync**的方法。 您將[DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)屬性傳遞做為方法的*deviceId*參數，並取得做為傳回值的裝置類別的執行個體。

下列程式碼片段會建立**DevicePicker**、 新增條碼掃描器篩選器，具有使用者挑選一個裝置，並接著會建立基礎的裝置識別碼的**BarcodeScanner**物件：

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

## <a name="method-2-get-first-available-device"></a>方法 2： 取得第一個可用裝置

若要取得的服務點裝置的最簡單方式是使用**GetDefaultAsync**取得服務點裝置類別中的第一個可用裝置。 

下列範例說明[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync)適用於[BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)的使用。 程式碼撰寫模式則是類似的所有服務點裝置類別。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> 因為它可能會從一個工作階段將不同的裝置，到下**GetDefaultAsync**必須小心使用。 許多事件可能影響這個列舉，導致第一個可用裝置不同，包括： 
> - 變更連接到您電腦的相機 
> - 變更 in Point of 服務裝置連接到您電腦
> - 在您網路上可用的 [網路連接服務點裝置中變更
> - 在您的電腦的範圍內的藍芽服務點裝置中變更 
> - 服務點設定的變更 
> - 安裝驅動程式或 OPOS 服務物件
> - 安裝服務點擴充功能
> - 更新 Windows 作業系統

## <a name="method-3-snapshot-of-devices"></a>方法 3： 裝置的快照

在某些情況下，或許您想要組建自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置。  在這些情況下，您可能會列舉目前連接或使用 [DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 與系統配對的裝置快照。  這個方法在整個列舉完成之前，將不會顯示任何結果。

> [!TIP]
> 建議使用**FindAllAsync**限制您查詢所需的連接類型時，用**PosConnectionTypes**參數使用**GetDeviceSelector**方法。  因為在傳回**FindAllAsync**結果前，必須完成他們的列舉網路及藍牙連線可能會延遲結果。

> [!CAUTION] 
> **FindAllAsync**傳回裝置的陣列。  這個陣列的順序可以在工作階段間變更，因此不建議透過在陣列中使用硬式編碼索引依賴特定順序。  使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)屬性來篩選您的結果，或是提供 UI 讓使用者可從中選擇。

這個範例使用上述定義的裝置使用**FindAllAsync**快照的選取器，然後透過每個傳回的集合的項目列舉並寫入偵錯輸出中的裝置名稱和識別碼。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 使用 [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 時，您通常需要使用 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 物件，以取得特定裝置的相關資訊。 例如， [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)屬性可用來復原及重複使用相同的裝置，如果是在未來的工作階段中可用，而[DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name)屬性可以基於顯示用途，在您的應用程式中使用。  如需有關其他可用屬性的資訊，請參閱 [DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 參考頁面。

## <a name="method-4-enumerate-and-watch"></a>方法 4： 列舉並監看

一個功能更強大且彈性的列舉裝置方法是建立一個 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  裝置監看員動態列舉裝置，如此一來，如果在初始列舉完成之後加入、移除或變更裝置，應用程式就會收到通知。  **DeviceWatcher**可讓您偵測網路連線的裝置何時上線，藍牙裝置是否處於範圍，以及是否拔除本機連接的裝置，以便您可以在您的應用程式中採取適當的動作。

這個範例使用上述定義建立**DeviceWatcher**的選取器，以及定義事件處理常式[新增](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)、[移除](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)和[已更新](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)的通知。 您必須填寫希望對每個通知採取動作的詳細資訊。

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
> 如需使用**DeviceWatcher**的詳細資訊，請參閱[列舉及監看裝置]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)。

## <a name="see-also"></a>請參閱
* [開始使用服務點](pos-basics.md)
* [DeviceInformation 類別](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [Pos 印表機類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 列舉](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 類別](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]