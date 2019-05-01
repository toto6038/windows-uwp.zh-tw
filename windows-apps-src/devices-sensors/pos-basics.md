---
title: 開始使用服務點
description: 本文包含有關如何開始使用 PointOfService UWP API 的資訊
ms.date: 06/13/2018
ms.topic: article
keywords: windows 10, uwp, 服務點, pos
ms.localizationpriority: medium
ms.openlocfilehash: f1e58dbf8bae22df0652ada6ff8aafff6d30e8aa
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63817396"
---
# <a name="getting-started-with-point-of-service"></a>開始使用服務點

## <a name="pointofservice-basics"></a>PointOfService 基本概念

本節包含所有服務點所有裝置類別常見的主題。

|主題 |描述 |
|------|------------|
| [功能宣告](pos-basics-capability.md)      | 了解如何將 **pointOfService** 功能新增至您的應用程式資訊清單。  要使用 Windows.Devices.PointOfService 命名空間需要具備這個功能。  |
| [列舉的裝置](pos-basics-enumerating.md)        | 了解如何定義裝置選取器，這是用來查詢可供系統使用的裝置，並且使用此選取器列舉服務點裝置。  |
| [建立裝置物件](pos-basics-deviceobject.md)  | 了解如何建立 PointOfService 裝置物件，讓您能夠存取周邊的唯讀內容，並宣告專屬使用周邊。 |
| [宣告和啟用 ](pos-basics-claim.md)  | 了解如何保留 PointOfService 專用週邊設備，並啟用的 I/O 作業。  |
| [與其他人共用週邊設備](pos-basics-sharing.md) | 了解如何與其中多部電腦依賴共用的週邊設備，而不是專用的週邊設備連接到每一部電腦環境中的其他電腦共用網路或連線的藍牙周邊設備。
| [PointOfService 端對端](pos-get-started.md)  | 這是如何利用上述範例的 PointOfService 週邊設備與互動的端對端範例。 |
|

## <a name="see-also"></a>另請參閱

| 主題   | 描述 |
|:--------|:------------|
| [應用程式發佈](../publish/distribute-lob-apps-to-enterprises.md) | 深入了解散發給企業客戶的應用程式的選項。 |
| [應用程式生命週期](../launch-resume/app-lifecycle.md) | 了解 UWP 應用程式生命週期，以及當 Windows 啟動，暫止，並繼續您的應用程式時，會發生什麼事。 |
| [應用程式資源](../app-resources/index.md) | 了解如何撰寫，套件，並取用您的應用程式的字串、 影像和檔案資源。 |
| [資料繫結](../data-binding/index.md) | 了解如何使用資料繫結至您的應用程式 UI 中顯示的資料。 |
| [裝置列舉型別](enumerate-devices.md) | 了解使用進階列舉技術來尋找您的週邊設備。|
| [版本的自適性應用程式](../debug-test-perf/version-adaptive-apps.md) | 了解如何設計您的應用程式，讓它在多個版本的 Windows 10 上執行。|
|


## <a name="sample-code"></a>範例程式碼
+ [條碼掃描器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
+ [現金隱藏式選單的範例]( https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CashDrawer)
+ [列顯示範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LineDisplay)
+ [磁條讀取器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MagneticStripeReader)
+ [POSPrinter 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PosPrinter)

