---
author: TerryWarwick
title: 使用條碼掃描器碼制
description: 本文內容涵蓋有關條碼掃描器碼制的相關資訊。
ms.author: jken
ms.date: 05/3/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: f6e03d62316a1b842330f39ac958e4471a895815
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874521"
---
# <a name="working-with-symbologies"></a>使用碼制
[條碼碼制](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是資料到特定條碼格式的對應。 一些常見的碼制包含 UPC、Code 128、QR 代碼等等。UWP 條碼掃描器 API 可讓應用程式控制掃描器如何處理這些碼制，而不需要手動設定掃描器。 

## <a name="determine-which-symbologies-are-supported"></a>判斷支援的碼制 
因為您的應用程式可能搭配多個製造商的不同條碼掃描器機型使用，您可能想要查詢掃描器以判斷其支援碼制的清單。  如果您的應用程式需要所有掃描器可能都不支援的特定碼制，或者您必須啟用掃描器上手動或以程式設計方式停用的碼制，這非常有用。
若您取得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 物件的方式是使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync)，呼叫 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 以取得裝置所支援碼制的清單。

## <a name="determine-if-a-specific-symbology-is-supported"></a>判斷是否支援特定碼制
若要判斷掃描器是否支援特定碼制，您可以呼叫 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)

## <a name="changing-which-symbologies-are-recognized"></a>變更所辨識的碼制
有時候，您可能想要使用條碼掃描器支援的碼制子集。  這特別適合封鎖您不想在您的應用程式中使用的碼制。 例如，為確保使用者掃描正確的條碼，您可以在取得項目 SKU 時限制掃描到 UPC 或 EAN，以及在取得序號時限制掃描到 Code 128。
一旦您知道掃描器支援的碼制，您可以設定要其辨識的碼制。  這可在您使用 [ClaimScannerAsyc](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync) 建立 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 物件後執行。 您可以呼叫 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 以啟用特定組的碼制，而這些從清單省略的碼制已停用。

## <a name="restricting-scan-data-by-data-length"></a>按長度限制掃描資料
某些碼制的長度可變，例如 Code 39 或 Code 128。  這個碼制的條碼可能位在通常包含特定長度的不同資料附近。 設定特定長度的資料，可以防止無效的掃描。

| 方法    | 描述 |
| :-------- | :---------- |
| [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetSymbologyAttributesAsync_System_UInt32_Windows_Devices_PointOfService_BarcodeSymbologyAttributes_) | 可讓您設定所要長度範圍的解碼資料，以及掃描器如何處理檢查數字。 |
| [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_) | 可讓您擷取目前的長度以及檢查數字設定。 |

> [!Important] 
> 確認您的條碼掃描器支援使用碼制屬性，做法是先檢查下列屬性： 
> - [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)
> - [ICheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitTransmissionSupported)
> - [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsCheckDigitValidationSupported)
