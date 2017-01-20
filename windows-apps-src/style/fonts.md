---
author: Jwmsft
Description: "為 UWP app 選取字型及指定字型大小與色彩時，請遵循這些指導方針。"
title: "適用於 UWP app 的字型"
ms.assetid: 1B8B90AD-CDC4-4997-ACDE-871C1E94A929
label: Fonts
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 0b25dc91a5ec82a83ae24a41854e9eeab8990128

---


# <a name="fonts-for-uwp-apps"></a>適用於 UWP app 的字型

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

本文列出針對 UWP app 建議的字型。 在所有支援 UWP app 的 Windows 10 版本中保證都有提供這些字型。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**FontFamily 屬性**](https://msdn.microsoft.com/library/windows/apps/br209655)</li>
</ul>
</div>

 [UWP 印刷樣式指南](typography.md)建議 App 使用 Segoe UI 字型，不過，雖然 Segoe UI 是適用於大多數 App 的絕佳選擇，但是您不一定要將它用於所有項目。 您可能會針對某些情況 (例如閱讀) 或在顯示某些非英文語言的文字時使用其他字型。 
 
## <a name="sans-serif-fonts"></a>Sans-serif 字型

Sans-serif 字型是標題和 UI 元素的絕佳選擇。 

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
<th align="left">注意事項</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left" style="font-family: Arial;">Arial</td>
<td align="left">標準、斜體、粗體、粗斜體、黑體</td>
<td align="left">支援歐洲與中東字集 (拉丁文、希臘文、斯拉夫文、阿拉伯文、亞美尼亞文及希伯來文) 黑色粗細僅支援歐洲字集。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Calibri;">Calibri</td>
<td align="left">標準、斜體、粗體、粗斜體、細體、細斜體</td>
<td align="left">支援歐洲與中東字集 (拉丁文、希臘文、斯拉夫文、阿拉伯文及希伯來文)。 阿拉伯文只提供直書形式。</td>
</tr>
<td style="font-family: Consolas;">Consolas</td>
<td>標準、斜體、粗體、粗斜體</td>
<td>支援歐洲字集的固定寬度字型 (拉丁文、希臘文及斯拉夫文)。</td>
</tr>

<tr>
<td style="font-family: Segoe UI;">Segoe UI</td>
<td>標準、斜體、細斜體、黑斜體、粗體、粗斜體、細體、半細體、半粗體、黑體</td>
<td>歐洲與中東字集 (阿拉伯文、亞美尼亞文、斯拉夫文、喬治亞文、希臘文、希伯來文、拉丁文) 以及栗粟文字集的使用者介面字型。</td>
</tr>

<tr class="odd">
<td>Segoe UI Historic</td>
<td align="left">標準</td>
<td align="left">歷史字集的後援字型</td>
</tr>

<tr class="even">
<td style="font-family: Selawik;">Selawik</td>
<td align="left">標準、半細體、細體、粗體、半粗體</td>
<td align="left">在格律上與 Segoe UI 相容的開放原始碼字型，用於其他不想要與 Segoe UI 搭配之平台上的 App。 [從 GitHub 取得 Selawik。](https://github.com/Microsoft/Selawik)</td>
</tr>

<tr class="even">
<td style="font-family: Verdana;">Verdana</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">支援歐洲字集 (拉丁文、希臘文及亞美尼亞文)。</td>
</tr>

</tbody>
</table>


## <a name="serif-fonts"></a>Serif 字型

Serif 字型適合呈現大量的文字。 

<table>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">樣式</th>
<th align="left">注意事項</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="font-family: Cambria;">Cambria</td>
<td align="left">標準</td>
<td align="left">支援歐洲字集的 Serif 字型 (拉丁文、希臘文、斯拉夫文)。</td>
</tr>
<tr class="even">
<td style="font-family: Courier New;">Courier New</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">Serif 固定寬度字型支援歐洲與中東字集 (拉丁文、希臘文、斯拉夫文、阿拉伯文、亞美尼亞文及希伯來文)。</td>
</tr>
<tr class="odd">
<td style="font-family: Georgia;">Georgia</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">支援歐洲字集 (拉丁文、希臘文及斯拉夫文)。</td>
</tr>


<tr class="even">
<td style="font-family: Times New Roman;">Times New Roman</td>
<td align="left">標準、斜體、粗體、粗斜體</td>
<td align="left">支援歐洲字集的傳統字型 (拉丁文、希臘文、斯拉夫文、阿拉伯文、亞美尼亞文、希伯來文)。</td>
</tr>

</tbody>
</table>

## <a name="symbols-and-icons"></a>符號與圖示


<table>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">樣式</th>
<th align="left">注意事項</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Segoe MDL2 Assets</td>
<td align="left">標準</td>
<td align="left">App 圖示的使用者介面字型。 如需詳細資訊，請參閱 [Segoe MDL2 Assets 文章](segoe-ui-symbol-font.md)。</td>
</tr>
<tr class="even">
<td align="left">Segoe UI Emoji</td>
<td align="left">標準</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left">Segoe UI Symbol</td>
<td align="left">標準</td>
<td align="left">符號的後援字型</td>
</tr>
</tbody>
</table>



## <a name="fonts-for-non-latin-languages"></a>非拉丁語言的字型

不過，許多這些字型都有提供拉丁字元。

<table>
<thead>
<tr class="header">
<th align="left">字型家族</th>
<th align="left">樣式</th>
<th align="left">注意事項</th>
</tr>
</thead>
<tbody>

<tr class="odd">
<td style="font-family: Embrima;">Ebrima</td>
<td align="left">標準、粗體</td>
<td align="left">非洲字集 (衣索比亞文、西非書面文、歐斯馬雅文、提弗納文、范文) 的使用者介面字型。</td>
</tr>
<tr class="even">
<td style="font-family: Gadugi;">Gadugi</td>
<td align="left">標準、粗體</td>
<td align="left">北美洲字集 (加拿大語音節、卻洛奇文) 的使用者介面字型。</td>
</tr>
<tr class="even">
<td align="left">Javanese Text Regular 爪哇文字集的後援字型</td>
<td align="left">標準</td>
<td align="left">爪哇字集的後援字型</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Leelawadee UI;">Leelawadee UI</td>
<td align="left">標準、半細體、粗體</td>
<td align="left">東南亞字集 (布吉斯文、寮文、高棉文、泰文) 的使用者介面字型。</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: Malgun Gothic;">Malgun Gothic</td>
<td align="left">標準</td>
<td align="left">韓文的使用者介面字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Himalaya;">Microsoft Himalaya</td>
<td align="left">標準</td>
<td align="left">藏文字集的後援字型。</td>
</tr>
<!--
<tr class="odd">
<td align="left" style="font-family: Microsoft JhengHei;">Microsoft JhengHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="even">
<td align="left" style="font-family: Microsoft JhengHei UI;">Microsoft JhengHei UI</td>
<td align="left">標準、粗體、細體</td>
<td align="left">繁體中文的使用者介面字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft New Tai Lue;">Microsoft New Tai Lue</td>
<td align="left">標準</td>
<td align="left">新傣文字集的後援字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft PhagsPa;">Microsoft PhagsPa</td>
<td align="left">標準</td>
<td align="left">八思巴文字集的後援字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Microsoft Tai Le;">Microsoft Tai Le</td>
<td align="left">標準</td>
<td align="left">傣哪文字集的後援字型。</td>
</tr>
<!--
<tr class="even">
<td align="left" style="font-family: Microsoft YaHei;">Microsoft YaHei</td>
<td align="left">Regular</td>
<td align="left"></td>
</tr>
-->
<tr class="odd">
<td align="left" style="font-family: Microsoft YaHei UI;">Microsoft YaHei UI</td>
<td align="left">標準、粗體、細體</td>
<td align="left">簡體中文的使用者介面字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Microsoft Yi Baiti;">Microsoft Yi Baiti</td>
<td align="left">標準</td>
<td align="left">爨文字集的後援字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Mongolian Baiti;">Mongolian Baiti</td>
<td align="left">標準</td>
<td align="left">蒙古文字集的後援字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: MV Boli;">MV Boli</td>
<td align="left">標準</td>
<td align="left">塔安那文字集的後援字型。</td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Myanmar Text;">Myanmar Text</td>
<td align="left">標準</td>
<td align="left">緬甸文字集的後援字型。</td>
</tr>
<tr class="even">
<td align="left" style="font-family: Nirmala UI;">Nirmala UI</td>
<td align="left">標準、Semilight、粗體</td>
<td align="left">南亞字集 (孟加拉文、梵文字母、古吉拉特文、果魯穆奇文、坎那達文、馬來亞拉姆文、歐迪亞文、桑塔爾文、僧伽羅文、索拉僧平文、坦米爾文、特拉古文) 的使用者介面字型</td>
</tr>

<tr class="odd">
<td align="left" style="font-family: SimSun;">SimSun</td>
<td align="left">標準</td>
<td align="left">傳統中文 UI 字型。 </td>
</tr>
<tr class="odd">
<td align="left" style="font-family: Yu Gothic;">Yu Gothic</td>
<td align="left">中</td>
<td align="left"></td>
</tr>
<tr class="even">
<td align="left" style="font-family: Yu Gothic UI;">Yu Gothic UI</td>
<td align="left">標準</td>
<td align="left">日文的使用者介面字型。</td>
</tr>
</tbody>
</table>


## <a name="globalizinglocalizing-fonts"></a>將字型全球化/當地語系化
您可以使用 [LanguageFont 字型對應 API](https://msdn.microsoft.com/library/windows/apps/br206864)，以程式設計方式存取特定語言的建議字型系列、大小、粗細及樣式。 LanguageFont 物件會針對各種內容類別 (包括 UI 標頭、通知、內文，以及使用者可以編輯的文件內文字型)，提供正確字型資訊的存取權。 如需詳細資訊，請參閱[調整配置和字型以支援全球化](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)。


## <a name="get-the-samples"></a>取得範例

* [可下載的字型範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlCloudFontIntegration)
* [UI 基礎範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)
* [DirectWrite 的行距範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/DWriteLineSpacingModes) 

## <a name="related-articles"></a>相關文章

* [調整配置和字型以支援全球化](https://msdn.microsoft.com/windows/uwp/globalizing/adjust-layout-and-fonts--and-support-rtl)
* [Segoe MDL2](segoe-ui-symbol-font.md)
* [文字控制項](../controls-and-patterns/text-controls.md)
* [XAML 佈景主題資源](https://msdn.microsoft.com/library/windows/apps/mt187274)

 

 







<!--HONumber=Dec16_HO2-->


