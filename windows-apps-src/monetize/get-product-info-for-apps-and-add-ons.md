---
author: Xansky
ms.assetid: 89178FD9-850B-462F-9016-1AD86D1F6F7F
description: 了解如何使用 Windows.Services.Store 命名空間，來取得目前 app 或其中一個附加元件的 Microsoft Store 相關產品資訊。
title: 取得應用程式和附加元件的產品資訊
ms.author: mhopkins
ms.date: 02/08/2018
ms.topic: article
keywords: Windows 10, UWP, App 內購買, IAP, 附加元件, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: f1544ee3404e77ec7565c626a6ca96e439832c90
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287875"
---
# <a name="get-product-info-for-apps-and-add-ons"></a>取得應用程式和附加元件的產品資訊

本文示範如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的方法，來存取目前應用程式及其中一個附加元件的 Microsoft Store 相關資訊。

如需完整的範例應用程式，請參閱 [Microsoft Store 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

> [!NOTE]
> **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[這篇文章](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## <a name="prerequisites"></a>必要條件

這些範例包含下列先決條件：
* 適用於目標為 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您必須在合作夥伴中心的 [[建立應用程式提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)，並在市集中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在 Microsoft Store 中搜尋。 如需詳細資訊，請參閱我們的[測試指南](in-app-purchases-and-trials.md#testing)。
* 如果您想要取得應用程式的附加元件的產品資訊，您也必須[建立在合作夥伴中心的附加元件](../publish/add-on-submissions.md)。

這些範例中的程式碼假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) 和名為 ```textBlock``` 的 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增這些範例中未顯示的額外程式碼來設定 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

## <a name="get-info-for-the-current-app"></a>取得目前 App 的資訊

若要取得目前 app 的 Microsoft Store 產品資訊，請使用 [GetStoreProductForCurrentAppAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductforcurrentappasync) 方法。 這是傳回 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件的非同步方法，您可以使用該物件來取得資訊，例如價格。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAppInfoPage.xaml.cs#GetAppInfo)]

## <a name="get-info-for-add-ons-with-known-store-ids-that-are-associated-with-the-current-app"></a>取得具有與目前應用程式相關聯的已知 Microsoft Store 識別碼的附加元件資訊

若要針對您已經知道 [Microsoft Store 識別碼](in-app-purchases-and-trials.md#store_ids)的與目前應用程式相聯之附加元件的 Microsoft Store 產品資訊，請使用 [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync) 方法。 這是傳回 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件集合的非同步方法，其代表每個附加元件。 除了 Store 識別碼，您還必須將一個字串清單傳入此方法，以識別附加元件的類型。 如需支援的字串值清單，請參閱 [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind) 屬性。

> [!NOTE]
> **GetStoreProductsAsync**方法傳回與應用程式相關聯之指定附加元件的產品資訊，無論目前是否能買到附加元件。 若要擷取的目前目前可購買的應用程式的所有附加元件相關資訊，請改為使用 [下列區段](#get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app)中所述的 **GetAssociatedStoreProductsAsync** 方式。

此範例利用與目前應用程式相關聯之指定 Microsoft Store 識別碼來擷取耐久性附加元件的資訊。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetProductInfoPage.xaml.cs#GetProductInfo)]

## <a name="get-info-for-add-ons-that-are-available-for-purchase-from-the-current-app"></a>從目前應用程式取得可購買的附加元件資訊

若要針對目前應用程式可購買的附加元件取得 Microsoft Store 產品資訊，請使用 [GetAssociatedStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getassociatedstoreproductsasync) 方法。 這是傳回 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件集合的非同步方法，其代表每個可用的附加元件。 您必須將一個字串清單傳入此方法，以識別您想要擷取的附加元件類型。 如需支援的字串值清單，請參閱 [ProductKind](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.productkind) 屬性。

> [!NOTE]
> 如果應用程式有很多可購買的附加元件，您也可以使用 [GetAssociatedStoreProductsWithPagingAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreContext.GetAssociatedStoreProductsWithPagingAsync) 方法，使用分頁來傳回附加元件結果。

下列範例會擷取適用於所有耐久性附加元件、Microsoft Store 管理的消費性附加元件，以及開發人員管理的消費性附加元件的相關資訊，這些均可從目前應用程式購買。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetAddOnInfoPage.xaml.cs#GetAddOnInfo)]


## <a name="get-info-for-add-ons-for-the-current-app-that-the-user-has-purchased"></a>取得使用者已購買之目前應用程式的附加元件資訊

若要針對目前使用者已購買的目前應用程式取得附加元件的 Microsoft Store 產品資訊，請使用 [GetUserCollectionAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionasync) 方法。 這是傳回 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件集合的非同步方法，其代表每個附加元件。 您必須將一個字串清單傳入此方法，以識別您想要擷取的附加元件類型。 如需支援的字串值清單，請參閱 [ProductKind](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.productkind.aspx) 屬性。

> [!NOTE]
> 如果 App 有很多附加元件，您也可以使用 [GetUserCollectionWithPagingAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getusercollectionwithpagingasync) 方法，使用分頁來傳回附加元件結果。

下列範例利用指定的 [Store 識別碼](in-app-purchases-and-trials.md#store_ids)來擷取耐久性附加元件的資訊。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetProductInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetUserCollectionPage.xaml.cs#GetUserCollection)]

## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
