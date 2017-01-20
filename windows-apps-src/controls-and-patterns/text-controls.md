---
author: Jwmsft
Description: "想想我們每天多常閱讀電子郵件、書籍、路標、菜單價格、胎壓標記，或是街道柱子上張貼海報中的文字。"
title: "文字控制項"
ms.assetid: 43DC68BF-FA86-43D2-8807-70A359453048
label: Text controls
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 04f8e82f05c46d41657dae0b30e04f49341bb5db

---
# <a name="text-controls"></a>文字控制項

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

文字控制項包含文字輸入方塊、密碼方塊、自動建議方塊以及文字區塊。 XAML 架構提供用於轉譯、輸入和編輯文字的數個控制項，以及一組用於格式化文字的屬性。

- 顯示唯讀文字的控制項是 [TextBlock](text-block.md) 與 [RichTextBlock](rich-text-block.md)。
- 輸入和編輯文字的控制項是：[TextBox](text-box.md)、[AutoSuggestBox](auto-suggest-box.md)、[PasswordBox](password-box.md) 和 [RichEditBox](rich-edit-box.md)。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**AutoSuggestBox 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.autosuggestbox.aspx)</li>
<li>[**PasswordBox 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx)</li>
<li>[**RichEditBox 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx)</li>
<li>[**RichTextBlock 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)</li>
<li> [**TextBlock 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)</li>
<li>[**TextBox 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx)</li>
</ul>
</div>

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

您使用的文字控制項取決於您的案例。 使用此資訊，以選擇適合在您應用程式中使用的文字控制項。

### <a name="render-read-only-text"></a>轉譯唯讀文字

使用 **TextBlock** 可在您的應用程式中顯示大部分的唯讀文字。 您可以使用它來顯示單行或多行文字、內嵌的超連結，以及已設定格式的文字 (例如，粗體、斜體或加上底線)。

TextBlock 通常很容易使用，並且提供較 RichTextBlock 更優異的文字轉譯效能，因此使其成為大部分應用程式 UI 文字的首選。 您可取得 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 屬性值，以在您的應用程式中透過 TextBlock 輕鬆存取和使用文字。

其還提供許多可用來自訂您文字轉譯方式的相同格式設定選項。 儘管您可以在文字中放置分行符號，但 TextBlock 是針對顯示單一段落而設計，不支援文字縮排。

當您需要支援多個段落、多欄文字或其他複雜的文字配置，或是內嵌的 UI 元素 (例如影像) 時，請使用 **RichTextBlock**。 RichTextBlock 提供數項適用於進階文字配置的功能。

RichTextBlock 的內容屬性是 [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx) 屬性，此屬性透過 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 元素支援以段落為基礎的文字。 它沒有您可以用來輕鬆存取應用程式中控制項文字內容的 **Text** 屬性。  

### <a name="text-input"></a>文字輸入

使用者可使用 **TextBox** 控制項來輸入和編輯未格式化的文字 (例如在表單中)。 您可以使用 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) 屬性來取得 TextBox 中的文字，並在其中設定文字。

您可將 TextBox 設為唯讀，但此應為暫時性的條件狀態。 若該文字永遠無法編輯，請考慮改用 TextBlock。

使用 **PasswordBox** 控制項，以收集密碼或其他私人資料，例如身分證字號。 密碼方塊是一個文字輸入方塊，其中隱藏基於隱私權考量而輸入的字元。 密碼方塊看起來就像文字輸入方塊，只不過它會將已輸入的文字轉譯成項目符號。 您可以自訂項目符號字元。

使用 **AutoSuggestBox** 控制項向使用者顯示建議清單，方便其在輸入時從中選擇。 自動建議方塊是一個會觸發基本搜尋建議清單的文字輸入方塊。 建議字彙可以從熱門搜尋以及使用者過去輸入的字彙的結合中得出。

您也應該使用 AutoSuggestBox 控制項實作搜尋方塊。

使用 **RichEditBox** 來顯示和編輯文字檔案。 您使用 RichEditBox 來讓使用者對應用程式進行輸入的方式與使用其他標準文字輸入方塊的方式不同。 更確切地說，您會使用它來處理與應用程式分開的文字檔案。 您通常會將輸入到 RichEditBox 的文字儲存為 .rtf 檔案。

**文字輸入是最佳選項？**

您可運用眾多方式，取得使用者在您應用程式中的輸入內容。 下列問題有助於了解最適合讓使用者輸入文字是標準文字輸入方塊，或是其他控制項。

