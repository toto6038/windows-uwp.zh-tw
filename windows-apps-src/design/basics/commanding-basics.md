---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: 通用 Windows 平台 (UWP) app 的命令設計基本知識
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 07b9ce7b5a57f6dc1ba202ed57e8b2d4d93e583f
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654387"
---
#  <a name="command-design-basics-for-uwp-apps"></a>UWP 應用程式的命令設計基本知識

在通用 Windows 平台 (UWP) app 中，*命令元素*是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。 

本文說明通用命令元素、支援的互動，以及裝載命令的命令介面。

![在地圖 app 中的命令元素](images/maps.png)

上方可看到地圖 app 中命令元素的範例。

## <a name="provide-the-right-type-of-interactions"></a>提供正確的互動類型

設計命令介面時，最重要的是決定使用者應該可以做甚麼事情。 若要規劃正確的互動類型，請專注於您的 App - 考慮您要實現的使用者體驗，以及使用者需執行的步驟。 一旦您決定希望使用者完成的事，就可以為他們提供做事所需的工具。

以下是您可能想要提供 App 的一些互動：

- 傳送或提交資訊 
- 選取設定和選擇
- 搜尋和篩選內容
- 開啟、儲存和刪除檔案
- 編輯或建立內容

## <a name="use-the-right-command-element-for-the-interaction"></a>針對互動使用正確的命令元素

使用正確的元素實現命令互動，可以讓人感覺出 app 是直覺易用的，還是混亂難用的 app。 通用 Windows 平台 (UWP) 提供許多可在您 App 中使用的命令元素。 以下是一些最常見的控制項清單以及它們可啟用的互動摘要。

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">類別</th>
<th align="left">元素</th>
<th align="left">互動</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>按鈕</b><br/><br/>
    <img src="../controls-and-patterns/images/controls/button.png" alt="button" /></td>
<td align="left"><a href="../controls-and-patterns/buttons.md">按鈕</a></td>
<td align="left">觸發立即的動作。 範例包括傳送電子郵件、提交表單資料，或確認對話方塊中的動作。</td>
</tr>
<tr class="even">
<td align="left">清單<br/><br/>
    <img src="../controls-and-patterns/images/controls/combo-box-open.png" alt="drop down list" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">下拉式清單、清單方塊、清單檢視與方格檢視</a></td>
<td align="left">呈現互動式清單或是方格中的項目。 通常用於多個選項或顯示項目。</td>
</tr>
<tr class="odd">
<td align="left">選取控制項<br/><br/>
    <img src="../controls-and-patterns/images/controls/radio-button.png" alt="radio button" /></td>
<td align="left"><a href="../controls-and-patterns/checkbox.md">核取方塊</a>、<a href="../controls-and-patterns/radio-button.md">選項按鈕</a>、<a href="../controls-and-patterns/toggles.md">切換開關</a></td>
<td align="left">可讓使用者選擇幾個選項，例如完成問卷或是進行 App 設定。</td>
</tr>
<tr class="even">
<td align="left">日期和時間選擇器<br/><br/>
    <img src="../controls-and-patterns/images/controls/calendar-date-picker-open.png" alt="date picker" /></td>
<td align="left"><a href="../controls-and-patterns/date-and-time.md">行事曆日期選擇器、行事曆檢視、日期選擇器、時間選擇器</a></td>
<td align="left">可讓使用者檢視與修改日期和時間資訊，例如建立事件或設定鬧鐘時。</td>
</tr>
<tr class="odd">
<td align="left">預測文字輸入<br/><br/>
    <img src="../controls-and-patterns/images/controls/auto-suggest-box.png" alt="autosuggest box" /></td>
<td align="left"><a href="../controls-and-patterns/auto-suggest-box.md">自動建議方塊</a></td>
<td align="left">在使用者輸入時提供建議，例如輸入資料或執行查詢時。</td>
</tr>
</tbody>
</table>
</div>

