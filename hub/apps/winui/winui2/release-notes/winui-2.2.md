---
title: WinUI 2.2 版本資訊
description: WinUI 2.2 的版本資訊，包括新功能和錯誤修正。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 603c2c439e47c82695c8bdf5cd16fceebc9bc068
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762937"
---
# <a name="windows-ui-library-22"></a>Windows UI 程式庫 2.2

WinUI 2.2 是 Windows UI 程式庫的最新官方版本。

您可以使用 NuGet 套件管理員，在應用程式中新增 WinUI 套件：如需詳細資訊，請參閱[開始使用 Windows UI 程式庫](../getting-started.md)。

WinUI 是裝載於 GitHub 上的開放原始碼專案。 我們歡迎您在 [Windows UI 程式庫存放庫](https://aka.ms/winui)中提出錯誤回報、功能要求和社群程式碼。

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 版本歷程記錄

### <a name="windows-ui-library-22-official-release"></a>Windows UI 程式庫 2.2 官方版本

2019 年 8 月

[GitHub 版本頁面](https://github.com/microsoft/microsoft-ui-xaml/releases)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>新功能

#### <a name="tabview"></a>TabView

![顯示索引標籤檢視控制項行為的短片。](../images/tabview-gif.gif)

#### <a name="description"></a>說明

TabView 控制項是索引標籤的集合，每個索引標籤各自代表應用程式中的一個新頁面或文件。 當應用程式有數頁內容，而且使用者希望能夠新增、關閉和重新排列索引標籤時，TabView 就很有用。 新的 [Windows 終端機](https://github.com/Microsoft/Terminal)會使用 TabView 來顯示多個命令列介面。

#### <a name="documentation"></a>文件

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>NavigationView 更新

##### <a name="a-navigationviews-back-button-update"></a>a) NavigationView 的 [上一頁] 按鈕更新

![顯示更新後的瀏覽檢視控制項 [上一頁] 按鈕行為的短片。](../images/navigationview-back-button.gif)

##### <a name="description"></a>說明

在 NavigationView 的最小模式中，[上一頁] 按鈕不會再消失。 使用者在開啟和關閉窗格時，不再需要移動其游標來按一下漢堡按鈕。 這項功能預設為運作狀態。 您不需要變更任何程式碼即可讓此功能運作。

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView - 無自動填補

![螢幕擷取畫面：顯示具有「無自動填補」的瀏覽檢視控制項行為。](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>說明

應用程式開發人員現在可以在使用 NavigationView 控制項時回收其應用程式視窗內的所有像素，並將其延伸到標題列區域。

##### <a name="documentation"></a>文件

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>視覺化樣式更新

##### <a name="a-corner-radius-update"></a>a) 圓角半徑更新

![螢幕擷取畫面：顯示更新後的圓角半徑樣式。](../images/corner-radius.png)

##### <a name="description"></a>說明

已新增 CornerRadius 屬性。 預設控制項已更新為使用微圓角。 開發人員可以輕鬆地自訂圓角半徑，在需要時讓應用程式具有獨特的外觀。

##### <a name="github-spec-link"></a>GitHub 規格連結

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) 框線粗細更新

![螢幕擷取畫面：顯示更新後的框線粗細樣式。](../images/border-thickness.png)

##### <a name="description"></a>說明

BorderThickness 屬性變得更容易自訂。 預設控制項已更新，可讓外框變得更細以獲得簡潔熟悉的外觀。

##### <a name="github-spec-link"></a>GitHub 規格連結

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) 按鈕視覺效果更新

![螢幕擷取畫面：顯示更新後的按鈕控制項樣式。](../images/button-hover-visual-update.png)

##### <a name="description"></a>描述： 
預設按鈕的視覺效果已更新，移除了會在滑鼠停留時出現的外框，以使預設按鈕的外觀更為簡潔。

##### <a name="github-spec-link"></a>GitHub 規格連結：  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>d) SplitButton 視覺效果更新

![螢幕擷取畫面：顯示更新後的分割按鈕控制項樣式。](../images/splitbutton-visual-update.png)

##### <a name="description"></a>描述： 
預設 SplitButton 的視覺效果已更新，使其更有別於 DropDownButton。

##### <a name="github-spec-link"></a>GitHub 規格連結： 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) ToggleSwitch 視覺效果更新

![螢幕擷取畫面：顯示更新後的切換開關控制項樣式。](../images/toggleswitch-update.png)

##### <a name="description"></a>描述： 
預設 ToggleSwitch 的寬度已從 44 px 縮減為 40 px，以使其外觀更為平衡，同時保有可用性。

##### <a name="github-spec-link"></a>GitHub 規格連結： 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) 核取方塊和選項按鈕視覺效果更新

![螢幕擷取畫面：顯示更新後的核取方塊和選項按鈕控制項樣式](../images/checkbox-radiobutton.png)

##### <a name="description"></a>描述： 
核取方塊和選項按鈕視覺效果已更新，以便與其餘的視覺化樣式變更保持一致。

##### <a name="github-spec-link"></a>GitHub 規格連結： 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>範例

Xaml 控制項庫範例應用程式包含 WinUI 控制項用法的互動式示範和範例程式碼。

* 從 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安裝 XAML 控制項庫應用程式

* Xaml 控制項庫也是 [GitHub 上的開放原始碼項目](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文件

Windows UI 程式庫控制項的操作說明文章隨附於[通用 Windows 平台控制項文件](/windows/uwp/design/controls-and-patterns/)。

API 參照文件位於此處：[Windows UI 程式庫 API](/uwp/api/overview/winui/)。

## <a name="microsoftuixaml-22-prerelease-version-history"></a>Microsoft.UI.Xaml 2.2-prerelease 版本歷程記錄

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-prerelease

2019 年 7 月

[GitHub 版本頁面](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>實驗性功能

* [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-prerelease

2019 年 4 月

[GitHub 版本頁面](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>實驗性功能

* [FlowLayout](/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](/uwp/api/microsoft.ui.xaml.controls.scrollviewer)