-   **有效率地列舉所有有效的值是否實際？** 如果是，請考慮使用其中一個選取控制項，如[核取方塊](checkbox.md)、[下拉式清單](lists.md)、清單方塊、[選項按鈕](radio-button.md)、[滑桿](slider.md)、[切換開關](toggles.md)、[日期選擇器](date-and-time.md)或時間選擇器。
-   **是否有一組相對小的有效值？** 如果是，請考慮使用[下拉式清單](lists.md)或清單方塊，特別是如果這些值大於幾個字元的長度。
-   **有效資料是否完全不受限制？ 或有效資料是否只有格式上的限制 (內容長度或字元類型)？** 如果是，請使用文字輸入控制項。 您可以限制能輸入的字元數，此外還可在應用程式程式碼中驗證格式。
-   **該值代表的資料類型是否已經有特殊的通用控制項？** 如果是，請使用適當的控制項而不是文字輸入控制項。 例如，使用 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681) 接受日期輸入，不要使用文字輸入控制項。
-   如果資料只能是數值：
    -   **輸入的值是否為近似和 (或) 相對於相同頁面上的其他數量？** 如果是，請使用[滑桿](slider.md)。
    -   **在變更設定時，獲得即時回應的效果是否為使用者帶來益處？** 如果是，請使用[滑桿](slider.md)，以及可能伴隨的控制項。
    -   **輸入的值可能會在結果出現後調整 (例如音量或亮度) 嗎？** 如果是，請使用[滑桿](slider.md)。

## <a name="examples"></a>範例

文字方塊

![文字方塊](images/text-box.png)

自動建議方塊

![展開的自動建議控制項範例](images/controls_autosuggest_expanded01.png)

密碼方塊

![密碼方塊焦點狀態輸入文字](images/passwordbox-focus-typing.png)

## <a name="create-a-text-control"></a>建立文字控制項

如需每個文字控制項的特定資訊與範例，請參閱以下文章。

-   [**AutoSuggestBox**](auto-suggest-box.md)
-   [**PasswordBox**](password-box.md)
-   [**RichEditBox**](rich-edit-box.md)
-   [**RichTextBlock**](rich-text-block.md)
-   [**TextBlock**](text-block.md)
-   [**TextBox**](text-box.md)

## <a name="font-and-style-guidelines"></a>字型和樣式指導方針
如需字型指導方針，請參閱以下文章：

- [**字型指導方針**](fonts.md)
- [**Segoe MDL2 圖示清單與指導方針**](segoe-ui-symbol-font.md)


## <a name="choose-the-right-keyboard-for-your-text-control"></a>選擇文字控制項的正確鍵盤

**適用於：**TextBox、PasswordBox RichEditBox

為協助使用者使用觸控式鍵盤或螢幕輸入面板 (SIP) 輸入資料，您可以設定文字控制項的輸入範圍，使其符合使用者要輸入的資料類型。

>秘訣：此資訊僅適用於 SIP。 它並不適用於硬體鍵盤或 Windows [輕鬆存取] 選項中提供的 [螢幕小鍵盤]。

當您的應用程式在具備觸控式螢幕的裝置上執行時，可以使用觸控式鍵盤輸入文字。 當使用者點選可編輯的輸入欄位 (例如 TextBox 或 RichEditBox) 時，就會叫用觸控式鍵盤。 您可以設定文字控制項的輸入範圍，使其符合您預期使用者輸入的資料類型，讓使用者在您的應用程式中輸入資料時更加快速方便。 輸入範圍會提供控制項所預期之文字輸入類型的提示給系統，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。

例如，如果文字方塊只用來輸入 4 位數 PIN，請將 [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) 屬性設定為 **Number**。 這會告訴系統顯示數字鍵台配置，方便使用者輸入 PIN。

>重要  
>輸入範圍並不會導致執行任何輸入驗證，也不會防止使用者透過硬體鍵盤或其他輸入裝置提供任何輸入。 您仍然必須視需要在程式碼中驗證輸入。

