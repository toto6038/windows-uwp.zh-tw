---
author: Karl-Bridge-Microsoft
title: 適用於鍵盤、遊戲台、遙控器與協助工具的焦點瀏覽
Description: ''
label: ''
template: detail.hbs
keywords: 鍵盤, 遊戲控制器, 遙控器, 瀏覽, 方向內部瀏覽, 方向區域, 瀏覽策略, 輸入, 使用者互動, 協助工具, 可用性
ms.date: 03/02/2018
ms.author: kbridge
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f4c86847e02c4ba987f8b1dcc42016145b175193
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5822399"
---
# <a name="focus-navigation-for-keyboard-gamepad-remote-control-and-accessibility-tools"></a>適用於鍵盤、遊戲台、遠端控制與協助工具的焦點瀏覽

![鍵盤、遠端與方向鍵](images/dpad-remote/dpad-remote-keyboard.png)

使用焦點瀏覽可在您的 UWP app 中提供完整且一致的互動體驗，以及為鍵盤使用者、行動不便或有其他可存取性需求的人士提供自訂控制項、電視畫面及 Xbox One 的 10 英尺體驗。

## <a name="overview"></a>概觀

焦點瀏覽指的是讓使用者能使用鍵盤、遊戲台或遙控器，透過 UWP 應用程式的 UI 瀏覽和互動的基礎機制。

> [!NOTE] 
> 輸入裝置通常會分類為指標裝置 (例如觸控、觸控板、手寫筆和滑鼠)，以及非指標裝置 (例如鍵盤、遊戲台和遙控器)。

本主題描述如何為倚賴非指標輸入類型的使用者最佳化 UWP 應用程式和建置自訂互動體驗。 

雖然我們聚焦於電腦上 UWP app 中自訂控制的鍵盤輸入，設計良好的鍵盤體驗對軟體鍵盤 (例如觸控式鍵盤和螢幕小鍵盤 (OSK))、支援協助工具 (例如 Windows 朗讀程式) 和支援 10 英尺體驗來說也是相當重要的。

請參閱[處理指標輸入](handle-pointer-input.md)以取得為指標裝置在 UWP 應用程式建置自訂體驗的指導。

如需更多為鍵盤建置應用程式和體驗的一般資訊，請參閱[鍵盤互動](keyboard-interactions.md)。

## <a name="general-guidance"></a>一般指導方針
只有需要使用者互動的 UI 元素才需要支援焦點瀏覽。不需要採取動作的元素，例如靜態圖片，則不需要鍵盤焦點。 螢幕助讀程式和相似的協助工具仍會宣讀這些靜態元素，即使他們並未包含在焦點瀏覽中。 

請務必記得，焦點瀏覽和使用指標裝置 (例如滑鼠或觸控) 瀏覽的不同之處，在於焦點瀏覽是線性的。 實作焦點瀏覽時，請考慮您的使用者與應用程式互動的方式，以及什麼樣的瀏覽才是合乎邏輯的方式。 在大部分的案例中，我們建議您遵循使用者文化的慣用閱讀模式來自訂焦點瀏覽。

其他焦點瀏覽考量項目包括：
- 控制項是依照邏輯分組的嗎？
- 有任何重要性更高的控制項群組嗎？ 
   - 如果有，那些群組是否包含子群組？
- 配置需要自訂方向瀏覽 (方向鍵) 和定位順序嗎？

