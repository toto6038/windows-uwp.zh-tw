---
author: mijacobs
Description: In a Universal Windows Platform (UWP) app, command elements are the interactive UI elements that enable the user to perform actions, such as sending an email, deleting an item, or submitting a form.
title: 通用 Windows 平台 (UWP) app 的命令設計基本知識
ms.assetid: 1DB48285-07B7-4952-80EF-02B57D4469F2
label: Command design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 05/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 09f775ad0ba596379b6d3ddf158285849520111f
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842565"
---
#  <a name="command-design-basics-for-uwp-apps"></a>UWP 應用程式的命令設計基本知識

在通用 Windows 平台 (UWP) app 中，*命令元素*是讓使用者執行動作，例如傳送電子郵件、刪除項目，或提交表單的互動式 UI 元素。 本文說明通用命令元素、支援的互動，以及裝載命令的命令介面。

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

:::列::: :::欄::: ![按鈕圖像](images/commanding/thumbnail-button.svg) :::欄結束::: :::欄範圍="2"::: <b>按鈕</b>

        <a href="../controls-and-patterns/buttons.md" style="text-decoration:none">Buttons</a> trigger an immediate action. Examples include sending an email, submitting form data, or confirming an action in a dialog.
:::列結束:::

:::列::: :::欄::: ![清單圖像](images/commanding/thumbnail-list.svg) :::欄結束::: :::欄範圍="2"::: <b>清單</b>

        <a href="../controls-and-patterns/lists.md" style="text-decoration:none">Lists</a> present items in a interactive list or a grid. Usually used for many options or display items. Examples include drop-down list, list box, list view and grid view.
:::列結束:::

::: 列:::::: 欄:::![選擇控制項圖像](images/commanding/thumbnail-selection.svg)::: 欄結束:::::: 欄範圍 ="2":::<b>選擇控制項</b>

        Lets users choose from a few options, such as when completing a survey or configuring app settings. Examples include <a href="../controls-and-patterns/checkbox.md">check box</a>, <a href="../controls-and-patterns/radio-button.md">radio button</a>, and <a href="../controls-and-patterns/toggles.md">toggle switch</a>.
:::列結束:::

::: 列:::::: 欄:::![行事曆圖像](images/commanding/thumbnail-calendar.svg)::: 欄結束:::::: 欄範圍 ="2":::<b>行事曆、日期和時間選擇器</b>

        <a href="../controls-and-patterns/date-and-time.md">Calendar, date and time pickers</a> enable users to view and modify date and time info, such as when creating an event or setting an alarm. Examples include calendar date picker, calendar view, date picker, time picker.
:::列結束:::

::: 列:::::: 欄:::![預測的文字項目圖像](images/commanding/thumbnail-autosuggest.svg)::: 欄結束:::::: 欄範圍 ="2":::<b>預測的文字項目</b>

        Provides suggestions as users type, such as when entering data or performing queries. Examples include <a href="../controls-and-patterns/auto-suggest-box.md">auto-suggest box</a>.<br>
:::列結束:::

如需完整清單，請參閱[控制項與 UI 元素](../controls-and-patterns/index.md)

##  <a name="place-commands-on-the-right-surface"></a>在正確介面上放置命令
您可以在 app 中的數個介面放置命令元素，包括 app 畫布或特殊命令容器，例如命令列、功能表、對話方塊，以及飛出視窗。

請注意，您應盡可能讓使用者直接操作內容，而不要使用命令來處理內容。 例如，讓使用者拖放清單項目來重新排列清單，而不要使用向上和向下命令按鈕。

如果使用者無法直接操作內容，則在您 App 中的命令介面上放置下列命令元素。 以下是一些常用的命令介面的清單。

