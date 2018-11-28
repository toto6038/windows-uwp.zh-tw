---
title: PointOfService 裝置共用
description: 與他人共用 PointOfService 周邊設備
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 53dc22b2aa35b5e69854f6fb489ff6a454c73bf6
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7853393"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 裝置共用

了解如何與其他之後，多部電腦依賴共用周邊設備，而不是專用的周邊裝置連接到每個電腦的環境中的電腦共用網路或已連線的藍牙周邊裝置。

## <a name="device-sharing"></a>裝置共用

網路和藍牙連接的 PointOfService 周邊裝置，通常是環境 wheere 中多個用戶端裝置共用相同的周邊裝置一天當中。  忙碌的零售或食品服務環境中的能力，若要附加到周邊設備的用戶端裝置中任何的延遲會影響的效率關聯可以關閉與客戶的交易和移至下一節。 收據印表機做為廚房印表機來將客戶的訂單的詳細資訊傳輸到的準備廚房其中快速服務餐廳案例中會有多個接受來自客戶訂單的用戶端裝置。  順序完成後每個用戶端裝置應該可以宣告共用的印表機，並立即列印廚房的順序。

在這些環境中，請務必要完全連裝置物件 [ **dispose**應用程式，讓另一個可以宣告相同的裝置。

處置 '使用' 區塊的結尾 pos 印表機

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


藉由明確呼叫 dispose （） 處置 pos 印表機

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>所使用的 API 方法 

+ [BarcodeScanner.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter.Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
