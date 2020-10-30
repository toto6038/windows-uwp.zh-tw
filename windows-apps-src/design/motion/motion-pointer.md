---
description: 使用指標動畫為使用者提供在項目上點選時的視覺化回饋。
title: 指標按一下動畫
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1f2d6367ebd8e87cbb32f7a12a829dba7fa38ace
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034181"
---
# <a name="pointer-click-animations"></a>指標按一下動畫



使用指標動畫為使用者提供在項目上點選時的視覺化回饋。 第一次點選項目時，會播放稍微縮小並傾斜已按下項目的指標向下動畫。 當使用者放開指標時，則會播放將項目還原至其原始位置的指標向上動畫。


> **重要 API** : [**PointerUpThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)、 [**PointerDownThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)


## <a name="dos-and-donts"></a>可行與禁止事項

-   如果使用指標向上動畫，當使用者放開指標時，將立即觸發動畫。 這樣可為使用者提供立即回應，告知他們動作已被辨識，但是這樣會造成點選觸發的動作 (如瀏覽到新的頁面) 比較慢回應。

## <a name="related-articles"></a>相關文章

* [動畫總覽](./xaml-animation.md)
* [讓指標按一下產生動畫效果](/previous-versions/windows/apps/jj649432(v=win.10))
* [快速入門：使用動畫庫讓 UI 產生動畫效果](/previous-versions/windows/apps/hh452703(v=win.10))
* [**PointerUpThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerUpThemeAnimation)
* [**PointerDownThemeAnimation 類別**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation)

 

 
