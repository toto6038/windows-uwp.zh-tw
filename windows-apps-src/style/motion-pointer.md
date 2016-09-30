---
author: mijacobs
Description: "使用指標動畫為使用者提供在項目上點選時的視覺化回饋。"
title: "UWP app 中的指標按一下動畫"
ms.assetid: EEB10A2C-629A-4705-8468-4D019D74DDFF
label: Motion--Pointer animations
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 24175bb7c822ceb53af5e1ec70bf83f701fd1cce

---

# 指標按一下動畫

使用指標動畫為使用者提供在項目上點選時的視覺化回饋。 第一次點選項目時，會播放稍微縮小並傾斜已按下項目的指標向下動畫。 當使用者放開指標時，則會播放將項目還原至其原始位置的指標向上動畫。




**重要 API**

-   [**PointerUpThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969168)
-   [**PointerDownThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969164)



## 可行與禁止事項


-   如果使用指標向上動畫，當使用者放開指標時，將立即觸發動畫。 這樣可為使用者提供立即回應，告知他們動作已被辨識，但是這樣會造成點選觸發的動作 (如瀏覽到新的頁面) 比較慢回應。

## 相關文章

**適用於開發人員 (XAML)**
* [動畫概觀](https://msdn.microsoft.com/library/windows/apps/mt187350)
* [讓指標按一下產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/jj649432)
* [快速入門：使用動畫庫讓 UI 產生動畫效果](https://msdn.microsoft.com/library/windows/apps/xaml/hh452703)
* [**PointerUpThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969168)
* [**PointerDownThemeAnimation 類別**](https://msdn.microsoft.com/library/windows/apps/hh969164)

 

 







<!--HONumber=Jun16_HO4-->


