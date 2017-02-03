---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: "了解如何處理 AdControl 類別的事件。"
title: "C 中的 AdControl 事件#"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: e25e0f915c0b9b6ec2423d2a95386b45b4502253

---

# <a name="adcontrol-events-in-c"></a>C\ 中的 AdControl 事件# #  


下列範例示範下列 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 事件的基本事件處理常式：[ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx)、[AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) 及 [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx)。 這些範例假設您已將事件處理常式指派至您 XAML 程式碼中的事件。 如需如何執行這項操作的詳細資訊，請參閱 [XAML 屬性範例](xaml-properties-example.md)。

如需處理 C# 中事件的詳細資訊，請參閱[事件與路由事件概觀 (使用 C#/VB/C++ 和 XAML 的通用 Windows 應用程式)](http://msdn.microsoft.com/library/windows/apps/hh758286)。

## <a name="examples"></a>範例

> [!div class="tabbedCodeSnippets"]
[!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MainPage.xaml.cs#EventHandlers)]

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [AdControl 錯誤處理](adcontrol-error-handling.md)
* [RoutedEventArgs 類別](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Dec16_HO2-->


