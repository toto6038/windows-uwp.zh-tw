---
author: Karl-Bridge-Microsoft
Description: Learn how to improve both the usability and the accessibility of your UWP app by providing an intuitive way for users to quickly navigate and interact with an app's visible UI through a keyboard instead of a pointer device (such as touch or mouse).
title: 便捷鍵設計指導方針
label: Access keys design guidelines
keywords: 鍵盤, 便捷鍵, keytip, 按鍵提示, 協助工具, 瀏覽, 焦點, 文字, 輸入, 使用者互動
template: detail.hbs
ms.author: kbridge
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8e842d6c5b8e62a9c043c97849fdf17f524ccfc7
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "4360741"
---
# <a name="access-keys"></a>便捷鍵

便捷鍵是一種鍵盤快速鍵，透過提供直覺的方式，讓使用者透過鍵盤而不是指標裝置 (例如觸控式或滑鼠)，快速瀏覽應用程式的可見 UI 及互動，改善 Windows 應用程式的可用性和協助工具。

請參閱[快速鍵](keyboard-accelerators.md)主題，以了解有關使用鍵盤快速鍵在 Windows 應用程式中叫用常見動作的詳細資訊。 

> [!NOTE]
> 鍵盤對身障使用者而言是不可或缺的工具 (請參閱[鍵盤協助工具](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility))，對於希望透過它而能更有效率地與應用程式互動的使用者而言，也是重要工具。

通用 Windows 平台 (UWP) 針對以鍵盤為基礎的便捷鍵，以及透過視覺提示 (稱為按鍵提示) 的相關 UI 回饋，提供內建支援的跨平台控制項。

## <a name="overview"></a>概觀

便捷鍵是 Alt 鍵與一或多個英數字元鍵 (有時稱為「*助憶鍵*」) 的組合，通常是循序，而不是同時按下。

按鍵提示是當使用者按 Alt 鍵時控制項旁邊顯示的徽章。 每個按鍵提示包含啟動相關聯控制項的英數字元按鍵。

> [!NOTE]
> 對於只有單一英數字元的便捷鍵，會自動支援鍵盤快速鍵。 例如，同時在 Word 中按 Alt + F，會開啟 \[檔案\] 功能表，而不會顯示按鍵提示。

按下 Alt 鍵會初始化便捷鍵功能，並顯示按鍵提示中所有目前使用的按鍵組合。 後續按鍵動作是由便捷鍵架構所處理，會拒絕無效的按鍵，直到按下有效的便捷鍵，或按下 Enter 鍵、Esc 鍵、Tab 鍵或方向鍵來停用便捷鍵，並將按鍵處理傳回到 app。

Microsoft Office 應用程式為便捷鍵提供廣泛的支援。 下圖顯示 Word 中已啟動便捷鍵的 [常用] 索引標籤（請注意，針對數字和多個按鍵動作的支援）。

![在 Microsoft Word 中的便捷鍵按鍵提示徽章](images/accesskeys/keytip-badges-word.png)

_在 Microsoft Word 便捷鍵 KeyTip 徽章_

若要新增便捷鍵到控制項，請使用 **AccessKey 屬性**。 這個屬性指定便捷鍵順序、捷徑 (如果是一個英數字元) 和按鍵提示。

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>便捷鍵使用時機

我們建議您在 UI 任何適當的地方指定便捷鍵，並在所有自訂控制項中支援便捷鍵。

1.  **便捷鍵讓您的 app 更容易存取**，適用於運動功能障礙的使用者，包括一次只能按一個按鍵或使用滑鼠有困難的使用者。

    設計良好的鍵盤 UI 是軟體協助工具的一個重要層面。 它讓視障使用者或受到某種程度運動神經傷害的使用者能夠瀏覽應用程式並與應用程式的功能互動。 這類使用者有可能無法使用滑鼠，因而必須仰賴像鍵盤增強功能工具、螢幕小鍵盤、螢幕放大機、螢幕助讀程式以及語音輸入公用程式這類協助技術。 對於這些使用者，完整命令涵蓋範圍很重要。

2.  **便捷鍵讓您的 app 更有用**，適用於偏好使用鍵盤進行互動的進階使用者。

    經驗豐富的使用者通常極度偏好使用鍵盤，因為鍵盤式命令的輸入更快速，而且不需要從鍵盤移開其雙手。 對於這些使用者而言，效率與一致性非常重要；完整性只對最常用的命令很重要。

