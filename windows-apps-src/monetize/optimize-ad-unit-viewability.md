---
description: 深入了解改善您的廣告單元的可見性的方法。
title: 最佳化您的廣告單元的可見性
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, ads, advertising, guidelines, viewability, 廣告, 指導方針, 可見性
ms.localizationpriority: medium
ms.openlocfilehash: 87e21f4e98c58f79f397c369891212eccb196c18
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8748918"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>最佳化您的廣告單元的可見性

[廣告效能報告](../publish/advertising-performance-report.md)包括您的廣告單元的可見性計量。 可見性是重要的衡量標準，因為廣告業越來越重視可檢視的廣告曝光，而非只是提供廣告曝光。 廣告客戶傾向於競標可檢視的廣告曝光，因為它們已增加機會讓使用者看到他們的廣告。  

為與 IAB 可見性指導方針一致，橫幅的廣告曝光若符合下列條件會被視為可檢視：

* 像素需求：廣告中大於或等於 50% 的像素是在 App 的可檢視空間。
* 時間需求：符合像素需求的時間大於或等於一個連續一秒、張貼廣告轉譯。

影片廣告曝光若符合下列條件會計算為可檢視：

* 像素需求：廣告中大於或等於 50% 的像素是在 App 的可檢視部分。
* 時間需求：符合像素需求的影片，且播放兩個連續秒的張貼廣告轉譯。

可見性使用下列公式計算：

**可見性 = [已檢視的廣告曝光] * 100 / [總廣告曝光]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>改善廣告單元可見性的指導方針

若要最佳化您的廣告單元的可檢視廣告曝光，請依照下列指導方針。

1. **測量效能**&nbsp;&nbsp;使用[廣告效能報告](../publish/advertising-performance-report.md)來檢閱每個您的廣告單元的可見性計量。
2.  **適當定位您的廣告控制項**&nbsp;&nbsp;一般而言，廣告最常可被看到的位置通常是摺疊上方 (也就是在使用者不需要捲動即可看到的網頁底部可見部分)。 顯示在網頁頂端的廣告並非可檢視。 可考慮檢查您的網頁的每個元素，以得知您可以如何最佳化 App 中可檢視的空間。 請確定您的廣告控制項不是放在應用程式的背景中。
3.  **管理網頁長度**&nbsp;&nbsp;較短的網頁內容可有更高的可見性。 如果您的 App 需要較長的網頁，實作無限捲動。
4.  **固定廣告位置**&nbsp;&nbsp;請確定您的廣告維持在固定位置，即使使用者捲動網頁。 這會協助您取得較高的檢視時間，以及增加您的可見率。
5.  **設定透明度**&nbsp;&nbsp;請確保包含廣告控制項的容器透明度為 100%。
6.  **實作延遲載入**&nbsp;&nbsp;直到使用者捲動至包含廣告控制項的檢視，請勿載入您的廣告。 這樣可確保廣告顯示較長時間供使用者檢視。
7.  **使用各種廣告大小進行實驗**&nbsp;&nbsp;選取幾個廣告大小，並使用[廣告效能報告](../publish/advertising-performance-report.md)為每一個測量可見性。 請挑選一個最適合您的。
8.  **重新瀏覽 App 設計**&nbsp;&nbsp;改進您的 App 網頁設計，以縮短網頁載入時間。 使用者傾向於留在載入速度較快的頁面上。
