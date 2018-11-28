---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: 校正感應器
description: 以磁力儀 (指南針、傾角計及方向感應器) 為基礎的裝置感應器會因為環境因素而需要校正。
ms.date: 03/22/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7a93d59a00630c240e74049a9fd98d50f285b0dd
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964537"
---
# <a name="calibrate-sensors"></a>校正感應器


**重要 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

以磁力儀 (指南針、傾角計及方向感應器) 為基礎的裝置感應器會因為環境因素而需要校正。 [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 列舉可以在您的裝置需要校正時協助判斷可採取的步驟。

## <a name="when-to-calibrate-the-magnetometer"></a>校正磁力儀的時機

[**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 列舉有四個值，可以協助您判斷正在執行 app 的裝置是否需要校正。 如果裝置需要校正，您應該讓使用者知道需要進行校正。 但是，您不應太過頻繁地提示使用者進行校正。 我們建議每 10 分鐘不要超過 1 次。

| 值           | 描述    |
| ----------------- | ------------------- |
| **不明**     | 感應器驅動程式無法報告目前的準確度。 這不一定代表裝置未校正。 如果傳回**未知**，您的 app 就必須決定要採取的最佳動作。 如果您的 App 依賴準確的感應器讀數，您可能會想提示使用者校正該裝置。 |
| **不可靠**  | 目前磁力儀的準確度很低。 第一次傳回此值時，App 就應該一律要求使用者進行校正。 |
| **大約** | 此資料對部分應用程式而言已經足夠準確。 如果虛擬實境 App 只需了解使用者是否已將裝置往上/下或往左/右移動，則該 App 不需要校正也能繼續執行。 需要絕對方向的 app (例如導航 app，該 app 需要知道您正在行駛的方向，才能為您提供方向) 就需要提出校正的要求。 |
| **高**        | 資料很精確。 不需要進行校正，即使需要知道絕對方向的 App (例如，擴增實境 App 或導航 App) 也一樣。 |