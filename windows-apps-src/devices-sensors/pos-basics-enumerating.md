---
title: 列舉 PointOfService 裝置
description: 了解如何列舉 PointOfService 裝置
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 27d25864941b9d73c9b12e6329eab79fac1b15bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620903"
---
# <a name="enumerating-point-of-service-devices"></a>列舉服務點裝置
在本節中，您將學習如何[定義裝置選取器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)用來查詢裝置提供給系統，並使用這個選取器，以列舉 Point of Service 的裝置，使用下列方法之一：

**方法 1:**[使用裝置選擇器](#method-1-use-a-device-picker)
<br/>
顯示裝置選擇器 UI，並讓使用者選擇連接的裝置。 這個方法會處理更新清單，裝置會連接並移除，並更簡單，而且比其他方法更安全。

**方法 2:**[取得第一個可用的裝置](#method-2-get-first-available-device)<br />使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync)存取特定的 Point of Service 的裝置類別中第一個可用的裝置。

**方法 3:**[快照集的裝置](#method-3-snapshot-of-devices)<br />列舉 Point of Service 的裝置，則會在指定的時間點的系統時間的快照集。 您想要建置自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置時，這非常實用。 [FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)會保留傳回結果之前完成整個列舉型別。

**方法 4:**[列舉，並觀看](#method-4-enumerate-and-watch)<br />[DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)更強大而彈性列舉的模型，可讓您列舉目前存在於，部署的裝置，並新增或從系統中移除裝置時，也收到通知。  當您想要保留背景中目前的裝置清單，可在您的 UI 中顯示，而不是等待發生快照時，這非常實用。

## <a name="define-a-device-selector"></a>定義裝置選取器
裝置選取器可讓您在列舉裝置時限制要搜尋的裝置。  這可讓您只取得相關的結果，並減少列舉所需的裝置所花費的時間。

您可以使用**GetDeviceSelector**想要為裝置選取器取得該類型的裝置類型的方法。 例如，使用[PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector)會提供您的選取器，以列舉所有[PosPrinters](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)附加至系統，包括 USB、 網路和藍芽 POS 印表機。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

**GetDeviceSelector**不同裝置類型的方法：

* [BarcodeScanner.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

使用**GetDeviceSelector**方法接受[PosConnectionTypes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)值做為參數，您可以限制您的選取器以列舉本機、 網路、 或藍芽附加 POS 裝置、 減少查詢完成所花費的時間。  下列範例會示範使用此方法，以定義的選取器，只會在本機支援附加 POS 印表機。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 請參閱[建置的裝置選取器](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)建置更進階的選取器字串。

## <a name="method-1-use-a-device-picker"></a>方法 1：使用裝置選擇器

[DevicePicker](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker)類別可讓您顯示選擇器飛出視窗，其中包含一份可從中選擇使用者的裝置。 您可以使用[篩選](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.filter)選擇的選擇器中顯示的裝置類型的屬性。 此屬性的類型是[DevicePickerFilter](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter)。 您可以加入篩選條件使用的裝置類型[SupportedDeviceClasses](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses)或是[SupportedDeviceSelectors](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors)屬性。

當您準備好要顯示的裝置選擇器時，您可以呼叫[PickSingleDeviceAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync)方法，它會顯示選擇器 UI，並傳回所選的裝置。 您必須指定[Rect](https://docs.microsoft.com/uwp/api/windows.foundation.rect) ，將會決定飛出視窗出現的位置。 這個方法會傳回[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)物件，因此若要使用與點的服務 Api，您必須使用**FromIdAsync**您想要的特定裝置類別的方法。 您傳遞[DeviceInformation.Id](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)屬性的方法做*deviceId*參數，並取得傳回值的裝置類別的執行個體。

下列程式碼片段會建立**DevicePicker**、 將條碼掃描器篩選、 挑選一個裝置，使用者，然後建立**BarcodeScanner**物件，根據裝置識別碼：

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

## <a name="method-2-get-first-available-device"></a>方法 2：取得第一個可用的裝置

取得 Point of Service 裝置的最簡單方式是使用**GetDefaultAsync**取得 Point of Service 的裝置類別中第一個可用的裝置。 

下列範例說明如何使用[GetDefaultAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) for [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)。 程式碼撰寫模式是類似於所有的 Point of Service 的裝置類別。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> **GetDefaultAsync**必須要小心使用，因為它可能會傳回不同的裝置從一個工作階段給下一步。 許多事件可能影響這個列舉，導致第一個可用裝置不同，包括： 
> - 變更連接到您電腦的相機 
> - 變更 in Point of 服務裝置連接到電腦
> - 變更在網路上可用的 網路連接點的服務裝置
> - 在您的電腦範圍內的藍芽 Point of Service 裝置中變更 
> - Point of Service 組態的變更 
> - 安裝驅動程式或 OPOS 服務物件
> - 安裝 Point of Service 的延伸模組
> - 更新 Windows 作業系統

## <a name="method-3-snapshot-of-devices"></a>方法 3：快照集的裝置

在某些情況下，或許您想要組建自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置。  在這些情況下，您可以列舉目前連線或與系統使用配對的裝置的快照集[DeviceInformation.FindAllAsync](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)。  這個方法在整個列舉完成之前，將不會顯示任何結果。

> [!TIP]
> 建議使用**GetDeviceSelector**方法**PosConnectionTypes**參數使用時**FindAllAsync**來限制查詢，以在連線類型所需。  網路 」 及藍芽連線可以延遲結果，如其列舉型別必須先完成才能**FindAllAsync**傳回結果。

> [!CAUTION] 
> **FindAllAsync**傳回裝置的陣列。  這個陣列的順序可以在工作階段間變更，因此不建議透過在陣列中使用硬式編碼索引依賴特定順序。  使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)屬性，以篩選結果，或提供使用者可從中選擇的 UI。

這個範例會使用上面定義來建立快照集的裝置使用的選取器**FindAllAsync**然後列舉每個集合所傳回的項目，並將裝置名稱和識別碼寫入偵錯輸出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 使用時[Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) Api，您會經常需要使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)來取得特定裝置的相關資訊的物件。 比方說， [DeviceInformation.ID](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id)屬性可用來復原和重複使用相同的裝置，如果它是用於未來的工作階段並[DeviceInformation.Name](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name)屬性可以用於顯示在您的應用程式的用途。  請參閱[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)可用的其他屬性的相關資訊的參考頁面。

## <a name="method-4-enumerate-and-watch"></a>方法 4：列舉，並觀看

有一個可用來列舉裝置、功能更強大且彈性的方法是建立 [DeviceWatcher](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  裝置監看員動態列舉裝置，如此一來，如果在初始列舉完成之後加入、移除或變更裝置，應用程式就會收到通知。  A **DeviceWatcher**可讓您偵測網路連線時，裝置在上線、 藍芽裝置是在範圍內，也如同在本機連接的裝置已拔除，讓您可以採取適當的動作，在您應用程式。

這個範例會使用上面定義來建立的選取器**DeviceWatcher**也做為定義的事件處理常式[Added](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)，[已移除](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)，和[更新](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)通知。 您必須填寫希望對每個通知採取動作的詳細資訊。

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
> 請參閱[Enumerate 和監看式裝置]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)如需使用詳細**DeviceWatcher**。

## <a name="see-also"></a>請參閱
* [Point of Service 使用者入門](pos-basics.md)
* [DeviceInformation 類別](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 列舉](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 類別](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 類別](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]