---
Description: Edge-based animations show or hide UI that originates from the edge of the screen.
title: UWP app 中以邊緣為基礎的 UI 動畫
ms.assetid: 5A8F73B1-F4F6-424b-9EDF-A9766C5DEAE8
label: Motion--edge-based UI
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e07ac565fe2e223b2fb33573ad083edfdfbc888a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8460931"
---
# <a name="edge-based-ui-animations"></a>以邊緣為基礎的 UI 動畫





以邊緣為基礎的動畫會顯示或隱藏從畫面邊緣出現的 UI。 可透過使用者或 app 來起始顯示和隱藏動作。 這個 UI 可以與 app 重疊，或者成為主 app 表面的一部分。 如果 UI 是應用程式表面的一部分，則可能需要重新調整應用程式其餘部分的大小以容納它。

> **重要 API**: [**EdgeUIThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh702324)


## <a name="dos-and-donts"></a>可行與禁止事項


-   使用邊緣 UI 動畫來顯示或隱藏無法延伸到畫面中的自訂訊息或錯誤列。
-   使用面板動畫來顯示會在畫面佔用很大距離的 UI，例如工作窗格或自訂螢幕小鍵盤。
-   從 UI 所處的相同邊緣滑入該 UI。
-   從 UI 進入的相同邊緣滑出該 UI。
-   如果應用程式的內容需要重新調整大小以回應滑入或滑出的 UI，可使用淡入/淡出動畫來重新調整大小。
    -   如果 UI 是滑入的，請在邊緣 UI 或面板動畫之後使用淡入/淡出動畫。
    -   如果 UI 是滑出的，請與邊緣 UI 或面板動畫同時使用淡入/淡出動畫。
-   不要將這些動畫套用到通知。 通知應該放置在以邊緣為基礎的 UI 內。
-   不要將邊緣 UI 或面板動畫套用到任何不是位於畫面邊緣的 UI 容器或控制項。 這些動畫只能用來顯示、重新調整大小及關閉位於畫面邊緣的 UI。 若要移動其他類型的 UI，可使用重新定位動畫。

    ![說明何時使用邊緣 UI 或面板動畫和重新定位。](images/edgevsreposition.png)

## <a name="related-articles"></a>相關文章


**適用於開發人員**
* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓以邊緣為基礎的 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649428)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**EdgeUIThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh702324)
* [**PaneThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh969160)
* [讓淡入/淡出產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649429)
* [讓重新定位產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649434)

 

 




