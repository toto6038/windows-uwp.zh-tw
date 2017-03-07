---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "了解如何以程式設計方式，使用 JavaScript 建立 **AdControl**。"
title: "使用 JavaScript 建立 AdControl"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, 做廣告, AdControl, JavaScript"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d2ceafb9781ca2b9cd640e65d9bb420f0bf37928
ms.lasthandoff: 02/07/2017

---

# <a name="create-an-adcontrol-in-javascript"></a>使用 JavaScript 建立 AdControl




本文中的範例示範如何以程式設計方式，使用 JavaScript 建立 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)。 本文假設您已新增專案使用 **AdControl** 的必要參考。 如需詳細資訊 (包含在 HTML 標註而非 JavaScript 中建立和初始化 **AdControl** 的詳細逐步解說)，請參閱 [HTML 5 和 JavaScript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。

## <a name="html-div-for-an-adcontrol"></a>適用於 AdControl 的 HTML div

在顯示廣告的 HTML 頁面上，**AdControl** 必須具有 **div**。 下列程式碼提供此類 **div** 的範例。

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## <a name="javascript-for-creating-an-adcontrol"></a>適用於建立 AdControl 的 JavaScript

下列範例假設您透過識別碼 **myAd**，在 HTML 中使用現有的 **div**。

在 **app.onactivated** 函式中，具現化 **AdControl**。

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

此範例假設您已宣告名為 **myAdError**、**myAdRefreshed** 和 **myAdEngagedChanged** 的事件處理常式方法。

>**注意**&nbsp;&nbsp;此範例中所顯示的 *applicationId* 和 *adUnitId* 值是[測試模式值](test-mode-values.md)。 您必須先從 Windows 開發人員中心[將這些值取代為實際值](set-up-ad-units-in-your-app.md)，再提交應用程式以進行提交。

若您使用此程式碼但未看到廣告，則可嘗試將 **position:relative** 的屬性，插入於包含 **AdControl** 的 **div**。 這將會覆寫 **IFrame** 的預設設定。 將會正確顯示廣告，除非由於此屬性的值致使未顯示這些廣告。 請注意，可能會有長達 30 分鐘的時間無法使用新廣告單位。

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 

 