## <a name="set-access-key-scope"></a>設定便捷鍵範圍

當畫面上有多個支援便捷鍵的項目時，我們建議設定便捷鍵範圍以減輕**認知負擔**。 這會最小化在畫面上的便捷鍵數目，讓它們更輕鬆地找到，增加效率和生產力。

例如，Microsoft Word 提供兩個便捷鍵範圍：功能區索引標籤的主要範圍和所選索引標籤上命令的次要領域。

下列影像示範 Word 中的兩個便捷鍵範圍。 第一個影像顯示主要便捷鍵，讓使用者選取索引標籤和其他最上層命令，而第二個影像顯示 [首頁] 索引標籤的次要便捷鍵。

![Microsoft Word 的主要便捷鍵](images/accesskeys/primary-access-keys-word.png)
_Microsoft Word 的主要便捷鍵_

![Microsoft Word 的次要便捷鍵](images/accesskeys/secondary-access-keys-word.png)
_Microsoft Word 的次要便捷鍵_

對於不同範圍中的項目，便捷鍵可以複製。 在先前的範例，“2”是主要範圍中 [復原] 的便捷鍵，也是次要範圍中 [斜體] 的便捷鍵。

在這裡，我們示範如何定義便捷鍵範圍。

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![CommandBar 的主要便捷鍵](images/accesskeys/primary-access-keys-commandbar.png)

_CommandBar 主要範圍和支援的便捷鍵_

![CommandBar 的次要便捷鍵](images/accesskeys/secondary-access-keys-commandbar.png)

_CommandBar 次要範圍和支援的便捷鍵_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Creators Update 及較舊版本

在 Windows 10 Fall Creators Update 之前，有些控制項，例如 CommandBar 並不支援內建的便捷鍵範圍。

以下範例顯示如何在叫用父命令（類似 Word 的功能區）時，以可用的便捷鍵支援 CommandBar 的 SecondaryCommands。

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>避免便捷鍵衝突

當相同範圍中兩個或多個項目有重複的便捷鍵，或以相同的英數字元開始時，會發生便捷鍵衝突。

系統處理第一個新增到視覺化樹狀結構之項目的，並忽略所有其他便捷鍵，解決重複的便捷鍵問題。

當多個便捷鍵以相同的字元開始（，例如「A」、「A1」和「AB」）時，系統處理單一字元便捷鍵，並會忽略其他所有便捷鍵。

使用唯一便捷鍵或範圍命令，避免衝突。

## <a name="choose-access-keys"></a>選擇便捷鍵

選擇便捷鍵時，請考慮下列事項：

-   使用一個字元，將按鍵動作最小化，並預設支援快速鍵 (Alt+AccessKey)
-   避免使用兩個以上的字元
-   避免便捷鍵衝突
-   避免難以區分的字元，例如字母「I」和數字「1」或字母「O」和數字「0」
-   使用其他常用 app (例如 Word) 的已知先例 (「F」用於 [檔案]，「H」用於 [常用] 以此類推)
-   使用命令名稱的第一個字元，或是與命令有密切關聯、有助於回憶的字元
    -   如果已經指派第一個字母，使用命令名稱中盡可能接近第一個字元的字母 (「N」用於 [插入])
    -   使用命令名稱的獨特子音 (「W」用於 [檢視])
    -   使用命令名稱的母音。

## <a name="localize-access-keys"></a>當地語系化便捷鍵

如果您的 app 將要以多種語言當地語系化，您也應該**考慮當地語系化便捷鍵**。 例如，在 en-US，“H” 用於 “Home”，在 es-ES，“I” 用於 “Incio”。

在標記中使用 x:Uid 延伸，套用當地語系化的資源，如下所示：

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
每一種語言的資源加入至專案中相對應的 String 資料夾：

![英文和西班牙文資源 string 資料夾](images/accesskeys/resource-string-folders.png)

_英文和西班牙文資源 string 資料夾_

在專案的 resources.resw 檔案中，指定當地語系化的便捷鍵：

![在 resources.resw 檔案中指定 AccessKey 屬性](images/accesskeys/resource-resw-file.png)

_在 resources.resw 檔案中指定 AccessKey 屬性_

