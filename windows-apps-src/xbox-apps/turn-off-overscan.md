---
author: payzer
title: "如何關閉溢出掃描"
description: 
area: Xbox
ms.sourcegitcommit: 32a875348debac9aec9f5a26bc4e7e0af2a0a5b4
ms.openlocfilehash: abd06e78364ff32cc10d733e33b153b854dbc467

---

# 如何在螢幕邊緣繪製 UI   
根據預設，應用程式在檢視區邊緣會放置框線。 這是為了處理電視安全區域。 如需詳細資訊，請參閱[針對 Xbox 與電視設計](http://go.microsoft.com/fwlink/?LinkID=760736#tv-safe-area)。  建議您關閉此功能，並繪製到螢幕邊緣。 您可以在應用程式啟動時，加入下列程式碼來繪製到螢幕邊緣：
   
`Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);`
   
注意︰C++/DirectX 應用程式不需要擔心這個問題。 系統會一律將您的應用程式繪製到螢幕邊緣。



<!--HONumber=Jun16_HO4-->


