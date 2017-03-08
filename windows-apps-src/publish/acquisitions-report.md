---
author: jnHs
Description: "Windows 開發人員中心儀表板的 下載數報告讓您能夠查看已取得您應用程式的對象、人數統計資料及平台詳細資訊。"
title: "取得次數報告"
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a668c3d03c11ac4c6c27cddeefafeb3c42caf1e3
ms.lasthandoff: 02/07/2017

---

# <a name="acquisitions-report"></a>取得次數報告


Windows 開發人員中心儀表板的**下載數**報告讓您能夠查看已取得您應用程式的對象、人數統計資料及平台詳細資訊。 您可以在儀表板中檢視此資料，或[下載報告](download-analytic-reports.md)以便離線檢視。 或者，您可透過程式設計方式使用 [Windows 市集分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 中的[取得應用程式下載數](../monetize/get-app-acquisitions.md)方法來擷取此資料。

在此報告中，一個下載數表示一位新客戶已取得您 app 的授權 (不論是付費或免費取得)。

> **注意**：**下載數** 報告不包含有關退款、解除安裝、信用卡退款等相關資料。若要評估您的應用程式收益，請造訪[款項摘要](payout-summary.md)。 在 \[**保留**\] 區段中，按一下 [\**下載保留的交易**\] 連結。



## <a name="apply-filters"></a>套用篩選


您可以在接近頁面頂端的地方，展開 [\**套用篩選**\]，依日期範圍和/或裝置類型來篩選此頁面上的所有資料。

-   **日期**：預設篩選為 **[過去 30 天]**，但是您可以擴展此範圍，最多可達 **[過去 12 個月]**。
-   **裝置類型**：預設設定為 [**所有裝置**]。 如果您只想要顯示來自特定裝置類型的下載數資料，您可以在此處選擇特定的類型。

下列圖表中的資訊將反映在 \[**套用篩選**\] 中所選取的時段。

下列所有圖表中的資訊將反映 **\[套用篩選\]** 區段中選取的時段。 根據預設，除非您使用 \[**套用篩選**\] 只選擇一個裝置類型，否則這將包含所有裝置類型的資料。

## <a name="acquisitions"></a>下載數



            **下載數** 圖表會顯示在選取時段內，每日或每週您 app 的下載數。 (當您利用 \[**套用篩選**\] 篩選較長期間的資料時，資料將會以週為單位分組)。

您也可以看到您 app 生命週期內的下載數。 這會顯示所有下載數的累計總數 (從您 app 第一次發行開始)。

您可以選擇依市場和 (或) 作業系統版本篩選結果。

## <a name="customer-demographic"></a>客戶人數統計


**\[客戶人數統計\]** 圖表顯示取得您 app 對象的人數統計資訊。 您可以依特定年齡層和依性別來查看取得對象在選取時段內的下載數。

> **注意**：部分客戶選擇不分享此資訊。 如果我們無法判斷年齡層或性別，即會將該次下載數分類為 \[**未知**\]。

 

## <a name="markets"></a>市場


\[**市場**\] 圖表會依市場顯示選取時段內的下載數總數。 根據預設，您會在最上方看到最高下載數的市場，然後依序往下排序。 您也可以切換此圖表中 **下載數** 欄中的箭號來變更此順序。

## <a name="os-version"></a>作業系統版本


**\[作業系統版本\]** 圖表會根據客戶的作業系統 (或透過[組織大量取得](organizational-licensing.md)) 來顯示下載數總數。 在某些情況下，我們可能無法判斷此資訊。 如果發生此狀況，就會將作業系統版本列為 **\[未知\]**。



 

 

