---
author: Jwmsft
title: "分割檢視"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "分割檢視控制項有一個可展開/可摺疊的窗格和內容區域。"
label: Split view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.openlocfilehash: 126fab3db9a0728626289788757f576648a43856
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/22/2017
---
# <a name="split-view-control"></a>分割檢視控制項

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

分割檢視控制項有一個可展開/可摺疊的窗格和內容區域。

> **重要 API**：[SplitView 類別](https://msdn.microsoft.com/library/windows/apps/dn864360)

以下是 Microsoft Edge app 使用 SplitView 顯示其「中心」的範例。

![Microsoft Edge 分割檢視範例](images/split_view_Edge.png)


 分割檢視的內容區域一律會顯示。 窗格可以展開或摺疊或維持在開啟狀態，並且可以從應用程式視窗的左邊或右邊顯示。 窗格有四種模式︰

-   **Overlay**

    窗格在開啟之前是隱藏的。 開啟窗格時，窗格會與內容區域重疊。

-   **Inline**

    一律顯示窗格，而且不會和內容區域重疊。 窗格和內容區域會劃分螢幕實際可用空間。

-   **CompactOverlay**

    在此模式中，窗格狹窄的一部分一律可見，其寬度剛好可顯示圖示。 預設關閉的窗格的寬度是 48px，可以使用 `CompactPaneLength` 來修改。 如果窗格已開啟，它會與內容區域重疊。

-   **CompactInline**

    在此模式中，窗格狹窄的一部分一律可見，其寬度剛好可顯示圖示。 預設關閉的窗格的寬度是 48px，可以使用 `CompactPaneLength` 來修改。 如果窗格已開啟，它會減少內容可用的空間，使內容向旁邊移動。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

分割檢視控制項可以用於[瀏覽窗格](navigationview.md)。 若要建立這種模式，請新增展開/摺疊按鈕 (「漢堡」按鈕) 和顯示瀏覽項目的清單檢視。

分割檢視控制項也可以用來建立任何「抽屜」體驗，讓使用者可以開啟和關閉補充的窗格。

## <a name="create-a-split-view"></a>建立分割檢視

以下是 SplitView 控制項，在內容的旁邊有一個以內嵌方式開啟的窗格。
```xaml
<SplitView IsPaneOpen="True"
           DisplayMode="Inline"
           OpenPaneLength="296">
    <SplitView.Pane>
        <TextBlock Text="Pane"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </SplitView.Pane>

    <Grid>
        <TextBlock Text="Content"
                   FontSize="24"
                   VerticalAlignment="Center"
                   HorizontalAlignment="Center"/>
    </Grid>
</SplitView>
```



## <a name="related-topics"></a>相關主題
* [瀏覽窗格模式](navigationview.md)
* [清單檢視](lists.md)
 

 
