---
本文列出並提供 Segoe MDL2 Asset 字型隨附之字符的用法指導方針。
Search.Refinement.TopicID: 184
Segoe MDL2 圖示的指導方針
ms.assetid: DFB215C2-8A61-4957-B662-3B1991AC9BE1
Segoe MDL2 圖示
template: detail.hbs
---

# Segoe MDL2 圖示的指導方針


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Symbol 列舉 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn252842)
-   [**AppBarIcon 列舉 (HTML)**](https://msdn.microsoft.com/library/windows/apps/hh770557)

本文列出並提供 Segoe MDL2 Asset 字型隨附之字符的用法指導方針。 若要取得字型，您必須安裝 Windows 10。

## <span id="Recommendations"> </span> <span id="recommendations"> </span> <span id="RECOMMENDATIONS"> </span>建議


-   只在您可以明確指定 **Segoe MDL2 Assets** 字型時才使用這些字符。

## <span id="Additional_usage_guidance"> </span> <span id="additional_usage_guidance"> </span> <span id="ADDITIONAL_USAGE_GUIDANCE"> </span>其他用法指導方針


自 Windows 10 發行之後，Windows 8/8.1 **Segoe UI Symbol** 字型已被取代為 **Segoe MDL2 Assets** 字型。 它的使用方式與較舊的字型大致相同，但是許多字符已使用具有字型衡量標準集的 Windows 10 圖示樣式重新繪製，讓圖示在字型的 em 正方形內對齊，而不是在印刷基準上。

**注意** **Em** 是字型中的度量單位。 字型中的 1 em 等於 72ppi 的 100% 指定點值。 例如，16pt 等於 72ppi 的 16px (也稱為 100% 縮放倍數)。 設計新的 MDL2 字型，讓圖示區域的範圍是正方形 Em。 因此如果您在字碼中針對寬度和高度放置 16px，您會取得 16x16px 圖示範圍。 這不一定表示該圖示是範圍的完整維度。

 

**Segoe UI Symbol** 仍可做為「傳統」資源。 但是我們建議您更新所有的應用程式以使用新的 **Segoe MDL2 Assets**。

大部分 **Segoe MDL2 Assets** 字型中包含的圖示和 UI 控制項會對應到 Unicode 專用區 (PUA)。 PUA 可讓字型開發人員將私人 Unicode 值指派給沒有對應到現有字碼指標的字符。 這在建立符號字型時很有用，但這會產生互通性問題。 如果無法使用字型，則不會顯示字符。 只有在您可以指定 **Segoe MDL2 Assets** 字型時才能使用這些字符。

如果您使用磚，則不能使用這些圖像，因為您無法指定磚字型，而且無法透過字型遞補使用 PUA 圖像。

不同於 **Segoe UI Symbol**，**Segoe MDL2 Assets** 字型中的圖示不是用於內嵌文字。 這表示像是進度公開箭頭的某些較舊「技巧」不再適用。 同樣地，因為所有新的圖示的大小和定位相同，所以不需要以零寬度製作；我們剛才確定它們可以做為集合運作。 在理想的情況下，您可以重疊設計為集合的兩個圖示，它們會落在位置上。 我們可以這樣做以允許字碼中的顏色標示。 例如，針對磚徽章狀態建立 U+EA3A 和 U+EA3B。 因為這些項目已經於圓形填滿部分置中，可以針對不同的狀態上色。

Segoe UI Symbol 也會依賴「零寬度」字符進行分層和顏色標示，在這個範例中，一個黑色外框 (U+E006) 可以繪製在零寬度的紅色心形 (U+E00B) 上方。

![使用零寬度字符](images/segoe-ui-symbol-layering.png)

**Segoe MDL2 Assets** 中的所有字符有相同的固定寬度和一致的高度與左邊的原始點，因此分層及顏色標示效果可以藉由直接在彼此上方繪製字符來達成。

很多圖示也以鏡像的形式存在，可用來顯示使用由右至左方向的語言，例如，阿拉伯文、波斯文及希伯來文。

如果您使用 C#/VB/C++ 和 XAML 開發 app，可以選用 [**Symbol enumeration**](https://msdn.microsoft.com/library/windows/apps/dn252842) 而不是 Unicode ID 指定 Segoe MDL2 Assets 字型的字符。 如果您使用 JavaScript 和 HTML 開發 app，可以選擇使用 [**AppBarIcon 列舉**](https://msdn.microsoft.com/library/windows/apps/hh770557) (而不是 Unicode ID) 指定 **Segoe MDL2 Assets** 字型的字符。

此外，請記住 **Segoe MDL2 Assets** 字型包含許多其他我們可以在這裡顯示以外的圖示。 許多是針對特殊目的且通常不用於其他地方。

## <span id="Hearts"> </span> <span id="hearts"> </span> <span id="HEARTS"> </span>傷心小棧


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">代碼</th>
<th align="left">符號</th>
<th align="left">列舉</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>U+E006</p></td>
<td align="left"><img src="images/heartlegacy.png" alt="HeartLegacy" /></td>
<td align="left"><p>HeartLegacy</p></td>
<td align="left"><p>外框的心形</p></td>
</tr>
<tr class="even">
<td align="left"><p>U+E0A5</p></td>
<td align="left"><img src="images/heartfilllegacy.png" alt="HeartFillLegacy" /></td>
<td align="left"><p>HeartFillLegacy</p></td>
<td align="left"><p>實心心形</p></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E007</p></td>
<td align="left"><img src="images/heartbrokenlegacy.png" alt="HeartBrokenLegacy" /></td>
<td align="left"><p>HeartBrokenLegacy</p></td>
<td align="left"><p>破碎心形</p></td>
</tr>
<tr class="even">
<td align="left"><p>U+E00B</p></td>
<td align="left"><img src="images/heartfillzerowidthlegacy.png" alt="HeartFillZeroWidthLegacy" /></td>
<td align="left"><p>HeartFillZeroWidthLegacy</p></td>
<td align="left"><p>實心心形 (寬度為零)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E00C</p></td>
<td align="left"><img src="images/heartbrokenzerowidthlegacy.png" alt="HeartBrokenZeroWidthLegacy" /></td>
<td align="left"><p>HeartBrokenZeroWidthLegacy</p></td>
<td align="left"><p>破碎心形 (寬度為零)</p></td>
</tr>
</tbody>
</table>

 

## <span id="Rating_stars"> </span> <span id="rating_stars"> </span> <span id="RATING_STARS"> </span>評等星星


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">代碼</th>
<th align="left">符號</th>
<th align="left">列舉</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">U+E224</td>
<td align="left"><img src="images/ratingstarlegacy.png" alt="RatingStarLegacy" /></td>
<td align="left">RatingStarLegacy</td>
<td align="left">外框的星形</td>
</tr>
<tr class="even">
<td align="left">U+E0B4</td>
<td align="left"><img src="images/ratingstarfilllegacy.png" alt="RatingStarFillLegacy" /></td>
<td align="left">RatingStarFillLegacy</td>
<td align="left">實心星形</td>
</tr>
<tr class="odd">
<td align="left">U+E00A</td>
<td align="left"><img src="images/ratingstarfillzerowidthlegacy.png" alt="RatingStarFillZeroWidthLegacy" /></td>
<td align="left">RatingStarFillZeroWidthLegacy</td>
<td align="left">實心星形 (寬度為零)</td>
</tr>
<tr class="even">
<td align="left">U+E082</td>
<td align="left"><img src="images/ratingstarfillreducedpaddinghtmllegacy.png" alt="RatingStarFillReducedPaddingHTMLLegacy" /></td>
<td align="left">RatingStarFillReducedPaddingHTMLLegacy</td>
<td align="left">實心星形 (減少邊框間距以在 HTML 中使用)</td>
</tr>
<tr class="odd">
<td align="left">U+E0B5</td>
<td align="left"><img src="images/ratingstarfillsmalllegacy.png" alt="RatingStarFillSmallLegacy" /></td>
<td align="left">RatingStarFillSmallLegacy</td>
<td align="left">小型星形</td>
</tr>
<tr class="even">
<td align="left">U+E7C6</td>
<td align="left"><img src="images/halfstarleft.png" alt="HalfStarLeft" /></td>
<td align="left">HalfStarLeft</td>
<td align="left">半側 星形，左邊</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E7C7</p></td>
<td align="left"><img src="images/halfstarright.png" alt="HalfStarRight" /></td>
<td align="left">HalfStarRight</td>
<td align="left">半側星形，右邊</td>
</tr>
</tbody>
</table>

 

## <span id="Checkbox_components"> </span> <span id="checkbox_components"> </span> <span id="CHECKBOX_COMPONENTS"> </span>核取方塊元件


|        |                                                                                |                                 |                         |
|--------|--------------------------------------------------------------------------------|---------------------------------|-------------------------|
| 代碼   | 符號                                                                         | 列舉                            | 說明             |
| U+E001 | ![checkmarklegacy](images/checkmarklegacy.png)                                 | CheckMarkLegacy                 | 核取記號              |
| U+E002 | ![checkboxfilllegacy](images/checkboxfilllegacy.png)                           | CheckboxFillLegacy              | 已填滿核取方塊         |
| U+E003 | ![checkboxlegacy](images/checkboxlegacy.png)                                   | CheckboxLegacy                  | 核取方塊                |
| U+E004 | ![checkboxindeterminatelegacy](images/checkboxindeterminatelegacy.png)         | CheckboxIndeterminateLegacy     | 不確定狀態     |
| U+E005 | ![checkboxcompositereversedlegacy](images/checkboxcompositereversedlegacy.png) | CheckboxCompositeReversedLegacy | 反轉                |
| U+E008 | ![checkmarkzerowidthlegacy](images/checkmarkzerowidthlegacy.png)               | CheckMarkZeroWidthLegacy        | 核取記號 (寬度為零) |
| U+E009 | ![checkboxfillzerowidthlegacy](images/checkboxfillzerowidthlegacy.png)         | CheckboxFillZeroWidthLegacy     | 填滿 (寬度為零)       |
| U+E0A2 | ![checkboxcompositelegacy](images/checkboxcompositelegacy.png)                 | CheckboxCompositeLegacy         | 複合               |
| U+E739 | ![核取方塊](images/checkbox.png)                                               | 核取方塊                        | 核取方塊                |
| U+E73A | ![checkboxcomposite](images/checkboxcomposite.png)                             | CheckboxComposite               | 複合核取方塊      |
| U+E73B | ![checkboxfill](images/checkboxfill.png)                                       | CheckboxFill                    | 已填滿核取方塊         |
| U+E73C | ![checkboxindeterminate](images/checkboxindeterminate.png)                     | CheckboxIndeterminate           | 不確定狀態     |
| U+E73D | ![checkboxcompositereversed](images/checkboxcompositereversed.png)             | CheckboxCompositeReversed       | 反轉複合      |
| U+E73E | ![checkmark](images/checkmark.png)                                             | CheckMark                       | 核取記號              |

 

## <span id="Miscellaneous"> </span> <span id="miscellaneous"> </span> <span id="MISCELLANEOUS"> </span>其他


<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">代碼</th>
<th align="left">符號</th>
<th align="left">列舉</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>U+E134</p></td>
<td align="left"><img src="images/commentlegacy.png" alt="CommentLegacy" /></td>
<td align="left">CommentLegacy</td>
<td align="left">註解</td>
</tr>
<tr class="even">
<td align="left"><p>U+E113</p></td>
<td align="left"><img src="images/favoritelegacy.png" alt="FavoriteLegacy" /></td>
<td align="left">FavoriteLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E195</p></td>
<td align="left"><img src="images/unfavoritelegacy.png" alt="UnfavoriteLegacy" /></td>
<td align="left">UnfavoriteLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E734</p></td>
<td align="left"><img src="images/favoritestar.png" alt="FavoriteStar" /></td>
<td align="left">FavoriteStar</td>
<td align="left">我的最愛外框線</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E735</p></td>
<td align="left"><img src="images/favoritestarfill.png" alt="FavoriteStarFill" /></td>
<td align="left">FavoriteStarFill</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E8D9</p></td>
<td align="left"><img src="images/unfavorite.png" alt="Unfavorite" /></td>
<td align="left">Unfavorite</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E19F</p></td>
<td align="left"><img src="images/likelegacy.png" alt="LikeLegacy" /></td>
<td align="left">LikeLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E19E</p></td>
<td align="left"><img src="images/dislikelegacy.png" alt="DislikeLegacy" /></td>
<td align="left">DislikeLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E19D</p></td>
<td align="left"><img src="images/likedislikelegacy.png" alt="LikeDislikeLegacy" /></td>
<td align="left">LikeDislikeLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E116</p></td>
<td align="left"><img src="images/videolegacy.png" alt="VideoLegacy" /></td>
<td align="left">VideoLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left"><p>U+E714</p></td>
<td align="left"><img src="images/video.png" alt="Video" /></td>
<td align="left">Video</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left"><p>U+E20B</p></td>
<td align="left"><img src="images/mailmessagelegacy.png" alt="MailMessageLegacy" /></td>
<td align="left">MailMessageLegacy</td>
<td align="left">郵件舊版</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E248</p></td>
<td align="left"><img src="images/replylegacy.png" alt="ReplyLegacy" /></td>
<td align="left">ReplyLegacy</td>
<td align="left">回覆</td>
</tr>
<tr class="even">
<td align="left"><p>U+E249</p></td>
<td align="left"><img src="images/favorite2legacy.png" alt="Favorite2Legacy" /></td>
<td align="left">Favorite2Legacy</td>
<td align="left">我的最愛填滿</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E24E</p></td>
<td align="left"><img src="images/unfavorite2legacy.png" alt="Unfavorite2Legacy" /></td>
<td align="left">Unfavorite2Legacy</td>
<td align="left">Unfavorite</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25A</p></td>
<td align="left"><img src="images/mobilecontactlegacy.png" alt="MobileContactLegacy" /></td>
<td align="left">MobileContactLegacy</td>
<td align="left">行動裝置連絡人</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E25B</p></td>
<td align="left"><img src="images/blockedlegacy.png" alt="BlockedLegacy" /></td>
<td align="left">BlockedLegacy</td>
<td align="left">已封鎖連絡人</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25C</p></td>
<td align="left"><img src="images/typingindicatorlegacy.png" alt="TypingIndicatorLegacy" /></td>
<td align="left">TypingIndicatorLegacy</td>
<td align="left">輸入指示器</td>
</tr>
<tr class="odd">
<td align="left"><p>U+E25D</p></td>
<td align="left"><img src="images/presencechickletvideolegacy.png" alt="PresenceChickletVideoLegacy" /></td>
<td align="left">PresenceChickletVideoLegacy</td>
<td align="left">視訊目前狀態 chicklet</td>
</tr>
<tr class="even">
<td align="left"><p>U+E25E</p></td>
<td align="left"><img src="images/presencechickletlegacy.png" alt="PresenceChickletLegacy" /></td>
<td align="left">PresenceChickletLegacy</td>
<td align="left">目前狀態 chicklet</td>
</tr>
</tbody>
</table>

 

## <span id="Scroll_bar_arrows"> </span> <span id="scroll_bar_arrows"> </span> <span id="SCROLL_BAR_ARROWS"> </span>捲軸箭頭


| 代碼   | 符號                                                                   | 列舉                         |
|--------|--------------------------------------------------------------------------|------------------------------|
| U+E00E | ![scrollchevronleftlegacy](images/scrollchevronleftlegacy.png)           | ScrollChevronLeftLegacy      |
| U+E00F | ![scrollchevronrightlegacy](images/scrollchevronrightlegacy.png)         | ScrollChevronRightLegacy     |
| U+E010 | ![scrollchevronuplegacy](images/scrollchevronuplegacy.png)               | ScrollChevronUpLegacy        |
| U+E011 | ![scrollchevrondownlegacy](images/scrollchevrondownlegacy.png)           | ScrollChevronDownLegacy      |
| U+E016 | ![scrollchevronleftboldlegacy](images/scrollchevronleftboldlegacy.png)   | ScrollChevronLeftBoldLegacy  |
| U+E017 | ![scrollchevronrightboldlegacy](images/scrollchevronrightboldlegacy.png) | ScrollChevronRightBoldLegacy |
| U+E018 | ![scrollchevronupboldlegacy](images/scrollchevronupboldlegacy.png)       | ScrollChevronUpBoldLegacy    |
| U+E019 | ![scrollchevrondownboldlegacy](images/scrollchevrondownboldlegacy.png)   | ScrollChevronDownBoldLegacy  |

 

## <span id="Back_buttons"> </span> <span id="back_buttons"> </span> <span id="BACK_BUTTONS"> </span>返回按鈕


返回按鈕的舊版字符有 2 種不同的大小，因此外環的粗細有 20pt 和 42pt。 另外還提供兩個按比例裝飾的新返回按鈕。 這些字符是設計用來支援圖層。

| 代碼   | 符號                                                                     | 列舉                          | 說明                               |
|--------|----------------------------------------------------------------------------|-------------------------------|-------------------------------------------|
| U+E0C4 | ![backbttnarrow20legacy](images/backbttnarrow20legacy.png)                 | BackBttnArrow20Legacy         | 返回按鈕箭頭，20pt                   |
| U+E0A6 | ![backbttnarrow42legacy](images/backbttnarrow42legacy.png)                 | BackBttnArrow42Legacy         | 返回按鈕箭頭，42pt                   |
| U+E0AD | ![backbttnmirroredarrow20legacy](images/backbttnmirroredarrow20legacy.png) | BackBttnMirroredArrow20Legacy | 反轉鏡像的返回按鈕箭頭，20pt |
| U+E0AB | ![backbttnmirroredarrow42legacy](images/backbttnmirroredarrow42legacy.png) | BackBttnMirroredArrow42Legacy | 反轉鏡像的返回按鈕箭頭，42pt |
| U+E830 | ![chromeback](images/chromeback.png)                                       | ChromeBack                    | Chrome 上一頁按鈕                        |
| U+EA47 | ![chromebackmirrored](images/chromebackmirrored.png)                       | ChromeBackMirrored            | Chrome 上一頁鏡像按鈕               |

 

## <span id="Back_arrows_for_HTML"> </span> <span id="back_arrows_for_html"> </span> <span id="BACK_ARROWS_FOR_HTML"> </span>HTML 的返回箭頭


新增額外的程式碼來建立圈住這些字符的圓圈。

| 代碼   | 符號                                                         | 列舉                    | 說明                |
|--------|----------------------------------------------------------------|-------------------------|----------------------------|
| U+E0D5 | ![arrowhtmllegacy](images/arrowhtmllegacy.png)                 | ArrowHTMLLegacy         | 用於 HTML 的箭頭         |
| U+E0AE | ![arrowhtmlmirroredlegacy](images/arrowhtmlmirroredlegacy.png) | ArrowHTMLMirroredLegacy | U+E0D5 的鏡像版本 |

 

## <span id="AppBar_glyphs"> </span> <span id="appbar_glyphs"> </span> <span id="APPBAR_GLYPHS"> </span>AppBar 字符


使用 [**AppBar**](https://msdn.microsoft.com/library/windows/apps/br229670) 之下列清單中的字符。 根據慣例，它們會以其列舉名稱代表。 它們是設計為沒有圓形的 20x20px 圖示。

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">代碼</th>
<th align="left">符號</th>
<th align="left">列舉</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">U+E8FB</td>
<td align="left"><img src="images/accept.png" alt="Accept" /></td>
<td align="left">Accept</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E910</td>
<td align="left"><img src="images/accounts.png" alt="Accounts" /></td>
<td align="left">Accounts</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E710</td>
<td align="left"><img src="images/add.png" alt="Add" /></td>
<td align="left">Add</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FA</td>
<td align="left"><img src="images/addfriend.png" alt="Addfriend" /></td>
<td align="left">AddFriend</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7EF</td>
<td align="left"><img src="images/admin.png" alt="Admin" /></td>
<td align="left">Admin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E3</td>
<td align="left"><img src="images/aligncenter.png" alt="Aligncenter" /></td>
<td align="left">AlignCenter</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E4</td>
<td align="left"><img src="images/alignleft.png" alt="Alignleft" /></td>
<td align="left">AlignLeft</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E2</td>
<td align="left"><img src="images/alignright.png" alt="Alignright" /></td>
<td align="left">Alignright</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71D</td>
<td align="left"><img src="images/allapps.png" alt="Allapps" /></td>
<td align="left">AllApps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E723</td>
<td align="left"><img src="images/attach.png" alt="Attach" /></td>
<td align="left">Attach</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A2</td>
<td align="left"><img src="images/attachcamera.png" alt="Attachcamera" /></td>
<td align="left">AttachCamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D6</td>
<td align="left"><img src="images/audio.png" alt="Audio" /></td>
<td align="left">Audio</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E72B</td>
<td align="left"><img src="images/back.png" alt="Back" /></td>
<td align="left">Back</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E73F</td>
<td align="left"><img src="images/backtowindow.png" alt="Backtowindow" /></td>
<td align="left">BackToWindow</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F8</td>
<td align="left"><img src="images/blockcontact.png" alt="Blockcontact" /></td>
<td align="left">BlockContact</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DD</td>
<td align="left"><img src="images/bold.png" alt="Bold" /></td>
<td align="left">Bold</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A4</td>
<td align="left"><img src="images/bookmarks.png" alt="Bookmarks" /></td>
<td align="left">Bookmarks</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C5</td>
<td align="left"><img src="images/browsephotos.png" alt="Browsephotos" /></td>
<td align="left">BrowsePhotos</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8FD</td>
<td align="left"><img src="images/bulletedlist.png" alt="Bulletedlist" /></td>
<td align="left">BulletedList</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EF</td>
<td align="left"><img src="images/calculator.png" alt="Calculator" /></td>
<td align="left">Calculator</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E787</td>
<td align="left"><img src="images/calendar.png" alt="Calendar" /></td>
<td align="left">Calendar</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8BF</td>
<td align="left"><img src="images/calendarday.png" alt="Calendarday" /></td>
<td align="left">CalendarDay</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F5</td>
<td align="left"><img src="images/calendarreply.png" alt="Calendarreply" /></td>
<td align="left">CalendarReply</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C0</td>
<td align="left"><img src="images/calendarweek.png" alt="Calendarweek" /></td>
<td align="left">CalendarWeek</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E722</td>
<td align="left"><img src="images/camera.png" alt="Camera" /></td>
<td align="left">Camera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E711</td>
<td align="left"><img src="images/cancel.png" alt="Cancel" /></td>
<td align="left">Cancel</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BA</td>
<td align="left"><img src="images/caption.png" alt="Caption" /></td>
<td align="left">Caption</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7F0</td>
<td align="left"><img src="images/cc.png" alt="CC" /></td>
<td align="left">CC</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8EA</td>
<td align="left"><img src="images/cellphone.png" alt="Cellphone" /></td>
<td align="left">Cellphone</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C1</td>
<td align="left"><img src="images/characters.png" alt="Characters" /></td>
<td align="left">Characters</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E894</td>
<td align="left"><img src="images/clear.png" alt="Clear" /></td>
<td align="left">Clear</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E6</td>
<td align="left"><img src="images/clearselection.png" alt="Clearselection" /></td>
<td align="left">ClearSelection</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89F</td>
<td align="left"><img src="images/closepane.png" alt="Closepane" /></td>
<td align="left">ClosePane</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E753</td>
<td align="left"><img src="images/cloud.png" alt="Cloud" /></td>
<td align="left">Cloud</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90A</td>
<td align="left"><img src="images/comment.png" alt="Comment" /></td>
<td align="left">Comment</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E77B</td>
<td align="left"><img src="images/contact.png" alt="Contact" /></td>
<td align="left">Contact</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D4</td>
<td align="left"><img src="images/contact2.png" alt="Contact2" /></td>
<td align="left">Contact2</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E779</td>
<td align="left"><img src="images/contactinfo.png" alt="Contactinfo" /></td>
<td align="left">ContactInfo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CF</td>
<td align="left"><img src="images/contactpresence.png" alt="Contactpresence" /></td>
<td align="left">ContactPresence</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C8</td>
<td align="left"><img src="images/copy.png" alt="Copy" /></td>
<td align="left">Copy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7A8</td>
<td align="left"><img src="images/crop.png" alt="Crop" /></td>
<td align="left">Crop</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C6</td>
<td align="left"><img src="images/cut.png" alt="Cut" /></td>
<td align="left">Cut</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E74D</td>
<td align="left"><img src="images/delete.png" alt="Delete" /></td>
<td align="left">Delete</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F0</td>
<td align="left"><img src="images/directions.png" alt="Directions" /></td>
<td align="left">Directions</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D8</td>
<td align="left"><img src="images/disableupdates.png" alt="Disableupdates" /></td>
<td align="left">DisableUpdates</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8CD</td>
<td align="left"><img src="images/disconnectdrive.png" alt="Disconnectdrive" /></td>
<td align="left">DisconnectDrive</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E0</td>
<td align="left"><img src="images/dislike.png" alt="Dislike" /></td>
<td align="left">Dislike</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E90E</td>
<td align="left"><img src="images/dockbottom.png" alt="Dockbottom" /></td>
<td align="left">DockBottom</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90C</td>
<td align="left"><img src="images/dockleft.png" alt="Dockleft" /></td>
<td align="left">DockLeft</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E90D</td>
<td align="left"><img src="images/dockright.png" alt="Dockright" /></td>
<td align="left">DockRight</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A5</td>
<td align="left"><img src="images/document.png" alt="Document" /></td>
<td align="left">Document</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E896</td>
<td align="left"><img src="images/download.png" alt="Download" /></td>
<td align="left">Download</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E70F</td>
<td align="left"><img src="images/edit.png" alt="Edit" /></td>
<td align="left">Edit</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E899</td>
<td align="left"><img src="images/emoji.png" alt="Emoji" /></td>
<td align="left">Emoji</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E76E</td>
<td align="left"><img src="images/emoji2.png" alt="Emoji2" /></td>
<td align="left">Emoji2</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E728</td>
<td align="left"><img src="images/favoritelist.png" alt="Favoritelist" /></td>
<td align="left">FavoriteList</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E734</td>
<td align="left"><img src="images/favoritestar.png" alt="Favoritestar" /></td>
<td align="left">FavoriteStar</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E735</td>
<td align="left"><img src="images/favoritestarfill.png" alt="Favoritestarfill" /></td>
<td align="left">FavoriteStarFill</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71C</td>
<td align="left"><img src="images/filter.png" alt="Filter" /></td>
<td align="left">Filter</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E11A</td>
<td align="left"><img src="images/findlegacy.png" alt="Findlegacy" /></td>
<td align="left">FindLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7C1</td>
<td align="left"><img src="images/flag.png" alt="Flag" /></td>
<td align="left">Flag</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B7</td>
<td align="left"><img src="images/folder.png" alt="Folder" /></td>
<td align="left">Folder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D2</td>
<td align="left"><img src="images/font.png" alt="Font" /></td>
<td align="left">Font</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D3</td>
<td align="left"><img src="images/fontcolor.png" alt="Fontcolor" /></td>
<td align="left">Fontcolor</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E7</td>
<td align="left"><img src="images/fontdecrease.png" alt="Fontdecrease" /></td>
<td align="left">FontDecrease</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8E8</td>
<td align="left"><img src="images/fontincrease.png" alt="Fontincrease" /></td>
<td align="left">FontIncrease</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E9</td>
<td align="left"><img src="images/fontsize.png" alt="Fontsize" /></td>
<td align="left">FontSize</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E72A</td>
<td align="left"><img src="images/forward.png" alt="Forward" /></td>
<td align="left">Forward</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E908</td>
<td align="left"><img src="images/fourbars.png" alt="Fourbars" /></td>
<td align="left">FourBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E740</td>
<td align="left"><img src="images/fullscreen.png" alt="Fullscreen" /></td>
<td align="left">FullScreen</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E774</td>
<td align="left"><img src="images/globe.png" alt="Globe" /></td>
<td align="left">Globe</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AD</td>
<td align="left"><img src="images/go.png" alt="Go" /></td>
<td align="left">Go</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8FC</td>
<td align="left"><img src="images/gotostart.png" alt="Gotostart" /></td>
<td align="left">GoToStart</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D1</td>
<td align="left"><img src="images/gototoday.png" alt="Gototoday" /></td>
<td align="left">GoToToday</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E778</td>
<td align="left"><img src="images/hangup.png" alt="Hangup" /></td>
<td align="left">Hangup</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E897</td>
<td align="left"><img src="images/help.png" alt="Help" /></td>
<td align="left">Help</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8C5</td>
<td align="left"><img src="images/hidebcc.png" alt="Hidebcc" /></td>
<td align="left">HideBCC</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7E6</td>
<td align="left"><img src="images/highlight.png" alt="Highlight" /></td>
<td align="left">Highlight</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E80F</td>
<td align="left"><img src="images/home.png" alt="Home" /></td>
<td align="left">Home</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B5</td>
<td align="left"><img src="images/import.png" alt="Import" /></td>
<td align="left">Import</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B6</td>
<td align="left"><img src="images/importall.png" alt="Importall" /></td>
<td align="left">ImportAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C9</td>
<td align="left"><img src="images/important.png" alt="Important" /></td>
<td align="left">Important</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8DB</td>
<td align="left"><img src="images/italic.png" alt="Italic" /></td>
<td align="left">Italic</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E765</td>
<td align="left"><img src="images/keyboardclassic.png" alt="Keyboardclassic" /></td>
<td align="left">KeyboardClassic</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89B</td>
<td align="left"><img src="images/leavechat.png" alt="Leavechat" /></td>
<td align="left">LeaveChat</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F1</td>
<td align="left"><img src="images/library.png" alt="Library" /></td>
<td align="left">Library</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E1</td>
<td align="left"><img src="images/like.png" alt="Like" /></td>
<td align="left">Like</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DF</td>
<td align="left"><img src="images/likedislike.png" alt="Likedislike" /></td>
<td align="left">LikeDislike</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E71B</td>
<td align="left"><img src="images/link.png" alt="Link" /></td>
<td align="left">Link</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+EA37</td>
<td align="left"><img src="images/list.png" alt="List" /></td>
<td align="left">List</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E81D</td>
<td align="left"><img src="images/location.png" alt="Location" /></td>
<td align="left">Location</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E715</td>
<td align="left"><img src="images/mail.png" alt="Mail" /></td>
<td align="left">Mail</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A8</td>
<td align="left"><img src="images/mailfill.png" alt="Mailfill" /></td>
<td align="left">MailFill</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E89C</td>
<td align="left"><img src="images/mailforward.png" alt="Mailforward" /></td>
<td align="left">MailForward</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CA</td>
<td align="left"><img src="images/mailreply.png" alt="Mailreply" /></td>
<td align="left">MailReply</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C2</td>
<td align="left"><img src="images/mailreplyall.png" alt="Mailreplyall" /></td>
<td align="left">MailReplyAll</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E912</td>
<td align="left"><img src="images/manage.png" alt="Manage" /></td>
<td align="left">Manage</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8CE</td>
<td align="left"><img src="images/mapdrive.png" alt="Mapdrive" /></td>
<td align="left">MapDrive</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E707</td>
<td align="left"><img src="images/mappin.png" alt="Mappin" /></td>
<td align="left">Mappin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E77C</td>
<td align="left"><img src="images/memo.png" alt="Memo" /></td>
<td align="left">Memo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BD</td>
<td align="left"><img src="images/message.png" alt="Message" /></td>
<td align="left">Message</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E720</td>
<td align="left"><img src="images/microphone.png" alt="Microphone" /></td>
<td align="left">Microphone</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E712</td>
<td align="left"><img src="images/more.png" alt="More" /></td>
<td align="left">More</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DE</td>
<td align="left"><img src="images/movetofolder.png" alt="Movetofolder" /></td>
<td align="left">MoveToFolder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90B</td>
<td align="left"><img src="images/musicinfo.png" alt="Musicinfo" /></td>
<td align="left">MusicInfo</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E74F</td>
<td align="left"><img src="images/mute.png" alt="Mute" /></td>
<td align="left">Mute</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F4</td>
<td align="left"><img src="images/newfolder.png" alt="Newfolder" /></td>
<td align="left">NewFolder</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E78B</td>
<td align="left"><img src="images/newwindow.png" alt="Newwindow" /></td>
<td align="left">NewWindow</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E893</td>
<td align="left"><img src="images/next.png" alt="Next" /></td>
<td align="left">Next</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E905</td>
<td align="left"><img src="images/onebar.png" alt="Onebar" /></td>
<td align="left">OneBar</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8E5</td>
<td align="left"><img src="images/openfile.png" alt="Openfile" /></td>
<td align="left">OpenFile</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DA</td>
<td align="left"><img src="images/openlocal.png" alt="Openlocal" /></td>
<td align="left">OpenLocal</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A0</td>
<td align="left"><img src="images/openpane.png" alt="Openpane" /></td>
<td align="left">OpenPane</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7AC</td>
<td align="left"><img src="images/openwith.png" alt="Openwith" /></td>
<td align="left">OpenWith</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B4</td>
<td align="left"><img src="images/orientation.png" alt="Orientation" /></td>
<td align="left">Orientation</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7EE</td>
<td align="left"><img src="images/otheruser.png" alt="Otheruser" /></td>
<td align="left">OtherUser</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E1CE</td>
<td align="left"><img src="images/outlinestarlegacy.png" alt="Outlinestarlegacy" /></td>
<td align="left">OutlineStarLegacy</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C3</td>
<td align="left"><img src="images/page.png" alt="Page" /></td>
<td align="left">Page</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E77F</td>
<td align="left"><img src="images/paste.png" alt="Paste" /></td>
<td align="left">Paste</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E769</td>
<td align="left"><img src="images/pause.png" alt="Pause" /></td>
<td align="left">Pause</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E716</td>
<td align="left"><img src="images/people.png" alt="People" /></td>
<td align="left">People</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D7</td>
<td align="left"><img src="images/permissions.png" alt="Permissions" /></td>
<td align="left">Permissions</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E717</td>
<td align="left"><img src="images/phone.png" alt="Phone" /></td>
<td align="left">Phone</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E780</td>
<td align="left"><img src="images/phonebook.png" alt="Phonebook" /></td>
<td align="left">PhoneBook</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E718</td>
<td align="left"><img src="images/pin.png" alt="Pin" /></td>
<td align="left">Pin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E768</td>
<td align="left"><img src="images/play.png" alt="Play" /></td>
<td align="left">Play</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F3</td>
<td align="left"><img src="images/postupdate.png" alt="Postupdate" /></td>
<td align="left">PostUpdate</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FF</td>
<td align="left"><img src="images/preview.png" alt="Preview" /></td>
<td align="left">Preview</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A1</td>
<td align="left"><img src="images/previewlink.png" alt="Previewlink" /></td>
<td align="left">PreviewLink</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E892</td>
<td align="left"><img src="images/previous.png" alt="Previous" /></td>
<td align="left">Previous</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8D0</td>
<td align="left"><img src="images/priority.png" alt="Priority" /></td>
<td align="left">Priority</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8A6</td>
<td align="left"><img src="images/protecteddocument.png" alt="Protecteddocument" /></td>
<td align="left">ProtectedDocument</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8C3</td>
<td align="left"><img src="images/read.png" alt="Read" /></td>
<td align="left">Read</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7A6</td>
<td align="left"><img src="images/redo.png" alt="Redo" /></td>
<td align="left">Redo</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E72C</td>
<td align="left"><img src="images/refresh.png" alt="Refresh" /></td>
<td align="left">Refresh</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AF</td>
<td align="left"><img src="images/remote.png" alt="Remote" /></td>
<td align="left">Remote</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E738</td>
<td align="left"><img src="images/remove.png" alt="Remove" /></td>
<td align="left">Remove</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AC</td>
<td align="left"><img src="images/rename.png" alt="Rename" /></td>
<td align="left">Rename</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E90F</td>
<td align="left"><img src="images/repair.png" alt="Repair" /></td>
<td align="left">Repair</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EE</td>
<td align="left"><img src="images/repeatall.png" alt="Repeatall" /></td>
<td align="left">RepeatAll</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8ED</td>
<td align="left"><img src="images/repeatone.png" alt="Repeatone" /></td>
<td align="left">RepeatOne</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E730</td>
<td align="left"><img src="images/reporthacked.png" alt="Reporthacked" /></td>
<td align="left">ReportHacked</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8EB</td>
<td align="left"><img src="images/reshare.png" alt="Reshare" /></td>
<td align="left">Reshare</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7AD</td>
<td align="left"><img src="images/rotate.png" alt="Rotate" /></td>
<td align="left">Rotate</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89E</td>
<td align="left"><img src="images/rotatecamera.png" alt="Rotatecamera" /></td>
<td align="left">RotateCamera</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E74E</td>
<td align="left"><img src="images/save.png" alt="Save" /></td>
<td align="left">Save</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E78C</td>
<td align="left"><img src="images/savelocal.png" alt="Savelocal" /></td>
<td align="left">SaveLocal</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8FE</td>
<td align="left"><img src="images/scan.png" alt="Scan" /></td>
<td align="left">Scan</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B3</td>
<td align="left"><img src="images/selectall.png" alt="Selectall" /></td>
<td align="left">SelectAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E724</td>
<td align="left"><img src="images/send.png" alt="Send" /></td>
<td align="left">Send</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7B5</td>
<td align="left"><img src="images/setlockscreen.png" alt="Setlockscreen" /></td>
<td align="left">SetLockScreen</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E97B</td>
<td align="left"><img src="images/settile.png" alt="Settile" /></td>
<td align="left">SetTile</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E713</td>
<td align="left"><img src="images/settings.png" alt="Settings" /></td>
<td align="left">Settings</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E72D</td>
<td align="left"><img src="images/share.png" alt="Share" /></td>
<td align="left">Share</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E719</td>
<td align="left"><img src="images/shop.png" alt="Shop" /></td>
<td align="left">Shop</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8C4</td>
<td align="left"><img src="images/showbcc.png" alt="Showbcc" /></td>
<td align="left">ShowBCC</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8BC</td>
<td align="left"><img src="images/showresults.png" alt="Showresults" /></td>
<td align="left">ShowResults</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8B1</td>
<td align="left"><img src="images/shuffle.png" alt="Shuffle" /></td>
<td align="left">Shuffle</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E786</td>
<td align="left"><img src="images/slideshow.png" alt="Slideshow" /></td>
<td align="left">Slideshow</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E1CF</td>
<td align="left"><img src="images/solidstarlegacy.png" alt="Solidstarlegacy" /></td>
<td align="left">SolidStarLegacy</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8CB</td>
<td align="left"><img src="images/sort.png" alt="Sort" /></td>
<td align="left">Sort</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71A</td>
<td align="left"><img src="images/stop.png" alt="Stop" /></td>
<td align="left">Stop</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E913</td>
<td align="left"><img src="images/street.png" alt="Street" /></td>
<td align="left">Street</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8AB</td>
<td align="left"><img src="images/switch.png" alt="Switch" /></td>
<td align="left">Switch</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F9</td>
<td align="left"><img src="images/switchapps.png" alt="Switchapps" /></td>
<td align="left">SwitchApps</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E895</td>
<td align="left"><img src="images/sync.png" alt="Sync" /></td>
<td align="left">Sync</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8F7</td>
<td align="left"><img src="images/syncfolder.png" alt="Syncfolder" /></td>
<td align="left">SyncFolder</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8EC</td>
<td align="left"><img src="images/tag.png" alt="Tag" /></td>
<td align="left">Tag</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E907</td>
<td align="left"><img src="images/threebars.png" alt="Threebars" /></td>
<td align="left">ThreeBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E7C9</td>
<td align="left"><img src="images/touchpointer.png" alt="Touchpointer" /></td>
<td align="left">TouchPointer</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E78A</td>
<td align="left"><img src="images/trim.png" alt="Trim" /></td>
<td align="left">Trim</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E906</td>
<td align="left"><img src="images/twobars.png" alt="Twobars" /></td>
<td align="left">TwoBars</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E89A</td>
<td align="left"><img src="images/twopage.png" alt="Twopage" /></td>
<td align="left">TwoPage</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8DC</td>
<td align="left"><img src="images/underline.png" alt="Underline" /></td>
<td align="left">Underline</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E7A7</td>
<td align="left"><img src="images/undo.png" alt="Undo" /></td>
<td align="left">Undo</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8D9</td>
<td align="left"><img src="images/unfavorite.png" alt="Unfavorite" /></td>
<td align="left">UnFavorite</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E77A</td>
<td align="left"><img src="images/unpin.png" alt="Unpin" /></td>
<td align="left">UnPin</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E8F6</td>
<td align="left"><img src="images/unsyncfolder.png" alt="Unsyncfolder" /></td>
<td align="left">UnSyncFolder</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E74A</td>
<td align="left"><img src="images/up.png" alt="Up" /></td>
<td align="left">Up</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E898</td>
<td align="left"><img src="images/upload.png" alt="Upload" /></td>
<td align="left">Upload</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8AA</td>
<td align="left"><img src="images/videochat.png" alt="Videochat" /></td>
<td align="left">VideoChat</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E890</td>
<td align="left"><img src="images/view.png" alt="View" /></td>
<td align="left">View</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A9</td>
<td align="left"><img src="images/viewall.png" alt="Viewall" /></td>
<td align="left">ViewAll</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E767</td>
<td align="left"><img src="images/volume.png" alt="Volume" /></td>
<td align="left">Volume</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8B8</td>
<td align="left"><img src="images/webcam.png" alt="Webcam" /></td>
<td align="left">Webcam</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E909</td>
<td align="left"><img src="images/world.png" alt="World" /></td>
<td align="left">World</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E904</td>
<td align="left"><img src="images/zerobars.png" alt="Zerobars" /></td>
<td align="left">ZeroBars</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71E</td>
<td align="left"><img src="images/zoom.png" alt="Zoom" /></td>
<td align="left">Zoom</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">U+E8A3</td>
<td align="left"><img src="images/zoomin.png" alt="Zoomin" /></td>
<td align="left">ZoomIn</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">U+E71F</td>
<td align="left"><img src="images/zoomout.png" alt="Zoomout" /></td>
<td align="left">ZoomOut</td>
<td align="left"></td>
</tr>
</tbody>
</table>

 

## <span id="Battery_icons"> </span> <span id="battery_icons"> </span> <span id="BATTERY_ICONS"> </span>電池圖示


| 代碼 | 符號                                             | 列舉              | 代碼 | 符號                                                  | 列舉                |
|------|----------------------------------------------------|-------------------|------|---------------------------------------------------------|---------------------|
| E996 | ![batteryunknown](images/batteryunknown.png)       | BatteryUnknown    | EC02 | ![mobbatteryunknown](images/mobbatteryunknown.png)      | MobBatteryUnknown   |
| E850 | ![battery0](images/battery0.png)                   | Battery0          | EBA0 | ![mobbattery0](images/mobbattery0.png)                  | MobBattery0         |
| E851 | ![battery1](images/battery1.png)                   | Battery1          | EBA1 | ![mobbattery1](images/mobbattery1.png)                  | MobBattery1         |
| E852 | ![battery2](images/battery2.png)                   | Battery2          | EBA2 | ![mobbattery2](images/mobbattery2.png)                  | MobBattery2         |
| E853 | ![battery3](images/battery3.png)                   | Battery3          | EBA3 | ![mobbattery3](images/mobbattery3.png)                  | MobBattery3         |
| E854 | ![battery4](images/battery4.png)                   | Battery4          | EBA4 | ![mobbattery4](images/mobbattery4.png)                  | MobBattery4         |
| E855 | ![battery5](images/battery5.png)                   | Battery5          | EBA5 | ![mobbattery5](images/mobbattery5.png)                  | MobBattery5         |
| E856 | ![battery6](images/battery6.png)                   | Battery6          | EBA6 | ![mobbattery6](images/mobbattery6.png)                  | MobBattery6         |
| E857 | ![battery7](images/battery7.png)                   | Battery7          | EBA7 | ![mobbattery7](images/mobbattery7.png)                  | MobBattery7         |
| E858 | ![battery8](images/battery8.png)                   | Battery8          | EBA8 | ![mobbattery8](images/mobbattery8.png)                  | MobBattery8         |
| E859 | ![battery9](images/battery9.png)                   | Battery9          | EBA9 | ![mobbattery9](images/mobbattery9.png)                  | MobBattery9         |
| E83F | ![battery10](images/battery10.png)                 | Battery10         | EBA0 | ![mobbatter10](images/mobbattery10.png)                 | MobBatter10         |
| E85A | ![batterycharging0](images/batterycharging0.png)   | BatteryCharging0  | EBAB | ![mobbatterycharging0](images/mobbatterycharging0.png)  | MobBatteryCharging0 |
| E85B | ![batterycharging1](images/batterycharging1.png)   | BatteryCharging1  | EBAC | ![mobbatterycharging1](images/mobbatterycharging1.png)  | MobBatteryCharging1 |
| E85C | ![batterycharging2](images/batterycharging2.png)   | BatteryCharging2  | EBAD | ![mobbatterycharging2](images/mobbatterycharging2.png)  | MobBatteryCharging2 |
| E85D | ![batterycharging3](images/batterycharging3.png)   | BatteryCharging3  | EBAE | ![mobbatterycharging3](images/mobbatterycharging3.png)  | MobBatteryCharging3 |
| E85E | ![batterycharging4](images/batterycharging4.png)   | BatteryCharging4  | EBAF | ![mobbatterycharging4](images/mobbatterycharging4.png)  | MobBatteryCharging4 |
| E85F | ![batterycharging5](images/batterycharging5.png)   | BatteryCharging5  | EBB0 | ![mobbatterycharging5](images/mobbatterycharging5.png)  | MobBatteryCharging5 |
| E860 | ![batterycharging6](images/batterycharging6.png)   | BatteryCharging6  | EBB1 | ![mobbatterycharging6](images/mobbatterycharging6.png)  | MobBatteryCharging6 |
| E861 | ![batterycharging7](images/batterycharging7.png)   | BatteryCharging7  | EBB2 | ![mobbatterycharging7](images/mobbatterycharging7.png)  | MobBatteryCharging7 |
| E862 | ![batterycharging8](images/batterycharging8.png)   | BatteryCharging8  | EBB3 | ![mobbatterycharging8](images/mobbatterycharging8.png)  | MobBatteryCharging8 |
| E83E | ![batterycharging9](images/batterycharging9.png)   | BatteryCharging9  | EBB4 | ![mobbatterycharging9](images/mobbatterycharging9.png)  | MobBatteryCharging9 |
| ea93 | ![batterycharging10](images/batterycharging10.png) | BatteryCharging10 | EBB5 | ![mobbatterychargin10](images/mobbatterycharging10.png) | MobBatteryChargin10 |
| E863 | ![batterysaver0](images/batterysaver0.png)         | BatterySaver0     | EBB6 | ![mobbatterysaver0](images/mobbatterysaver0.png)        | MobBatterySaver0    |
| E864 | ![batterysaver1](images/batterysaver1.png)         | BatterySaver1     | EBB7 | ![mobbatterysaver1](images/mobbatterysaver1.png)        | MobBatterySaver1    |
| E865 | ![batterysaver2](images/batterysaver2.png)         | BatterySaver2     | EBB8 | ![mobbatterysaver2](images/mobbatterysaver2.png)        | MobBatterySaver2    |
| E866 | ![batterysaver3](images/batterysaver3.png)         | BatterySaver3     | EBB9 | ![mobbatterysaver3](images/mobbatterysaver3.png)        | MobBatterySaver3    |
| E867 | ![batterysaver4](images/batterysaver4.png)         | BatterySaver4     | EBBA | ![mobbatterysaver4](images/mobbatterysaver4.png)        | MobBatterySaver4    |
| E868 | ![batterysaver5](images/batterysaver5.png)         | BatterySaver5     | EBBB | ![mobbatterysaver5](images/mobbatterysaver5.png)        | MobBatterySaver5    |
| E869 | ![batterysaver6](images/batterysaver6.png)         | BatterySaver6     | EBBC | ![mobbatterysaver6](images/mobbatterysaver6.png)        | MobBatterySaver6    |
| E86A | ![batterysaver7](images/batterysaver7.png)         | BatterySaver7     | EBBD | ![mobbatterysaver7](images/mobbatterysaver7.png)        | MobBatterySaver7    |
| E86B | ![batterysaver8](images/batterysaver8.png)         | BatterySaver8     | EBBE | ![mobbatterysaver8](images/mobbatterysaver8.png)        | MobBatterySaver8    |
| EA94 | ![batterysaver9](images/batterysaver9.png)         | BatterySaver9     | EBBF | ![mobbatterysaver9](images/mobbatterysaver9.png)        | MobBatterySaver9    |
| EA95 | ![batterysaver10](images/batterysaver10.png)       | BatterySaver10    | EBC0 | ![mobbatterysaver10](images/mobbatterysaver10.png)      | MobBatterySaver10   |

 

## <span id="related_topics"> </span>相關主題


**適用於設計人員**
* [字型的指導方針](fonts.md)
* [W3C 哪些語言是由右至左撰寫 (RTL)？](http://www.i18nguy.com/temp/rtl.mdl)
**適用於開發人員 (XAML)**
* [**Symbol 列舉**](https://msdn.microsoft.com/library/windows/apps/dn252842)


 






<!--HONumber=Mar16_HO1-->


