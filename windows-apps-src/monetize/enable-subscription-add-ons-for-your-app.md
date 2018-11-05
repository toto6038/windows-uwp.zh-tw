---
author: Xansky
description: 了解如何使用 Windows.Services.Store 命名空間來實作訂閱附加元件。
title: 啟用應用程式的訂閱附加元件
keywords: Windows 10, UWP, 訂閱, 附加元件, 在應用程式內購買, IAP, Windows.Services.Store
ms.author: mhopkins
ms.date: 12/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 207203805dfee0fd54a9d6d6fd4987b098710f4c
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6024832"
---
# <a name="enable-subscription-add-ons-for-your-app"></a>啟用應用程式的訂閱附加元件

您的通用 Windows 平台 (UWP) 應用程式可以向您的客戶提供 App 內購買的*訂用帳戶*附加元件。 您可以使用訂閱，利用自動固定計費週期在您的應用程式中銷售數位產品 (例如應用程式功能或數位內容)。

> [!NOTE]
> 若要在您的應用程式中提供購買訂閱附加元件，您的專案必須以 Visual Studio 中的 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本為目標 (這對應到 Windows 10 (版本 1607))，且其必須使用 **Windows.Services.Store** 命名空間中的 API 來實作 App 內購買體驗，而不是使用 **Windows.ApplicationModel.Store** 命名空間。 如需有關這些命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

## <a name="feature-highlights"></a>特色重點

UWP app 的訂閱附加元件支援下列功能：

