---
author: Jwmsft
Description: Theme resources in XAML are a set of resources that apply different values depending on which system theme is active.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_theme\_resources
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: XAML 佈景主題資源
ms.assetid: 41B87DBF-E7A2-44E9-BEBA-AF6EEBABB81B
label: XAML theme resources
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e576814617204749a37963ac5f2724f290520349
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5822622"
---
# <a name="xaml-theme-resources"></a>XAML 佈景主題資源

XAML 中的佈景主題資源是一組資源，可根據作用的系統佈景主題套用不同的值。 XAML 架構支援的佈景主題有 3 個：Light、Dark 和 HighContrast。

**必要條件**：本主題假設您已閱讀 [ResourceDictionary 與 XAML 資源參考](resourcedictionary-and-xaml-resource-references.md)。

## <a name="theme-resources-v-static-resources"></a>佈景主題資源與 靜態資源

有兩個 XAML 標記延伸可以參考現有 XAML 資源字典中的 XAML 資源：[{StaticResource} 標記延伸](../../xaml-platform/staticresource-markup-extension.md)和 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)。

在應用程式載入時以及後續每次在執行階段變更佈景主題時，即會評估 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)。 這通常是因為使用者變更其裝置設定所導致，或者因為在應用程式內以程式設計方式更改其目前佈景主題所導致。

相反地，只有在應用程式第一次載入 XAML 時，才會評估 [{StaticResource} 標記延伸](../../xaml-platform/staticresource-markup-extension.md)。 它不會更新。 這類似於在應用程式啟動時使用實際的執行階段值，在 XAML 中進行尋找並取代。

## <a name="theme-resources-in-the-resource-dictionary-structure"></a>資源字典結構中的佈景主題資源

每個佈景主題資源都是 XAML 檔案 themeresources.xaml 的一部分。 基於設計目的，Windows 軟體開發套件 (SDK) 安裝的 \\(Program Files)\\Windows Kits\\10\\DesignTime\\CommonConfiguration\\Neutral\\UAP\\&lt;SDK version&gt;\\Generic 資料夾中會提供 themeresources.xaml。 themeresources.xaml 中的資源字典也會重現於相同目錄的 generic.xaml 中。

Windows 執行階段不會使用這些實體檔案進行執行階段查詢。 因此特別將它們放在 DesignTime 資料夾中，而且預設也不會複製到應用程式。 相反地，這些資源字典會保留在記憶體中成為 Windows 執行階段本身的一部分，而您應用程式的 XAML 資源會參考在執行階段於記憶體中解析的佈景主題資源 (或系統資源)。

## <a name="guidelines-for-custom-theme-resources"></a>自訂佈景主題資源的指導方針

當您定義並取用自己的自訂佈景主題資源時，請遵循下列指導方針：

- 除了 "HighContrast" 字典之外，也請為 "Light" 和 "Dark" 指定佈景主題字典。 雖然您可以使用 "Default" 做為索引鍵來建立 [ResourceDictionary](https://msdn.microsoft.com/library/windows/apps/br208794)，但最好是採用明確的方式，改用 "Light"、"Dark" 及 "HighContrast"。

- 在以下項目中使用 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)：樣式、Setter、控制項範本、屬性 Setter，以及動畫。

