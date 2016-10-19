---
author: Jwmsft
title: "分割檢視"
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: "分割檢視控制項有一個可展開/可收合的窗格和內容區域。"
label: Split view
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 7fae1477b997508ade92a5bbb977c1d6530a181f

---
# 分割檢視控制項

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

分割檢視控制項有一個可展開/可收合的窗格和內容區域。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn864360"><strong>SplitView 類別 (XAML)</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn919970"><strong>SplitView 物件 (HTML)</strong></a></li>
</ul>

</div>
</div>




 分割檢視的內容區域一律會顯示。 窗格可以展開或收合或維持在開啟狀態，並且可以從 app 視窗的左邊或右邊顯示。 窗格有四種模式︰

-   **Overlay**

    窗格在開啟之前是隱藏的。 開啟窗格時，窗格會與內容區域重疊。

-   **Inline**

    一律顯示窗格，而且不會和內容區域重疊。 窗格和內容區域會劃分螢幕實際可用空間。

-   **CompactOverlay**

    在此模式中，窗格狹窄的一部分一律可見，其寬度剛好可顯示圖示。 預設關閉的窗格的寬度是 48px，可以使用 `CompactPaneLength` 來修改。 如果窗格已開啟，它會與內容區域重疊。

-   **CompactInline**

    在此模式中，窗格狹窄的一部分一律可見，其寬度剛好可顯示圖示。 預設關閉的窗格的寬度是 48px，可以使用 `CompactPaneLength` 來修改。 如果窗格已開啟，它會減少內容可用的空間，使內容向旁邊移動。

## 這是正確的控制項嗎？

分割檢視控制項可以用於[瀏覽窗格](nav-pane.md)。 若要建立這種模式，請新增展開/收合按鈕 (「漢堡」按鈕) 和顯示瀏覽項目的清單檢視。

分割檢視控制項也可以用來建立任何「抽屜」體驗，讓使用者可以開啟和關閉補充的窗格。

## 範例

分割檢視控制項的預設形式是一個基本容器。 以下是 Microsoft Edge app 使用 SplitView 顯示「中心」的範例。

![Microsoft Edge 分割檢視範例](images/split_view_Edge.png)



## 相關主題


* [瀏覽窗格模式](nav-pane.md)
* [清單檢視](lists.md)
 

 



<!--HONumber=Aug16_HO3-->


