---
author: mcleanbyron
Description: "您可以利用 Windows 開發人員中心儀表板，使用 A/B 測試執行通用 Windows 平台 (UWP) app 的實驗。"
title: "使用 A/B 測試執行 app 實驗"
ms.assetid: 790B4B37-C72D-4CEA-97AF-D226B2216DCC
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 88fd0516e3c10b657884b93377480b62c1758992

---

# 使用 A/B 測試執行 app 實驗

您可以利用 Windows 開發人員中心儀表板，使用 A/B 測試執行通用 Windows 平台 (UWP) app 的實驗。

在 A/B 測試中，您會透過開發人員中心在您的 app 中實驗程式變數指派。 藉由建立以 A/B 可測試程式變數為主的 app 邏輯，您可以對您的使用者群的隨機化區段啟用您 app 的變化。 A/B 測試的目的是要找出可能改善轉換率的變化 (例如，更多的應用程式內購買)。

在您找出最符合您的業務目標的變化之後，您可以立即結束實驗，並從開發人員中心儀表板對整個使用者群啟用該變化，而不必重新發佈您的應用程式。

## 建立和執行 A/B 測試

若要建立和執行 A/B 測試，請遵循下列步驟：

1. [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)。 每個實驗都包含下列項目︰
  * 「檢視事件」**，表示使用者開始檢視屬於實驗一部分的變化的時候。
  * 具有「轉換事件」**的一或多個目標，表示已達到目標的時候。
  * 一或多個「變化」**，定義您的實驗所使用的設定。
2. [編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)。 使用 Microsoft Store Engagement and Monetization SDK 中的 API 來取得實驗的變化設定、使用此資料來修改您測試的功能行為，以及將檢視事件和轉換事件傳送到開發人員中心。
3. [在開發人員中心儀表板中執行和管理您的實驗](manage-your-experiment.md)。 使用此儀表板來檢閱實驗的結果並完成實驗。

如需示範端對端處理程序的逐步解說，請參閱[使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)。

## 需求

只有 UWP app 支援在 Windows 開發人員中心進行 A/B 測試。

您必須先設定您的開發電腦，才可以使用 A/B 測試執行實驗︰

* 依照[這裡](../get-started/get-set-up.md)的指示，設定 UWP 開發用的開發電腦。
* 安裝 [Microsoft Store Engagement and Monetization SDK](http://aka.ms/store-em-sdk)。 除了實驗的 API，此 SDK 也會提供其他功能的 API，例如顯示廣告以及將您的客戶導向至意見反應中樞來收集有關您的 app 的意見反應。 如需這個 SDK 的詳細資訊，請參閱[利用 Store Engagement and Monetization SDK 讓您的 app 獲利](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)。

## 最佳作法

如需最實用的結果，我們建議您在使用 A/B 測試執行實驗時遵循下列建議︰

* 請考慮執行只有兩項變化的實驗，並以隨機化 50/50 分割分佈進行變化指派。
* 執行實驗至少 2-4 週，以收集具統計顯著性和可行動性的足夠資料。

## 相關主題

* [在開發人員中心儀表板中定義您的實驗](define-your-experiment-in-the-dev-center-dashboard.md)
* [編寫實驗用的 app 程式碼](code-your-experiment-in-your-app.md)
* [在開發人員中心儀表板中管理您的實驗](manage-your-experiment.md)
* [使用 A/B 測試建立和執行您的第一個實驗](create-and-run-your-first-experiment-with-a-b-testing.md)



<!--HONumber=Jun16_HO4-->


