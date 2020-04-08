---
Description: 在通用 Windows 平台 (UWP) app 中，命令元素是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。
title: Universal Windows Platform (UWP) app 的命令設計基本知識
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.date: 11/01/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 6be51c274078d3b8db5ae50033bbf714ec4aa12a
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081397"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP 應用程式的命令設計基本知識

在通用 Windows 平台 (UWP) 應用程式中，「命令元素」  是讓使用者執行動作 (例如傳送電子郵件、刪除項目，或提交表單) 的互動式 UI 元素。 「命令介面」  是由常見命令元素、將其裝載的命令介面、其所支援的互動，以及它們所提供的體驗所組成。

## <a name="provide-the-best-command-experience"></a>提供最佳的命令體驗

命令介面的最重要層面是您嘗試讓使用者完成的工作。 當您規劃您應用程式的功能時，請考慮完成這些工作與您希望達到的使用者體驗所需的步驟。 在您完成這些體驗的初始草稿後，您就可以決定用於實作它們的工具和互動方式。

以下是一些常見的命令體驗：

- 傳送或提交資訊
- 選取設定和選擇
- 搜尋和篩選內容
- 開啟、儲存和刪除檔案
- 編輯或建立內容

您的命令體驗的設計要有創意。 選擇您的應用程式支援的輸入裝置，以及您的應用程式回應每個裝置的方式。 透過支援最廣泛的功能和喜好設定，盡可能讓您的應用程式可使用、可攜性及可存取 (如需詳細資訊，請參閱[通用 Windows 平台 (UWP) 應用程式的命令設計](../controls-and-patterns/commanding.md))。



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>選擇正確的命令元素

在命令介面中使用正確的元素，可以讓人感覺出應用程式是直覺易用的，還是混亂難用的應用程式。 通用 Windows 平台 (UWP) 有一組完整的命令元素可用。 以下是一些常用的 UWP 命令元素清單。

:::row:::
    :::column:::
