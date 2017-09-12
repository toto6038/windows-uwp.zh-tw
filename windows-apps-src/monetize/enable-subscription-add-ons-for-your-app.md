---
author: mcleanbyron
description: "了解如何使用 Windows.Services.Store 命名空間來實作訂閱附加元件。"
title: "啟用應用程式的訂閱附加元件"
keywords: "Windows 10, UWP, 訂閱, 附加元件, 在應用程式內購買, IAP, Windows.Services.Store"
ms.author: mcleans
ms.date: 08/01/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 16e2e8d160ad2236220dc6f19f578bbaa82c9dc0
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2017
---
# <a name="enable-subscription-add-ons-for-your-app"></a>啟用應用程式的訂閱附加元件

> [!IMPORTANT]
> 目前，只有參與早期採用者計畫的一組開發人員帳戶才可以建立訂閱附加元件。 未來我們會讓所有開發人員帳戶都可以建立訂閱附加元件，這份初步文件現在可讓開發人員預覽此功能。

如果您的 UWP app 目標為 Windows 10 版本 1607 或更新版本，您可以提供在應用程式內購買的*訂閱*附加元件給客戶。 您可以使用訂閱，利用自動固定計費週期在您的應用程式中銷售數位產品 (例如應用程式功能或數位內容)。

## <a name="feature-highlights"></a>特色重點

UWP app 的訂閱附加元件支援下列功能：

