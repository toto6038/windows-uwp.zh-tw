---
title: PointOfService 裝置共用
description: 瞭解如何在多部電腦依賴共用周邊的環境中，與其他電腦共用網路或 Bluetooth 連接的週邊設備。
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 5416628b88a070c7bd4f361f9f438fe690951d34
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054158"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 裝置共用

瞭解如何在多部電腦依賴共用周邊的環境中（而不是連接到每部電腦的專用週邊設備），與其他電腦共用網路或 Bluetooth 連接的週邊設備。

## <a name="device-sharing"></a>裝置共用

網路和藍牙連線 PointOfService 周邊通常用於環境中 wheere 多個用戶端裝置會在一天內共用相同的周邊。  在忙碌的零售或食物服務環境中，用戶端裝置連接到周邊的任何延遲時間，都會影響到關聯可以關閉客戶的交易並移至下一個的效率。 在使用回條印表機作為廚房印表機的快速服務餐廳案例中，若要將客戶訂單的詳細資料傳送至廚房以進行準備，則會有多個用戶端裝置會向客戶取得訂單。  當訂單完成時，每個用戶端裝置應該都能宣告共用印表機，並立即列印廚房的順序。

在這些環境中，應用程式必須完全 **處置** 裝置物件，才能讓另一個物件宣告相同的裝置。

在 ' using ' 區塊結尾處置 PosPrinter

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


明確呼叫 Dispose ( # A1 來處置 PosPrinter

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>使用的 API 方法 

+ [BarcodeScanner。 Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer。 Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay。 Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader。 Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter。 Dispose](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]
