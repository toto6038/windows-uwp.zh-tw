---
author: Karl-Bridge-Microsoft
Description: 使用可在外部應用程式中啟動及執行單一動作的語音命令，來延伸 Cortana 的基本功能。
title: Cortana 互動
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
---

# UWP app 中的 Cortana 互動




使用可在外部應用程式中啟動及執行單一動作的語音命令，來延伸 **Cortana** 的基本功能。 


**其他語音元件**

-   如需直接整合語音辨識和文字轉換語音 (也稱為 TTS，或語音合成) 到應用程式，請參閱[語音設計指導方針](speech-interactions.md)。

> **注意**  
> 語音命令定義在語音命令定義 (VCD) 檔中，它是一種具有特定用途的單次語言表達，會透過 **Cortana** 導向已安裝的應用程式。

> VCD 檔案會定義一或多個語音命令，每一個都具有唯一的用途。

> 語音命令定義的複雜度各有不同。 它的支援範圍可從單一、限制的語句，到更具彈性、自然的語言表達集合，而這全都表示同樣的用途。


目標應用程式可以在前景 (應用程式取得焦點而 **Cortana** 失去焦點) 或背景啟動 (**Cortana** 保持焦點但提供來自應用程式應用程式的結果)，這根據互動的複雜性而定。 例如，需要額外內容或使用者輸入的語音命令最好在前景應用程式處理，而基本命令則可由 **Cortana** 透過背景應用程式處理。

 

整合應用程式的基本功能，並提供一個中心進入點，讓使用者不需要開啟應用程式就能直接完成大部分的工作，讓 **Cortana** 成為您的應用程式與使用者之間的橋樑。 將此捷徑提供給應用程式功能以及減少切換應用程式的需求，可以讓使用者節省很多時間和精力。


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">文章</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[設計指導方針](cortana-design-guidelines.md)</p></td>
<td align="left"><p>這些指導方針與建議會說明您的 app 如何充分利用 **Cortana** 與使用者互動，以協助他們完成工作，並清楚傳達發生的過程。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用語音命令啟動前景應用程式](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>除了使用 <strong>Cortana</strong> 中的語音命令來存取系統功能之外，您也可以透過 <strong>Cortana</strong> 使用語音命令來啟動前景應用程式，以及指定要在應用程式中執行的動作或命令。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[動態修改 VCD 片語清單](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>了解如何在執行期間，利用語音辨識結果來存取和更新 VCD 檔案中支援的片語清單 (<strong>PhraseList</strong> 元素)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[使用語音命令啟動背景應用程式](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>除了透過 <strong>Cortana</strong> 中的語音命令來使用系統功能之外，有一些具備語音命令功能的背景應用程式，可以指定要在 app 中執行的動作或命令，利用這些背景應用程式，就可以擴充 <strong>Cortana</strong> 特性與功能。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[與背景應用程式互動](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>了解使用者如何在執行語音命令時，透過 <strong>Cortana</strong> 語音和畫布與背景應用程式互動。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[背景應用程式的深層連結](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>在 <strong>Cortana</strong> 中提供來自背景應用程式服務的深層連結，以便將 App 啟動到處於特定狀態或內容的前景。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[支援自然語言語音命令](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>了解如何利用更具彈性且自然的語音命令來延伸 <strong>Cortana</strong> 的功能，讓使用者可以在命令中的任何地方說出 App 名稱。</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>相關文章


* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**設計人員**
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


