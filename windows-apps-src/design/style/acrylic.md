---
author: mijacobs
description: 一種建立透明紋理的筆刷。
title: 壓克力材質
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f9d56090e8fc1de83eeb4e8a68ca1830692c5b2f
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6468845"
---
# <a name="acrylic-material"></a>壓克力材質

![主角圖像](images/header-acrylic.svg)

壓克力是一種建立透明紋理的[筆刷](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush)。 您可以將壓克力套用到應用程式表面來增加深度，並協助建立視覺階層。  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要 API**：[AcrylicBrush 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush)、[背景屬性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>壓克力和 Fluent Design 系統

 Fluent Design 系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 壓克力是將實體紋理 (材質) 與深度加入應用程式中的 Fluent Design 系統元件。 若要深入瞭解，請參閱[適用於 UWP 的 Fluent Design 概觀](../fluent-design-system/index.md)。

 ## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>範例

:::row:::
    :::column span:::
        ![Some image](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
        **XAML Controls Gallery**<br>
        If you have the XAML Controls Gallery app installed, click <a href="xamlcontrolsgallery:/item/Acrylic">here</a> to open the app and see acrylic in action.

        <a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a><br>
        <a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Get the source code (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>壓克力混合類型
壓克力最明顯的特徵就是透明度。 有兩種壓克力類型能改變可穿透此材質顯示的項目：
 - **背景壓克力**會顯示在目前使用中應用程式背後的桌面底色圖案及其他視窗，以增加應用程式視窗之間的深度，並呈現使用者個人化的喜好設定。
 - **應用程式內壓克力**可增加應用程式框架內的深度感，讓應用程式能夠聚焦並有層次。

 ![背景壓克力](images/BackgroundAcrylic_DarkTheme.png)

 ![應用程式內壓克力](images/AppAcrylic_DarkTheme.png)

 謹慎使用多個壓克力表面分層： 多層的背景壓克力可以建立讓使用者分心的視覺錯覺。

## <a name="when-to-use-acrylic"></a>使用壓克力的時機

* 針對支援的 UI，例如 NavigationView 或命令列中的元素使用應用程式內壓克力。 
* 針對暫時性 UI 元素，例如操作功能表、 飛出視窗，以及光線 dimsissable UI 使用背景壓克力。<br />使用壓克力暫時性的案例中，有助於維持與已觸發暫時性 UI 之內容的視覺關係。

如果您使用應用程式內壓克力上瀏覽的表面，請考慮擴充下方以改善您的應用程式上流程的壓克力窗格的內容。 使用 NavigationView 會這麼做為您自動。 不過，若要避免產生條紋效果，不要嘗試進行放置多個部分壓克力邊緣到邊緣-這會產生兩個模糊表面之間不想要的接縫。 壓克力工具可讓您的設計的視覺違，但是當使用不正確，可能會導致視覺雜訊。

請考慮下列使用模式來判定將壓克力合併到您的應用程式的最佳方式：

### <a name="horizontal-navigation-or-commanding"></a>水平瀏覽或命令功能

如果您的應用程式無法利用 NavigationView，且您打算增壓克力自己，我們建議使用相對透明壓克力色調不透明度 60%。
 - 當窗格以重疊於其他應用程式內容之上的方式開啟時，則應為 [60% 應用程式內壓克力](#acrylic-theme-resources)
 - 當窗格以與主要應用程式內容並排的方式開啟時，則應為 [60% 背景壓克力](#acrylic-theme-resources)

![使用應用程式內水平命令功能的地圖應用程式](images/Maps_In_App_Acrylic_1.png)

此外，需要您的內容延伸或捲動下壓克力頂端將可讓您的應用程式提供更身歷其境且順暢的體驗。

### <a name="vertical-panes"></a>垂直窗格

對於垂直窗格或表面，可協助您的應用程式關閉內容區段，我們建議您使用不透明的背景，而不是壓克力。 如果您的垂直窗格開啟在內容上方，像是在 NavigationView 的**Collapsed**或**最少**的模式中，我們建議您使用來協助維持在頁面的內容，當使用者開啟這個窗格的應用程式內壓克力。

### <a name="transient-surfaces"></a>暫時性表面

對於有功能表飛出視窗，非強制回應快顯視窗中，應用程式或消失關閉的窗格中，我們建議使用背景壓克力。

![使用資訊的飛出視窗的郵件應用程式模式](images/Mail_TransientContextMenu.png)

許多我們的控制項預設會使用壓克力。 [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus)、 [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box)、[下拉式方塊](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox)和相似的控制項與光線 dimiss 快顯視窗將所有暫時性的壓克力時使用它們會叫用。

> [!Note]
> 呈現壓克力表面 GPU 會耗用大量時，裝置耗電量增加並縮短電池使用時間。 壓克力效果會自動停用當電池進入省電模式，使用者可以停用所有的應用程式的壓克力效果時，如果使用者選擇。

## <a name="usability-and-adaptability"></a>可用性與適應性
壓克力會自動針對各種不同的裝置與內容調適其外觀。

在高對比模式中，使用者會繼續看見其所選熟悉的背景色彩取代壓克力。 此外，背景壓克力與應用程式內壓克力會顯示成單色中：
 - 當使用者關閉設定中的透明度 > 個人化 > 色彩
 - 當省電模式啟動時
 - 當應用程式在低階硬體上執行時

此外，只有背景壓克力將會取代其半透明和紋理純色：
 - 當桌面上的應用程式視窗啟用時
 - 當 UWP 應用程式正在電話、Xbox、HoloLens 或平板電腦模式中執行時

### <a name="legibility-considerations"></a>可讀性考量事項
請務必確定您應用程式呈現給使用者的任何文字皆[符合對比率](../accessibility/accessible-text-requirements.md)。 我們已最佳化壓克力配方，讓高彩的黑色、白色，甚或是中彩的灰色文字，皆符合壓克力上面的對比率。 由平台提供的佈景主題資源預設為不透明度 80% 的對比色調色彩。 將高彩內文文字放置在壓克力上時，您可以降低色調不透明度，同時保持可讀性。 在夜間模式中，色調不透明度可以是 70%，同時淺色模式壓克力將符合 50% 不透明度的對比率。

不建議將輔色文字放置在壓克力表面，因為這些組合可能無法以 15px 字型大小通過最小對比率需求。 嘗試避免將[超連結](../controls-and-patterns/hyperlinks.md)放置在壓克力元素上。 此外，如果您選擇自訂由佈景主題資源所提供平台預設值之外的壓克力色調色彩或不透明度，請記住對可讀性的影響。

## <a name="acrylic-theme-resources"></a>壓克力佈景主題資源
您可以使用新的 XAML AcrylicBrush 或預先定義的 AcrylicBrush 佈景主題資源，輕鬆地將壓克力套用到應用程式表面。 首先，您必須決定要使用應用程式內壓克力或背景壓克力。 務必檢閱本文先前所述的通用應用程式模式以了解建議。

我們已為背景和應用程式內壓克力類型建立筆刷佈景主題資源的集合，這兩種壓克力類型與應用程式的佈景主題有關，並可視需要回復為單色。 命名為 *AcrylicWindow* 的資源代表背景壓克力，*AcrylicElement* 則是指應用程式內壓克力。

<table>
    <tr>
        <th align="center">資源索引鍵</th>
        <th align="center">色調不透明度</th>
        <th align="center"><a href="color.md">回復色彩</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>建議的使用方式︰</b>這些是以各種不同使用方式皆能正常運作的一般用途壓克力資源。 如果您的應用程式使用色彩為 AltMedium 且文字大小小於 18px 的次要文字，請將 80% 壓克力資源放置在文字之後，以<a href="../accessibility/accessible-text-requirements.md">符合對比率需求</a>。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>建議的使用方式：</b>如果您的應用程式使用次要文字色彩為 altmedium 且文字大小為 18px 或較大，您可以放置在文字的這些更半透明 70%壓克力資源。 建議在應用程式的頂端水平瀏覽與命令區中使用這些資源。  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>建議的使用方式︰</b>僅將色彩為 AltHigh 主要文字放置在壓克力之上時，您的應用程式可以利用 60% 的資源。 建議以 60% 壓克力繪製您應用程式的<a href="../controls-and-patterns/navigationview.md">垂直瀏覽窗格</a>，亦即漢堡式功能表。 </td>
    </tr>
</table>

除了非色彩相關壓克力外，我們也已新增會以使用者指定的輔色將壓克力上色的資源。 我們建議謹慎使用彩色的壓克力。 對於提供的 dark1 與 dark2 變化，將與深色佈景主題文字一致的白色或淺色文字放置在這些資源上。
<table>
    <tr>
        <th align="center">資源索引鍵</th>
        <th align="center">色調不透明度</th>
        <th align="center"><a href="color.md">色調與回復色彩</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


若要繪製特定的表面，請將其中一個上述的佈景主題資源套用到元素背景，就如同您套用任何其他筆刷資源。

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>自訂壓克力筆刷
您可以選擇將色彩色調新增到您的應用程式壓克力，以顯示品牌或與頁面上的其他元素產生視覺平衡。 若要顯示彩色而非灰階，您必須使用下列屬性定義自己的壓克力筆刷。
 - **TintColor**︰色彩/色調重疊層。 考慮指定 RGB 色彩值與 Alpha 色板不透明度。
 - **TintOpacity**︰色調層不透明度。 雖然不同色彩可能看起來更具其他 translucencies，我們建議做為起點，80%不透明度。
 - **BackgroundSource**︰此旗標可指定您要背景壓克力或應用程式內壓克力。
 - **FallbackColor**： 在省電模式中的壓克力的單色。 對於背景壓克力，當您的應用程式不是使用中桌面視窗，或當應用程式在手機與 Xbox 上執行時，回復色彩也會取代壓克力。

![淺色佈景主題壓克力色樣](images/CustomAcrylic_Swatches_LightTheme.png)

![深色佈景主題壓克力色樣](images/CustomAcrylic_Swatches_DarkTheme.png)

若要新增壓克力筆刷，請為深色、淺色及高對比佈景主題定義三個資源。 請注意在高對比中，我們建議使用 x:Key 與深色/淺色 AcrylicBrush 相同的 SolidColorBrush。

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

以下範例示範如何在程式碼中宣告 AcrylicBrush。 如果您的應用程式支援多個作業系統目標，請確定檢查使用者的電腦上是否有此 API。

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>將壓克力延伸至標題列

若要讓您的應用程式視窗呈現順暢的外觀，您可以在標題列區域中使用壓克力風格。 此範例透過設定 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 物件的 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) 以及 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) 屬性為 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent)，將壓克力風格延伸到標題列。 

```csharp
/// Extend acrylic into the title bar. 
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

您可以將這段程式碼放在應用程式的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 方法 (_App.xaml.cs_) 中，對 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) 的呼叫之後 (如這裡所示)，或放在應用程式的第一個頁面中。 


```csharp
// Call your extend acrylic code in the OnLaunched event, after 
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

此外，您必須使用 `CaptionTextBlockStyle` 來繪製採用 TextBlock 的應用程式標題，此標題通常會自動顯示在標題列中。 如需詳細資訊，請參閱[標題列自訂](../shell/title-bar.md)。

## <a name="dos-and-donts"></a>可行與禁止注意事項
* 請使用壓克力作為非主要應用程式表面 (如瀏覽窗格) 的背景材質。
* 請將壓克力延伸至應用程式的至少一個邊緣，藉此與應用程式背景巧妙地混合以呈現無縫的效果。
* 不要在您的應用程式的大型背景表面上放置桌面 arylic-這會中斷主要用於暫時性表面的壓克力心理模式。
* 請勿將應用程式內壓克力與背景壓克力直接相鄰放置，以避免在接縫處產生視覺壓力。
* 請勿將多個色調與不透明度相同的壓克力窗格彼此相鄰，因為這會產生不想要讓人看到的接縫。
* 請勿將輔色文字放置在壓克力表面。

## <a name="how-we-designed-acrylic"></a>我們如何設計壓克力

我們將壓克力的重要元件微調至讓壓克力有獨特的外觀與屬性。 我們一開始使用半透明、 模糊並將視覺的深度和維度新增到平面的雜訊。 我們新增了排除混合模式層，以確保放置在壓克力背景上的對比與可讀性。 最後，我們加入了色彩色調讓壓克力個人化。 這些圖層最後一齊產生全新、可使用的材質。

![壓克力配方](images/AcrylicRecipe_Diagram.jpg)
<br/>壓克力配方：背景、排除混合、色彩/色調重疊、雜訊


## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

[**顯色顯目提示**](reveal.md)