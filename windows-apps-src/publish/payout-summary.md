---
description: 付款報表會顯示您的應用程式和附加元件所獲得的金錢詳細資料。 也可以讓您了解何時會收到付款與付款金額。
title: 支出報告
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, 支付摘要, 聲明, 付款, 收入, 支付, 付帳, 收益
ms.localizationpriority: medium
ms.openlocfilehash: 996f77db55d959fb1e328e4e722166cc466a8dbd
ms.sourcegitcommit: d5ad11e2289d1d04319405a78dd02aaacf866e3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97390356"
---
# <a name="payout-reports"></a>支出報告

付款 **摘要** 會顯示您已使用 Microsoft 所獲得之錢的詳細資料。 也可以讓您了解何時會收到付款與付款金額。

如果您在 Azure Marketplace 銷售產品，您也會在付款 **摘要** 中看到成功支出的資訊。 如需 Azure Marketplace 付款的相關詳細資料，請參閱 [Microsoft Azure Marketplace 參與原則](/legal/marketplace/participation-policy)及 [Microsoft Azure Marketplace 發行者合約](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)。

> [!NOTE]
> 若要符合支付資格，收益必須達到美金 $50 元的[付款門檻](payment-thresholds-methods-and-timeframes.md)。 如需付款門檻的詳細資料，請參閱此頁面並檢閱應用程式開發人員合約。

