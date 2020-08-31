---
title: 使用軟體觸發器
description: 瞭解如何使用非同步軟體觸發程式以程式設計方式控制條碼掃描流程。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: baa556958750b4f88997746786a9b5fe92e601b4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168512"
---
# <a name="use-a-software-trigger"></a>使用軟體觸發器

如果您在簡報模式中使用條碼掃描器，或者掃描器沒有如相機型條碼掃描器的實體觸發器，它可以從軟體控制掃描的動作。 您可以藉由呼叫 [StartSoftwareTriggerAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.startsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StartSoftwareTriggerAsync) 來起始掃描程序。

根據 [IsDisabledOnDataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的值，掃描器可能只掃描一個條碼然後停止或持續掃描，直到您呼叫 [StopSoftwareTriggerAsync](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.stopsoftwaretriggerasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_StopSoftwareTriggerAsync) 為止。

設定 [IsDisabledOnDataReceived](/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdisabledondatareceived#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDisabledOnDataReceived) 的所需值，來控制解碼條碼時的掃描器行為。

| 值 | 描述 |
| ----- | ----------- |
| True   | 只掃描一個條碼然後停止 |
| False  | 持續掃描條碼而不停止 |


> [!Important]
> 確認您的條碼掃描器支援使用軟體觸發器，首先檢查屬性 [IsSoftwareTriggerSupported](/uwp/api/windows.devices.pointofservice.barcodescannercapabilities.issoftwaretriggersupported#Windows_Devices_PointOfService_BarcodeScannerCapabilities_IsSoftwareTriggerSupported)。

下列範例示範如何使用軟體觸發程式來起始掃描，此觸發程式會在掃描一個條碼之後停止掃描：

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