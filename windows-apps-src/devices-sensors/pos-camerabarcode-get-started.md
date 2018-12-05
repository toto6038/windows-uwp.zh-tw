---
title: 開始使用相機條碼掃描器
description: 了解如何使用相機條碼掃描器
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: b49ba463e39d09b915ce3925f94ae7d9f11a9a47
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8697806"
---
# <a name="getting-started-with-a-camera-barcode-scanner"></a>開始使用相機條碼掃描器
## <a name="step-1-add-capability-declarations-to-your-app-manifest"></a>步驟 1：將功能宣告加入至 App 資訊清單
1. 在 Microsoft Visual Studio 中，按兩下 **\[方案總管\]** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2. 選取 **\[功能\]** 索引標籤
3. 核取 **\[網路攝影機\]** 和 **\[PointOfService\]** 的方塊 

>[!NOTE] 
> 要讓軟體解碼器從相機接收要解碼的畫面格解碼，以及提供應用程式的預覽畫面，需要**網路攝影機**功能

## <a name="step-2-add-using-directives"></a>步驟 2：新增使用指示詞

```Csharp
using Windows.Devices.Enumeration;
using Windows.Devices.PointOfService;
```
## <a name="step-3-define-your-device-selector"></a>步驟 3：定義您的裝置選取器

### **<a name="option-a-find-all-barcode-scanners"></a>選項 A：尋找所有條碼掃描器**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector();       
```

### **<a name="option-b-scoping-device-selector-to-connection-type"></a>選項 B：將裝置選取器範圍設定至連接類型**

```Csharp
string selector = BarcodeScanner.GetDeviceSelector(PosConnectionTypes.Local);
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

## <a name="step-4-enumerate-barcode-scanners"></a>步驟 4：列舉條碼掃描器
如果您未預期在應用程式的存留期間裝置清單有所變更，您只要使用 *FindAllAsync* 照一次就可以列舉，但如果您認為條碼掃描器清單可能在應用程式的存留期間變更，您應該改為使用 *DeviceWatcher*。  

> [!Important] 
> 使用 GetDefaultAsync 列舉 PointOfService 裝置可能產生一致的行為，因其只會傳回類別中找到的第一個裝置，而這可能會隨工作階段變動。

### **<a name="option-a-enumerate-a-snapshot-of-barcode-scanners"></a>選項 A：列舉條碼掃描器的快照**
```Csharp
DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);
```

> [!TIP]
> 如需使用 *FindAllAsync* 的詳細資訊，請參閱[*列舉裝置的快照*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-a-snapshot-of-devices)。

### **<a name="option-b-enumerate-and-watch-for-changes-in-available-barcode-scanners"></a>選項 B：列舉及監看可用條碼掃描器中的變更**
```Csharp
DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);

// TODO: Add Event Listeners and Handlers
```
> [!TIP]
> 如需詳細資訊，請參閱[*列舉及監看裝置變更*](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices#enumerate-and-watch-devices)和 [*DeviceWatcher*](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。

## <a name="step-5-identify-camera-barcode-scanners"></a>步驟 5：找出相機條碼掃描器
相機條碼掃描器是動態建立，只要 Windows 使用軟體解碼器配對連接到您電腦的相機。  每個相機 - 解碼器配對是完全可用的條碼掃描器。

對於產生的裝置集合中的每個條碼掃描器，您可以透過檢查 [*BarcodeScanner.VideoDeviceID*](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.videodeviceid#Windows_Devices_PointOfService_BarcodeScanner_VideoDeviceId) 屬性來判斷哪些是相機條碼掃描器和實體條碼掃描器。  非空值的 VideoDeviceID 會指出您的裝置集合的條碼掃描器物件是相機條碼掃描器。  如果您有多個相機條碼掃描器，您可能想要建置一個排除實體條碼掃描器的不同集合。 

使用 Windows 隨附解碼器的相機條碼掃描器將會顯示為： 

> Microsoft BarcodeScanner (*您的相機名稱在此*)

如果您的相機內建於電腦機殼，若有多個相機，名稱可能區分為*前置*和*背面*。  在未來，可能提供其他軟體解碼器，並執行另一種命名配置。

## <a name="step-6-claim-the-camera-barcode-scanner"></a>步驟 6：宣告相機條碼掃描器 
使用 [BarcodeScanner.ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 來取得專用的相機條碼掃描器。

## <a name="step-7-system-provided-preview"></a>步驟 7：系統提供的預覽
使用者需要相機預覽才能成功將相機瞄準條碼。  Windows 提供簡單的相機預覽，其將啟動對話方塊，可基本控制相機條碼掃描器。  只要呼叫 [ClaimedBarcodeScanner.ShowVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.showvideopreviewasync) 即可開啟對話方塊，以及呼叫 [ClaimedBarcodeScanner.HideVideoPreview](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.hidevideopreview) 在完成時關閉它。

> [!TIP]
> 請參閱[主控預覽](pos-camerabarcode-hosting-preview.md)以針對您的應用程式中的相機條碼掃描器主控預覽。

## <a name="step-8-initiate-scan"></a>步驟 8： 初始化掃描 
您可以藉由呼叫 [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 來起始掃描程序。  
根據 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，掃描器可能只掃描一個條碼然後停止或持續掃描，直到您呼叫 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync) 為止。

設定 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的所需值，來控制解碼條碼時的掃描器行為。

| 值 | 描述 |
| ----- | ----------- |
| True   | 只掃描一個條碼然後停止 |
| False  | 持續掃描條碼而不停止 |