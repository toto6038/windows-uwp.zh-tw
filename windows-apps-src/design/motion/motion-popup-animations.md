---
Description: 使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。
title: UWP app 中的快顯 UI 動畫
ms.assetid: 4E9025CE-FC90-4d4c-9DE6-EC6B6F2AD9DF
label: Motion--Pop-up animations
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d79c369e14236b827bdc18aba6c74349528728b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635173"
---
# <a name="pop-up-ui-animations"></a>快顯 UI 動畫



使用快顯動畫顯示和隱藏飛出視窗的快顯 UI 或自訂快顯的 UI 元素。 快顯元素是顯示在應用程式內容之上的容器，如果使用者點選或按一下快顯元素以外的地方則會關閉。

> **重要的 Api**:[**PopInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210383)， [ **PopupThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh969172)


## <a name="dos-and-donts"></a>可行與禁止事項


-   使用快顯動畫顯示或隱藏非應用程式頁面本身一部分的自訂快顯 UI 元素。 Windows 提供的常用控制項已經內建這些動畫。
-   請勿對工具提示或對話方塊使用快顯動畫。
-   請勿使用快顯動畫顯示或隱藏應用程式主要內容中的 UI；僅使用快顯動畫顯示或隱藏會顯示在應用程式主要內容最上方的快顯容器。

## <a name="related-articles"></a>相關文章

* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [以動畫顯示快顯 UI](https://msdn.microsoft.com/library/windows/apps/xaml/jj649433)
* [快速入門：以動畫顯示您使用程式庫動畫的 UI](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PopInThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210383)
* [**PopOutThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/br210391)
* [**PopupThemeTransition 類別**](https://msdn.microsoft.com/library/windows/apps/hh969172)

 

 




