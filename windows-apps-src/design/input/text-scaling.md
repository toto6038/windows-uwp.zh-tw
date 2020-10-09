---
description: 建立 Windows 應用程式和支援平臺文字調整的自訂/範本控制項。
title: 文字大小調整
label: Text scaling
template: detail.hbs
keywords: UWP、文字、縮放、協助工具、「輕鬆存取」、顯示「讓文字變大」、使用者互動、輸入
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0d47523ca69f8088d5e13ab944c5dd2be2d1d8ba
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860165"
---
# <a name="text-scaling"></a>文字大小調整

![顯示從100% 到225% 的文字縮放範例的主圖影像。](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10 (100% 到225% 的文字縮放範例 ) *

## <a name="overview"></a>概觀

將電腦螢幕上的文字 (從行動裝置至膝上型電腦監視器，到 Surface Hub) 的大型畫面，對許多人來說可能是一大挑戰。 相反地，某些使用者會發現應用程式和網站中所使用的字型大小大於所需。

為了確保文字盡可能能清楚地提供最廣泛的使用者，Windows 提供讓使用者在作業系統和個別應用程式之間變更相對字型大小的能力。 相較於使用 [放大鏡] 應用程式 (通常只是放大螢幕區域內的所有內容，並引進自己的可用性問題)、變更顯示器解析度，或依賴 DPI 縮放比例 (根據顯示器和一般觀看距離調整所有內容)，使用者可以快速存取設定以僅調整文字大小，範圍從 100% (預設大小) 到 225%。

## <a name="support"></a>支援

通用 Windows 應用程式 (標準和 PWA) ，預設支援文字縮放。

如果您的 Windows 應用程式包含自訂控制項、自訂文字介面、硬式編碼的控制項高度、較舊的架構或協力廠商架構，您可能必須進行一些更新，以確保使用者能擁有一致且實用的體驗。  

DirectWrite、GDI 和 XAML SwapChainPanels 原本就不支援文字調整，而 Win32 支援僅限於功能表、圖示和工具列。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>使用者體驗

使用者可以在 [設定->] 輕鬆存取-> 視覺/顯示畫面上，使用 [讓文字放大] 滑杆來調整文字縮放比例。

![輕鬆存取視覺/顯示設定] 頁面的螢幕擷取畫面，其中顯示 [讓文字變大] 滑杆。](images/coretext/text-scaling-settings-100-small.png)  
*設定中的文字縮放設定-> 輕鬆存取-> 視覺/顯示畫面*

## <a name="ux-guidance"></a>UX 指導方針

當文字調整大小時，控制項和容器也必須調整大小並重新排列，以容納文字及其新的版面配置。 如先前所述，根據應用程式、架構和平臺，大部分的工作都是為您完成。 下列 UX 指引涵蓋不是的情況。

### <a name="use-the-platform-controls"></a>使用平臺控制項

我們已經說過了嗎？ 值得一提的是，如果可能的話，請一律使用各種 Windows 應用程式架構所提供的內建控制項，以取得最全面的使用者體驗。

例如，所有 UWP 文字控制項都支援完整文字調整體驗，而不需要任何自訂或範本。

以下是基本 UWP 應用程式中包含幾個標準文字控制項的程式碼片段：

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

![文字調整100% 到225% 的動畫。](images/coretext/text-scaling.gif)  
*動畫文字縮放比例*

### <a name="use-auto-sizing"></a>使用自動調整大小

請勿指定控制項的絕對大小。 盡可能讓平臺根據使用者和裝置設定，自動調整控制項的大小。  

在上述範例的這個程式碼片段中，我們會 `Auto` `*` 針對一組方格資料行使用和寬度值，並讓平臺根據方格中包含的元素大小來調整應用程式版面配置。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文字換行

為了確保您的應用程式佈建盡可能具彈性且可調整，請在包含文字的任何控制項中啟用文字換行 (許多控制項都不支援預設) 的文字換行。

如果您未指定文字換行，平臺會使用其他方法來調整版面配置，包括裁剪 (請參閱先前的範例) 。

在這裡，我們 `AcceptsReturn` 會使用和 `TextWrapping` TextBox 屬性，以確保我們的版面配置盡可能具有彈性。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![文字換行調整100% 到225% 的動畫。](images/coretext/text-scaling-textwrap.gif)  
*文字換行的動畫文字縮放比例*

### <a name="specify-text-trimming-behavior"></a>指定文字修剪行為

如果文字換行不是慣用的行為，大部分的文字控制項都可讓您裁剪文字或指定文字修剪行為的省略號。 在橢圓形中偏好裁剪，因為省略號會自行佔用空間。

> [!NOTE]
> 如果您需要裁剪您的文字，請裁剪字串的結尾，而不是開頭。

在此範例中，我們會示範如何使用 [system.windows.controls.textblock.texttrimming](/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) 屬性來裁剪 TextBlock 中的文字。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![使用文字裁剪調整100% 到225% 的文字螢幕擷取畫面。](images/coretext/text-scaling-clipping-small.png)  
*文字裁剪的文字縮放*

### <a name="use-a-tooltip"></a>使用工具提示

如果您裁剪文字，請使用工具提示，為使用者提供完整文字。

在這裡，我們會將工具提示新增至不支援文字換行的 TextBlock：

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>不要調整以字型為基礎的圖示或符號

使用以字型為基礎的圖示進行強調或裝飾時，請停用這些字元的縮放比例。

[IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) `false` 針對大部分的 XAML 控制項，將 IsTextScaleFactorEnabled 屬性設定為。

### <a name="support-text-scaling-natively"></a>以原生方式支援文字調整

在您的自訂架構和控制項中處理 [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系統事件。 每次使用者在其系統上設定文字縮放比例時，就會引發此事件。

## <a name="summary"></a>摘要

本主題概要說明 Windows 中的文字調整支援，並包含如何自訂使用者體驗的 UX 和開發人員指引。

## <a name="related-articles"></a>相關文章

### <a name="api-reference"></a>API 參考資料

- [IsTextScaleFactorEnabled](/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
