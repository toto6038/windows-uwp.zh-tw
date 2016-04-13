---
title: 分割檢視
ms.assetid: E9E4537F-1160-4183-9A83-26602FCFDC9A
description: 分割檢視控制項有一個可展開/可收合的窗格和內容區域。
label: 分割檢視
template: detail.hbs
---

# 分割檢視控制項的指導方針


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**SplitView 類別 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn864360)
-   [**SplitView 物件 (HTML)**](https://msdn.microsoft.com/library/windows/apps/dn919970)

分割檢視控制項有一個可展開/可收合的窗格和內容區域。 內容區域一律會顯示。 窗格可以展開或收合或維持在開啟狀態，並且可以從應用程式視窗的左邊或右邊顯示。 窗格有三種模式：

-   **重疊**

    窗格在開啟之前是隱藏的。 開啟窗格時，窗格會與內容區域重疊。

-   **內嵌**

    一律顯示窗格，而且不會和內容區域重疊。 窗格和內容區域會劃分螢幕實際可用空間。

-   **精簡**

    窗格一律以只足以顯示圖示 (通常寬度為 48 epx) 之寬度的模式顯示。 窗格和內容區域會劃分螢幕實際可用空間。 雖然標準精簡模式不會與內容區域重疊，但是可以轉變成更寬的窗格以顯示更多內容，但將會與內容區域重疊。

## <span id="Is_this_the_right_control_"> </span> <span id="is_this_the_right_control_"> </span> <span id="IS_THIS_THE_RIGHT_CONTROL_"> </span>這是正確的控制項嗎？


分割檢視控制項可以用於[瀏覽窗格模式](nav-pane.md)。 若要建立這種模式，請新增展開/收合按鈕 (「漢堡」按鈕) 和清單檢視到分割檢視控制項。

## <span id="Examples"> </span> <span id="examples"> </span> <span id="EXAMPLES"> </span>範例


分割檢視控制項的預設形式是一個基本容器。 新增按鈕和清單檢視之後，分割檢視控制項就可以做為瀏覽功能表使用。 以下是做為瀏覽功能表使用的分割檢視在已展開及精簡模式下的範例。

![重疊模式與精簡模式之分割檢視功能表的範例](images/controls-splitview-menu01.png)
## <span id="Recommendations"> </span> <span id="recommendations"> </span> <span id="RECOMMENDATIONS"> </span>建議


-   為瀏覽功能表使用分割檢視時，我們建議在窗格中置入可以存取 app 其他範圍的瀏覽控制項。 將窗格用於提供一致使用者體驗的瀏覽。 此外，這個功能表實作可協助使用者熟悉 app 的所有部分、提供快速存取 app 首頁的功能，而且可以吸引使用者多探索 app 的其他部分。

\[本文包含通用 Windows 平台 (UWP) app 與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## <span id="related_topics"> </span>相關主題


* [瀏覽窗格模式](nav-pane.md)
* [清單檢視](lists.md)
 

 






<!--HONumber=Mar16_HO1-->