- 不要在 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 內的資源定義中使用 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)。 改用 [{StaticResource} 標記延伸](../../xaml-platform/staticresource-markup-extension.md)。

    例外狀況：您可以使用 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)來參考 [ThemeDictionaries](https://msdn.microsoft.com/library/windows/apps/br208807) 中，應用程式佈景主題無從驗證的資源。 這些資源的範例為輔色資源 (例如 `SystemAccentColor`)，或系統色彩資源 (通常包含 "SystemColor" 前置碼，例如 `SystemColorButtonFaceColor`)。

> [!CAUTION]
> 如果您未遵循這些指導方針，可能會看到與您的應用程式中佈景主題相關的非預期行為。 如需詳細資訊，請參閱[疑難排解佈景主題資源](#troubleshooting-theme-resources)一節。

## <a name="the-xaml-color-ramp-and-theme-dependent-brushes"></a>XAML 色彩坡形和佈景主題相依的筆刷

適用於 "Light"、"Dark" 及 "HighContrast" 佈景主題的色彩組合可在 XAML 中組成 [Windows 色彩坡度]**。 不論您是否想要修改系統佈景主題，或者將系統佈景主題套用到自己的 XAML 元素，都請務必了解色彩資源的結構。

如需有關如何在 UWP app 中套件色彩的詳細資訊，請參閱 [UWP app 中的色彩](../style/color.md)。

### <a name="light-and-dark-theme-colors"></a>Light 和 Dark 佈景主題色彩

XAML 架構提供一組已命名的 [Color](/uwp/api/Windows.UI.Color) 資源，其中包含針對 "Light" 和 "Dark" 佈景主題量身訂做的值。 您用來參考這些項目的索引鑰需遵循下列命名格式：`System[Simple Light/Dark Name]Color`。

此表格會列出索引鍵、簡單名稱，以及代表色彩的字串 (使用 \#aarrggbb 格式)，適用於 XAML 架構提供的 "Light" 和 "Dark" 資源。 索引鑰可用來參考應用程式中的資源。 「簡單的亮色調/暗色調名稱」是用來做為筆刷命名慣例的一部分 (稍後將會說明)。

| 索引鍵                             | 簡單的亮色調/暗色調名稱 | 亮色調      | 暗色調       |
|---------------------------------|------------------------|------------|------------|
| SystemAltHighColor              | AltHigh                | \#FFFFFFFF | \#FF000000 |
| SystemAltLowColor               | AltLow                 | \#33FFFFFF | \#33000000 |
| SystemAltMediumColor            | AltMedium              | \#99FFFFFF | \#99000000 |
| SystemAltMediumHighColor        | AltMediumHigh          | \#CCFFFFFF | \#CC000000 |
| SystemAltMediumLowColor         | AltMediumLow           | \#66FFFFFF | \#66000000 |
| SystemBaseHighColor             | BaseHigh               | \#FF000000 | \#FFFFFFFF |
| SystemBaseLowColor              | BaseLow                | \#33000000 | \#33FFFFFF |
| SystemBaseMediumColor           | BaseMedium             | \#99000000 | \#99FFFFFF |
| SystemBaseMediumHighColor       | BaseMediumHigh         | \#CC000000 | \#CCFFFFFF |
| SystemBaseMediumLowColor        | BaseMediumLow          | \#66000000 | \#66FFFFFF |
| SystemChromeAltLowColor         | ChromeAltLow           | \#FF171717 | \#FFF2F2F2 |
| SystemChromeBlackHighColor      | ChromeBlackHigh        | \#FF000000 | \#FF000000 |
| SystemChromeBlackLowColor       | ChromeBlackLow         | \#33000000 | \#33000000 |
| SystemChromeBlackMediumLowColor | ChromeBlackMediumLow   | \#66000000 | \#66000000 |
| SystemChromeBlackMediumColor    | ChromeBlackMedium      | \#CC000000 | \#CC000000 |
| SystemChromeDisabledHighColor   | ChromeDisabledHigh     | \#FFCCCCCC | \#FF333333 |
| SystemChromeDisabledLowColor    | ChromeDisabledLow      | \#FF7A7A7A | \#FF858585 |
| SystemChromeHighColor           | ChromeHigh             | \#FFCCCCCC | \#FF767676 |
| SystemChromeLowColor            | ChromeLow              | \#FFF2F2F2 | \#FF171717 |
| SystemChromeMediumColor         | ChromeMedium           | \#FFE6E6E6 | \#FF1F1F1F |
| SystemChromeMediumLowColor      | ChromeMediumLow        | \#FFF2F2F2 | \#FF2B2B2B |
| SystemChromeWhiteColor          | ChromeWhite            | \#FFFFFFFF | \#FFFFFFFF |
| SystemListLowColor              | ListLow                | \#19000000 | \#19FFFFFF |
| SystemListMediumColor           | ListMedium             | \#33000000 | \#33FFFFFF |

:::row:::
    :::column:::
        #### Light theme
    :::column-end:::
    :::column:::
        #### Dark theme
    :::column-end:::
:::row-end:::

#### <a name="base"></a>Base

:::row:::
    :::column:::
        ![The base light theme](images/themes/light-base.png)
    :::column-end:::
    :::column:::
        ![The base dark theme](images/themes/dark-base.png)
    :::column-end:::
:::row-end:::

#### <a name="alt"></a>Alt

:::row:::
    :::column:::
        ![The alt light theme](images/themes/light-alt.png)
    :::column-end:::
    :::column:::
        ![The alt dark theme](images/themes/dark-alt.png)
    :::column-end:::
:::row-end:::

#### <a name="list"></a>清單

:::row:::
    :::column:::
        ![The list light theme](images/themes/light-list.png)
    :::column-end:::
    :::column:::
        ![The list dark theme](images/themes/dark-list.png)
    :::column-end:::
:::row-end:::

#### <a name="chrome"></a>Chrome

:::row:::
    :::column:::
        ![The chrome light theme](images/themes/light-chrome.png)
    :::column-end:::
    :::column:::
        ![The chrome dark theme](images/themes/dark-chrome.png)
    :::column-end:::
:::row-end:::

### <a name="windows-system-high-contrast-colors"></a>Windows 系統的高對比色彩

除了由 XAML 架構提供的資源組，還有一組衍生自 Windows 系統調色盤的色彩值。 這些色彩並不是 Windows 執行階段或通用 Windows 平台 (UWP) 應用程式專用的。 不過，當系統的運作 (和應用程式的執行) 是使用 "HighContrast" 佈景主題時，有許多 XAML [Brush](/uwp/api/Windows.UI.Xaml.Media.Brush) 資源會取用這些色彩。 XAML 架構提供這些全系統的色彩做為已設定索引鍵的資源。 索引鍵需遵循下列命名格式：`SystemColor[name]Color`。

此表格列出全系統的色彩，XAML 可提供來做為衍生自 Windows 系統調色盤的資源物件。 [輕鬆存取名稱] 欄顯示如何在 Windows 設定 UI 中標示色彩。 [簡單的高對比名稱] 欄是簡單的說明，描述色彩如何套用到 XAML 通用控制項的方式。 它是用來做為筆刷命名慣例的一部分 (稍後將會說明)。 [初始預設值] 欄顯示若系統完全不是以高對比執行時您會獲得的值。

| 索引鍵                           | 輕鬆存取名稱            | 簡單的高對比名稱 | 初始預設值 |
|-------------------------------|--------------------------------|--------------------------|-----------------|
| SystemColorButtonFaceColor    | **按鈕文字** (背景)   | 背景               | \#FFF0F0F0      |
| SystemColorButtonTextColor    | **按鈕文字** (前景)   | 前景               | \#FF000000      |
| SystemColorGrayTextColor      | **停用的文字**              | 已停用                 | \#FF6D6D6D      |
| SystemColorHighlightColor     | **選取的文字** (背景) | 醒目顯示                | \#FF3399FF      |
| SystemColorHighlightTextColor | **選取的文字** (前景) | HighlightAlt             | \#FFFFFFFF      |
| SystemColorHotlightColor      | **超連結**                 | Hyperlink                | \#FF0066CC      |
| SystemColorWindowColor        | **背景**                 | PageBackground           | \#FFFFFFFF      |
| SystemColorWindowTextColor    | **文字**                       | PageText                 | \#FF000000      |

Windows 提供不同的高對比佈景主題，可讓使用者透過 [輕鬆存取中心] 設定其高對比設定特有的色彩，如此處所示。 因此，無法提供明確的高對比色彩值清單。

![Windows 高對比設定 UI](images/high-contrast-settings.png)

如需支援高對比佈景主題的詳細資訊，請參閱[高對比佈景主題](https://msdn.microsoft.com/library/windows/apps/mt244346)。

### <a name="system-accent-color"></a>系統輔色

除了系統高對比佈景主題色彩以外，還使用索引鍵 `SystemAccentColor` 來提供系統輔色做為特殊的色彩資源。 在執行階段，這個資源會取得使用者已在 Windows 個人化設定中指定為輔色的色彩。

> [!NOTE]
> 雖然可以覆寫色彩系統資源，但是最佳做法還是尊重使用者的色彩選擇，特別是針對高對比設定。

### <a name="theme-dependent-brushes"></a>佈景主題相依筆刷

前述各節中顯示的色彩資源可用來設定系統佈景主題資源字典中 [SolidColorBrush](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 資源的 [Color](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 屬性。 您可以使用筆刷資源，將色彩套用到 XAML 元素。 適用於筆刷資源的索引鍵會遵循下列命名格式：`SystemControl[Simple HighContrast name][Simple light/dark name]Brush`。 例如，`SystemControlBackroundAltHighBrush`。

讓我們看看如何在執行階段決定此筆刷的色彩值。 在 "Light" 和 "Dark" 資源字典中，此筆刷的定義如下：

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{StaticResource SystemAltHighColor}"/>`

在 "HighContrast" 資源字典中，此筆刷的定義如下：

`<SolidColorBrush x:Key="SystemControlBackgroundAltHighBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>`

將這個筆刷套用到 XAML 元素時，即會在執行階段依據目前的佈景主題決定它的色彩，如下表所示。

| 佈景主題        | 色彩的簡單名稱 | 色彩資源             | 執行階段值                                              |
|--------------|-------------------|----------------------------|------------------------------------------------------------|
| 亮色調        | AltHigh           | SystemAltHighColor         | \#FFFFFFFF                                                 |
| 暗色調         | AltHigh           | SystemAltHighColor         | \#FF000000                                                 |
| HighContrast | 背景        | SystemColorButtonFaceColor | 設定中針對按鈕背景指定的色彩。 |

您可以使用 `SystemControl[Simple HighContrast name][Simple light/dark name]Brush` 命名配置來決定要將哪一個筆刷套用到您自己的 XAML 元素。

<!--
For many examples of how the brushes are used in the XAML control templates, see the [Default control styles and templates](default-control-styles-and-templates.md).
-->

> [!NOTE]
> 並非每個 \[*簡單 HighContrast 名稱*\]\[*簡單淺色/深色名稱*\] 組合都是提供來做為筆刷資源。

## <a name="the-xaml-type-ramp"></a>XAML 字體坡形

themeresources.xaml 檔案會定義數個資源，其定義您可以套用到 UI 中文字容器的 [Style](https://msdn.microsoft.com/library/windows/apps/br208849)，特別是針對 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 或 [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)。 這些不是預設的隱含樣式。 提供這些資源是為了讓您更容易建立符合[字型的指導方針](../style/typography.md)中所記載之 *Windows 字體坡形*的 XAML UI 定義。

這些樣式是用於您想要套用到整個文字容器的文字屬性。 如果只想將樣式套用到文字的區段，請針對容器內的文字元素設定屬性，例如，[TextBlock.Inlines](https://msdn.microsoft.com/library/windows/apps/br209959) 中的 [Run](https://msdn.microsoft.com/library/windows/apps/br209668) 或 [RichTextBlock.Blocks](https://msdn.microsoft.com/library/windows/apps/br244503) 中的 [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244347)。

當套用至 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 時樣式看起來像這樣：

![文字區塊樣式](../style/images/type/text-block-type-ramp.svg)

```XAML
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>
```

如需有關如何在您的應用程式中使用 UWP 字體坡形的指引，請參閱 [UWP app 中的印刷樣式](../style/typography.md)。

### <a name="basetextblockstyle"></a>BaseTextBlockStyle

**TargetType**: [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)

為所有其他 [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) 容器樣式提供通用屬性。

```XAML
<!-- Usage -->
<TextBlock Text="Base" Style="{StaticResource BaseTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
</Style>
```

### <a name="headertextblockstyle"></a>HeaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Header" Style="{StaticResource HeaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="HeaderTextBlockStyle" TargetType="TextBlock"
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="46"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subheadertextblockstyle"></a>SubheaderTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubHeader" Style="{StaticResource SubheaderTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubheaderTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="34"/>
    <Setter Property="FontWeight" Value="Light"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="titletextblockstyle"></a>TitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Title" Style="{StaticResource TitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="TitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="SemiLight"/>
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="subtitletextblockstyle"></a>SubtitleTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="SubTitle" Style="{StaticResource SubtitleTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="SubtitleTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="20"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodytextblockstyle"></a>BodyTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Body" Style="{StaticResource BodyTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="BodyTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
    <Setter Property="FontSize" Value="15"/>
</Style>
```

### <a name="captiontextblockstyle"></a>CaptionTextBlockStyle

```XAML
<!-- Usage -->
<TextBlock Text="Caption" Style="{StaticResource CaptionTextBlockStyle}"/>

<!-- Style definition -->
<Style x:Key="CaptionTextBlockStyle" TargetType="TextBlock" 
       BasedOn="{StaticResource BaseTextBlockStyle}">
    <Setter Property="FontSize" Value="12"/>
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

### <a name="baserichtextblockstyle"></a>BaseRichTextBlockStyle

**TargetType**: [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)

為所有其他 [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565) 容器樣式提供通用屬性。

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BaseRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BaseRichTextBlockStyle" TargetType="RichTextBlock">
    <Setter Property="FontFamily" Value="Segoe UI"/>
    <Setter Property="FontWeight" Value="SemiBold"/>
    <Setter Property="FontSize" Value="15"/>
    <Setter Property="TextTrimming" Value="None"/>
    <Setter Property="TextWrapping" Value="Wrap"/>
    <Setter Property="LineStackingStrategy" Value="MaxHeight"/>
    <Setter Property="TextLineBounds" Value="Full"/>
    <Setter Property="OpticalMarginAlignment" Value="TrimSideBearings"/>
</Style>
```

### <a name="bodyrichtextblockstyle"></a>BodyRichTextBlockStyle

```XAML
<!-- Usage -->
<RichTextBlock Style="{StaticResource BodyRichTextBlockStyle}">
    <Paragraph>Rich text.</Paragraph>
</RichTextBlock>

<!-- Style definition -->
<Style x:Key="BodyRichTextBlockStyle" TargetType="RichTextBlock" BasedOn="{StaticResource BaseRichTextBlockStyle}">
    <Setter Property="FontWeight" Value="Normal"/>
</Style>
```

**注意**： [RichTextBlock](https://msdn.microsoft.com/library/windows/apps/br227565)樣式沒有[TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652)執行程序，所有文字坡形樣式，主要是因為**RichTextBlock**的區塊型文件物件模型可以更容易針對個別的文字設定屬性項目。 此外，使用 XAML 內容屬性來設定 [TextBlock.Text](https://msdn.microsoft.com/library/windows/apps/br209676) 會導致一種情況，即沒有文字元素可供設定樣式，因此您必須設定容器的樣式。 這對 **RichTextBlock** 來說並不是問題，因為它的文字內容一律必須位於特定的文字元素 (例如 [Paragraph](https://msdn.microsoft.com/library/windows/apps/br244503)) 中，這是您可能為頁首、子頁首及類似文字坡形定義套用 XAML 樣式的地方。

## <a name="miscellaneous-named-styles"></a>其他具名樣式

其他還有一組已設定索引鍵的 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 定義，您可套用來為 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 設定與其預設隱含樣式不同的樣式。

### <a name="textblockbuttonstyle"></a>TextBlockButtonStyle

**TargetType**: [ButtonBase](https://msdn.microsoft.com/library/windows/apps/br227736)

當您需要顯示使用者可按一下以採取動作的文字時，請將此樣式套用到 [Button](https://msdn.microsoft.com/library/windows/apps/br209265)。 文字的樣式是使用目前的輔色所設定，可區別出它是可互動，且具備適用於文字的焦點矩形。 與 [HyperlinkButton](https://msdn.microsoft.com/library/windows/apps/br242739) 的隱含樣式不同，**TextBlockButtonStyle** 不會為文字加上底線。

該範本也設定所顯示文字的樣式來使用 **SystemControlHyperlinkBaseMediumBrush** (適用於 "PointerOver" 狀態)、**SystemControlHighlightBaseMediumLowBrush** (適用於 "Pressed" 狀態) 以及 **SystemControlDisabledBaseLowBrush** (適用於 "Disabled" 狀態)。

以下是套用到它的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 與 **TextBlockButtonStyle** 資源。

```XAML
<Button Content="Clickable text" Style="{StaticResource TextBlockButtonStyle}"
        Click="Button_Click"/>
```

它的外觀如下：

![已設定按鈕樣式，使其看起來就像文字](images/styles-textblock-button-style.png)

### <a name="navigationbackbuttonnormalstyle"></a>NavigationBackButtonNormalStyle

**TargetType**：[Button](https://msdn.microsoft.com/library/windows/apps/br209265)

這個 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 提供可做為瀏覽 app 之向後瀏覽按鈕的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 完整範本。 預設尺寸是 40 x 40 像素。 若要量身打造樣式，您可以在 **Button** 上明確設定 [Height](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)、[Width](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)、[FontSize](https://msdn.microsoft.com/library/windows/apps/br209406) 及其他屬性，或者使用 [BasedOn](https://msdn.microsoft.com/library/windows/apps/br208852) 建立衍生的樣式。

以下是套用到它的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 與 **NavigationBackButtonNormalStyle** 資源。

```XAML
<Button Style="{StaticResource NavigationBackButtonNormalStyle}" />
```

它的外觀如下：

![將按鈕樣式設定為返回按鈕](images/styles-back-button-normal.png)

### <a name="navigationbackbuttonsmallstyle"></a>NavigationBackButtonSmallStyle

**TargetType**：[Button](https://msdn.microsoft.com/library/windows/apps/br209265)

這個 [Style](https://msdn.microsoft.com/library/windows/apps/br208849) 提供可做為瀏覽 app 之向後瀏覽按鈕的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 完整範本。 與 **NavigationBackButtonNormalStyle** 類似，但尺寸是 30 x 30 像素。

以下是套用到它的 [Button](https://msdn.microsoft.com/library/windows/apps/br209265) 與 **NavigationBackButtonSmallStyle** 資源。

```XAML
<Button Style="{StaticResource NavigationBackButtonSmallStyle}" />
```

## <a name="troubleshooting-theme-resources"></a>疑難排解佈景主題資源

如果您未遵循[使用佈景主題資源的指導方針](#guidelines-for-using-theme-resources)，可能會看到與您的應用程式中佈景主題相關的非預期行為。

例如，當您開啟亮色調佈景主題的飛出視窗時，暗色調佈景主題應用程式的部分也會變更，就像它們是處於亮色調佈景主題中。 或者，如果您瀏覽到亮色調佈景主題頁面，然後瀏覽回來，則原始的暗色調佈景主題頁面 (或它的某些部分) 現在看起來就像是在亮色調佈景主題中。

通常這些類型的問題會在您提供 "Default" 佈景主題和 "HighContrast" 佈景主題以支援高對比案例，然後在應用程式的不同部分使用 "Light" 和 "Dark" 佈景主題時發生。

以這個佈景主題字典定義為例：

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

直覺看來這是正確的。 在處於高對比時，您想要變更 `myBrush` 所指向的色彩，但在非高對比時，您可以依賴 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)來確定 `myBrush` 會指向適用於佈景主題的正確色彩。 如果您的 app 絕對不會在其視覺化樹狀結構內的元素上設定 [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/dn298515)，這通常會以預期的方式運作。 不過，一旦您開始重新為視覺化樹狀結構的不同部分設定佈景主題，就會立即在應用程式中遇到問題。

與大部分其他的 XAML 類型不同，由於筆刷是共用的資源，因此會發生此問題。 如果您在含有不同佈景主題的 XAML 樹狀子目錄中擁有 2 個參考同一個筆刷資源的元素，則當架構進行到每個樹狀子目錄來更新其 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)運算式時，對於共用筆刷資源的變更即會反映在其他樹狀子目錄中，而這並非您想要的結果。

若要修正此問題，除了 "HighContrast" 之外，還可以針對 "Light" 和 "Dark" 佈景主題，使用不同的佈景主題字典來取代 "Default" 字典：

```XAML
<!-- DO NOT USE. THIS XAML DEMONSTRATES AN ERROR. -->
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

不過，如果繼承的屬性 (例如 [Foreground](https://msdn.microsoft.com/library/windows/apps/br209414)) 中參考到這些資源的任一個，仍會發生問題。 您的自訂控制項範本可能會使用 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)來指定元素的前景色彩，但是當架構將繼承的值傳播到子元素時，它會提供資源的直接參考 (這會透過 {ThemeResource} 標記延伸運算式來解析)。 當架構在查看您控制項的視覺化樹狀結構，處理到佈景主題變更時，這就會引發問題。 它會重新評估 {ThemeResource} 標記延伸運算式來取得新的筆刷資源，但尚未將這個參考向下傳播到控制項的子項；這會在稍後發生，例如，在下一次測量階段期間。

因此，在查看控制項視覺化樹狀結構以回應佈景主題變更之後，架構會查看子項，並更新其上設定的任何 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)運算式，或者位於其屬性上所設定的物件。 這就是發生問題的所在。架構會查看筆刷資源，而且由於它會使用 {ThemeResource} 標記延伸來指定色彩，因此會重新進行評估。

到目前為止，架構會顯示為已干擾了您的佈景主題字典，因為它現在擁有來自某一個字典的資源，而其色彩是從其他字典設定的。

若要解決此問題，請不要使用 [{ThemeResource} 標記延伸](../../xaml-platform/staticresource-markup-extension.md)，改用 [{StaticResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)。 套用指導方針之後，佈景主題字典看起來像這樣：

```XAML
<ResourceDictionary>
  <ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Light">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="Dark">
      <SolidColorBrush x:Key="myBrush" Color="{StaticResource SystemBaseHighColor}"/>
    </ResourceDictionary>
    <ResourceDictionary x:Key="HighContrast">
      <SolidColorBrush x:Key="myBrush" Color="{ThemeResource SystemColorButtonFaceColor}"/>
    </ResourceDictionary>
  </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

請注意，仍會在 "HighContrast" 字典中使用 [{ThemeResource} 標記延伸](../../xaml-platform/themeresource-markup-extension.md)，而不是使用 [{StaticResource} 標記延伸](../../xaml-platform/staticresource-markup-extension.md)。 這種情況屬於指導方針中稍早指定的例外狀況。 大多數用於 "HighContrast" 佈景主題的筆刷值都是使用由系統全域控制但對 XAML 顯示為特別命名之資源的色彩選擇 (這些項目的名稱都是以 ‘SystemColor’ 做為首碼)。 系統可讓使用者透過 [輕鬆存取中心]，來設定其高對比設定應使用的特定色彩。 這些色彩選擇會套用到特別命名的資源。 XAML 架構會使用相同的佈景主題變更事件，當其在系統層級上偵測到這些筆刷已變更時，也會更新它們。 這就是為什麼要在此處使用 {ThemeResource} 標記延伸的原因。