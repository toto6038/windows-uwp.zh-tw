---
author: QuinnRadich
title: 分割檢視
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: 分割檢視控制項有一個可展開/可摺疊的窗格和內容區域。
label: Split view
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: kimsea
dev-contact: tpaine
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cde4b5d95a0c978faa647fcc108d74874ff52c40
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/07/2018
ms.locfileid: "3663758"
---
# <a name="split-view-control"></a>分割檢視控制項

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

分割檢視控制項可以用來建立任何「抽屜」體驗，讓使用者可以開啟和關閉補充的窗格。 例如，您可以使用 SplitView 建置[主要/詳細資料](master-details.md)模式。

如果您想要使用展開/摺疊按鈕及瀏覽項目清單來建置瀏覽功能表並瀏覽項目清單，請使用 [NavigationView](navigationview.md) 控制項。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/SplitView">開啟應用程式並查看 SplitView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

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

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題
- [瀏覽窗格模式](navigationview.md)
- [清單檢視](lists.md)
- [主要/詳細資料](master-details.md)