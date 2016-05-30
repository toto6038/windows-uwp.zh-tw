---
author: mijacobs
Description: 了解如何使用磚、徽章、快顯通知以及通知提供您 App 的進入點，並將使用者維持在最新狀態。
title: 磚、徽章及通知
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
---

# UWP App 的磚、徽章及通知




了解如何使用磚、徽章、快顯通知以及通知提供您 App 的進入點，並將使用者維持在最新狀態。

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><img src="images/tile-and-live-tile.png" alt="Breakdown of tile elements" /></td>
<td align="left"><p>每個 app 都會有一個磚。 App 在 [開始] 功能表上以<em>磚</em>的形式顯示。 您可以啟用不同的磚大小 (小、中、寬及大)。 您可以使用<em>磚通知</em>更新磚，以傳遞新的資訊 (例如新聞頭條或最新未讀郵件的主旨) 給使用者。 您可以使用<em>徽章</em>或<em>通知徽章</em>，以系統提供的字符或 1-99 的數字形式，提供狀態或摘要資訊。</p>
<p><em>快顯通知</em>是您的 App 透過名為 <em>toast</em> (或 <em>banner</em>) 的快顯 UI 元素傳送給使用者的通知。 無論使用者是否在您的 App 中，都可以看到通知。</p>
<p><em>推播通知</em>或<em>原始通知</em>是從 Windows 推播通知服務 (WNS) 或從背景工作傳送到您 app 的通知。 您的應用程式可以通知使用者有趣事發生 (透過徽章更新、磚更新或快顯通知) 以回應這些通知，或是以您所選擇的任何方式來回應。</p></td>
</tr>
</tbody>
</table>

 
## 磚 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[建立磚](tiles-and-notifications-creating-tiles.md)</p></td>
<td align="left"><p>自訂您的 App 的預設磚，並提供不同螢幕大小的資產。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)</p></td>
<td align="left"><p>彈性磚範本是 Windows 10 中的新功能，可讓您使用能夠適應不同螢幕密度的簡易靈活標記語言，設計專屬的磚通知內容。 本文會告訴您如何為通用 Windows 平台 (UWP) App 建立彈性動態磚。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[彈性磚結構描述](tiles-and-notifications-adaptive-tiles-schema.md)</p></td>
<td align="left"><p>以下是用來建立彈性磚的元素和屬性。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[特殊的磚範本](tiles-and-notifications-special-tile-templates-catalog.md)</p></td>
<td align="left"><p>特殊的磚範本是獨特的範本，它們可能具有動畫效果，或只是能讓您執行使用彈性磚無法達成的工作。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[App 圖示資產](tiles-and-notifications-app-assets.md)</p></td>
<td align="left"><p>以各種形式出現在整個 Windows 10 作業系統的 App 圖示資產，好比通用 Windows 平台 (UWP) App 的名片。 這些指導方針詳細說明應用程式圖示資產出現在系統的何處，並提供如何建立最優美圖示的深入設計祕訣。</p></td>
</tr>
</tbody>
</table>

## 通知


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[調適型和互動式快顯通知](tiles-and-notifications-adaptive-interactive-toasts.md)</p></td>
<td align="left"><p>調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[通知視覺化工具](tiles-and-notifications-notifications-visualizer.md)</p></td>
<td align="left"><p>通知視覺化檢視是[市集](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)中新的通用 Windows 平台 (UWP) app，可協助開發人員設計適用於 Windows 10 的彈性動態磚。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[選擇通知傳遞方法](tiles-and-notifications-choosing-a-notification-delivery-method.md)</p></td>
<td align="left"><p>本文涵蓋四個通知選項：本機、排程、定期和推播，它們會傳遞磚和徽章更新及快顯通知內容。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[傳送本機磚通知](tiles-and-notifications-sending-a-local-tile-notification.md)</p></td>
<td align="left"><p>本文說明如何使用彈性磚範本，將本機磚通知傳送到主要磚和次要磚。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[定期通知概觀](tiles-and-notifications-periodic-notification-overview.md)</p></td>
<td align="left"><p>定期通知也稱為輪詢通知，可以在固定的時間間隔從雲端服務下載內容來更新磚和徽章。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Windows 推播通知服務 (WNS) 概觀](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</p></td>
<td align="left"><p>Windows 推播通知服務 (WNS) 可以讓協力廠商開發人員從自己的雲端服務傳送快顯通知、磚、徽章和原始更新。 這提供一種機制，用省電又可靠的方法，將最新的更新資訊傳送給使用者。</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[由推播通知精靈產生的程式碼](tiles-and-notifications-the-code-generated-by-the-push-notification-wizard.md)</p></td>
<td align="left"><p>您可以藉由 Visual Studio 中的精靈，從利用 Azure 行動服務建立的行動服務產生推播通知。 Visual Studio 精靈會產生程式碼，協助您開始使用。 這個主題說明精靈如何修改您的專案、所產生的程式碼如何作用、如何使用此程式碼，以及如何進一步充分利用推播通知。 請參閱 [Windows 推播通知服務 (WNS) 概觀](tiles-and-notifications-windows-push-notification-services--wns--overview.md)。</p></td>
</tr>
<tr class="even">
<td align="left"><p>[原始通知概觀](tiles-and-notifications-raw-notification-overview.md)</p></td>
<td align="left"><p>原始通知是簡短、一般用途的推播通知。 它們只是指示，不會包含 UI 元件。 正如其他推播通知一樣，WNS 功能會將原始通知從雲端服務傳送到您的應用程式。</p></td>
</tr>
</tbody>
</table>

 

 

 






<!--HONumber=May16_HO2-->


