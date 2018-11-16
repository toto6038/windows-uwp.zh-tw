---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: 磚、徽章及通知
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eaca23a72ff794a85ffd8ac13c3f522cabf32aa7
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6843665"
---
# <a name="tiles-badges-and-notifications-for-uwp-apps"></a>UWP app的磚、徽章及通知
 

了解如何使用磚、徽章、快顯通知以及通知提供 App 的進入點，並將使用者維持在最新狀態。

> **重要 API**：[UWP Community Toolkit Notifications NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

<p><img style="float: left; margin: 0px 15px 15px 0px;" src="images/tile-and-live-tile.png" />
磚是 App 在 [開始] 功能表中的表示方式。 每個 UWP app 都會有一個磚。 您可以啟用不同的磚大小 (小、中、寬及大)。</p>

<p>您可以使用「磚通知」<em></em>更新磚，以將新的資訊 (例如新聞頭條或最新未讀郵件的主旨) 傳遞給使用者。</p>

<p>您可以使用「徽章」<em></em>以系統提供的字符或 1-99 的數字形式，提供狀態或摘要資訊。 徽章也會出現在 app 的工作列圖示上。 </p>

<p>「快顯通知」<em></em>是您的 App 透過名為 <em>toast</em> (或 <em>banner</em>) 的快顯 UI 元素傳送給使用者的通知。 無論使用者是否在您的 App 中，都可以看到通知。</p>
<p><em>推播通知</em>或<em>原始通知</em>是從 Windows 推播通知服務 (WNS) 或從背景工作傳送到您 app 的通知。 您的應用程式可以通知使用者有趣事發生 (透過徽章更新、磚更新或快顯通知) 以回應這些通知，或是以您所選擇的任何方式來回應。</p>

 
## <a name="tiles"></a>磚
| 文章 | 說明 |
| --- | --- |
| [建立磚](creating-tiles.md) | 自訂您的 App 的預設磚，並提供不同螢幕大小的資產。 |
| [App 圖示資產](app-assets.md) | 以各種形式出現在整個 Windows 10 作業系統的 App 圖示資產，好比通用 Windows 平台 (UWP) App 的名片。 這些指導方針詳細說明應用程式圖示資產出現在系統的何處，並提供如何建立最優美圖示的深入設計祕訣。 |
| [主要磚 API](primary-tile-apis.md) | 要求釘選 App 的主要磚，並檢查主要磚目前是否已釘選。 |
| [磚內容](create-adaptive-tiles.md) | 磚通知內容使用指定的彈性，可讓您設計您自己的新功能在 windows 10，磚通知內容使用配合不同螢幕密度調整的簡易靈活標記語言。 本文告訴您如何為通用 Windows 平台 (UWP) app 建立調適型動態磚。 |
| [磚內容結構描述](../tiles-and-notifications/tile-schema.md) | 以下是用來建立調適型磚的元素和屬性。 |
| [特殊的磚範本](special-tile-templates-catalog.md) | 特殊磚範本是獨特的範本，這些範本不是呈現動畫，就不過是讓您可以執行無法透過調適型磚達成的工作。 |
| [傳送本機磚通知](sending-a-local-tile-notification.md) | 了解如何傳送本機磚通知，並將豐富的動態內容新增至動態磚。 |


## <a name="notifications"></a>通知

| 文章 | 描述 |
| --- | --- |
| [快顯通知](adaptive-interactive-toasts.md) | 調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像及選擇性使用者互動的彈性快顯通知。 |
| [傳送本機快顯通知](send-local-toast.md) | 了解如何傳送互動式快顯通知。 |
| [通知視覺化工具](notifications-visualizer.md) | 通知視覺化工具是新的通用 Windows 平台 (UWP) 應用程式，[在市集中](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)，可協助開發人員設計調適型中適用於 windows 10 動態磚。 |
| [選擇通知傳遞方法](choosing-a-notification-delivery-method.md) | 本文探討四個傳遞磚和徽章更新及快顯通知內容的通知選項：本機、排程、定期和推播。 |
| [定期通知概觀](periodic-notification-overview.md) | 定期通知也稱為輪詢通知，可以在固定的時間間隔從雲端服務下載內容來更新磚和徽章。 |
| [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md) | Windows 推播通知服務 (WNS) 可以讓協力廠商開發人員從自己的雲端服務傳送快顯通知、磚、徽章和原始更新。 這提供一種機制，用省電又可靠的方法，將最新的更新資訊傳送給使用者。 |
| [由推播通知精靈產生的程式碼](the-code-generated-by-the-push-notification-wizard.md) | 您可以藉由 Visual Studio 中的精靈，從利用 Azure 行動服務建立的行動服務產生推播通知。 Visual Studio 精靈會產生程式碼，協助您開始使用。 這個主題說明精靈如何修改您的專案、所產生的程式碼如何作用、如何使用此程式碼，以及如何進一步充分利用推播通知。 請參閱 [Windows 推播通知服務 (WNS) 概觀](windows-push-notification-services--wns--overview.md)。 |
| [原始通知概觀](raw-notification-overview.md) | 原始通知是簡短、一般用途的推播通知。 它們只是指示，不會包含 UI 元件。 正如其他推播通知一樣，WNS 功能會將原始通知從雲端服務傳送到您的應用程式。 |
