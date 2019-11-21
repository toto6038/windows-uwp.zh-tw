---
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
Description: TextBox 控制項可讓使用者在應用程式中輸入文字。
title: 文字方塊
label: Text box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 67c729455c6eb2d8f5e8b07db5e1be7ac13f59b8
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258180"
---
# <a name="text-box"></a>文字方塊

TextBox 控制項可讓使用者在應用程式中輸入文字。 其通常用來擷取單行文字，但亦可設為擷取多行文字。 文字在畫面上會以簡單、統一的純文字格式呈現。

TextBox 具有眾多可精簡文字輸入的實用功能。 其提供熟悉的內建操作功能表，支援複製與貼上文字。 「全部清除」按鈕可讓使用者快速刪除所有已輸入的文字。 其亦具備預設啟用的內建拼字檢查功能。

> **重要 API**：[TextBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)、[Text 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用者可使用 **TextBox** 控制項來輸入和編輯未格式化的文字 (例如在表單中)。 您可以使用 [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) 屬性來取得 TextBox 中的文字，並在其中設定文字。

您可將 TextBox 設為唯讀，但此應為暫時性的條件狀態。 若該文字永遠無法編輯，請考慮改用 [TextBlock](text-block.md)。

使用 [PasswordBox](password-box.md) 控制項，以收集密碼或其他私人資料，例如身分證字號。 密碼方塊看起來就像文字輸入方塊，只不過它會將已輸入的文字轉譯成項目符號。

使用 [AutoSuggestBox](auto-suggest-box.md) 控制項讓使用者輸入搜尋字詞，或向使用者顯示建議清單方便其在輸入時從中選擇。

使用 [RichEditBox](rich-edit-box.md) 顯示和編輯純文字檔案。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/TextBox">開啟應用程式並查看 TextBox 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

![文字方塊](images/text-box.png)

## <a name="create-a-text-box"></a>建立文字方塊

以下是簡易文字方塊的 XAML，其中具有標頭和預留位置文字。

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

以下是此 XAML 所產生的文字方塊。

![簡易文字方塊](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>使用文字方塊在表單中輸入資料

常見的做法是使用文字方塊支援在表單上輸入資料，並使用 [Text](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) 屬性從文字方塊取得完整的文字字串。 您通常會使用諸如按一下提交按鈕等事件存取 Text 屬性，不過若您需要在文字變更時執行某些工作，則可處理 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 或 [TextChanging](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanging) 事件。

此範例示範如何取得和設定文字方塊中目前的內容。

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```

您可以新增 [Header](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.header) (或標籤) 與 [PlaceholderText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.placeholdertext) (或浮水印) 至文字方塊，以告知使用者其用途。 若要自訂標頭的外觀，您可以設定 [HeaderTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.headertemplate) 屬性，而不是 Header。 *如需設計資訊，請參閱標籤指導方針*。

您可藉由設定 [MaxLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.maxlength) 屬性，限制使用者可以輸入的字元數目。 不過，MaxLength 不會限制已貼上文字的長度。 若對於應用程式而言具重要性，請使用 [Paste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.paste) 事件以修改貼上的文字。

文字方塊包含全部清除按鈕 (「X」)，當您在方塊中輸入文字時會顯示此按鈕。 使用者按一下「X」時，會清除文字方塊中的文字。 它的外觀如下。

![具全部清除按鈕的文字方塊](images/text-box-clear-all.png)

僅有包含文字且具焦點的可編輯單行文字方塊，才會顯示全部清除按鈕。

以下任一情況皆不會顯示全部清除按鈕：

- **IsReadOnly** 為 **true**
- **AcceptsReturn** 為 **true**
- **TextWrap** 具有非 **NoWrap** 的值

此範例示範如何取得和設定文字方塊中目前的內容。

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```


### <a name="make-a-text-box-read-only"></a>將文字方塊設為唯讀

您可藉由將 [IsReadOnly](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isreadonly) 屬性設為 **true**，使文字方塊變為唯讀。 您通常會根據您應用程式中的條件，在應用程式程式碼中切換此屬性。 若文字必須一律設為唯讀，請考慮改用 TextBlock。

您可以藉由將 IsReadOnly 屬性設定成 true，使 TextBox 變成唯讀。 例如您可能希望僅在特定條件下，啟用 TextBox 讓使用者輸入註解。 您可將 TextBox 設為在符合條件前保持唯讀狀態。 若您僅需要顯示文字，請考慮改用 TextBlock 或 RichTextBlock。

唯讀文字方塊外觀看起來與讀取/寫入文字方塊類似，因此可能會混淆使用者。
使用者可選取和複製文字。
IsEnabled

### <a name="enable-multi-line-input"></a>啟用多行輸入

您可使用兩種屬性來控制文字方塊是否採用多行方式顯示文字。 您通常會同時設定這兩個屬性，以產生多行文字方塊。

- 若要讓文字方塊允許並顯示新行或傳回字元，請將 [AcceptsReturn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.acceptsreturn) 屬性設為 **true**。
- 若要啟用文字換行，請將 [TextWrapping](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textwrapping) 屬性設為 **Wrap**。 這會導致文字在達到文字方塊邊緣時換行，不受行分隔字元的影響。

> **注意**&nbsp;&nbsp;TextBox 和 RichEditBox 的 TextWrapping 屬性均不支援 **WrapWholeWords** 值。 若您嘗試使用 WrapWholeWords 做為 TextBox.TextWrapping 或 RichEditBox.TextWrapping 的值，則會擲回無效的引數例外狀況。

多行 TextBox 大小會隨著輸入文字而繼續垂直擴展 (除非您使用其 [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 或 [MaxHeight](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.maxheight) 屬性，或是以父容器加以限制)。 您應測試多行文字方塊大小是否會擴展超出顯示範圍，並限制其擴展 (若確定會超出顯示範圍)。 我們建議您一律為多行文字方塊指定適當的高度，不讓其隨著使用者輸入文字而擴展。

必要時會啟用使用滾輪或觸控方式捲動瀏覽。 不過，依預設不會顯示垂直捲軸。 您可在內嵌 ScrollViewer 上，將 [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) 設為 [自動]  ，以顯示垂直捲軸 (如此處所示)。

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap"
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

以下是新增文字後文字方塊呈現的外觀。

![多行文字方塊](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>格式化文字顯示方式

使用 [TextAlignment](/uwp/api/windows.ui.xaml.controls.textbox.textalignment) 屬性，以對齊文字方塊中的文字。 若要在頁面配置中對齊文字方塊，請使用 [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) 和 [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) 屬性。

雖然文字方塊僅支援未格式化的文字，不過您可自訂文字在文字方塊中的顯示方式，以符合您的品牌風格。 您可設定標準[控制項](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control)屬性 (例如 [FontFamily](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontfamily)、[FontSize](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontsize)、[FontStyle](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.fontstyle)、[Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background)、[Foreground](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.foreground) 和 [CharacterSpacing](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.characterspacing))，以變更文字的外觀。 這些屬性僅會影響文字方塊在本機顯示文字的方式，因此舉例來說，若您將文字複製並貼至文字控制項，即不會套用任何格式化設定。

此範例顯示已設定數個屬性來自訂文字外觀的唯讀文字方塊。

```xaml
<TextBox Text="Sample Text" IsReadOnly="True"
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic"
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

產生的文字方塊外觀如下。

![格式化文字方塊](images/text-box-formatted.png)

### <a name="modify-the-context-menu"></a>修改操作功能表

根據預設，文字方塊操作功能表中顯示的命令會取決於文字方塊的狀態。 例如，當文字方塊可編輯時會顯示下列命令。

命令 | 顯示時機...
------- | -------------
複製 | 已選取文字。
剪下 | 已選取文字。
貼上 | 剪貼簿包含文字。
全選 | TextBox 包含文字。
復原 | 已變更文字。

若要修改操作功能表中顯示的命令，請處理 [ContextMenuOpening](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.contextmenuopening) 事件。 如需此範例，請參閱案例 2 的 [ContextMenu 範例](https://code.msdn.microsoft.com/windowsapps/Context-menu-sample-40840351)。 如需設計資訊，請參閱操作功能表指導方針。

### <a name="select-copy-and-paste"></a>選取、複製以及貼上

您可以使用 [SelectedText](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectedtext) 屬性來取得或設定文字方塊中的所選文字。 使用 [SelectionStart](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) 和 [SelectionLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) 屬性，以及 [Select](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.select) 和 [SelectAll](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectall) 方法，來操控文字選取動作。 處理 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged) 事件，可在使用者選取或取消選取文字時執行任務。 您可設定 [SelectionHighlightColor](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionhighlightcolor) 屬性，以變更反白所選文字時使用的色彩。

根據預設，TextBox 支援複製與貼上。 您可以在應用程式的可編輯文字控制項上，提供 [Paste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.paste) 事件的自訂處理。 舉例而言，您在將多行地址貼至單行搜尋方塊時，可能會移除其中的分行符號。 或者，您可能會檢查已貼上文字的長度，並在其超過可儲存至資料庫的長度上限時警告使用者。 如需詳細資訊和範例，請參閱 [Paste](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.paste) 事件。

以下是使用這些屬性與方法的範例。 您選取第一個文字方塊中的文字時，所選文字會顯示於第二個文字方塊 (唯讀)。 [SelectionLength](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) 與 [SelectionStart](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) 屬性的值會顯示在兩個文字區塊中。 這是使用 [SelectionChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged) 事件所完成。

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below."
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5"
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

以下是此程式碼的結果。

![文字方塊中選取的文字](images/text-box-selection.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>選擇文字控制項的正確鍵盤

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。

當您的應用程式在具備觸控式螢幕的裝置上執行時，可以使用觸控式鍵盤輸入文字。 當使用者點選可編輯的輸入欄位 (例如 TextBox 或 RichEditBox) 時，就會叫用觸控式鍵盤。 您可以設定文字控制項的輸入範圍，使其符合您預期使用者輸入的資料類型，讓使用者在您的應用程式中輸入資料時更加快速方便。 輸入範圍會提供控制項所預期之文字輸入類型的提示給系統，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。

例如，如果文字方塊只用來輸入 4 位數 PIN，請將 [InputScope](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.inputscope) 屬性設定為 **Number**。 這會告訴系統顯示數字鍵台配置，方便使用者輸入 PIN。

> **重要**&nbsp;&nbsp;輸入範圍並不會導致執行任何輸入驗證，也不會防止使用者透過硬體鍵盤或其他輸入裝置提供任何輸入。 您仍然必須視需要在程式碼中驗證輸入。

其他會影響觸控式鍵盤的屬性包括 [IsSpellCheckEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled)、[IsTextPredictionEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) 和 [PreventKeyboardDisplayOnProgrammaticFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus)。 (IsSpellCheckEnabled 也會影響使用硬體鍵盤時的 TextBox。)

如需詳細資訊和範例，請參閱[使用輸入範圍以變更觸控式鍵盤](https://docs.microsoft.com/windows/uwp/design/input/use-input-scope-to-change-the-touch-keyboard)與屬性文件。

## <a name="recommendations"></a>建議

- 如果文字方塊的目的不清楚，請使用標籤或預留位置文字。 無論文字輸入方塊是否包含值，都會顯示標籤。 預留位置文字會顯示在文字輸入方塊內，只要輸入值就會消失。
- 為文字方塊指定一個適合所要輸入的值範圍的寬度。 每個語言的單字長度都不相同，所以如果您希望您的應用程式能夠全球化，請考慮到當地語系化。
- 文字輸入方塊通常是單行 (`TextWrap = "NoWrap"`)。 當使用者需要輸入或編輯長字串時，請將文字輸入方塊設定為多行 (`TextWrap = "Wrap"`)。
- 文字輸入方塊通常使用於可編輯的文字。 但是您也可讓文字輸入方塊成為唯讀，如此一來，使用者可以閱讀、選取和複製它的內容，但是不能進行編輯。
- 如果您需要讓檢視看起來不那麼擁擠，請考慮將一組文字輸入方塊設定為只在選取控制的核取方塊時才顯示。 您也可以將文字輸入方塊的啟用狀態繫結至核取方塊之類的控制項。
- 請考慮當文字輸入方塊包含值且使用者點選它時，文字輸入方塊會執行什麼行為。 適當的預設行為是編輯值而不是取代它；插入點放置在單字之間，而不選取任何單字。 如果對於特定文字輸入方塊最常用的方式是取代，您可以在控制項接收焦點時選取欄位中的所有文字，而輸入文字會取代選取的文字。

### <a name="single-line-input-boxes"></a>單行輸入方塊

- 使用數個單行文字方塊來擷取許多小段的文字資訊。 如果文字方塊本質上是相關的，則將它們群組在一起。

- 讓單行文字方塊的寬度稍微大於預估的最長輸入內容。 如果這樣做會讓控制項變得太寬，請將其分成兩個控制項。 例如，您可以將單一地址輸入分割成「地址行 1」和「地址行 2」。
- 設定可以輸入的字元長度上限。 如果支援的資料來源不允許輸入長字串，請限制輸入長度，並且使用驗證快顯視窗讓使用者得知已達限制。
- 使用單行文字輸入控制項收集使用者提供的少量文字。

    下列範例顯示的單行文字方塊會擷取安全性問題的回答。 這個回答預期是簡短的回答，因此適合使用單行文字方塊。

    ![基本資料輸入](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

- 使用一組簡短、固定大小的單行文字輸入控制項，以特定格式輸入資料。

    ![格式化資料輸入](images/textinput_example_productkey.png)

- 使用不受限制的單行文字輸入控制項輸入或編輯字串，並與命令按鈕結合，幫助使用者選取有效的值。

    ![協助資料輸入](images/textinput_example_assisted.png)

### <a name="multi-line-text-input-controls"></a>多行文字輸入控制項

- 建立 RTF 方塊時，提供設定樣式按鈕並實作其動作。
- 使用與您的應用程式樣式一致的字型。
- 文字控制項的高度必須能夠容納一般輸入。
- 擷取橫跨多行但有字元或字數上限的文字時，請使用純文字方塊並且提供即時計數器，讓使用者得知達到上限之前還剩下多少字元數或字數。 您需要自行建立計數器；將它放在文字方塊下方，並且隨著使用者輸入的每個字元或文字動態更新。

    ![橫跨多行的文字](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

- 不要讓文字輸入控制項的高度隨著使用者輸入增加。
- 使用者只需要單行時，不要使用多行文字方塊。
- 使用純文字控制項就足夠時，不要使用 RTF 控制項。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [拼字檢查指導方針](text-controls.md)
- [新增搜尋](https://docs.microsoft.com/previous-versions/windows/apps/hh465231(v=win.10))
- [文字輸入的指導方針](text-controls.md)
- [TextBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [PasswordBox 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [String.Length 屬性](https://docs.microsoft.com/dotnet/api/system.string.length) \(部分機器翻譯\)
