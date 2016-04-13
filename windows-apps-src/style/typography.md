---
Description: 如同語言的視覺表示，印刷格式的主要工作會被清除。 其樣式絕不應干擾該目標。 但是，印刷格式也具有配置元件的重要角色，不僅在設計的密度與複雜性方面具有強大的效果，對於該設計的使用者經驗，也是如此。 
title: 印刷格式
ms.assetid: ca35f78a-e4da-423d-9f5b-75896e0b8f82
label: Typography
template: detail.hbs
extraBodyClass: style-typography
brief: As the visual representation of language, typography’s main task is to be clear. Its style should never get in the way of that goal. But typography also has an important role as a layout component—with a powerful effect on the density and complexity of the design—and on the user’s experience of that design.
---

# 適用於 UWP 應用程式的印刷格式

如同語言的視覺表示，印刷格式的主要工作會被清除。 其樣式絕不應干擾該目標。 但是，印刷格式也具有配置元件的重要角色，不僅在設計的密度與複雜性方面具有強大的效果，對於該設計的使用者經驗，也是如此。

## 字樣

我們選取了 Segoe UI，用於所有的 Microsoft 數位設計。 Segoe UI 提供各種字元，且依設計可讓不同的大小和像素密度維持最佳易讀性。 它提供了清晰、簡潔開放的美感，可補強系統的內容。

![Segoe UI 字型的範例文字](images/segoe-sample.png)

## 粗細

我們在處理印刷格式時，會考量到簡易性和效率。 我們選擇使用一個字樣、最小的粗細與大小，以及明確的階層。 定位和對齊方式會遵循指定語言的預設樣式。 英文的順序為由左至右、由上至下。 文字與影像之間的關係是清楚明瞭的。

![顯示支援的字型粗細。 細、Semilight、標準、Semibold 和粗體](images/weights.png)

## 行距

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


<!-- OP version -->

## 字型間距調整和追蹤

Segoe 是很人性化的字樣，具有柔和、易讀的外觀，並且採用以手寫文字為基礎的開放格式。 若要確保最佳的易讀性，並維持其人性化的完整性、字型間距調整和追蹤設定必須具有特定值。

字型間距調整應設為「計量」，而追蹤應設為 "0"。

<img src="images/kerning-tracking.png" alt="Shows the difference between kerning and tracking" />

## 文字和字母間距

類似於字型間距調整和追蹤，字距和字母間距會使用特定的設定，以確保最佳易讀性和人性化的完整性。

字距依預設一律為 100%，字母間距則應設為 "0"。

<img src="images/word-letter.png" alt="Shows the difference between word and letter spacing" />


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In a XAML text control use [Typogrphy.Kerning](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.documents.typography.kerning.aspx) to control kerning and [FontStretch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.control.fontstretch.aspx) to control tracking. By default Typography.Kerning is set to “true” and FontStretch is set to “Normal”, which are the recommended values.
    </div>
</aside>



<!-- OP version -->
## 對齊方式

一般而言，我們建議字體的視覺元素和欄應靠左對齊。 在大部分情況下，靠左和不齊右方法會提供一致的內容錨定與統一的配置。

<img src="images/alignment.png" alt="Shows flush-left text" />

## 行尾

當印刷格式未定位為靠左和不齊右時，請試著保持整齊的行尾，並避免斷字。

<img src="images/line-endings.png" alt="Shows even line endings" />

## 段落

若要提供對齊的欄邊緣，應以跳過一行且不縮排來表示段落。

![在段落之間顯示一整行的空間](images/paragraphs.png)

## 字元計數

如果一行太短，眼睛就會被迫要頻繁地來回游移，而破壞閱讀的節奏。 可能的話，每行 50-60 個字母是最易於閱讀的。

Segoe 提供各種字元，且依設計可讓不同的大小以及高與低的像素密度都能維持最佳易讀性。 在文字欄行中使用最佳的字母數目，可確保應用程式中的良好易讀性。

一行太長會使眼睛疲累，且使用者可能會不知道讀到哪了。 一行太短則會迫使讀者的目光頻繁地來回游移，而造成眼睛疲勞。

![顯示行的長度不同的 3 個段落](images/character-count.png)

## 凸排文字對齊方式

圖示與文字的水平對齊可用多種方式來處理，取決於圖示的大小和文字的數量。 當文字 (單行或多行) 符合圖示的高度時，文字應垂直置中。

一旦文字的高度超出圖示的高度時，第一行文字應垂直對齊，而其他文字應在下方自然排列。 在使用端點、上升幅度和下降幅度高度較大的字元時，應謹守相同的對齊方式指導方針。

![顯示數個圖示和文字配對](images/hanging-text-alignment.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            XAML's [TextBlock.TextLineBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.textlinebounds.aspx) property provides access to the cap height and baseline font metrics. It can be used to visually vertically center or top-align type.
    </div>
</aside>

## 裁剪和省略符號

依預設剪裁 — 假設文字會自動換行，除非紅線另有指定。 使用非換行文字時，我們建議使用裁剪，而不要使用省略符號。 剪裁可在容器的邊緣、裝置的邊緣和捲軸的邊緣等位置執行。

例外狀況 — 對於未明確定義的容器 （例如，沒有區別的背景色彩），可將非換行文字個別設定成使用省略符號 ”…”。

![顯示使用文字裁剪的裝置框架](images/clipping.png)

# 字體坡形

您應不同大小的 Segoe UI 在字體坡形中建立階層。 此階層可做為基礎結構，讓使用者輕鬆地瀏覽已撰寫的通訊。

<figure class="figure-img" >
    <img src="images/type-ramp.png" alt="Shows the type ramp"  />
        <figcaption>所有大小皆在有效像素中。 如需詳細資訊，請參閱 TODO: 連結。</figcaption>
</figure>

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Most levels of the ramp are available as XAML [static resources](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_type_ramp) that follow the `*TextBlockStyle` naming convention (ex: `HeaderTextBlockStyle`). 
    </div>
</aside>


## 主要和次要文字

若要在字體坡形以外建立其他階層，請將次要文字設為 60% 的不透明度。 在[佈景主題調色盤](color.md#color-themes)中，您可以使用 BaseMedium。 主要文字一律應為 100% 的不透明或 BaseHigh。

## 全部大寫的標題

特定頁面標題應採用「全部大寫」的格式，以新增另一個階層維度。 這些標題應使用 BaseAlt，且字元間距應設為 em 的千分之 75。 這種處理方式也可用來輔助應用程式瀏覽。

不過，在某些語言中，特定的名稱在採用大寫時會變更其意義，因此，任何以名稱或使用者輸入為基礎的頁面標題均*不應*轉換為全部大寫。


## 可行與禁止事項
* 大部分文字使用內文
* 標題在空間有限使用基準
* 納入 SubtitleAlt，以藉由強調最上層內容來建立對比和階層
* 對於長字串或任何主要動作，請不要使用輔助字幕
* 如果文字需要自動換行，請不要使用標題或副標題
* 請不要在同一頁面上結合 Subtitle 和 SubtitleAlt

## 相關文章

* [文字控制項](../controls-and-patterns/text-controls.md)


<!--HONumber=Mar16_HO5-->


