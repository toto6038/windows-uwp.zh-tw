---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: "了解如何處理 AdControl 類別的事件。"
title: "JavaScript 中的 AdControl 事件"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 廣告, AdControl, 活動, JavaScript"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 17d087b8174b057501795d76c5dc29d5f91b1050
ms.lasthandoff: 02/07/2017

---

# <a name="adcontrol-events-in-javascript"></a>JavaScript 中的 AdControl 事件

下列範例示範下列 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 事件的基本事件處理常式：[ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx)、[AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) 及 [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx)。 這些範例假設您已將事件處理常式指派至您 HTML 標記中的事件。 如需如何執行這項操作的詳細資訊，請參閱 [HTML 屬性範例](html-properties-example.md)。

在 JavaScript 中，**AdControl** 事件必須包含在 [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函式中。 如需在 JavaScript 中處理事件的詳細資訊，請參閱[撰寫基本應用程式的程式碼 (HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers)。

## <a name="examples"></a>範例

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#EventHandlers)]

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
* [AdControl 錯誤處理](adcontrol-error-handling.md)
* [RoutedEventArgs 類別](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 

