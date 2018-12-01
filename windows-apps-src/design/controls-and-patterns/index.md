---
description: 取得對您的 UWP 應用程式新增控制項和模式的設計指導方針 &amp; 程式碼撰寫指示。 尋找 45 種以上的實用控制項來用於您的應用程式。
title: UWP 控制項和模式 - Windows 應用程式開發
keywords: uwp 控制項, 使用者介面,應用程式控制項
label: Controls & patterns
template: detail.hbs
ms.date: 11/16/2017
ms.topic: article
ms.assetid: ce2e611c-c419-4a14-9095-b88ac711d1b8
ms.localizationpriority: medium
ms.openlocfilehash: c2f1a2b5ae514222ed6ef06cc7099a0261747dbc
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8344558"
---
# <a name="controls-and-patterns-for-uwp-apps"></a>適用於 UWP 應用程式的控制項和模式
 

在 UWP 應用程式開發中，「控制項」<i></i>是顯示內容或啟用互動的 UI 元素。 控制項是使用者介面的基本要素。 「模式」<i></i>是可結合數個控制項以創造新項目的配方。

我們提供超過 45 種控制項供您使用，從簡單的按鈕到強大的資料控制項 (例如資料格檢視) 都有。  這些控制項是 Fluent 設計系統的一部分，並可幫助您建立在所有裝置與螢幕大小上皆美觀的粗體、可調整 UI。 

本節中的文章提供對您的 UWP 應用程式新增控制項和模式的設計指導方針與程式碼撰寫指示。 

## <a name="intro"></a>簡介

以 XAML 和 C# 新增控制項並設定其樣式的一般指示。

:::row:::
    :::column:::
      <p><b><a href="controls-and-events-intro.md">新增控制項和處理事件</a></b> <br/>
將控制項新增到應用程式有 3 個主要步驟︰將控制項新增到應用程式 UI、在控制項上設定屬性，以及將程式碼新增到控制項的事件處理常式以便使其執行某些功能。</p>
    :::column-end:::
    :::column:::
      <p><b><a href="xaml-styles.md">設定控制項的樣式</a></b> <br/>
您可以使用 XAML 架構，以許多方式自訂 app 的外觀。 樣式可讓您設定控制項屬性，並在多個控制項重複使用這些設定來擁有一致的外觀。</p>
    :::column-end:::
:::row-end:::

## <a name="get-the-windows-ui-library"></a>取得 Windows UI 文件庫
有些控制項只都可在 Windows UI 文件庫中。 若要取得它，請參閱[Windows UI 文件庫概觀和安裝指示](/uwp/toolkits/winui/)。

## <a name="alphabetical-index"></a>依字母排序的索引 

特定控制項和模式的相關詳細資訊。 (如需依功能排序的清單，請參閱<a href="controls-by-function.md">依功能排序的控制項索引</a>)。

<div style="column-count: 2; column-gap: 40px; margin-top: 40px;" >
<ul style="margin-top: 0px; padding-top: 0px; list-style-type: none;">
<li style="list-style-type: none;"><a href="auto-suggest-box.md">自動建議方塊</a></li>

<li style="list-style-type: none;"><a href="app-bars.md">列</a></li>

<li style="list-style-type: none;"><a href="buttons.md">按鈕</a></li>

<li style="list-style-type: none;"><a href="checkbox.md">核取方塊 </a></li>

<li style="list-style-type: none;"><a href="color-picker.md">色彩選擇器</a></li>

<li style="list-style-type: none;"><a href="contact-card.md">連絡人卡片</a></li>

<li style="list-style-type: none;"><a href="date-and-time.md">日期和時間控制項</a></li>

<li style="list-style-type: none;"><a href="dialogs-and-flyouts/index.md">對話方塊和飛出視窗</a></li>

<li style="list-style-type: none;"><a href="flipview.md">翻轉檢視</a></li>

<li style="list-style-type: none;"><a href="forms.md">表單</a></li>

<li style="list-style-type: none;"><a href="hub.md">中樞</a></li>

