---
author: mijacobs
Description: 色彩可提供 app 的各種資訊層級提供直覺式尋找路徑方法，並做為強化互動模型的重要工具。
title: 色彩
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad

label: Color
template: detail.hbs
extraBodyClass: style-color
---

# 色彩

色彩可提供 app 的各種資訊層級提供直覺式尋找路徑方法，並做為強化互動模型的重要工具。

在 Windows 中，色彩也可以個人化。 使用者可以選擇在他們的體驗中要反映的色彩和淺色或深色的佈景主題。

## 輔色

使用者可以從 [設定] &gt; [個人化] &gt; [色彩]** 挑選稱為輔色的單一色彩。 他們可以從 48 種色樣規劃中選擇 (除了擁有 21 種 TV-safe 色彩調色盤的 Xbox 以外)。

<!-- Alternate version for the dev center. Need to add hex values. -->
![預設輔色](images/accentcolorswatch.png) 預設輔色

![Xbox 輔色](images/accentcolorswatch_xbox.png) Xbox 輔色



當使用者選擇輔色時，它會顯示為其系統佈景主題的一部分。 受影響的區域是開始畫面、工作列、視窗、選取的互動狀態，以及[通用控制項](https://dev.windows.com/design/controls-patterns)內的超連結。 每個 app 都可透過其印刷樣式、背景及互動進一步併入輔色，或覆寫它以保留其特定品牌。

## 色彩上的色彩

一旦選取輔色之後，該輔色的淺色和深色色調都會根據色彩亮度的 HSB 值來建立。 App 可以使用色調變化來建立視覺階層，並提供互動的指示。

根據預設，超連結會使用使用者的輔色。 如果頁面背景是類似的色彩，您可以選擇為超連結指派較淺的 (或較深的) 輔色漸層，以獲得較佳的對比。

<figure class="figure-img" >
    <img src="images/shades.png" alt="A single accent color with its 6 shades"  />
        <figcaption><p>預設輔色的各種淺色/深色漸層。</p>
</figcaption>
</figure>

<figure class="figure-img" >
    <img src="images/action_center_redline_zoom.png" alt="Redlines for Colored Action Center"  />
        <figcaption><p>色彩邏輯如何套用到設計規格的範例。</p>
</figcaption>
</figure>

<aside class="aside-dev">
    <div class="aside-dev-title">
    </div>
    <div class="aside-dev-content">
            在 XAML 中，主要輔色公開為名稱為`SystemAccentColor` 的 [佈景主題資源](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx)。 這些漸層可用來做為 `SystemAccentColorLight3`、`SystemAccentColorLight2`、`SystemAccentColorLight1`、`SystemAccentColorDark1`、`SystemAccentColorDark2` 以及 `SystemAccentColorDark3`。 也可以透過 [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) 與 [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) 列舉，以程式設計的方式提供使用。
    </div>
</aside>

## 色彩佈景主題

使用者也可以在系統的淺色或深色佈景主題之間選擇。 有些 app 選擇根據使用者的喜好設定變更其佈景主題，而其他則會退出。

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
            每個色彩都能夠以遵循 `System*Color` 命名慣例 (例如：`SystemChromeHighColor`) 的 XAML [佈景主題資源](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx#the_xaml_color_ramp_and_theme-dependent_brushes)方式提供使用。 您可以透過 [Application.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.requestedtheme.aspx) 或 [FrameworkElement.RequestedTheme](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.requestedtheme.aspx) 控制您 app 的佈景主題。
    </div>
</aside>

## 協助工具

針對螢幕的使用方式，將調色盤最佳化。 建議您針對背景維持 4.5:1 的文字對比率，以獲得最佳可讀性。 有許多免費工具可用來測試您的顏色是否通過，例如[對比率](http://leaverou.github.io/contrast-ratio/)。


<!--HONumber=May16_HO2-->


