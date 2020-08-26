---
Description: 取得您的 Windows 應用程式、合作夥伴中心或其他方法的詳細分析。
title: 分析應用程式效能
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、uwp、分析、報表、儀表板、應用程式、資料、計量
ms.localizationpriority: medium
ms.openlocfilehash: 13cd8d08fb4c93bdcc273e84fbfdcfe986f28140
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846718"
---
# <a name="analyze-app-performance"></a>分析應用程式效能

您可以在 [合作夥伴中心](https://partner.microsoft.com/dashboard)中查看應用程式的詳細分析。 統計資料和圖表讓您能夠了解 App 的受歡迎程度範圍 — 可從您已接觸到多少位客戶，到他們使用您 App 的方式以及他們給該 App 的評價。 您也可以找到應用程式健康情況、廣告使用量等的計量。

您可以直接在合作夥伴中心中查看分析報表，或 [下載您需要的報表](download-analytic-reports.md) 以離線分析您的資料。 我們也提供數種方式，讓您在 [合作夥伴中心之外存取分析資料](#outside)。

## <a name="view-key-analytics-for-all-your-apps"></a>檢視您所有 App 的關鍵分析

若要檢視您最多下載的應用程式的關鍵分析，請展開 **\[分析\]** 並選取 **\[概觀\]**。 根據預設，概觀頁面會顯示生命週期內下載數最多的五個應用程式的資訊。 若要選擇顯示不同發行的應用程式，請選取 **\[篩選條件\]**。

## <a name="view-individual-reports-for-each-app"></a>檢視每個 app 的個別報告

在本節中，您將會找到下列每個報告所顯示的相關詳細資訊：

-   [下載數報告](acquisitions-report.md)
-   [附加元件下載數報告](add-on-acquisitions-report.md)
-   [使用方式報告](usage-report.md)
-   [健康情況報告](health-report.md)
-   [評分報告](ratings-report.md)
-   [評論報告](reviews-report.md)
-   [意見反應報告](feedback-report.md)
-   [Xbox 分析報告](xbox-analytics-report.md)
-   [見解報告](insights-report.md)
-   [廣告績效報告](advertising-performance-report.md)
-   [廣告行銷活動報告](promote-your-app-report.md)


> [!NOTE]
> 您可能無法在所有報告中都能夠看到資料，這會依據您應用程式的特定功能和實作而定。

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>存取合作夥伴中心之外的分析資料

除了在合作夥伴中心中查看報表之外，您還可以使用其他方式來存取應用程式分析。

### <a name="microsoft-store-analytics-api"></a>Microsoft Store 分析 API

使用 [Microsoft Store 分析 API](../monetize/access-analytics-data-using-windows-store-services.md)，可用程式設計方式擷取 App 的分析資料。 這個 REST API 可讓您擷取 App 的資料和附加元件下載數、錯誤、App 評分與評論。 這個 API 使用 Azure Active Directory (Azure AD) 來驗證您 App 或服務的呼叫。

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Power BI 的 Windows 開發人員中心內容套件

使用 [Power BI 的 Windows 開發人員中心內容套件](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) 來探索和監視 Power BI 中的合作夥伴中心分析資料。 Power BI 是一項雲端型業務分析服務，可提供您業務資料的單一檢視。

使用下列資源來開始使用 Power BI 存取您的分析資料。

* [註冊 Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [了解如何使用 Power BI](https://powerbi.microsoft.com/guided-learning/)
* [了解如何使用適用於 Power BI 的 Windows 開發人員中心內容封裝，以連接到您的分析資料](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> 若要連接到 Power BI 的 Windows 開發人員中心內容套件，建議您從與合作夥伴中心帳戶相關聯的 Azure AD 目錄中指定認證。 如果您使用您的 Microsoft 帳戶認證，您在 Power BI 中的分析資料不會自動重新整理，且您將需要登入 Power BI，才能重新整理資料。 如果您的組織已使用 Microsoft 365 或 Microsoft 的其他商務服務，您就已經有 Azure AD。 否則，您可以[免費取得它](https://account.azure.com/organization)。 如需設定關聯的詳細資訊，請參閱 [將 Azure Active Directory 與您的合作夥伴中心帳戶建立關聯](associate-azure-ad-with-dev-center.md)。
