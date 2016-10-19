---
author: payzer
title: "如何在螢幕邊緣繪製 UI"
description: 
translationtype: Human Translation
ms.sourcegitcommit: b5961d3266a031ab09a9da63319e9883cf050789
ms.openlocfilehash: cddde27a17e897ab8a68bbed099e532a8cd48f07

---

# 如何在螢幕邊緣繪製 UI   
根據預設，應用程式會針對電視安全區域，將邊框放在檢視區的邊緣 (如需詳細資訊，請參閱[針對 Xbox 和電視進行設計](../input-and-devices/designing-for-tv.md#tv-safe-area))。 

我們建議您關閉此功能，並繪製到螢幕邊緣。 您可以在應用程式啟動時，加入下列程式碼來繪製到螢幕邊緣：
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 應用程式不需要擔心這個問題。 系統會一律將您的應用程式繪製到螢幕邊緣。

## 另請參閱
- [Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Aug16_HO3-->


