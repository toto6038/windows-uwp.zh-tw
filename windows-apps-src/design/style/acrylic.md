---
author: mijacobs
description: "一種筆刷，可建立部分透明的紋理。"
title: "壓克力材質"
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: high
ms.openlocfilehash: 8f8266878356ae182b6abf59a6729bf7066d6e4c
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2018
---
# <a name="acrylic-material"></a>壓克力材質

壓克力是一種[筆刷](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush)，可建立部分透明的紋理。 您可以將壓克力套用到應用程式表面來增加深度，並協助建立視覺階層。  <!-- By allowing user-selected wallpaper or colors to shine through, Acrylic keeps users in touch with the OS personalization they've chosen. -->

> **重要 API**：[AcrylicBrush 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush)、[背景屬性](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control#Windows_UI_Xaml_Controls_Control_Background)


![在淺色佈景主題中的壓克力](images/Acrylic_DarkTheme_Base.png)

![在深色佈景主題中的壓克力](images/Acrylic_LightTheme_Base.png)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下此處以<a href="xamlcontrolsgallery:/item/Acrylic">開啟應用程式並查看壓克力材質運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="acrylic-and-the-fluent-design-system"></a>壓克力和 Fluent 設計系統

 Fluent 設計系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 壓克力是將實體紋理 (材質) 與深度加入應用程式中的 Fluent 設計系統元件。 若要深入瞭解，請參閱[適用於 UWP 的 Fluent 設計概觀](../fluent-design-system/index.md)。

## <a name="when-to-use-acrylic"></a>使用壓克力的時機

我們建議您將支援的 UI (例如應用程式內瀏覽或命令元素) 放置在壓克力表面上。 此材質對於如對話方塊與飛出視窗等暫時性 UI 元素很有幫助，因為它有助於維持與已觸發暫時性 UI 之內容間的視覺關係。 我們設計出壓克力做為背景材質，並顯示在講究視覺呈現的窗格中，因此請勿將壓克力套用到精細的前景元素上。

於主要應用程式內容後的表面應採用實心、不透明的背景。

請考慮讓壓克力延伸到應用程式的一或多個邊緣，包括視窗標題列，以改善視覺流程。 透過將不同混合類型的壓克力彼此相鄰地堆疊在一起，可避免產生條紋效果。 壓克力是一種讓您的設計賞心悅目的工具，但若使用方式不正確，會產生視覺上的違和感。

請考慮採用下列使用模式來判定將壓克力融入應用程式的最佳方式。

### <a name="vertical-acrylic-pane"></a>垂直壓克力窗格

對於採用垂直瀏覽的應用程式，建議將壓克力套用到包含瀏覽元素的次要窗格上。

![使用單一垂直壓克力窗格的應用程式模式](images/acrylic_app-pattern_vertical.png)

[NavigationView](../controls-and-patterns/navigationview.md) 是用於將瀏覽功能新增至應用程式的新通用控制項，其視覺設計中包含壓克力。 NavigationView 的窗格開啟時若與主要內容並排，便會顯示背景壓克力，若是以重疊方式開啟的，便會自動轉換成應用程式內壓克力。

如果您的應用程式無法利用 NavigationView，且您打算自行新增壓克力，建議您使用色調不透明度為 60% 的相對透明壓克力。
 - 當窗格以重疊於其他應用程式內容之上的方式開啟時，則應為 [60% 應用程式內壓克力](#acrylic-theme-resources)
 - 當窗格以與主要應用程式內容並排的方式開啟時，則應為 [60% 背景壓克力](#acrylic-theme-resources)

### <a name="multiple-acrylic-panes"></a>多壓克力窗格

對於有三個不同垂直窗格的應用程式，建議將壓克力新增至非主要內容。
 - 對於最靠近主要內容的第二窗格，使用 [80% 背景壓克力](#acrylic-theme-resources)
 - 對於離主要內容稍遠的第三窗格，使用 [60% 背景壓克力](#acrylic-theme-resources)

![使用兩個垂直壓克力窗格的應用程式模式](images/acrylic_app-pattern_double-vertical.png)

### <a name="horizontal-acrylic-pane"></a>水平壓克力窗格

對於採用水平瀏覽、命令或其他橫跨應用程式頂端之強式水平元素的應用程式，建議將 [70% 壓克力](#acrylic-theme-resources)套用到此視覺元素上。

![使用水平壓克力窗格的應用程式模式](images/acrylic_app-pattern_horizontal.png)

強調內容連續、可縮放的畫布應用程式，應在頂端列中使用應用程式內壓克力，讓使用者能夠與此內容連接。 例如地圖、繪畫與繪圖軟體等，皆為畫布應用程式。

對於沒有單一連續性畫布的應用程式，建議使用背景壓克力將使用者與其整體桌面環境連接。

### <a name="acrylic-in-utility-apps"></a>在公用應用程式中的壓克力

Widget 或輕量應用程式可在其應用程式視窗內部的邊緣到邊緣繪製壓克力以增加做為公用應用程式的氣勢。 屬於這個類別的應用程式，其使用者持續使用的時間通常很短暫，不大可能佔據使用者的整個桌面畫面。 比如 [小算盤] 和 [控制中心] 便是。

![以壓克力做為其整個背景的 [小算盤] 公用應用程式](images/acrylic_app-pattern_full.png)

> [!Note]
> 呈現壓克力表面會大量佔用 GPU，可能讓裝置耗電量增加並縮短電池使用時間。 當電池進入省電模式時，壓克力效果會自動停用，使用者也可以選擇停用所有應用程式的壓克力效果。


## <a name="acrylic-blend-types"></a>壓克力混合類型
壓克力最明顯的特徵就是透明度。 有兩種壓克力類型能改變可穿透此材質顯示的項目：
 - **背景壓克力**會顯示在目前使用中應用程式背後的桌面底色圖案及其他視窗，以增加應用程式視窗之間的深度，並呈現使用者個人化的喜好設定。
 - **應用程式內壓克力**可增加應用程式框架內的深度感，讓應用程式能夠聚焦並有層次。

 ![背景壓克力](images/BackgroundAcrylic_DarkTheme.png)

 ![應用程式內壓克力](images/AppAcrylic_DarkTheme.png)

 將多壓克力表面分層時應小心謹慎。 背景壓克力如其名所示，在圖層順序上不應最靠近使用者。 多層的背景壓克力容易產生非預期的視覺錯覺，也應該避免使用。 如果您選擇將壓克力分層，請使用應用程式內壓克力進行，並考慮讓壓克力的色調值變淡，以幫助讓圖層在視覺上更往前靠近檢視者。


## <a name="usability-and-adaptability"></a>可用性與適應性
壓克力會自動針對各種不同的裝置與內容調適其外觀。

在高對比模式中，使用者會繼續看見其所選熟悉的背景色彩取代壓克力。 此外，背景壓克力與應用程式內壓克力在下列狀況下會顯示成單色
 - 當使用者在 \[個人化\] 設定中關閉透明度時
 - 當省電模式啟動時
 - 當應用程式在低階硬體上執行時

此外，在下列狀況下只有背景壓克力將以單色取代其透明度與紋理
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
        <th align="center">[回復色彩](color.md)</th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80% </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> **建議的使用方式︰**這些是以各種不同使用方式皆能正常運作的一般用途壓克力資源。 如果您的應用程式使用色彩為 AltMedium 且文字大小小於 18px 的次要文字，請將 80% 壓克力資源放置在文字之後，以[符合對比率需求](../accessibility/accessible-text-requirements.md)。 </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70% </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> **建議的使用方式︰**如果您的應用程式使用色彩為 AltMedium 且文字大小為 18px 或以上的次要文字，您可以將這些透明度大於 70% 的壓克力資源放在文字後面。 建議在應用程式的頂端水平瀏覽與命令區中使用這些資源。  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60% </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> **建議的使用方式︰**僅將色彩為 AltHigh 主要文字放置在壓克力之上時，您的應用程式可以利用 60% 的資源。 建議以 60% 壓克力繪製您應用程式的[垂直瀏覽窗格](../controls-and-patterns/navigationview.md)，亦即漢堡式功能表。 </td>
    </tr>
</table>

除了非色彩相關壓克力外，我們也已新增會以使用者指定的輔色將壓克力上色的資源。 我們建議謹慎使用彩色的壓克力。 對於提供的 dark1 與 dark2 變化，將與深色佈景主題文字一致的白色或淺色文字放置在這些資源上。
<table>
    <tr>
        <th align="center">資源索引鍵</th>
        <th align="center">色調不透明度</th>
        <th align="center">[色調與回復色彩](color.md)</th>
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
 - **TintOpacity**︰色調層不透明度。 建議一開始設定 80% 不透明度，儘管其他透明度的不同色彩可能看起來更具吸引力。
 - **BackgroundSource**︰此旗標可指定您要背景壓克力或應用程式內壓克力。
 - **FallbackColor**︰在低電力模式中取代表壓克力的單色。 對於背景壓克力，當您的應用程式不是使用中桌面視窗，或當應用程式在手機與 Xbox 上執行時，回復色彩也會取代壓克力。


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

若要讓您的應用程式視窗呈現順暢的外觀，您可以在標題列區域中使用壓克力風格。 此範例透過設定 [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) 物件的 [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar#Windows_UI_ViewManagement_ApplicationViewTitleBar_ButtonBackgroundColor) 以及 [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar#Windows_UI_ViewManagement_ApplicationViewTitleBar_ButtonInactiveBackgroundColor) 屬性為 [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors#Windows_UI_Colors_Transparent)，將壓克力風格延伸到標題列。 

```csharp
/// Extend acrylic into the title bar. 
private void extendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

您可以將這段程式碼放在應用程式的 [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 方法 (_App.xaml.cs_) 中，對 [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window#Windows_UI_Xaml_Window_Activate) 的呼叫之後 (如這裡所示)，或放在應用程式的第一個頁面中。 


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
        extendAcrylicIntoTitleBar();
    }
}
```

此外，您必須使用 `CaptionTextBlockStyle` 來繪製採用 TextBlock 的應用程式標題，此標題通常會自動顯示在標題列中。 如需詳細資訊，請參閱[標題列自訂](../shell/title-bar.md)。

## <a name="dos-and-donts"></a>可行與禁止注意事項
* 請使用壓克力作為非主要應用程式表面 (如瀏覽窗格) 的背景材質。
* 請將壓克力延伸至應用程式的至少一個邊緣，藉此與應用程式背景巧妙地混合以呈現無縫的效果。
* 請勿將應用程式內壓克力與背景壓克力直接相鄰放置，以避免在接縫處產生視覺壓力。
* 請勿將多個色調與不透明度相同的壓克力窗格彼此相鄰，因為這會產生不想要讓人看到的接縫。
* 請勿將輔色文字放置在壓克力表面。

## <a name="how-we-designed-acrylic"></a>我們如何設計壓克力

我們將壓克力的重要元件微調至讓壓克力有獨特的外觀與屬性。 我們從透明度、模糊與雜訊開始設定，以增加平面表面的深度與大小。 我們新增了排除混合模式層，以確保放置在壓克力背景上的對比與可讀性。 最後，我們加入了色彩色調讓壓克力個人化。 這些圖層最後一齊產生全新、可使用的材質。

![壓克力配方](images/AcrylicRecipe_Diagram.png)
<br/>壓克力配方：背景、排除混合、色彩/色調重疊、雜訊

<!--
<div class="microsoft-internal-note">
When designing your app, please utilize these [design resources](http://uni/DesignDepot.FrontEnd/#/Search?t=Resources%7CNeon%7CToolkit&f=Acrylic%20Material) to show acrylic in comps. The linked templates are the most accurate way to represent acrylic material in Photoshop and Illustrator. The ordering, as noted in the recipe diagram above, should start from the top: <br/>
 - Noise asset (tiled) at 2% opacity <br/>
 - Base color/tint/alpha layer <br/>
 - Exclusion blend (white @ 10% opacity) <br/>
 - Gaussian blur (30px radius) <br/>
</div>
-->

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

[**顯色顯目提示**](reveal.md)
