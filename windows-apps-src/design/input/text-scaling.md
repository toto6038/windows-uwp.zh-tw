---
Description: 建置 UWP 應用程式和支援的平台文字調整的自訂/樣板化控制項。
title: 文字大小調整
label: Text scaling
template: detail.hbs
keywords: UWP、 文字、 調整、 協助工具、 「 輕鬆存取 」，顯示 「 讓文字更大 」，使用者互動，輸入
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 22ad7a1ac6160fd8b1cfb70c69f299c5d89192d3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600813"
---
# <a name="text-scaling"></a>文字大小調整

![調整 225%到 100%的文字範例](images/coretext/text-scaling-news-hero-small.png)  
*調整 Windows 10 （100%到 225 的 %） 中的文字範例*

## <a name="overview"></a>概觀

許多人時，頗具挑戰性讀取 （從行動裝置，到 Surface Hub 的巨大螢幕的桌上型監視器的膝上型電腦） 的電腦螢幕上的文字。 相反地，某些使用者發現大於必要應用程式和網站中使用的字型大小。

若要確保文字是以易於閱讀盡最大範圍的使用者，Windows 會提供變更相對字型大小跨 OS 和個別的應用程式的使用者的能力。 而不是使用 [放大鏡] 應用程式 （這通常只是可以放大螢幕上的區域內的所有項目，並引進自己的可用性問題）、 變更顯示器解析度，或依賴 DPI 縮放比例 （這會顯示和一般檢視為基礎的所有項目調整大小距離），使用者可以快速存取設定，以可調整大小只是文字，範圍從 100%（預設大小） 225%。

## <a name="support"></a>支援

通用 Windows 應用程式 (這兩個標準，並將 PWA)，支援縮放比例預設的文字。

如果您的 UWP 應用程式包含自訂控制項、 自訂文字介面、 硬式編碼的控制項的高度，較舊的架構或第 3 個合作對象的架構，您可能必須進行一些更新，以確保一致且有用的體驗，為您的使用者。  

DirectWrite、 GDI 和 XAML SwapChainPanels 原本不支援文字大小調整，而 Win32 支援僅限於功能表、 圖示和工具列。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>使用者體驗

使用者可以調整文字大小以讓文字更大的滑桿，在 設定-> 輕鬆存取-> 願景/顯示畫面。

![調整 225%到 100%的文字範例](images/coretext/text-scaling-settings-100-small.png)  
*文字調整規模設定從 設定-> 輕鬆存取-> 願景/顯示畫面*

## <a name="ux-guidance"></a>UX 指導方針

文字是調整大小時，控制項和容器也必須調整大小並且自動重排以容納文字和其新的版面配置。 如前文所述，根據應用程式、 架構和平台，是為您完成大部分的這項工作。 下列的 UX 指導方針涵蓋這些情況下，很不。

### <a name="use-the-platform-controls"></a>使用平台控制項

我們說這已經嗎？ 值得連載：可能的話，請一律使用提供的各種不同的 Windows 應用程式架構的內建控制項，來取得最完整的使用者體驗可能最少的投入時間。

例如，所有 UWP 文字控制項都支援全文檢索規模調整而不需要任何自訂或樣板化的體驗。

以下是從基本的 UWP 應用程式，其中包含幾個標準文字控制項的程式碼片段：

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![動態調整 225%到 100%的文字](images/coretext/text-scaling.gif)  
*調整動畫效果的文字*

### <a name="use-auto-sizing"></a>使用自動調整大小

未指定控制項的絕對的大小。 可能的話，可讓您根據使用者和裝置設定自動的控制項重新調整大小的平台。  

在此程式碼片段前一個範例中，我們會使用`Auto`和`*`一組方格資料行和可讓平台的寬度值調整應用程式版面配置方格中包含的項目大小為基礎。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文字換行

若要確保您的應用程式的版面配置是富彈性並具可調適性越好，可讓任何包含的文字 （許多控制項並不支援文字換行的預設值） 的控制項中的文字換行。

如果您未指定文字換行，此平台會使用其他方法，在調整版面配置，包括剪取 （請參閱上一個範例）。

在這裡，我們使用`AcceptsReturn`和`TextWrapping`文字方塊屬性，以確保我們的版面配置是盡可能有彈性。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![動畫效果的文字與文字換行調整 225%到 100%](images/coretext/text-scaling-textwrap.gif)  
*動態調整文字換行的文字*

### <a name="specify-text-trimming-behavior"></a>指定文字修剪行為

如果文字換行不是慣用的行為，大部分的文字控制項可讓您裁剪文字，或指定的文字修剪行為的省略符號。 裁剪是慣用的省略符號，如省略符號會佔用空間本身。

> [!NOTE]
> 若要裁剪文字，裁剪字串的結尾，無法開始。

在此範例中，我們會示範如何裁剪在 TextBlock 中使用的文字[TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)屬性。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![縮放比例 100%以 225%取代成文字裁剪的文字](images/coretext/text-scaling-clipping-small.png)  
*使用文字裁剪相應的文字*

### <a name="use-a-tooltip"></a>使用工具提示

如果您裁剪文字，可用於工具提示提供給使用者的完整文字。

在這裡，我們將工具提示新增至 TextBlock，不支援文字換行：

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>無法擴充字型架構圖示或符號

當使用字型架構圖示強調或裝飾，停用縮放比例，這些字元。

設定[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)屬性設`false`大部分 XAML 的控制項。

### <a name="support-text-scaling-natively"></a>調整原生支援文字

處理[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系統事件，在您自訂的架構和控制項。 每次使用者在他們的系統設定的文字的縮放比例時，會引發這個事件。

## <a name="summary"></a>摘要

本主題提供縮放支援在 Windows 中的文字的概觀，並包含有關如何自訂使用者體驗的 UX 和開發人員指引。

## <a name="related-articles"></a>相關文章

### <a name="api-reference"></a>API 參考資料

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
