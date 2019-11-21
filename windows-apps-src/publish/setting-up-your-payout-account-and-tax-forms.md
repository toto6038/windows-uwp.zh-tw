---
Description: In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms.
title: 設定您的支出及納稅申報表
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a56630a0a2f0acdc71241ac0234cad463e45ace
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259904"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>設定您的支出及納稅申報表

In order to receive money from app sales in the Microsoft Store, you need to set up your payout account and fill out the necessary tax forms in [Partner Center](https://partner.microsoft.com/dashboard).

如果您只打算列出免費 app (而不打算提供 app 內購買，或者不打算使用 Microsoft Advertising)，就不需要設定支付帳戶或填寫任何納稅申報表。 If you change your mind later and decide you do want to sell apps (or add-ons), you can set up your payout account and fill out tax forms at that time. 您要先完成支付帳戶和稅務設定檔，才能提交任何付費 App 或附加元件。

> [!NOTE]
> 在[特定市場](account-types-locations-and-fees.md#developer-account-and-app-submission-markets)中，開發人員只能提交免費 App。 如果您的帳戶在上述其中一個市場中註冊，就無法選擇設定支付帳戶。

After you have [set up your developer account](opening-a-developer-account.md), there are two things you need to do before you can sell apps (or add-ons) in the Microsoft Store:

- [Fill out your tax forms](#tax-forms)
- [Set up your payout account](#payout-account)

> [!NOTE]
> 如需深入了解取得 app 銷售款項的方法和時間，請參閱[獲得報酬](getting-paid-apps.md)。

## <a name="tax-forms"></a>納稅申報表

### <a name="filling-out-your-tax-forms"></a>Filling out your tax forms

First, you'll need to create a tax profile and assign it to the programs you participate in. You can create your *tax profile* for the Microsoft Store by completing the following steps:

- 指定您的居住國家/地區和國籍。
- 填寫適當的納稅申報表。

You can complete and submit your tax forms electronically in Partner Center; in most cases, you don't need to print and mail any forms.

> [!IMPORTANT]
> 不同國家和地區的稅金要求不同。 您必須繳交的稅金確切金額取決於您銷售應用程式的國家和地區。 請參閱[應用程式開發人員合約](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)，以了解 Microsoft 在哪些國家/地區為您代繳銷售和使用稅。 在其他國家/地區 (視您在何處註冊而定)，您可能必須直接到當地稅務機構繳納應用程式的銷售和使用稅。 此外，您收到的應用程式銷售收入可能會列為應稅收入。 We strongly encourage you to contact the relevant authority for your country or region that can best help you determine the right tax info for your Microsoft Store developer activities.

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Account settings** icon in the top right corner, then select **Developer settings**.
2. In the left navigation menu, select **Payout and tax**, then select **Payout and tax assignments**.

    ![Payout and tax profile assignment](images/payout-tax-profile-assignment.png)

3. Select the program and seller id combination for which you want to configure tax information.

    ![Payout select seller id](images/payout-select-seller-id.png)

4. If you would like to use an existing tax profile, select it from the dropdown. Otherwise, select **Create new profile** and press **Submit**. You will be taken to the tax profiles page.
5. Click the **Edit** button to edit your tax information.
6. Select the appropriate radio button, and select your country if prompted. This step determines the Microsoft business entity that will be used to make payouts on your account.

    ![Payout select tax country](images/payout-select-tax-country.png)

7. Depending on your selections in step 6, you will be prompted to provide tax information required for your country.

> [!NOTE]
> Regardless of your country of residence or citizenship, you must fill out United States tax forms to sell any apps or add-ons through the Microsoft Store. 符合特定美國居住規定的開發人員必須填寫 IRS W-9 表單。 美國以外的其他開發人員必須填寫 IRS W-8 表單。 完成稅金設定檔之後，您就可以在線上填寫這些表單。

### <a name="withholding-rates"></a>扣繳率

您在納稅申報表中送出的資訊將決定適當的稅金扣繳率。 扣繳率只適用於您在美國完成的銷售；非美國地區的銷售不適用扣繳率。 扣繳率視情況而有不同，但對於在美國以外地區註冊的大多數開發人員而言，預設扣繳率為 30%。 如果您的國家/地區和美國已簽訂所得稅協定，您可以選擇降低此扣繳率。

### <a name="tax-treaty-benefits"></a>稅務協定優惠

如果您在美國以外的地區，可能可以享有稅務協定優惠。 These benefits vary from country to country, and may allow you to reduce the amount of taxes that the Microsoft Store withholds. 您可以完成 W-8BEN 表單的 Part II 來要求稅務協定優惠。 建議您與您國家或地區的適當資源聯繫，以確認您是否適用這些優惠。

> [!NOTE]
> 向 Microsoft 收到付款或要求稅務協定優惠時不需要美國個人納稅識別號碼 (或 ITIN)。

## <a name="payout-account"></a>支付帳戶

支付帳戶是我們用來將銷售收益支付給您的銀行帳戶。 You can view all payment accounts that you have entered on the Profile page.

> [!NOTE]
> 在某些市場中，PayPal 可以用於支付帳戶。 See [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to find out if PayPal is supported for a specific market, and read the [PayPal info](#paypal-info) below for more details.

### <a name="create-a-payment-profile"></a>Create a payment profile

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Settings** gear icon in the top right corner, then select **Developer settings**.
2. Underneath the *Payout and tax* heading, select **Payout and tax profile assignment**.

    > [!NOTE]
    > 這是機密資訊，所以會提示您再次登入。

3. Select the payment method you would like to configure.

    ![Payout account type selection](images/payout-account-type-selection.png)

4. Select an existing payment profile, or click **Create a new payment profile** to create a new profile for the chosen payment method.

> [!NOTE]
> If, for some reason, your account is not ready to receive funds from Microsoft, you may check the **Hold my payment** checkbox. You will continue to earn proceeds from your sales, but payments will not be distributed until you disable **Hold my payment.**

### <a name="create-a-bank-based-payment-profile"></a>Create a bank-based payment profile

If you elected to use a bank account to receive payouts, you'll complete the following process to configure your bank account.

1. On the *Bank Profile* page, provide the required information about your bank.
2. Provide your bank account details.

    > [!NOTE]
    > 用來提供您帳戶資訊的欄位僅接受英數字元。

    ![Payout bank info](images/payout-bank-info.png)

3. Provide beneficiary details.
4. Back on the *Profile assignment* page, select the currency you would like us to use when we issue your payouts.

    > [!WARNING]
    > Make sure your bank accepts the payout currency you select.

5. You will need to select a payment profile for each program you participate in, though you can use the same profile for multiple programs.

    ![Payout use bank profile](images/payout-use-bank-profile.png)

6. Click submit to save your changes.

> [!NOTE]
> Microsoft may take up to 48 hours to validate the information in your profile. When this process is complete *verification status* will show **Complete**

為確保付款能夠成功，請記住下列內容：

- The **Account holder name** entered for your payout account in Partner Center must be the exact same name associated with your bank account. 例如，如果您的銀行帳戶名稱包含中間名，請在 **帳戶持有人姓名** 中加入中間名。
- 付款會直接從 Microsoft 轉入您的銀行帳戶，貨幣單位為美元 (USD)。
- Bank information entered in Partner Center in Latin characters is translated to Cyrillic characters.

### <a name="editing-existing-payment-profiles"></a>Editing existing payment profiles

You can edit existing payment profiles if you need to make changes or correct any incorrect information.

1. In [Partner Center](https://partner.microsoft.com/dashboard), select the **Settings** gear icon in the top right corner, then select **Developer settings**.
2. Underneath the *Payout and tax* heading, select **Payout and tax profiles**.
3. Your payment profiles will be listed along with their status. Find the profile you wish to edit and click **Edit** at the far right

> [!IMPORTANT]
> 變更支付帳戶會讓付款時間最多延遲一個付款週期。 之所以會發生延遲，是因為我們必須確認帳戶變更，正如我們在您首次建立支付帳戶時所做的一樣。 等到您的帳戶通過驗證後，仍然可以取得全額款項，目前付款週期到期的任何款項都會在下一個付款週期支付。 如需詳細資訊，請參閱[獲得報酬](getting-paid-apps.md)。

### <a name="paypal-info"></a>PayPal 資訊

在特定國家與地區，您可以透過輸入您的 PayPal 資訊來建立付款帳戶。 但是，請在選擇 PayPal 做為付款帳戶選項之前：

- Check [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to confirm whether PayPal is a supported payment method in your country or region.
- 檢閱下列常見問題集。 根據您的情況，PayPal 可能不是最適合您的付款帳戶選項，也許銀行帳戶才是最佳選項。

使用 PayPal 做為付款方式的常見問題：

- **What PayPal settings do I need to have in order to receive payments?** 您必須確定您的 PayPal 帳戶並未封鎖 eCheck 付款。 您可以在 PayPal 的 Payment Receiving Preferences \(付款接收喜好設定\) 頁面管理此設定。 如需詳細資訊，請參閱 [PayPal 的帳戶設定頁面](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/)。
- **Is my country/region supported?** See [Payment thresholds, methods, and timeframes](payment-thresholds-methods-and-timeframes.md) to find out where PayPal is a supported payment method.
- **Does my PayPal account have to be registered in the same country/region as my Partner Center account?** 不。 當您設定 PayPal 帳戶時，您可以接受預設設定。 接收來自其他國家/地區與其他貨幣的付款時，應該不會有問題，除非您已封鎖使用某些貨幣付款。 您可以在 PayPal 的 Payment Receiving Preferences \(付款接收喜好設定\) 頁面管理此設定。
- **Do I have to accept PayPal payments manually?** 不。 PayPal 帳戶的預設設定會要求使用者手動接受付款，這表示若您未在 30 天內接受付款，該款項將被退回。 您可以在 PayPal 的 \[More Settings\] \(更多設定\) 頁面關閉 \[Ask Me\] \(詢問我\) 來變更此設定。
- **What currencies does PayPal support?** Please see [PayPal's support page](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) for the current list

### <a name="specific-requirements-for-certain-countriesregions"></a>某些國家/地區的特定需求

在某些國家和地區，必須遵守支付帳戶的其他需求。 如果您是巴基斯坦、俄羅斯或烏克蘭的居民，請注意下列需求。

#### <a name="pakistan"></a>巴基斯坦

R 表格 (Form-R) 是巴基斯坦的金融規範需求。 它用於指示從海外接受款項的目的和原因。 因此，每當您有資格從 Microsoft 接受每月支付的時候，您必須在支付款項可匯入您的帳戶之前，提交 R 表格 (Form-R) 給您的銀行。 請連絡您當地的銀行分行，以獲得如何取得一份 R 表格 (Form-R) 的指示。

您有資格接受支付款項的每個月都必須提交 R 表格 (Form-R) 給您的銀行。 例如，如果您預期該年內的每個月都會收到支付款項，您就必須提交 12 次 (每個月一次) R 表格 (Form-R)。

當支付款項提交至您的銀行後，您有 30 天可以提交 R 表格 (Form-R)。 若未在 30 天內提交，該款項會退還至 Microsoft。

#### <a name="russia"></a>俄羅斯

如果您是住在俄羅斯的開發人員，可能需要先提供文件給您的銀行，您的銀行才能將資金存入您的帳戶。 當您具備受款資格之後，我們將透過電子郵件為您提供下列資訊：

1. 接受證書 (AC) - 包含轉帳到您帳戶的付款金額。
2. App 開發人員合約 (ADA) - 需要副簽的開發人員合約的簽署複本。

為確保付款能夠成功，請記住下列內容：

- The **Account holder name** entered for your payout account in Partner Center must be the exact same name associated with your bank account. 例如，如果您的銀行帳戶名稱包含中間名，請在 **帳戶持有人姓名** 中加入中間名。
- 付款會直接從 Microsoft 轉入您的銀行帳戶，貨幣單位為盧布 (RUB)。
- Bank information entered in Partner Center in Latin characters is translated to Cyrillic characters.
- 支付的款項會進入銀行帳戶，而不是銀行卡。

#### <a name="ukraine"></a>烏克蘭

如果您是住在烏克蘭的開發人員，可能需要先提供文件給您的銀行，您的銀行才能將資金存入您的帳戶。 當您具備受款資格之後，我們將透過電子郵件為您提供下列資訊：

1. 接受證書 (AC) - 包含轉帳到您帳戶的付款金額。
2. App 開發人員合約 (ADA) - 需要副簽的開發人員合約的簽署複本。
3. 增修條款合約 (AA) - 這份文件可協助您的銀行用來識別您的支付款項。

嘗試支付您的第一筆款項時，Microsoft 會提供所有三份文件。 針對任何後續的支付，您只會收到 AC 文件。 請保留 ADA 和 AA 文件，以防您需要它們來從您的銀行接收未來的支付款項。

### <a name="create-a-paypal-payment-profile"></a>Create a PayPal payment profile

If you elected to use a bank account to receive payouts, you'll complete the following process to configure your bank account.

1. On the *PayPal* page, provide the required information about your PayPal account.
2. Provide your paypal account details.

    > [!NOTE]
    > 用來提供您帳戶資訊的欄位僅接受英數字元。

    ![Payout paypal info](images/payout-paypal-info.png)

3. Provide beneficiary details.
4. Back on the *Profile assignment* page, select the currency you would like us to use when we issue your payouts.
5. You will need to select a payment profile for each program you participate in, though you can use the same profile for multiple programs.
6. Click submit to save your changes.