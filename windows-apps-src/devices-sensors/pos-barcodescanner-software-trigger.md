---
title: 使用軟體觸發器
description: 了解如何控制掃描軟體的動作。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 2b6f06ea66767a1bcdd7e20fa05aa7af275eb892
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645523"
---
# <a name="use-a-software-trigger"></a>使用軟體觸發器

如果您在簡報模式中使用條碼掃描器，或者掃描器沒有如相機型條碼掃描器的實體觸發器，它可以從軟體控制掃描的動作。 您可以藉由呼叫起始掃描程序[StartSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync)。

值而定[IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)掃描器可能會掃描只有一個條碼，然後停止或掃描持續直到您呼叫[StopSoftwareTriggerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync)。

設定所需的值[IsDisabledOnDataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived)來控制時已解碼的條碼掃描器行為。

| 值 | 描述 |
| ----- | ----------- |
| True   | 只掃描一個條碼然後停止 |
| False  | 持續掃描條碼而不停止 |


> [!Important]
> 確認條碼掃描器支援軟體觸發程序使用，藉由先檢查屬性[IsSoftwareTriggerSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)。

下列範例示範如何初始化使用的軟體觸發程序，將會停止掃描之後它會掃描一個條碼掃描：

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