---
author: mijacobs
description: "色彩可提供 app 的各種資訊層級提供直覺式尋找路徑方法，並做為強化互動模型的重要工具。"
title: "色彩"
ms.assetid: 3ba7176f-ac47-498c-80ed-4448edade8ad
template: detail.hbs
extraBodyClass: style-color
translationtype: Human Translation
ms.sourcegitcommit: d7236006f2c620a4ff0de4e0f413f32a2eaf5687
ms.openlocfilehash: 8e253c93f932e04b825478cf0801e4c8c0d43b9d

---

# 色彩

色彩不僅提供一種可尋找 App 各種不同層級資訊的直覺方式，也作為一種強化互動模型的重要工具。

在 Windows 中，色彩也是個人化的。 使用者可以選擇在他們的體驗中要反映的色彩和淺色或深色的佈景主題。

## 輔色

使用者可以從 [設定] &gt; [個人化] &gt; [色彩]** 挑選稱為輔色的單一色彩。 他們可以從 48 種色樣規劃中選擇 (除了擁有 21 種 TV-safe 色彩調色盤的 Xbox 以外)。

<!-- Alternate version for the dev center. Need to add hex values. -->
![預設輔色](images/accentcolorswatch.png) 預設輔色

![Xbox 輔色](images/accentcolorswatch_xbox.png) Xbox 輔色


當使用者選擇輔色時，它會顯示為其系統佈景主題的一部分。 受影響的區域是開始畫面、工作列、視窗、選取的互動狀態，以及[通用控制項](https://dev.windows.com/design/controls-patterns)內的超連結。 每個 App 都可透過其印刷樣式、背景及互動來進一步併入輔色，或覆寫它以保留其特定品牌。

## 調色盤建置組塊

在選取輔色之後，就會根據色彩亮度的 HSB 值來建立該輔色的淺色和深色色調。 App 可以使用色調變化來建立視覺階層，並提供互動的指示。

根據預設，超連結會使用使用者的輔色。 如果頁面背景是類似的色彩，您可以選擇為超連結指派較淺的 (或較深的) 輔色色調，以獲得較佳的對比。

![單一輔色及其 6 種色調](images/shades.png) 預設輔色的各種淺色/深色色調。

![彩色重要訊息中心的充分運用](images/action_center_redline_zoom.png) 色彩邏輯如何套用到設計規格的範例。

**注意**：  在 XAML 中，主要輔色會公開為名為 `SystemAccentColor` 的[佈景主題資源](https://msdn.microsoft.com/library/windows/apps/Mt187274.aspx)。 這些色調會以 `SystemAccentColorLight3`、`SystemAccentColorLight2`、`SystemAccentColorLight1`、`SystemAccentColorDark1`、`SystemAccentColorDark2` 及 `SystemAccentColorDark3` 的形式可供使用。 此外，也可以透過 [UISettings.GetColorValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uisettings.getcolorvalue.aspx) 與 [UIColorType](https://msdn.microsoft.com/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) 列舉，以程式設計的方式提供使用。

## 色彩佈景主題設定

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


## 變更佈景主題

您可以透過變更 App.xaml 中的 **RequestedTheme** 屬性，輕鬆變更佈景主題。

```XAML
<Application
    x:Class="App9.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App9"
    RequestedTheme="Dark">

</Application>
```

如果移除 **RequestedTheme**，即表示您的應用程式將會遵循使用者的 App 模式設定，而他們將能夠選擇以深色或淺色主題來檢視您的 App。 

請確定您在建立 App 時將佈景主題納入考量，因為佈景主題對您 App 的外觀有很大的影響。

## 協助工具

針對螢幕的使用方式，將調色盤最佳化。 建議您針對背景維持 4.5:1 的文字對比率，以獲得最佳可讀性。 有許多免費工具可用來測試您的色彩是否通過，例如[對比率](http://leaverou.github.io/contrast-ratio/)。

## 相關文章

* [XAML 樣式](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)
* [XAML 佈景主題資源](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/xaml-theme-resources)



<!--HONumber=Aug16_HO3-->


