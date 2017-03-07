---
author: mcleanbyron
Description: "您可以透過市集商業平台提供消費性的應用程式內產品，亦即可購買、使用然後再次購買的項目，為客戶提供既健全又可靠的購買體驗。"
title: "啟用消費性應用程式內產品購買"
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: "UWP, 消費性, 附加元件, 應用程式內購買, IAP, Windows.ApplicationModel.Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7395cf28f96b2f7aa9bc6a1d4c461385d50fcbf6
ms.lasthandoff: 02/07/2017

---

# <a name="enable-consumable-in-app-product-purchases"></a>啟用消費性應用程式內產品購買


>**注意**&nbsp;&nbsp;本文章示範如何使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間的成員。 如果 App 的目標為 Windows 10 版本 1607 或更新版本，則我們建議您使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間的成員來管理附加元件 (也稱為應用程式內產品或 IAP)，而不是使用 **Windows.ApplicationModel.Store** 命名空間。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

您可以透過市集商業平台提供消費性的應用程式內產品 (亦即可購買、使用，然後再次購買的項目)，為客戶提供既健全又可靠的購買體驗。 這對於像遊戲內貨幣 (金幣、錢幣等) 這種可在買來後用來購買特定火力升級配備的東西，特別有用。

## <a name="prerequisites"></a>先決條件

-   本主題涵蓋消費性應用程式內產品的購買和履行狀況報告。 如果您不熟悉應用程式內產品，請檢閱[啟用應用程式內產品購買](enable-in-app-product-purchases.md)，以了解授權資訊及如何在市集中正確列出應用程式內產品。
-   初次撰寫並測試新應用程式內產品的程式碼時，您必須使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 物件，而不是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 物件。 如此一來，您就可以利用對授權伺服器進行模擬呼叫來驗證授權邏輯，而不是呼叫使用中的伺服器。 若要這樣做，您必須自訂 %userprofile%\\AppData\\local\\packages\\&lt;套件名稱&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中名為 WindowsStoreProxy.xml 的檔案。 Microsoft Visual Studio 模擬器會在您第一次執行您的 App 時建立這個檔案，或者您也可以在執行階段載入自訂的檔案。 如需詳細資訊，請參閱[使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。
-   本主題也會參照[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)中提供的程式碼範例。 這個範例非常適合用來體驗實機操作針對通用 Windows 平台 (UWP) app 提供的不同貨幣選項。

## <a name="step-1-making-the-purchase-request"></a>步驟 1：提出購買要求

就像透過市集進行的任何其他購買一樣，初始購買要求是以 [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381) 提出。 消費性應用程式內產品的不同之處在於，客戶在順利購買這類產品之後，除非應用程式已通知市集先前的購買已順利履行，否則客戶無法再次購買相同的產品。 您的應用程式必須負責履行已購買的消費性產品，並在履行後通知市集。

下列範例示範消費性應用程式內產品購買要求。 您會注意到程式碼註解指出在下列兩種不同的情況下，App 應該於何時在本機履行消費性應用程式內產品，一是在要求成功的情況，二是在因為購買尚未履行的相同產品而導致要求失敗的情況。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#MakePurchaseRequest)]

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>步驟 2：追蹤消費性產品的本機履行

將消費性應用程式內產品的存取權授與客戶時，請務必追蹤哪些產品已履行 (*productId*)，以及該履行動作與哪個交易關聯 (*transactionId*)。

>**重要**&nbsp;&nbsp;您的 App 必須將履行動作準確回報給市集。 若要為客戶維護公平可靠的購買體驗，這個步驟是不可或缺的。

下列範例示範如何使用上一個步驟之 [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263381) 呼叫中的 [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 屬性，來識別購買的產品是否已經履行。 此範例使用集合將產品資訊儲存在可供參照的位置，以便稍後確認是否已在本機順利履行。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GrantFeatureLocally)]

下個範例說明將履行回報給市集後，如何使用上個範例的陣列來存取之後要使用的產品識別碼/交易識別碼。

>**重要**&nbsp;&nbsp;無論您的 App 使用哪種方法來追蹤和確認履行，都必須提供審查評鑑，以確保不會針對客戶尚未收到的項目向客戶收費。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#IsLocallyFulfilled)]

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>步驟 3：將產品履行回報給市集

完成本機履行之後，您的 App 必須進行 [ReportConsumableFulfillmentAsync](https://msdn.microsoft.com/library/windows/apps/dn263380) 呼叫，此呼叫包含 *productId* 及含括該項產品購買的交易。

>**重要**&nbsp;&nbsp;若未將已履行的消費性應用程式內產品報告給市集，將導致使用者無法再次購買該產品，必須等到回報已履行上次的購買後才能再購買。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#ReportFulfillment)]

## <a name="step-4-identifying-unfulfilled-purchases"></a>步驟 4：識別未履行的購買

您的 App 可以隨時使用 [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) 方法，來檢查是否有任何未履行的消費性應用程式內產品。 您應該定期呼叫這個方法，以檢查是否有任何因為未預期的應用程式事件 (例如網路連線中斷或應用程式終止) 而未履行的消費性產品存在。

下列範例示範如何使用 [GetUnfulfilledConsumablesAsync](https://msdn.microsoft.com/library/windows/apps/dn263379) 列舉未履行的消費性產品，以及您的 App 如何重複此清單來完成本機履行。

> [!div class="tabbedCodeSnippets"]
[!code-cs[EnableConsumablePurchases](./code/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs#GetUnfulfilledConsumables)]

## <a name="related-topics"></a>相關主題

* [啟用應用程式內產品購買](enable-in-app-product-purchases.md)
* [市集範例 (示範試用版和 app 內購買)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/br225197)
 

 

