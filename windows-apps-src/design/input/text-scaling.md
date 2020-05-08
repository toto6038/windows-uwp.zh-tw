---
Description: 建立 Windows 應用程式，以及支援平臺文字縮放的自訂/範本控制項。
title: 文字大小調整
label: Text scaling
template: detail.hbs
keywords: UWP，文字，縮放，協助工具，「輕鬆存取」，顯示，「讓文字變大」，使用者互動，輸入
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4db3af0d2ec0ce1dbd0866f569ad9bf9b0392aa8
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970563"
---
# <a name="text-scaling"></a>文字大小調整

![文字縮放比例100% 到225% 的範例](images/coretext/text-scaling-news-hero-small.png)  
*Windows 10 中的文字縮放範例（100% 到225%）*

## <a name="overview"></a>概觀

將電腦螢幕上的文字（從 [行動裝置] 到 [膝上型電腦] 到 [桌面監視器]）讀取到 Surface Hub 的 [巨型] 畫面，對於許多人來說可能是一項挑戰。 相反地，有些使用者會發現應用程式和網站中所使用的字型大小大於所需的大小。

為了確保最大範圍的使用者能夠盡可能地閱讀文字，Windows 提供使用者在 OS 和個別應用程式之間變更相對字型大小的能力。 相較於使用 [放大鏡] 應用程式 (通常只是放大螢幕區域內的所有內容，並引進自己的可用性問題)、變更顯示器解析度，或依賴 DPI 縮放比例 (根據顯示器和一般觀看距離調整所有內容)，使用者可以快速存取設定以僅調整文字大小，範圍從 100% (預設大小) 到 225%。

## <a name="support"></a>支援

通用 Windows 應用程式（標準和 PWA）預設支援文字縮放。

如果您的 Windows 應用程式包含自訂控制項、自訂文字介面、硬式編碼控制項高度、舊版架構或協力廠商架構，您可能必須進行一些更新，以確保使用者的一致且有用的體驗。  

DirectWrite、GDI 和 XAML SwapChainPanels 原本就不支援文字縮放，而 Win32 支援僅限於功能表、圖示和工具列。  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>使用者經驗

使用者可以使用 [設定] 上的 [讓文字變大] 滑杆來調整文字大小-> 輕鬆存取-> 視覺/顯示畫面。

![文字縮放比例100% 到225% 的範例](images/coretext/text-scaling-settings-100-small.png)  
*設定的文字縮放設定-> 輕鬆存取-> 視覺/顯示畫面*

## <a name="ux-guidance"></a>UX 指導方針

調整文字大小時，控制項和容器也必須調整大小和重新排列，以容納文字及其新版面配置。 如先前所述，根據應用程式、架構和平臺，會為您完成大部分的工作。 下列 UX 指導方針涵蓋不適用的案例。

### <a name="use-the-platform-controls"></a>使用平臺控制項

我們已經說過了嗎？ 值得一提的是，在可行的情況下，一定要使用各種 Windows 應用程式架構所提供的內建控制項，以取得最全面的使用者體驗。

例如，所有 UWP 文字控制項都支援完整的文字縮放體驗，而不需要任何自訂或範本。

以下是基本 UWP 應用程式的程式碼片段，其中包含幾個標準文字控制項：

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

![動畫文字縮放比例100% 到225%](images/coretext/text-scaling.gif)  
*動畫文字縮放比例*

### <a name="use-auto-sizing"></a>使用自動調整大小

請勿指定控制項的絕對大小。 盡可能讓平臺根據使用者和裝置設定，自動調整控制項大小。  

在前一個範例的這個程式碼片段中，我們`Auto`會`*`針對一組格線資料行使用和寬度值，並讓平臺根據方格中包含的元素大小來調整應用程式的版面配置。

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>使用文字換行

若要確保應用程式的版面配置盡可能彈性且可調整，請在包含文字的任何控制項中啟用文字換行（許多控制項預設不支援文字換行）。

如果您未指定文字換行，平臺會使用其他方法來調整版面配置，包括裁剪（請參閱上一個範例）。

在此，我們會`AcceptsReturn`使用`TextWrapping`和 TextBox 屬性，以確保我們的版面配置盡可能彈性。

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![文字換行的動畫文字縮放比例100% 到225%](images/coretext/text-scaling-textwrap.gif)  
*文字換行的動畫文字縮放*

### <a name="specify-text-trimming-behavior"></a>指定文字修剪行為

如果文字換行不是慣用的行為，大部分的文字控制項都可讓您裁剪文字，或為文字修剪行為指定省略號。 對橢圓形而言偏好裁剪，因為省略號會佔用空間本身。

> [!NOTE]
> 如果您需要裁剪文字，請裁剪字串結尾，而不是開始。

在此範例中，我們會示範如何使用[system.windows.controls.textblock.texttrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming)屬性來裁剪 TextBlock 中的文字。

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![文字裁剪比例為100% 到225%](images/coretext/text-scaling-clipping-small.png)  
*使用文字裁剪縮放文字*

### <a name="use-a-tooltip"></a>使用工具提示

如果您裁剪文字，請使用工具提示來提供完整的文字給您的使用者。

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

當您使用以字型為基礎的圖示進行強調或裝飾時，請停用這些字元的縮放。

針對大部分[IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)的 XAML 控制項`false` ，將 IsTextScaleFactorEnabled 屬性設定為。

### <a name="support-text-scaling-natively"></a>以原生方式支援文字縮放

在您的自訂架構和控制項中處理[TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings 系統事件。 每次使用者在其系統上設定文字縮放比例時，就會引發此事件。

## <a name="summary"></a>總結

本主題概述 Windows 中的文字調整支援，並包含 UX 和開發人員指引，說明如何自訂使用者體驗。

## <a name="related-articles"></a>相關文章

### <a name="api-reference"></a>API 參考資料

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)
