---
Description: 在通用 Windows 平台 (UWP) app 中，命令元素是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。
title: 通用 Windows 平台 (UWP) app 的命令設計基本知識
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: 命令設計基本知識
template: detail.hbs
---

#  UWP app 的命令設計基本知識


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


在通用 Windows 平台 (UWP) app 中，*命令元素*是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。 此文件說明命令元素，例如按鈕和核取方塊、它們支援的互動，以及裝載它們的命令表面 (例如命令列和操作功能表)。

## <span id="Provide_the_right_type_of_interactions"> </span> <span id="provide_the_right_type_of_interactions"> </span> <span id="PROVIDE_THE_RIGHT_TYPE_OF_INTERACTIONS"> </span>提供正確的互動類型


設計命令介面時，最重要的是決定使用者應該可以做甚麼事情。 例如，如果您正在建立相片應用程式，使用者會需要用來編輯相片的工具。 不過，如果您正在建立會顯示相片的社交媒體應用程式，影像編輯或許不是優先考量，因此可以省略編輯工具以節省空間。 決定您希望使用者完成的事，並提供工具協助他們完成。

如需如何規劃 app 正確互動的建議，請參閱[規劃您的 app](https://msdn.microsoft.com/library/windows/apps/hh465427.aspx)。

## <span id="Use_the_right_command_element_for_the_interaction"> </span> <span id="use_the_right_command_element_for_the_interaction"> </span> <span id="USE_THE_RIGHT_COMMAND_ELEMENT_FOR_THE_INTERACTION"> </span>針對互動使用正確的命令元素


針對正確的互動使用正確的元素，可以讓人感到應用程式是直覺易用的，並與混亂難用的應用程式產生區別。 Universal Windows Platform (UWP) 在控制項的表單中，提供您許多可在應用程式中使用的命令元素。 以下是一些最常見的控制項清單以及它們啟用的互動摘要。

| 類別              | 元素                                                                                                                                                                                                            | 互動                                                                                                                                        |
|-----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|
| 按鈕               | [按鈕](https://msdn.microsoft.com/library/windows/apps/hh465470)                                                                                                                                                     | 觸發一個立即的動作，例如發送電子郵件、確認對話方塊中的動作、送出表單資料。                                    |
| 日期和時間選擇器 | [行事曆日期選擇器、行事曆檢視、日期選擇器、時間選擇器](https://msdn.microsoft.com/library/windows/apps/hh465466)                                                                                                                 | 可讓使用者檢視與修改日期和時間資訊，例如當要輸入信用卡到期日，或是設定鬧鐘時。                   |
| 清單                 | [下拉式清單、清單方塊、清單檢視與方格檢視](https://msdn.microsoft.com/library/windows/apps/mt186889)                                                                                                                                              | 呈現互動式清單或是方格中的項目。 使用這些元素，可讓使用者從最新發行的清單中選取電影，或是管理詳細目錄。 |
| 預測文字輸入 | [自動建議方塊](https://msdn.microsoft.com/library/windows/apps/dn997762)                                                                                                                                                                    | 當使用者輸入資料或執行查詢時，提供輸入建議以節省使用者的時間。                                                   |
| 選取控制項    | [核取方塊](https://msdn.microsoft.com/library/windows/apps/hh700393)、[選項按鈕](https://msdn.microsoft.com/library/windows/apps/hh700395)、[切換開關](https://msdn.microsoft.com/library/windows/apps/hh465475) | 可讓使用者選擇不同選項，例如完成問卷或是設定應用程式選項。                                      |

 

(如需完整清單，請參閱[控制項與 UI 元素](https://dev.windows.com/design/controls-patterns)。)

## <span id="_________Place_commands_on_the_right_surface_______"> </span> <span id="_________place_commands_on_the_right_surface_______"> </span> <span id="_________PLACE_COMMANDS_ON_THE_RIGHT_SURFACE_______"> </span>在正確的表面放置命令


您可以在應用程式中的數個表面放置命令元素，包括應用程式畫布 (應用程式的內容區域) 或可當作命令容器的命令元素，例如命令列、功能表、對話方塊，以及飛出視窗。 以下是一些放置命令的一般建議：

-   盡可能讓使用者在應用程式的畫布上直接操作內容，不要新增用來處理內容的命令。 例如，在旅遊應用程式中，讓使用者在畫布上拖放清單中的活動來重新安排旅遊行程，而不是選取某個活動，然後使用 [上移] 或 [下移] 命令按鈕。
-   如果使用者無法直接操作內容，請在下列這些 UI 表面上放置命令：

    -   [命令列](https://msdn.microsoft.com/library/windows/apps/hh465302)：您應該將大部分命令放在命令列，它能協助整理命令，並讓它們容易存取。
    -   應用程式畫布：如果使用者在具單一用途的頁面或檢視中，您可以在畫布直接提供該用途適用的命令。 這些命令的數量應該非常少。
    -   在[操作功能表](https://msdn.microsoft.com/library/windows/apps/hh465308)中：您可以使用操作功能表提供剪貼簿動作 (例如剪下、複製及貼上)，或是提供可針對無法選取的內容套用的命令 (例如在地圖的某個位置加上固定釘)。

以下是 Windows 提供的命令表面清單，以及何時使用的建議。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">表面</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">應用程式畫布 (內容區域)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>
<td align="left"><p>如果命令很重要，而且使用者經常用來完成核心案例，請將它放在畫布上 (應用程式內容區域)。 因為您可將命令放在靠近它們影響的物件 (或放在物件上)，將命令放在畫布上使它們更明顯、更易於使用。</p>
<p>然而，請仔細選擇要放在畫布上的命令。 應用程式畫布上過多的命令會佔去寶貴的螢幕空間，而且會妨礙使用者。 如果某個命令不常使用，請考慮將它放在其他的命令表面，例如在功能表或是命令列中的「更多」區域。</p></td>
</tr>
<tr class="even">
<td align="left">[Command bar](https://msdn.microsoft.com/library/windows/apps/hh465302)
<p><img src="images/controls-appbar-icons-200.png" alt="Example of a command bar with icons" /></p></td>
<td align="left"><p>命令列可讓使用者輕鬆存取動作。 您可以使用命令列來顯示使用者內容專用命令或選項，如相片選取或繪圖模式。</p>
<p>命令列可以放置於畫面頂端、畫面底部，或同時放置於畫面頂端與底部。 相片編輯應用程式的這個設計顯示了內容區域和命令列：</p>
<p><img src="images/commands-appcanvas-example.png" alt="A photo app" /></p>
<p>如需有關命令列的詳細資訊，請參閱[Guidelines for command bar](https://msdn.microsoft.com/library/windows/apps/hh465302)文章。</p></td>
</tr>
<tr class="odd">
<td align="left">[Menus and context menus](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left"><p>有時候將多個命令群組為命令功能表是更有效率的做法。 功能表能讓您使用更少的空間顯示更多的選項。 功能表可以包含互動式控制項。</p>
<p>操作功能表可以提供常用動作的快速鍵，並提供存取只與特定內容相關的次要命令。</p>
<p>操作功能表是針對以下類型的命令和命令情境：</p>
<ul>
<li>選取文字時的內容相關動作，例如複製、剪下、貼上、檢查拼字等等。</li>
<li>需要對其執行動作，但無法選取或指示之物件的命令。</li>
<li>顯示剪貼簿命令。</li>
<li>自訂命令。</li>
</ul>
<p>這個範例示範設計使用操作功能表來修改路線、將路線加入書籤，或選取其他車次的捷運應用程式。</p>
<p><img src="images/subway/uap-subway-ak-8in-dashboard-200.png" alt="A context menu in an subway app" /></p>
<p>如需有關操作功能表的詳細資訊，請參閱[Guidelines for context menu](https://msdn.microsoft.com/library/windows/apps/hh465308)文章。</p></td>
</tr>
<tr class="even">
<td align="left">[Dialog controls](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left"><p>對話方塊是提供內容相關應用程式資訊的強制回應 UI 重疊項目。 大部分的狀況下，對話方塊會阻擋與應用程式視窗的互動，直到對話方塊確實關閉為止，而且通常需要使用者執行某種類型的動作來關閉對話方塊。</p>
<p>對話方塊具有破壞性，因此只應在特定情況下使用。 如需詳細資訊，請參閱[When to confirm or undo actions](#whentoconfirm)一節。</p></td>
</tr>
<tr class="odd">
<td align="left">[Flyout](../controls-and-patterns/dialogs-popups-menus.md)
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left"><p>一種輕量型的內容相關快顯視窗，可顯示與使用者的動作相關的 UI。 使用飛出視窗可以：</p>
<p></p>
<ul>
<li>顯示功能表。</li>
<li>顯示有關項目的更多細節。</li>
<li>要求使用者確認動作而不中斷與應用程式的互動。</li>
</ul>
<p>只要點選或按一下飛出視窗外的地方，就可以關閉飛出視窗。 如需有關飛出控制項的詳細資訊，請參閱[Dialogs, menus, and popups](../controls-and-patterns/dialogs-popups-menus.md)文章。</p></td>
</tr>
</tbody>
</table>

 

## <span id="whentoconfirm"> </span> <span id="WHENTOCONFIRM"> </span>確認或復原動作的時機


無論是設計多良好的使用者介面和多細心的使用者，在有些時候使用者還是會執行意料外的動作而想取消。 在這些情況下，應用程式透過要求使用者確認執行動作，或提供復原最近動作的功能會十分有幫助。

-   對於無法復原和有嚴重後果的動作，建議您使用確認對話方塊。 類似動作的範例包含：
    -   覆寫檔案
    -   關閉檔案前尚未存檔
    -   確認永久刪除檔案或資料
    -   進行線上購物 (除非使用者選擇不需要確認)
    -   提交表單，例如註冊
-   對於可以復原的動作，提供簡單的復原命令即可。 類似動作的範例包含：
    -   刪除檔案
    -   刪除電子郵件 (非永久性)
    -   修改內容或編輯文字
    -   重新命名檔案

**提示** 請注意應用程式使用確認對話方塊的頻率；在使用者犯錯時雖然非常有幫助，但在使用者有意執行某些動作時也是一種妨礙。

 

## <span id="_________Optimize_for_specific_input_types_______"> </span> <span id="_________optimize_for_specific_input_types_______"> </span> <span id="_________OPTIMIZE_FOR_SPECIFIC_INPUT_TYPES_______"> </span>最佳化特定的輸入類型


如需最佳化特定輸入類型或裝置之使用者體驗的詳細資訊，請參閱[互動基本資訊](../input-and-devices/input-primer.md)。


\[此文章包含 UWP app 與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

 

 






<!--HONumber=Mar16_HO1-->


