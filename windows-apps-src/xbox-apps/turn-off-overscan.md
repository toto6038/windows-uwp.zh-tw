---
title: 如何在螢幕邊緣繪製 UI
description: 瞭解如何關閉放置於區緣邊緣的預設框線，並將您的 UI 繪製到螢幕邊緣。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: d34da1bbf129358f4549b3a4a04a7c3f84f872ab
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154842"
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
