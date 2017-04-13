---
author: Jwmsft
Description: "提供最上層瀏覽同時節省螢幕空間。"
title: "瀏覽窗格的指導方針"
ms.assetid: 8FB52F5E-8E72-4604-9222-0B0EC6A97541
label: Nav pane
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 2d48a92d5af75f8543f7b69ac59865e51bd334ee
ms.sourcegitcommit: d1d53f5100edffe3f3ee57b853dc8cd1568fe7a2
translationtype: HT
---
# <a name="nav-panes"></a>瀏覽窗格

瀏覽窗格是一種瀏覽模式，它可以有多個最上層瀏覽項目，同時節省螢幕空間。 瀏覽窗格廣泛使用於行動裝置應用程式，但也適用於較大的螢幕。 以重疊模式使用時，窗格會維持摺疊直到使用者按下按鈕，這適用於較小的螢幕。 以停駐模式使用時，窗格會維持開啟，如果有足夠的螢幕實際可用空間時，這可以提高使用效率。

![瀏覽窗格的範例](images/navHero.png)


**重要 API**

* [**SplitView 類別**](https://msdn.microsoft.com/library/windows/apps/dn864360)

## <a name="is-this-the-right-pattern"></a>這是正確的模式嗎？

瀏覽窗格適用於：

-   具有許多相似類型最上層瀏覽項目的 app。 例如，具有像是美式足球、棒球、籃球、足球等類別的運動 app。
-   提供跨應用程式一致的瀏覽體驗。 瀏覽窗格中應該只包含瀏覽元素，不包含動作。
-   最上層瀏覽類別的中到高數字 (5-10+)。
-   保留螢幕實際可用空間 (以重疊方式)。
-   不常存取的瀏覽項目。 (以重疊方式)。

## <a name="building-a-nav-pane"></a>建置瀏覽窗格

瀏覽窗格模式是由瀏覽類別窗格、內容區域以及開啟或關閉窗格的選用按鈕所組成。 建置瀏覽窗格的最簡單方式是使用[分割檢視控制項](split-view.md)，它內建空的窗格與一律會顯示的內容區域。

若要嘗試實作此模式的程式碼，請從 GitHub 下載 [XAML 瀏覽解決方案](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlNavigation)。


### <a name="pane"></a>窗格

瀏覽類別的標頭會在窗格中。 應用程式設定和帳戶管理的進入點也會在窗格中 (若適用的話)。 瀏覽標頭通常是供使用者選擇的項目清單。

![瀏覽窗格的窗格範例](images/nav_pane_expanded.png)

### <a name="content-area"></a>內容區域

內容區域是顯示已選取瀏覽位置資訊的地方。 它能夠包含個別元素或其他子層級的瀏覽。

### <a name="button"></a>按鈕

顯示時，按鈕可讓使用者開啟和關閉窗格。 按鈕會保持在固定位置顯示，並不會隨窗格移動。 我們建議將該按鈕放在您 app 的左上角。 瀏覽窗格按鈕以三個堆疊的水平線段顯示，通常稱為「漢堡」按鈕。

![瀏覽窗格按鈕範例](images/nav_button.png)

該按鈕通常與一個文字字串關聯。 在 app 的最上層，應用程式標題可以顯示在按鈕旁。 在 app 的較下層，文字字串可能會是使用者目前所在之頁面的標題。

## <a name="nav-pane-variations"></a>瀏覽窗格變化

瀏覽窗格有三種模式：重疊、精簡以及內嵌。 重疊時會視需要摺疊及展開。 當精簡時，窗格會一律顯示為可以展開的窄條格式。 內嵌窗格預設會維持開啟。

### <a name="overlay"></a>重疊

-   重疊可以在任何大小螢幕上以縱向或橫向方式使用。 在預設 (摺疊) 狀態中，重疊不會佔用實際空間，因為只會顯示按鈕。
-   可在節省螢幕空間的情況下提供隨選瀏覽功能。 適合手機和平板手機上的應用程式。
-   根據預設會隱藏窗格，只顯示按鈕。
-   按 [瀏覽窗格] 按鈕可開啟和關閉重疊。
-   展開狀態只是暫時性的，會在進行選擇時、使用 [上一頁] 按鈕時，或是當使用者點選窗格外部時關閉。
-   重疊會覆蓋在內容上，且不會使內容自動重排。

### <a name="compact"></a>精簡

-   精簡模式可以指定為 `CompactOverlay` \(這會在開啟時重疊內容\) 或者 `CompactInline` \(這會將內容移出\)。 建議使用 CompactOverlay。
-   精簡窗格使用少量螢幕實際可用空間，也同時提供所選位置的一些指示。
-   這個模式很適合中型螢幕，例如平板電腦。
-   根據預設，窗格為關閉，只顯示瀏覽圖示和按鈕。
-   按下瀏覽窗格按鈕會開啟和關閉窗格，像是根據指定的顯示模式重疊或內嵌的行為。
-   清單圖示上應顯示選取狀態，以強調使用者在瀏覽樹狀結構中的所在位置。

### <a name="inline"></a>內嵌

-   瀏覽窗格會維持開啟。 這個模式適合用於較大的螢幕。
-   支援在窗格之間來回拖放的情況。
-   這個狀態不需要瀏覽窗格按鈕。 如果使用按鈕，則會將內容區域推出螢幕之外，而該區域中的內容將會自動重排。
-   清單項目上應顯示選取狀態，以強調使用者在瀏覽樹狀結構中的所在位置。

## <a name="adaptability"></a>適應性

若要最大化各種裝置上的可用性，我們建議使用[中斷點](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)及根據其 app 的視窗寬度調整瀏覽窗格的模式。
-   小型視窗
   -   寬度小於或等於 640px。
   -   瀏覽窗格應該是在重疊模式中，預設已關閉。
-   中型視窗
   -   寬度大於 640px 且小於或等於 1007px。
   -   瀏覽窗格中應該在窄條模式中，預設已關閉。
-   大型視窗
   -   寬度大於 1007px。
   -   瀏覽窗格中應該在停駐模式中，預設已開啟。

## <a name="tailoring"></a>自訂

若要最佳化您 app 的 [10 英呎體驗](http://go.microsoft.com/fwlink/?LinkId=760736)，請考慮藉由改變其瀏覽元素的視覺外觀，自訂瀏覽窗格。 根據互動的內容，引起使用者注意已選取的瀏覽項目或是焦點瀏覽項目，可能更為重要。 針對 10ft 體驗 (其中遊戲台是最常見的輸入裝置)，確保使用者可以輕易地追蹤螢幕上目前焦點項目的位置更是特別重要。

![自訂瀏覽窗格項目的範例](images/nav_item_states.png)

## <a name="related-topics"></a>相關主題

* [分割檢視控制項](split-view.md)
* [主要/詳細資料](master-details.md)
* [瀏覽基本知識](https://msdn.microsoft.com/library/windows/apps/dn958438)
 

 
