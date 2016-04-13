---
Description: 色彩可提供 app 的各種資訊層級提供直覺式尋找路徑方法，並做為強化互動模型的重要工具。
title: 色彩
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
label: Color
template: detail.hbs
extraBodyClass: style-color
brief: Color provides intuitive wayfinding through an app's various levels of information and serves as a crucial tool for reinforcing the interaction model.<br /><br />In Windows, color is also personal. Users can choose a color and a light or dark theme to be reflected throughout their experience.
---

# 適用於 UWP App 的色彩
色彩可提供 app 的各種資訊層級提供直覺式尋找路徑方法，並做為強化互動模型的重要工具。

## 輔色

使用者可以挑選名為輔色的單一色彩。 他們必須從一組經過挑選的 48 個色彩樣本中進行選擇。


<!-- Alternate version for the dev center. Need to add hex values. -->
<figure>
![Accent colors](images/accentcolorswatch.png)
<figcaption>根據經驗法則，使用原始輔色做為背景時，一律在其上方放入白色文字。</figcaption>
</figure>

當使用者選擇輔色時，它會顯示為其系統佈景主題的一部分。 受影響的區域是開始畫面、工作列、視窗、選取的互動狀態，以及[通用控制項](https://dev.windows.com/design/controls-patterns)內的超連結。 每個 app 都可透過其印刷樣式、背景及互動進一步併入輔色，或覆寫它以保留其特定品牌。

## 色彩選取

一旦選取輔色之後，該輔色的淺色和深色色調都會根據色彩亮度的 HCL 值來建立。 App 可以使用色調變化來建立視覺階層，並提供互動的指示。

![含有 6 個色調的單一輔色](images/shades.png)

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            In XAML, the accent color is exposed as a [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx) named `SystemAccentColor`. It's also available programmatically from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx). You can programmatically access the different shades from [UISettings.GetColorValue](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx), see the [UIColorType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) enum.
    </div>
</aside>

## 色彩佈景主題

使用者也可以在系統的淺色或深色佈景主題之間進行選擇 (在手機上，但平板電腦和桌上型電腦尚未提供該選項 - 它們可以提供 App 內設定)。 有些 app 選擇根據使用者的喜好設定變更其佈景主題，而其他則會退出。

使用淺色佈景主題的 app 適用於有關生產力 app 的案例。 範例是搭配 Microsoft Office 的可用 app 套件。 淺色佈景主題讓使用者能輕鬆閱讀冗長文字，以及長時間處理工作。

深色佈景主題讓 app 的內容對比更顯著，而這類 app 是可向使用者顯示大量視訊或影像的媒體中心或案例。 在這些案例中，閱讀不一定是主要工作，有可能是電影觀賞體驗，也可能是在周遭環境光線不足的情況下顯示。

如果您的 app 與這些描述中任何一種都不相符，請考慮使用下列系統佈景主題，讓使用者決定最適合他們的佈景主題。

為了讓佈景主題的設計更容易，Windows 會提供會自動調整為佈景主題的其他調色盤。


<!-- OP version -->
### 淺色佈景主題
#### 基本
![基本淺色佈景主題](images/themes-light-base.png)
#### Alt
![Alt 淺色佈景主題](images/themes-light-alt.png)
#### 清單
![清單淺色佈景主題](images/themes-light-list.png)
#### Chrome
![Chrome 淺色佈景主題](images/themes-light-chrome.png)
### 深色佈景主題
#### 基本
![基本深色佈景主題](images/themes-dark-base.png)
#### Alt
![Alt 深色佈景主題](images/themes-dark-alt.png)
#### 清單
![清單深色佈景主題](images/themes-dark-list.png)
#### Chrome
![Chrome 深色佈景主題](images/themes-dark-chrome.png)


<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            Each color is available as a XAML [theme resource](https://msdn.microsoft.com/en-us/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes) that follows the `System*Color` naming convention (ex: `SystemChromeHighColor`). You can control your app's theme through either [Application.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) or [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx).
    </div>
</aside>

## 協助工具

針對螢幕的使用方式，將調色盤最佳化。 我們建議您維持最基本的文字對比率 4.5:1，以獲得最佳的可讀性。


<!--HONumber=Mar16_HO5-->


