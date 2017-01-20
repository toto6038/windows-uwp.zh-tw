---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "了解如何在 HTML 中指派 **AdControl** 屬性。"
title: "AdControl HTML 屬性範例"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 741cf19ee0310c84d1a85f4a1e82b353d88d1b9e

---

# <a name="adcontrol-html-properties-example"></a>AdControl HTML 屬性範例

以下範例示範如何在 HTML 中指派 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 屬性。 **applicationId** 和 **adUnitId** 是必要的屬性。 其他屬性和事件是選擇性的。 如果您未提供選擇性屬性的值，該控制項將會使用預設值，這會建立與應用程式使用者體驗一致的廣告。

最後五行是將函式登錄到 **AdControl** 事件的範例。 您只能登錄已在您的 JavaScript 程式碼中定義的函式。

這些值為範例。 您會在您的程式碼中設定這些函式和屬性的值，來使它們能適合您的應用程式。

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl"
    data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                       adUnitId: '10865270',
                       isAutoRefreshEnabled: false,
                       onAdRefreshed: myAdRefreshed,
                       onErrorOccurred: myAdError,
                       onEngagedChanged: myAdEngagedChanged,
                       onPointerDown: myPointerDown,
                       onPointerUp: myPointerUp }" />
```

## <a name="related-topics"></a>相關主題

* [HTML 5 和 JavaScript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [GitHub 上的廣告範例](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


