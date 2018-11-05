---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: 通用 Windows 平台 (UWP) app 的命令設計基本知識
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 10/01/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 028cc9586180f2d94337282c3ed0cd58317b539b
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6044791"
---
# <a name="command-design-basics-for-uwp-apps"></a>UWP 應用程式的命令設計基本知識

在通用 Windows 平台 (UWP) app 中，*命令元素*是讓使用者執行動作，例如傳送電子郵件、 刪除項目，或提交表單的互動式 UI 元素。 *命令介面*都被組成通用命令元素、 裝載它們的命令表面、 它們支援的互動，以及它們所提供的體驗。

## <a name="provide-the-best-command-experience"></a>提供最佳的命令體驗

最重要的命令介面是哪些您嘗試讓使用者完成。 當您計劃您的應用程式的功能，請考慮要完成這些工作與您想要啟用的使用者體驗的必要步驟。 一旦您已完成的初始草稿的這些體驗，您可以進行決策的工具和互動來實作它們。

以下是一些常見的應用程式體驗：

- 傳送或提交資訊
- 選取設定和選擇
- 搜尋和篩選內容
- 開啟、儲存和刪除檔案
- 編輯或建立內容

發揮創意與您的命令體驗的設計。 選擇裝置的輸入您的應用程式支援，以及每個裝置的應用程式會如何回應。 藉由支援廣泛的功能和喜好設定中，您可以讓您的應用程式做為可用性、 可攜性，並盡可能無障礙。



<!--
When designing a command interface, the most important decision is choosing what a user can do. To plan the right type of interactions, focus on your app - consider the user experiences you want to enable, and what steps users will need to take. Once you decide what you want users to accomplish, then you can provide them the tools to do so.
-->

## <a name="choose-the-right-command-elements"></a>選擇正確的命令元素

使用正確的元素中的命令介面，可以讓 app 是直覺方便使用與難以、 令人混淆的應用程式之間的差異。 在通用 Windows 平台 (UWP) 提供一組完整的命令元素。 以下是一些最常見的 UWP 命令元素的清單。

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

## <a name="place-commands-on-the-right-surface"></a>在正確介面上放置命令

在您的應用程式，包括 app 畫布或特殊命令容器，例如命令列、 命令列飛出視窗、 功能表列或對話方塊中，您可以在數個表面放置命令元素。

一律嘗試讓使用者直接操作內容，而非透過命令，該內容，例如拖放功能以重新排列清單項目，而不是向上和向下命令按鈕上的動作。 

不過，這可能無法使用某些輸入裝置，或當儘特定使用者的能力和喜好設定。 在這些情況下，提供多個命令能供性資料，並在您的應用程式中的命令介面上放置下列命令元素。

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

## <a name="provide-command-feedback"></a>提供命令意見反應 

命令的意見反應會傳達給使用者，互動或命令已偵測到、 它已在如何解譯和處理，和是否它是否成功。 這可協助使用者了解他們完成，以及它們可以後續動作。 意見反應最好能自然整合在 UI 中，除非絕對必要，否則不讓使用者被打斷，或是執行其他動作。

> [!NOTE]
> 沒有提供意見反應，除非絕對必要，意見反應沒有可用的其他地方。 簡潔、 整齊除非您要新增的值，讓您的應用程式 UI。

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

       <a href="../controls-and-patterns/dialogs-and-flyouts/index.md">飛出視窗</a>是輕量型的內容相關快顯的可關閉只要點選或按一下飛出視窗以外的地方。
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

無論是設計多良好應用程式的 UI 時，所有使用者都執行動作，而想細心。 您的應用程式可協助在這些情況下，透過要求確認的動作，或提供復原最近動作的方式。

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

##  <a name="optimize-for-specific-input-types"></a>為特定的輸入類型最佳化

如需最佳化特定輸入類型或裝置之使用者體驗的詳細資訊，請參閱[互動基本資訊](../input/index.md)。

