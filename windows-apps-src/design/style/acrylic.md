---
description: 能建立半透明紋理的筆刷類型。
title: 壓克力材質
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8d969c5282fa03fb11d108d2b2c8e0fe44dfde49
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968453"
---
# <a name="acrylic-material"></a>壓克力材質

![主角圖像](images/header-acrylic.svg)

Acrylic 是一種能建立半透明紋理的 [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) \(英文\) 類型。 您可以將壓克力套用到應用程式表面來增加深度，並協助建立視覺階層。  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要 API**：[AcrylicBrush 類別](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.acrylicbrush) \(英文\)、[Background 屬性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.Background) \(英文\)

:::row:::
    :::column:::
淺色主題中的壓克力![淺色主題中的壓克力](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
深色主題中的壓克力![深色主題中的壓克力](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acrylic 和 Fluent Design 系統

 Fluent Design 系統能協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 Acrylic 是 Fluent Design 系統元件，其能將實體紋理 (材質) 與深度加入應用程式。 若要深入了解，請參閱 [Fluent Design 概觀](/windows/apps/fluent-design-system)。

 ## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>範例

:::row:::
    :::column span:::
![一些影像](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML 控制項庫**<br>
如果您已安裝 XAML 控制項庫應用程式，請按一下<a href="xamlcontrolsgallery:/item/Acrylic">此處</a>以開啟該應用程式並查看壓克力的運作方式。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>壓克力混合類型
Acrylic 最明顯的特徵就是它的透明度。 有兩種壓克力混合類型能改變可穿透此材質顯示的項目：
 - **背景壓克力**會顯示位於目前作用中應用程式背後的桌面背景及其他視窗，以增加應用程式視窗之間的深度，並呈現使用者個人化的喜好設定。
 - **應用程式內壓克力**能增加應用程式框架內的深度感，讓應用程式能夠聚焦並有層次。

 ![背景壓克力](images/BackgroundAcrylic_DarkTheme.png)

 ![應用程式內壓克力](images/AppAcrylic_DarkTheme.png)

 請謹慎堆疊多個壓克力表面：多層背景壓克力可能會產生令人分心的視覺錯覺。

## <a name="when-to-use-acrylic"></a>使用壓克力的時機

* 將應用程式內壓克力用來支援 UI，例如在捲動或互動時可能會使內容重疊的表面上。
* 將背景壓克力用於暫時性的 UI 元素，例如操作功能表、飛出視窗，以及可消失關閉的 UI。<br />在暫時性案例中使用 Acrylic 可協助維持與觸發暫時性 UI 的內容之間的視覺關聯性。

如果您在瀏覽表面上使用應用程式內壓克力，請考慮將內容延伸至壓克力窗格下，以改善應用程式上的流程。 使用 NavigationView 將能自動為您做到這點。 不過，為了避免產生分割效果，請試著不要將多個壓克力組件以相連的方式放置在一起，因為這可能會在兩個模糊表面之間產生不必要的接合。 Acrylic 是一個能讓您的設計賞心悅目的工具，但若使用方式不正確，便可能會產生視覺上的干擾。

請考慮採用下列使用模式來決定將壓克力融入應用程式的最佳方式：

### <a name="vertical-panes"></a>垂直窗格

針對能協助區隔應用程式內容的垂直窗格或表面，我們建議使用不透明的背景，而非壓克力。 如果您的垂直窗格會在內容上方開啟 (例如 NavigationView 的 **Compact** 或 **Minimal** 模式)，我們建議您使用應用程式內壓克力來協助在使用者開啟此窗格時維持頁面的內容。

### <a name="transient-surfaces"></a>暫時性表面

針對具有功能表飛出視窗、非強制回應快顯示窗，或是消失關閉窗格的應用程式，建議使用背景壓克力。

![使用資訊飛出視窗的郵件應用程式模式](images/Mail_TransientContextMenu.png)

我們的許多控制項預設都會使用壓克力。 [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus) \(部分機器翻譯\)、[AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box) \(部分機器翻譯\)、[ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) \(英文\) 及具有消失關閉快顯示窗的類似控制項，都會在被叫用時使用暫時性壓克力。

> [!Note]
> 呈現壓克力表面會大量佔用 GPU，這可能會提升裝置耗電量並縮短電池使用時間。 當電池進入省電模式時，壓克力效果會自動停用，使用者也可以選擇停用所有應用程式的壓克力效果。

## <a name="usability-and-adaptability"></a>可用性與適應性
壓克力會自動針對各種不同的裝置與內容調整其外觀。

在高對比模式中，使用者會繼續看見其所選擇的熟悉背景色彩，而非壓克力效果。 此外，背景壓克力與應用程式內壓克力都會在下列狀況中顯示為純色：
 - 當使用者在 [設定] > [個人化] > [色彩] 中關閉透明度時
 - 當省電模式啟動時
 - 當應用程式在低階硬體上執行時

此外，在下列狀況中，只有背景壓克力會以純色取代其透明度與紋理：
 - 當桌面上的應用程式視窗停用時
 - 當 Windows 應用程式正在電話、Xbox、HoloLens 或平板電腦模式中執行時

### <a name="legibility-considerations"></a>可讀性考量
請務必確定您應用程式呈現給使用者的任何文字皆[符合對比率](../accessibility/accessible-text-requirements.md)。 我們已最佳化壓克力的配方，來使高彩黑色、白色，或甚至中彩的灰色文字都能在壓克力上方符合對比率。 由平台所提供的佈景主題資源預設為不透明度 80% 的對比色調色彩。 將高彩本文在壓克力上顯示時，您可以在降低色調不透明度的情況下保持可讀性。 在深色模式中，色調不透明度可以是 70%，而淺色模式壓克力將能在 50% 不透明度時符合對比率。

我們不建議將輔色文字放置在壓克力表面上，因為這些組合很可能無法在 15px 字型大小的情況下通過最低對比率需求。 請嘗試避免將[超連結](../controls-and-patterns/hyperlinks.md)放置在壓克力元素上。 此外，如果您選擇不按照由佈景主題資源所提供的平台預設值並自訂壓克力色調色彩或不透明度，請牢記其對可讀性的影響。

## <a name="acrylic-theme-resources"></a>壓克力佈景主題資源
您可以使用新的 XAML AcrylicBrush 或預先定義的 AcrylicBrush 佈景主題資源，輕鬆地將壓克力套用到應用程式表面。 首先，您必須決定要使用應用程式內壓克力或背景壓克力。 請務必檢閱本文先前所述的常見應用程式模式以取得建議。

我們已針對背景和應用程式內壓克力類型建立筆刷佈景主題資源集合，這些資源會採用應用程式的佈景主題，並會視需求切換回純色。 命名為 *AcrylicWindow* 的資源代表背景壓克力，而 *AcrylicElement* 則是指應用程式內壓克力。

<table>
    <tr>
        <th align="center">資源索引鍵</th>
        <th align="center">色調不透明度</th>
        <th align="center"><a href="color.md">回復色彩</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush、SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush、SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush、SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush、SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush、SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush、SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>建議的使用方式：</b>這些是能在眾多使用方式中正常運作的一般用途壓克力資源。 如果您的應用程式使用色彩為 AltMedium 且文字大小小於 18px 的次要文字，請將 80% 壓克力資源放置在文字之後以<a href="../accessibility/accessible-text-requirements.md">符合對比率需求</a>。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush、SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush、SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>建議的使用方式：</b>如果您的應用程式使用色彩為 AltMedium 且文字大小為 18px 或以上的次要文字，您可以將這些更加透明的 70% 壓克力資源放在文字後面。 建議在應用程式的頂端水平瀏覽與命令區中使用這些資源。  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush、SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush、SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush、SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush、SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush、SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush、SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>建議的使用方式：</b>僅將色彩為 AltHigh 的主要文字放置在壓克力之上時，您的應用程式可以利用這些 60% 的資源。 建議以 60% 壓克力繪製您應用程式的<a href="../controls-and-patterns/navigationview.md">垂直瀏覽窗格</a>，亦即漢堡式功能表。 </td>
    </tr>
</table>

除了色彩中性壓克力之外，我們也已新增會以使用者指定的輔色將壓克力上色的資源。 我們建議謹慎使用彩色的壓克力。 針對所提供的 dark1 與 dark2 變化，請將與深色佈景主題文字色彩一致的白色或淺色文字放置在這些資源上。
<table>
    <tr>
        <th align="center">資源索引鍵</th>
        <th align="center">色調不透明度</th>
        <th align="center"><a href="color.md">色調與回復色彩</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush、SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush、SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80% </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush、SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70% </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


若要繪製特定的表面，請將其中一個上述的佈景主題資源套用到元素背景，如同您套用任何其他筆刷資源一般。

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>自訂壓克力筆刷
您可以選擇將色彩色調新增到您應用程式的壓克力來顯示品牌，或與頁面上的其他元素產生視覺平衡。 若要顯示彩色而非灰階，您必須使用下列屬性定義自己的壓克力筆刷。
 - **TintColor**︰色彩/色調重疊層。 請考慮指定 RGB 色彩值與 Alpha 色板不透明度。
 - **TintOpacity**︰色調層的不透明度。 建議一開始設定 80% 不透明度，儘管不同的色彩在其他透明度下可能看起來更具吸引力。
 - **TintLuminosityOpacity**：控制背景透過壓克力表面所顯示的飽和度。
 - **BackgroundSource**︰此旗標可指定是否要背景壓克力或應用程式內壓克力。
 - **FallbackColor**︰在省電模式中會取代表壓克力的純色。 對於背景壓克力，當您的應用程式不是作用中的桌面視窗時，或當應用程式是在手機與 Xbox 上執行時，回復色彩也會取代壓克力。

![淺色佈景主題壓克力樣本](images/CustomAcrylic_Swatches_LightTheme.png)

![深色佈景主題壓克力樣本](images/CustomAcrylic_Swatches_DarkTheme.png)

![與色調不透明度相比的亮度不透明度](images/LuminosityVersusTint.png)

若要新增壓克力筆刷，請針對深色、淺色及高對比佈景主題定義三個資源。 請注意，在高對比中，我們建議使用 SolidColorBrush 並搭配與深色/淺色 AcrylicBrush 相同的 x:Key。

> [!Note]
> 如果您不指定 TintLuminosityOpacity 值，系統將會根據您的 TintColor 和 TintOpacity 自動調整其值。

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
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
            TintLuminosityOpacity="0.5"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

以下範例示範如何在程式碼中宣告 AcrylicBrush。 如果您的應用程式支援多個作業系統目標，請務必檢查使用者電腦上是否有此 API。

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.AcrylicBrush"))
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

若要讓您的應用程式視窗呈現順暢的外觀，您可以在標題列區域中使用壓克力。 此範例透過將 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) \(英文\) 物件的 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) \(英文\) 及 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) \(英文\) 屬性設定為 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent) \(英文\)，來將壓克力延伸到標題列。

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

