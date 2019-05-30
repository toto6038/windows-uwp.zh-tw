---
Description: 使用指標動畫為使用者提供在項目上點選時的視覺化回饋。
title: UWP app 中的指標按一下動畫
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ee87a62796ed51d09d44cabecd0b49873145bd90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366078"
---
# <a name="pointer-click-animations"></a>指標按一下動畫



使用指標動畫為使用者提供在項目上點選時的視覺化回饋。 第一次點選項目時，會播放稍微縮小並傾斜已按下項目的指標向下動畫。 當使用者放開指標時，則會播放將項目還原至其原始位置的指標向上動畫。


> **重要的 Api**:[**PointerUpThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)， [ **PointerDownThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>可行與禁止事項

-   如果使用指標向上動畫，當使用者放開指標時，將立即觸發動畫。 這樣可為使用者提供立即回應，告知他們動作已被辨識，但是這樣會造成點選觸發的動作 (如瀏覽到新的頁面) 比較慢回應。

## <a name="related-articles"></a>相關文章

* [動畫概觀](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)
* [建立動畫的指標按一下](https://docs.microsoft.com/previous-versions/windows/apps/jj649432(v=win.10))
* [快速入門：以動畫顯示您使用程式庫動畫的 UI](https://docs.microsoft.com/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation 類別**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 




