---
author: mijacobs
Description: Use pop-up animations to show and hide pop-up UI for flyouts or custom pop-up UI elements. Pop-up elements are containers that appear over the app's content and are dismissed if the user taps or clicks outside of the pop-up element.
title: UWP app 中的快顯 UI 動畫
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a304df30986c904f19cc2401c9a1fb468514f6f
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6666876"
---
# <a name="pop-up-ui-animations"></a>快顯 UI 動畫



使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。

> **重要 API**: [**PopInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210383)、[**PopupThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>可行與禁止事項


-   使用快顯動畫顯示或隱藏非應用程式頁面本身一部分的自訂快顯 UI 元素。 Windows 提供的常用控制項已經內建這些動畫。
-   請勿對工具提示或對話方塊使用快顯動畫。
-   請勿使用快顯動畫顯示或隱藏應用程式主要內容中的 UI；僅使用快顯動畫顯示或隱藏會顯示在應用程式主要內容最上方的快顯容器。

## <a name="related-articles"></a>相關文章

* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓快顯 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PopInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**PopOutThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**PopupThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




