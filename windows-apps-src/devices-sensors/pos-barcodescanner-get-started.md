---
author: TerryWarwick
title: 開始使用條碼掃描器
description: 了解如何從通用 Windows 應用程式與條碼掃描器互動
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: ed0fa79f5bbdfdaf8ca1f3273fa8d741f17efe1d
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1833114"
---
# <a name="getting-started-with-barcode-scanners"></a>開始使用條碼掃描器

了解如何從通用 Windows 應用程式與條碼掃描器互動。  本主題提供有關條碼掃描器特定功能的資訊。

## <a name="configuring-your-barcode-scanner"></a>設定您的條碼掃描器
條碼掃描器可以幾種不同模式設定。  請務必針對其用途妥善設定條碼掃描器。  許多條碼掃描器可在**鍵盤 Wedge** 模式中設定，其可使條碼掃描器顯示為 Windows 鍵盤。  這可讓您掃描條碼到不是條碼掃描器感知的應用程式，例如記事本。  當您在此模式中掃描條碼時，從條碼掃描器解碼的資料會插入到您的插入點，宛如您使用鍵盤輸入資料一般。  若要進一步從您的 UWP 應用程式控制條碼掃描器，您必須在非鍵盤 Wedge 模式中設定。

### <a name="usb-barcode-scanner"></a>USB 條碼掃描器
USB 連接的條碼掃描器必須在 **HID POS 掃描器**模式中設定，才能搭配 Windows 內含的條碼掃描器驅動程式使用。 這個驅動程式是發佈到 [**USB-HID**](http://www.usb.org/developers/hidpage/) 的 **HID 銷售點使用量表格**規格的實作。  如需啟用 **HID POS 掃描器**模式的指示操作，請參閱條碼掃描器的文件，或連絡條碼掃描器的製造商。  一旦設定為 **HID POS 掃描器**，您的條碼掃描器會在 **POS 條碼掃描器**節點下的 \[裝置管理員\] 中顯示為 **POS HID 條碼掃描器**。
您的條碼掃描器製造商也可能會有廠商特定驅動程式，其使用 **HID POS 掃描器**以外的模式支援 UWP 條碼掃描器 API。  如果您已經安裝製造商所提供與 UWP 條碼掃描器 API 相容的驅動程式，您可能會在 \[裝置管理員\] 中的 **POS 條碼掃描器**下看到列出的廠商特定裝置。

### <a name="bluetooth-barcode-scanner"></a>藍牙條碼掃描器
藍牙連接的掃描器必須在**序列埠通訊協定 - 簡單序列介面 (SPP SSI)** 模式中設定，以搭配 UWP 條碼掃描器 API 使用。  如需啟用 **SPP-SSI 模式**的指示操作，請參閱條碼掃描器的文件，或連絡條碼掃描器的製造商。  
在您可以使用藍牙條碼掃描器之前，您必須使用 \[設定\] - \[裝置\] - 藍牙和其他裝置 - 新增藍牙或其他裝置來進行配對。  
您可以使用 **Windows.Devices.Enumeration** 命名空間起始和控制配對儀式。  如需詳細資訊，請參閱[**配對裝置**](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

## <a name="working-with-symbologies"></a>使用碼制
[**條碼碼制**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是資料到特定條碼格式的對應。 一些常見的碼制包含 UPC、Code 128、QR 代碼等等。UWP 條碼掃描器 API 可讓應用程式控制掃描器如何處理這些碼制，而不需要手動設定掃描器。 

### <a name="determine-which-symbologies-are-supported"></a>判斷支援的碼制 
因為您的應用程式可能搭配多個製造商的不同條碼掃描器機型使用，您可能想要查詢掃描器以判斷其支援碼制的清單。  如果您的應用程式需要所有掃描器可能都不支援的特定碼制，或者您必須啟用掃描器上手動或以程式設計方式停用的碼制，這非常有用。
一旦您使用 [**BarcodeScanner.FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 取得 [**BarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 物件，呼叫 [**GetSupportedSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 可取得裝置所支援碼制的清單。

### <a name="determine-if-a-specific-symbology-is-supported"></a>判斷是否支援特定碼制
若要判斷掃描器是否支援特定碼制，您可以呼叫  [**IsSymbologySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

### <a name="changing-which-symbologies-are-recognized"></a>變更所辨識的碼制
有時候，您可能想要使用條碼掃描器支援的碼制子集。  這特別適合封鎖您不想在您的應用程式中使用的碼制。 例如，為確保使用者掃描正確的條碼，您可以在取得項目 SKU 時限制掃描到 UPC 或 EAN，以及在取得序號時限制掃描到 Code 128。
一旦您知道掃描器支援的碼制，您可以設定要其辨識的碼制。  這可在您使用 [**ClaimScannerAsyc**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 建立 [**ClaimedBarcodeScanner**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 物件後執行。 您可以呼叫 [**SetActiveSymbologiesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__)，以啟用特定組的碼制，而這些從清單省略的碼制已停用。

### <a name="restricting-scan-data-by-data-length"></a>按長度限制掃描資料
某些碼制的長度可變，例如 Code 39 或 Code 128。  這個碼制的條碼可能位在通常包含特定長度的不同資料附近。 設定特定長度的資料，可以防止無效的掃描。

| 方法    | 描述 |
| :-------- | :---------- |
| [**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | 可讓您設定所要長度範圍的解碼資料，以及掃描器如何處理檢查數字。 |
| [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 可讓您擷取目前的長度以及檢查數字設定。 |

> [!Important] 
> 確認您的條碼掃描器支援使用碼制屬性，方式是先檢查下列屬性：[**SetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) 或 [**GetSymbologyAttributesAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 可讓您擷取目前的長度及檢查數字設定。 :
> - [**IsDecodeLengthSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [**ICheckDigitTransmissionSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [**IsCheckDigitValidationSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)

## <a name="using-software-trigger-with-barcode-scanners"></a>使用軟體觸發器搭配條碼掃描器
### <a name="initiate-scan-from-software"></a>從軟體啟動掃描
如果您在簡報模式中使用條碼掃描器，或者掃描器沒有如相機型條碼掃描器的實體觸發器，它可以從軟體控制掃描的動作。 您可以藉由呼叫 [**StartSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 來起始掃描程序。  
根據 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，掃描器可能只掃描一個條碼然後停止或持續掃描，直到您呼叫 [**StopSoftwareTriggerAsync**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync) 為止。

設定 [**IsDisabledOnDataReceived**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的所需值，來控制解碼條碼時的掃描器行為。

| 值 | 描述 |
| ----- | ----------- |
| True   | 只掃描一個條碼然後停止 |
| False  | 持續掃描條碼而不停止 |


> [!Important]
> 確認您的條碼掃描器支援使用軟體觸發器，首先檢查屬性 [**IsSoftwareTriggerSupported**](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)。
