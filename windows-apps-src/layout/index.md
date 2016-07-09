---
description: "這些文章可協助您設計在各種裝置與螢幕尺寸上都很容易瀏覽且看起來很棒的 UWP app 並撰寫 UWP app 程式碼。"
title: "配置設計 – Windows 應用程式開發"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: 35a8f78420256cb2da02d7fd4720939a16676600

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div class="uwpd-banner">
<h1 class="uwpd-ruledheader">UWP app 的配置</h1>
</div>

App 結構、版面配置及瀏覽都是您的 App 使用者體驗的基礎。 本節中的文章可協助您建立一個很容易瀏覽，且在不同裝置與螢幕大小上有不同外觀的 App。

## 簡介

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
  <p><b>[App UI 設計簡介](design-and-ui-intro.md)</b><br />
當您設計 UWP app 時，必須建立適合顯示器大小不同之各種裝置的使用者介面。 本文章提供 UI 相關功能與 UWP app 的優點，以及設計回應式 UI 的一些秘訣與技巧的概觀。 </p>
  </div>
  <div class="side-by-side-content-right">
    ![An app running on multiple devices](images/rspd-reposition-type1-sm.png)
  </div>
</div>
</div>

## App 配置與結構
查看這些建議以協助您建構 App 及使用三種類型的 UI 元素：瀏覽、命令及內容。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p>
<b>[瀏覽基本知識](navigation-basics.md)</b><br/>
UWP app 中的瀏覽，以瀏覽結構、瀏覽元素和系統層級功能的彈性模型為基礎。 本文會為您介紹這些元件，並示範如何搭配使用來建立最佳的瀏覽體驗。
</p>
<p>
<b>[內容基本知識](content-basics.md)</b><br/>
任何一種 App 的主要用途都是讓使用者存取內容：在相片編輯 App 中，內容是相片；在旅遊 App 中，內容是地圖與旅遊地點的相關資訊；以此類推。 本文提供三種內容案例的內容設計建議：取用、建立與互動。
</p> 
  </div>
  <div class="side-by-side-content-right">
<p><b>[命令基本知識](commanding-basics.md)</b> <br />
命令元素是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。 此文件說明命令元素，例如按鈕和核取方塊、它們支援的互動，以及裝載它們的命令表面 (例如命令列和操作功能表)。</p>
  </div>
</div>
</div>

## 頁面配置 
這些文章可協助您建立在不同螢幕尺寸、視窗大小、解析度及方向上看起來都會很棒的彈性化 UI。 


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[螢幕大小與中斷點](screen-sizes-and-breakpoints-for-responsive-design.md)</b><br/>
整個 Windows 10 生態系統中，裝置目標和螢幕大小有太多種，以致於無法針對每一個最佳化您的 UI。 我們建議使用幾個重要的寬度 (也稱做「中斷點」) 的設計：360、640、1024 以及 1366 有效像素，做為替代方案。</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[使用 XAML 定義版面配置](layouts-with-xaml.md)</b> <br/>
如何使用 XAML 屬性和版面配置面板，讓您的 App 具有回應性及調適性。</p>
  </div>
</div>
</div>
<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[版面配置面板](layout-panels.md)</b> <br />
深入了解每個面板的每種配置類型，以及示範如何使用它們來配置 XAML UI 元素。</p>
  </div>
  <div class="side-by-side-content-right">
 <p><b>[對齊、邊界及邊框間距](alignment-margin-padding.md)</b> <br />
除了維度屬性 (寬度、高度及限制) 之外，元素還可以有對齊、邊界及邊框間距屬性，這些屬性會在元素進行版面配置階段並在 UI 中轉譯時，影響配置行為。</p> 
  </div>
</div>
</div>





<!--HONumber=Jun16_HO4-->

