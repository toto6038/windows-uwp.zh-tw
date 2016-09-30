---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "了解如何在 HTML 中指派 **AdControl** 屬性。"
title: "HTML 屬性範例"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 53b9afdcdc697e3ed508ef6e5ed5d684bdf5aa69


---

# HTML 屬性範例


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下範例顯示如何在 HTML 中指派 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 屬性。 **applicationId** 和 **adUnitId** 是必要的屬性。 其他屬性和事件是選擇性的。 如果您未提供選擇性屬性的值，該控制項將會使用預設值，這會建立與 App 使用者體驗一致的廣告。

最後五行是將函式登錄到 **AdControl** 事件的範例。 您只能登錄已在您的 JavaScript 程式碼中定義的函式。

這些值為範例。 您會在您的程式碼中設定這些函式和屬性的值，來使它們能適合您的 App。

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


