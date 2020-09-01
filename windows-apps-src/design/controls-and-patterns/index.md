---
description: 取得對 Windows 應用程式新增控制項和模式的設計指導方針與程式碼撰寫指示。 尋找 45 種以上的實用控制項來用於您的應用程式。
title: Windows 控制項和模式 - Windows 應用程式開發
keywords: uwp 控制項, 使用者介面, 應用程式控制項, windows 控制項
label: Controls & patterns
template: detail.hbs
ms.date: 03/23/2020
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: 08471b8a04d37c34378b549b8534979a4188d7b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173952"
---
# <a name="controls-for-windows-apps"></a>Windows 應用程式控制項

![控制項](../images/controls-2x.png)

在 Windows 應用程式開發中，「控制項」<i></i>是顯示內容或啟用互動的 UI 元素。 控制項是使用者介面的基本要素。 「模式」<i></i>是可結合數個控制項以創造新項目的配方。

我們提供超過 45 種控制項供您使用，從簡單的按鈕到強大的資料控制項 (例如資料格檢視) 都有。  這些控制項是 Fluent 設計系統的一部分，並可幫助您建立在所有裝置與螢幕大小上皆美觀的粗體、可調整 UI。

本節中的文章提供對 Windows 應用程式新增控制項和模式的設計指導方針與程式碼撰寫指示。

## <a name="intro"></a>簡介

以 XAML 和 C# 新增控制項並設定其樣式的一般指示。

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">新增控制項和處理事件</a></b> <br/>
將控制項新增到應用程式有 3 個主要步驟：將控制項新增到應用程式 UI、在控制項上設定屬性，以及將程式碼新增到控制項的事件處理常式以便使其執行某些功能。</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">設定控制項的樣式</a></b> <br/>
您可以使用 XAML 架構，以許多方式自訂應用程式的外觀。 樣式可讓您設定控制項屬性，並在多個控制項重複使用這些設定來擁有一致的外觀。</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>取得 Windows UI 程式庫

|  |  |
| - | - |
| ![WinUI 標誌](images/winui-logo-64x64.png) | 某些控制項僅包含在 Windows UI 程式庫 (WinUI) 中，此程式庫是包含新控制項和 UI 功能的 NuGet 封裝。 若要加以取得，請參閱 [Windows UI 程式庫概觀和安裝指示](/uwp/toolkits/winui/)。<br/>從 WinUI 2.2 版開始，許多控制項的預設樣式已更新為使用圓角。 如需詳細資訊，請參閱[圓角半徑](../style/rounded-corner.md)。 |

## <a name="alphabetical-index"></a>依字母排序的索引

特定控制項和模式的相關詳細資訊。 (如需依功能排序的清單，請參閱[依功能排序的控制項索引](controls-by-function.md))。

:::row:::
    :::column:::

- 動畫視覺播放器 (請參閱 [Lottie](/windows/communitytoolkit/animations/lottie)) ![WinUI 標誌](images/winui-logo-16x16.png)
- [自動建議方塊](auto-suggest-box.md)
- [Button](buttons.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [核取方塊](checkbox.md)
- [色彩選擇器](color-picker.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [下拉式方塊](combo-box.md)
- [命令列](app-bars.md)
- [命令列飛出視窗](command-bar-flyout.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [連絡人卡片](contact-card.md)
- [內容對話方塊](dialogs-and-flyouts/dialogs.md)
- [內容連結](content-links.md)
- [操作功能表](menus.md)
- [日期選擇器](date-picker.md)
- [對話方塊和飛出視窗](dialogs-and-flyouts/index.md)
- [下拉按鈕](buttons.md#create-a-drop-down-button) ![WinUI 標誌](images/winui-logo-16x16.png)
- [翻轉檢視](flipview.md)
- [飛出視窗](dialogs-and-flyouts/flyouts.md)
- [表單](forms.md) (模式)
- [格線檢視](listview-and-gridview.md)
- [超連結](hyperlinks.md)
- [超連結按鈕](hyperlinks.md#create-a-hyperlinkbutton)
- [影像與影像筆刷](images-imagebrushes.md)
- [筆跡控制項](inking-controls.md)
- [清單檢視](listview-and-gridview.md)
- [地圖控制項](../../maps-and-location/display-maps.md)
- [主要/詳細資料](master-details.md) (模式)
- [媒體播放](media-playback.md)
- [功能表列](menus.md#create-a-menu-bar) ![WinUI 標誌](images/winui-logo-16x16.png)
- [功能表飛出視窗](menus.md)
- [瀏覽檢視](navigationview.md) ![WinUI 標誌](images/winui-logo-16x16.png)

    :::column-end:::
    :::column:::

- [數字方塊](number-box.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [視差檢視](..\motion\parallax.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [密碼方塊](password-box.md)
- [個人圖片](person-picture.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [樞紐分析](pivot.md)
- [進度列](progress-controls.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [進度環](progress-controls.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [選項按鈕](radio-button.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [評分控制項](rating.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [重複按鈕](buttons.md#create-a-repeat-button)
- [Rich Edit 方塊](rich-edit-box.md)
- [RTF 區塊](rich-text-block.md)
- [捲動檢視器](scroll-controls.md)
- [搜尋](search.md) (模式)
- [語意式縮放](semantic-zoom.md)
- [形狀](shapes.md)
- [滑桿](slider.md)
- [分割按鈕](buttons.md#create-a-split-button) ![WinUI 標誌](images/winui-logo-16x16.png)
- [分割檢視](split-view.md)
- [撥動控制項](swipe.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [索引標籤檢視](tab-view.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [教學提示](dialogs-and-flyouts/teaching-tip.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [文字區塊](text-block.md)
- [文字方塊](text-box.md)
- [時間選擇器](time-picker.md)
- [切換開關](toggles.md)
- [切換按鈕](buttons.md)
- [切換分割按鈕](buttons.md#create-a-toggle-split-button)
- [工具提示](tooltips.md)
- [樹狀檢視](tree-view.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [雙窗格檢視](two-pane-view.md) ![WinUI 標誌](images/winui-logo-16x16.png)
- [網頁檢視](web-view.md)

    :::column-end:::
:::row-end:::




## <a name="xaml-controls-gallery"></a>XAML 控制項庫

從 Microsoft Store 取得 _XAML 控制項庫_應用程式，以查看這些控制項與 Fluent Design 系統的運作情形。 此應用程式是此網站的互動小幫手。 安裝過後，您就可以使用個別控制頁面上的連結，啟動此應用程式並查看控制項的運作情形。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>其他控制項

<a href="https://www.telerik.com/">Telerik</a>、<a href="https://www.syncfusion.com/uwp-ui-controls">SyncFusion</a>、<a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>、<a href="https://www.infragistics.com/products/universal-windows-platform">Infragistics</a>、<a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> 及 <a href="https://www.actiprosoftware.com/products/controls/universal">ActiPro</a> 等各家公司所推出適用於 Windows 開發的其他控制項。 這些控制項，透過擴大標準系統控制項與自訂控制和服務，為企業與 .NET 開發人員提供額外的支援。