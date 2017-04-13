---
author: shawjohn
Description: "在 Windows 開發人員中心儀表板的安裝報告，可讓您查看您的應用程式在 Windows 10 裝置成功安裝的次數。"
title: "安裝報告"
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, 應用程式, 安裝, 安裝, 報告, 分析"
ms.assetid: 46c08fd2-00bd-4be5-b29f-01a3b5fea4c2
ms.openlocfilehash: 7912775e17a70c1d6fe9810c780017dcfa2db60e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="installs-report"></a>安裝報告

在 Windows 開發人員中心儀表板的**安裝**報告，可讓您查看客戶在 Windows 10 裝置上成功安裝您的應用程式的次數。 您可以在儀表板檢視此資料，或[下載報告](download-analytic-reports.md)來離線檢視。 或者，您可透過程式設計方式使用 [Windows 市集分析 REST API](../monetize/access-analytics-data-using-windows-store-services.md) 中的[取得 App 安裝](../monetize/get-app-installs.md)方法來擷取此資料。


## <a name="apply-filters"></a>套用篩選


您可以展開靠近頁面頂端的 **\[套用篩選\]**，依日期、裝置類型和/或套件版本來篩選此頁面上的所有資料。

-   **日期**：預設篩選為 **\[過去 30 天\]**，但是您可以擴展此範圍，最多可達 **\[過去 12 個月\]**。
-   **裝置類型**︰ 預設篩選是**所有裝置**，但您可以選取特定裝置類型 (**電腦**、**手機**、**平板電腦**、**虛擬機器**、**IoT**、**Holographic**、**主控台**、**其他**或**未知**)。
-   **套件版本**︰預設篩選是 **\[所有版本\]**，但您可以選擇特定的套件版本。


## <a name="installs-daily"></a>每日安裝


**\[每日安裝\]** 圖表顯示在所選時段內每日安裝應用程式的總數。

安裝總數包括︰
-   **在多部 Windows 10 裝置上安裝。** 例如，如果客戶在兩部 Windows 10 電腦和一支 Windows 10 手機上安裝您的應用程式，會算為三個安裝。
-   **重新安裝。** 例如，如果客戶今天安裝您的應用程式，明天解除安裝應用程式，然後在下個月重新安裝應用程式，便算成兩個安裝。

安裝總數不包括或反映︰
-   **在非 Windows 10 裝置上安裝。** 例如，如果客戶在未執行 Windows 10 的裝置上安裝應用程式，我們不計入此安裝。
-   **解除安裝。** 例如，如果客戶解除安裝您的應用程式，我們不會從總安裝數減去該次安裝。
-   **更新。** 例如，如果客戶今天安裝您的應用程式，然後在一星期後安裝應用程式更新，則只計入一次安裝 (不是兩部)。
-   **預先安裝。** 例如，如果客戶購買已預先安裝應用程式的裝置，我們不會將其計算為一次安裝。
-   **系統起始的安裝。** 例如，如果 Windows 因某些原因自動安裝您的應用程式，我們不會其算為一次安裝。

> **注意**目前您無法透過 API 以程式設計方式擷取**每日安裝**資料。

## <a name="markets"></a>市場


\[**市場**\] 圖表會依市場顯示選取時段內的安裝總數。 根據預設，我們會顯示所有市場的資料。 不過，您可以依照特定市集篩選此項。


## <a name="package-version"></a>套件版本


\[**套件版本**\] 圖表會依套件版本顯示所選時段內的安裝總數。



 

 
