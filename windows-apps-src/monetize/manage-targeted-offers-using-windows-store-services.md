---
author: mcleanbyron
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: "使用 Windows 市集針對性優惠 API，為 app 可用的針對性優惠請款。"
title: "使用市集服務管理針對性優惠"
ms.author: mcleans
ms.date: 05/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10 uwp, 市集服務, Windows 市集針對性優惠 API, 針對性優惠"
ms.openlocfilehash: 684c37d4439f415ad607b7f3e6a166966cc9f835
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/17/2017
---
# <a name="manage-targeted-offers-using-store-services"></a>使用市集服務管理針對性優惠

如果您在 Windows 開發人員中心儀表板中的 **\[互動\] > \[針對性優惠\]** 頁面建立適用於 app 的*針對性優惠*，請在 app 程式碼中使用 *Windows 市集針對性優惠 API* 實作針對性優惠 App 內體驗。 如需有關針對性優惠，以及如何在儀表板中建立它們的詳細資訊，請參閱[使用針對性優惠提高吸引力與轉換數](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)。

針對性優惠 API 是可用來執行這些的 REST API：

* 根據使用者是不是針對性優惠客戶區隔的一部分，取得適用於目前使用者的針對性優惠。
* 如果使用者購買針對性優惠，您可以提交請款要求至市集，以便您可以接收與針對性優惠相關聯的獎勵。 這只在針對性優惠與 Microsoft 贊助促銷活動相關聯，為每個成功轉換支付獎勵給開發人員時，才是需要的。

若要在 app 程式碼中使用這個 API，請依照下列步驟執行：

