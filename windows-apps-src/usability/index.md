---
description: "了解如何讓您的應用程式包容世界各地的使用者且無障礙。"
keywords: "uwP 應用程式協助工具, 全球化, 設計包容性應用程式, 協助工具應用程式需求"
title: "UWP 應用程式的可用性 - Windows 應用程式開發"
author: mijacobs
label: Usability
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.openlocfilehash: d05db2899fe2ad26eaf5e200ae2673b8efafddd0
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="usability-for-uwp-apps"></a>UWP 應用程式的可用性

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

在小地方的貼心設計和對細節的嚴苛要求，才能讓使用者體驗轉變成可滿足全球使用者需求，而成為真正具有包容性的使用者體驗。

本節中的設計與程式碼撰寫指示可協助您透過新增協助工具功能、支援全球化與當地語系化、讓使用者能夠自訂其體驗，以及在使用者需要時提供協助，來提高您 UWP 應用程式的包容性。


## <a name="accessiblity"></a>協助工具

協助工具可以針對無法或不便使用傳統使用者介面的殘障人士，讓他們也能使用應用程式。 根據法律的規定，某些情況必須提供協助工具， 不過無論法律的規定為何，最好能解決無障礙問題，這樣應用程式便能吸引更多的使用者。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[協助工具概觀](../accessibility/accessibility-overview.md)</b> <br/> 這篇文章提供 UWP 應用程式協助工具案例相關概念和技術的概觀。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[設計全人軟體](../accessibility/designing-inclusive-software.md)</b><br/>了解如何透過適用於 Windows 10 的通用 Windows 平台 (UWP) 應用程式來改善全人設計。  在設計和建置全人軟體時考量提供無障礙功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[開發全人 Windows 應用程式](../accessibility/developing-inclusive-windows-apps.md)</b><br/> 此文章是適用於開發無障礙 UWP 應用程式的藍圖。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[協助工具測試](../accessibility/accessibility-testing.md) </b><br/>確認 UWP 應用程式可提供無障礙功能的測試程序。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[市集中的協助工具](../accessibility/accessibility-in-the-store.md)</b><br/>說明在 Windows 市集中，宣告 UWP 應用程式提供無障礙功能的條件。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[協助工具檢查清單](../accessibility/accessibility-checklist.md)</b><br/>提供一份檢查清單，協助您確認 UWP 應用程式提供無障礙功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[公開基本的協助工具資訊](../accessibility/basic-accessibility-information.md)</b><br/>基本協助工具資訊通常分類為名稱、角色以及值。 本主題說明的程式碼可協助您的應用程式公開輔助技術所需的基本資訊。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[鍵盤協助工具](../accessibility/keyboard-accessibility.md)</b><br/>如果應用程式未能提供適切的鍵盤功能操作，盲眼或行動不便的使用者將難以使用應用程式，或者根本無法使用。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[高對比佈景主題](../accessibility/high-contrast-themes.md)</b><br/>說明 UWP 應用程式在使用高對比佈景主題時，確保其可以正常運作所需的步驟。 </p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[協助工具文字的需求](../accessibility/accessible-text-requirements.md)</b><br/>本主題說明應用程式中文字的協助工具最佳做法，方法是確保色彩和背景能夠滿足必要的對比率。 本主題也討論 UWP 應用程式中文字元素可以擁有的 Microsoft 使用者介面自動化角色，以及圖形中文字的最佳做法。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[協助工具應避免的做法](../accessibility/practices-to-avoid.md)</b><br/>列出建立無障礙 UWP 應用程式時應避免的做法。</p>
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



## <a name="globalization-and-localization"></a>全球化與當地語系化

全球不同文化特性、地區和語言的對象都使用 Windows。 使用者可能會說任何語言，甚至是多種語言。 使用者可能位於世界上的任何地方，而且可能會說任何位置的任何語言。 您可以使用全球化和當地語系化，將您的應用程式設計成容易適應，以增加應用程式的潛在市場。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[可行與禁止事項](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)</b><br/>在將您的應用程式推廣至全球更廣泛的受眾，以及對特定市場將應用程式當地語系化時，請遵循這些最佳做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[使用全球通用格式](../globalizing/use-global-ready-formats.md)</b><br/>藉由為日期、時間、數字及貨幣進行適當的格式設定，即可開發全球通用的應用程式。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[管理語言及地區](../globalizing/manage-language-and-region.md)</b><br/>藉由使用 Windows 所提供的各種語言及地區設定，來控制 Windows 如何選取 UI 資源，以及如何將應用程式的 UI 元素格式化。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[使用模式來設定日期和時間的格式](../globalizing/use-patterns-to-format-dates-and-times.md)</b><br/>使用 [<strong>DateTimeFormatting</strong>](https://msdn.microsoft.com/library/windows/apps/br206859) API 搭配自訂模式，以您想要的格式來顯示日期和時間。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[調整配置和字型並支援 RTL](../globalizing/adjust-layout-and-fonts--and-support-rtl.md)</b><br/>開發您的應用程式以支援多種語言的配置和字型，包括 RTL (從右至左 ) 文字方向。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[準備應用程式以進行當地語系化](../globalizing/prepare-your-app-for-localization.md)</b><br/>準備應用程式以針對其他市場、語言或地區進行當地語系化。</p>
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


## <a name="app-settings"></a>應用程式設定

應用程式設定可讓您的使用者自訂您的應用程式、將應用程式最佳化以滿足他們的個別需求和喜好。 提供正確的設定並正確儲存設定甚至可讓良好的使用者體驗變得更好。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[指導方針](../app-settings/guidelines-for-app-settings.md)</b><br/>建立和顯示應用程式設定的最佳做法。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[儲存和擷取應用程式資料](../app-settings/store-and-retrieve-app-data.md)</b><br/>如何儲存及擷取本機、漫遊和暫存的應用程式資料。</p>
  </div>
</div>
</div>

## <a name="in-app-help"></a>應用程式內說明
無論您的應用程式設計有多完美，某些使用者也將會需要一些額外的說明。

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[應用程式說明的指導方針](../in-app-help/guidelines-for-app-help.md)</b><br/>應用程式可能十分複雜，提供有效的說明，讓使用者可以大幅改善其體驗。
</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[指示性 UI](../in-app-help/instructional-ui.md)</b><br/>有些時候，教導使用者有關應用程式中不明顯的功能 (例如特定觸控互動) 可能十分有用。 在這些情況下，您需要透過 UI 對使用者顯示指示，讓他們可以探索和使用他們可能遺漏的功能。</p>
  </div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
<p><b>[應用程式內說明](../in-app-help/in-app-help.md)</b><br/>大部分時間，最好是在應用程式內顯示說明，並在使用者選擇檢視時才顯示。 當建立應用程式內說明時，請考慮下列指導方針。</p>
  </div>
  <div class="side-by-side-content-right">
<p><b>[外部說明](../in-app-help/external-help.md)</b><br/>大部分時間，最好是在應用程式內顯示說明，並在使用者選擇檢視時才顯示。 當建立應用程式內說明時，請考慮下列指導方針。</p>
  </div>
</div>
</div>
