---
title: PointOfService 裝置共用
description: 與其他人共用 PointOfService 週邊設備
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618943"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 裝置共用

了解如何與其中多部電腦依賴共用的週邊設備，而不是專用的週邊設備連接到每一部電腦環境中的其他電腦共用網路或連線的藍牙周邊設備。

## <a name="device-sharing"></a>共用裝置

網路和連線的 PointOfService 週邊設備通常會使用藍芽環境 wheere 中多個用戶端裝置會共用相同的週邊設備，全天。  在忙碌的零售或餐飲業環境的能力將附加到週邊設備的用戶端裝置的任何延遲情形會影響的效率關聯可以關閉與客戶的交易並移至下一步。 在快速的服務餐廳的情況下，接收印表機做位置廚房印表機準備廚房交給客戶的訂單的詳細資料會從客戶取得訂單的多個用戶端裝置。  一旦完成順序的每個用戶端裝置應該能夠宣告共用的印表機，並立即列印廚房的順序。

在這些環境中，它是很重要的應用程式完全**處置**裝置物件，讓另一個可以宣告在同一部裝置。

處置 PosPrinter 'using' 區塊的結尾

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


藉由明確呼叫 dispose （） 處置 PosPrinter

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>使用 API 方法 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
