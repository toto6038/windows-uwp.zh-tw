---
title: WinUI 2.4 版本資訊
description: WinUI 2.4 的版本資訊，包括新功能和錯誤修正。
ms.date: 07/15/2020
ms.topic: reference
ms.openlocfilehash: 22fd028ba2059a092ee2f2be47a114fb2d618ce1
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492823"
---
# <a name="windows-ui-library-24"></a>Windows UI 程式庫 2.4

WinUI 2.4 是 Windows UI 程式庫 (WinUI) 的最新官方版本。

WinUI 是開放原始碼專案，裝載於 GitHub 上的 [Windows UI 程式庫存放庫](https://aka.ms/winui)。 請在此存放庫中註冊所有錯誤報告、功能要求和社群程式碼參與。

WinUI 版本：[GitHub 版本頁面](https://github.com/microsoft/microsoft-ui-xaml/releases)

您可以透過 NuGet 套件管理員，將 WinUI 套件新增至 Visual Studio 專案。 如需詳細資訊，請參閱[開始使用 Windows UI 程式庫](../getting-started.md)。

NuGet 套件下載：[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新功能

### <a name="radialgradientbrush"></a>RadialGradientBrush

RadialGradientBrush 會在 Center、RadiusX 和 RadiusY 屬性所定義的橢圓形內繪製。 漸層的色彩會從橢圓形的中央開始，並在半徑上結束。

![放射狀漸層筆刷](../images/radialgradientbrush.gif)<br>
*放射狀漸層筆刷*

[使用方針](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[API 參考](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

ProgressRing 控制項是用於強制回應的互動，直到 ProgressRing 消失之前，使用者都會被封鎖。 如果作業需要暫停與應用程式的大部分互動，直到作業完成為止，請使用此控制項。

![ProgressRing 控制項](../images/progressring.gif)<br>
*ProgressRing 控制項*

[使用方針](/windows/uwp/design/controls-and-patterns/progress-controls)

[API 參考](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>TabView 更新

TabView 控制項更新可讓您更充分掌控索引標籤的轉譯方式。

您可以設定未選取索引標籤的寬度，只顯示一個圖示來節省螢幕空間：

![TabView 控制項索引標籤大小](..\images\tabview-sizing.gif)<br>
*TabView 控制項索引標籤大小*

您也可以隱藏未選取索引標籤上的關閉按鈕，直到使用者將滑鼠停留在索引標籤上 (在先前的版本中一律會顯示)：

![TabView 控制項暫留以關閉](..\images\tabview-closebuttononhover.gif)<br>
*TabView 控制項暫留以關閉*

[使用方針](/windows/uwp/design/controls-and-patterns/tab-view)

[API 參考](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>深色佈景主題更新為 TextBox 系列控制項

啟用深色佈景主題時，TextBox 系列控制項的背景色彩在文字插入時預設保持深色 (在舊版中，背景色彩會在文字插入期間變更為白色)。

| 之前 | 之後 |
| - | - |
| ![TextBox 深色佈景主題更新 (之前)](..\images\textbox-darkthemeupdates-before1.gif)<br>*TextBox 深色佈景主題更新 (之前)* | ![TextBox 深色佈景主題更新 (之後)](..\images\textbox-darkthemeupdates-after1.gif)<br>*TextBox 深色佈景主題更新 (之後)* |
| ![TextBox 深色佈景主題更新 (之前)](..\images\textbox-darkthemeupdates-before2.gif)<br>*TextBox 深色佈景主題更新 (之前)* | ![TextBox 深色佈景主題更新 (之後)](..\images\textbox-darkthemeupdates-after2.gif)<br>*TextBox 深色佈景主題更新 (之後)* |

以下是 TextBox 系列控制項所包含的一些控制項：

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [可編輯的 ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>階層式瀏覽

[NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4) 控制項現在支援階層式瀏覽，包括 Left、Top 和 LeftCompact 顯示模式。 階層式 NavigationView 適用於顯示頁面類別、識別具有相關子頁面的頁面，或是在有中樞樣式頁面連結到其他許多頁面的應用程式內使用。

![階層式 NavigationView 控制項](..\images\HierarchicalNavView.gif)<br>*階層式 NavigationView 控制項*

[使用方針](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[API 參考](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>範例

**XAML 控制項庫**提供上述每個 WinUI 2.4 功能的範例。

如果您尚未安裝 XAML 控制項庫應用程式，請從 [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 取得。

您也可以在 [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) 上檢視 XAML 控制項庫原始程式碼。
