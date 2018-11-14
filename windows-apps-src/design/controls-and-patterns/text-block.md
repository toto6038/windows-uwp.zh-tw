---
author: Jwmsft
ms.assetid: DA562509-D893-425A-AAE6-B2AE9E9F8A19
Description: Text block is the primary control for displaying read-only text in apps.
title: 文字區塊
label: Text block
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 423634acc2d3806b4652618331fafd68c908e477
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6192554"
---
# <a name="text-block"></a>文字區塊

 

 TextBlock 是在應用程式中顯示唯讀文字的主要控制項。 您可以使用它來顯示單行或多行文字、內嵌的超連結，以及已設定格式的文字 (例如，粗體、斜體或加上底線)。
 
 > **重要 API**：[TextBlock 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.aspx)、[Text 屬性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)、[Inlines 屬性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

文字區塊通常很容易使用，且提供較 RTF 文字區塊更優異的文字轉譯效能，因此使其成為大部分應用程式 UI 文字的首選。 您可取得 [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 屬性值，以在您的應用程式中透過文字區塊輕鬆存取和使用文字。 其還提供許多可用來自訂您文字轉譯方式的相同格式設定選項。

儘管您可以在文字中放置分行符號，但文字區塊是設計來顯示單一段落，不支援文字縮排。 當您需要支援多個段落、多欄文字或其他複雜的文字配置，或是內嵌的 UI 元素 (例如影像) 時，請使用 **RichTextBlock**。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/TextBlock">開啟應用程式並查看 TextBlock 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-text-block"></a>建立文字區塊

以下說明如何定義簡易 TextBlock 控制項，以及將其 Text 屬性設定為字串。

```xaml
<TextBlock Text="Hello, world!" />
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
```

    <TextBlock Text="Hello, world!" />

    TextBlock textBlock1 = new TextBlock();
    textBlock1.Text = "Hello, world!";

### <a name="content-model"></a>內容模型

有兩個屬性可用來將內容新增到 TextBlock：[Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx) 和 [Inlines](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.inlines.aspx)。

顯示文字的最常見方法是將 Text 屬性設為字串值，如先前範例所示。

您也可以在 TextBox.Inlines 屬性中放置內嵌的流動內容元素來新增內容，如下所示。
```xaml
<TextBlock>Text can be <Bold>bold</Bold>, <Underline>underlined</Underline>, 
    <Italic>italic</Italic>, or a <Bold><Italic>combination</Italic></Bold>.</TextBlock>
```

衍生自 Inline 類別的元素 (例如 Bold、Italic、Run、Span 和 LineBreak) 會針對不同的文字部分啟用不同的格式設定。 如需詳細資訊，請參閱[格式化文字]()一節。 內嵌的 Hyperlink 元素讓您能夠在文字中新增超連結。 不過，使用 Inlines 也會停用文字轉譯的快速路徑，如下一節所述。


## <a name="performance-considerations"></a>效能考量

XAML 會在可行時使用更有效率的程式碼路徑來配置文字。 這個快速路徑會減少整體記憶體使用量，同時大幅減少文字測量和排列的 CPU 時間。 這個快速路徑僅適用於 TextBlock，因此它應該是當 RichTextBlock 可行時的慣用項目。

特定情況需要 TextBlock 針對文字轉譯讓出更豐富的功能和 CPU 密集程式碼路徑。 若要保持在快速路徑上進行文字轉譯，請確定當設定這裡列出的屬性時，遵循這些指導方針。
- [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.text.aspx)：最重要的條件是只有在您藉由明確設定 Text 屬性，在 XAML 或程式碼中 (如同前面範例所示)，才使用快速路徑。 透過 TextBlock 的 Inlines 集合 (例如 `<TextBlock>Inline text</TextBlock>`) 設定文字將會停用快速路徑，原因在於多重格式具有潛在的複雜性。
- [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.characterspacing.aspx)：只有預設值為 0 才是快速路徑。
- [TextTrimming](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.texttrimming.aspx)：只有 **None**、**CharacterEllipsis** 和 **WordEllipsis** 值才是快速路徑。 **Clip** 值會停用快速路徑。

> **注意**&nbsp;&nbsp;在 Windows 10 版本 1607 之前，其他屬性也會影響快速路徑。 如果應用程式是在舊版 Windows 上執行，這些情況也會造成您的文字在慢速路徑上轉譯。 如需版本的相關詳細資訊，請參閱＜版本調適型程式碼＞。
- [Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)：只有各種 Typography 屬性的預設值才是快速路徑。
- [LineStackingStrategy](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.linestackingstrategy.aspx)：如果 [LineHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.lineheight.aspx) 不是 0，則 **BaselineToBaseline** 和 **MaxHeight** 值會停用快速路徑。
- [IsTextSelectionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textblock.istextselectionenabled.aspx)：只有 **false** 是快速路徑。 將此屬性設為 **true** 以停用快速路徑。

您可以在偵錯期間將 [DebugSettings.IsTextPerformanceVisualizationEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.debugsettings.istextperformancevisualizationenabled.aspx) 屬性設為 **true** 以判斷文字是否使用快速路徑轉譯。 當這個屬性設為 true 時，快速路徑上的文字會以亮綠色顯示。

>**提示**&nbsp;&nbsp;這個功能已在 Build 2015 - [XAML 效能：最大化使用 XAML 建置的通用 Windows 應用程式經驗的技術](https://channel9.msdn.com/Events/Build/2015/3-698)這個研討會中進行深度說明。



您通常會在 [OnLaunched](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.application.onlaunched.aspx) 方法中設定偵錯設定，覆寫應用程式.xaml 的程式碼後置頁面，如下所示。
```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
#if DEBUG
    if (System.Diagnostics.Debugger.IsAttached)
    {
        this.DebugSettings.IsTextPerformanceVisualizationEnabled = true;
    }
#endif

// ...

}
```

在這個範例中，第一個 TextBlock 是使用快速路徑轉譯，而第二個則不是。
```xaml
<StackPanel>
    <TextBlock Text="This text is on the fast path."/>
    <TextBlock>This text is NOT on the fast path.</TextBlock>
<StackPanel/>
```

當您在偵錯模式中將 IsTextPerformanceVisualizationEnabled 設為 true 以執行此 XAML 時，結果看起來就像這樣。

![在偵錯模式中轉譯的文字](images/text-block-rendering-performance.png)

>**注意**&nbsp;&nbsp;不在快速路徑上的文字色彩不會變更。 如果您將應用程式中的文字色彩指定為亮綠色，則在較慢的轉譯路徑上顯示該文字時，仍會將它顯示為亮綠色。 請注意，不要將因為文字位於快速路徑上而在應用程式中設定為綠色的文字，以及因為偵錯設定而設定為綠色的文字相互混淆了。

## <a name="formatting-text"></a>格式化文字

雖然 Text 屬性會儲存純文字，但是您可以將不同的格式設定選項套用到 TextBlock 控制項，來自訂應用程式中轉譯文字的方式。 您可以設定標準控制項屬性 (例如，FontFamily、FontSize、FontStyle、Foreground 及 CharacterSpacing) 來變更文字的外觀。 您也可以使用內嵌文字元素和 Typography 附加屬性來設定文字的格式。 這些選項只會影響 TextBlock 在本機顯示文字的方式，因此舉例來說，如果您複製文字並貼到 RTF 控制項，即不會套用任何格式設定。

>**注意**&nbsp;&nbsp;請記住，如上一節所述，快速路徑上的內嵌文字元素和非預設印刷格式值不會轉譯。


### <a name="inline-elements"></a>內嵌元素

[Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) 命名空間提供各種不同的內嵌文字元素，讓您可用來設定文字格式，例如 Bold、Italic、Run、Span 和 LineBreak。

您可以在 TextBlock 顯示一連串的字串，其中每個字串的格式都不同。 透過 Run 元素顯示每個字串及其格式，或是以 LineBreak 元素分開每個 Run 元素，您就可以這麼做。

以下說明如何透過以 LineBreak 分隔的 Run 物件，在 TextBlock 中定義多個不同格式的文字字串。
```xaml
<TextBlock FontFamily="Segoe UI" Width="400" Text="Sample text formatting runs">
    <LineBreak/>
    <Run Foreground="Gray" FontFamily="Segoe UI Light" FontSize="24">
        Segoe UI Light 24
    </Run>
    <LineBreak/>
    <Run Foreground="Teal" FontFamily="Georgia" FontSize="18" FontStyle="Italic">
        Georgia Italic 18
    </Run>
    <LineBreak/>
    <Run Foreground="Black" FontFamily="Arial" FontSize="14" FontWeight="Bold">
        Arial Bold 14
    </Run>
</TextBlock>
```

結果如下。

![使用執行元素格式化的文字](images/text-block-run-examples.png)

### <a name="typography"></a>印刷樣式

[Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) 類別的附加屬性提供一組 Microsoft OpenType 印刷樣式屬性的存取權。 您可以在 TextBlock 或個別的內嵌文字元素上設定這些附加屬性。 這些範例示範如何使用這兩者。
```xaml
<TextBlock Text="Hello, world!"
           Typography.Capitals="SmallCaps"
           Typography.StylisticSet4="True"/>
```

```csharp
TextBlock textBlock1 = new TextBlock();
textBlock1.Text = "Hello, world!";
Windows.UI.Xaml.Documents.Typography.SetCapitals(textBlock1, FontCapitals.SmallCaps);
Windows.UI.Xaml.Documents.Typography.SetStylisticSet4(textBlock1, true);
```

```xaml
<TextBlock>12 x <Run Typography.Fraction="Slashed">1/3</Run> = 4.</TextBlock>
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [拼字檢查指導方針](text-controls.md)
- [新增搜尋](search.md)
- [文字輸入的指導方針](text-controls.md)
- [TextBox 類別](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 類別](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length property](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
