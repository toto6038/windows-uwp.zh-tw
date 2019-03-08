---
Description: 在通用 Windows 平台 (UWP) app 中，命令元素是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。
title: Universal Windows Platform (UWP) app 的命令設計基本知識
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 11/01/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ac2bd55d1cea25359c3c609148c7098532d76c46
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654053"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP 應用程式的命令設計基本知識

在通用 Windows 平台 (UWP) 應用程式中，*命令項目*是互動式的 UI 項目可讓使用者執行動作傳送電子郵件、 刪除項目，或送出表單。 *命令介面*是組成的常見命令項目、 將其裝載的命令介面、 其所支援的互動和它們所提供的體驗。

## <a name="provide-the-best-command-experience"></a>提供最佳的命令體驗

命令介面的最重要的一點是您想讓使用者完成。 當您在規劃您的應用程式的功能，請考慮完成這些工作與您想要啟用的使用者體驗所需的步驟。 當您完成之後的初始的草稿，這些體驗時，您就可以進行決策來實作它們的工具和互動。

以下是一些常見的命令體驗：

- 傳送或提交資訊
- 選取設定和選擇
- 搜尋和篩選內容
- 開啟、儲存和刪除檔案
- 編輯或建立內容

要與您的命令經驗的設計有創意。 選擇哪個輸入裝置，您的應用程式支援，以及您的應用程式如何回應每個裝置。 透過支援最廣泛的功能和喜好設定就可以將您的應用程式，可使用狀態，可攜性，並盡可能可存取 (請參閱[命令執行通用 Windows 平台 (UWP) 應用程式的設計](../controls-and-patterns/commanding.md)如需詳細資訊)。



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>選擇正確的命令項目

在命令介面中使用正確的項目，可以讓直覺、 簡單易用的應用程式和困難且令人困惑的應用程式之間的差異。 通用 Windows 平台 (UWP) 提供一組完整的命令元素。 以下是一些最常見的 UWP 命令元素的清單。

:::row:::
    :::column:::
        ![button image](images/commanding/thumbnail-button.svg)
    :::column-end:::
    :::column span="2":::
        <b>Buttons</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::row-end:::

:::row:::
    :::column:::
        ![list image](images/commanding/thumbnail-list.svg)
    :::column-end:::
    :::column span="2":::
        <b>Lists</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::row-end:::

:::row:::
    :::column:::
        ![selection control image](images/commanding/thumbnail-selection.svg)
    :::column-end:::
    :::column span="2":::
        <b>Selection controls</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::row-end:::

:::row:::
    :::column:::
        ![Calendar  image](images/commanding/thumbnail-calendar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Calendar, date and time pickers</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::row-end:::

:::row:::
    :::column:::
        ![Predictive text entry image](images/commanding/thumbnail-autosuggest.svg)
    :::column-end:::
    :::column span="2":::
        <b>Predictive text entry</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::row-end:::

如需完整清單，請參閱[控制項與 UI 元素](../controls-and-patterns/index.md)

## <a name="place-commands-on-the-right-surface"></a>在正確的表面放置命令

在您的應用程式，包括應用程式畫布或特殊命令的容器，例如命令列、 命令列飛出視窗、 功能表列或對話方塊中，您可以在幾個介面的放置命令項目。

永遠嘗試讓使用者直接操作內容而不是透過命令的內容，例如將拖放以重新排列清單項目，而不是向上和向下 命令按鈕。 

不過，這可能無法使用特定的輸入裝置，或當配合特定的使用者能力和喜好設定。 在這些情況下，盡可能的情況下，提供多個命令的提供，並將這些命令項目放在您的應用程式中的命令介面上。

以下是一些常用的命令介面的清單。

:::row:::
    :::column:::
        ![app canvas image](images/commanding/thumbnail-canvas.svg)
    :::column-end:::
    :::column span="2":::
        <b>App canvas (content area)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::row-end:::

:::row:::
    :::column:::
        ![commandbar image](images/commanding/thumbnail-commandbar.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bars and menu bars</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen (a <a href="../controls-and-patterns/menus.md#create-a-menu-bar">MenuBar</a> can also be used when the functionality in your app is too complex for a command bar).
:::row-end:::

:::row:::
    :::column:::
        ![context menu image](images/commanding/thumbnail-contextmenu.svg)
    :::column-end:::
    :::column span="2":::
        <b>Menus and context menus</b>

        <p>Menus and context menus save space by organizing commands and hiding them until the user needs them. Users typically access a menu or context menu by clicking a button or right-clicking a control.</p> 

        <p>The <a href="../controls-and-patterns/command-bar-flyout.md">command bar flyout </a> is a type of contextual menu that combines the benefits of a command bar and a context menu into a single control. It can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands.</p>

        <p>UWP also provides a set of traditional menus and context menus; for more info, see the <a href="../controls-and-patterns/menus.md">menus and context menus overview</a>.</p>
:::row-end:::

## <a name="provide-command-feedback"></a>提供命令的意見反應 

命令的意見反應與使用者，偵測到的互動或命令、 如何命令已解譯和處理，以及是否命令成功與否。 這可協助使用者了解他們所做什麼，以及他們可以接著做。 意見反應最好能自然整合在 UI 中，除非絕對必要，否則不讓使用者被打斷，或是執行其他動作。

> [!NOTE]
> 只在必要時，且只有在還未出現在其他地方，請提供意見反應。 簡潔且整齊除非您要新增的值，保留您的應用程式 UI。

以下是在 App 中提供意見反應的一些方式。

:::row:::
    :::column:::
        ![commandbar content area image](images/commanding/thumbnail-commandbar2.svg)
    :::column-end:::
    :::column span="2":::
        <b>Command bar</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::row-end:::

:::row:::
    :::column:::
        ![Flyout image](images/commanding/thumbnail-flyout.svg)
    :::column-end:::
    :::column span="2":::
        <b>Flyouts</b>

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">延伸顯示</a>是輕量級內容的快顯可以藉由點選或按一下飛出視窗外的任何一處關閉。
:::row-end:::

:::row:::
    :::column:::
        ![Dialog image](images/commanding/thumbnail-dialog.svg)
    :::column-end:::
    :::column span="2":::
        <b>Dialog controls</b>

        <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::row-end:::

> [!TIP]
> 請注意應用程式使用確認對話方塊的頻率；在使用者犯錯時雖然非常有幫助，但在使用者有意執行某些動作時也是一種妨礙。

### <a name="when-to-confirm-or-undo-actions"></a>確認或復原動作的時機

不論如何妥善設計您的應用程式 UI 是，所有使用者都執行他們想要它們所沒有的動作。 您的應用程式可以在需要確認的動作，或是提供的方式來復原最近的動作，在這些情況下幫助。

:::row:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can't be undone and have major consequences, we recommend using a confirmation dialog. Examples of such actions include:
        -   Overwriting a file
        -   Not saving a file before closing
        -   Confirming permanent deletion of a file or data
        -   Making a purchase (unless the user opts out of requiring a confirmation)
        -   Submitting a form, such as signing up for something
    :::column-end:::
    :::column:::
        ![do image](images/do.svg)

        For actions that can be undone, offering a simple undo command is usually enough. Examples of such actions include:
        -   Deleting a file
        -   Deleting an email (not permanently)
        -   Modifying content or editing text
        -   Renaming a file
:::row-end:::

##  <a name="optimize-for-specific-input-types"></a>最佳化特定的輸入類型

如需最佳化特定輸入類型或裝置之使用者體驗的詳細資訊，請參閱[互動基本資訊](../input/index.md)。