* 您可以選擇 1 個月、3 個月、6 個月、1 年或 2 年的訂閱期間。
* 您可以將 1 週或 1 個月的免費試用期新增到您訂用帳戶。
* Windows SDK [提供 API](#code-examples) 供您用於應用程式中，以取得應用程式的可用訂閱附加元件相關資訊並可啟用購買訂閱附加元件。 我們也提供 REST API，您可以從您的服務呼叫以[管理使用者的訂閱](#manage-subscriptions)。
* 您可以檢視分析報告，這些報告提供特定時間內的訂閱數、使用中訂閱者數以及取消訂閱數。
* 客戶可以在其 Microsoft 帳戶的 [http://account.microsoft.com/services](http://account.microsoft.com/services) 頁面上管理其訂閱。 客戶可以使用此頁面來檢視他們已取得的所有訂閱、取消訂閱，以及變更與其訂閱相關聯的付款形式。

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>在應用程式中啟用訂閱附加元件的步驟

若要在您的應用程式中啟用購買訂閱附加元件，請執行下列步驟。

1. 為您的訂閱，在合作夥伴中心[建立附加元件提交](../publish/add-on-submissions.md)與發佈提交。 當您依照附加元件提交程序執行時，請密切注意下列屬性：

    * [產品類型](../publish/set-your-add-on-product-id.md#product-type)：請務必選取**訂閱**。

    * [訂閱期間](../publish/enter-add-on-properties.md#subscription-period)：為您的訂閱選擇週期性計費期間。 發佈附加元件之後，您就無法變更訂閱期間。

        每個訂閱附加元件支援單一訂閱期間和試用期間。 您必須為您想在應用程式中提供的每種訂閱類型，各建立一個訂閱附加元件。 例如，如果您想要提供無試用版、每月訂閱含 1 個月試用版、年度訂閱不含試用版，以及年度訂閱含一個月試用版，您就必須建立四個訂閱附加元件。

    * [試用期間](../publish/enter-add-on-properties.md#free-trial)：請考慮為您的訂閱選擇 1 星期或 1 個月試用期間，讓使用者在購買之前可以先試用您的訂閱內容。 發佈訂閱附加元件之後，您就無法變更或移除試用期間。

        若要取得訂閱的免費試用版，使用者必須透過標準 App 內購買程序購買您的訂閱，包括有效的付款表單。 試用期間不會向使用者收費。 試用期間結束後，訂閱會自動轉換為完整訂閱，並透過使用者的付款方式收取第一期付費訂閱的費用。 如果使用者在試用期間選擇取消其訂閱，則訂閱將保持作用中直到試用期結束。 某些試用期無法適用於所有訂閱期。

        > [!NOTE]
        > 每個客戶只能取得一次訂閱附加元件的免費試用版。 客戶取得免費試用訂用帳戶之後，Microsoft Store 會阻止相同客戶再次取得相同的免費試用訂用帳戶。

    * [Visibility](../publish/set-add-on-pricing-and-availability.md#visibility)：如果您正在建立一個測試附加元件，此測試附加元件只用於測試訂閱的 App 內購買體驗，我們建議您選擇其中一個**在 Microsoft Store 中隱藏**選項。 否則，您可以選擇最適合您情形的可見性選項。

    * [定價](../publish/set-add-on-pricing-and-availability.md?#pricing)：在本區段中選擇您的訂閱價格。 附加元件發佈後，您無法提高訂閱的價格。 不過，您可以稍後降低價格。
        > [!IMPORTANT]
        > 根據預設，當您建立任何附加元件時，最初價格設定為**免費**。 因為您完成附加元件提交後就無法提高訂閱附加元件的價格，所以請務必在這裡選擇您的訂閱價格。

2. 在您的應用程式中，使用 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 API 來確定目前使用者是否已獲得您的訂閱附加元件，然後將其作為 App 內購買銷售給使用者。 如需詳細資訊，請查看本文中的[程式碼範例](#code-examples)。

3. 在您的應用程式中測試訂閱的 App 內購買實作。 您必須從市集下載您的應用程式到開發裝置，才能使用它的授權進行測試。 如需詳細資訊，請參閱我們的 App 內購買[測試指導方針](in-app-purchases-and-trials.md#testing)。  

4. 建立並發佈包含更新後的應用程式套件的應用程式提交，包括您的測試程式碼。 如需詳細資訊，請參閱 [App 提交](../publish/app-submissions.md)。

<span id="code-examples"/>

## <a name="code-examples"></a>程式碼範例

本節中的程式碼範例示範如何使用 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 API，取得目前應用程式的訂閱附加元件相關資訊，並要求代表目前使用者購買訂閱附加元件。

這些範例包含下列先決條件：
* 適用於目標為 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您必須在合作夥伴中心的 [[建立應用程式提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)，並在市集中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在市集中搜尋。 如需詳細資訊，請參閱[測試指導方針](in-app-purchases-and-trials.md#testing)。
* 在合作夥伴中心已[建立應用程式的訂閱附加元件](../publish/add-on-submissions.md)。

這些範例中的程式碼假設：
* 程式碼檔案含有適用於 **Windows.Services.Store** 和 **System.Threading.Tasks** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增這些範例中未顯示的額外程式碼來設定 [**StoreContext**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

### <a name="purchase-a-subscription-add-on"></a>購買訂閱附加元件

此範例示範如何代表目前客戶要求為您的應用程式購買已知訂閱附加元件。 此範例也會顯示如何處理含有試用期的訂閱之案例。

1. 該程式碼首先判斷客戶是否已經擁有用於該訂閱的使用中授權。 如果該客戶已經有使用中授權，則您的驗證碼應根據需要來解除鎖定訂閱功能 (因為這是專屬於您的應用程式，在本範例中以註解作標示)。
2. 接下來，程式碼取得代表客戶想要購買的訂閱的 [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 物件。 程式碼假設您已經知道要購買的訂閱附加元件的 [Store 識別碼](in-app-purchases-and-trials.md#store-ids)，並且您已經將此值指定至 *subscriptionStoreId* 變數。
3. 程式碼然後判斷試用是否可用於訂閱。 您的應用程式可以選擇使用此資訊，以向客戶顯示出有關可用試用版或完整訂閱的詳細資料。
4. 最後，程式碼 呼叫 [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.RequestPurchaseAsync) 方法來要求購買訂閱。 如果試用可用於訂閱，則試用將提供給客戶購買。 否則，將提供完整的訂閱供購買。

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnTrialPage.xaml.cs#PurchaseTrialSubscription)]

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>取得有關目前應用程式的訂閱附加元件資訊

此程式碼範例示範如何取得應用程式中可用的所有訂閱附加元件資訊。 若要取得此項資訊，請先使用 [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsAsync) 方法，取得代表應用程式可用之每個附加元件的 [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) 物件集合。 然後，取得每個產品的 [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) 並使用 [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.IsSubscription) 和 [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.SubscriptionInfo) 屬性存取訂閱資訊。

> [!div class="tabbedCodeSnippets"]
[!code-cs[Subscriptions](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

<span id="manage-subscriptions" />

## <a name="manage-subscriptions-from-your-services"></a>從您的服務中管理訂閱

在市集中更新您的應用程式，且客戶可以購買您的訂閱附加元件之後，您可能需要管理客戶的訂閱案例。 我們提供 REST API，您可以從您的服務呼叫以執行下列訂閱管理工作：

* [取得使用者有使用權利的訂閱](get-subscriptions-for-a-user.md)。 如果您的訂閱屬於跨平台服務的一部分，您可以呼叫此 API 以判斷指定的使用者是否擁有您的訂閱的使用權利，以及他們在 UWP app 內中的訂閱狀態。 接著您可使用此資訊來更新您服務支援的其他平台上的訂閱狀態。

* [變更特定使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。 使用此 API 可以取消、延長或停用訂閱的自動續約。

## <a name="cancellations"></a>取消

客戶可以使用他們的 Microsoft 帳戶的 [http://account.microsoft.com/services](http://account.microsoft.com/services) 頁面檢視他們已取得的所有訂閱，取消訂閱並更改與訂閱關聯的付款方式。 當客戶使用此頁面取消訂閱時，他們在目前計費期間繼續存取訂閱。 他們無法取得目前計費期間的任何部分的退款。 在目前計費期間結束後，其訂閱被停用。

您也可以使用 REST API 來代表使用者取消訂閱，[變變更特定使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

## <a name="subscription-renewals-and-grace-periods"></a>訂閱續約和寬限期

在每個帳單期間的某個時間點，我們會嘗試向客戶收取下一個帳單期間的信用卡費用。 如果收費失敗，客戶的訂閱將進入*催繳*狀態。 這表示其訂閱在目前計費期間的剩餘時間內仍處於使用中，但我們會定期嘗試向其信用卡收費以自動續約訂閱。 此狀態會持續兩週，直到目前計費期間結束和下一個計費期間的續訂日期。

我們不提供訂閱計費的寬限期。 如果我們在目前計費期間結束時無法成功向客戶的信用卡收費，則將取消訂閱，客戶在目前計費期間後將無法存取訂閱。

## <a name="unsupported-scenarios"></a>不支援的案例

訂閱附加元件目前不支援下列案例。

* 目前不支援透過市集直接銷售訂閱給客戶。 訂閱僅適用於數位產品的 App 內購買。
* 客戶無法使用其 Microsoft 帳戶的 [http://account.microsoft.com/services](http://account.microsoft.com/services) 頁面切換訂閱期限。 若要切換到不同的訂閱週期，客戶必須先取消目前訂閱，然後使用不同訂閱週期從您的應用程式購買訂閱。
* 訂閱附加元件目前不支援切換層級 (例如從基本訂閱切換至功能更多的優質訂閱)。
* 訂閱附加元件目前不支援[銷售](../publish/put-apps-and-add-ons-on-sale.md)與[促銷代碼](../publish/generate-promotional-codes.md)。


## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
