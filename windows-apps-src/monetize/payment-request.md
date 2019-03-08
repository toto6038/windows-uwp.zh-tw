---
description: 付款要求 API 提供了整合的解決方案以略過的程序需要使用者輸入付款資訊，然後選取傳送方法的 UWP 應用程式。
title: 使用付款要求 API 簡化付款
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10 uwp，付款要求
ms.openlocfilehash: e5fb5cead7833b8cc213c6633cae6cee0da3466b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607863"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>使用付款要求 API 簡化付款
付款所要求的 API 用於 UWP 應用程式依據[W3C 付款要求 API 規格](https://w3c.github.io/browser-payment-api/)。它可讓您能夠簡化您的 UWP 應用程式中的簽出程序。 使用者可以透過簽出加快速度，藉由使用付款選項，以及送貨地址已儲存以 Microsoft 帳戶。 您可以增加轉換率，並減少資料外洩的風險，因為付款資訊會 token 化。 從 Windows 10 Creators Update 開始，使用者可以使用他們的已儲存的付款選項，輕鬆地支付跨 UWP 應用程式中的體驗。

## <a name="prerequisites"></a>必要條件
在您開始使用付款要求 API 之前，有幾件事，您必須執行或的注意。

### <a name="getting-a-merchant-id"></a>取得商家識別碼
付款要求程序的一部分，Microsoft 會從您的服務提供者要求代表您的付款語彙基元。 因此您可以開始使用 API 之前，我們需要您的授權，來要求這些權杖。  您必須遵循幾個步驟來註冊賣方帳戶，並提供必要的授權。 若要這樣做，請前往[Microsoft 賣方 Center](https://seller.microsoft.com/en-us/dashboard/registration/seller/?accountprogram=uwp)。 之後您已經這樣做，請將複製產生的 merchant ID，從您的應用程式建構付款要求時的合作夥伴中心。 然後，當您的應用程式提交付款要求時，您會收到付款的語彙基元從您提交您的付款，您將需要的處理器。

### <a name="geographic-restrictions-and-language-support"></a>地理限制和語言支援
付款要求 API 僅供美國為基礎的企業在美國境內的處理交易。

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>在您的應用程式中使用付款要求 API： 逐步解說
本節示範如何使用[UWP 付款要求 API](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)應用程式中。 我們在最簡單的形式，為求清楚起見使用的 API。 如需更進階使用這些 Api 的範例，請參閱 < [UWP 購物 GitHub 上的應用程式範例](https://github.com/Microsoft/Windows-appsample-shopping)。

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1.建立一份所有您所接受的付款選項。
> [!Note]
> 取代**merchant id-從-賣方-入口網站**文字 merchant ID 您收到 「 賣方 」。

[!code-cs[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2.一起提取付款 」 詳細資料。 

這些詳細資料將會顯示給使用者的付款應用程式。 

[!code-cs[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3.含營業稅。 

> [!Important]
> API 不會加入項目也為您計算的營業稅。 請記住稅率因管轄權。 為了清楚起見，我們可以使用假設 9.5%稅率。

[!code-cs[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4.（選擇性） 加入總計的折扣或其他修飾詞。 

以下是新增使用特定 Contoso 信用卡來顯示項目可以享有優惠的範例。 (*Contoso*是虛構的名稱。)

[!code-cs[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5.召集所有付款詳細資料。

[!code-cs[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-cs[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6.送出付款要求。 

呼叫**SubmitPaymentRequestAsync**方法，以提交您付款的要求。 這會顯示可用的付款方式付費應用程式。

[!code-cs[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

系統會提示使用者使用他們的 Microsoft 帳戶登入。

使用者登入之後，他們可以選取付款選項和先前已儲存的送貨地址。

![付款要求 UI](./images/33.png "付款要求 UI")

您的應用程式等待使用者點選**支付**，然後完成順序。

[!code-cs[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

完成付款之後，使用者會看見**訂單確認**螢幕。

![確認的訂單](./images/44.png "確認的訂單 ")

## <a name="see-also"></a>請參閱
- [Windows.ApplicationModel.Payments 參考文件](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [在 GitHub 上 UWP 購物應用程式範例](https://github.com/Microsoft/Windows-appsample-shopping)
- [W3C 付款要求 API 規格](https://www.w3.org/TR/payment-request/)
- [付款要求 API ](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/device/payment-request-api)

