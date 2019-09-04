---
title: 開始使用相機條碼掃描器
description: 了解如何使用相機條碼掃描器
ms.date: 09/02/2019
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: b35ff6b183a6344fbc8da6b44a6cb81ea695a1c9
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243297"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>開始使用相機條碼掃描器

此處使用的程式碼片段僅供示範之用。 如需實用範例, 請參閱[條碼掃描器範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)。

## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>步驟 1:將功能宣告新增至您的應用程式資訊清單

1. 在 Microsoft Visual Studio 中，按兩下 [方案總管] 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2. 選取 **\[功能\]** 索引標籤
3. 核取 **\[網路攝影機\]** 和 **\[PointOfService\]** 的方塊

>[!NOTE]
> 要讓軟體解碼器從相機接收要解碼的畫面格解碼，以及提供應用程式的預覽畫面，需要**網路攝影機**功能

## <a name="step-2-add-using-directives"></a>步驟 2:新增 using 指示詞

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```

## <a name="step-3-define-your-device-selector"></a>步驟 3：定義您的裝置選取器

### <a name="option-a-find-all-barcode-scanners"></a>**選項 A:尋找所有條碼掃描器**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();
```

### <a name="option-b-scoping-device-selector-to-connection-type"></a>**選項 B:將裝置選取器範圍設定為連線類型**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-all-barcode-scanners"></a>步驟 4：列舉所有條碼掃描器

如果您不想要在應用程式的存留期內變更的裝置清單, 您可以只使用[DeviceInformation](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)來列舉快照集一次, 但如果您認為條碼掃描器清單可能會在您的期限內變更應用程式您應該改用[DeviceWatcher](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher) 。  

> [!Important]
> 使用 GetDefaultAsync 列舉 PointOfService 裝置可能產生一致的行為，因其只會傳回類別中找到的第一個裝置，而這可能會隨工作階段變動。

### <a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>**選項 A:列舉條碼掃描器的快照集**

```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> 如需使用 *FindAllAsync* 的詳細資訊，請參閱[*列舉裝置的快照*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices)。

### <a name="option-b-enumerate-available-barcode-scanners-and-watch-for-changes-to-the-available-scanners"></a>**選項 B:列舉可用的條碼掃描器, 並監看是否有可用掃描器的變更**

```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
watcher.Added += Watcher_Added;
watcher.Removed += Watcher_Removed;
watcher.Updated += Watcher_Updated;
watcher.Start();
```

> [!TIP]
> 如需詳細資訊，請參閱[*列舉及監看裝置變更*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)和 [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。

## <a name="step-5-identify-camera-barcode-scanners"></a>步驟 5：識別相機條碼掃描器

相機條碼掃描器是動態建立，只要 Windows 使用軟體解碼器配對連接到您電腦的相機。  每個相機 - 解碼器配對是完全可用的條碼掃描器。

針對產生的裝置集合中的每個條碼掃描器, 您可以藉由檢查[*BarcodeScanner. VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId)屬性來區分相機條碼掃描器和實體條碼掃描器。  非 Null 的 VideoDeviceID 表示裝置集合中的條碼掃描器物件是相機條碼掃描器。  如果您有多個相機條碼掃描器, 您可能會想要建立不同的集合, 以排除實體條碼掃描器。

使用 Windows 隨附之解碼器的相機條碼掃描器會識別為:

> Microsoft BarcodeScanner (*您的相機名稱在此*)

如果您有一個以上的相機, 而且它們內建在電腦的底座中, 則名稱可能會區分*前端*和*後端*相機。

> [!NOTE]
> 未來可能會發行其他具有不同命名配置的軟體解碼器。

當 DeviceWatcher 啟動時 (步驟 4), 它會列舉每個連線的裝置。 在這裡, 我們將可用的掃描器新增至條碼掃描器集合, 並將集合系結至 ListBox。

```csharp
ObservableCollection<BarcodeScannerInfo> barcodeScanners = new ObservableCollection<BarcodeScannerInfo>();

private async void Watcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        barcodeScanners.Add(new BarcodeScannerInfo(args.Name, args.Id));

        // Select the first scanner by default.
        if (barcodeScanners.Count == 1)
        {
            ScannerListBox.SelectedIndex = 0;
        }
    });
}
```

當 ListBox 的 SelectedIndex 變更時 (在上一個程式碼片段中, 預設會選取第一個專案), 我們會查詢裝置資訊。

```csharp
private async void ScannerSelection_Changed(object sender, SelectionChangedEventArgs args)
{
    var selectedScannerInfo = (BarcodeScannerInfo)args.AddedItems[0];
    var deviceId = selectedScannerInfo.DeviceId;

    await SelectScannerAsync(deviceId);
}
```

## <a name="step-6-claim-the-camera-barcode-scanner"></a>步驟 6：索取相機條碼掃描器

使用 [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 來取得專用的相機條碼掃描器。

```csharp
private async Task SelectScannerAsync(string scannerDeviceId)
{
    selectedScanner = await BarcodeScanner.FromIdAsync(scannerDeviceId);

    if (selectedScanner != null)
    {
        claimedScanner = await selectedScanner.ClaimScannerAsync();
        if (claimedScanner != null)
        {
            await claimedScanner.EnableAsync();
        }
        else
        {
            rootPage.NotifyUser("Failed to claim the selected barcode scanner", NotifyType.ErrorMessage);
        }
    }
    else
    {
        rootPage.NotifyUser("Failed to create a barcode scanner object", NotifyType.ErrorMessage);
    }
}
```

## <a name="step-7-system-provided-preview"></a>步驟 7：系統提供的預覽

使用者需要相機預覽才能成功將相機瞄準條碼。  Windows 提供簡單的相機預覽, 可啟動適用于相機條碼掃描器基本控制的對話方塊。  只要呼叫 [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) 即可開啟對話方塊，以及呼叫 [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) 在完成時關閉它。

> [!TIP]
> 請參閱[主控預覽](pos-camerabarcode-hosting-preview.md)以針對您的應用程式中的相機條碼掃描器主控預覽。

## <a name="step-8-initiate-scan"></a>步驟 8：起始掃描

您可以藉由呼叫 [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 來起始掃描程序。

視[**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)的值而定, 掃描器只會掃描一個條碼, 然後在您呼叫[**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)之前, 一直停止或掃描。

設定 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的所需值，來控制解碼條碼時的掃描器行為。

| 值 | 描述 |
| ----- | ----------- |
| True   | 只掃描一個條碼然後停止 |
| 偽  | 持續掃描條碼而不停止 |

## <a name="see-also"></a>另請參閱

### <a name="samples"></a>範例

- [條碼掃描器範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
