---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: 開始使用常用控制項
title: 開始使用常用控制項
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 05cf78d7dec260b990d2ce71662e3db6eb07d07f
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8482433"
---
# <a name="getting-started-common-controls"></a>開始使用：常用控制項


## <a name="common-controls-list"></a>常用控制項清單

在上一個小節中，您只使用了兩個控制項：按鈕和文字區塊。 有，當然，您可以使用非常多的控制項。 以下是一些您會在 app 及其 iOS 對等項目中使用的常用控制項。 這裡依字母順序列出 iOS 控制項，旁邊是最相似的通用 Windows 平台 (UWP) 控制項。

UWP 控制項的好處是它們可以感應正在執行的所在裝置類型，並適當地變更其外觀和功能。 例如，如果您的專案使用 [**DatePicker**](https://msdn.microsoft.com/library/windows/apps/br211681) 控制項，它會有足夠的智慧自行最佳化，以便在桌上型電腦與手機 (舉例) 上有不同的外觀和行為。 您不需要執行任何動作：控制項會在執行階段自我調整。

| iOS 控制項 (類別/通訊協定) | 對等的 UWP 控制項 |
|------------------------------|--------------------------------------|
| 活動指示器 (**UIActivityIndicatorView**) | [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) <br/> 另請參閱[快速入門：新增進度控制項](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| 廣告橫幅檢視 (**ADBannerView**) 和廣告檢視委派 (**ADBannerViewDelegate**) | [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) <br/> 另請參閱[在您的應用程式中顯示廣告](../monetize/display-ads-in-your-app.md) |
| 按鈕 (UIButton) | [Button](https://msdn.microsoft.com/library/windows/apps/br209265) <br/> 另請參閱[快速入門：新增按鈕控制項](https://msdn.microsoft.com/library/windows/apps/xaml/jj153346) |
| 日期選擇器 (UIDatePicker) | [DatePicker](https://msdn.microsoft.com/library/windows/apps/br211681) |
| 影像檢視 (UIImageView) | [Image](https://msdn.microsoft.com/library/windows/apps/br242752) <br/> 另請參閱 [Image 和 ImageBrush](https://msdn.microsoft.com/library/windows/apps/mt280382) |
| 標籤 (UILabel) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/> 另請參閱[快速入門：顯示文字](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| 地圖檢視 (MKMapView) 和地圖檢視委派 (MKMapViewDelegate) | 請參閱[適用於 UWP app 的 Bing 地圖](http://go.microsoft.com/fwlink/p/?LinkId=263496) |
| 瀏覽控制項 (UINavigationController) 和瀏覽控制項委派 (UINavigationControllerDelegate) | [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/> 另請參閱[瀏覽](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| 頁面控制項 (UIPageControl) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/> 另請參閱[瀏覽](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| 選擇器檢視 (UIPickerView) 和選擇器檢視委派 (UIPickerViewDelegate) | [ComboBox](https://msdn.microsoft.com/library/windows/apps/br209348) <br/> 另請參閱[新增下拉式方塊與清單方塊](https://msdn.microsoft.com/library/windows/apps/xaml/hh780616) |
| 進度列 (UIProgressView) | [ProgressBar](https://msdn.microsoft.com/library/windows/apps/br227529) <br/> 另請參閱[快速入門：新增進度控制項](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651) |
| 捲動檢視 (UIScrollView) 和捲動檢視委派 (UIScrollViewDelegate) | [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/br209527) <br/>  另請參閱[Extensible Application Markup Language (XAML) 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?LinkId=238577) |
| 搜尋列 (UISearchBar) 和搜尋列委派 (UISearchBarDelegate) | 請參閱[將搜尋新增到應用程式](https://msdn.microsoft.com/library/windows/apps/xaml/jj130767) <br/>  另請參閱[快速入門：將搜尋新增到應用程式](https://msdn.microsoft.com/library/windows/apps/xaml/hh868180) |
| 分段控制項 (UISegmentedControl) | 無 |
| 滑桿 (UISlider) | [Slider](https://msdn.microsoft.com/library/windows/apps/br209614) <br/>  另請參閱[如何新增滑桿](https://msdn.microsoft.com/library/windows/apps/xaml/hh868197) |
| 分割檢視控制項 (UISplitViewController) 和分割檢視控制項委派 (UISplitViewControllerDelegate) | 無 |
| 開關 (UISwitch) | [ToggleSwitch](https://msdn.microsoft.com/library/windows/apps/br209712) <br/>  另請參閱[如何新增切換開關](https://msdn.microsoft.com/library/windows/apps/xaml/hh868198) |
| 索引標籤列控制項 (UITabBarController) 和索引標籤列控制項委派 (UITabBarControllerDelegate) | 無 |
| 表格檢視控制項 (UITableViewController)、表格檢視 (UITableView)、表格檢視委派 (UITableViewDelegate)、表格儲存格 (UITableViewCell) | [ListView](https://msdn.microsoft.com/library/windows/apps/br242878) <br/>  另請參閱[快速入門：新增 ListView 和 GridView 控制項](https://msdn.microsoft.com/library/windows/apps/xaml/hh780650) |
| 文字欄位 (UITextField) 和文字欄位委派 (UITextFieldDelegate) | [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683) <br/>  另請參閱[顯示和編輯文字](https://msdn.microsoft.com/library/windows/apps/mt280218) |
| 文字檢視 (UITextView) 和文字檢視委派 (UITextViewDelegate) | [TextBlock](https://msdn.microsoft.com/library/windows/apps/br209652) <br/>  另請參閱[快速入門：顯示文字](https://msdn.microsoft.com/library/windows/apps/xaml/hh700392) |
| 檢視 (UIView) 和檢視控制項 (UIViewController) | [Page](https://msdn.microsoft.com/library/windows/apps/br227503) <br/>  另請參閱[瀏覽](https://msdn.microsoft.com/library/windows/apps/mt187344) |
| 網頁檢視 (UIWebView) 和網頁檢視委派 (UIWebViewDelegate) | [WebView](https://msdn.microsoft.com/library/windows/apps/br227702) <br/>  另請參閱 [XAML WebView 控制項範例](http://go.microsoft.com/fwlink/p/?LinkId=238582) |
| 視窗 (UIWindow) | [Frame](https://msdn.microsoft.com/library/windows/apps/br242682) <br/>  另請參閱[瀏覽](https://msdn.microsoft.com/library/windows/apps/mt187344) |

如果想知道更多控制項，請參閱[控制項清單](https://msdn.microsoft.com/library/windows/apps/mt185406)。

**注意：** 控制項適用於使用 JavaScript 和 HTML 的 UWP app 的清單，請參閱[控制項清單](https://msdn.microsoft.com/library/windows/apps/hh465453)。

### <a name="next-step"></a>下一步

[開始使用：瀏覽](getting-started-navigation.md)

## <a name="related-topics"></a>相關主題

* [Build 2014：XAML UI 和控制項呢？](http://go.microsoft.com/fwlink/p/?LinkID=397897)
* [Build 2014：使用通用的 XAML UI 架構開發應用程式](http://go.microsoft.com/fwlink/p/?LinkID=397898)
* [Build 2014：使用 Visual Studio 建置 XAML 交集的應用程式](http://go.microsoft.com/fwlink/p/?LinkID=397876)
