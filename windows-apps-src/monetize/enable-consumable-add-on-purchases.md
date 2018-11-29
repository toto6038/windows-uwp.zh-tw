---
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: 了解如何使用 Windows.Services.Store 命名空間來搭配使用消費性附加元件。
title: 啟用消費性附加元件購買
keywords: Windows 10, UWP, 消費性, 附加元件, 應用程式內購買, IAP, Windows.Services.Store
ms.date: 05/09/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 0446269fcbde87dfa25b7bff25f7160335950fba
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "7988122"
---
# <a name="enable-consumable-add-on-purchases"></a>啟用消費性附加元件購買

本文章示範如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的方法，來管理使用者如何在您的 UWP app 中完成消費性附加元件。 請針對可購買、使用，然後再次購買的項目使用消費性附加元件。 這對於像遊戲內貨幣 (金幣、錢幣等) 這種可在買來後用來購買特定火力升級配備的東西，特別有用。

> [!NOTE]
> **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[這篇文章](enable-consumable-in-app-product-purchases.md)。

## <a name="overview-of-consumable-add-ons"></a>消費性附加元件概觀

應用程式可提供兩種類型的消費性附加元件，其差別在於管理完成的方式：

* **開發人員管理的消費性產品**。 針對這種類型的消費性產品，您必須負責持續追蹤使用者對該附加元件所代表之項目的餘額，以及在使用者用完所有項目之後，向市集回報已完成附加元件的購買。 在您的 App 回報已完成先前的附加元件購買之前，使用者將無法再次購買該附加元件。

  例如，如果您的附加元件在遊戲中代表 100 個金幣，而使用者花費了 10 個金幣，則您的應用程式或服務必須針對該使用者保留 90 個金幣的新餘額。 當使用者花光 100 個金幣之後，您的 App 必須回報該附加元件已完成，接著使用者就能再次購買 100 個金幣的附加元件。

* **市集管理的消費性產品**。 針對這種類型的消費性產品，市集會持續追蹤使用者對該附加元件所代表之項目的餘額。 當使用者取用任何項目時，您必須負責向市集回報這些項目已完成，而市集會更新使用者的餘額。 使用者可以隨時多次購買附加元件 (不需要先取用項目)。 您的應用程式可以隨時查詢 Microsoft Store 有關使用者目前的餘額。

  例如，如果您的附加元件在遊戲中代表最初的 100 個金幣數量，而使用者花費了 50 個金幣，則您的應用程式會向 Microsoft Store 回報已完成附加元件的 50 個單位，而 Microsoft Store 會更新剩下的餘額。 如果使用者然後重新購買附加元件以獲取多 100 個硬幣，它們現在總計會有 150 個硬幣。
    > [!NOTE]
    > Microsoft Store 管理的消費性產品是從 Windows 10 (版本 1607) 開始提供。

若要為使用者提供消費性附加元件，請依照下列一般程序執行：

1. 讓使用者能夠從您的 App [購買附加元件](enable-in-app-purchases-of-apps-and-add-ons.md)。
3. 當使用者取用附加元件 (例如，他們在遊戲中花費金幣) 時，[將附加元件回報為已完成](enable-consumable-add-on-purchases.md#report_fulfilled)。

您也可以隨時針對市集管理的消費性產品來[取得剩下的餘額](enable-consumable-add-on-purchases.md#get_balance)。

## <a name="prerequisites"></a>先決條件

這些範例包含下列先決條件：
* 適用於目標為 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您必須在合作夥伴中心的 [[建立應用程式提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)，並在市集中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在 Microsoft Store 中搜尋。 如需詳細資訊，請參閱我們的[測試指南](in-app-purchases-and-trials.md#testing)。
* 在合作夥伴中心已[建立消費性附加元件的應用程式](../publish/add-on-submissions.md)。

這些範例中的程式碼假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) 和名為 ```textBlock``` 的 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

如需完整的範例應用程式，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增這些範例中未顯示的額外程式碼來設定 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

<span id="report_fulfilled" />

## <a name="report-a-consumable-add-on-as-fulfilled"></a>將消費性附加元件回報為已完全交付

當使用者從您的 app [購買附加元件](enable-in-app-purchases-of-apps-and-add-ons.md)並取用您的附加元件之後，您的 app 必須藉由呼叫 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 方法來將附加元件回報為已完成。 您必須將下列資訊傳遞給此方法：

* 您想要回報為已完成之附加元件的[市集識別碼](in-app-purchases-and-trials.md#store-ids)。
* 您想要回報為已完成之附加元件的單位數。
  * 對於開發人員管理的消費性產品，請針對 *quantity* 參數指定 1。 這會警示市集，消費性產品已完成，而客戶接著可再次購買消費性產品。 在您的 app 通知市集該消費性產品已完成之前，使用者將無法再次購買該產品。
  * 針對市集管理的消費性產品，指定實際已取用的單位數。 市集將會更新該消費性產品剩下的餘額。
* 適用於完成操作的追蹤識別碼。 這是開發人員提供的 GUID，可基於追蹤目的用來識別與完成操作相關聯的特定交易。 如需詳細資訊，請參閱 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 中的備註。

這個範例示範如何將 Microsoft Store 管理的消費性產品回報為已完成。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/ConsumeAddOnPage.xaml.cs#ConsumeAddOn)]

<span id="get_balance" />

## <a name="get-the-remaining-balance-for-a-store-managed-consumable"></a>取得市集管理的消費性產品剩下的餘額。

這個範例示範如何使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的 [GetConsumableBalanceRemainingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getconsumablebalanceremainingasync) 方法，來取得市集管理的消費性產品剩下的餘額。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumables](./code/InAppPurchasesAndLicenses_RS1/cs/GetRemainingAddOnBalancePage.xaml.cs#GetRemainingAddOnBalance)]

## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
