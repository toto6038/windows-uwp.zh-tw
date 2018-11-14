---
author: Xansky
ms.assetid: 5E722AFF-539D-456E-8C4A-ADE90CF7674A
description: 如果您的 app 提供大型的 app 內產品型錄，您可以選擇性地依照本主題中描述的程序來協助管理型錄。
title: 管理大型的應用程式內產品型錄
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
keywords: Windows 10, UWP, app 內購買, IAP, 附加元件, 目錄, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: f57adf62939c28794e3ecdf6e59f2c4763de9c21
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2018
ms.locfileid: "6276006"
---
# <a name="manage-a-large-catalog-of-in-app-products"></a>管理大型的應用程式內產品型錄

如果您的 App 提供大型的應用程式內產品型錄，您可以選擇性地依照本主題中描述的程序來協助管理型錄。 在 Windows 10 之前的版本中，Microsoft Store 將每個開發人員帳戶的產品清單數目限制為 200 個，而本主題中所描述的處理程序可用來解決該限制。 從 windows 10 開始，「 市集 」 的每個開發人員帳戶的產品清單數目沒有限制，不再需要本文中所述的程序。

> [!IMPORTANT]
> 本文章示範如何使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間的成員。 此命名空間不再提供新功能更新，建議您改為使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間。 **Windows.Services.Store**命名空間支援最新的附加元件類型，例如市集管理的消費性附加元件和訂閱，並設計成與未來產品與合作夥伴中心和市集所支援的功能類型相容。 **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md)。

若要啟用此功能，您將針對特定的價格區間建立一些產品項目，其中每個產品項目都能夠代表型錄中的數百個產品。 使用 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 方法多載指定 App 定義的選項 (與 Microsoft Store 中所列的應用程式內產品相關聯)。 除了在呼叫期間指定購買選項和產品關聯，您的 App 也應該傳送包含大型型錄購買選項詳細資料的 [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384) 物件。 如果未提供這些詳細資料，即會改用所列出產品的詳細資料。

Microsoft Store 只會使用產生的 [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392) 中來自購買要求的 *offerId*。 這個程序不會直接修改[在 Microsoft Store 中列出應用程式內產品](../publish/add-on-submissions.md)時原本提供的資訊。

## <a name="prerequisites"></a>先決條件

-   本主題涵蓋使用 Microsoft Store 中列出的單一應用程式內產品呈現多個應用程式內的購買選項之 Microsoft Store 支援。 如果您不熟悉 App 內購買，請檢閱[啟用應用程式內產品購買](enable-in-app-product-purchases.md)，以了解授權資訊及如何在 Microsoft Store 中正確列出您的 App 內購買。
-   初次撰寫並測試新的 App 內購買選項程式碼時，您必須使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 物件，而不是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 物件。 如此一來，您就可以利用對授權伺服器進行模擬呼叫來驗證授權邏輯，而不是呼叫使用中的伺服器。 若要這樣做，您必須自訂 %userprofile%\\AppData\\local\\packages\\&lt;套件名稱&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中名為 WindowsStoreProxy.xml 的檔案。 Microsoft Visual Studio 模擬器會在您第一次執行您的 App 時建立這個檔案，或者您也可以在執行階段載入自訂的檔案。 如需詳細資訊，請參閱[使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。
-   本主題也會參照[ Microsoft Store 範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)中提供的程式碼範例。 這個範例非常適合用來體驗實機操作針對通用 Windows 平台 (UWP) app 提供的不同貨幣選項。

## <a name="make-the-purchase-request-for-the-in-app-product"></a>針對應用程式內產品提出購買要求

處理針對大型型錄內特定產品的購買要求時，方式與處理 App 內的任何其他購買要求大致相同。 您的 App 在呼叫新的 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 方法多載時，會提供 *OfferId*，以及當中已填入應用程式內產品名稱的 [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263390) 物件。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#MakePurchaseRequest)]

## <a name="report-fulfillment-of-the-in-app-offer"></a>回報 App 內購買選項的履行

當購買選項已在本機履行之後，您的應用程式需要儘速將產品履行回報給 Microsoft Store。 在大型型錄案例中，如果您的應用程式未回報購買選項已履行，使用者將無法使用該相同 Microsoft Store 產品清單來購買任何應用程式內的購買選項。

如先前所提及，Microsoft Store 只會使用提供的購買選項資訊來填入 [PurchaseResults](https://msdn.microsoft.com/library/windows/apps/dn263392)，不會在大型型錄購買選項和 Microsoft Store 產品清單之間建立永久性關聯。 因此，您需要追蹤使用者對產品的權益，並除了 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 操作之外，為使用者提供產品專屬內容 (例如所購買項目的名稱，或項目相關詳細資料)。

下列程式碼會示範履行呼叫，並示範當中插入特定購買選項資訊的 UI 訊息模式。 如果沒有該特定產品資訊，此範例就會使用來自產品 [ListingInformation](https://msdn.microsoft.com/library/windows/apps/br225163) 的資訊。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ManageCatalog](./code/InAppPurchasesAndLicenses/cs/ManageCatalog.cs#ReportFulfillment)]

## <a name="related-topics"></a>相關主題

* [啟用應用程式內產品購買](enable-in-app-product-purchases.md)
* [啟用消費性 app 內產品購買](enable-consumable-in-app-product-purchases.md)
* [Microsoft Store 範例 (示範試用版和 app 內購買)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [RequestProductPurchaseAsync](https://msdn.microsoft.com/library/windows/apps/dn263382)
* [ProductPurchaseDisplayProperties](https://msdn.microsoft.com/library/windows/apps/dn263384)
