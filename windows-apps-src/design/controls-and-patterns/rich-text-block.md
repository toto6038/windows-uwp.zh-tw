---
author: Jwmsft
Description: Use a RichTextBlock with RichTextBlockOverflow elements to create advanced text layouts.
title: RichTextBlock
ms.assetid: E4BE4B1B-418E-4075-88F1-22C09DDF8E45
label: Rich text block
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 16ebc375a72984af8bbc40823121d2d94689fcf2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039010"
---
# <a name="rich-text-block"></a>RTF 區塊

 

RTF 區塊提供數個適用於進階文字配置的功能，當您需要支援段落、內嵌的 UI 元素或複雜的文字配置時，可以使用 RTF 區塊。

> **重要 API**：[RichTextBlock 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.aspx)、[RichTextBlockOverflow 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx)、[Paragraph 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx)、[Typography 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您需要支援多個段落、多欄或其他複雜的文字配置，或內嵌的 UI 元素 (例如影像) 時，請使用 **RichTextBlock**。

使用 **TextBlock** 可在您的 app 中顯示大部分的唯讀文字。 您可以使用它來顯示單行或多行文字、內嵌的超連結，以及已設定格式的文字 (例如，粗體、斜體或加上底線)。 TextBlock 提供較簡單的內容模型，如此一來，通常就能讓它更容易使用，而且它可以提供比 RichTextBlock 更好的文字轉譯效能。 大部分的 app UI 文字都慣用這個控制項。 儘管您可以在文字中放置分行符號，但 TextBlock 是針對顯示單一段落而設計，不支援文字縮排。

如需如何選擇正確文字控制項的詳細資訊，請參閱[文字控制項](text-controls.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/RichTextBlock">開啟應用程式並查看 RichTextBlock 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-rich-text-block"></a>建立 RTF 區塊

RichTextBlock 的內容屬性是 [Blocks](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.blocks.aspx) 屬性，此屬性透過 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 元素支援以段落為基礎的文字。 它沒有您可以用來輕鬆存取 app 中控制項文字內容的 **Text** 屬性。 不過，RichTextBlock 提供數種 TextBlock 不提供的獨特功能。 

RichTextBlock 支援：
- 多個段落。 藉由設定 [TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx) 屬性來設定段落的縮排。
- 內嵌的 UI 元素。 使用 [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) 來顯示 UI 元素，例如內嵌文字的影像。
- 溢出區的容器。 使用 [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) 元素來建立多欄文字配置。

### <a name="paragraphs"></a>段落

您可以使用 [Paragraph](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.aspx) 元素來定義要顯示在 RichTextBlock 控制項內的文字區塊。 每個 RichTextBlock 應該至少包含一個 Paragraph。 

您可以透過在 RichTextBlock 中設定 [RichTextBlock.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblock.textindent.aspx) 屬性，來設定所有段落的縮排量。 您可以在 RichTextBlock 中，將 [Paragraph.TextIndent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.paragraph.textindent.aspx) 屬性設為不同的值，來覆寫特定段落的此項設定。

```xaml
<RichTextBlock TextIndent="12">
  <Paragraph TextIndent="24">First paragraph.</Paragraph>
  <Paragraph>Second paragraph.</Paragraph>
  <Paragraph>Third paragraph. <Bold>With an inline.</Bold></Paragraph>
</RichTextBlock>
```

### <a name="inline-ui-elements"></a>內嵌的 UI 元素

[InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.inlineuicontainer.aspx) 類別可讓您將任何 UIElement 內嵌於文字中。 常見的案例是將 Image 與您的文字內嵌在一起，但是您也可以使用互動式元素，例如 Button 或 CheckBox。

如果您想在相同位置中內嵌多個元素，請考慮使用面板做為單一 InlineUIContainer 子項，然後在該面板中放置多個元素。

以下範例示範如何使用 InlineUIContainer 將影像插入 RichTextBlock。 

```xaml
<RichTextBlock>
    <Paragraph>
        <Italic>This is an inline image.</Italic>
        <InlineUIContainer>
            <Image Source="Assets/Square44x44Logo.png" Height="30" Width="30"/>
        </InlineUIContainer>
        Mauris auctor tincidunt auctor.
    </Paragraph>
</RichTextBlock>
```

## <a name="overflow-containers"></a>溢出區的容器

您可以使用 RichTextBlock 搭配 [RichTextBlockOverflow](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richtextblockoverflow.aspx) 元素來建立多欄或其他進階頁面配置。 RichTextBlockOverflow 元素的內容一律來自 RichTextBlock 元素。 您可以將 RichTextBlockOverflow 元素設定為 RichTextBlock 的 OverflowContentTarget 或另一個 RichTextBlockOverflow 來連結它們。

以下這個簡單範例會建立雙欄配置。 請參閱＜範例＞一節，以取得更複雜的範例。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition/>
        <ColumnDefinition/>
    </Grid.ColumnDefinitions>
    <RichTextBlock Grid.Column="0" 
                   OverflowContentTarget="{Binding ElementName=overflowContainer}" >
        <Paragraph>
            Proin ac metus at quam luctus ultricies.
        </Paragraph>
    </RichTextBlock>
    <RichTextBlockOverflow x:Name="overflowContainer" Grid.Column="1"/>
</Grid>
```

## <a name="formatting-text"></a>格式化文字

雖然 RichTextBlock 會儲存純文字，但是您可以套用不同的格式設定選項來自訂 app 中轉譯文字的方式。 您可以設定標準控制項屬性 (例如，FontFamily、FontSize、FontStyle、Foreground 及 CharacterSpacing) 來變更文字的外觀。 您也可以使用內嵌文字元素和 Typography 附加屬性來設定文字的格式。 這些選項只會影響 RichTextBlock 在本機顯示文字的方式，因此，舉例來說，如果您複製文字並貼到 RTF 控制項，就不會未套用任何格式設定。

### <a name="inline-elements"></a>內嵌元素

[Windows.UI.Xaml.Documents](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.aspx) 命名空間提供各種不同的內嵌文字元素，讓您可用來設定文字格式，例如 Bold、Italic、Run、Span 和 LineBreak。 將格式設定套用到區段的標準方法是將文字放置在 Run 或 Span 元素中，然後在該元素上設定屬性。

以下這個 Paragraph 的第一個片語會以粗體、藍色 16pt 的文字來顯示。

```xaml
<Paragraph>
    <Bold><Span Foreground="DarkSlateBlue" FontSize="16">Lorem ipsum dolor sit amet</Span></Bold>
    , consectetur adipiscing elit.
</Paragraph>
```

### <a name="typography"></a>印刷樣式

[Typography](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.documents.typography.aspx) 類別的附加屬性提供一組 Microsoft OpenType 印刷樣式屬性的存取權。 您可以在 RichTextBlock 或個別的內嵌文字元素上設定這些附加屬性，如下所示。

```xaml
<RichTextBlock Typography.StylisticSet4="True">
    <Paragraph>
        <Span Typography.Capitals="SmallCaps">Lorem ipsum dolor sit amet</Span>
        , consectetur adipiscing elit.
    </Paragraph>
</RichTextBlock>
```

## <a name="recommendations"></a>建議

請參閱＜印刷樣式與字型的指導方針＞。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

[文字控制項](text-controls.md)

**適用於設計人員**
- [拼字檢查的指導方針](text-controls.md)
- [新增搜尋](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [文字輸入的指導方針](text-controls.md)

**適用於開發人員 (XAML)**
- [TextBox 類別](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox 類別](https://msdn.microsoft.com/library/windows/apps/br227519)


**適用於開發人員 (其他)**
- [String.Length property](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