如需詳細資訊，請參閱[使用輸入範圍來變更觸控式鍵盤](https://msdn.microsoft.com/library/windows/apps/mt280229)。

## <a name="color-fonts"></a>色彩字型

**適用於：**TextBlock、RichTextBlock、TextBox、RichEditBox

Windows 可讓字型針對每個字符包含多重色層。 例如，Segoe UI Emoji 字型會定義表情符號與其他 Emoji 字元的色彩版本。

標準和 rtf 文字控制項支援顯示色彩字型。 根據預設，**IsColorFontEnabled** 屬性為 **true**，而具有這些額外色層的字型會以彩色方式轉譯。 系統上的預設色彩字型為 Segoe UI Emoji，而控制項會改為使用此字型來顯示彩色字符。

```xaml
<TextBlock FontSize="30">Hello ☺⛄☂♨⛅</TextBlock>
```

經過轉譯的文字外觀如下：

![使用色彩字型的文字區塊](images/text-block-color-fonts.png)

如需詳細資訊，請參閱 [**IsColorFontEnabled**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.iscolorfontenabled.aspx) 屬性。

## <a name="guidelines-for-line-and-paragraph-separators"></a>行與段落分隔字元的指導方針

**適用於：**TextBlock、RichTextBlock、多行 TextBox、RichEditBox

使用行分隔字元 (0x2028) 與段落分隔字元 (0x2029) 來劃分純文字。 在每個行分隔字元後方會開始新行。 在每個段落分隔字元後方會開始新段落。

您無須在檔案中使用這些字元的檔案開始首行或首段，或是使用這些字元結束末行或末段；這樣做表示該位置具有空白的行或段落。

您的應用程式可使用行分隔字元來表示無條件的最後一行。 不過，行分隔字元不會對應至個別的歸位字元與換行字元，或是對應至這些字元的組合。 行分隔字元必須與歸位字元和換行字元分開個別處理。

您的應用程式可以在文字段落之間插入段落分隔字元。 使用此分隔字元可讓您建立純文字檔案，以在不同作業系統上使用各種不同的行寬度進行格式化。 目標系統可以忽略任何行分隔符號，而只在段落分格符號所在位置分段。

## <a name="guidelines-for-spell-checking"></a>拼字檢查指導方針

**適用於：**TextBox、RichEditBox

進行文字輸入和編輯時，拼字檢查會在拼錯的單字上以紅色波浪線醒目提示來告知使用者該字拼錯，並為使用者提供修正拼字錯誤的方式。

以下是內建的拼字檢查工具範例：

![內建的拼字檢查工具](images/spellchecking.png)

您可以針對下列兩個目的，搭配文字輸入控制項使用拼字檢查：

-   **自動校正拼字錯誤**

    如果確定能夠修正的話，拼字檢查引擎會自動校正拼字錯誤的字。 例如，引擎會自動將 "teh" 變更成 "the"。

-   **顯示替代拼法**

    當拼字檢查引擎不確定修正是否正確時，會在拼字錯誤的單字底下加上紅色底線，然後在您點選或以滑鼠右鍵按一下該單字時，於操作功能表中顯示替代單字。

-   使用拼字檢查以在使用者於文字輸入控制項中輸入單字或句子時協助使用者。 拼字檢查可與觸控、滑鼠及鍵盤輸入搭配使用。
-   當單字不太可能出現在字典中或使用者不重視拼字檢查時，請勿使用拼字檢查。 例如，如果文字方塊是要用來擷取電話號碼或名稱，請勿開啟此功能。
-   不要只是因為目前的拼字檢查引擎不支援您的應用程式語言，就停用拼字檢查。 當拼字檢查工具不支援語言時，它不會執行任何動作，所以保留啟用該選項並不會造成損害。 另外，有些使用者可能會使用輸入法 (IME) 將另一種語言輸入您的應用程式中，可能就會支援該語言。 例如，建置日文應用程式時，雖然拼字檢查引擎目前無法辨識該語言，但是不要將拼字檢查關閉。 使用者可能會切換到英文 IME，然後在應用程式中輸入英文；如果已啟用拼字檢查，就會進行英文拼字檢查。

TextBox 和 RichEditBox 控制項預設會開啟拼字檢查。 您可以將 **IsSpellCheckEnabled** 屬性設定為 **true** 來關閉它。

## <a name="related-articles"></a>相關文章

**適用於設計人員**
- [**字型指導方針**](fonts.md)
- [**Segoe MDL2 圖示清單與指導方針**](segoe-ui-symbol-font.md)
- [新增搜尋](https://msdn.microsoft.com/library/windows/apps/hh465231)

**適用於開發人員 (XAML)**
- [**TextBox 類別**](https://msdn.microsoft.com/library/windows/apps/br209683)
- [**Windows.UI.Xaml.Controls PasswordBox 類別**](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length property](https://msdn.microsoft.com/library/system.string.length.aspx)



<!--HONumber=Dec16_HO2-->


