---
author: mcleanbyron
ms.assetid: FD381669-F962-465E-940B-AED9C8D19C90
description: "了解如何使用 Windows.Services.Store 命名空間來搭配使用消費性附加元件。"
title: "啟用消費性附加元件購買"
keywords: "App 內的購買選項程式碼範例"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 1e9ecad5abb9addbe41b38d0b56b84404716f2a8

---

# 啟用消費性附加元件購買

目標為 Windows 10 版本 1607 或更新版本的 app，可以在 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的方法，來管理使用者如何在您的 UWP app 中完成消費性附加元件 (也稱為 App 內產品或 IAP)。 請針對可購買、使用，然後再次購買的項目使用消費性附加元件。 這對於像遊戲內貨幣 (金幣、錢幣等) 這種可在買來後用來購買特定火力升級配備的東西，特別有用。

>**注意**&nbsp;&nbsp;本文適用於目標為 Windows 10 版本 1607 或更新版本的 app。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)

## 消費性附加元件概觀

目標為 Windows 10 版本 1607 或更新版本的 app，可提供兩種類型的消費性附加元件，其差別在於管理完成的方式：

* **開發人員管理的消費性產品**。 針對這種類型的消費性產品，您必須負責持續追蹤使用者對該附加元件所代表之項目的餘額，以及在使用者用完所有項目之後，向市集回報已完成附加元件的購買。 在您的 App 回報已完成先前的附加元件購買之前，使用者將無法再次購買該附加元件。

  例如，如果您的附加元件在遊戲中代表 100 個金幣，而使用者花費了 10 個金幣，則您的 App 或服務必須針對該使用者保留 90 個金幣的新餘額。 當使用者花光 100 個金幣之後，您的 App 必須回報該附加元件已完成，接著使用者就能再次購買 100 個金幣的附加元件。

* **市集管理的消費性產品**。 針對這種類型的消費性產品，市集會持續追蹤使用者對該附加元件所代表之項目的餘額。 當使用者取用任何項目時，您必須負責向市集回報這些項目已完成，而市集會更新使用者的餘額。 您的 App 可以隨時查詢使用者目前的餘額。 當使用者用完所有項目之後，該使用者就能再次購買該附加元件。

  例如，如果您的附加元件在遊戲中代表最初的 100 個金幣數量，而使用者花費了 10 個金幣，則您的 App 會向市集回報已完成附加元件的 10 個單位，而市集會更新剩下的餘額。 當使用者花光 100 個金幣之後，該使用者就能再次購買 100 個金幣的附加元件。

  >**注意**&nbsp;&nbsp;市集管理的消費性產品是從 Windows 10 版本 1607 開始提供。 即將推出在 Windows 開發人員中心儀表板建立市集管理的消費性產品的能力。

若要為使用者提供消費性附加元件，請依照下列一般程序執行：

1. 讓使用者能夠從您的 App [購買附加元件](enable-in-app-purchases-of-apps-and-add-ons.md)。
3. 當使用者取用附加元件 (例如，他們在遊戲中花費金幣) 時，[將附加元件回報為已完成](enable-consumable-add-on-purchases.md#report_fulfilled)。

您也可以隨時針對市集管理的消費性產品來[取得剩下的餘額](enable-consumable-add-on-purchases.md#get_balance)。

## 先決條件

這些範例包含下列先決條件：
* 適用於目標為 Windows 10 版本 1607 或更新版本的通用 Windows 平台 (UWP) app 的 Visual Studio 專案。
* 您已在 Windows 開發人員中心儀表板中建立 app 並具備消費性附加元件 (亦稱為App 內購買或 IAP)，而且已在市集中發佈此 app 且可供使用。 這可以是您想要釋出給客戶的 app，或者可以是符合最低 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)需求的基本 app，而您只能基於測試目的加以使用。 如需詳細資訊，請參閱[測試指導方針](in-app-purchases-and-trials.md#testing)。

這些範例中的程式碼假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) 和名為 ```textBlock``` 的 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

如需完整範例應用程式，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span id="report_fulfilled" />
## 將消費性附加元件回報為已完成

當使用者從您的 app [購買附加元件](enable-in-app-purchases-of-apps-and-add-ons.md)並取用您的附加元件之後，您的 app 必須藉由呼叫 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) 方法來將附加元件回報為已完成。 您必須將下列資訊傳遞給此方法：

* 您想要回報為已完成之附加元件的[市集識別碼](in-app-purchases-and-trials.md#store_ids)。
* 您想要回報為已完成之附加元件的單位數。
  * 對於開發人員管理的消費性產品，請針對 *quantity* 參數指定 1。 這會警示市集，消費性產品已完成，而客戶接著可再次購買消費性產品。 在您的 app 通知市集該消費性產品已完成之前，使用者將無法再次購買該產品。
  * 針對市集管理的消費性產品，指定實際已取用的單位數。 市集將會更新該消費性產品剩下的餘額。
* 適用於完成操作的追蹤識別碼。 這是開發人員提供的 GUID，可基於追蹤目的用來識別與完成操作相關聯的特定交易。 如需詳細資訊，請參閱 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.reportconsumablefulfillmentasync.aspx) 中的備註。

這個範例示範如何將市集管理的消費性產品回報為已完成。

```csharp
private StoreContext context = null;

public async void ConsumeAddOn(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // This is an example for a Store-managed consumable, where you specify the actual number
    // of units that you want to report as consumed so the Store can update the remaining
    // balance. For a developer-managed consumable where you maintain the balance, specify 1
    // to just report the add-on as fulfilled to the Store.
    uint quantity = 10;
    string addOnStoreId = "9NBLGGH4TNNR";
    Guid trackingId = Guid.NewGuid();

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.ReportConsumableFulfillmentAsync(
        addOnStoreId, quantity, trackingId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "The fulfillment was successful. Remaining balance: " +
                result.BalanceRemaining;
            break;

        case StoreConsumableStatus.InsufficentQuantity:
            textBlock.Text = "The fulfillment was unsuccessful because the user " +
            "doesn't have enough remaining balance." + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "The fulfillment was unsuccessful due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "The fulfillment was unsuccessful due to a server error.";
            break;

        default:
            textBlock.Text = "The fulfillment was unsuccessful due to an unknown error.";
            break;
    }
}
```

<span id="get_balance" />
## 取得市集管理的消費性產品剩下的餘額。

這個範例示範如何使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的 [GetConsumableBalanceRemainingAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getconsumablebalanceremainingasync.aspx) 方法，來取得市集管理的消費性產品剩下的餘額。

```csharp
private StoreContext context = null;

public async void GetRemainingBalance(string storeId)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    string addOnStoreId = "9NBLGGH4TNNR";

    workingProgressRing.IsActive = true;
    StoreConsumableResult result = await context.GetConsumableBalanceRemainingAsync(addOnStoreId);
    workingProgressRing.IsActive = false;

    if (result.ExtendedError != null)
    {
        // The user may be offline or there might be some other server failure.
        textBlock.Text = $"ExtendedError: {result.ExtendedError.Message}";
        return;
    }

    switch (result.Status)
    {
        case StoreConsumableStatus.Succeeded:
            textBlock.Text = "Remaining balance: " + result.BalanceRemaining;
            break;

        case StoreConsumableStatus.NetworkError:
            textBlock.Text = "Could not retrieve balance due to a network error.";
            break;

        case StoreConsumableStatus.ServerError:
            textBlock.Text = "Could not retrieve balance due to a server error.";
            break;

        default:
            textBlock.Text = "Could not retrieve balance due to an unknown error.";
            break;
    }
}
```

## 相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Aug16_HO5-->


