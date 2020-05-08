---
Description: 使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。
title: 快顯 UI 動畫
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 78adc32dccc66b98e413796d9cfe20b2a158261e
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970533"
---
# <a name="pop-up-ui-animations"></a>快顯 UI 動畫



使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。

> **重要 API**: [**PopInThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)、[**PopupThemeTransition 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)


## <a name="dos-and-donts"></a>可行與禁止事項


-   使用快顯動畫顯示或隱藏非應用程式頁面本身一部分的自訂快顯 UI 元素。 Windows 提供的常用控制項已經內建這些動畫。
-   請勿對工具提示或對話方塊使用快顯動畫。
-   請勿使用快顯動畫顯示或隱藏應用程式主要內容中的 UI；僅使用快顯動畫顯示或隱藏會顯示在應用程式主要內容最上方的快顯容器。

## <a name="related-articles"></a>相關文章

* [動畫概觀](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [讓快顯 UI 產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/jj649433(v=win.10))
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PopInThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopInThemeAnimation)
* [**PopOutThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopOutThemeAnimation)
* [**PopupThemeTransition 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PopupThemeTransition)

 

 




