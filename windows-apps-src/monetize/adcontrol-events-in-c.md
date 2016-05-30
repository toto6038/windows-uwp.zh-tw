---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: 了解如何處理 AdControl 類別的事件。
title: C 中的 AdControl 事件#
---

# C\ 中的 AdControl 事件# #  


\[ 針對 Windows 10 上的 UWP App 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下範例示範如何處理 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別的事件。 這些範例假設您已經將事件處理器指派給 XAML 中的 **AdControl** 事件。 如需如何執行這項操作的詳細資訊，請參閱 [XAML 屬性範例](xaml-properties-example.md)。

如需處理 C# 中事件的詳細資訊，請參閱[事件與路由事件概觀 (使用 C#/VB/C++ 和 XAML 的通用 Windows app)](http://msdn.microsoft.com/library/windows/apps/hh758286)。

## 範例


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [AdControl 錯誤處理](adcontrol-error-handling.md)
* [RoutedEventArgs 類別](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 


<!--HONumber=May16_HO2-->


