---
author: eliotcowley
title: 使用軟體觸發器
description: 了解如何控制的軟體從掃描動作。
ms.author: elcowle
ms.date: 08/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: ddd8ec979cb6d5a72b48b9b8b6a60adb73c35657
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2018
ms.locfileid: "4573719"
---
# <a name="use-a-software-trigger"></a>使用軟體觸發器

如果您在簡報模式中使用條碼掃描器，或者掃描器沒有如相機型條碼掃描器的實體觸發器，它可以從軟體控制掃描的動作。 您可以藉由呼叫 [StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 來起始掃描程序。

根據 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，掃描器可能只掃描一個條碼然後停止或持續掃描，直到您呼叫 [StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync) 為止。

設定 [IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的所需值，來控制解碼條碼時的掃描器行為。

| 值 | 描述 |
| ----- | ----------- |
| True   | 只掃描一個條碼然後停止 |
| False  | 持續掃描條碼而不停止 |


> [!Important]
> 確認您的條碼掃描器支援使用軟體觸發器，首先檢查屬性 [IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)。

下列範例示範如何開始使用的軟體觸發程序，將會停止掃描之後，它會掃描一個條碼掃描︰

```cs
private void SoftwareTrigger(BarcodeScanner barcodeScanner, ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    if (barcodeScanner.Capabilities.IsSoftwareTriggerSupported)
    {
        claimedBarcodeScanner.IsDisabledOnDataReceived = true;
        await claimedBarcodeScanner.StartSoftwareTriggerAsync();
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]