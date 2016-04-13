---
Description: 選取字型及指定字型大小與色彩時，請遵循這些指導方針。
title: 字型
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
---

# 字型的指導方針


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**FontFamily 屬性**](https://msdn.microsoft.com/library/windows/apps/br209655)

正確使用字型大小、粗細、色彩、字距和間距，有助於讓通用 Windows 平台 (UWP) 應用程式有清楚和整齊的外觀，使用起來更為容易。 選取字型及指定字型大小與色彩時，請遵循這些指導方針。

如果您要尋找 Segoe UI Symbol 圖示的清單，請參閱 [**Segoe UI Symbol 圖示的指導方針**](segoe-ui-symbol-font.md)。

## <span id="The_Windows_10_type_ramp"> </span> <span id="the_windows_10_type_ramp"> </span> <span id="THE_WINDOWS_10_TYPE_RAMP"> </span>Windows 10 字體坡形


字體坡形從標題到內文文字建立了一個重要的設計關係，確保不同的層級之間有一個一目了然且容易理解的階層。 使用者可以立即了解在哪裡尋找資訊，以及如何剖析頁面。

以下是 UWP app 的建議字體坡形：

| 文字樣式 | 字體 | 粗細    | 大小 (epx) | 行距 (epx) | 字距 | 字距調整 (1/1000 em) | XAML 樣式索引鍵          |
|------------|----------|-----------|------------|--------------------|--------------|----------------------|-------------------------|
| 標頭     | Segoe UI | 淡     | 46         | 56                 | 100%         | 0                    | HeaderTextBlockStyle    |
| 子標頭  | Segoe UI | 淡     | 34         | 40                 | 100%         | 0                    | SubheaderTextBlockStyle |
| 標題      | Segoe UI | Semilight | 24         | 28                 | 100%         | 0                    | TitleTextBlockStyle     |
| 次標題   | Segoe UI | 標準   | 20         | 24                 | 100%         | 0                    | SubtitleTextBlockStyle  |
| 基本       | Segoe UI | Semibold  | 15         | 20                 | 100%         | 0                    | BaseTextBlockStyle      |
| 內文       | Segoe UI | 標準   | 15         | 20                 | 100%         | 0                    | BodyTextBlockStyle      |
| 說明文字    | Segoe UI | 標準   | 12         | 14                 | 100%         | 0                    | CaptionTextBlockStyle   |

 

## <span id="Recommended_fonts"> </span> <span id="recommended_fonts"> </span> <span id="RECOMMENDED_FONTS"> </span>建議字型


您並不需要針對所有項目使用 Segoe UI。 您可能會針對特定情況使用其他字型，例如閱讀，或顯示非英文語言的文字。

下面的清單列出在所有支援 UWP app 之 Windows 10 版本中保證可以使用的字型。

**注意** 如果您使用的字型不在此清單中，您的 app 可能會觸發從 Microsoft 服務自動下載該字型。 這可能會有效能和其他影響的考量，特別是針對行動裝置。 請特別注意，這可能會使用使用者的行動數據方案，或造成行動數據用量的花費。 可在行動裝置上使用的 UWP app 的 UI 內容永遠不應該使用在此清單以外的字型。

 

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">樣式</th>
<th align="left">註解</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Arial</td>
<td align="left">標準、斜體、粗體、粗斜體、Black</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Calibri</td>
<td align="left">標準、斜體、粗體、粗斜體、淡、淡斜體</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Cambria</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Cambria Math</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Comic Sans MS</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Courier New</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Ebrima</td>
<td align="left">標準、粗體</td>
<td align="left">非洲字集 (衣索比亞文、西非書面文、歐斯馬雅文、提弗納文、范文) 的使用者介面字型</td>
</tr>
<tr class="even">
<td align="left">Gadugi</td>
<td align="left">標準</td>
<td align="left">北美洲字集 (加拿大語音節、卻洛奇文) 的使用者介面字型</td>
</tr>
<tr class="odd">
<td align="left">Georgia</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">爪哇文文字 標準 爪哇文字集的後援字型</td>
<td align="left">標準</td>
<td align="left">爪哇字集的後援字型</td>
</tr>
<tr class="odd">
<td align="left">Leelawadee UI</td>
<td align="left">標準、Semilight、粗體</td>
<td align="left">東南亞字集 (布吉斯文、寮文、高棉文、泰文) 的使用者介面字型</td>
</tr>
<tr class="even">
<td align="left">Lucida Console</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Malgun Gothic</td>
<td align="left">標準</td>
<td align="left">韓文的使用者介面字型</td>
</tr>
<tr class="even">
<td align="left">Microsoft Himalaya</td>
<td align="left">標準</td>
<td align="left">藏文字集的後援字型</td>
</tr>
<tr class="odd">
<td align="left">Microsoft JhengHei</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Microsoft JhengHei UI</td>
<td align="left">標準</td>
<td align="left">繁體中文的使用者介面字型</td>
</tr>
<tr class="odd">
<td align="left">Microsoft New Tai Lue</td>
<td align="left">標準</td>
<td align="left">新傣文字集的後援字型</td>
</tr>
<tr class="even">
<td align="left">Microsoft PhagsPa</td>
<td align="left">標準</td>
<td align="left">八思巴文字集的後援字型</td>
</tr>
<tr class="odd">
<td align="left">Microsoft Tai Le</td>
<td align="left">標準</td>
<td align="left">傣哪文字集的後援字型</td>
</tr>
<tr class="even">
<td align="left">Microsoft YaHei</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Microsoft YaHei UI</td>
<td align="left">標準</td>
<td align="left">簡體中文的使用者介面字型</td>
</tr>
<tr class="even">
<td align="left">Microsoft Yi Baiti</td>
<td align="left">標準</td>
<td align="left">爨文字集的後援字型</td>
</tr>
<tr class="odd">
<td align="left">Mongolian Baiti</td>
<td align="left">標準</td>
<td align="left">蒙古文字集的後援字型</td>
</tr>
<tr class="even">
<td align="left">MV Boli</td>
<td align="left">標準</td>
<td align="left">塔安那文字集的後援字型</td>
</tr>
<tr class="odd">
<td align="left">Myanmar Text</td>
<td align="left">標準</td>
<td align="left">緬甸文字集的後援字型</td>
</tr>
<tr class="even">
<td align="left">Nirmala UI</td>
<td align="left">標準、Semilight、粗體</td>
<td align="left">南亞洲字集 (孟加拉文、梵文字母、古吉拉特文、果魯穆奇文、坎那達文、馬來亞拉姆文、歐迪亞文、桑塔爾文、錫蘭文、索拉僧平文、坦米爾文、特拉古文) 的使用者介面字型</td>
</tr>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">標準</td>
<td align="left">應用程式圖示的使用者介面字型</td>
</tr>
<tr class="even">
<td align="left">Segoe Print</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Segoe UI</td>
<td align="left">標準、斜體、粗體、粗斜體、淡、Semilight、Semibold、Black</td>
<td align="left">歐洲與中東字集 (阿拉伯文、亞美尼亞文、斯拉夫文、喬治亞文、希臘文、希伯來文、拉丁)，以及栗粟文字集的使用者介面字型</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">標準</td>
<td align="left">隨附於 Windows Phone 的版本在每個表情符號周圍加入白色外框，以確保在任何顏色背景都會顯示。 它與隨附於 Windows 的版本規格相容。</td>
</tr>
<tr class="odd">
<td align="left">Segoe UI Historic</td>
<td align="left">標準</td>
<td align="left">歷史字集的後援字型</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">標準</td>
<td align="left">符號的後援字型</td>
</tr>
<tr class="odd">
<td align="left">Simsun</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Times New Roman</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Trebuchet MS</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Verdana</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Webdings</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Wingdings</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="odd">
<td align="left">Yu Gothic</td>
<td align="left">中</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Yu Gothic UI</td>
<td align="left">標準</td>
<td align="left">日文的使用者介面字型</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"> </span>相關主題


**適用於設計人員**
* [標籤 (或文字區塊)](labels.md)
* [Segoe UI Symbol 圖示](segoe-ui-symbol-font.md)
**適用於開發人員 (XAML)**
* [XAML 佈景主題資源](https://msdn.microsoft.com/library/windows/apps/mt187274)
* [配置 app 頁面](https://msdn.microsoft.com/library/windows/apps/hh872191)
* [Segoe UI Symbol 圖示](segoe-ui-symbol-font.md)
* [**TextBlock.FontFamily 屬性**](https://msdn.microsoft.com/library/windows/apps/br209655)

**範例**
* [XAML 文字顯示範例](http://go.microsoft.com/fwlink/p/?linkid=238578)
* [CSS 樣式：建立應用程式商標的範例](http://go.microsoft.com/fwlink/p/?linkid=231641)
* [語言字型對應範例](http://go.microsoft.com/fwlink/p/?linkid=231603)
 

 






<!--HONumber=Mar16_HO4-->


