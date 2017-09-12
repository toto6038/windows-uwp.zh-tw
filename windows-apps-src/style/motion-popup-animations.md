---
author: mijacobs
Description: "使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。"
title: "UWP 應用程式中的快顯 UI 動畫"
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c91e5cd3d4bad1b29d070f4750beb3dd95b3c5dc
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
# <a name="pop-up-ui-animations"></a>快顯 UI 動畫

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**PopInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210383)</li>
<li>[**PopupThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh969172)</li>
</ul>
</div>


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

 

 




