---
author: mijacobs
description: "作為語言的視覺表示，印刷格式的主要工作是清楚傳達。 其樣式絕對不能阻礙這項目標。 但是，印刷格式也具有配置元件的重要角色，不僅在設計的密度與複雜性方面具有強大的效果，對於該設計的使用者經驗，也是如此。"
title: "印刷樣式"
ms.assetid: ca35f78a-e4da-423d-9f5b-75896e0b8f82
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: e13e9c8b559c16676628ab6e77ddad019a4c22e0

---

# <a name="typography"></a>印刷樣式

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

如同語言的視覺呈現，印刷格式的主要任務就是清晰呈現。 其樣式絕對不能阻礙這項目標。 但是，印刷格式也具有配置元件的重要角色，不僅在設計的密度與複雜性方面具有強大的效果，對於該設計的使用者經驗，也是如此。

## <a name="typeface"></a>字樣

我們選取了 Segoe UI，用於所有的 Microsoft 數位設計。 Segoe UI 提供各種字元，且依設計可讓不同的大小和像素密度維持最佳易讀性。 它提供了清晰、簡潔開放的美感，可補強系統的內容。

![Segoe UI 字型的範例文字](images/segoe-sample.png)

## <a name="weights"></a>粗細

我們在處理印刷格式時，會考量到簡易性和效率。 我們選擇使用一個字樣、最小的粗細與大小，以及明確的階層。 定位和對齊方式會遵循指定語言的預設樣式。 英文的順序為由左至右、由上至下。 文字與影像之間的關係是清楚明瞭的。

![顯示支援的字型粗細。 細、Semilight、標準、Semibold 和粗體](images/weights.png)

## <a name="line-spacing"></a>行距

![行距為 125% 的範例](images/line-spacing.png)

行距應以字型大小的 125% 計算，必要時四捨五入至最接近的四的倍數。 以 15px Segoe UI 為例，15px 的 125% 為 18.75px。 建議採用四捨五入，並將行高設定為 20px，以維持 4px 格線。 這可確保良好的閱讀經驗，並確保變音符號有足夠的空間。 如需特定範例，請參閱下面的「字體坡形」一節。

在較小的字體上堆疊較大的字體，從較大字體的最後一個基準線到較小字體的第一個基準線的距離，應等於較大字體的行高。

![說明大型字體堆疊在小型字體上的方式](images/line-height-stacking.png)

