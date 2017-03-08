---
description: "取得對您的 UWP 應用程式新增控制項和模式的設計指導方針與程式碼撰寫指示。 尋找 45 種以上的實用控制項來用於您的應用程式。"
title: "UWP 控制項和模式 - Windows 應用程式開發"
author: mijacobs
keywords: "uwp 控制項, 使用者介面,應用程式控制項"
label: Controls & patterns
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
translationtype: Human Translation
ms.sourcegitcommit: 412a3f70861c6cd1bbf003fe0bd78c8547a5f3f8
ms.openlocfilehash: 7b525267c8f4d24af95f6d41d46d33a3adf10f8f
ms.lasthandoff: 02/08/2017

---
# <a name="controls-and-patterns-for-uwp-apps"></a>適用於 UWP 應用程式的控制項和模式
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

在 UWP 應用程式開發中，「控制項」<i></i>是顯示內容或啟用互動的 UI 元素。 控制項是使用者介面的基本要素。 我們提供超過 45 種控制項供您使用，從簡單的按鈕到強大的資料控制項 (例如資料格檢視) 都有。 「模式」<i></i>是可結合數個控制項以創造新項目的配方。

本節中的文章提供對您的 UWP 應用程式新增控制項和模式的設計指導方針與程式碼撰寫指示。 

## <a name="intro"></a>簡介

以 XAML 和 C# 新增控制項並設定其樣式的一般指示。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[新增控制項和處理事件](controls-and-events-intro.md)</b> <br/>
將控制項新增到應用程式有 3 個主要步驟︰將控制項新增到應用程式 UI、在控制項上設定屬性，以及將程式碼新增到控制項的事件處理常式以便使其執行某些功能。</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>[設定控制項的樣式](styling-controls.md)</b> <br/>
您可以使用 XAML 架構，以許多方式自訂應用程式的外觀。 樣式可讓您設定控制項屬性，並在多個控制項重複使用這些設定來擁有一致的外觀。</p>
  </div>
</div>
</div>

## <a name="alphabetical-index"></a>依字母排序的索引 

特定控制項和模式的相關詳細資訊。

(如需依功能排序的清單，請參閱[依功能排序的控制項索引](controls-by-function.md))。

<div class="uwpd-list-of-links">
<ul>

<li>[自動建議方塊](auto-suggest-box.md)</li>

<li>[列](app-bars.md)</li>

<li>[按鈕](buttons.md)</li>

<li>[核取方塊 ](checkbox.md)</li>

<li>[日期和時間控制項](date-and-time.md)
<ul>

<li>[行事曆日期選擇器](calendar-date-picker.md)</li>

<li>[行事曆檢視](calendar-view.md)</li>

<li>[日期選擇器](date-picker.md)</li>

<li>[時間選擇器](time-picker.md)</li>
</ul>
</li>


<li>[對話方塊和飛出視窗](dialogs.md)</li>

<li>[翻轉檢視](flipview.md)</li>

<li>[中樞](hub.md)</li>

<li>[超連結](hyperlinks.md)</li>

<li>[影像與影像筆刷](images-imagebrushes.md)</li>

<li>[清單](lists.md)</li>

<li>[地圖控制項](../maps-and-location/controls-map.md)</li>

<li>[主要/詳細資料](master-details.md)</li>

<li>[媒體播放](media-playback.md)
<ul>
<li>[自訂傳輸控制項](custom-transport-controls.md)</li>
</ul>
</li>

<li>[功能表和操作功能表](menus.md)</li>

<li>[瀏覽窗格](nav-pane.md)</li>

<li>[進度控制項](progress-controls.md)</li>

<li>[選項按鈕](radio-button.md)</li>

<li>[捲動和移動瀏覽控制項](scroll-controls.md)</li>

<li>[搜尋](search.md)</li>

<li>[語意式縮放](semantic-zoom.md)</li>

<li>[滑桿](slider.md)</li>

<li>[分割檢視](split-view.md)</li>

<li>[索引標籤與樞紐](tabs-pivot.md)</li>

<li>[文字控制項](text-controls.md)
<ul>

<li>[標籤](labels.md)</li>

<li>[密碼方塊](password-box.md)</li>

<li>[可編輯對話方塊](rich-edit-box.md)</li>

<li>[RTF 區塊](rich-text-block.md)</li>

<li>[拼字檢查和預測](spell-checking-and-prediction.md)</li>

<li>[文字區塊](text-block.md)</li>

<li>[文字方塊](text-box.md)</li>
</ul>
</li>



<li>[磚、徽章及通知](tiles-badges-notifications.md)
<ul>

<li>[磚](tiles-and-notifications-creating-tiles.md)</li>

<li>[彈性磚](tiles-and-notifications-create-adaptive-tiles.md)</li>

<li>[彈性磚結構描述](tiles-and-notifications-adaptive-tiles-schema.md)</li>

<li>[資產指導方針](tiles-and-notifications-app-assets.md)</li>

<li>[特殊磚範本](tiles-and-notifications-special-tile-templates-catalog.md)</li>

<li>[調適型和互動式快顯通知](tiles-and-notifications-adaptive-interactive-toasts.md)</li>

<li>[徽章通知](tiles-and-notifications-badges.md)</li>

<li>[通知視覺化工具](tiles-and-notifications-notifications-visualizer.md)</li>

<li>[通知傳遞方法](tiles-and-notifications-choosing-a-notification-delivery-method.md)</li>

<li>[本機磚通知](tiles-and-notifications-sending-a-local-tile-notification.md)</li>

<li>[定期通知](tiles-and-notifications-periodic-notification-overview.md)</li>

<li>[WNS](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</li>

<li>[原始通知](tiles-and-notifications-raw-notification-overview.md)</li>
</ul>
</li>


<li>[切換](toggles.md)</li>
<li>[工具提示](tooltips.md)</li>

<li>[網頁檢視](web-view.md)</li>
</ul>
</div>

## <a name="additional-controls-options"></a>其他控制選項

公司提供的其他 UWP 開發適用的控制項，例如 [Telerik](http://www.telerik.com/)、[SyncFusion](https://www.syncfusion.com/products/uwp)、[DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)、[Infragistics](http://www.infragistics.com/products/universal-windows-platform)、[ComponentOne](https://www.componentone.com/Studio/Platform/UWP) 及 [ActiPro](http://www.actiprosoftware.com/products/controls/universal)。 這些控制項，透過擴大標準系統控制項與自訂控制和服務，為企業與 .NET 開發人員提供額外的支援。  

如果您想要深入了解這些控制項，請查看 GitHub 上的[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)範例。 這個範例使用資料格和 Telerik 的資料項驗證，這是其 UWP 套件的部分 UI。 UWP 套件的 UI 是超過 20 個控制項的集合，透過 .NET Foundation 以開放原始碼專案形式提供。

![客戶訂單資料庫影像](images/customerOrdersDataGrid.png)
