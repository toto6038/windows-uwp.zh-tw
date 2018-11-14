---
author: jnHs
Description: The Feedback report in Partner Center lets you see the problems, suggestions, and upvotes that your Windows 10 customers have submitted through Feedback Hub.
title: 意見反應報告
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ab5e5f3fe533568079869c4fbd62530504544bf7
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6259027"
---
# <a name="feedback-report"></a>意見反應報告

合作夥伴中心中的**意見反應報告**可讓您查看問題、 建議及附議您的 Windows 10 客戶已透過意見反應中樞提交。 您可以在合作夥伴中心中檢視此資料，或匯出資料以便離線檢視。

> [!NOTE]
> 您也可以從此報告直接[回應意見反應](respond-to-customer-feedback.md)，讓客戶知道您正在聆聽。

鼓勵客戶提供有關您 App 的意見反應，是了解對他們而言最重要的問題和功能的不錯方式。 當客戶知道他們可以直接將意見反應直接傳送給您時，較不可能在市集中留下負面評論的意見反應。

您可以使用 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) 中的意見反應 API，讓客戶[直接從您的 App 啟動意見反應中樞](../monetize/launch-feedback-hub-from-your-app.md)。 請記住，在支援意見反應中樞的 Windows 10 裝置下載您 App 的所有客戶，都可以使用意見反應中樞 App 留下意見反應。 基於這個原因，您可能會看到此報告中的客戶意見反應，即使您未明確要求意見反應在您的應用程式內。

使用[套件正式發行前小眾](package-flights.md)，因為**意見反應**報告會顯示您每個客戶在留下意見反應時已安裝在其裝置的特定套件時，意見反應也會很有幫助。

> [!TIP]
> 如需快速查看評論、 評分，並在所有您的應用程式的使用者意見反應過去 30 天內，展開左側瀏覽功能表中的**互動**，然後選取**評論和意見反應。** 


## <a name="apply-filters"></a>套用篩選

在頁面頂端附近，您可以選取您想要顯示資料的時間週期。 預設選項是 **\[存留期\]**，但是您可以選擇顯示 30 天、3 個月、6 個月或 12 個月的資料。

您也可以展開 **\[篩選條件\]**，依以下選項篩選此頁面上的所有資料。

- **意見反應類型**：預設設定為 **\[全部\]**。 您可以選取 [**問題**] 或 [**建議**] 僅顯示該類型的意見反應。
- **裝置類型**：預設設定為 **\[所有裝置\]**。 如果您只想在此頁面上顯示使用該類型之客戶所給予的意見反應，則可以選擇特定裝置類型。
- **套件版本**：預設設定為 **\[所有套件\]**。 您可以選取其中一個套件，僅顯示在留下意見反應時使用該特定套件的客戶所留下的意見反應。
- **市場**：預設設定為 [**所有市場**]。 您可以選擇特定市場，僅顯示來自該市場中客戶的意見反應。
- **群組**：預設設定為 [**全部**]。 您可以選擇僅檢視 [Windows 測試人員](http://insider.windows.com)所提交的意見反應。

> [!TIP]
> 如果您在頁面上看不到任何意見反應，請檢查以確定您的篩選並未排除所有意見反應。 例如，如果您依應用程式不支援的 **\[裝置類型\]** 進行篩選，則看不到任何意見反應。


## <a name="viewing-feedback-details"></a>檢視意見反應詳細資料

在這份報告，您會看到您客戶留下的個人意見反應。 在每個項目的意見反應文字的左邊，您會看到其他客戶在意見反應中樞中附議該意見反應的次數。 您可以使用三種方式來排序意見反應︰

- **附議** (預設值)：顯示其他客戶已附議的意見反應，而收到最多附議的意見反應顯示在最前面。
- **新鮮貨**：顯示其他客戶在最後七天附議的意見反應，而取得最新活動的意見反應顯示在最前面。
- **最近**︰顯示所有意見反應，而最近留下的意見反應顯示在最前面。

在每個意見的旁邊，您會看到留下意見反應的日期，以及意見反應的類型。 您也會看到客戶的市場，留下意見反應，該裝置類型和**Windows 測試人員**如果提交意見反應的客戶是 Windows 測試人員的成員時所使用的裝置已安裝的特定套件計畫。

您也會在此看到[回應意見反應](respond-to-customer-feedback.md)的選項。


## <a name="translating-feedback"></a>翻譯評論

根據預設，不在您的慣用語言撰寫的意見反應是幫您翻譯。 您也可以取消核取頁面篩選附近的 **\[翻譯意見反應\]** 核取方塊，停用意見反應翻譯的功能。

請注意：意見反應是由系統自動翻譯，結果不一定正確。 我們也提供原文，供您與翻譯比較，或透過其他方式翻譯。


## <a name="launching-feedback-hub-directly-from-your-app"></a>從您的應用程式直接啟動意見反應中樞

如前所述，建議直接在您的應用程式中納入意見反應中樞的連結，鼓勵客戶提供意見反應。 如需詳細資訊，請參閱[從您的應用程式啟動意見反應中樞](../monetize/launch-feedback-hub-from-your-app.md)。
