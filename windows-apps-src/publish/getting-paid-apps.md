---
Description: 瞭解如何接收應用程式的付款、附加元件 (應用程式內產品) 和廣告收益。
title: 取得付款
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: windows 10, uwp, 付款, 應用程式銷售, 應用程式收益, 支出, Microsoft Store 費用, 支付保留, 百分比
ms.localizationpriority: medium
ms.openlocfilehash: 5e2b67984c43d799f0f4e3a1c6662b57bdc6ae9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167392"
---
# <a name="getting-paid"></a>取得付款
以下是關於接收您的應用程式、附加元件和廣告收益之付款的一些重要資訊。

> [!IMPORTANT]
> 您必須先 [設定您的付款帳戶，並填寫必要的稅務表單](setting-up-your-payout-account-and-tax-forms.md)，才能在 Microsoft Store 中收到應用程式銷售的款項。

> [!NOTE]
> 若正在尋找與支出相關的支援，包括設定支出帳戶、遺漏支出、暫停支出等，請在[這裡](https://developer.microsoft.com/windows/support)連絡支援人員。

## <a name="store-fee"></a>Store 費用

當您[註冊開發人員帳戶](https://developer.microsoft.com/store/register)時，會接受[應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)。 本合約說明當您在 Microsoft Store 銷售 App 時您與 Microsoft 之間的關係，其中包含 Microsoft 針對每筆銷售收取的 Microsoft Store 費用。

費用在[應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)中有正式的定義。 如果您有任何問題，請務必檢閱該文件。

Microsoft Store 費用適用於 Windows 市集收取的所有 App 銷售金額，包括附加元件。


## <a name="price-tiers"></a>價格區間

所選取的價格區間會在您選擇散佈應用程式的所有國家/地區設定[銷售價格](set-and-schedule-app-pricing.md#base-price)。 您也可以使用其他定價功能，例如[為不同市場的選擇不同的價格](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)或[降價促銷應用程式](put-apps-and-add-ons-on-sale.md)。

您可以免費提供應用程式，或是挑選一個客戶必須支付才能取得應用程式的價格。 價格區間起價為 .99 美元，並有額外增量 (1.09 美元、1.19 美元，依此類推)。 價格上漲時，價格區間之間的增量會增加。

> [!NOTE] 
> 這些價格區間也適用於您從應用程式內提供的所有附加元件。

每個價格區間在 Store 所提供的每個貨幣中都有對應的值。 我們使用這些值協助您在全球以可資比較的價格帶來銷售應用程式。 但是，由於匯率的變更，確切的銷售金額在各貨幣之間可能會略有不同。 匯率會每月計算一次。 根據您的交易發生時間，會套用適當的匯率。 在 [資料行] exchangeRate 和 exchangeRateDate 中，您的付款報告會分別指出其為強制執行的匯率和日期範圍。

您也有選項可以輸入您選擇以特定市場當地貨幣表示的自由格式價格。 當您這麼做時，除非您提交具有新價格的更新，否則不會調整價格 (即使轉換率變更)。 

請記住，您所選取的價格可能包括您的客戶必須支付的銷售或加值稅。 如需詳細資訊，請參閱[付費 app 的稅務詳細資料](tax-details-for-paid-apps.md)。


## <a name="payout-reporting"></a>付款報告

您可以存取有關付款資訊的詳細資料，並在[合作夥伴中心](https://partner.microsoft.com/dashboard)的 [支出摘要] 中下載報告。 如需此處所顯示的詳細資訊，以及我們將您所賺取金額分類的方式，請參閱[支付摘要](payout-summary.md)。


## <a name="payout-timeframe"></a>支付時間範圍

付款是每月進行一次 (假設已符合適用的付款門檻，而您尚未進行保留支付，如下所述)。 對於任何在指定月份到期的付款，我們一般會在當月的 15 號以前匯出。 請注意，付款通常需要 3 到 10 個額外的工作天，才能到達您的支出帳戶。 如需詳細資訊，請參閱[付款門檻、方法和時間範圍](payment-thresholds-methods-and-timeframes.md)。


##  <a name="payout-hold-status"></a>支出保留狀態

我們預設會每個月傳送付款，如上所述。 不過，您可以選擇保留計畫支出，這會避免我們匯款到您的帳戶。 如果您選擇保留支出，我們會繼續記錄您所獲得的任何收益，並在您的付款 **摘要**中提供詳細資料。 不過，除非您移除保留，否則我們不會將任何付款傳送到您的帳戶。

若要保留您的付款，請移至 [開發人員設定]。 在 [支出與稅務] 的 [發放款與稅金設定檔指派] 區段中，找出您想要保留付款的計畫。 按一下 [保留我的付款] 核取方塊，以保留此計畫的付款。 您可以隨時變更支出保留狀態，但請注意，您的決定會影響下一個每月支出。 例如，如果您想要保留四月的支出，請務必將您的付款保留狀態設為 **在** 三月月底之前。

一旦您將支出保留狀態設定為 [開啟]，則會保留此計畫的所有支出，直到您將滑桿切換回 [關閉] 為止。 這樣做時，在下一個每月支付週期將會包含您 (假設已符合適用的付款門檻)。 例如，如果您的支出已保留，但想要在六月產生支出，則請務必在5月底之前將付款保留狀態切換為 **關閉** 。

> [!NOTE]
> 您的**支出保留狀態**會個別套用至每個計畫 (Microsoft Store、廣告、Azure Marketplace 等等)。 如果您想要保存所有計畫的付款，您必須個別保留每個計畫的付款。


 

 