---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: 設定附加內容價格與可用性
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 附加元件, iap, 價格
ms.localizationpriority: medium
ms.openlocfilehash: b5b7a6424fea3d62849e992f56b0b40ab72a55f5
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2018
ms.locfileid: "4052016"
---
# <a name="set-add-on-pricing-and-availability"></a>設定附加內容價格與可用性


提交附加內容時，在**價格與可用性**頁面上的選項會決定您的附加內容計費內容，以及提供給客戶的方式。

## <a name="markets"></a>市場

您的附加內容預設會以其基本價格列在所有可能的市場中，包括任何我們未來可能會新增的市場。

不過，就像應用程式一樣，您可以選擇您要提供附加內容的市場。 在大多數情況下，您應該會選擇與應用程式相同的一組市場，但是您還是能夠視需要做變更。 

如需詳細資訊和可用市場的完整清單，請參閱[定義市場選取項目](define-pricing-and-market-selection.md)。

## <a name="visibility"></a>可見度

您可以決定您的附加元件是否應提供給客戶購買。 

預設選項是 **\[可以在父產品的市集清單中顯示\]**。 對於可供任何客戶使用的附加元件，請維持勾選此選項。 

對於您不想要廣泛提供的附加元件，選取 **\[在市集中隱藏\]** 以及下列其中一個選項：

-   **僅限從父產品可供購買**：選擇此選項可讓任何客戶從 App 內購買附加元件，但是附加元件不會顯示在您的 App 市集清單中。 請只在項目尚未廣泛提供時才使用此選項，例如在內部測試初期期間。
-   **停止取得：擁有直接連結的任何客戶可以看到產品的市集清單，但除非他們已擁有該產品，或是有促銷碼而且使用 Windows 10 裝置，才能下載。此附加元件不會顯示在父產品的清單中**：選擇此選項表示附加元件不會顯示在您的應用程式清單中，而且也不讓新客戶購買附加元件。 不過，**Windows 8.1 或更舊版本的客戶不能使用此選項**。 如果在 Windows 8.1 或更舊版本上可以使用您的應用程式，這些客戶還是可以購買附加內容。 若要對 Windows 8.1 或更舊版本的客戶停止提供此附加內容，您就必須更新 App 以移除提供附加內容的程式碼，然後為 App 發佈新的提交作業。 即使您的 App 不在 Windows 8.1 或更舊版本上使用，還是建議您這麼做。因為如果您不要提供已選擇無法取得的附加內容，您的客戶會擁有比較好的體驗。
    
 > [!NOTE] 
 > 選擇 **\[停止取得\]** 選項並/或提交從 App 程式碼移除附加元件的 App 更新，無論使用的作業系統為何，都不會影響已經購買附加元件的客戶。


## <a name="schedule"></a>排程

根據預設，(除非您已選取 **\[可見度\]** 區段的其中一個 **\[在市集中隱藏\]** 選項)，您的附加元件將在通過認證並完成發行程序後，立即對客戶提供。 若要選擇其他日期，請選取 **\[顯示選項\]** 展開這個區段。 

如需詳細資訊，請參閱[設定精確發行時間表](configure-precise-release-scheduling.md)。


## <a name="pricing"></a>定價

（除非您在 [**可見度**\] 區段中選取 [**停止取得**] 選項），您必須針對您的附加內容選取基本價格。 預設選項是**免費**的所以，如果您想要收取附加元件的銷售款項的方法，請務必選擇其中一個可用的價格區間 （從.99 美元開始）。

您也可以排程價格變更，表示附加元件的價格應變更的日期和時間。 此外，您可以選擇針對特定市場自訂這些變更。 

> [!TIP]
> 對於訂閱附加元件，您無法提高的價格之後您發佈附加元件，在稍後的提交中選取較高的基本價格，或排程增加價格的價格變更。 您可以選取較低價格，使用這兩種方法，但價格降低之後您將無法引發高於該新的價格。 基於這個原因，它是特別重要，確定您選取的適當的價格區間的訂閱附加元件。 

如需詳細資訊，請參閱[設定與排程應用程價格](set-and-schedule-app-pricing.md)。


## <a name="sale-pricing"></a>銷售定價

如果您想要以降低的價格提供附加內容一段有限的時間，您可以建立及排程銷售。 如需詳細資訊，請參閱[降價銷售應用程式與附加內容](put-apps-and-add-ons-on-sale.md)。



