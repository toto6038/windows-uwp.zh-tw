---
author: DBirtolo
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: "校正感應器"
description: "以磁力儀 (指南針、傾角計及方向感應器) 為基礎的裝置感應器會因為環境因素而需要校正。"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 8b669d7f939da9ee93e5a49d2f6434d5573e23c0

---
# 校正感應器

\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要 API **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

以磁力儀 (指南針、傾角計及方向感應器) 為基礎的裝置感應器會因為環境因素而需要校正。 [**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 列舉可以在您的裝置需要校正時協助判斷可採取的步驟。

## 校正磁力儀的時機

[**MagnetometerAccuracy**](https://msdn.microsoft.com/library/windows/apps/Dn297552) 列舉有四個值，可以協助您判斷正在執行 app 的裝置是否需要校正。 如果裝置需要校正，您應該讓使用者知道需要進行校正。 但是，您不應太過頻繁地提示使用者進行校正。 我們建議每 10 分鐘不要超過 1 次。

| 值 | 描述 |-----------------|-------------------| | **未知** | 感應器驅動程式無法報告目前的準確度。 這不一定代表裝置未校正。 如果傳回**未知**，您的 app 就必須決定要採取的最佳動作。 如果您的 app 依賴準確的感應器讀數，您可能會想提示使用者校正該裝置。 | | **不可靠** | 目前磁力儀的準確度很低。 第一次傳回此值時，app 就應該一律要求使用者進行校正。 | | **大約** | 此資料對部分應用程式而言已經足夠準確。 如果虛擬實境 app 只需了解使用者是否已將裝置往上/下或往左/右移動，則該 app 不需要校正也能繼續執行。 需要絕對方向的 app (例如導航 app，該 app 需要知道您正在行駛的方向，才能為您提供方向) 就需要提出校正的要求。 | | **高** | 資料很精確。 不需要進行校正，即使需要知道絕對方向的 app (例如，擴增實境 app 或導航 app) 也一樣。 |

## 校正磁力計的方式

這個簡短影片提供如何校正磁力計的概觀。<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/727bd0e3-9116-49c3-8af6-0b4339324b71/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">開發人員短片：感應器校正</iframe>






<!--HONumber=Aug16_HO3-->


