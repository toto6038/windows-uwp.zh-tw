---
description: "本節中的設計與程式碼撰寫指示可協助您針對特定類型輸入與裝置自訂您的 UWP app。"
title: "UWP app 的可用性 - Windows 應用程式開發"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: f02713dfee278866af53c6dd529d2faa3e9f625c

---

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

# UWP app 的可用性

UWP app 可自動處理各種輸入並在各種不同的裝置上執行。例如，您不需要額外執行任何動作，即可啟用觸控輸入或讓您的 App 在手機上執行。 

但是，有時您可能會想要針對特定類型的輸入或裝置，將您的 App 最佳化。 例如，如果您正在建立繪圖 App，您可以自訂手寫筆輸入的處理方式。 

本節中的設計與程式碼撰寫指示可協助您針對特定類型輸入與裝置自訂您的 UWP app。 

## 協助工具

協助工具可以針對無法或不便使用傳統使用者介面的殘障人士，讓他們也能使用應用程式。 根據法律的規定，某些情況必須提供協助工具， 不過無論法律的規定為何，最好能解決無障礙問題，這樣應用程式便能吸引更多的使用者。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[協助工具概觀](../accessibility/accessibility-overview.md)</b> <br/> 這篇文章提供 UWP app 協助工具案例相關概念和技術的概觀。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[設計全人軟體](../accessibility/designing-inclusive-software.md)</b><br/>了解如何透過適用於 Windows 10 的通用 Windows 平台 (UWP) app 來改善全人設計。  在設計和建置全人軟體時考量提供無障礙功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[開發全人 Windows 應用程式](../accessibility/developing-inclusive-windows-apps.md)</b><br/> 此文章是適用於開發無障礙 UWP app 的藍圖。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[協助工具測試](../accessibility/accessibility-testing.md) </b><br/>確認 UWP app 可提供無障礙功能的測試程序。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[市集中的協助工具](../accessibility/accessibility-in-the-store.md)</b><br/>說明在 Windows 市集中，宣告 UWP app 提供無障礙功能的條件。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[協助工具檢查清單](../accessibility/accessibility-checklist.md)</b><br/>提供一份檢查清單，協助您確認 UWP app 提供無障礙功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[公開基本的協助工具資訊](../accessibility/basic-accessibility-information.md)</b><br/>基本協助工具資訊通常分類為名稱、角色以及值。 本主題說明的程式碼可協助您的 App 公開輔助技術所需的基本資訊。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[鍵盤協助工具](../accessibility/keyboard-accessibility.md)</b><br/>如果 app 未能提供適切的鍵盤功能操作，盲眼或行動不便的使用者將難以使用 app，或者根本無法使用。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[高對比佈景主題](../accessibility/high-contrast-themes.md)</b><br/>說明 UWP app 在使用高對比佈景主題時，確保其可以正常運作所需的步驟。 </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[協助工具文字的需求](../accessibility/accessible-text-requirements.md)</b><br/>本主題說明 App 中文字的協助工具最佳做法，方法是確保色彩和背景能夠滿足必要的對比率。 本主題也討論 UWP app 中文字元素可以擁有的 Microsoft 使用者介面自動化角色，以及圖形中文字的最佳做法。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[協助工具應避免的做法](../accessibility/practices-to-avoid.md)</b><br/>列出建立無障礙 UWP app 時應避免的做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[自訂自動化對等](../accessibility/custom-automation-peers.md)</b><br/>說明使用者介面自動化的自動化對等觀念，以及如何提供自訂 UI 類別的自動化支援。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[控制項模式和介面](../accessibility/control-patterns-and-interfaces.md)</b><br/>本文列出 Microsoft UI 自動化控制項模式、用戶端用來存取控制項模式的類別，以及介面提供者用來實作控制項模式的類別。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b></b>   
</p>
  </div>
</div>
</div>



## 全球化與當地語系化

全球不同文化特性、地區和語言的對象都使用 Windows。 使用者可能會說任何語言，甚至是多種語言。 使用者可能位於世界上的任何地方，而且可能會說任何位置的任何語言。 您可以使用全球化和當地語系化，將您的 App 設計成容易適應，以增加 App 的潛在市場。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[可行與禁止事項](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>當您要將 app 全球化以適應更廣泛的使用對象，或是針對特定的市場將 app 當地語系化時，請遵循這些最佳做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[使用全球通用格式](../globalizing/use-global-ready-formats.md)</b><br/>藉由為日期、時間、數字及貨幣進行適當的格式設定，即可開發全球通用的 app。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[管理語言及地區](../globalizing/manage-language-and-region.md)</b><br/>藉由使用 Windows 所提供的各種語言及地區設定，來控制 Windows 如何選取 UI 資源，以及如何將 app 的 UI 元素格式化。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[使用模式來設定日期和時間的格式](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>使用 [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API 搭配自訂模式，以您想要的格式來顯示日期和時間。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[調整配置和字型並支援 RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>開發您的 app 以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[準備 app 以進行當地語系化](../globalizing/prepare-your-app-for-localization.md)</b><br/>準備 app 以針對其他市場、語言或地區進行當地語系化。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[將 UI 字串放入資源](../globalizing/put-ui-strings-into-resources.md)</b><br/>將 UI 的字串資源放入資源檔。 接著您就可以從程式碼或標記中參照這些字串。</p>
  </div>
  <div class="side-by-side-content-right">
<b></b>   
<p></p>
  </div>
</div>
</div>


## 應用程式設定

應用程式設定可讓您的使用者自訂您的 App、將 App 最佳化以滿足他們的個別需求和喜好。 提供正確的設定並正確儲存設定甚至可讓良好的使用者體驗變得更好。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[指導方針](../app-settings/guidelines-for-app-settings.md)</b><br/>建立和顯示 app 設定的最佳做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[儲存和擷取應用程式資料](../app-settings/store-and-retrieve-app-data.md)</b><br/>如何儲存及擷取本機、漫遊和暫存的 App 資料。</p>
  </div>
</div>
</div>

## 應用程式內說明
無論您的 App 設計有多完美，某些使用者也將會需要一些額外的說明。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[App 說明的指導方針](../app-help-guidelines/guidelines-for-app-help.md)</b><br/>應用程式可能十分複雜，提供有效的說明，讓使用者可以大幅改善其體驗。 
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[指示性 UI](../app-help-guidelines/instructional-ui.md)</b><br/>有些時候，教導使用者有關 App 中不明顯的功能 (例如特定觸控互動) 可能十分有用。 在這些情況下，您需要透過 UI 對使用者顯示指示，讓他們可以探索和使用他們可能遺漏的功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[應用程式內說明](../app-help-guidelines/in-app-help.md)</b><br/>大部分時間，最好是在應用程式內顯示說明，並在使用者選擇檢視時才顯示。 當建立應用程式內說明時，請考慮下列指導方針。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[外部說明](../app-help-guidelines/external-help.md)</b><br/>大部分時間，最好是在應用程式內顯示說明，並在使用者選擇檢視時才顯示。 當建立應用程式內說明時，請考慮下列指導方針。</p>
  </div>
</div>
</div>






<!--HONumber=Jun16_HO4-->


