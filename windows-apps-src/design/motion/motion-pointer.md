---
Description: 使用指標動畫為使用者提供在項目上點選時的視覺化回饋。
title: UWP app 中的指標按一下動畫
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f1abd71cda8358e3f7e2fe36091f9c42f05bcb00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57663793"
---
# <a name="pointer-click-animations"></a>指標按一下動畫



使用指標動畫為使用者提供在項目上點選時的視覺化回饋。 第一次點選項目時，會播放稍微縮小並傾斜已按下項目的指標向下動畫。 當使用者放開指標時，則會播放將項目還原至其原始位置的指標向上動畫。


> **重要的 Api**:[**PointerUpThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969168)， [ **PointerDownThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969164)


## <a name="dos-and-donts"></a>可行與禁止事項

-   如果使用指標向上動畫，當使用者放開指標時，將立即觸發動畫。 這樣可為使用者提供立即回應，告知他們動作已被辨識，但是這樣會造成點選觸發的動作 (如瀏覽到新的頁面) 比較慢回應。

## <a name="related-articles"></a>相關文章

* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [建立動畫的指標按一下](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [快速入門：以動畫顯示您使用程式庫動畫的 UI](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 




