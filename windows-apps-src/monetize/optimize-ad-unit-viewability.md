---
description: 瞭解將您的 ad 單位 viewability 優化的指導方針，以及如何使用「廣告效能報表」來測量 viewability 計量。
title: 最佳化您的廣告單元的可見性
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, ads, advertising, guidelines, viewability, 廣告, 指導方針, 可見性
ms.localizationpriority: medium
ms.openlocfilehash: 1653651c41128d359fcacddf0c0d7d408e798d07
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942788"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>最佳化您的廣告單元的可見性

>[!WARNING]
> 從2020年6月1日起，適用于 Windows UWP 應用程式的 Microsoft Ad 營收平臺將會關閉。 [深入了解](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

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

1. **測量效能** &nbsp; &nbsp;使用「[廣告效能報表](../publish/advertising-performance-report.md)」來檢查每個 ad 單位的 viewability 計量。
2.  **適當地** &nbsp; &nbsp; 放置您的 ad 控制項一般來說，廣告的最大可見位置是在 (折迭的正上方，也就是使用者可以看到的頁面可見部分底部，而不需要滾動) 。 顯示在網頁頂端的廣告並非可檢視。 可考慮檢查您的網頁的每個元素，以得知您可以如何最佳化 App 中可檢視的空間。 請確定您的廣告控制項不是放在應用程式的背景中。
3.  **管理頁面長度** &nbsp; &nbsp;較短的頁面內容會導致較高的 viewability。 如果您的 App 需要較長的網頁，實作無限捲動。
4.  **修正廣告位置** &nbsp; &nbsp;確定您的廣告即使在使用者滾動時仍保持固定位置。 這會協助您取得較高的檢視時間，以及增加您的可見率。
5.  **設定不透明度** &nbsp; &nbsp;確定包含 ad 控制項的容器不透明度為100%。
6.  **執行消極式載入** &nbsp; &nbsp;請不要載入您的廣告，直到使用者進入包含廣告控制項的觀點為止。 這樣可確保廣告顯示較長時間供使用者檢視。
7.  **使用各種廣告大小** &nbsp; &nbsp; 進行實驗選取幾個 ad 大小，並使用「[廣告效能報表](../publish/advertising-performance-report.md)」來測量每個大小的 viewability。 請挑選一個最適合您的。
8.  重新流覽**應用程式設計** &nbsp; &nbsp;改進您的應用程式頁面設計，以減少頁面載入時間。 使用者傾向於留在載入速度較快的頁面上。
