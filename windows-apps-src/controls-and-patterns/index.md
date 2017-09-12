---
description: "取得對您的 UWP 應用程式新增控制項和模式的設計指導方針 &amp; 程式碼撰寫指示。 尋找 45 種以上的實用控制項來用於您的應用程式。"
title: "UWP 控制項和模式 - Windows 應用程式開發"
author: mijacobs
keywords: "uwp 控制項, 使用者介面,應用程式控制項"
label: Controls & patterns
template: detail.hbs
ms.author: mijacobs
ms.date: 09/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.openlocfilehash: 0946a32df990f08f00f07ad0094125709b45dcaf
ms.sourcegitcommit: 0d5b3daddb3ae74f91178c58e35cbab33854cb7f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2017
---
# <a name="controls-and-patterns-for-uwp-apps"></a>適用於 UWP 應用程式的控制項和模式
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

在 UWP 應用程式開發中，「控制項」<i></i>是顯示內容或啟用互動的 UI 元素。 控制項是使用者介面的基本要素。 「模式」<i></i>是可結合數個控制項以創造新項目的配方。

我們提供超過 45 種控制項供您使用，從簡單的按鈕到強大的資料控制項 (例如資料格檢視) 都有。  這些控制項是 Fluent 設計系統的一部分，並可幫助您建立在所有裝置與螢幕大小上皆美觀的粗體、可調整 UI。 

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
您可以使用 XAML 架構，以許多方式自訂 app 的外觀。 樣式可讓您設定控制項屬性，並在多個控制項重複使用這些設定來擁有一致的外觀。</p>
  </div>
</div>
</div>

## <a name="alphabetical-index"></a>依字母排序的索引 

特定控制項和模式的相關詳細資訊。 (如需依功能排序的清單，請參閱[依功能排序的控制項索引](controls-by-function.md))。

<div style="column-count: 2; column-gap: 40px; margin-top: 40px;" >
<ul style="margin-top: 0px; padding-top: 0px; list-style-type: none;">
<li style="list-style-type: none;">[自動建議方塊](auto-suggest-box.md)</li>

<li style="list-style-type: none;">[列](app-bars.md)</li>

<li style="list-style-type: none;">[按鈕](buttons.md)</li>

<li style="list-style-type: none;">[核取方塊 ](checkbox.md)</li>

<li style="list-style-type: none;">[色彩選擇器](color-picker.md)</li>

<li style="list-style-type: none;">[日期和時間控制項](date-and-time.md)</li>


<li style="list-style-type: none;">[對話方塊和飛出視窗](dialogs.md)</li>

<li style="list-style-type: none;">[翻轉檢視](flipview.md)</li>

<li style="list-style-type: none;">[中樞](hub.md)</li>

<li style="list-style-type: none;">[超連結](hyperlinks.md)</li>

<li style="list-style-type: none;">[影像與影像筆刷](images-imagebrushes.md)</li>

<li style="list-style-type: none;">[筆跡控制項](inking-controls.md)</li>

<li style="list-style-type: none;">[清單](lists.md)</li>

<li style="list-style-type: none;">[地圖控制項](../maps-and-location/controls-map.md)</li>

<li style="list-style-type: none;">[主要/詳細資料](master-details.md)</li>

<li style="list-style-type: none;">[媒體播放](media-playback.md)</li>

<li style="list-style-type: none;">[功能表和操作功能表](menus.md)</li>

<li style="list-style-type: none;">[導覽檢視](navigationview.md)</li>

<li style="list-style-type: none;">[個人圖片](person-picture.md)</li>

<li style="list-style-type: none;">[進度控制項](progress-controls.md)</li>

<li style="list-style-type: none;">[選項按鈕](radio-button.md)</li>

<li style="list-style-type: none;">[評分控制項](rating.md)</li>

<li style="list-style-type: none;">[捲動和移動瀏覽控制項](scroll-controls.md)</li>

<li style="list-style-type: none;">[搜尋](search.md)</li>

<li style="list-style-type: none;">[語意式縮放](semantic-zoom.md)</li>

<li style="list-style-type: none;">[滑桿](slider.md)</li>

<li style="list-style-type: none;">[分割檢視](split-view.md)</li>

<li style="list-style-type: none;">[索引標籤與樞紐](tabs-pivot.md)</li>

<li style="list-style-type: none;">[文字控制項](text-controls.md)</li>

<li style="list-style-type: none;">[磚、徽章及通知](tiles-badges-notifications.md)</li>


<li style="list-style-type: none;">[切換](toggles.md)</li>
<li style="list-style-type: none;">[工具提示](tooltips.md)</li>

<li style="list-style-type: none;">[樹狀檢視](tree-view.md)</li>

<li style="list-style-type: none;">[網頁檢視](web-view.md)</li>
</ul>
</div>

## <a name="additional-controls"></a>其他控制項

[Telerik](http://www.telerik.com/)、[SyncFusion](https://www.syncfusion.com/products/uwp)、[DevExpress](https://www.devexpress.com/Products/NET/Controls/Win10Apps/)、[Infragistics](http://www.infragistics.com/products/universal-windows-platform)、[ComponentOne](https://www.componentone.com/Studio/Platform/UWP) 及 [ActiPro](http://www.actiprosoftware.com/products/controls/universal) 等各家公司所推出適用於 UWP 開發的其他控制項。 這些控制項，透過擴大標準系統控制項與自訂控制和服務，為企業與 .NET 開發人員提供額外的支援。  

如果您想要深入了解這些控制項，請查看 GitHub 上的[客戶訂單資料庫](https://github.com/Microsoft/Windows-appsample-customers-orders-database)範例。 這個範例使用資料格和 Telerik 的資料項驗證，這是其 UWP 套件的部分 UI。 UWP 套件的 UI 是超過 20 個控制項的集合，透過 .NET Foundation 以開放原始碼專案形式提供。