* 您可以選擇 1 個月、3 個月、6 個月、1 年或 2 年的訂閱期間。 某些應用程式也可以使用僅供測試的 6 小時訂閱期間。
* 您可以新增 1 星期或 1 個月的免費試用期間到您的訂閱。
* Windows SDK [提供 API](#code-examples) 供您用於應用程式中，以取得應用程式的可用訂閱附加元件相關資訊並可啟用購買訂閱附加元件。 我們也提供 REST API，您可以從您的服務呼叫以[管理使用者的訂閱](#manage-subscriptions)。
* 您可以檢視分析報告，其中提供訂閱數、使用中的訂閱者，以及在特定時段內的取消訂閱。
* 客戶可以使用其 Microsoft 帳戶的 [http://account.microsoft.com/services](http://account.microsoft.com/services) 頁面來管理其訂閱。 客戶可以使用此頁面來檢視他們已取得的所有訂閱、取消訂閱，以及變更與其訂閱相關聯的付款形式。

> [!NOTE]
> 若要在您的應用程式中啟用購買訂閱附加元件，您的應用程式必須以 Windows 10 版本 1607 或更新版本為目標，而且必須使用 **Windows.Services.Store** 命名空間中的 API 來實作應用程式內體驗，而不是使用 **Windows.ApplicationModel.Store** 命名空間中的 API。 如需有關這些命名空間之間差異的詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

## <a name="steps-to-enable-a-subscription-add-on-for-your-app"></a>在應用程式中啟用訂閱附加元件的步驟

若要在您的應用程式中啟用購買訂閱附加元件，請執行下列步驟。

1. 在開發人員中心儀表板中為您的訂閱[建立附加元件提交](../publish/add-on-submissions.md)並發佈提交。 當您依照附加元件提交程序執行時，請密切注意下列屬性：

  * [產品類型](../publish/set-your-add-on-product-id.md#product-type)：請務必選取**訂閱**。

  * [訂閱期間](../publish/enter-add-on-properties.md#subscription-period)：為您的訂閱選擇週期性計費期間。 發佈附加元件之後，您就無法變更訂閱期間。

    每個訂閱附加元件支援單一訂閱期間和試用期間。 您必須為您想在應用程式中提供的每種訂閱類型，各建立一個訂閱附加元件。 例如，如果您想要提供無試用版、每月訂閱含 1 個月試用版、年度訂閱不含試用版，以及年度訂閱含一個月試用版，您就必須建立四個訂閱附加元件。
        > [!NOTE]
        > If you are in the process of implementing the in-app purchase experience for your subscription, we recommend that you create a test add-on with the **For testing only – 6 hours** subscription period to help you test the experience. You can choose this test period only if you select one of the **Hidden in the Store** [visibility options](../publish/set-add-on-pricing-and-availability.md#visibility) for your test add-on.

  * [試用期間](../publish/enter-add-on-properties.md#free-trial)：請考慮為您的訂閱選擇 1 星期或 1 個月試用期間，讓使用者在購買之前可以先試用您的訂閱內容。 發佈訂閱附加元件之後，您就無法變更或移除試用期間。

    若要取得訂閱的免費試用版，使用者必須透過標準 App 內購買程序購買您的訂閱，包括有效的付款表單。 試用期間不會向使用者收費。 試用期間結束後，訂閱會自動轉換為完整訂閱，並透過使用者的付款方式收取第一期付費訂閱的費用。 如果使用者在試用期間選擇取消訂閱，在試用期間結束之前訂閱都會維持有效。
        > [!NOTE]
        > Some trial durations are not available for all subscription periods.

  * [可見性](../publish/set-add-on-pricing-and-availability.md#visibility)：如果您建立只用來測試訂閱之 App 內購買體驗的測試附加元件，我們建議您選取一個 **\[在市集中隱藏\]** 選項。 否則，您可以選擇最適合您情形的可見性選項。

  * [定價](../publish/set-add-on-pricing-and-availability.md?#pricing)：在本區段中選擇您的訂閱價格。 發佈附加元件之後就無法提高訂閱的價格 (但您稍後可以降低價格)。
      > [!IMPORTANT]
      > 根據預設，當您建立附加元件時，最初價格設為 **\[免費\]**。 因為您完成附加元件提交後就無法提高訂閱附加元件的價格，所以請務必在這裡選擇您的訂閱價格。

2. 在您的應用程式中，使用 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 API 來確認目前使用者是否已取得您的訂閱附加元件，然後再以 App 內購買提供特價給使用者。 如需詳細資訊，請查看本文中的[程式碼範例](#code-examples)。

3. 在您的應用程式中測試訂閱的 App 內購買實作。 您必須從市集下載您的應用程式到開發裝置，才能使用它的授權進行測試。 如需詳細資訊，請參閱 App 內購買的[測試指導方針](in-app-purchases-and-trials.md#testing)。  
    > [!NOTE]
    > 如果您的應用程式可以存取 **\[僅供測試 – 6 小時\]** 訂閱期間，建議您在這段期間建立測試附加元件，可協助您測試體驗。 您必須為您的測試附加元件選取一個 **\[在市集中隱藏\]** [可見性選項](../publish/set-add-on-pricing-and-availability.md#visibility)，才能選擇這個測試期間。

4. 建立並發行包含已更新應用程式套件的 App 提交，其中包含您的測試程式碼。 如需詳細資訊，請參閱 [App 提交](../publish/app-submissions.md)。

<span id="code-examples"/>
## <a name="code-examples"></a>程式碼範例

本節中的程式碼範例示範如何使用 [**Windows.Services.Store**](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間中的 API，取得目前應用程式的訂閱附加元件相關資訊，並要求代表目前使用者購買訂閱附加元件。

這些範例包含下列先決條件：
* 適用於目標為 Windows 10 版本 1607 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您已在 Windows 開發人員中心儀表板中[建立 App 提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)，而且已在市集中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在市集中搜尋。 如需詳細資訊，請參閱[測試指導方針](in-app-purchases-and-trials.md#testing)。
* 您已在開發人員中心儀表板中[為應用程式建立訂閱附加元件](../publish/add-on-submissions.md)。

這些範例中的程式碼假設：
* 程式碼會在 [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) 和名為 ```textBlock``` 的 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增這些範例中未顯示的額外程式碼來設定 [**StoreContext**](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

### <a name="get-info-about-subscription-add-ons-for-the-current-app"></a>取得目前應用程式的訂閱附加元件相關資訊

此程式碼範例示範如何取得您應用程式中可用之訂閱附加元件的相關資訊。 若要取得此項資訊，請先使用 [**GetAssociatedStoreProductsAsync**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext#Windows_Services_Store_StoreContext_GetAssociatedStoreProductsAsync_) 方法，取得代表應用程式可用之每個附加元件的 [**StoreProduct**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) 物件集合。 然後，取得每個產品的 [**StoreSku**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku)，並使用 [**IsSubscription**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_IsSubscription_) 和 [**SubscriptionInfo**](https://docs.microsoft.com/uwp/api/windows.services.store.storesku#Windows_Services_Store_StoreSku_SubscriptionInfo_) 屬性來存取訂閱資訊。

> [!div class="tabbedCodeSnippets"]
[!code-cs[訂閱](./code/InAppPurchasesAndLicenses_RS1/cs/GetSubscriptionAddOnsPage.xaml.cs#GetSubscriptions)]

### <a name="purchase-a-subscription-add-on"></a>購買訂閱附加元件

此範例示範如何使用 [**StoreProduct**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) 類別的 [**RequestPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct#Windows_Services_Store_StoreProduct_RequestPurchaseAsync_) 方法來購買訂閱附加元件。 此範例假設您已經知道您要購買之訂閱附加元件的[市集識別碼](in-app-purchases-and-trials.md#store-ids)。

> [!div class="tabbedCodeSnippets"]
[!code-cs[訂閱](./code/InAppPurchasesAndLicenses_RS1/cs/PurchaseSubscriptionAddOnPage.xaml.cs#PurchaseSubscription)]

<span id="manage-subscriptions" />
## <a name="manage-subscriptions-from-your-services"></a>從您的服務管理訂閱

在市集中更新您的應用程式，且客戶可以購買您的訂閱附加元件之後，您可能需要管理客戶的訂閱案例。 我們提供 REST API，您可以從您的服務呼叫以執行下列訂閱管理工作：

* [取得使用者有使用權利的訂閱](get-subscriptions-for-a-user.md)。 如果您的訂閱屬於跨平台服務的一部分，您可以呼叫此 API 以判斷指定的使用者是否擁有您的訂閱的使用權利，以及他們在 UWP app 內中的訂閱狀態。 接著您可使用此資訊來更新您服務支援的其他平台上的訂閱狀態。

* [變更特定使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。 使用此 API 可以取消、延長或停用訂閱的自動續約。

<span id="cancellations" />
## <a name="cancellations"></a>取消

客戶可以使用其 Microsoft 帳戶的 [http://account.microsoft.com/services](http://account.microsoft.com/services) 頁面，來檢視他們已取得的所有訂閱、取消訂閱，以及變更與其訂閱相關聯的付款形式。 客戶使用這個頁面取消訂閱時，他們在目前的計費週期可以繼續存取訂閱。 他們無法取得目前計費週期的的任何部分退款。 在目前的計費週期結束後，其訂閱會停用。

您也可以使用 REST API 來代表使用者取消訂閱，[變變更特定使用者訂閱的帳單狀態](change-the-billing-state-of-a-subscription-for-a-user.md)。

## <a name="unsupported-scenarios"></a>不支援的案例

訂閱附加元件目前不支援下列案例。

* 目前不支援透過市集直接銷售訂閱給客戶。 只能從數位內容的在應用程式內購買取得訂閱。
* 客戶不能使用其 Microsoft 帳戶的 [http://account.microsoft.com/services](http://account.microsoft.com/services) 頁面來切換訂閱週期。 若要切換到不同的訂閱週期，客戶必須先取消目前訂閱，然後使用不同訂閱週期從您的應用程式購買訂閱。
* 訂閱附加元件目前不支援切換層級 (例如從基本訂閱切換至功能更多的優質訂閱)。
* 訂閱附加元件目前不支援[銷售](../publish/put-apps-and-add-ons-on-sale.md)與[促銷代碼](../publish/generate-promotional-codes.md)。


## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
