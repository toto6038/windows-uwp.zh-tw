---
title: 服務點基本概念
description: 本文包含有關如何開始使用 PointOfService UWP API 的資訊
ms.date: 12/3/2019
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4749b66cc5ce2593aead75d65993f70106da7c8b
ms.sourcegitcommit: 2d709ddcc31f52d2a4ace1134aea45057d99a615
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782572"
---
# <a name="point-of-service-basics"></a>服務點基本概念

本節包含所有服務點所有裝置類別常見的主題。

|主題 |說明 |
|------|------------|
| [功能聲明](pos-basics-capability.md)      | 了解如何將 **pointOfService** 功能新增至您的應用程式資訊清單。  要使用 Windows.Devices.PointOfService 命名空間需要具備這個功能。  |
| [列舉裝置](pos-basics-enumerating.md)        | 了解如何定義裝置選取器，這是用來查詢可供系統使用的裝置，並且使用此選取器列舉服務點裝置。  |
| [建立裝置物件](pos-basics-deviceobject.md)  | 了解如何建立 PointOfService 裝置物件，讓您能夠存取周邊的唯讀內容，並宣告專屬使用周邊。 |
| [宣告並啟用](pos-basics-claim.md)  | 瞭解如何保留 PointOfService 外設以供獨佔使用，並啟用 i/o 作業。  |
| [與其他人共用週邊設備](pos-basics-sharing.md) | 瞭解如何在多部電腦依賴共用週邊設備的環境中，與其他電腦共用網路或藍牙連線的週邊設備，而不是連接到每部電腦的專用週邊設備。
| [PointOfService 端對端](pos-get-started.md)  | 這是一種端對端範例，說明如何使用上述範例與 PointOfService 外設互動。 |
|

## <a name="see-also"></a>請參閱

| 主題   | 說明 |
|:--------|:------------|
| [應用程式散發](../publish/distribute-lob-apps-to-enterprises.md) | 深入瞭解將您的應用程式散發給企業客戶的選項。 |
| [應用程式生命週期](../launch-resume/app-lifecycle.md) | 瞭解 UWP 應用程式的生命週期，以及 Windows 啟動、暫停和繼續您的應用程式時，會發生什麼事。 |
| [應用程式資源](../app-resources/index.md) | 瞭解如何撰寫、封裝及使用您的應用程式的字串、影像和檔案資源。 |
| [資料繫結](../data-binding/index.md) | 瞭解如何使用資料系結在應用程式的 UI 中顯示資料。 |
| [裝置列舉](enumerate-devices.md) | 瞭解使用 advanced 列舉技術來尋找您的週邊設備。|
| [版本調適型應用程式](../debug-test-perf/version-adaptive-apps.md) | 瞭解如何設計您的應用程式，使其在多個 Windows 10 版本上執行。|
|


## <a name="sample-code"></a>範例程式碼
+ [條碼掃描器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [銀箱範例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [行顯示範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁條讀取器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)
