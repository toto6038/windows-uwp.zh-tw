---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "了解如何以程式設計方式，使用 JavaScript 建立 **AdControl**。"
title: "使用 JavaScript 建立 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 68bc124aea079bc60fa22e1e6a038caf95fe765c


---

# 使用 JavaScript 建立 AdControl




此範例說明如何以程式設計方式，使用 JavaScript 建立 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)。

## 適用於 AdControl 的 HTML div

在顯示廣告的 HTML 頁面上，**AdControl** 必須具有 **div**。 下列程式碼提供此類 **div** 的範例。

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## 適用於建立 AdControl 的 JavaScript

下列範例假設您透過識別碼 **myAd**，在 HTML 中使用現有的 **div**。

在 **app.onactivated** 函式中，具現化 **AdControl**。

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

這些值為範例。 您會在您的程式碼中設定適合您 App 的這些函式和屬性的值。

若您使用此程式碼但未看到廣告，則可嘗試將 **position:relative** 的屬性，插入於包含 **AdControl** 的 **div**。 這將會覆寫 **IFrame** 的預設設定。 將會正確顯示廣告，除非由於此屬性的值致使未顯示這些廣告。 請注意，可能會有長達 30 分鐘的時間無法使用新廣告單位。

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 

 



<!--HONumber=Aug16_HO3-->


