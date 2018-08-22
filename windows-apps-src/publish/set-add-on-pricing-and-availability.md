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
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2789444"
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

您必須選取基底的價格的附加元件 （除非您在 [**可見度**] 區段中選取**停止擷取**的選項）。 選取預設**空閒**，因此如果您想要收費 money 的附加元件，請務必要選擇一個可用的價格各層 （在.99 USD 開始）。

您也可以排程價格變更，表示附加元件的價格應變更的日期和時間。 此外，您可以選擇針對特定市場自訂這些變更。 

> [!TIP]
> 訂閱的附加元件，您不能引發價格之後選取較高的基底價格中更新送出或排程會增加價格價格變更將發佈的附加元件。 您可以選取較低價格使用其中一種方法，但之後降低價格您不可以引發高於這個新的價格。 因此，它是特別重要並且確定您選取 [訂閱附加元件的適當價格層。 

如需詳細資訊，請參閱[設定與排程應用程價格](set-and-schedule-app-pricing.md)。


## <a name="sale-pricing"></a>銷售定價

如果您想要以降低的價格提供附加內容一段有限的時間，您可以建立及排程銷售。 如需詳細資訊，請參閱[降價銷售應用程式與附加內容](put-apps-and-add-ons-on-sale.md)。



