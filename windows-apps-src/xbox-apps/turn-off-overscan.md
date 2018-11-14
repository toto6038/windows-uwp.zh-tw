---
author: payzer
title: 如何在螢幕邊緣繪製 UI
description: 若要關閉畫面有效呈現區域的自動縮放功能。
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: 32932845db47ed47b7e80f68cf4e424ba97e85c0
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6659375"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>如何在螢幕邊緣繪製 UI   
根據預設，應用程式會針對電視安全區域，將邊框放在檢視區的邊緣 (如需詳細資訊，請參閱[針對 Xbox 和電視進行設計](../design/devices/designing-for-tv.md#tv-safe-area))。 

我們建議您關閉此功能，並繪製到螢幕邊緣。 您可以在應用程式啟動時，加入下列程式碼來繪製到螢幕邊緣：
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 應用程式不需要擔心這個問題。 系統會一律將您的應用程式繪製到螢幕邊緣。

## <a name="see-also"></a>另請參閱
- [Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)