<li style="list-style-type: none;"><a href="hyperlinks.md">超連結</a></li>

<li style="list-style-type: none;"><a href="images-imagebrushes.md">影像與影像筆刷</a></li>

<li style="list-style-type: none;"><a href="inking-controls.md">筆跡控制項</a></li>

<li style="list-style-type: none;"><a href="lists.md">清單</a></li>

<li style="list-style-type: none;"><a href="../../maps-and-location/controls-map.md">地圖控制項</a></li>

<li style="list-style-type: none;"><a href="master-details.md">主要/詳細資料</a></li>

<li style="list-style-type: none;"><a href="media-playback.md">媒體播放</a></li>

<li style="list-style-type: none;"><a href="menus.md">功能表和特色選單</a></li>

<li style="list-style-type: none;"><a href="navigationview.md">瀏覽檢視</a></li>

<li style="list-style-type: none;"><a href="person-picture.md">個人圖片</a></li>

<li style="list-style-type: none;"><a href="pivot.md">樞紐分析</a></li>

<li style="list-style-type: none;"><a href="progress-controls.md">進度控制項</a></li>

<li style="list-style-type: none;"><a href="radio-button.md">選項按鈕</a></li>

<li style="list-style-type: none;"><a href="rating.md">評分控制項</a></li>

<li style="list-style-type: none;"><a href="scroll-controls.md">捲動和移動瀏覽控制項</a></li>

<li style="list-style-type: none;"><a href="search.md">搜尋</a></li>

<li style="list-style-type: none;"><a href="semantic-zoom.md">語意式縮放</a></li>

<li style="list-style-type: none;"><a href="shapes.md">形狀</a></li>

<li style="list-style-type: none;"><a href="slider.md">滑桿</a></li>

<li style="list-style-type: none;"><a href="split-view.md">分割檢視</a></li>

<li style="list-style-type: none;"><a href="text-controls.md">文字控制項</a></li>


<li style="list-style-type: none;"><a href="toggles.md">切換</a></li>
<li style="list-style-type: none;"><a href="tooltips.md">工具提示</a></li>

<li style="list-style-type: none;"><a href="tree-view.md">樹狀檢視</a></li>

<li style="list-style-type: none;"><a href="web-view.md">網頁檢視</a></li>
</ul>
</div>

## <a name="xaml-controls-gallery"></a>XAML 控制項庫

從 Microsoft Store 取得 _XAML 控制項庫_ App，以便查看這些控制項與 Fluent Design 系統的運作情形。 此 App 是此網站的互動小幫手。 安裝過後，您就可以使用個別控制頁面上的連結，啟動此 App 並查看控制項的運作情形。

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a>

<a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a>

<img src="images/xaml-controls-gallery.png" alt="XAML Controls Gallery screen" />

## <a name="additional-controls"></a>其他控制項

<a href="http://www.telerik.com/">Telerik</a>、<a href="https://www.syncfusion.com/products/uwp">SyncFusion</a>、<a href="https://www.devexpress.com/Products/NET/Controls/Win10Apps/">DevExpress</a>、<a href="http://www.infragistics.com/products/universal-windows-platform">Infragistics</a>、<a href="https://www.componentone.com/Studio/Platform/UWP">ComponentOne</a> 及 <a href="http://www.actiprosoftware.com/products/controls/universal">ActiPro</a> 等各家公司所推出適用於 UWP 開發的其他控制項。 這些控制項，透過擴大標準系統控制項與自訂控制和服務，為企業與 .NET 開發人員提供額外的支援。  

如果您想要深入了解這些控制項，請查看 GitHub 上的<a href="https://github.com/Microsoft/Windows-appsample-customers-orders-database">客戶訂單資料庫</a>範例。 這個範例使用資料格和 Telerik 的資料項驗證，這是其 UWP 套件的部分 UI。 UWP 套件的 UI 是超過 20 個控制項的集合，透過 .NET Foundation 以開放原始碼專案形式提供。
