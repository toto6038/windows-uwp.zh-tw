---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "了解如何在 HTML 中指派 **AdControl** 屬性。"
title: "HTML 屬性範例"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 1898ed2ccad74ac33c5130c627363e0a9daebceb


---

# HTML 屬性範例




以下範例示範如何在 HTML 中指派 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 屬性。 **applicationId** 和 **adUnitId** 是必要的屬性。 其他屬性和事件是選擇性的。 如果您未提供選擇性屬性的值，該控制項將會使用預設值，這會建立與 App 使用者體驗一致的廣告。

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

 



<!--HONumber=Aug16_HO3-->