您可以在對 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) \(英文\) 的呼叫之後 (如這裡所示)，將此程式碼置於您應用程式的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) \(英文\) 方法 (_App.xaml.cs_) 中，或是將它置於您應用程式的第一個頁面。

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

此外，您必須使用 `CaptionTextBlockStyle` 搭配 TextBlock 繪製您應用程式的標題，其通常會自動顯示在標題列中。 如需詳細資訊，請參閱[標題列自訂](../shell/title-bar.md)。

## <a name="dos-and-donts"></a>可行與禁止事項
* 請使用壓克力作為非主要應用程式表面 (如瀏覽窗格) 的背景材質。
* 請至少將壓克力延伸至應用程式的其中一個邊緣，來與應用程式的週遭巧妙混合，以呈現順暢的體驗。
* 請勿將桌面壓克力置於應用程式的大型背景表面上，這會干擾壓克力主要是用於暫時性表面的心智模型。
* 請勿將應用程式內壓克力與背景壓克力直接相鄰放置，以避免在接縫處產生視覺壓力。
* 請勿將具有相同色調與不透明度的多個壓克力窗格彼此相鄰，因為這會產生不理想的可見接縫。
* 請勿將輔色文字放置在壓克力表面上。

## <a name="how-we-designed-acrylic"></a>我們設計壓克力的方式

我們對壓克力的重要元件進行微調，以締造出壓克力獨特的外觀與屬性。 我們從透明度、模糊與雜訊開始，以增加平面表面的深度與維度。 我們加入排除混合模式層，以確保放置在壓克力背景上之 UI 的對比與可讀性。 最後，我們加入色彩色調來提供個人化的可能性。 這些層面會相互搭配，並產生全新且可使用的材質。

![壓克力配方](images/AcrylicRecipe_Diagram.jpg)
<br/>壓克力配方：背景、模糊、排除混合、色彩/色調重疊、雜訊


## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

[**顯示顯目提示**](reveal.md)
