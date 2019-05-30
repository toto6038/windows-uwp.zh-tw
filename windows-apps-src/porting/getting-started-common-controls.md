---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 開始使用常用控制項
title: 開始使用常用控制項
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a64dbd6a9530f81c55b0d4b52e4c0fd55c4b9956
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372831"
---
# <a name="getting-started-common-controls"></a>開始使用：通用控制項


## <a name="common-controls-list"></a>常用控制項清單

在上一個小節中，您只使用了兩個控制項：按鈕和文字區塊。 有，當然，您可以使用許多更多的控制。 以下是一些您會在 app 及其 iOS 對等項目中使用的常用控制項。 這裡依字母順序列出 iOS 控制項，旁邊是最相似的通用 Windows 平台 (UWP) 控制項。

UWP 控制項的好處是它們可以感應正在執行的所在裝置類型，並適當地變更其外觀和功能。 例如，如果您的專案使用 [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) 控制項，它會有足夠的智慧自行最佳化，以便在桌上型電腦與手機 (舉例) 上有不同的外觀和行為。 您不需要執行任何動作：控制項會在執行階段自我調整。

| iOS 控制項 (類別/通訊協定) | 對等的 UWP 控制項 |
|------------------------------|--------------------------------------|
| 活動指示器 (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> 另請參閱[快速入門：新增進度控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| 廣告橫幅檢視 (**ADBannerView**) 和廣告檢視委派 (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> 另請參閱[在您的應用程式中顯示廣告](../monetize/display-ads-in-your-app.md) |
| 按鈕 (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) <br/> 另請參閱[快速入門：加入按鈕控制項](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| 日期選擇器 (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| 影像檢視 (UIImageView) | [影像](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> 另請參閱 [Image 和 ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| 標籤 (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> 另請參閱[快速入門：顯示文字](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| 地圖檢視 (MKMapView) 和地圖檢視委派 (MKMapViewDelegate) | 請參閱[UWP 應用程式的 Bing 地圖服務](https://go.microsoft.com/fwlink/p/?LinkId=263496) |
| 瀏覽控制項 (UINavigationController) 和瀏覽控制項委派 (UINavigationControllerDelegate) | [畫面格](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> 另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 頁面控制項 (UIPageControl) | [頁面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> 另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 選擇器檢視 (UIPickerView) 和選擇器檢視委派 (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> 另請參閱[新增下拉式方塊與清單方塊](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| 進度列 (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> 另請參閱[快速入門：新增進度控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| 捲動檢視 (UIScrollView) 和捲動檢視委派 (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  另請參閱[Extensible Application Markup Language (XAML) 捲動、移動瀏覽和縮放範例](https://go.microsoft.com/fwlink/p/?LinkId=238577) |
| 搜尋列 (UISearchBar) 和搜尋列委派 (UISearchBarDelegate) | 請參閱[將搜尋新增到應用程式](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  另請參閱[快速入門：將搜尋新增至應用程式](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| 分段控制項 (UISegmentedControl) | None |
| 滑桿 (UISlider) | [Slider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  另請參閱[如何新增滑桿](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| 分割檢視控制項 (UISplitViewController) 和分割檢視控制項委派 (UISplitViewControllerDelegate) | None |
| 開關 (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  另請參閱[如何新增切換開關](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| 索引標籤列控制項 (UITabBarController) 和索引標籤列控制項委派 (UITabBarControllerDelegate) | None |
| 表格檢視控制項 (UITableViewController)、表格檢視 (UITableView)、表格檢視委派 (UITableViewDelegate)、表格儲存格 (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  另請參閱[快速入門：新增 ListView 和 GridView 控制項](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| 文字欄位 (UITextField) 和文字欄位委派 (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  另請參閱[顯示和編輯文字](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| 文字檢視 (UITextView) 和文字檢視委派 (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  另請參閱[快速入門：顯示文字](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| 檢視 (UIView) 和檢視控制項 (UIViewController) | [頁面](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| 網頁檢視 (UIWebView) 和網頁檢視委派 (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  另請參閱 [XAML WebView 控制項範例](https://go.microsoft.com/fwlink/p/?LinkId=238582) |
| 視窗 (UIWindow) | [畫面格](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  另請參閱[瀏覽](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |

如果想知道更多控制項，請參閱[控制項清單](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/)。

**附註**  控制項，用於使用 JavaScript 和 HTML 的 UWP 應用程式的清單，請參閱[Controls 清單](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10))。

### <a name="next-step"></a>後續步驟

[開始使用：瀏覽](getting-started-navigation.md)

## <a name="related-topics"></a>相關主題

* [build 2014:XAML UI 以及控制項呢？](https://go.microsoft.com/fwlink/p/?LinkID=397897)
* [build 2014:使用常見的 XAML UI 架構開發應用程式](https://go.microsoft.com/fwlink/p/?LinkID=397898)
* [build 2014:使用 Visual Studio 來建立 XAML 交集應用程式](https://go.microsoft.com/fwlink/p/?LinkID=397876)
