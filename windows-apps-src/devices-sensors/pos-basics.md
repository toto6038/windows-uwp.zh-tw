---
title: 開始使用服務點
description: 本文包含有關如何開始使用 PointOfService UWP API 的資訊
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, point of service, pos, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 1b4ff9443c40cf44e171bf898b627de3e2819034
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7976628"
---
# <a name="getting-started-with-point-of-service"></a>開始使用服務點

## <a name="pointofservice-basics"></a>PointOfService 基本知識

本節包含所有服務點所有裝置類別常見的主題。

|主題 |描述 |
|------|------------|
| [功能宣告](pos-basics-capability.md)      | 了解如何將 **pointOfService** 功能新增至您的應用程式資訊清單。  要使用 Windows.Devices.PointOfService 命名空間需要具備這個功能。  |
| [列舉裝置](pos-basics-enumerating.md)        | 了解如何定義裝置選取器，這是用來查詢可供系統使用的裝置，並且使用此選取器列舉服務點裝置。  |
| [建立裝置物件](pos-basics-deviceobject.md)  | 了解如何建立 PointOfService 裝置物件，讓您能夠存取周邊的唯讀內容，並宣告專屬使用周邊。 |
| [宣告和啟用 ](pos-basics-claim.md)  | 了解如何保留 PointOfService 周邊以專屬使用並啟用的 I/O 作業。  |
| [與其他人共用周邊設備](pos-basics-sharing.md) | 了解如何與其他之後，多部電腦依賴共用周邊設備，而不是專用的周邊裝置連接到每個電腦的環境中的電腦共用網路或已連線的藍牙周邊裝置。
| [PointOfService 端對端](pos-get-started.md)  | 這是如何與上述的範例利用 PointOfService 周邊設備互動的端對端範例。 |
|

## <a name="see-also"></a>請參閱

| 主題   | 描述 |
|:--------|:------------|
| [應用程式散發](../publish/distribute-lob-apps-to-enterprises.md) | 深入了解將 app 發佈給企業客戶的選項。 |
| [應用程式生命週期](../launch-resume/app-lifecycle.md) | 了解生命週期的 UWP 應用程式和 Windows 啟動、 暫停，並繼續執行您的應用程式時，會發生什麼事。 |
| [應用程式資源](../app-resources/index.md) | 了解如何製作、 封裝，及使用您的應用程式的字串、 影像和檔案資源。 |
| [資料繫結](../data-binding/index.md) | 了解如何使用資料繫結到您的應用程式 UI 中顯示資料。 |
| [裝置列舉](enumerate-devices.md) | 了解使用進階列舉技術，來尋找您的周邊裝置。|
| [版本調適型應用程式](../debug-test-perf/version-adaptive-apps.md) | 了解如何設計您的應用程式，讓它在多個 Windows 10 版本上執行。|
|


## <a name="sample-code"></a>範例程式碼
+ [條碼掃描器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [收銀機範例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行顯示範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁條讀取器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POS 印表機範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