1.  為您的 app 目前已登入的使用者[取得 Microsoft 帳戶權杖](#obtain-a-microsoft-account-token)。
2.  [為目前使用者取得針對性優惠](#get-targeted-offers)。
3.  [購買與針對性優惠相關聯的附加元件](#purchase-add-on)。
3.  [為針對性優惠請款](#claim-targeted-offer) (如果針對性優惠與 Microsoft 贊助促銷活動相關聯，為每個成功轉換支付獎勵給開發人員時)。

> [!NOTE]
> 將針對性優惠與 Microsoft 贊助促銷活動相關聯，然後為優惠提交請款要求的功能，目前未提供給所有開發人員帳戶。

如需示範所有步驟的完整程式碼範例，請參閱本文結尾的[程式碼範例](#code-example)。 下列各節提供每個步驟的詳細資料。

<span id="obtain-a-microsoft-account-token" />
## <a name="get-a-microsoft-account-token-for-the-current-user"></a>為目前使用者取得 Microsoft 帳戶權杖

在您的 app 程式碼，為目前已登入的使用者取得 Microsoft 帳戶 (MSA) 權杖。 您必須針對 Windows 市集針對性優惠 API 中的每一個方法，在 ```Authorization``` 要求標頭中傳送此權杖。 市集會使用此權杖來擷取適用於目前使用者的針對性優惠。

若要取得 MSA 權杖，請使用[WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 類別來要求使用範圍 ```devcenter_implicit.basic,wl.basic``` 的權杖。 以下範例示範操作方法。 此範例是[完整範例](#code-example)的摘要，並需要完整範例中提供的 **using** 陳述式。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

如需有關如何取得 MSA 權杖的詳細資訊，請[Web 帳戶管理員](../security/web-account-manager.md)。

<span id="get-targeted-offers" />
## <a name="get-the-targeted-offers-for-the-current-user"></a>為目前使用者取得針對性優惠

當您取得目前使用者的 MSA 權杖之後，呼叫 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI 的 GET 方法，取得適用於目前使用者的針對性優惠。 如需有關這個 REST 方法的詳細資訊，請參閱[取得針對性優惠](get-targeted-offers.md)。

這個方法傳回附加元件的產品識別碼，這些附加元件與適用於目前使用者的針對性優惠相關聯。 使用此資訊，您可以為使用者提供一或多個針對性優惠做為 App 內購買。 這個方法也會傳回追蹤識別碼，您可以使用來[提交請款要求](#claim-targeted-offer)至市集，以便您可以接收與其中一個針對性優惠相關聯的獎勵。

下列範例示範如何取得適用於目前使用者的針對性優惠。 此範例是[完整範例](#code-example)的摘要。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫和其他類別，以及完整範例中提供的 **using** 陳述式。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="purchase-add-on" />
## <a name="purchase-the-add-on-that-is-associated-with-a-targeted-offer"></a>購買與針對性優惠相關聯的附加元件

接下來，提供其中一個針對性優惠，供使用者購買。 如果使用者同意購買針對性優惠，使用下列方法以程式設計方式購買與針對性優惠相關聯的附加元件。 使用您在上一個步驟中擷取的產品識別碼，識別您想要購買的附加元件。

* 如果您 App 的目標是 Windows10 版本 1607 或更新版本，建議您使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空間的其中一個 **RequestPurchaseAsync** 方法。 如需使用這些方法的詳細資訊，請參閱[啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)。

* 如果您 App 的目標是舊版 Windows 10，請使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間的 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) 方法來購買附加元件。 如需有關如何使用此方法的詳細資訊，請參閱[啟用 app 內產品購買](enable-in-app-product-purchases.md)。

如需示範每個選項的程式碼範例，請參閱本文結尾的[程式碼範例](#code-example)。

> [!NOTE]
> 有兩個不同的命名空間可實作 UWP app 中的 App 內購買：[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)（Windows 10 版本 1607 中導入）和 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)（適用於 Windows 10 的所有版本）。 如需有關這些命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

<span id="claim-targeted-offer" />
## <a name="submit-a-claim-for-a-targeted-offer"></a>對於針對性優惠提交請款

如果使用者購買與 Microsoft 贊助促銷活動相關聯的針對性優惠，為每個成功轉換支付獎勵給開發人員時，您必須提交請款要求至市集，以便接收獎勵。 當您提交請款時，必須提供代表購買確認的值。 取得此購買確認的方式，取決於您的應用程式是否使用 **Windows.ApplicationModel.Store** 命名空間或 **Windows.Services.Store** 命名空間中的 API 來購買附加元件。

> [!NOTE]
> 將針對性優惠與 Microsoft 贊助促銷活動相關聯，然後為優惠提交請款要求的功能，目前未提供給所有開發人員帳戶。

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>使用 Windows.ApplicationModel.Store 命名空間的應用程式

1. 使用 **Windows.ApplicationModel.Store** 命名空間的 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_RequestProductPurchaseAsync_System_String_) 方法購買附加元件後，請確定您擷取[購買收據](use-receipts-to-verify-product-purchases.md)。 此收據位於 [PurchaseResults.ReceiptXml](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.purchaseresults#Windows_ApplicationModel_Store_PurchaseResults_ReceiptXml_) 物件中 (由 **RequestProductPurchaseAsync** 方法傳回)。 此收據代表將在下一個步驟中用到的購買確認。

2. 傳送 POST 訊息到 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI，為針對性優惠的轉換提交請款要求。 如需有關這個 REST 方法的詳細資訊，請參閱[為針對性優惠請款](claim-a-targeted-offer.md)。 在要求本文中，傳遞下列資料：

  * *storeOffer* 欄位：傳遞您從[取得針對性優惠](get-targeted-offers.md)方法的回應本文收到的其中一個 JSON 物件位 (這個物件應該包括 *offers* 和 *trackingId* 欄位)。

  * *receipt* 欄位：傳遞您在上一個步驟中擷取的收據字串 (這可從 **RequestProductPurchaseAsync** 方法傳回的 **PurchaseResults.ReceiptXml** 物件提供)。

下列範例示範如何使用 **Windows.ApplicationModel.Store** 命名空間中的 API 來購買並為針對性優惠請款。 此範例是[完整範例](#code-example)的摘要。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫和其他類別，以及完整範例中提供的 **using** 陳述式。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnAnyVersionWindows10)]

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>使用 Windows.Services.Store 命名空間的應用程式

1. 使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store) 命名空間的其中一個 **RequestPurchaseAsync** 方法購買附加元件後，依照下列步驟可取得購買的*訂單識別碼*。 此值代表購買確認。

  1. 呼叫 [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetUserCollectionAsync_Windows_Foundation_Collections_IIterable_System_String__) 方法以取得 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件的集合，代表使用者取得的附加元件。

  2. 在這個集合中，取得代表已為針對性優惠購買之附加元件的 **StoreProduct** 物件。

  3. 取得此 **StoreProduct** 物件之 [ExtendedJsonData](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct#Windows_Services_Store_StoreProduct_ExtendedJsonData_) 屬性的值。 此屬性會傳回 JSON 格式字串，其中包含附加元件的完整市集相關資料。

  4. 剖析此 JSON 字串，並擷取字串中 *orderId* 欄位的值。

2. 傳送 POST 訊息到 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI，為針對性優惠的轉換提交請款要求。 如需有關這個 REST 方法的詳細資訊，請參閱[為針對性優惠請款](claim-a-targeted-offer.md)。 在要求本文中，傳遞下列物件：

  * *storeOffer* 欄位：傳遞您從[取得針對性優惠](get-targeted-offers.md)方法的回應本文收到的其中一個 JSON 物件位 (這個物件應該包括 *offers* 和 *trackingId* 欄位)。

  * *receipt* 欄位：傳遞您在上一個步驟擷取的 *orderId* 值。

下列範例示範如何使用 **Windows.Services.Store** 命名空間中的 API 來購買並為針對性優惠請款。 此範例是[完整範例](#code-example)的摘要。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫和其他類別，以及完整範例中提供的 **using** 陳述式。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#ClaimOfferOnWindows1607AndLater)]

<span id="code-example" />
## <a name="complete-code-example"></a>完整程式碼範例

以下程式碼範例示範下列工作：

* 取得目前使用者的 MSA 權杖。
* 使用[取得針對性優惠](get-targeted-offers.md)方法，取得適用於目前使用者所有針對性優惠。
* 購買與針對性優惠相關聯的附加元件。
* 使用[為針對性優惠請款](claim-a-targeted-offer.md)方法，為針對性優惠請款。

此範例需要來自 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 程式庫。 範例使用此程式庫來序列化和還原序列化 JSON 格式的資料。

> [!NOTE]
> 有兩個不同的命名空間可實作 UWP app 中的 App 內購買：[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)（Windows 10 版本 1607 中導入）和 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)（適用於 Windows 10 的所有版本）。 此範例使用[版本調適型程式碼](../debug-test-perf/version-adaptive-code.md)以在同一個應用程式中同時使用兩個命名空間，購買附加元件並為針對性優惠提交請款要求。 若要使用此代碼，專案的目標作業系統版本必須是 **Windows 10 Anniversary Edition (10.0 版；組建 14394)** 或更新版本，不過如果您希望在更早的 Windows 10 版本上執行您的應用程式，最小作業系統版本可以是更早的版本。 如需有關這些命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>相關主題

* [使用針對性優惠提高吸引力與轉換數](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [取得針對性優惠](get-targeted-offers.md)
* [為針對性優惠請款](claim-a-targeted-offer.md)