[針對協助工具的軟體工程設計](https://www.microsoft.com/download/details.aspx?id=19262)電子書，針對*設計邏輯階層*有一篇絕佳章節。

## <a name="2d-directional-navigation-for-keyboard"></a>適用於鍵盤的 2D 方向瀏覽

控制項或控制項群組的 2D 內部瀏覽區域，稱為其「方向區域」。 當焦點移動到此物件時，鍵盤方向鍵 (左、右、上、下) 可用來瀏覽方向區域內的子元素。

![方向區域](images/keyboard/directional-area-small.png)

*控制項群組的 2D 內部瀏覽區域，或方向區域*

您可以使用 [XYFocusKeyboardNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_XYFocusKeyboardNavigation) 屬性 (其可能值為 [Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)、[Enabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) 或 [Disabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)) 管理鍵盤方向鍵的 2D 內部瀏覽。

> [!NOTE]  
> 定位順序不會受到此屬性影響。 若要避免產生混亂的瀏覽體驗，我們建議「不要」** 在您應用程式的 Tab 瀏覽順序中明確指定方向區域的子元素。 請參閱 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 和 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 屬性以取得元素定位行為的詳細資訊。  

### <a name="autohttpsdocsmicrosoftcomuwpapiwindowsuixamlinputxyfocuskeyboardnavigationmode-default-behavior"></a>[Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode) (預設行為)

當設定為 Auto 時，方向瀏覽的行為會由元素的上階或繼承階層決定。 如果所有上階都是預設模式 (設定為 **Auto**)，則「不」** 支援使用鍵盤的方向瀏覽。

### [<a name="disabled"></a>停用](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

將 **XYFocusKeyboardNavigation** 設為 **Disabled** 則會封鎖控制項及其子元素的方向瀏覽。

![XYFocusKeyboardNavigation 停用行為](images/keyboard/xyfocuskeyboardnav-disabled.gif)

*XYFocusKeyboardNavigation 停用行為*

在此範例中，主要 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) 已將 **XYFocusKeyboardNavigation** 設為 **Enabled**。 所有子元素都會繼承此設定，可使用方向鍵進行瀏覽。 但是，B3 和 B4 元素則位於次要 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary) 中，其 **XYFocusKeyboardNavigation** 已設為 **Disabled**，因此會覆寫主要容器並停用自身和其子元素間的方向鍵瀏覽。

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="75"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
                Grid.Row="0" 
                FontWeight="ExtraBold" 
                HorizontalTextAlignment="Center"
                TextWrapping="Wrap" 
                Padding="10" />
    <StackPanel Name="ContainerPrimary" 
                XYFocusKeyboardNavigation="Enabled" 
                KeyDown="ContainerPrimary_KeyDown" 
                Orientation="Horizontal" 
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" />
        <Button Name="B2" 
                Content="B2" 
                GettingFocus="Btn_GettingFocus" />
        <StackPanel Name="ContainerSecondary" 
                    XYFocusKeyboardNavigation="Disabled" 
                    Orientation="Horizontal" 
                    BorderBrush="Red" 
                    BorderThickness="2">
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" />
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" />
        </StackPanel>
    </StackPanel>
</Grid>
```

### [<a name="enabled"></a>啟用](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocuskeyboardnavigationmode)

將 **XYFocusKeyboardNavigation** 設為 **Enabled** 則會支援控制項和它每一個 [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 子物件的 2D 方向瀏覽。

當設定時，使用方向鍵瀏覽會限制在方向區域內的元素。 Tab 瀏覽不會受到影響，因為所有控制項仍然可透過其定位順序階層進行存取。

![XYFocusKeyboardNavigation 啟用行為](images/keyboard/xyfocuskeyboardnav-enabled.gif)

*XYFocusKeyboardNavigation 啟用行為*

在此範例中，主要 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerPrimary) 已將 **XYFocusKeyboardNavigation** 設為 **Enabled**。 所有子元素都會繼承此設定，可使用方向鍵進行瀏覽。 B3 和 B4 元素位於次要 [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) (ContainerSecondary) 中，其中並未設定 **XYFocusKeyboardNavigation**，因此會繼承主要容器設定。 B5 元素並未位於宣告的方向區域中，因此不支援方向鍵瀏覽，但支援 Tab 瀏覽行為。

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="100"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Grid.Row="1"
                Orientation="Horizontal"
                HorizontalAlignment="Center">
        <StackPanel Name="ContainerPrimary"
                    XYFocusKeyboardNavigation="Enabled"
                    KeyDown="ContainerPrimary_KeyDown"
                    Orientation="Horizontal"
                    BorderBrush="Green"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B1"
                    Content="B1"
                    GettingFocus="Btn_GettingFocus" Margin="5" />
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus" />
            <StackPanel Name="ContainerSecondary"
                        Orientation="Horizontal"
                        BorderBrush="Red"
                        BorderThickness="2"
                        Margin="5">
                <Button Name="B3"
                        Content="B3"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
                <Button Name="B4"
                        Content="B4"
                        GettingFocus="Btn_GettingFocus"
                        Margin="5" />
            </StackPanel>
        </StackPanel>
        <Button Name="B5"
                Content="B5"
                GettingFocus="Btn_GettingFocus"
                Margin="5" />
    </StackPanel>
</Grid>
```

您可以有多個層級的巢狀方向區域。 如果所有父元素都將 XYFocusKeyboardNavigation 設為 Enabled，則會忽略內部瀏覽區域邊界。

以下是位於不明確支援 2D 方向瀏覽元素中兩個巢狀方向區域的範例。 在此案例中，兩個巢狀區域間不支援方向瀏覽。

