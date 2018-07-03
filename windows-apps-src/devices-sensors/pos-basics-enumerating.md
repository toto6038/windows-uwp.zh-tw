---
author: TerryWarwick
title: 列舉 PointOfService 裝置
description: 了解如何列舉 PointOfService 裝置
ms.author: jken
ms.date: 06/8/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: be1fdc42295fc03ff86a69e287a4089abe547689
ms.sourcegitcommit: ee77826642fe8fd9cfd9858d61bc05a96ff1bad7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2018
ms.locfileid: "2018436"
---
# <a name="enumerating-point-of-service-devices"></a>列舉服務點裝置
本節中，您會了解如何 [**定義裝置選取器**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)，用來查詢提供給系統的裝置並用此選取器使用其中一項下列方法列舉服務點裝置：

**方法 1:**[**取得第一個可用裝置**](#Method-1:-get-first-available-device)<br />在本節中，您將會了解如何使用 **GetDefaultAsync** 存取特定 PointOfService 裝置類別中的第一個可用裝置。

**方法 2:**[**裝置的快照**](#Method-2:-Snapshot-of-devices)<br />在本節中，您將會了解如何列舉 PointOfService 裝置的快照，其在指定的時間點出現在系統上。 您想要建置自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置時，這非常實用。 FindAllAsync 會保留回結果直到整個列舉完成。

**方法 3:** [**列舉並監看**](#Method-3:-Enumerate-and-watch)<br />在本節中，您將深入了解更多強大和彈性的列舉模式，可讓您列舉目前存在的裝置，且從系統新增或移除裝置時也會收到通知。  當您想要保留背景中目前的裝置清單，可在您的 UI 中顯示，而不是等待發生快照時，這非常實用。
 

---
## <a name="define-a-device-selector"></a>定義裝置選取器
裝置選取器可讓您在列舉裝置時限制要搜尋的裝置。  這會讓您只取得相關的結果，並減少列舉所需裝置所花費的時間。  

使用 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 將提供您選取器以列舉所有附加至系統的 POSPrinters，包括 USB、網路和藍牙 POSPrinters。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();   

```

使用 [**GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector_Windows_Devices_PointOfService_PosConnectionTypes_) 採用 [**PosConnectionTypes**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posconnectiontypes) 做為參數的方法，您可以限制選取器列舉本機、網路或附加 POSPrinters 的藍牙，減少完成查詢所需的時間。  下列範例顯示此方法的使用方式，以定義只支援本機附加 POSPrinters 的選取器。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);   

```
> [!TIP]
> 如需建置進階選取器的詳細資訊，請參閱 [**建置裝置選取器**](https://docs.microsoft.com/windows/uwp/devices-sensors/build-a-device-selector)。

---

## <a name="method-1-get-first-available-device"></a>方法 1：取得第一個可用裝置

若要取得 PointOfService 裝置的最簡單方式是使用 **GetDefaultAsync** 以取得 PointOfService 裝置類別中的第一個可用裝置。 

下列範例簡單示範適用於 BarcodeScanner 的 [**GetDefaultAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 使用。 所有 PointOfService 裝置類別的編碼模式都相似。

```Csharp

using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();

```

> [!CAUTION]
> 使用 GetDefaultAsync 必須小心，因為它可能會將不同的裝置從一個工作階段傳回到下一個。 許多事件可能影響這個列舉，導致第一個可用裝置不同，包括： 
> - 變更連接到您電腦的相機 
> - 變更連接到您電腦的 PointOfService 裝置
> - 變更網路中連接您網路上可用的 PointOfService 裝置
> - 變更您電腦範圍中的藍牙 PointOfService 裝置 
> - PointOfService 設定的變更 
> - 安裝驅動程式或 OPOS 服務物件
> - 安裝 PointOfService 擴充功能
> - 更新 Windows 作業系統

---

## <a name="method-2-snapshot-of-devices"></a>方法 2：裝置的快照

在某些情況下，或許您想要組建自己的 UI，或者需要在不向使用者顯示 UI 的情況下列舉裝置。  在這些情況下，您可能會列舉目前連接或使用 [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 與系統配對的裝置快照。  這個方法在整個列舉完成之前，將不會顯示任何結果。

> [!TIP]
> 使用 FindAllAsync 限制您查詢所需的連接類型時，建議用 PosConnectionTypes 參數使用 GetDeviceSelector 方法。  網路和藍牙連線可能會延遲結果，因為在傳回 FindAllAsync 結果前必須先完成他們的列舉。

>[!CAUTION] 
>FindAllAsync 傳回裝置的陣列。  這個陣列的順序可以在工作階段間變更，因此不建議透過在陣列中使用硬式編碼索引依賴特定順序。  使用 DeviceInformation 屬性來篩選您的結果，或是提供 UI，讓使用者可從中選擇。

這個範例使用上述定義的選取器，使用 FindAllAsync 來替裝置進行快照，然後通過每個由集合傳回的項目列舉，並將裝置名稱與 ID 寫入偵錯輸出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 使用 [**Windows.Devices.Enumeration**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) API 時，您通常需要使用 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 物件，以取得特定裝置的相關資訊。 例如，[**DeviceInformation.ID**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.id) 屬性可用來復原及重複使用相同的裝置，如果在未來的工作階段可用的話，且 [**DeviceInformation.Name**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.name) 屬性可以基於顯示用途用在您的應用程式中。  如需有關其他可用屬性的資訊，請參閱 [**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation) 參考頁面。

---

## <a name="method-3-enumerate-and-watch"></a>方法 3：列舉並監看

一個功能更強大且彈性的列舉裝置方法是建立一個 [**DeviceWatcher**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  裝置監看員動態列舉裝置，如此一來，如果在初始列舉完成之後加入、移除或變更裝置，應用程式就會收到通知。  DeviceWatcher 可讓您偵測連接裝置的網路何時上線，藍牙裝置是否處於範圍，以及是否已拔除本機連接裝置，如此一來，您可以在應用程式中採取適當的動作。

這個範例使用上述定義的選取器，建立 DeviceWatcher 以及為新增、移除和已更新的通知定義事件處理常式。 您必須填寫希望對每個通知採取動作的詳細資訊。

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
> 如需使用 DeviceWatcher 的詳細資料，請參閱 [**列舉並監控裝置**]( https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)
