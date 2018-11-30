---
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: 文字大小調整
label: Text scaling
template: detail.hbs
keywords: UWP，文字、 縮放比例、 協助工具、 」 輕鬆存取 」，顯示 「 大使文字 」，使用者互動，輸入
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f81c435690c7bf17066be5f49de4994f146fc5c9
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8201249"
---
# <a name="text-scaling"></a>文字大小調整

![文字縮放比例 100%到 225%的範例](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10 （100%到 225 的 %） 中的縮放比例的文字的範例*

## <a name="overview"></a>概觀

對許多人可以挑戰性讀取文字 （從桌面監視 Surface Hub 的巨大螢幕的膝上型電腦的行動裝置） 的電腦螢幕上。 相反地，某些使用者會發現應用程式和網站中用於較大不必要的字型大小。

若要確保文字是一樣清晰，盡最大範圍的使用者，Windows 會提供使用者變更相對字型大小跨作業系統和個別應用程式的能力。 而不是使用放大鏡 app （這通常只會放大螢幕的區域內的所有項目，並引進了自己的可用性問題）、 變更顯示器解析度，或依賴 DPI 縮放比例 （這會調整大小，根據顯示和一般檢視的所有項目距離），使用者可以快速存取調整大小只是文字，涵蓋範圍可從 100%（預設大小） 的設定 225%。

## <a name="support"></a>支援

通用 Windows 應用程式 (這兩個標準和 PWA)，支援預設縮放比例的文字。

如果您的 UWP 應用程式包含自訂控制項、 自訂文字表面、 硬式編碼的控制項的高度、 較舊的架構或第 3 個廠商架構，您可能需要一些更新以確保一致且實用的體驗，為您的使用者。  

DirectWrite、 GDI，以及 XAML SwapChainPanels 原本不支援文字縮放比例，雖然 Win32 支援僅限於功能表、 圖示和工具列。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>使用者體驗

使用者可以調整文字縮放比例以使文字較大的滑桿，在 \ [設定]-> [輕鬆存取]-> [願景/顯示畫面。

![文字縮放比例 100%到 225%的範例](images/coretext/text-scaling-settings-100-small.png)  
*文字縮放比例設定從 [設定]-> [輕鬆存取]-> [願景/顯示畫面*

## <a name="ux-guidance"></a>UX 指導方針

隨著文字調整大小時，控制項和容器必須也調整大小和自動重排，以容納文字和其新的版面配置。 如前所述，取決於應用程式、 架構及平台，大部分的這項工作是為您完成。 下列的 UX 指導方針涵蓋它是不這些情況。

### <a name="use-the-platform-controls"></a>使用平台控制項

我們說這已經？ 值得重複： 若有可能，一律使用各種不同的 Windows 應用程式架構提供內建控制項取得最完整的使用者經驗可能最少的心力。

例如，所有 UWP 文字控制項都支援完整的文字縮放比例不需要任何的自訂項目或範本化的體驗。

以下是程式碼片段會從包含幾個標準的文字控制項的基本 UWP 應用程式：

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

![動畫的 225%到 100%縮放比例的文字](images/coretext/text-scaling.gif)  
*動畫的文字縮放比例*

### <a name="use-auto-sizing"></a>使用自動調整大小

不要指定為您的控制項的絕對大小。 可能的話，可讓您的控制項能夠根據使用者和裝置設定，自動調整大小的平台。  

在這個片段從上一個範例中，我們會使用`Auto`和`*`寬度值的一組方格資料行與可讓平台調整應用程式配置根據在方格內包含的項目大小。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文字換行

若要確保您的應用程式的版面配置是以彈性和適應，以盡可能，啟用文字換行中包含的文字 （多個控制項並不支援文字換行預設） 的任何控制項。

如果您沒有指定文字換行，平台要使用其他方法來調整版面配置，包括裁剪 （請參閱上一個範例）。

在這裡，我們會使用`AcceptsReturn`和`TextWrapping`TextBox 屬性，以確保我們的配置是儘可能是彈性。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![動畫效果的 225%到 100%縮放文字換行的文字](images/coretext/text-scaling-textwrap.gif)  
*動畫縮放文字換行的文字*

### <a name="specify-text-trimming-behavior"></a>指定文字修剪行為

如果文字換行並不是慣用的行為，請裁剪文字，或指定的文字修剪行為的刪節號，可讓大部分文字控制項。 裁剪是慣用省略符號以省略符號佔用空間本身。

> [!NOTE]
> 如果您需要裁剪文字，裁剪的字串不開頭的結尾。

在此範例中，我們示範如何在 TextBlock 中裁剪文字使用[TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)屬性。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![縮放比例 225%到 100%使用文字裁剪的文字](images/coretext/text-scaling-clipping-small.png)  
*縮放比例使用文字裁剪的文字*

### <a name="use-a-tooltip"></a>使用工具提示

如果您裁剪文字，使用工具提示給您的使用者提供完整的文字。

在這裡，我們將工具提示新增到 TextBlock 不支援文字換行：

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>不調整字型為基礎的圖示或符號

強調或裝飾使用字型為基礎的圖示時, 停用縮放比例，這些字元。

將[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)屬性設定為`false`適用於大部分的 XAML 控制項。

### <a name="support-text-scaling-natively"></a>縮放比例原生支援文字

處理[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系統事件，在您的自訂架構和控制項中。 每個使用者在他們的系統設定文字縮放比例係數的時間時，會引發此事件。

## <a name="summary"></a>摘要

本主題概述的縮放比例支援在 Windows 中的文字，並包含有關如何自訂使用者體驗的 UX 與開發人員指導方針。

## <a name="related-articles"></a>相關文章

### <a name="api-reference"></a>API 參考資料

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
