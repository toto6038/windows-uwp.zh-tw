---
author: TerryWarwick
title: 開始使用服務點
description: 本文包含有關如何開始使用 PointOfService UWP API 的資訊
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, point of service, pos, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: a0583adbcef9e45dfe0b2e56e03ce7e0451ac5bb
ms.sourcegitcommit: ce45a2bc5ca6794e97d188166172f58590e2e434
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2018
ms.locfileid: "1983541"
---
# <a name="getting-started-with-point-of-service"></a>開始使用服務點

本節包含所有服務點所有裝置類別常見的主題。

|主題 |描述 |
|------|------------|
| [功能宣告](pos-basics-capability.md)      | 了解如何將 **pointOfService** 功能新增至您的應用程式資訊清單。  要使用 Windows.Devices.PointOfService 命名空間需要具備這個功能。  |
| [列舉裝置](pos-basics-enumerating.md)        | 了解如何定義裝置選取器，這是用來查詢可供系統使用的裝置，並且使用此選取器列舉服務點裝置。  |
| [建立裝置物件](pos-basics-deviceobject.md)  | 了解如何建立 PointOfService 裝置物件，讓您能夠存取周邊的唯讀內容，並宣告專屬使用周邊。 |
| [宣告專屬使用裝置 ](pos-basics-claim.md)  | 了解如何使用 PointOfService 宣告模型保留 PointOfService 周邊以專屬使用，同時允許相同電腦上的其他應用程式在需要專屬使用時存取 PointOfService 周邊。  |
|

## <a name="see-also"></a>另請參閱
[開始使用 Windows.Devices.PointOfService](pos-get-started.md)


## <a name="sample-code"></a>範例程式碼
+ [條碼掃描器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [收銀機範例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行顯示範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁條讀取器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POS 印表機範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