::: 列:::::: 欄::: ![應用程式畫布影像](images/commanding/thumbnail-canvas.svg)::: 欄結束:::::: 欄範圍 ="2":::<b>應用程式畫布 (內容區域)</b>

        If a command is constantly needed for users to complete core scenarios, put it on the canvas. Because you can put commands near (or on) the objects they affect, putting commands on the canvas makes them easy and obvious to use. However, choose the commands you put on the canvas carefully. Too many commands on the app canvas take up valuable screen space and can overwhelm the user. If the command won't be frequently used, consider putting it in another command surface.
:::列結束:::

:::列::: :::欄::: ![命令列影像](images/commanding/thumbnail-commandbar.svg) :::欄結束::: :::欄範圍="2"::: <b>命令列</b>

        <a href="../controls-and-patterns/app-bars.md">Command bars</a> help organize commands and make them easy to access. Command bars can be placed at the top of the screen, at the bottom of the screen, or at both the top and bottom of the screen.
:::列結束:::

::: 列:::::: 欄:::![特色選單影像](images/commanding/thumbnail-contextmenu.svg)::: 欄結束:::::: 欄範圍 ="2":::<b>選單及特色選單</b>

        Sometimes it is more efficient to group multiple commands into a command menu to save space. <a href="../controls-and-patterns/menus.md">Menus and context menus</a> display a list of commands or options when the user requests them. Context menus can provide shortcuts to commonly-used actions and provide access to secondary commands that are only relevant in certain contexts, such as clipboard or custom commands. Context menus are usually prompted by a user right-clicking.
:::列結束:::

## <a name="provide-feedback-for-interactions"></a>提供有關互動的意見反應

意見反應可溝通命令結果，並讓使用者了解他們完成的操作以及後續動作。 意見反應最好能自然整合在 UI 中，除非絕對必要，否則不讓使用者被打斷，或是執行其他動作。 

以下是在 App 中提供意見反應的一些方式。

:::列::: :::欄::: ![命令列內容區域影像](images/commanding/thumbnail-commandbar2.svg) :::欄結束::: :::欄範圍="2"::: <b>命令列</b>

        The content area of the <a href="../controls-and-patterns/app-bars.md">command bar</a> is an intuitive place to communicate status to users if they'd like to see feedback.
:::列結束:::

:::列::: :::欄::: ![飛出視窗影像](images/commanding/thumbnail-flyout.svg) :::欄結束::: :::欄範圍="2"::: <b>飛出視窗</b>

       <a href="../controls-and-patterns/dialogs.md">Flyouts</a> are lightweight contextual popups that can be dismissed by tapping or clicking somewhere outside the flyout.
:::列結束:::

::: 列:::::: 欄:::![對話方塊影像](images/commanding/thumbnail-dialog.svg)::: 欄結束:::::: 欄範圍 ="2":::<b>對話方塊控制項</b>

        <a href="../controls-and-patterns/dialogs.md">Dialog controls</a> are modal UI overlays that provide contextual app information. In most cases, dialogs block interactions with the app window until being explicitly dismissed, and often request some kind of action from the user. Dialogs can be disruptive and should only be used in certain situations. For more info, see the [When to confirm or undo actions](#when-to-confirm-or-undo-actions) section.
    :::column-end:::
:::列結束:::

> [!TIP]
> 請注意應用程式使用確認對話方塊的頻率；在使用者犯錯時雖然非常有幫助，但在使用者有意執行某些動作時也是一種妨礙。

### <a name="when-to-confirm-or-undo-actions"></a>確認或復原動作的時機

無論是設計多良好的使用者介面和多細心的使用者，在有些時候使用者還是會執行意料外的動作而想取消。 在這些情況下，應用程式透過要求使用者確認執行動作，或提供復原最近動作的功能會十分有幫助。

::: 列:::::: 欄:::![執行影像](images/do.svg)

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
:::列結束:::

##  <a name="optimize-for-specific-input-types"></a>為特定的輸入類型最佳化

如需最佳化特定輸入類型或裝置之使用者體驗的詳細資訊，請參閱[互動基本資訊](../input/index.md)。

