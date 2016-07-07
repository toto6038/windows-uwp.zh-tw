---
author: jnHs
Description: "Windows 開發人員中心儀表板的 [使用方式] 報告可讓您查看客戶使用您 app 的方式。"
title: "使用方式報告"
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: 056642044953bab02f78912c7611ddcf5d6d48e6
ms.openlocfilehash: 476e7ee0c9c7ea7dce7f5e3a0389091ede9132c4

---

# 使用方式報告


Windows 開發人員中心儀表板中的**使用量** 報告可讓您查看客戶使用您 App 的方式，以及取得您所定義之自訂事件的相關資訊。 您可以在儀表板檢視此資料，或[下載報告](download-analytic-reports.md)以便離線檢視。

> **注意** 過去**使用量**報告只提供資料，如果您在您的 App 中啟用 Visual Studio Application Insights SDK。 有了更新的**使用量**報告，這不再需要。

## 套用篩選


您可以在接近頁面頂端處，展開 [套用篩選]****，依日期範圍和/或產品群組 (相關作業系統版本) 來篩選此頁面上的所有資料。

-   **日期**：預設篩選為 [過去 30 天]****，但是您可以擴展此範圍，最多可達 [過去 12 個月]****。
-   **套件版本**：預設設定為 [所有版本]****。 如果您的 App 包含多個套件，您可以在這裡選擇一個特定的版本。
-   **裝置類型**：預設設定是 [全部]****，但是您可以選擇只顯示一個特定裝置類型的資料。

下列所有圖表中的資訊將反映 [**套用篩選**] 中選取的時段。 根據預設，除非您只選擇一個套件版本來使用 [套用篩選]**** 區段，否則這將包含所有套件版本以及支援的裝置類型的資料。

## 使用者工作階段總計

[使用者工作階段總計]**** 圖表顯示在所選時段內，您 App 的每日使用者工作階段數目。

每個使用者工作階段代表一段客戶與您的 App 互動時的不同期間。 每個使用者工作階段會被視為在一段閒置時間後結束，因此單一客戶可以在同一天中有多個使用者工作階段。 請注意，此圖表無法分辨您 App 的單獨使用者。

## 作用中使用者

**作用中使用者**圖表顯示在所選時段內，在特定某一天使用您 app 的客戶數目。

每個作用中使用者都代表在那一天使用您 app 的客戶。 此圖表無法分辨單獨的使用者工作階段 (亦即，無論在該日客戶只使用一次您的 App 或使用多次，在此圖表中都只會呈現一個客戶)。

## 自訂活動

**自訂活動**圖表顯示您為 App 定義的任何自訂活動發生次數總計。 這可能包括同一客戶的多次活動。

自訂活動會在 [Microsoft Store Engagement and Monetization SDK](../monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) 中使用 [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) 方法實作。



 







<!--HONumber=Jun16_HO4-->


