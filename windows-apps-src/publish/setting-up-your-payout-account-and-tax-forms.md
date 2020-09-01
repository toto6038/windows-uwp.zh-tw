---
Description: 若要從 Microsoft Store 中的應用程式銷售獲得金錢，您需要設定您的付款帳戶，並填寫必要的稅務表單。
title: 設定支出帳戶和稅賦形式
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 1/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b7f7209d77e05453126f885e37a251e8b6511e95
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172872"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>設定支出帳戶和稅賦形式

> [!NOTE]
> 若正在尋找與支出相關的支援，包括設定支出帳戶、遺漏支出、暫停支出等，請在[這裡](https://partner.microsoft.com/support)連絡支援人員。

若要從 Microsoft Store 中的應用程式銷售獲得金錢，您需要設定您的付款帳戶，並在 [合作夥伴中心](https://partner.microsoft.com/dashboard)中填寫必要的稅務表單。

如果您只打算列出免費 app (而不打算提供 app 內購買，或者不打算使用 Microsoft Advertising)，就不需要設定支付帳戶或填寫任何納稅申報表。 如果您稍後變更您的想法，並決定要銷售應用程式 (或附加元件) ，您可以設定您的付款帳戶，並在該時間填寫稅務表單。 您要先完成支付帳戶和稅務設定檔，才能提交任何付費 App 或附加元件。

> [!NOTE]
> 在[特定市場](account-types-locations-and-fees.md#developer-account-and-app-submission-markets)中，開發人員只能提交免費 App。 如果您的帳戶註冊於上述其中一個市場，就無法選擇設定支付帳戶。

[設定您的開發人員帳戶](opening-a-developer-account.md)之後，您必須執行兩件事，才能在 Microsoft Store 中銷售應用程式 (或附加元件) ：

- [填妥稅務表單](#tax-forms)
- [設定支付帳戶](#payout-account)

> [!NOTE]
> 如需深入了解取得 app 銷售款項的方法和時間，請參閱[獲得報酬](getting-paid-apps.md)。

## <a name="tax-forms"></a>稅務表單

### <a name="filling-out-your-tax-forms"></a>填寫您的稅務表單

首先必須建立稅務設定檔，並指派給您參與的計畫。 您可以完成下列步驟，以建立 Microsoft Store 的 *稅務設定檔* ：

- 指定您的居住國家/地區和國籍。
- 填妥適用的稅務表單。

您可以在合作夥伴中心以電子方式填妥並提交您的稅務表單；在大部分的情況下不需要列印郵寄任何表單。

> [!IMPORTANT]
> 不同的國家和地區有不同的稅務規定。 您必須繳交的稅金確切金額取決於您銷售應用程式的國家和地區。 請參閱[應用程式開發人員合約](/legal/windows/agreements/app-developer-agreement)，以了解 Microsoft 在哪些國家/地區為您代繳銷售和使用稅。 在其他國家/地區 (視您在何處註冊而定)，您可能必須直接到當地稅務機構繳納應用程式的銷售和使用稅。 此外，您收到的應用程式銷售收益，可能會視為收入而予以課稅。 強烈建議您洽詢您的國家或地區的相關授權單位，這可協助您為您的 Microsoft Store 開發人員活動判斷正確的稅務資訊。

1. 在 [合作夥伴中心](https://partner.microsoft.com/dashboard)中，選取右上角的 [ **帳戶設定** ] 圖示，然後選取 [ **開發人員設定**]。
2. 在左側導覽功能表中，選取 [支付與稅務]，然後選取 [支付與稅務指派]。

    ![付款和稅務設定檔指派](images/payout-tax-profile-assignment.png)

3. 選取您要設定稅務資訊的程式和賣方識別碼組合。

    ![付款選取賣方識別碼](images/payout-select-seller-id.png)

4. 如果您想要使用現有的稅務設定檔，請從下拉式清單中選取。 否則，請選取 [建立新的設定檔]，然後按下 [提交]。 如此即會進入稅務設定檔頁面。
5. 按一下 [編輯] 按鈕，以編輯您的稅務資訊。
6. 選取適當的選項按鈕，如果出現提示，請選取您的國家/地區。 此步驟會決定用來在您的帳戶上進行付款的 Microsoft 商務實體。

    ![支付選取的國家/地區](images/payout-select-tax-country.png)

7. 根據您在步驟6中的選取專案，系統會提示您提供您的國家/地區所需的稅務資訊。

> [!NOTE]
> 無論您的居住國家或公民國家/地區為何，都必須填寫美國稅務表單，以透過 Microsoft Store 銷售任何應用程式或附加元件。 符合特定美國居住規定的開發人員必須填寫 IRS W-9 表單。 美國以外的其他開發人員必須填寫 IRS W-8 表單。 您完成您的稅務設定檔時，可在線上填寫這些表單。

### <a name="withholding-rates"></a>預繳匯率

您在稅務表單中提交的資訊會決定適當的扣繳稅率。 此扣繳率僅適用於您在美國所進行的銷售，在非美國位置進行的銷售，扣繳規範則不適用。 扣繳率不盡相同，但對於大部分在美國境外註冊的開發人員，預設扣繳率為 30%。 如果您的國家/地區和美國已簽訂所得稅協定，您可以選擇降低此扣繳率。

### <a name="tax-treaty-benefits"></a>租稅協定減免優惠

如果您居住在美國境外，則可利用租稅協定減免優惠。 這些權益會因國家/地區而異，並可讓您減少 Microsoft Store 所 withholds 的稅金數量。 您可以填妥 W-8BEN 表單的第二部分，來申請租稅協定減免優惠。 建議向您所在國家或地區的合適資源洽詢，以判斷您是否適用這類權益。

> [!NOTE]
> 不需要美國的個人納稅人識別碼 (ITIN)，即可接受 Microsoft 的付款或要求享有稅務條約權益。

## <a name="payout-account"></a>支付帳戶

支付帳戶是我們支付您的銷售收入款所用的銀行帳戶。 可在 [設定檔] 頁面上查看您輸入的所有支付帳戶。

> [!NOTE]
> 在某些市場中，可使用 PayPal 做為支付帳戶。 請參閱 [付款閾值、方法和](payment-thresholds-methods-and-timeframes.md) 時間範圍，以瞭解特定市場是否支援 paypal，並閱讀以下的 [paypal 資訊](#paypal-info) 以取得詳細資料。

### <a name="create-a-payment-profile"></a>建立付款設定檔

1. 在 [合作夥伴中心](https://partner.microsoft.com/dashboard)中，選取右上角的 **設定** 齒輪圖示，然後選取 [ **開發人員設定**]。
2. 在 [支付與稅務] 標題底下，選取 [支付與稅務設定檔指派]。

    > [!NOTE]
    > 由於這是敏感性資訊，因此系統可能會提示您再次登入。

3. 選取您想要設定的付款方法。

    ![支付帳戶類型選擇](images/payout-account-type-selection.png)

4. 選取現有的付款設定檔，或按一下 [建立新的付款設定檔]，為所選的付款方法建立新的設定檔。

> [!NOTE]
> 如果基於某些原因，您的帳戶尚未準備好接收來自 Microsoft 的資金，您可以勾選 [ **保留我的付款** ] 核取方塊。 您將會繼續從您的銷售獲得持續，但不會散發付款，直到您停用 [**保留我的付款**] 為止。

### <a name="create-a-bank-based-payment-profile"></a>建立以銀行為基礎的付款設定檔

如果您選擇使用銀行帳戶來接收付款，需完成下列程序來設定您的銀行帳戶。

1. 在 [銀行設定檔] 頁面上，提供必要的銀行資訊。
2. 提供您的銀行帳戶詳細資料。

    > [!NOTE]
    > 您用來提供帳戶資訊的欄位只接受英數字元。

    ![付款銀行資訊](images/payout-bank-info.png)

3. 提供受益人詳細資料。
4. 返回 [設定檔指派] 頁面，選取您希望我們以何種貨幣匯付款項。

    > [!WARNING]
    > 請確定您的銀行接受您選取的付款貨幣。

5. 需要為您參與的每項計畫選取付款設定檔，但可以對多項計畫使用相同的設定檔。

    ![支出使用銀行設定檔](images/payout-use-bank-profile.png)

6. 按一下 [提交] 以儲存變更。

> [!NOTE]
> Microsoft 最多可能需要 48 小時的時間來驗證您的設定檔中的資訊。 此流程完成時，驗證狀態會顯示**完成**

為確保付款能夠成功，請記住下列內容：

- 在合作夥伴中心內輸入的支付帳戶**帳戶持有人**名稱，必須與您的銀行帳戶完全相同。 例如，如果您的銀行帳戶名稱包含中間名，請在您的**帳戶持有人名稱**加進中間名。
- 支出會以美元貨幣直接從 Microsoft 傳送到您的銀行帳戶。
- 在合作夥伴中心以拉丁字元輸入的銀行資訊會轉譯成斯拉夫文字元。

### <a name="editing-existing-payment-profiles"></a>編輯現有的付款設定檔

如果您需要進行變更，或需要更正任何不正確的資訊，可編輯現有的付款設定檔。

1. 在 [合作夥伴中心](https://partner.microsoft.com/dashboard)中，選取右上角的 **設定** 齒輪圖示，然後選取 [ **開發人員設定**]。
2. 在 [支付與稅務] 標題底下，選取 [支付與稅務設定檔]。
3. 您的付款設定檔會連同其狀態一起列出。 尋找您要編輯的設定檔，然後按一下最右側的 [編輯]

> [!IMPORTANT]
> 變更您的付款帳戶會延遲您的付款最多一次付款週期。 會發生此延遲是因為我們必須確認帳戶變更，就像我們在您第一次設定付款帳戶時所做的一樣。 您仍然會在帳戶確認之後收到全額款項。當期付款週期未付的款項會新增至下一期。 如需詳細資訊，請參閱[獲得報酬](getting-paid-apps.md)。

### <a name="paypal-info"></a>PayPal 資訊

在選取國家和地區時，可以輸入 PayPal 資訊來建立付款帳戶。 不過，在選擇 PayPal 做為付款帳戶之前，請選擇：

- 查看[付款閾值、方法和時間範圍](payment-thresholds-methods-and-timeframes.md)，確認 PayPal 是否屬於您所在國家/地區支援的付款方式。
- 請詳讀下列常見問題： 根據您的情況，PayPal 不一定是最適合您的帳戶選擇，可能會更推薦使用銀行帳戶。

使用 PayPal 做為付款方式的常見問題：

- **我需要如何 PayPal 設定才能接收款項？** 您必須確定您的 PayPal 帳戶並未封鎖 eCheck 付款。 您可以在 PayPal 的 Payment Receiving Preferences \(付款接收喜好設定\) 頁面管理此設定。 如需詳細資訊，請參閱 [PayPal 的帳戶設定頁面](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/) 。
- **我的國家/地區是否受支援？** 請參閱[付款閾值、方法和時間範圍](payment-thresholds-methods-and-timeframes.md)，確認可支援以 PayPal 付款的地點。
- **我的 PayPal 帳戶是否必須在與合作夥伴中心帳戶相同的國家/地區中註冊？** 否。 您設定 PayPal 帳戶時，可以接受預設設定。 接收來自其他國家/地區與其他貨幣的付款時，應該不會有問題，除非您已封鎖使用某些貨幣付款。 您可以在 PayPal 的 Payment Receiving Preferences \(付款接收喜好設定\) 頁面管理此設定。
- **我必須手動接受 PayPal 付款嗎？** 否。 PayPal 帳戶的預設設定會要求使用者手動接受付款，這表示若您未在 30 天內接受付款，該款項將被退回。 您可以在 PayPal 的 \[More Settings\] \(更多設定\) 頁面關閉 \[Ask Me\] \(詢問我\) 來變更此設定。
- **PayPal 支援哪些貨幣？** 請參閱 [PayPal 的支援頁面](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) ，以取得目前的清單

### <a name="specific-requirements-for-certain-countriesregions"></a>某些國家/地區的特別規定

在某些國家和地區，必須遵循支付帳戶的其他規定。 如果您是巴基斯坦、俄羅斯或烏克蘭的居民，請注意下列需求。

#### <a name="pakistan"></a>巴基斯坦

Form-R 是巴基斯坦銀行法規規定。 用於註明從國外接收款項的目的和原因。 因此只要您符合 Microsoft 的每月支付資格，就必須先向銀行提交 Form-R，才能將支付款項轉到您的帳戶。 如需如何取得 Form-R 的說明，請聯絡當地銀行分行。

符合付款資格的每個月都必須向銀行提交 Form-R。 例如，如果您預期會每個月收到付款，就必須提交 Form-R 12 次 (每個月一次)。

付款匯至您的銀行時，您有 30 天的時間可以提交 Form-R。 如果未在 30 天內提交，款項將會退回到 Microsoft。

#### <a name="russia"></a>俄羅斯

如果您是住在俄羅斯的開發人員，可能需要先提供文件給您的銀行，您的銀行才能將資金存入您的帳戶。 當您具備受款資格之後，我們將透過電子郵件為您提供下列資訊：

1. 接受憑證 (AC) – 包含要傳送至您帳戶的付款金額。
2. App 開發人員合約 (ADA) - 需要副簽的開發人員合約的簽署複本。

為確保付款能夠成功，請記住下列內容：

- 在合作夥伴中心內輸入的支付帳戶**帳戶持有人**名稱，必須與您的銀行帳戶完全相同。 例如，如果您的銀行帳戶名稱包含中間名，請在您的**帳戶持有人名稱**加進中間名。
- 付款會以盧布貨幣直接從 Microsoft 傳送到您的銀行帳戶。
- 在合作夥伴中心以拉丁字元輸入的銀行資訊會轉譯成斯拉夫文字元。
- 接受付款的對象必須是銀行帳戶，而不是銀行卡。

#### <a name="ukraine"></a>烏克蘭

如果您是住在烏克蘭的開發人員，可能需要先提供文件給您的銀行，您的銀行才能將資金存入您的帳戶。 當您具備受款資格之後，我們將透過電子郵件為您提供下列資訊：

1. 接受憑證 (AC) – 包含要傳送至您帳戶的付款金額。
2. App 開發人員合約 (ADA) - 需要副簽的開發人員合約的簽署複本。
3. 修訂合約 (AA) -您的銀行可以使用這份文件來協助識別您的付款資金。

您嘗試第一次付款時，Microsoft 會提供這三份文件。 至於後續的任何支付款項，您只會收到 AC 文件。 請保留 ADA 和 AA 文件，以防您需要它們來從您的銀行接收未來的支付款項。

### <a name="create-a-paypal-payment-profile"></a>建立 PayPal 付款設定檔

如果您選擇使用銀行帳戶來接收付款，需完成下列程序來設定您的銀行帳戶。

1. 在 [ PayPal] 頁面上，提供必要的 PayPal 帳戶資訊。
2. 請提供您的 paypal 帳戶詳細資料。

    > [!NOTE]
    > 您用來提供帳戶資訊的欄位只接受英數字元。

    ![支付 paypal 資訊](images/payout-paypal-info.png)

3. 提供受益人詳細資料。
4. 返回 [設定檔指派] 頁面，選取您希望我們以何種貨幣匯付款項。
5. 需要為您參與的每項計畫選取付款設定檔，但可以對多項計畫使用相同的設定檔。
6. 按一下 [提交] 以儲存變更。