如需完整清單，請參閱[控制項與 UI 元素](https://dev.windows.com/design/controls-patterns)

##  <a name="place-commands-on-the-right-surface"></a>在正確的介面放置命令
您可以在 app 中的數個介面放置命令元素，包括 app 畫布或特殊命令容器，例如命令列、功能表、對話方塊，以及飛出視窗。

請注意，您應盡可能讓使用者直接操作內容，而不要使用命令來處理內容。 例如，讓使用者拖放清單項目來重新排列清單，而不要使用向上和向下命令按鈕。
  
如果使用者無法直接操作內容，則在您 App 中的命令介面上放置下列命令元素：

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">介面</th>
<th align="left">說明</th>
<th align="left">範例</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top">應用程式畫布 (內容區域)
<p><img src="images/content-area.png" alt="The content area of an app" /></p></td>

<td align="left" style="vertical-align: top;">如果是使用者完成核心案例時經常需要的命令，請將它放在畫布上。 因為您可將命令放在靠近它們影響的物件 (或放在物件上)，將命令放在畫布上使它們更明顯、更易於使用。
<p>然而，請仔細選擇要放在畫布上的命令。 App 畫布上過多的命令會佔去寶貴的螢幕空間，而且會妨礙使用者。 如果某個命令不常使用，請考慮將它放在其他的命令介面。</p> 
</td><td>
地圖 App 畫布上的自動建議方塊。
<br></br>
  <img src="images/maps-canvas.png" alt="autosuggest box on Maps app canvas"/>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/app-bars.md">命令列</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p></td>
<td align="left" style="vertical-align: top;"> 命令列有助於組織命令，讓它們更容易存取。 命令列可以放置於畫面頂端、畫面底部，或同時放置於畫面頂端與底部。 
</td>
<td>
地圖 App 頂端的命令列。
<br></br>
<img src="images/maps-commandbar.png" alt="command bar in Maps app"/>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/menus.md">功能表和操作功能表</a>
<p><img src="images/controls-contextmenu-singlepane.png" alt="Example of a single-pane context menu" /></p></td>
<td align="left" style="vertical-align: top;">有時候將多個命令群組為命令功能表是更有效率的做法，可節省空間。 功能表和操作功能表會在使用者要求命令或選項時，顯示它們的清單。
<p>操作功能表可以提供常用動作的快速鍵，並提供存取只與特定內容相關的次要命令，例如剪貼簿或自訂命令。 操作功能表通常透過使用者按一下滑鼠右鍵提示。</p>
</td><td>
當使用者在地圖 app 中按右鍵，就會顯示操作功能表。
<br></br>
  <img src="images/maps-contextmenu.png" alt="context menu in Maps app"/>
</td>
</tr>
</table>
</div>

## <a name="provide-feedback-for-interactions"></a>提供有關互動的意見反應

意見反應可溝通命令結果，並讓使用者了解他們完成的操作以及後續動作。 意見反應最好能自然整合在 UI 中，除非絕對必要，否則不讓使用者被打斷，或是執行其他動作。 

以下是在 App 中提供意見反應的一些方式。

<div class="mx-responsive-img">
<table class="uwpd-top-aligned-table">

<tr class="header">
<th align="left">介面</th>
<th align="left">描述</th>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"> <a href="../controls-and-patterns/app-bars.md">命令列</a>
<p><img src="../controls-and-patterns/images/controls_appbar_icons.png" alt="Example of a command bar with icons" /></p>
</td>
<td align="left" style="vertical-align: top;"> 命令列的內容區域是與使用者直接溝通狀態的位置，如果使用者想要查看意見反應。
<p>
  <img src="images/commandbar_anatomy.png" alt="Command bar content area for feedback"/>
  </p>
</td>
</tr>

<tr class="even">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">飛出視窗</a>
<p><img src="images/controls-flyout-default-200.png" alt="Image of default flyout" /></p></td>
<td align="left" style="vertical-align: top;">
一種輕量型的內容相關快顯視窗，只要點選或按一下飛出視窗外的地方就可以關閉。
<p>
  <img src="../controls-and-patterns/images/controls/flyout.png" alt="Flyout above button"/>
  </p>
</td>
</tr>

<tr class="odd">
<td align="left" style="vertical-align: top;"><a href="../controls-and-patterns/dialogs.md">對話方塊控制項</a>
<p><img src="images/controls-dialog-twobutton-200.png" alt="Example of a simple two-button dialog" /></p></td>
<td align="left" style="vertical-align: top;">對話方塊是提供內容相關應用程式資訊的強制回應 UI 重疊項目。 大部分的狀況下，對話方塊會阻擋與應用程式視窗的互動，直到對話方塊確實關閉為止，而且通常需要使用者執行某種類型的動作來關閉對話方塊。
<p>對話方塊具有破壞性，因此只應在特定情況下使用。 如需詳細資訊，請參閱 [When to confirm or undo actions](#when-to-confirm-or-undo-actions) (確認或復原動作的時機) 章節。</p>
<p>
  <img src="../controls-and-patterns/images/dialogs/dialog_RS2_delete_file.png" alt="dialog delete file"/></p>
</td>
</tr>

</table>
</div>

> [!TIP]
> 請注意應用程式使用確認對話方塊的頻率；在使用者犯錯時雖然非常有幫助，但在使用者有意執行某些動作時也是一種妨礙。

### <a name="when-to-confirm-or-undo-actions"></a>確認或復原動作的時機

無論是設計多良好的使用者介面和多細心的使用者，在有些時候使用者還是會執行意料外的動作而想取消。 在這些情況下，應用程式透過要求使用者確認執行動作，或提供復原最近動作的功能會十分有幫助。

-   對於無法復原和有嚴重後果的動作，建議您使用確認對話方塊。 類似動作的範例包含：
    -   覆寫檔案
    -   關閉檔案前尚未存檔
    -   確認永久刪除檔案或資料
    -   進行線上購物 (除非使用者選擇不需要確認)
    -   提交表單，例如註冊
-   對於可以復原的動作，提供簡單的復原命令即可。 類似動作的範例包含：
    -   刪除檔案
    -   刪除電子郵件 (非永久性)
    -   修改內容或編輯文字
    -   重新命名檔案

##  <a name="optimize-for-specific-input-types"></a>最佳化特定的輸入類型

如需最佳化特定輸入類型或裝置之使用者體驗的詳細資訊，請參閱[互動基本資訊](../input/index.md)。