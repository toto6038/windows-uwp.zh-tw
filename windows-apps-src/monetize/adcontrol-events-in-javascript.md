---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: 了解如何處理 AdControl 類別的事件。
title: JavaScript 中的 AdControl 事件

---

# JavaScript 中的 AdControl 事件


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下範例示範如何處理 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別的事件。 這些範例假設您已經將事件處理器指派給 **AdControl** 事件。 如需如何執行這項操作的詳細資訊，請參閱 [HTML 屬性範例](html-properties-example.md)。

在 JavaScript 中，**AdControl** 事件必須包含在 [MarkSupportedForProcessing](http://msdn.microsoft.com/en-us/library/windows/apps/Hh967819.aspx) 函式中。 如需在 JavaScript 中處理事件的詳細資訊，請參閱[撰寫基本應用程式的程式碼 (HTML)](https://msdn.microsoft.com/en-us/library/windows/apps/hh780660.aspx#adding-event-handlers)。

## 範例

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [AdControl 錯誤處理](adcontrol-error-handling.md)
* [RoutedEventArgs 類別](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 


<!--HONumber=May16_HO2-->


