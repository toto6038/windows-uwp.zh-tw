---
author: shawjohn
Description: "您可以在 Windows 開發人員中心儀表板中，檢視應用程式的詳細分析。"
title: "分析"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 分析, 報表, 儀表板, 應用程式"
translationtype: Human Translation
ms.sourcegitcommit: b01924366a0bc2afabe2f381e72e45862f0dd682
ms.openlocfilehash: 13a37a4ae2cea67fdce843ed4e6189797d85b93e
ms.lasthandoff: 02/08/2017

---

# <a name="analytics"></a>分析

您可以在 Windows 開發人員中心儀表板中，檢視應用程式的詳細分析。 統計資料和圖表讓您能夠了解 App 的受歡迎程度範圍 — 可從您已接觸到多少位客戶，到他們使用您 App 的方式以及他們給該 App 的評價。 您也可以找到 App 健康情況、廣告使用量等等的相關資訊。 在儀表板中檢視報告，或者[下載所需的報告](download-analytic-reports.md)以便離線分析資料。 我們也提供數種讓您[不使用儀表板即可存取分析資料](#no-dashboard)的方法。

> [!NOTE]
> 除了儀表板的報告之外，您還可以透過 [Windows 市集分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md)，以程式設計的方式存取部分的分析資料。

## <a name="analytics-for-all-your-apps"></a>您所有 App 的分析

若要檢視您下載的多數 app 的關鍵分析，請選取頂端瀏覽功能表中的 **\[分析\]** > **\[概觀\]**。 根據預設，**\[分析概觀\]** 頁面會顯示生命週期內下載數最多的五個 app 的資訊。 若要選擇顯示不同的應用程式，請選取 **\[變更篩選\]**。

## <a name="available-reports-for-each-app"></a>每個 app 的可用報告

在本節中，您將會找到下列每個報告所顯示的相關詳細資訊：

-   [取得次數報告](acquisitions-report.md)
-   [附加元件下載數報告](add-on-acquisitions-report.md)
-   [安裝報告](installs-report.md)
-   [使用量報告](usage-report.md)
-   [健康情況報告](health-report.md)
-   [評分報告](ratings-report.md)
-   [評論報告](reviews-report.md)
-   [意見反應報告](feedback-report.md)
-   [通道與轉換報告](channels-and-conversions-report.md)
-   [廣告流量分配報告](ad-mediation-report.md)
-   [廣告效益報告](advertising-performance-report.md)
-   [聯盟效益報告](affiliates-performance-report.md)
-   [宣傳您的應用程式報告](promote-your-app-report.md)

> [!NOTE]
> 您可能無法在所有報告中都能夠看到資料，這會依據您應用程式的特定功能和實作而定。

## <a name="page-and-section-filters"></a>頁面與區段篩選

每個報告都會包含您可用來深入探索資料的篩選。 您在接近頁面頂端的地方會看到 **\[套用篩選\]**。 您可以使用這類篩選來限制或擴展所有圖表的範圍以及頁面上的資訊。

在每個特定圖表內，您也可能會看見個別的 [區段篩選]。 這些可以限制資料，僅顯示符合該特定圖表的資料。

特定篩選會依報告而有所不同。 本節中的主題將說明可以使用的篩選，以及每個報告頁面上的其他資料。

<span id="no-dashboard"/>
## <a name="access-analytics-data-without-using-the-dev-center-dashboard"></a>不使用開發人員中心儀表板存取分析資料

除了在儀表板上的分析報告，還有數個其他方法可存取您的分析資料。

### <a name="windows-store-analytics-api"></a>Windows 市集分析 API

使用 [Windows 市集分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，以程式設計方式擷取 App 的分析資料。 這個 REST API 可讓您擷取 App 的資料和附加元件下載數、錯誤、App 評分與評論。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>適用於 Power BI 的 Windows 開發人員中心內容封裝

使用 [適用於 Power BI 的 Windows 開發人員中心內容封裝](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) 在 Power BI 探索及監視開發人員中心分析資料。 Power BI 是一項雲端型業務分析服務，可提供您業務資料的單一檢視。

使用下列資源來開始使用 Power BI 存取您的分析資料。

* [註冊 Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [了解如何使用 Power BI](https://powerbi.microsoft.com/guided-learning/)
* [了解如何使用適用於 Power BI 的 Windows 開發人員中心內容封裝，以連接到您的分析資料](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> 若要連接到適用於 Power BI 的 Windows 開發人員中心內容封裝，我們建議您從與您的開發人員中心帳戶關聯的 Azure AD 目錄指定認證。 如果您使用您的 Microsoft 帳戶認證，您在 Power BI 中的分析資料不會自動重新整理，且您將需要登入 Power BI，才能重新整理資料。 如果您的組織已經使用 Office 365 或其他 Microsoft 所提供的商務服務，您就已經具備 Azure AD。 否則，您可以[免費取得它](http://go.microsoft.com/fwlink/p/?LinkId=703757)。 如需如何關聯您的開發人員中心帳戶與 Azure AD 的詳細資訊，請參閱[管理帳戶使用者](manage-account-users.md)。

### <a name="dev-center-app"></a>開發人員中心 App

安裝[開發人員中心](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) App，以便在任何 Windows 10 裝置上快速檢視 App 的健康狀況與效能的詳細資料。

## <a name="related-topics"></a>相關主題
- [發佈 Windows 應用程式](index.md)