![按鈕影像](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
<b>按鈕</b>

<a href="../controls-and-patterns/buttons.md" style="text-decoration:none">按鈕</a>會觸發立即動作。 範例包括傳送電子郵件、提交表單資料，或確認對話方塊中的動作。
:::row-end:::

:::row:::
    :::column:::
![列出影像](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
<b>清單</b>

<a href="../controls-and-patterns/lists.md" style="text-decoration:none">清單</a>呈現互動式清單或是方格中的項目。 通常用於多個選項或顯示項目。 範例包含下拉式清單、清單方塊、清單檢視與方格檢視。
:::row-end:::

:::row:::
    :::column:::
![選取控制項影像](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
<b>選取控制項</b>

可讓使用者選擇幾個選項，例如完成問卷或是進行 App 設定的時間。 範例包括<a href="../controls-and-patterns/checkbox.md">核取方塊</a>、<a href="../controls-and-patterns/radio-button.md">選項按鈕</a>和<a href="../controls-and-patterns/toggles.md">切換開關</a>。
:::row-end:::

:::row:::
    :::column:::
![行事曆影像](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
<b>行事曆、日期和時間選擇器</b>

<a href="../controls-and-patterns/date-and-time.md">行事曆、日期和時間選擇器</a>可讓使用者檢視與修改日期和時間資訊，例如建立事件或設定鬧鐘的時間。 範例包含行事曆日期選擇器、行事曆檢視、日期選擇器、時間選擇器。
:::row-end:::

:::row:::
    :::column:::
![預測文字輸入影像](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
<b>預測文字輸入</b>

在使用者輸入 (例如輸入資料或執行查詢) 時提供建議。 範例包括<a href="../controls-and-patterns/auto-suggest-box.md">自動建議方塊</a>。<br>
:::row-end:::

如需完整清單，請參閱[控制項與 UI 元素](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>在正確的表面放置命令

您可以在應用程式中的數個介面放置命令元素，包括應用程式畫布或特殊命令容器，例如命令列、命令列飛出視窗、功能表列或對話方塊。

一律嘗試讓使用者直接操作內容，而不是透過在內容上作用的命令，例如拖放以重新排列清單項目，而不是使用向上和向下命令按鈕。 

不過，這對於特定的輸入裝置，或在適應特定使用者能力和喜好設定時，有可能不可行。 在這些情況下，盡可能提供許多命令能供性，並將這些命令元素置於您應用程式中的命令介面上。

以下是一些常用的命令介面的清單。

:::row:::
    :::column:::
![應用程式畫布影像](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
<b>應用程式畫布 (內容區域)</b>

如果是使用者完成核心案例時經常需要的命令，請將它放在畫布上。 因為您可將命令放在靠近它們影響的物件 (或放在物件上)，將命令放在畫布上使它們更明顯、更易於使用。 然而，請仔細選擇要放在畫布上的命令。 應用程式畫布上過多的命令會佔去寶貴的螢幕空間，而且會妨礙使用者。 如果某個命令不常使用，請考慮將它放在其他命令介面。
:::row-end:::

:::row:::
    :::column:::
![CommandBar 影像](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
<b>命令列和功能表列</b>

<a href="../controls-and-patterns/app-bars.md">命令列</a>有助於組織命令，讓它們更容易存取。 命令列可以放在畫面頂端、畫面底部或同時放在畫面頂端和底部 (當應用程式中的功能對命令列而言太複雜時，也可以使用 <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a>)。
:::row-end:::

:::row:::
    :::column:::
![操作功能表影像](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
<b>功能表和操作功能表</b>

<p>功能表和操作功能表可組織命令並在使用者不需要它們時加以隱藏，藉以節省空間。 使用者通常會藉由按一下按鈕或以滑鼠右鍵按一下控制項，來存取功能表或操作功能表。</p> 

<p><a href="../controls-and-patterns/command-bar-flyout.md">命令列飛出視窗</a>是一種內容相關的功能表，可將命令列和操作功能表的優點結合成單一控制項。 這個功能表可以提供常用動作的快速鍵，並提供存取只與特定內容相關的次要命令，例如剪貼簿或自訂命令。</p>

<p>UWP 也提供一組傳統功能表和操作功能表；如需詳細資訊，請參閱<a href="../controls-and-patterns/menus.md">功能表和操作功能表概觀</a>。</p>
:::row-end:::

## <a name="provide-command-feedback"></a>提供命令意見反應 

命令意見反應會將以下資訊傳達給使用者：偵測到互動或命令、解譯和處理命令的方式，以及命令成功與否。 這可協助使用者了解他們做了什麼，以及他們接下來可以做什麼。 意見反應最好能自然整合在 UI 中，除非絕對必要，否則不讓使用者被打斷，或是執行其他動作。

> [!NOTE]
> 只有在必要時或其他地方無法取得時，提供意見反應。 讓您的應用程式 UI 保持整齊簡潔，除非您要增加價值。

以下是在應用程式中提供意見反應的一些方式。

:::row:::
    :::column:::
![commandbar 內容區域影像](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
<b>命令列</b>

如果使用者想要查看意見反應，<a href="../controls-and-patterns/app-bars.md">命令列</a>的內容區域是與使用者直接溝通狀態的位置。
:::row-end:::

:::row:::
    :::column:::
![飛出視窗影像](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
<b>飛出視窗</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">飛出視窗</a>是一種輕量型關聯式快顯視窗，只要點選或按一下飛出視窗外的地方就可以關閉。
:::row-end:::

:::row:::
    :::column:::
![對話方塊影像](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
<b>對話方塊控制項</b>

<a href="../controls-and-patterns/dialogs-and-flyouts/index.md">對話方塊控制項</a>是提供內容相關應用程式資訊的強制回應 UI 重疊項目。 大部分的狀況下，對話方塊會阻擋與應用程式視窗的互動，直到對話方塊確實關閉為止，而且通常需要使用者執行某種類型的動作來關閉對話方塊。 對話方塊具有破壞性，因此只應在特定情況下使用。 如需詳細資訊，請參閱 [When to confirm or undo actions](#when-to-confirm-or-undo-actions) (確認或復原動作的時機) 章節。
    :::column-end:::
:::row-end:::

> [!TIP]
> 請注意應用程式使用確認對話方塊的頻率；在使用者犯錯時雖然非常有幫助，但在使用者有意執行某些動作時也是一種妨礙。

### <a name="when-to-confirm-or-undo-actions"></a>確認或復原動作的時機

不論您應用程式 UI 的設計有多好，所有使用者都會執行他們所沒有的動作。 在這些情況下，應用程式透過要求確認動作，或提供復原最近動作的方法會有所幫助。

:::row:::
    :::column:::
![可行項目影像](images/do.svg)

對於無法復原和有嚴重後果的動作，建議您使用確認對話方塊。 類似動作的範例包含：
-   覆寫檔案
-   關閉檔案前尚未存檔
-   確認永久刪除檔案或資料
-   進行線上購物 (除非使用者選擇不需要確認)
-   提交表單，例如註冊
    :::column-end:::
    :::column:::
![可行項目影像](images/do.svg)

對於可以復原的動作，提供簡單的復原命令即可。 類似動作的範例包含：
-   刪除檔案
-   刪除電子郵件 (非永久性)
-   修改內容或編輯文字
- 重新命名檔案
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>最佳化特定的輸入類型

如需最佳化特定輸入類型或裝置之使用者體驗的詳細資訊，請參閱[互動基本資訊](../input/index.md)。