在 XAML 中，這會透過堆疊兩個 [TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 並設定適當的邊界來完成。

```xaml
<StackPanel Width="200">
    <!-- Setting a bottom margin of 3px on the header
         puts the baseline of the body text exactly 24px
         below the baseline of the header. 24px is the
         recommended line height for a 20px font size,
         which is what's set in SubtitleTextBlockStyle.
         The bottom margin will be different for
         different font size pairings. -->
    <TextBlock
        Style="{StaticResource SubtitleTextBlockStyle}"
        Margin="0,0,0,3"
        Text="Header text" />
    <TextBlock
        Style="{StaticResource BodyTextBlockStyle}"
        TextWrapping="Wrap"
        Text="This line of text should be positioned where the above header would have wrapped." />
</StackPanel>
```


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<h2>字型間距調整和追蹤</h2>

Segoe 是很人性化的字樣，具有柔和、易讀的外觀，並且採用以手寫文字為基礎的開放格式。 若要確保最佳的易讀性，並維持其人性化的完整性、字型間距調整和追蹤設定必須具有特定值。

字元間距調整應設為「計量」，而追蹤應設定為 "0"。
  </div>
  <div class="side-by-side-content-right">
<h2>文字和字母間距</h2>

類似於字型間距調整和追蹤，字距和字母間距會使用特定的設定，以確保最佳易讀性和人性化的完整性。

字距預設一律為 100%，字母間距則應設為 "0"。
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
![字元間距調整與追蹤之間的差異](images/kerning-tracking.png)  
  </div>
  <div class="side-by-side-content-right">
![文字與字母間距之間的差異。](images/word-letter.png) 
  </div>
</div>
</div>


>[!NOTE]
>在 XAML 文字控制項中使用 [Typogrphy.Kerning](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.documents.typography.kerning.aspx) to control kerning and [FontStretch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.control.fontstretch.aspx) 控制追蹤。 根據預設，Typography.Kerning 會設定為 “true” 而 FontStretch 會設定為 “Normal”，這些是建議的值。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<h2>對齊方式</h2>

一般而言，我們建議字體的視覺元素和欄應靠左對齊。 在大部分情況下，這個靠左和不齊右方法可提供一致的內容錨定與統一的配置。 
  </div>
  <div class="side-by-side-content-right">
<h2>行尾</h2>

當印刷樣式未定位為靠左和不齊右時，請嘗試確保行尾整齊並避免斷字。
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
![顯示靠左文字。](images/alignment.png)  
  </div>
  <div class="side-by-side-content-right">
![顯示整齊行尾。](images/line-endings.png) 
  </div>
</div>
</div>


## <a name="paragraphs"></a>段落

若要提供對齊的欄邊緣，應以跳過一行且不縮排來表示段落。

![在段落之間顯示一整行的空間](images/paragraphs.png)

## <a name="character-count"></a>字元計數

如果一行太短，眼睛就會被迫要頻繁地來回游移，而破壞閱讀的節奏。 可能的話，每行 50-60 個字母是最易於閱讀的。

Segoe 提供各種字元，且依設計可讓不同的大小以及高與低的像素密度都能維持最佳易讀性。 在文字欄行中使用最佳的字母數目，可確保應用程式中的良好易讀性。

一行太長會使眼睛疲累，且使用者可能會不知道讀到哪了。 一行太短則會迫使讀者的目光頻繁地來回游移，而造成眼睛疲勞。

![顯示行的長度不同的 3 個段落](images/character-count.png)

## <a name="hanging-text-alignment"></a>凸排文字對齊方式

圖示與文字的水平對齊可用多種方式來處理，取決於圖示的大小和文字的數量。 當文字 (單行或多行) 符合圖示的高度時，文字應垂直置中。

一旦文字的高度超出圖示的高度時，第一行文字應垂直對齊，而其他文字應在下方自然排列。 在使用端點、上升幅度和下降幅度高度較大的字元時，應謹守相同的對齊方式指導方針。

![顯示數個圖示和文字配對](images/hanging-text-alignment.png)

>[!NOTE]
>XAML 的 [TextBlock.TextLineBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.textlinebounds.aspx)屬性提供端點高度及基準字型計量的存取權。 它可以用來以視覺方式垂直置中或頂端對齊類型。

## <a name="clipping-and-ellipses"></a>裁剪和省略符號

依預設剪裁 — 假設文字會自動換行，除非紅線另有指定。 使用非換行文字時，我們建議使用裁剪，而不要使用省略符號。 剪裁可在容器的邊緣、裝置的邊緣和捲軸的邊緣等位置執行。

例外狀況 — 對於未明確定義的容器 （例如，沒有區別的背景色彩），可將非換行文字個別設定成使用省略符號 ”…”。

![顯示使用文字裁剪的裝置框架](images/clipping.png)

## <a name="type-ramp"></a>字體坡形
字體坡形從標題到本文建立了一個重要的設計關係，確保不同層級之間有一個清楚明瞭的階層。 此階層建立了一個結構，可讓使用者輕鬆地瀏覽已撰寫的通訊。

<div class="uwpd-image-with-caption">
    <img src="images/type-ramp.png" alt="Shows the type ramp" />
    <div>所有大小皆以有效像素為單位。 如需詳細資訊，請參閱 [UWP 應用程式設計簡介](../layout/design-and-ui-intro.md)。</div>
</div>

>[!NOTE]
>多數坡形層級可作為 XAML [靜態資源](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_type_ramp)取得，其依照 `*TextBlockStyle` 命名規範 (例如：`HeaderTextBlockStyle`)。


<div class="microsoft-internal-note">
目前不包含 SubtitleAlt、BaseAlt 及 CaptionAlt。 您可以在自己專屬的應用程式中建立樣式，方法是依照上方連結中的程式碼片段。 另請注意 XAML 目前並不恰好符合行高。
</div>


## <a name="primary-and-secondary-text"></a>主要和次要文字

若要在字體坡形以外建立其他階層，請將次要文字設為 60% 的不透明度。 在[佈景主題調色盤](color.md#color-theming)中，您可以使用 BaseMedium。 主要文字一律應為 100% 的不透明或 BaseHigh。

<!-- Need new images
![Two phone apps using SubtitleAlt](images/type-ramp-example-2.png)
Recommended use of SubtitleAlt. Also note the primary and secondary text usage in list items.

![Two phone apps using CaptionAlt](images/type-ramp-example-1.png)
Recommended use of CaptionAlt.
-->

## <a name="all-caps-titles"></a>全部大寫的標題

特定頁面標題應採用「全部大寫」的格式，以新增另一個階層維度。 這些標題應使用 BaseAlt，且字元間距應設為 em 的千分之 75。 這種處理方式也可用來輔助應用程式瀏覽。

不過，在某些語言中，特定的名稱在採用大寫時會變更其意義，因此，任何以名稱或使用者輸入為基礎的頁面標題均「*不應*」轉換為全部大寫。


<!-- Need new images
![Shows several apps where they should and should not use all caps](images/all-caps.png)
Green shows where all caps should be used. Red shows where it should not.
-->

## <a name="dos-and-donts"></a>可行與禁止事項
* 讓大部分文字使用 Body
* 標題在空間有限使用基準
* 納入 SubtitleAlt，以藉由強調最上層內容來建立對比和階層
* 對於長字串或任何主要動作，請不要使用 Caption
* 如果文字需要自動換行，請不要使用標題或副標題
* 請不要在同一頁面上結合 Subtitle 和 SubtitleAlt


## <a name="related-articles"></a>相關文章

* [文字控制項](../controls-and-patterns/text-controls.md)
* [字型](fonts.md)
* [Segoe MDL2 圖示](segoe-ui-symbol-font.md)



<!--HONumber=Dec16_HO2-->


