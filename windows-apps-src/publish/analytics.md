---
author: jnHs
Description: "您可以在 Windows 開發人員中心儀表板中，檢視 app 的詳細分析。"
title: "分析"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
translationtype: Human Translation
ms.sourcegitcommit: dfaf348956b19746aa5332aeb7ad5cbc4b224e8c
ms.openlocfilehash: 8922a53da8b1bc97bef7faf49d0e412a26127188

---

# 分析

您可以在 Windows 開發人員中心儀表板中，檢視 app 的詳細分析。 統計資料和圖表讓您能夠了解 app 的受歡迎程度範圍 — 可從您已接觸到多少位客戶，到他們使用您 app 的方式以及他們給該 app 的評價。 您也可以找到 app 健康情況、廣告使用量及其他更多項目的相關資訊。 在儀表板中檢視報告，或者[下載所需的報告](download-analytic-reports.md)以便離線分析資料。 我們也提供數種讓您[不使用儀表板即可存取分析資料](#no-dashboard)的方法。

> **注意** &nbsp;&nbsp;除了儀表板的報告之外，您可透過 [Windows 市集分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md)，以程式設計的方式存取部分的分析資料。

## 分析您所有的 app


您的儀表板概觀頁面也包含可以用來蒐集關於您所有 app 詳細資料的縮合檢視。 概觀頁面上所顯示的統計資料會根據您的 app 而有所不同。

當您[下載分析報告](download-analytic-reports.md)時，您也可以選擇下載有關您所有 app 的報告。 請注意，您必須存取您其中一個 app 其 [分析]**** 小節的 [下載報告]**** 頁面，但並不限於只能下載該特定 app 的資料。

## 每個 app 的可用報告


在本節中，您將會找到下列每個報告所顯示的相關詳細資訊：

-   [取得次數報告](acquisitions-report.md)
-   [健康情況報告](health-report.md)
-   [評等報告](ratings-report.md)
-   [評論報告](reviews-report.md)
-   [意見反應報告](feedback-report.md)
-   [使用方式報告](usage-report.md)
-   [IAP 下載數報告](iap-acquisitions-report.md)
-   [廣告流量分配報告](ad-mediation-report.md)
-   [廣告績效報告](advertising-performance-report.md)
-   [App 安裝廣告報告](app-install-ads-reports.md)
-   [通道與轉換報告](channels-and-conversions-report.md)

> **注意** &nbsp;&nbsp;您可能無法在所有報告中都能夠看到資料，這會依據您 app 的特定功能和實作而定。

## 頁面與區段篩選

每個報告都會包含您可用來深入探索資料的篩選。 您在接近頁面頂端的地方會看到 [套用篩選]****。 您可以使用這類篩選來限制或擴展所有圖表的範圍以及頁面上的資訊。

在每個特定圖表內，您也可能會看見個別的 [區段篩選]。 這些可以限制資料，僅顯示符合該特定圖表的資料。

特定篩選會依報告而有所不同。 本節中的主題將說明可以使用的篩選，以及每個報告頁面上的其他資料。

<span id="no-dashboard"/>
## 不使用開發人員中心儀表板存取分析資料

除了在儀表板上的分析報告，還有數個其他方法可存取您的分析資料。

### Windows 市集分析 API

使用 [Windows 市集分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，以程式設計方式擷取 app 的分析資料。 這個 REST API 可讓您擷取 app 的資料和 IAP 的下載數、錯誤、app 評分與評論。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 app 或服務的呼叫。

### 適用於 Power BI 的 Windows 開發人員中心內容封裝

使用 [適用於 Power BI 的 Windows 開發人員中心內容封裝](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) 在 Power BI 探索及監視開發人員中心分析資料。 Power BI 是一項雲端型業務分析服務，可提供您業務資料的單一檢視。

使用下列資源來開始使用 Power BI 存取您的分析資料。

* [註冊 Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [了解如何使用 Power BI](https://powerbi.microsoft.com/guided-learning/)
* [了解如何使用適用於 Power BI 的 Windows 開發人員中心內容封裝，以連接到您的分析資料](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> **注意** &nbsp;&nbsp;若要連接到適用於 Power BI 的 Windows 開發人員中心內容封裝，我們建議您從與您的開發人員中心帳戶關聯的 Azure AD 目錄指定認證。 如果您使用您的 Microsoft 帳戶認證，您在 Power BI 中的分析資料不會自動重新整理，且您將需要登入 Power BI 來重新整理資料。 如果您的組織已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD。 否則，您可以[免費取得它](http://go.microsoft.com/fwlink/p/?LinkId=703757)。 如需如何關聯您的開發人員中心帳戶與 Azure AD 的詳細資訊，請參閱[管理帳戶使用者](manage-account-users.md)。

### 開發人員中心 App

安裝 [開發人員中心](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) app，以在任何 Windows 10 裝置上快速檢視 app 的健康狀況與效能的詳細資料。 



<!--HONumber=Jun16_HO4-->


