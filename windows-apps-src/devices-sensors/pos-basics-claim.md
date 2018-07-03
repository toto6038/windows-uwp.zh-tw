---
author: TerryWarwick
title: PointOfService 裝置宣告模型
description: 深入了解 PointOfService 宣告模型
ms.author: jken
ms.date: 06/4/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, point of service, pos, 服務點
ms.localizationpriority: medium
ms.openlocfilehash: 202234530945e55ef9c0d0fb68cf9ca83d2e15c3
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983727"
---
# <a name="point-of-service-device-claim-model"></a>服務點裝置宣告模型

## <a name="claiming-a-device-for-exclusive-use"></a>宣告專屬使用裝置

在您成功建立 PointOfService 裝置物件之後，您必須先使用適合裝置類型的宣告方法來宣告它，您才能使用裝置來進行輸入或輸出。  宣告授與應用程式專屬存取許多裝置的功能，以確保某個應用程式不會干擾其他應用程式使用裝置。  一次只有一個應用程式可以宣告 PointOfService 裝置專屬使用。 

這個範例顯示如何在您成功建立條碼掃描器物件之後，宣告條碼掃描器裝置。

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

> [!Warning]
> 宣告可能在以下情況遺失：
> 1. 其他應用程式已要求宣告相同裝置，而您的應用程式並未發出 **RetainDevice** 以回應 **ReleaseDeviceRequested** 事件。  (如需詳細資訊，請參閱以下[宣告交涉](#Claim-negotiation)。)
> 2. 您的應用程式已暫停，它會導致裝置物件關閉，因此宣告不再有效。 (如需詳細資訊，請參閱[裝置物件週期](pos-basics-deviceobject.md#device-object-lifecycle)。

### <a name="apis-used-for-claiming"></a>用於宣告的 API

|裝置|宣告 |
|-|:-|
|BarcodeScanner | [ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync) | 
|CashDrawer | [ClaimDrawerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.claimdrawerasync) | 
|LineDisplay | [ClaimAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.claimasync) |
|MagneticStripeReader | [ClaimReaderAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.claimreaderasync) | 
|PosPrinter | [ClaimPrinterAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.claimprinterasync) | 
|

## <a name="claim-negotiation"></a>宣告交涉

由於 Windows 是一個多工環境，它可讓相同電腦上的多個應用程式以合作方式要求存取周邊。  PointOfService API 提供交涉模型，允許多個應用程式共用連接到電腦的周邊。

當相同的電腦上的第二個應用程式要求宣告 PointOfService 周邊，而其他應用程式已經宣告該周邊，則會發佈 **ReleaseDeviceRequested** 事件通知。 如果應用程式目前使用裝置以避免遺失宣告，使用具主動式宣告的應用程式必須透過呼叫 **RetainDevice** 來回應事件通知。 

如果具主動式宣告的應用程式未立即回應 **RetainDevice**，則可能應用程式已暫停，或者不需要裝置，而且宣告撤銷，並提供給新的應用程式。 

此範例示範如何在其他應用程式要求釋放已宣告的條碼掃描器之後保留該裝置。  

```Csharp
claimedBarcodeScanner.ReleaseDeviceRequested += claimedBarcodeScanner_ReleaseDeviceRequested;

void claimedBarcodeScanner_ReleaseDeviceRequested(object sender, ClaimedBarcodeScanner myScanner)
{
    // Retain exclusive access to the device
    myScanner.RetainDevice();  
}
```
### <a name="apis-used-for-claim-negotiation"></a>用於宣告交涉的 API

|已宣告的裝置|發行通知| 保留裝置 |
|-|:-|:-|
|ClaimedBarcodeScanner | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.retaindevice)
|ClaimedCashDrawer | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedcashdrawer.retaindevice)
|ClaimedLineDisplay | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedMagneticStripeReader | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedlinedisplay.retaindevice)
|ClaimedPosPrinter | [ReleaseDeviceRequested](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.releasedevicerequested) | [RetainDevice](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedposprinter.retaindevice)
|
