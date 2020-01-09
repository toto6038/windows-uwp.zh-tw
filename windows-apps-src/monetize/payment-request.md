---
description: 付款要求 API 提供適用于 UWP 應用程式的整合式解決方案，以略過要求使用者輸入付款資訊，並選取出貨方法的流程。
title: 使用付款要求 API 簡化付款
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10，uwp，付款要求
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685039"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>使用付款要求 API 簡化付款
UWP 應用程式的付款要求 API 是以[W3C 付款要求 api 規格](https://w3c.github.io/browser-payment-api/)為基礎。它讓您能夠簡化 UWP 應用程式中的結帳流程。 使用者可以使用已隨 Microsoft 帳戶儲存的付款選項和寄送位址，以快速完成結帳。 您可以增加轉換率並降低資料缺口的風險，因為付款資訊已標記化。 從 Windows 10 建立者更新開始，使用者可以使用其已儲存的付款選項，輕鬆地在 UWP 應用程式的體驗中付費。

## <a name="prerequisites"></a>先決條件
開始使用付款要求 API 之前，您必須做幾件事或留意。

### <a name="getting-a-merchant-id"></a>取得商家識別碼
在付款要求程式期間，Microsoft 會代表您的服務提供者要求付款權杖。 因此，在您可以開始使用 API 之前，我們需要您的授權來要求這些權杖。  您必須遵循幾個步驟來註冊賣方帳戶，並提供必要的授權。 若要這麼做，請移至[Microsoft 賣方中心](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp)。 完成此動作之後，請在建立付款要求時，將產生的商家識別碼從合作夥伴中心複製到您的應用程式。 然後，當您的應用程式提交付款要求時，您會收到您的處理器所需的付款權杖，您必須提交付款。

### <a name="geographic-restrictions-and-language-support"></a>地理限制和語言支援
付款要求 API 只能供以美國為基礎的企業用來處理美國的交易。

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>在您的應用程式中使用付款要求 API：逐步解說
本節示範如何在您的應用程式中使用[UWP 付款要求 API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) 。 我們在這裡以最簡單的形式使用 API，是為了清楚起見。 如需更深入使用這些 Api 的範例，請參閱[GitHub 上的 UWP 購物應用程式範例](https://github.com/Microsoft/Windows-appsample-shopping)。

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. 建立一組您接受的所有付款選項。
> [!Note]
> 將**商家識別碼從賣方入口網站**文字取代為您從賣方中心收到的商家識別碼。

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. 同時提取付款詳細資料。 

這些詳細資料會在付款應用程式中向使用者顯示。 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. 包含銷售稅額。 

> [!Important]
> API 不會新增專案或計算您的銷售稅額。 請記住，稅務費率會因管轄區而異。 為了清楚起見，我們使用假設9.5% 的稅率。

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. （選擇性）將折扣或其他修飾詞新增至總計。 

以下範例說明如何使用特定的 Contoso 信用卡，將折扣新增至顯示專案。 （*Contoso*是虛構的名稱）。

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. 組合所有付款詳細資料。

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. 提交付款要求。 

呼叫**SubmitPaymentRequestAsync**方法以提交您的付款要求。 這會開啟付款應用程式，顯示可用的付款選項。

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

系統會提示使用者使用其 Microsoft 帳戶登入。

使用者登入之後，他們可以選取先前儲存的付款選項和寄送位址。

![付款要求 UI](./images/33.png "付款要求 UI")

您的應用程式會等待使用者按下 [**付費**]，然後完成訂單。

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

付款完成後，使用者會看到 [已**確認訂單**] 畫面。

![已確認訂單](./images/44.png "已確認訂單")

## <a name="see-also"></a>請參閱
- [ApplicationModel 付款參考檔](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [GitHub 上的 UWP 購物應用程式範例](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 付款要求 API 規格](https://www.w3.org/TR/payment-request/)
- [付款要求 API](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

