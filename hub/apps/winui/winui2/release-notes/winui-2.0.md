---
title: WinUI 2.0 版本資訊
description: WinUI 2.0 的版本資訊。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 8518f528c67e11b5ac895bb4e000751993fa1a91
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493593"
---
# <a name="windows-ui-library-20"></a>Windows UI 程式庫 2.0

WinUI 2.0 是 Windows UI 程式庫的第一個公開發行版本。

WinUI 可讓您以最簡單的方式為 Windows 打造絕佳 Fluent Design 體驗。

其包含兩個 NuGet 套件：

* [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)：適用於 UWP 應用程式的控制項和 Fluent Design。 這是主要的 WinUI 套件。

* [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct)：用於中介軟體元件的低層級 API。

您可以使用 NuGet 套件管理員，在您的應用程式中下載及使用 WinUI 套件：如需詳細資訊，請參閱[開始使用 Windows UI 程式庫](https://docs.microsoft.com/uwp/toolkits/winui/getting-started)。

WinUI 是裝載於 GitHub 上的開放原始碼專案。 我們歡迎您在 [Windows UI 程式庫存放庫](https://aka.ms/winui)中提出錯誤回報、功能要求和社群程式碼。

## <a name="microsoftuixaml-20181011001"></a>Microsoft.UI.Xaml 2.0.181011001

2018 年 10 月

這是 [Microsoft.UI.Xaml NuGet 套件](https://www.nuget.org/packages/Microsoft.UI.Xaml)的第一個發行版本。 其包含適用於 Windows UWP 應用程式的官方原生 Fluent 控制項和功能。

### <a name="new-features"></a>新功能

此版本中的控制項和模式包括：

| 功能 | 說明 |
| --- | --- |
|[AcrylicBrush]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.acrylicbrush)| 繪製一個使用多種效果的半透明材質區域，這些效果包括模糊和雜訊紋理。|
|[BitmapIconSource]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.bitmapiconsource)| 代表以點陣圖作為其內容的圖示來源。|
|[ColorPicker]( https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.colorpicker)| 代表可讓使用者使用色彩頻譜、滑杆和文字輸入來挑選色彩的控制項。|
|[CommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.commandbarflyout)|代表專門的飛出視窗，可為 AppBarButton 和相關命令元素提供版面配置。|
|[DropDownButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.dropdownbutton)|代表一個按鈕，其具有＞形箭號可用來開啟功能表。|
|[FontIconSource ](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.fonticonsource)|代表所使用的字符來自指定字型的圖示來源。|
|[MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)|代表專門的容器，其會在水平列中顯示一組功能表，一般會出現在應用程式視窗的頂端。|
|[MenuBarItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubaritem)|代表 MenuBar 控制項中的最上層功能表。|
|[NavigationView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.navigationview)|代表可供瀏覽應用程式內容的容器。 其具有一個標頭、一個主要內容檢視，以及一個導覽命令功能表窗格。|
|[ParallaxView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.parallaxview)|代表一個容器，此容器會將前景元素 (例如清單) 的捲動位置，繫結至背景元素 (例如影像)。 當您捲動前景元素時，背景元素會有動畫效果，以產生視差效果。|
|[PersonPicture](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.personpicture)|代表一個控制項，此控制項會顯示個人的虛擬人偶影像 (如果有的話)。要是沒有，則顯示個人的縮寫名或一般字符。|
|[RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)|代表可讓使用者輸入星級評等的控制項。|
|[RefreshContainer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshcontainer)|代表一個容器控制項，此控制項會為可捲動的內容提供 RefreshVisualizer 和「拖動以重新整理」功能。|
|[RefreshVisualizer](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.refreshvisualizer)|代表可為內容重新整理提供動畫狀態指示器的控制項。|
|[RevealBackgroundBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbackgroundbrush)|使用組合筆刷和光線效果，為控制項背景繪製顯色效果。|
|[RevealBorderBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealborderbrush)|使用組合筆刷和光線效果，為控制項框線繪製顯色效果。|
|[RevealBrush](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.media.revealbrush)|筆刷的基底類別，會使用組合效果和光線來實作顯色視覺化設計處理方式。|
|[SplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.splitbutton)|代表一個按鈕，其具有兩個可分別叫用的組件。 一個組件的行為就像標準按鈕，另一個組件則會叫用飛出視窗。|
|[SwipeControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.swipecontrol)|代表一個容器，可透過觸控互動來讓您存取關聯式命令。|
|[SymbolIconSource](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.symboliconsource)|代表一個圖示來源，其使用來自 Segoe MDL2 Assets 的字符作為其內容。|
|[TextCommandBarFlyout](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.textcommandbarflyout)|代表專門的命令列飛出視窗，其中包含用來編輯文字的命令。|
|[ToggleSplitButton](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton)|代表一個按鈕，其具有兩個可分別叫用的組件。 一個組件的行為就像切換按鈕，另一個組件則會叫用飛出視窗。|
|[TreeView](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.treeview)|代表一個階層式清單，其具有內含巢狀項目的展開及摺疊節點。|

## <a name="examples"></a>範例

Xaml 控制項庫範例應用程式包含 WinUI 控制項用法的互動式示範和範例程式碼。

* 從 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安裝 XAML 控制項庫應用程式

* Xaml 控制項庫也是 [GitHub 上的開放原始碼項目](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文件

Windows UI 程式庫控制項的操作說明文章隨附於[通用 Windows 平台控制項文件](/windows/uwp/design/controls-and-patterns/)。

API 參照文件位於此處：[Windows UI 程式庫 API](/uwp/api/overview/winui/)。
