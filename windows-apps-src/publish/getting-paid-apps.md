---
author: jnHs
Description: Learn about receiving payments for your apps, add-ons (in-app products), and advertising earnings.
title: 獲得報酬
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 付款, 應用程式銷售, 應用程式收益, 支出, Microsoft Store 費用, 支付保留, 百分比
ms.localizationpriority: medium
ms.openlocfilehash: 96845e81b093b7cddb6d334286e9cfa468a43b28
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5816831"
---
# <a name="getting-paid"></a>獲得報酬
以下是一些關於接收適用於您的應用程式、 附加元件，以及廣告營收之付款的重要資訊。

> [!IMPORTANT]
> 您可以接收來自 Microsoft Store 中的應用程式銷售的金額之前，您需要[設定支付帳戶](setting-up-your-payout-account-and-tax-forms.md)並填寫所需的納稅申報表。

## <a name="store-fee"></a>市集費用

當您[註冊開發人員帳戶](http://go.microsoft.com/fwlink/p/?LinkID=615100)時，會接受[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)。 本合約說明當您在 Microsoft Store 銷售 App 時您與 Microsoft 之間的關係，其中包含 Microsoft 針對每筆銷售收取的 Microsoft Store 費用。

在大部分情況下，市集費用為 30%。 費用在[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)中有正式的定義。 如果您有任何問題，請一律檢閱該文件。

Microsoft Store 費用適用於 Windows 市集收取的所有 App 銷售金額，包括附加元件。


## <a name="price-tiers"></a>價格區間

所選取的價格區間會在您選擇散佈應用程式的所有國家/地區設定[銷售價格](set-and-schedule-app-pricing.md#base-price)。 您也可以使用其他定價功能，例如[為不同市場的選擇不同的價格](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets)或[降價促銷應用程式](put-apps-and-add-ons-on-sale.md)。

您可以免費提供應用程式，或是挑選一個客戶必須支付才能取得應用程式的價格。 價格區間從 .99 美元開始，逐額遞增 (1.09 美元、1.19 美元等)。 價格區間之間的遞增值會隨著價格變高而增加。

> [!NOTE] 
> 這些價格區間也適用於您從應用程式內提供的所有附加元件。

每個價格區間對於市集所提供的貨幣都有對應值。 我們使用這些值協助您在全球以可資比較的價格帶來銷售應用程式。 不過，由於貨幣匯率隨時都在變化，因此確切的銷售金額可能會隨貨幣種類的不同而有些微差異。

您也有選項可以輸入您選擇以特定市場當地貨幣表示的自由格式價格。 這樣做時，除非您提交新價格的更新，否則不會調整價格 (即使轉換率變更)。 

請記住，您所選取的價格可能會包含客戶必須支付的銷售或增值稅。 如需詳細資訊，請參閱[付費 app 的稅務詳細資料](tax-details-for-paid-apps.md)。


## <a name="payout-reporting"></a>支付報告

您可以存取您付款資訊的詳細資料，並下載報告[合作夥伴中心](https://partner.microsoft.com/dashboard)**支付摘要**中。 如需此處所顯示的詳細資訊，以及我們將您所賺取金額分類的方式，請參閱[支付摘要](payout-summary.md)。


## <a name="payout-timeframe"></a>支付時間範圍

付款是每月進行一次 (假設已符合適用的付款門檻，而您尚未進行保留支付，如下所述)。 我們通常會在某個指定月份，於該月份的 15 日之前傳送任何付款。 請注意，付款通常需要 3 到 10 個額外工作天，才能送達您的支付帳戶。 如需詳細資訊，請參閱[付款門檻、方法和時間範圍](payment-thresholds-methods-and-timeframes.md)。


##  <a name="payout-hold-status"></a>支付保留狀態

我們預設會每個月傳送付款，如上所述。 不過，您可以選擇保留支付，讓我們無法將付款傳送到您的帳戶。 如果您選擇保留支付，我們會繼續記錄任何您賺到的收入，並在 [**支付摘要**] 中提供詳細資料。 不過，除非您移除保留，否則我們不會將任何付款傳送到您的帳戶。 

若要保留付款，請移至 [**帳戶設定**]。 在 [**財務詳細資料**] 的 [**支付保留狀態**] 區段中，將滑桿切換到 [**開啟**]。 您隨時都可以變更您的支付保留狀態，但是請注意您決定將會影響後續的每月支付。 例如，如果您想要保留年四月的支付，請務必在三月底之前將支付保留狀態設為 [**開啟**]。

將支付保留狀態設為 [**開啟**] 之後，除非將滑桿切換回 [**關閉**]，否則會保留所有支付。 這樣做時，在下一個每月支付週期將會包含您 (假設已符合適用的付款門檻)。 例如，如果您已經保留支付，但想要產生六月的支付，則請確定在五月底之前將支付保留狀態切換為[**關閉**]。

> [!NOTE]
> 您**支付保留狀態**選項套用至**所有**透過 Windows 開發人員計畫，在合作夥伴中心 (Microsoft Store、 廣告、 Azure Marketplace 等等) 付款的收入來源。 您無法針對個別收入來源選取不同保留狀態。


 

 