> [!NOTE]
> 若正在尋找與支出相關的支援，包括設定支出帳戶、遺漏支出、暫停支出等，請在[這裡](https://developer.microsoft.com/windows/support)連絡支援人員。

## <a name="access-the-payout-summary-pages"></a>存取支出摘要頁面

開啟其中一個支出摘要頁面：

1. 選取位於右上角的支出圖示。
2. 選取 [交易記錄]、[付款] 或 [匯出資料]。

## <a name="transaction-history-page"></a>[交易記錄] 頁面

此頁面會顯示所有的個人收益，包括日期、類型和每個項目的收益。 您可以選取要查看的時段，也可以依註冊識別碼、程式、付款識別碼、收益類型、杠杆、估計的付款月份、調整和狀態進行篩選。 其中提供目前會計年度 (7 月 1 日到 6 月 30 日) 及前兩個會計年度的資料。

若要查看收益的詳細資料，請選取位於頁面右側的向下箭號。 這會顯示 [杠杆]、[收入金額]、[估計的付款月份] 和 [產品]。 如果因為某些原因而無法使用這項資料，但您需要存取它，請聯絡 [支援](https://developer.microsoft.com/windows/support)人員。 如果收益是調整的結果，而不是交易，則不會顯示產品欄位。 除非有調整，且已選取調整篩選，否則調整原因將不會顯示。

若要匯出此頁面上的任何交易資料，請選取 [匯出]，然後依照 [匯出資料] 頁面上的指示進行。 從 [交易歷程記錄] 頁面匯出的檔案會顯示交易貨幣中的資料、交易貨幣和美元的收益，以及付費的付費值（以貨幣為單位）。

## <a name="payments-page"></a>[付款] 頁面

此頁面上總計代表參與的所有計劃。 您可透過參與者識別碼、計劃、付款識別碼和收益類型進行篩選。 金額的單位是美金。 付款值也會以付款貨幣顯示。

| 區域                   | 描述                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| 今年總付款   | 您的所有程式的總支付金額（以美元為單位）。       |
| 下一個預估付款 | 您 (的下一次付款，即使有其他人即將) ，也會以美元為單位。 |
| 上一次付款           | 美元 (金額) 、方案名稱，以及最新款項的方案。           |
| 付款 (依來源)     | 過去12個月內依程式表示的付款金額（以美元為單位）。           |
| 付款               | 選取 [付費] 或 [擱置]，然後依您的需要排序。 如需特定付款的其他詳細資料，請選取 [檢視]。 若要下載付款匯款單的複本，請選取 [下載]。 請注意，交易記錄資料最多可能需要24小時才會出現，因此您可能不會立即看到相關聯的收益。 |

若要匯出此頁面上的任何資料，請選取 [匯出]，然後依照 [匯出資料] 頁面上的指示進行。

## <a name="payment-status"></a>付款狀態

| 收益狀態           | 原因                                                                                                                                      | 需要合作夥伴動作嗎？                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| 尚未處理              | 收益符合付款資格。 其會在獎勵計劃的計劃指南中所定義的冷卻期間內處於此狀態。 | 否                                                         |
| 即將付款                 | 付款訂單會在付款處理之前產生等待內部審核。                                                               | 否                                                         |
| 稅務發票正在暫止      | 您的稅務發票未完成或無效。                                                                                                  | 您需要更新稅務發票才能接收付款 |
| 檢閱期間遭到拒絕   | 評論期間的付款已遭到拒絕。                                                                                                     | 請連絡 [Microsoft 支援服務](https://developer.microsoft.com/windows/support)以取得詳細資料                      |
| 失敗                   | 付款因 Microsoft 系統錯誤而失敗。                                                                                         | 請連絡 [Microsoft 支援服務](https://developer.microsoft.com/windows/support)以取得詳細資料                      |
| 進行中              | 付款正在進行中。                                                                                                                 | 否                                                         |
| 付款不正確        | 付款 recouping 正在進行中。                                                                                                       | 否                                                         |
| 已傳送                     | 付款已傳送至您的銀行。                                                                                                     | 否                                                         |
| 正在重新處理             | 付款發生 Microsoft 系統錯誤，正在重新處理。                                                                  | 否                                                         |
| Reversed                 | 您的銀行已反轉付款，並將在下一個付款週期再次傳送。                                                     | 否                                                         |
| 稅務發票遭拒     | 稅務發票在檢閱期間遭到拒絕。 所有暫止的付款都將會暫停，直到稅務發票檢閱完成。                 | 請連絡 [Microsoft 支援服務](https://developer.microsoft.com/windows/support)以取得詳細資料                      |
| 正在檢閱稅務發票 | 正在檢閱稅務發票。 付款將會在稅務發票獲得核准後發出。                                   | 否                                                         |
| 已拒絕                 | 您的銀行拒絕付款。                                                                                                      | 請連絡銀行以了解詳細資料。                             |

## <a name="export-data-page"></a>[匯出資料] 頁面

遵循此頁面上的指示，匯出您要的資料。

注意：

- [匯出資料] 頁面不會自行重新整理。 您可能需要手動重新整理頁面，才能看到最新的資料。
- 篩選條件可能會導致無可用資料錯誤。 這可能表示您已將預設的時間週期保留在三個月，然後從該期間以外的收益中選取付款識別碼。 請展開時間間隔，然後再試一次。

## <a name="payments"></a>付款

![匯出付款](images/pc-export-payments.png)

此選項可為指定計劃、關聯稅務和彙總金額提供您在銀行內所收到的付款下載。 這份報告可用於許多合作夥伴中心計劃，因此有些資料行可能不適用於報告。 這些資料行標記如下。

| 資料行名稱              | 描述                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | 根據該計劃獲取收益的合作夥伴主要身分識別                                                                             |
| participantIDType        | 商店程式的獎勵計畫和賣方識別碼的程式識別碼                                                                |
| participantName          | 收益合作夥伴的名稱                                                                                                               |
| programName              | 獎勵/Microsoft Store 計劃名稱                                                                                                              |
| 收益                   | 該計劃/participantID 的付款貨幣收益金額                                                                       |
| earnedUSD                | 方案/participantID 的收益金額，單位為美金                                                                                      |
| withheldTax              | 方案/participantID 的付款貨幣收益金額中扣除的稅務金額                                                               |
| salesTax                 | 方案/participantID 付款貨幣中營業稅的總金額 (僅適用於獎勵計劃)                   |
| serviceFeeTax            | 方案/participantID 付款貨幣中的 serviceFeeTax 總金額 (僅適用於 Microsoft Store 計劃和 Azure Marketplace) |
| totalPayment             | 方案/participantID 以當地貨幣計算的總付款，排除扣除的稅額並包含營業稅 (若適用的話)   |
| currencyCode             | 付款貨幣代碼                                                                                                                      |
| paymentMethod            | 用來支付夥伴的方法，例如，電子銀行轉帳、點數注意事項                                                     |
| paymentID                | 付款的唯一識別碼。 此數位通常會顯示在您的 bank 語句中。  (僅適用于 SAP 付款)               |
| paymentStatus            | 付款狀態                                                                                                                            |
| paymentStatusDescription | 付款狀態的易記描述                                                                                                    |
| paymentDate              | 從 Microsoft 傳送付款時的日期                                                                                                      |

## <a name="transaction-history"></a>交易記錄

![匯出交易記錄](images/pc-export-transaction.png)

此選項可為您在 [交易記錄] 頁面中所看到的每一個收益明細項目提供下載，包含收益類型、日期、相關聯的交易金額、客戶、產品，以及其他適用計劃的交易詳細資料。

| 資料行名稱                    | 描述                                                                                                                              | 獎勵/Microsoft Store/Azure Marketplace 的適用性           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | 每一項收益的唯一識別碼                                                                                                       | 全部                                                            |
| participantId                  | 根據該計劃獲取收益的合作夥伴主要身分識別                                                                            | 全部                                                            |
| participantIdType              | 大多數是獎勵計劃的計劃識別碼，以及 Microsoft Store 計劃和 Azure Marketplace 的賣方識別碼                                          | 全部                                                            |
| participantName                | 收益合作夥伴的名稱                                                                                                              | 全部                                                            |
| partnerCountryCode             | 賺取合作夥伴的地點/國家/地區                                                                                                  | 全部                                                            |
| programName                    | 獎勵/Microsoft Store 計劃名稱                                                                                                             | 全部                                                            |
| transactionId                  | 交易的唯一識別碼                                                                                                    | 全部                                                            |
| transactionCurrency            | 進行原始客戶交易時所使用的貨幣 (不是合作夥伴位置的貨幣)                                     | 全部                                                            |
| transactionDate                | 交易的日期。 對於將許多交易貢獻給一項收益的計劃相當實用                                           | 全部                                                            |
| transactionExchangeRate        | 用來顯示對應交易美金金額的匯率日期                                                                 | 全部                                                            |
| transactionAmount              | 原始交易貨幣 (據以產生收益) 的交易金額                                              | 全部                                                            |
| transactionAmountUSD           | 以美金為單位的交易金額                                                                                                                | 全部                                                            |
| lever                          | 指出收益的商務規則                                                                                                  | 全部                                                            |
| earningRate                    | 套用到交易金額以產生收益的獎勵率                                                                      | 全部                                                            |
| quantity                       | 根據計劃而有所不同。 指出交易計劃的計費數量                                                            | 全部                                                            |
| quantityType                   | 表示數量的類型，例如，計費數量、MAU                                                                             | 全部                                                            |
| earningType                    | 指出其為收費、退款、合作行銷、銷售等。                                                                                          | 全部                                                            |
| earningAmount                  | 以原始交易貨幣為單位的收益金額                                                                                      | 全部                                                            |
| earningAmountUSD               | 以美金為單位的收益金額                                                                                                                    | 全部                                                            |
| earningDate                    | 收益的日期                                                                                                                      | 全部                                                            |
| calculationDate                | 在系統中計算收益的日期                                                                                            | 全部                                                            |
| earningExchangeRate            | 用來顯示對應美金金額的匯率                                                                                  | 全部                                                            |
| exchangeRateDate               | 用來計算 EarningAmount (以美金為單位) 的匯率日期                                                                                   | 全部                                                            |
| paymentAmountWOTax             | 在「僅傳送」付款中， (沒有稅金) 的付款金額                                                                 | 全部                                                            |
| paymentCurrency                | 由合作夥伴在付款設定檔中選擇的付款貨幣。 只會針對已傳送的付款顯示                                                   | 全部                                                            |
| paymentExchangeRate            | 使用 ExchangeRateDate 以付款貨幣計算 paymentAmountWOTax 的匯率                                            | 全部                                                            |
| claimId                        | 宣告的唯一識別碼                                                                                                              | 獎勵 - 僅限部分計劃                                |
| planId                         | 計劃的唯一識別碼                                                                                                               | 獎勵 - 僅限部分計劃                                |
| paymentId                      | 付款的唯一識別碼。 這個數字通常會顯示在銀行對帳單中                                                 | 僅限 SAP 付款                                              |
| paymentStatus                  | 付款狀態                                                                                                                           | 全部                                                            |
| paymentStatusDescription       | 付款狀態的易記描述                                                                                                   | 全部                                                            |
| customerId                     | 一律為空白                                                                                                                     | 僅限獎勵計劃 (例外：OEM) 及 Azure Marketplace |
| customerName                   | 一律為空白                                                                                                                     | 僅限獎勵計劃 (例外：OEM) 及 Azure Marketplace |
| partNumber                     | 一律為空白                                                                                                                     | 部分獎勵和 Microsoft Store 計劃及 Azure Marketplace        |
| productName                    | 連結至交易的產品名稱                                                                                                       | 全部                                                            |
| productId                      | 唯一產品識別碼                                                                                                                | Microsoft Store 與 Azure Marketplace                                    |
| parentProductId                | 唯一的父產品識別碼。 請注意：如果該交易沒有父系產品，則父系產品識別碼 = 產品識別碼。 | Microsoft Store 與 Azure Marketplace                                    |
| parentProductName              | 父產品的名稱。 請注意：如果該交易沒有父系產品，則父系產品名稱 = 產品名稱。   | Microsoft Store 與 Azure Marketplace                                    |
| productType                    | 產品類型 (例如應用程式、附加元件、遊戲等等)                                                                                        | Microsoft Store 與 Azure Marketplace                                    |
| invoiceNumber                  | 發票編號 (僅適用於 EA)                                                                                                  | 獎勵及 Azure Marketplace- 僅限部分計劃           |
| subscriptionId                 | 與客戶建立關聯的訂閱識別碼                                                                                         | 獎勵 - 僅限部分計劃                                 |
| subscriptionStartDate          | 訂閱開始日期                                                                                                                  | 獎勵 - 僅限部分計劃                                 |
| subscriptionEndDate            | 訂閱結束日期                                                                                                                    | 獎勵 - 僅限部分計劃                                 |
| resellerId                     | 轉銷商識別碼                                                                                                                      | 獎勵 - 僅限部分計劃                                 |
| resellerName                   | 轉銷商名稱                                                                                                                            | 獎勵 - 僅限部分計劃                                 |
| distributorId                  | 散發者識別碼                                                                                                                   | 獎勵 - 僅限部分計劃                                 |
| distributorName                | 散發者名稱                                                                                                                         | 獎勵 - 僅限部分計劃                                 |
| agreementNumber                | 合約編號                                                                                                                         | 獎勵 - 僅限部分計劃                                 |
| agreementStartDate             | 合約開始日期                                                                                                                     | 獎勵 - 僅限部分計劃                                 |
| agreementEndDate               | 合約結束日期                                                                                                                       | 獎勵 - 僅限部分計劃                                 |
| workload                       | 工作負載                                                                                                                                 | 獎勵 - 僅限部分計劃                                 |
| transactionType                | 交易類型 (例如購買、退貨、作廢、退款等等)                                                               | Microsoft Store 與 Azure Marketplace                                    |
| localProviderSeller            | 記錄的本地提供者/賣方                                                                                                          | 僅限 Microsoft Store                                                     |
| taxRemitted                    | 已繳交稅金的金額 (營業稅、使用稅或增值稅/貨物及服務稅)。                                                                                   | Microsoft Store 與 Azure Marketplace                                    |
| taxRemitModel                  | 負責繳交稅金 (營業稅、使用稅或增值稅/貨物及服務稅) 的合作對象。                                                                    | 僅限 Microsoft Store                                                     |
| storeFee                       | Microsoft 保留的金額，可讓應用程式或附加元件可在存放區中使用。                                           | 僅限 Microsoft Store                                                     |
| transactionPaymentMethod       | 交易使用的客戶付款方式 (例如，卡片、行動裝置電信業者帳單、PayPal 等)。                                | Microsoft Store 與 Azure Marketplace                                    |
| tpan                           | 指出協力廠商廣告網路                                                                                                     | Microsoft Store - 僅限廣告                                               |
| customerCountry                | 客戶國家/地區                                                                                                                         | Microsoft Store 與 Azure Marketplace                                    |
| customerCity                   | 客戶縣/市                                                                                                                            | Microsoft Store 與 Azure Marketplace                                    |
| customerState                  | 客戶州/省                                                                                                                           | Microsoft Store 與 Azure Marketplace                                    |
| customerZip                    | 客戶郵遞區號                                                                                                                 | Microsoft Store 與 Azure Marketplace                                    |
| purchaseTypeCode               | 一律為空白                                                                                                                     | 獎勵計劃 - CRI                                        |
| purchaseOrderType              | 一律為空白                                                                                                                     | 獎勵計劃 - CRI                                        |
| purchaseOrderCoverageStartDate | 一律為空白                                                                                                                     | 獎勵計劃 - CRI                                        |
| purchaseOrderCoverageEndDate   | 一律為空白                                                                                                                     | 獎勵計劃 - CRI                                        |
| programOfferingLevel           |                                                                                                                                          | 獎勵計劃 - CRI                                        |
| TenantID                       |                                                                                                                                          | 獎勵計劃                                             |
| externalReferenceId            | 計劃的唯一識別碼                                                                                                        | 直接付款計劃 (獎勵和 Microsoft Store)                      |
| externalReferenceIdLabel       | 唯一識別碼標籤                                                                                                                  | 直接付款計劃 (獎勵和 Microsoft Store)                      |

## <a name="historical-statements"></a>歷史對帳單

![匯出歷史對帳單](images/pc-export-statements.png)

2019 年 7 月 1 日以前的交易記錄會分開處理。 對帳單會使用下列欄位，而非目前的欄位。

> [!NOTE]
> 舊版交易記錄包含稱為「保留」的資料行，該資料行對應到新式交易記錄中的「收益」資料行，但其差別在於後者會排除所有狀態為「付款已傳送」的收益。

> [!NOTE]
> 篩選 (3M、>6 分鐘、12M 等 ) 不會套用至歷程 **記錄語句** 區段。

| 欄位名稱              | 描述                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 收益來源          | 您的收入來源，根據該筆交易發生的位置 (例如 Microsoft Store、Windows Phone 市集、 Windows 市集 8、廣告等)。                  |
| 訂單識別碼                | 唯一的訂單識別碼。 此識別碼可讓您識別購買交易與其各自的非購買交易 (如退款等)。 這兩者都會具備相同的訂單識別碼。 此外，如果是分割付款的情況 (單一購買使用多種付款方式)，這樣可讓您連結各購買交易。 |
| Transaction ID          | 唯一的交易識別碼。                                                                                                                                          |
| 交易日期時間   | 發生交易的日期和時間 (UTC)。                                                                                                                       |
| 父產品識別碼       | 唯一的父產品識別碼。 請注意：如果該交易沒有父系產品，則父系產品識別碼 = 產品識別碼。                                |
| Product ID              | 唯一的產品識別碼。                                                                                                                                              |
| 父產品名稱     | 父產品的名稱。 請注意：如果該交易沒有父系產品，則父系產品名稱 = 產品名稱。                                  |
| 產品名稱            | 產品的名稱。                                                                                                                                                    |
| 產品類型            | 產品類型 (例如應用程式、附加元件、遊戲等等)                                                                                                                       |
| 數量                | 當收入來源是商務用 Microsoft Store 時，Quantity 代表購買的授權數。 針對其他所有的收入來源，Quantity 一律為 1。 注意︰即使單一交易分成兩個行項目，因為使用兩個不同的付款方式，每個行項目會顯示數量 1。 |
| 交易類型        | 交易類型 (例如購買、退貨、作廢、退款等等)                                                                                              |
| 付款方式          | 交易使用的客戶付款方式 (例如，卡片、行動裝置電信業者帳單、PayPal 等)。                                                               |
| 國家/地區        | 發生該筆交易的國家/地區。                                                                                                                          |
| 本地提供者/賣方 | 記錄的當地提供者/賣方。                                                                                                                                        |
| 交易貨幣    | 交易的貨幣。                                                                                                                                            |
| 交易金額      | 交易的金額。                                                                                                                                              |
| 已繳交稅金            | 已繳交稅金的金額 (營業稅、使用稅或增值稅/貨物及服務稅)。                                                                                                                  |
| 淨收入            | 交易金額減去免稅額。                                                                                                                                   |
| Microsoft Store 費用               | Microsoft 保留做為在市集提供 App 或附加元件之費用的淨收入百分比。                                                      |
| 應用程式收入            | 淨收入減去市集費用。                                                                                                                                       |
| 已預繳稅金          | 代扣稅額的收入金額  (不包含在 **保留** 的 .csv 檔案中。 )                                                                                                 |
| 付款                 | 扣除任何適用已預繳所得稅之後的應用程式收入 (以交易貨幣顯示)  (不包含在 **保留** 的 .csv 檔案中。 )                                |
| FX 率                 | 用來將「交易貨幣」轉換為「付款貨幣」的外匯匯率。                                                                                         |
| 付款貨幣        | 付款給您時使用的貨幣。                                                                                                                                       |
| 已轉換付款       | 使用 FX 匯率轉換為付款貨幣的付款金額。                                                                                                         |
| 稅務匯款模型         | 負責繳交稅金 (營業稅、使用稅或增值稅/貨物及服務稅) 的合作對象。                                                                                                   |
| 資格日期時間   | 交易收入符合支出資格的日期和時間 (UTC)。 建立支付時，它會包含支付建立日期之前，具有資格日期時間的交易收益  (僅包含在 **保留的** .csv 檔案中)。 |
| Charges                 | 顯示 [交易金額] 欄位中所有彙總之收費詳細資料的明細 (只有 Azure Marketplace 包含；不包含在 **保留的** .csv 檔案中)。 |
