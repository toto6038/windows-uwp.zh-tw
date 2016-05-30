---
author: mcleanbyron
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: 如果您的 app 提供大型的 app 內產品型錄，您可以選擇性地依照本主題中描述的程序來協助管理型錄。
title: 管理大型的 app 內產品型錄
---

# 管理大型的 app 內產品型錄


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

如果您的 app 提供大型的 app 內產品型錄，您可以選擇性地依照本主題中描述的程序來協助管理型錄。 您將針對特定的價格區間建立一些產品項目，其中每個產品項目都能夠代表型錄中的數百個產品。

若要啟用這項功能，請使用 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 方法多載指定 app 定義的選項 (與市集中所列的 app 內產品相關聯)。 除了在呼叫期間指定購買選項和產品關聯，您的 App 也應該傳送包含大型型錄購買選項詳細資料的 [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384) 物件。 如果未提供這些詳細資料，即會改用所列出產品的詳細資料。

市集只會使用產生的 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392) 中來自購買要求的 *offerId*。 這個程序不會直接修改[在市集中列出 app 內的產品](https://msdn.microsoft.com/library/windows/apps/mt148551)時原本提供的資訊。

**注意** 從 Windows 10 開始，市集不會限制每個開發人員帳戶的產品清單數目。 在先前的版本中，市集將每個開發人員帳戶的產品清單數目限制為 200 個，而本主題中所描述的處理程序可用來解決該限制。

## 先決條件

-   本主題涵蓋使用市集中列出的單一應用程式內產品呈現多個應用程式內的購買選項之市集支援。 如果您不熟悉 app 內購買，請檢閱[啟用 App 內產品購買](enable-in-app-product-purchases.md)，以了解授權資訊及如何在市集中正確列出您的 app 內購買。
-   初次撰寫並測試新的應用程式內購買選項程式碼時，您必須使用 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 物件，而不是 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 物件。 如此一來，您就可以利用對授權伺服器進行模擬呼叫來驗證授權邏輯，而不是呼叫使用中的伺服器。 若要這樣做，您必須自訂 %userprofile%\AppData\local\packages\&lt;套件名稱&gt;\LocalState\Microsoft\Windows Store\ApiData 中名為 "WindowsStoreProxy.xml" 的檔案。 Microsoft Visual Studio 模擬器會在您第一次執行您的 app 時建立這個檔案，或者您也可以在執行階段載入自訂的檔案。 如需詳細資訊，請參閱 **CurrentAppSimulator**。
-   本主題也會參照[市集範例](http://go.microsoft.com/fwlink/p/?LinkID=627610) (英文) 中提供的程式碼範例。 這個範例非常適合用來體驗實機操作針對通用 Windows 平台 (UWP) app 提供的不同貨幣選項。

## 針對應用程式內產品提出購買要求

處理針對大型型錄內特定產品的購買要求時，方式與處理 app 內的任何其他購買要求大致相同。 您的 app 在呼叫新的 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 方法多載時，會提供 *OfferId*，以及當中已填入 app 內產品名稱的 [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263390) 物件。

```CSharp
string offerId = "1234";
string displayPropertiesName = "MusicOffer1";
var displayProperties = new ProductPurchaseDisplayProperties(displayPropertiesName);

try
{
    PurchaseResults purchaseResults = await CurrentAppSimulator.RequestProductPurchaseAsync("product1", offerId, displayProperties);
    switch (purchaseResults.Status)
    {
        case ProductPurchaseStatus.Succeeded:
            // Grant the user their purchase here, and then pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotFulfilled:
            // First check for unfulfilled purchases and grant any unfulfilled purchases from an earlier transaction.
            // Once products are fulfilled pass the product ID and transaction ID to currentAppSimulator.reportConsumableFulfillment
            // To indicate local fulfillment to the Windows Store.
            break;
        case ProductPurchaseStatus.NotPurchased:
            // Notify user that the purchase was not completed due to cancellation or error.
            break;
    }
}
catch (Exception)
{
    //Notify the user of the purchase error.
}
```

## 回報 app 內購買選項的履行

當購買選項已在本機履行之後，您的應用程式需要儘速將產品履行回報給市集。 在大型型錄案例中，如果您的應用程式未回報購買選項已履行，使用者將無法使用該相同市集產品清單來購買任何應用程式內的購買選項。

如先前所提及，市集只會使用提供的購買選項資訊來填入 [**PurchaseResults**](https://msdn.microsoft.com/library/windows/apps/dn263392)，不會在大型型錄購買選項和市集產品清單之間建立永久性關聯。 因此，您需要追蹤使用者對產品的權益，並除了 [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382) 操作之外，為使用者提供產品專屬內容 (例如所購買項目的名稱，或項目相關詳細資料)。

下列程式碼會示範履行呼叫，並示範當中插入特定購買選項資訊的 UI 訊息模式。 如果沒有該特定產品資訊，此範例就會使用來自產品 [**ListingInformation**](https://msdn.microsoft.com/library/windows/apps/br225163) 的資訊。

```CSharp
string offerId = "1234";
product1ListingName = product1.Name;
string displayPropertiesName = "MusicOffer1";

if (String.IsNullOrEmpty(displayPropertiesName))
{
    displayPropertiesName = product1ListingName;
}
var offerIdMsg = " with offer id " + offerId;
if (String.IsNullOrEmpty(offerId))
{
    offerIdMsg = " with no offer id";
}

FulfillmentResult result = await CurrentAppSimulator.ReportConsumableFulfillmentAsync(productId, transactionId);
switch (result)
{
    case FulfillmentResult.Succeeded:
        Log("You bought and fulfilled " + displayPropertiesName + offerIdMsg, NotifyType.StatusMessage);
        break;
    case FulfillmentResult.NothingToFulfill:
        Log("There is no purchased product 1 to fulfill.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchasePending:
        Log("You bought product 1. The purchase is pending so we cannot fulfill the product.", NotifyType.StatusMessage);
        break;
    case FulfillmentResult.PurchaseReverted:
        Log("You bought product 1. But your purchase has been reverted.", NotifyType.StatusMessage);
        // Since the user' s purchase was revoked, they got their money back.
        // You may want to revoke the user' s access to the consumable content that was granted.
        break;
    case FulfillmentResult.ServerError:
        Log("You bought product 1. There was an error when fulfilling.", NotifyType.StatusMessage);
        break;
}
```

## 相關主題

* [啟用 app 內產品購買](enable-in-app-product-purchases.md)
* [啟用消費性 app 內產品購買](enable-consumable-in-app-product-purchases.md)
* [市集範例 (示範試用版和 app 內購買)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [**RequestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [**ProductPurchaseDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/dn263384)


<!--HONumber=May16_HO2-->