![XYFocusKeyboardNavigation 啟用及巢狀行為](images/keyboard/xyfocuskeyboardnav-enabled-nested1.gif)

*XYFocusKeyboardNavigation 啟用及巢狀行為*

以下是三個巢狀方向區域更複雜的範例，其中：

-   當 B1 有焦點時，只能瀏覽至 B5 (反之亦然)，因為 XYFocusKeyboardNavigation 設為 Disabled 的位置存在方向區域邊界，使用者無法使用方向鍵瀏覽至 B2、B3 和 B4。
-   當 B2 有焦點時，只能瀏覽至 B3 (反之亦然)，因為方向區域邊界會防止使用方向鍵瀏覽至 B1、B4 和 B5
-   當 B4 有焦點時，必須使用 TAB 鍵巡覽控制項。

![XYFocusKeyboardNavigation 啟用及複雜巢狀行為](images/keyboard/xyfocuskeyboardnav-enabled-nested2.gif)

*XYFocusKeyboardNavigation 啟用及複雜巢狀行為*

## <a name="tab-navigation"></a>Tab 瀏覽

當可使用方向鍵進行控制項或控制項群組的 2D 方向瀏覽時，仍然可以使用 TAB 鍵巡覽 UWP 應用程式中的所有控制項。 

所有互動式控制項都預設支援 TAB 鍵瀏覽 ([IsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_IsEnabled) 和 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 屬性為 **true**)，其中邏輯定位順序會衍生自您應用程式中的控制項配置。 不過，預設的順序不一定和肉眼觀察的順序一致。 實際的顯示位置可能取決於上層配置容器，以及可在子元素上設定而影響配置的某些屬性。

避免讓焦點在應用程式中四處跳躍的自訂定位順序。 例如，表單中的控制項清單應該會有由上至下和由左至右的定位順序 (取決於地區)。

在本節中，我們會描述如何完全自訂此定位順序，使其適合您的應用程式。

### <a name="set-the-tab-navigation-behavior"></a>設定 Tab 瀏覽行為 

[UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 的 [TabFocusNavigation](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 屬性會指定整個物件樹狀結構 (或方向區域) 的 Tab 瀏覽。

> [!NOTE]
> 針對不使用 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) 的物件使用此屬性而非 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) 屬性，以定義其外觀。

如同我們在先前章節提到的，若要避免產生混亂的瀏覽體驗，我們建議「不要」** 在您應用程式的 Tab 瀏覽順序中明確指定方向區域的子元素。 請參閱 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 和 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 屬性以取得元素定位行為的詳細資訊。   
> 針對 Windows 10 Creators Update (組建 10.0.15063) 先前的版本，Tab 設定僅限 [ControlTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.controltemplate) 物件。 如需詳細資訊，請參閱 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation)。

[TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 具有 [KeyboardNavigationMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardnavigationmode) 類型的值，其可能值如下 (請注意這些範例並非自訂控制群組，因此不需要使用方向鍵的內部瀏覽)：

- **區域** (預設)   
  標籤索引會在容器內的位置子樹上識別。 針對此範例，定位順序為 B1、B2、B3、B4、B5、B6、B7、B1。

   ![「區域」Tab 瀏覽行為](images/keyboard/tabnav-local.gif)

   *「區域」Tab 瀏覽行為*

- **一次**  
  容器和所有子元素都會取得焦點一次。 針對此範例，定位順序為 B1、B2、B7、B1 (此處也會同時展示使用方向鍵的內部瀏覽)。

   ![「一次」Tab 瀏覽行為](images/keyboard/tabnav-once.gif)

   *「一次」Tab 瀏覽行為*

- **循環**   
  焦點循環會回到容器內第一個可設定為焦點的元素。 針對此範例，定位順序為 B1、B2、B3、B4、B5、B6、B2...

   ![「循環」Tab 瀏覽行為](images/keyboard/tabnav-cycle.gif)

   *「循環」Tab 瀏覽行為*

以下是先前範例的程式碼 (TabFocusNavigation =「循環」)。

```XAML
<Grid 
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" 
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0" 
               FontWeight="ExtraBold" 
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap" 
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown" 
                Orientation="Horizontal" 
                HorizontalAlignment="Center"
                BorderBrush="Green" 
                BorderThickness="2" 
                Grid.Row="1" 
                Padding="10" 
                MaxWidth="200">
        <Button Name="B1" 
                Content="B1" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
        <StackPanel Name="ContainerSecondary" 
                    KeyDown="Container_KeyDown"
                    XYFocusKeyboardNavigation="Enabled" 
                    TabFocusNavigation ="Cycle"
                    Orientation="Vertical" 
                    VerticalAlignment="Center"
                    BorderBrush="Red" 
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2" 
                    Content="B2" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B3" 
                    Content="B3" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B4" 
                    Content="B4" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B5" 
                    Content="B5" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
            <Button Name="B6" 
                    Content="B6" 
                    GettingFocus="Btn_GettingFocus" 
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7" 
                Content="B7" 
                GettingFocus="Btn_GettingFocus" 
                Margin="5"/>
    </StackPanel>
</Grid>
```

### [<a name="tabindex"></a>TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex)

使用 [TabIndex](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabIndex) 指定使用者使用 TAB 鍵巡覽控制項時，元素取得焦點的順序。 擁有較低標籤索引的控制項會在擁有較高索引的控制項之後取得焦點。

當控制項沒有指定 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 時，它便會根據範圍，指派高於目前視覺化樹狀結構中所有互動控制項內最高索引值的索引值 (最低優先順序)， 

所有控制項的子元素都會視為在同一個範圍內，若其中有一個元素也有子元素，則會視為另一個範圍。 任何語意模糊都會透過選擇範圍內視覺化樹狀結構的第一個元素來解析。 

若要將控制項從定位順序中排除，請將 [IsTabStop](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_IsTabStop) 屬性設為 **false**。

設定 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 屬性以覆寫預設定位順序。

> [!NOTE] 
> [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 的運作方式與 [UIElement.TabFocusNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement#Windows_UI_Xaml_UIElement_TabFocusNavigation) 和 [Control.TabNavigation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_TabNavigation) 相同。


在這裡我們會展示焦點瀏覽受到特定元素上 [TabIndex](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control#Windows_UI_Xaml_Controls_Control_TabIndex) 屬性影響的方式。 

![使用 TabIndex 的「區域」Tab 瀏覽行為](images/keyboard/tabnav-tabindex.gif)

*使用 TabIndex 的「區域」Tab 瀏覽行為*

在先前的範例中，有兩個範圍： 
- B1，方向區域 (B2 - B6)，和 B7
- 方向區域 (B2 - B6)

當 B3 (位於方向區域中) 取得焦點時，範圍會產生變更，Tab 瀏覽則會傳輸到識別出下一個最佳焦點的方向區域。 在此案例中，B2 之後為 B4、B5 和 B6。 範圍會再次變更，焦點會移動到 B1。

以下是此範例的程式碼。

```xaml
<Grid
    Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"
    TabFocusNavigation="Cycle">
    <Grid.RowDefinitions>
        <RowDefinition Height="40"/>
        <RowDefinition Height="300"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <TextBlock Name="KeyPressed"
               Grid.Row="0"
               FontWeight="ExtraBold"
               HorizontalTextAlignment="Center"
               TextWrapping="Wrap"
               Padding="10" />
    <StackPanel Name="ContainerPrimary"
                KeyDown="Container_KeyDown"
                Orientation="Horizontal"
                HorizontalAlignment="Center"
                BorderBrush="Green"
                BorderThickness="2"
                Grid.Row="1"
                Padding="10"
                MaxWidth="200">
        <Button Name="B1"
                Content="B1"
                TabIndex="1"
                ToolTipService.ToolTip="TabIndex = 1"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
        <StackPanel Name="ContainerSecondary"
                    KeyDown="Container_KeyDown"
                    TabFocusNavigation ="Local"
                    Orientation="Vertical"
                    VerticalAlignment="Center"
                    BorderBrush="Red"
                    BorderThickness="2"
                    Padding="5" Margin="5">
            <Button Name="B2"
                    Content="B2"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B3"
                    Content="B3"
                    TabIndex="3"
                    ToolTipService.ToolTip="TabIndex = 3"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B4"
                    Content="B4"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B5"
                    Content="B5"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
            <Button Name="B6"
                    Content="B6"
                    GettingFocus="Btn_GettingFocus"
                    Margin="5"/>
        </StackPanel>
        <Button Name="B7"
                Content="B7"
                TabIndex="2"
                ToolTipService.ToolTip="TabIndex = 2"
                GettingFocus="Btn_GettingFocus"
                Margin="5"/>
    </StackPanel>
</Grid>
```

## <a name="2d-directional-navigation-for-keyboard-gamepad-and-remote-control"></a>適用於鍵盤、遊戲台和遙控器的 2D 方向瀏覽

非指標輸入類型 (例如鍵盤、遊戲台、遙控器和協助工具，例如 Windows 朗讀程式等) 針對巡覽和與您 UWP 應用程式的 UI 互動都有相同的基礎機制。

在本節中，我們涵蓋如何指定慣用的瀏覽策略和透過一組支援所有焦點式、非指標輸入類型的瀏覽策略屬性微調您應用程式內的焦點瀏覽。

如需更多建置 Xbox/TV 應用程式和體驗的一般資訊，請參閱[鍵盤互動](keyboard-interactions.md)和[為 Xbox 和 TV 設計](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction)。

### <a name="navigation-strategies"></a>瀏覽策略

> 瀏覽策略適用於鍵盤、遊戲台、遙控器和各種協助工具。

以下瀏覽策略屬性可讓您影響根據方向鍵、方向鍵 (D-pad) 按鈕或按下的相似按鍵取得焦點的控制項。 

-   XYFocusUpNavigationStrategy
-   XYFocusDownNavigationStrategy
-   XYFocusLeftNavigationStrategy
-   XYFocusRightNavigationStrategy

這些屬性的可能值為[Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy) (預設)、[NavigationDirectionDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)、[Projection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)，或 [RectilinearDistance](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.xyfocusnavigationstrategy)。

如果設為 **Auto**，元素行為是根據元素的上階。 如果所有元素都設定為 **Auto**，則使用 **Projection**。

> [!NOTE]
> 其他如先前取得焦點之元素或瀏覽方向軸鄰近性等因素會影響結果。

### <a name="projection"></a>Projection

Projection 策略會在目前取得焦點元素的邊緣受到「投影」** 時，將焦點移至遇到的第一個元素。

在此範例中，每個焦點瀏覽方向都設為 Projection。 請注意焦點從 B1 向下移動到 B4，並略過 B3 的方式。 這是因為 B3 並未位於投影區域中。 也請注意從 B1 往左側移動時並未識別出焦點候選項目。 這是因為 B2 相對於 B1 的位置會排除 B3 作為候選項目。 若 B3 與 B2 位於同一列上，便會是左側瀏覽的可用候選項目。 因為其未受阻礙的與瀏覽方向軸的鄰近性，B2 為可用的候選項目。

![投影瀏覽策略](images/keyboard/xyfocusnavigationstrategy-projection.gif)

*投影瀏覽策略*

### <a name="navigationdirectiondistance"></a>NavigationDirectionDistance

NavigationDirectionDistance 策略會將焦點移至最接近瀏覽方向軸的元素。

對應到瀏覽方向的週框邊緣會「延伸」** 並「投影」**，以識別候選目標。 系統會將碰到的第一個元素視為目標。 如果有多個候選項目，則會將最靠近的元素視為目標。 如果仍有多個候選項目，則會將最上方/最左邊的元素視為候選項目。

![NavigationDirectionDistance 瀏覽策略](images/keyboard/xyfocusnavigationstrategy-navigationdirectiondistance.gif)

*NavigationDirectionDistance 瀏覽策略*

### <a name="rectilineardistance"></a>RectilinearDistance

RectilinearDistance 策略會根據 2D 直線距離 ([計程車幾何](https://en.wikipedia.org/wiki/Taxicab_geometry)) 將焦點移至最接近的元素。

會使用與每個潛在候選項目的主要距離和次要距離的加總識別最佳候選項目。 在平局中，如果是要求的方向是向上或向下，會選取左邊的第一個元素。如果要求的方向是是向左或向右，則選取頂端的第一個元素。

![RectilinearDistance 瀏覽策略](images/keyboard/xyfocusnavigationstrategy-rectilineardistance.gif)

*RectilinearDistance 瀏覽策略*

此影像顯示當 B1 取得焦點且要求方向為向下時，B3 便是 RectilinearDistance 焦點候選項目。 這是根據針對此範例的以下計算而得：
-   距離 (B1, B3, 向下) = 10 + 0 = 10
-   距離 (B1, B2, 向下) = 0 + 40 = 30
-   距離 (B1, D, 向下) = 30 + 0 = 30


## <a name="related-articles"></a>相關文章
- [程式設計焦點瀏覽](focus-navigation-programmatic.md)
- [鍵盤互動](keyboard-interactions.md)
- [鍵盤協助工具](../accessibility/keyboard-accessibility.md) 