如需詳細資訊，請參閱[翻譯 UI 資源](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx)

## <a name="key-tip-positioning"></a>按鍵提示位置

按鍵提示會顯示為浮動徽章，相對於其相對應的 UI 項目，並考慮其他 UI 項目、其他按鍵提示和螢幕邊緣。

通常預設按鍵提示的位置已經足夠，並提供調適型 UI 的內建支援。

![自動按鍵提示位置的範例](images/accesskeys/auto-keytip-position.png)

_自動按鍵提示位置的範例_

但是，如果您需要對按鍵提示放置位置有更多的控制，我們建議下列方式：

1.  **明顯關聯原則**：使用者可以輕鬆地關聯控制項與按鍵提示。

    a.  Key Tip 應該**接近**擁有便捷鍵的項目（擁有者）。  
    b.  按鍵提示應該**避免遮蔽具有便捷鍵的已啟用元素**。   
    c.  如果按鍵提示無法接近擁有者放置，它應該與擁有者重疊。 

2.  **可搜尋性**：使用者可以快速探索具有按鍵提示的控制項。

    a.  按鍵提示絕不與其他按鍵提示**重疊**。  

3.  **輕鬆掃描：** 使用者可以輕鬆地瀏覽按鍵提示。

    a.  按鍵提示應該彼此**對齊**並與 UI 項目對齊。
    b.  按鍵提示應該盡可能**分組**。 

### <a name="relative-position"></a>相對位置

使用 **KeyTipPlacementMode** 屬性以每個項目或每個群組的方式，自訂按鍵提示的位置。

位置模式是：Top、Bottom、Right、Left、Hidden、Center 和 Auto。

![按鍵提示位置模式](images/accesskeys/keytip-postion-modes.png)

_按鍵提示位置模式_

控制項的中心線用於計算按鍵提示的垂直及水平對齊。

以下範例示範如何使用 StackPanel 容器的 KeyTipPlacementMode 屬性，設定控制項群組的按鍵提示位置。

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>位移

使用項目的 KeyTipHorizontalOffset 和 KeyTipVerticalOffset 屬性，提供對按鍵提示位置更細微的控制。

> [!NOTE]
> KeyTipPlacementMode 設定為 Auto 時無法設定位移。

KeyTipHorizontalOffset 屬性指示將按鍵提示向左或向右移動多遠。 範例示範如何設定按鈕的按鍵提示位移。

![按鍵提示位置模式](images/accesskeys/keytip-offsets.png)

_設定按鍵提示的垂直及水平位移_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>螢幕邊緣對齊 {#screen-edge-alignment .ListParagraph}

按鍵提示位置會根據螢幕邊緣自動調整，來確保完全顯示按鍵提示。 發生這種情形時，控制項和按鍵提示對齊點之間的距離可能會不同於水平和垂直位移的指定值。

![按鍵提示位置模式](images/accesskeys/keytips-screen-edge.png)

_螢幕邊緣會造成按鍵提示自動重新定位_

## <a name="key-tip-style"></a>按鍵提示樣式

我們建議使用平台佈景主題 (包括高對比) 的內建按鍵提示支援。

如果您需要指定自己的按鍵提示樣式，請使用應用程式資源例如 KeyTipFontSize (字型大小)、KeyTipFontFamily (字型系列)、KeyTipBackground (背景)、KeyTipForeground (前景)、KeyTipPadding (填補)、KeyTipBorderBrush (框線色彩)，以及 KeyTipBorderThemeThickness (框線粗細)。

![按鍵提示位置模式](images/accesskeys/keytip-customization.png)

_按鍵提示自訂選項_

這個範例示範如何變更這些應用程式資源：

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>便捷鍵和朗讀程式

XAML 架構公開自動化屬性，可讓 UI 自動化用戶端探索使用者介面項目的相關資訊。

如果您在 UIElement 或 TextElement 控制項上指定 AccessKey 屬性，可以使用 [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) 屬性取得這個值。 協助工具用戶端，例如「朗讀程式」，會在項目取得焦點時朗讀這個屬性的值。

## <a name="related-articles"></a>相關文章

* [鍵盤互動](keyboard-interactions.md)
* [鍵盤快速操作](keyboard-accelerators.md)

**範例**
* [XAML 控制項庫 (亦即 XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


