---
description: 了解如何讓您的應用程式包容世界各地的使用者且無障礙。
keywords: uwP 應用程式協助工具, 全球化, 設計包容性應用程式, 協助工具應用程式需求
title: UWP 應用程式的可用性 - Windows 應用程式開發
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: c725839a29c093c78eb977538da4c43d906051c6
ms.sourcegitcommit: 08cb5a4ca2e02179ad6b768c841fe3d5216bcae3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614955"
---
# <a name="usability-for-uwp-apps"></a>UWP 應用程式的可用性

在小地方的貼心設計和對細節的嚴苛要求，才能讓使用者體驗轉變成可滿足全球使用者需求，而成為真正具有包容性的使用者體驗。

本節中的設計與程式碼撰寫指示可協助您透過新增協助工具功能、支援全球化與當地語系化、讓使用者能夠自訂其體驗，以及在使用者需要時提供協助，來提高您 UWP 應用程式的包容性。

## <a name="accessibility"></a>協助工具

協助工具可以針對無法或不便使用傳統使用者介面的殘障人士，讓他們也能使用應用程式。 根據法律的規定，某些情況必須提供協助工具。 不過無論法律的規定為何，最好能解決無障礙問題，這樣應用程式便能吸引更多的使用者。

[協助工具入口網站](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your UWP app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>應用程式設定

應用程式設定可讓您的使用者自訂您的應用程式、將應用程式最佳化以滿足他們的個別需求和喜好。 提供正確的設定並正確儲存設定甚至可讓良好的使用者體驗變得更好。

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../app-settings/guidelines-for-app-settings.md">指導方針</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">建立和顯示應用程式設定的最佳做法。</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../app-settings/store-and-retrieve-app-data.md">儲存和擷取應用程式資料</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">如何儲存及擷取本機、漫遊和暫存的應用程式資料。</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">Guidelines</a></b><br/>Best practices for creating and displaying app settings.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">Store and retrieve app data</a></b><br/>How to store and retrieve local, roaming, and temporary app data.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul> -->

## <a name="globalization-and-localization"></a>全球化與當地語系化

全球不同語言、地區和文化的對象都使用 Windows。 您的使用者使用各種不同的語言，居住在各種不同的國家和地區。 有些使用者甚至會說一種以上的語言。 因此，您的應用程式是根據包含許多語言、地區及文化系統設定變更的設定執行的。 使用*全球化*和*當地語系化*可設計您的應用程式並使它能做出調整，以提升應用程式的潛在市場。

<a href="../globalizing/globalizing-portal.md">全球化和當地語系化入口網站</a>

## <a name="in-app-help"></a>應用程式內說明
無論您的應用程式設計有多完美，某些使用者也將會需要一些額外的說明。

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/guidelines-for-app-help.md">應用程式說明的指導方針</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">應用程式可能十分複雜，並提供有效的說明，讓使用者可以大幅改善其體驗。</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/instructional-ui.md">指示性 UI</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">有些時候，教導使用者有關應用程式中不明顯的功能 (例如特定觸控互動) 可能十分有用。 在這些情況下，您需要透過 UI 對使用者顯示指示，讓他們可以探索和使用他們可能遺漏的功能。</p>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/in-app-help.md">App 內說明</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">大部分時間，最好是在 app 內顯示說明，並在使用者選擇檢視時才顯示。 當建立 App 內說明時，請考慮下列指導方針。</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/external-help.md">外部說明</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">大部分時間，最好是在 app 內顯示說明，並在使用者選擇檢視時才顯示。 當建立 App 內說明時，請考慮下列指導方針。</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">Guidelines for app help</a></b><br/>Applications can be complex, and providing effective help for your users can greatly improve their experience.
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">Instructional UI</a></b><br/>Sometimes it can be helpful to teach the user about functions in your app that might not be obvious to them, such as specific touch interactions. In these cases, you need to present instructions to the user through the UI so they can discover and use features they might have missed.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">In-app help</a></b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">External help</a></b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul> -->

