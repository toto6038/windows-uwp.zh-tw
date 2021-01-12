---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: Windows SDK、Microsoft Advertising SDK、Microsoft Store Services SDK 及 Microsoft Store 提供許多功能，可讓您透過應用程式賺更多的錢，並讓客戶透過吸引您的使用者來獲利。
title: 營利、參與和 Microsoft Store 服務
ms.date: 11/29/2017
ms.topic: article
keywords: Windows 10, UWP, 營利, 參與, 促銷, Microsoft Store 服務
ms.localizationpriority: medium
ms.openlocfilehash: 29dcc02375358c7f16f38748ca92dbaeaf3ebdb7
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104699"
---
# <a name="monetization-engagement-and-store-services"></a>營利、參與和 Microsoft Store 服務

Windows SDK、Microsoft Advertising SDK、Microsoft Store Services SDK 及 Microsoft Store 提供許多的功能，可讓您透過與使用者互動，從您的應用程式賺更多的錢並獲得客戶。 本節中的主題說明如何在您的應用程式中建置這些功能。

如需深入了解 Microsoft Store 收取的費用，以及取得應用程式銷售款項的方法，請參閱[獲得報酬](/partner-center/marketplace-get-paid)。

## <a name="choose-a-pricing-model"></a>選擇計價模式

:::row:::
    :::column:::
        ![就您的應用程式收費](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**就您的應用程式收費**

您可以就您的應用程式預先收費。 我們支援多樣化的價格區間，包括設定各個市場價格的選項。 您甚至可以排定促銷活動，限時降價銷售您的應用程式。

[為應用程式定價](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![免費試用](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**免費試用**

您可以提供應用程式的免費試用版，讓更多客戶試用。 為了吸引客戶購買完整版，您可以限制試用版的功能 (例如遊戲只能玩第一關)、顯示廣告，或是指定限時試用。

[提供試用優惠](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![應用程式內建購買功能](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**在應用程式內購買**

無論您是就應用程式收費或提供免費試用，您的應用程式都可利用 App 內購買的方式持續賺取營收。 透過 App 內購買讓客戶從免費版的應用程式升級至付費版本，或由您的應用程式內提供耐久性或消費性附加元件讓客戶加購。

[使用應用程式內購買](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>利用廣告從您的應用程式獲利

:::row:::
    :::column:::
        ![適合各種情境的廣告](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**適合各種情境的廣告**

我們支援各式各樣符合大多數人需求的廣告體驗，包括橫幅廣告、插播式廣告 (橫幅與影片)、線性影片廣告、播放式廣告以及原生廣告。 我們的平台符合 OpenRTB、VAST 2.x、MRAID 2 及 VPAID 3 等標準，而且與 MOAT 及 IAS 相容。

[探索廣告選項]()
[安裝廣告 SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![廣告流量分配服務](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**廣告流量分配服務**

利用 Microsoft 廣告流量分配服務，提供取材自各大廣告網路的廣告，讓您從應用程式內的廣告獲得最大收益。 您可以由合作夥伴中心進行流量分配設定，而無須更動任何程式碼。 如果您交給我們代為設定流量分配，我們的機器學習演算法將能協助您從應用程式支援的所有市場獲取最高的廣告收益。

[使用廣告服務](https://blogs.windows.com/windowsdeveloper/2017/05/08/announcing-microsofts-ad-mediation-service/)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![分析](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**分析**

詳細分析報告讓您了解應用程式內廣告的績效，並提供最大化廣告收益的所需資訊。 我們也提供一套 RESTful API，讓您能以程式設計方式取得這些資料。

[檢閱效能](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>其他獲利機會

![其他獲利機會](images/monetize-other-opportunities.png)

尋找其他能讓您提高獲利的方法嗎？ 請考慮採用下列選項。

 主題                | 描述                 |
|--------------------|-----------------------------|
| [Microsoft 聯盟計畫](https://www.microsoftaffiliates.com/) | 從您的應用程式、部落格、網頁或其他通訊設備連結至 Microsoft 產品以賺取佣金。 您可以連結至 Microsoft Store 中銷售的應用程式、遊戲、音樂、電影、硬體、配件及其他商品。
| [A/B 實驗](./run-app-experiments-with-a-b-testing.md) | 在您為所有使用者啟用變更前，以部分客戶為對象，由應用程式中執行 A/B 測試來衡量功能變更的有效性。
| [透過 Microsoft Store Services SDK 與客戶互動](microsoft-store-services-sdk.md) | Microsoft Store Services SDK 提供一些可讓您在應用程式中新增功能的程式庫和工具，以協助您吸引客戶。 這些功能包括目標式通知、A/B 測試，以及從您的應用程式啟動「意見反應中樞」。
| [從您的應用程式啟動意見反應中樞](launch-feedback-hub-from-your-app.md) | 在您的 UWP 應用程式中新增程式碼來將您的 Windows 10 客戶引導至「意見反應中樞」，以便他們在其中提交問題、建議及附議。 然後，在「合作夥伴中心」的[意見反應報告](../publish/feedback-report.md)中管理此意見反應。 這個功能需要 Microsoft Store Services SDK。 
| [設定您的應用程式以接收合作夥伴中心推播通知](configure-your-app-to-receive-dev-center-notifications.md) | 為您的 UWP 應用程式註冊通知通道，以便讓應用程式能夠接收[合作夥伴中心推播通知](../publish/send-push-notifications-to-your-apps-customers.md)，以及追蹤由推播通知產生的應用程式啟動率。 這個功能需要 Microsoft Store Services SDK。
| [記錄合作夥伴中心的自訂事件](log-custom-events-for-dev-center.md) | 記錄來自您 UWP 應用程式的自訂事件，然後在「合作夥伴中心」的[使用報告](../publish/usage-report.md)中檢閱這些事件。 這個功能需要 Microsoft Store Services SDK。
| [要求評分與評論](request-ratings-and-reviews.md) | 歡迎您的客戶透過程式設計方式顯示評分和評論 UI 來對您的應用程式進行評分或評論。
| [Microsoft Store 服務](using-windows-store-services.md) | 了解如何使用 RESTful API 來自動化提交至 Microsoft Store，存取您的應用程式的分析資料以及自動化與 Microsoft Store 相關的其他工作。
| [將零售示範 (RDX) 功能新增至您的應用程式](retail-demo-experience.md) | 在您的 Windows 應用程式中加入零售示範模式，讓在銷售場所試用電腦和裝置的客戶可以直接開始試用。

## <a name="monetization-analytics"></a>創造營收分析

![創造營收分析](images/monetize-analytics.png)

利用下列報告，追蹤您的應用程式在 Microsoft Store 內的成效表現。

- [支付摘要](/partner-center/payout-statement)
- [下載數報告](../publish/acquisitions-report.md)
- [附加元件下載數報告](../publish/add-on-acquisitions-report.md)
- [廣告績效報告](../publish/advertising-performance-report.md)
- [使用 REST API 取得分析資料](access-analytics-data-using-windows-store-services.md)
- [建立客戶區隔](../publish/create-customer-segments.md)
- [意見反應報告](../publish/feedback-report.md)
- [使用方式報告](../publish/usage-report.md)