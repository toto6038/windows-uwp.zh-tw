---
title: WinUI 2.1 版本資訊
description: WinUI 2.1 的版本資訊，包括新功能和錯誤修正。
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: eb9d01c49dffd8017b2a70557f9408ceb758ae1e
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96603855"
---
# <a name="windows-ui-library-21"></a>Windows UI 程式庫 2.1

Windows UI 程式庫的第一個開放原始碼版本 – WinUI 2.1 (於 2019 年 4 月發行)。

WinUI 提供了許多最新的 Windows UX 平台功能，包括最新的 Fluent 控制項和樣式，其可供您立即使用，並與舊版的 Windows 10 年度更新版 (14393) 相容。 [XAML 控制項庫](/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery)提供了一些範例，以供您探索程式庫內新增的所有酷炫新功能。

下載 [WinUI 2.1 NuGet 套件](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

您可以使用 NuGet 套件管理員，以選擇在應用程式中使用 WinUI 套件：如需詳細資訊，請參閱[開始使用 Windows UI 程式庫](/uwp/toolkits/winui/getting-started)。

WinUI 是裝載於 GitHub 上的開放原始碼專案。 我們歡迎您在 [Windows UI 程式庫存放庫](https://aka.ms/winui)中提出錯誤回報、功能要求和社群程式碼。

## <a name="whats-new-in-this-release"></a>此版本的新功能

### <a name="itemsrepeater"></a>ItemsRepeater

使用 ItemsRepeater，即可使用靈活的版面配置系統、自訂檢視畫面、進行模擬，藉以建立自訂的集合體驗。
與 ListView 不同，ItemsRepeater 並未提供全面的使用者體驗，無預設的 UI，也未提供焦點、選取或使用者互動的相關原則。 然而，其為建置組塊，可供您用來建立自己的唯一集合型體驗和自訂控制項。 其支援建置更豐富且更具效能的體驗。

![顯示項目重複器控制項行為的短片。](../images/ItemsRepeater%20-%20MSN%20News.gif)

[文件](/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

AnimatedVisualPlayer 會裝載並控制動畫視覺效果的播放，讓您可以將高效能的自訂動作圖形新增至應用程式。 例如，AnimatedVisualPlayer 可用來顯示和控制 Lottie 動畫。

![顯示動畫視覺播放器控制項行為的短片。](../images/AnimatedVisualPlayerUpdated.gif)

[文件](/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

TeachingTip 提供了吸引人的 Fluent 方式，讓應用程式能夠使用非侵入式且內容豐富的秘訣來引導和通知使用者。 TeachingTip 可以將焦點帶到新的或重要的功能，告訴使用者如何執行工作，以及如何藉由為您手邊的工作提供內容相關的資訊來增強工作流程。

![顯示教學提示控制項行為的短片。](../images/TeachingTipUpdated.gif)

[文件](/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

包含可在 MenuBar 中使用「選項按鈕」樣式選項的能力。 這可讓具有項目符號且繫結在一起的選項形成群組，例如選項按鈕群組。 系統會為開發人員處理此邏輯。

![顯示廣播功能表飛出視窗項目控制項行為的螢幕擷取畫面。](../images/RadioMenuFlyoutItem1.png)

[文件](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

精簡模式可讓開發人員為任意數目的案例建立舒適的體驗。 只要新增資源字典，您的應用程式就可以適應平均 ~33% 以上的 UI。

![顯示精簡密度控制項行為的螢幕擷取畫面。](../images/CompactDensityUpdated.png)

[文件](/windows/uwp/design/style/spacing)

### <a name="shadows"></a>陰影

![範例](../images/shadow.gif)

在 UI 中建立元素的視覺階層，可讓 UI 輕鬆掃描並傳達應該聚焦的重要事項。 系統往往會使用提高權限 (將 UI 的精選元素帶出的動作) 的方式在軟體中實現這類階層。 

在 Windows 10 2019 年 5 月更新中，許多常見的控制項預設會使用 z 深度和陰影來新增提高權限的能力。 WinUI 2.1 中的 NavigationView 和 TeachingTip 控制項在具有 Windows 10 2019 年 5 月更新的 OS 上執行時，也會有預設陰影。 在 Windows 10 2019 年 5 月更新發行之後，便會提供具有預設陰影的控制項完整清單，以及要如何使用其他 API 的方法，並在此公佈連結。

## <a name="examples"></a>範例

Xaml 控制項庫範例應用程式包含 WinUI 控制項用法的互動式示範和範例程式碼。

* 從 [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) 安裝 XAML 控制項庫應用程式

* Xaml 控制項庫也是 [GitHub 上的開放原始碼項目](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>文件

Windows UI 程式庫控制項的操作說明文章隨附於[通用 Windows 平台控制項文件](/windows/uwp/design/controls-and-patterns/)。

API 參照文件位於此處：[Windows UI 程式庫 API](/windows/winui/api/)。

## <a name="microsoftuixaml-21-version-history"></a>Microsoft.UI.Xaml 2.1 版本歷程記錄

### <a name="microsoftuixaml-21-official-release"></a>Microsoft.UI.Xaml 2.1 官方版本

2019 年 4 月

[GitHub 版本頁面](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>新功能 (未包含在舊版發行前版本中)

* [CompactDensity](/windows/uwp/design/style/spacing)：精簡模式可讓開發人員為任意數目的案例建立舒適的體驗。 只要新增資源字典，您的應用程式就可以適應平均 ~33% 以上的 UI。

* 陰影：在 UI 中建立元素的視覺階層，可讓 UI 輕鬆掃描並傳達應該聚焦的重要事項。 系統往往會使用提高權限 (將 UI 的精選元素帶出的動作) 的方式在軟體中實現這類階層。 許多常見的控制項預設會使用 z 深度和陰影來新增提高權限的能力。  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-prerelease

2019 年 2 月

[GitHub 版本頁面](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

新的實驗性功能：

* [TeachingTip 控制項](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  這個新的控制項提供了一個方法，讓您的應用程式可以使用非侵入式和內容豐富的通知來引導和通知應用程式中的使用者。 TeachingTip 可用於將焦點帶到新的或重要的功能、教使用者如何執行工作，或是藉由為其手邊的工作提供內容相關的資訊，來增強使用者工作流程。

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-prerelease

2019 年 2 月

[GitHub 版本頁面](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

新的實驗性功能：

* [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  這個新的控制項可讓您播放複雜的高效能向量動畫，包括使用 [Lottie-Windows](/windows/communitytoolkit/animations/lottie) 所建立的 [Lottie](https://github.com/airbnb/lottie) 動畫。

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-prerelease

2018 年 12 月

[GitHub 版本頁面](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[NuGet 套件下載](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

新的實驗性功能：

* [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)