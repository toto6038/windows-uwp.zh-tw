---
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: 了解如何使用 Windows.Services.Store 命名空間，來取得目前 app 及其附加元件的授權資訊。
title: 取得應用程式和附加元件的授權資訊
ms.date: 12/04/2017
ms.topic: article
keywords: Windows 10, UWP, 授權, 應用程式, 附加元件, App 內購買, IAP, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 2c83ecc7b5bfc8158d80469eab12710719fa0834
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164672"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>取得應用程式和附加元件的授權資訊

本文示範如何使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空間中的 [StoreContext](/uwp/api/windows.services.store.storecontext) 類別的方法，來取得目前前應用程式及其附加元件的授權資訊。 例如，您可以使用這項資訊來判斷 app 或其附加元件的授權是否有效，或者它們是否為試用版授權。

> [!NOTE]
> **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[這篇文章](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

## <a name="prerequisites"></a>先決條件

這個範例包含下列先決條件：
* 適用於目標為 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您已在合作夥伴中心中 [建立提交應用](../publish/app-submissions.md) 程式，並在存放區中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在市集中搜尋。 如需詳細資訊，請參閱我們的[測試指南](in-app-purchases-and-trials.md#testing)。
* 如果您想要取得應用程式附加元件的授權資訊，您也必須 [在合作夥伴中心中建立附加](../publish/add-on-submissions.md)元件。

這個範例中的程式碼假設：
* 程式碼會在 [Page](/uwp/api/windows.ui.xaml.controls.page) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](/uwp/api/windows.ui.xaml.controls.progressring) 和名為 ```textBlock``` 的 [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增此範例中未顯示的額外程式碼來設定 [StoreContext](/uwp/api/windows.services.store.storecontext) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

## <a name="code-example"></a>程式碼範例

若要取得目前 App 的授權資訊，請使用 [GetAppLicenseAsync](/uwp/api/windows.services.store.storecontext.getapplicenseasync) 方法。 這是一個非同步方法，會傳回 [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense) 物件來提供應用程式的授權資訊，包括指出使用者目前是否具備使用應用程式的有效授權 ([IsActive](/uwp/api/windows.services.store.storeapplicense.isactive)) 及該授權是否是試用版授權 ([IsTrial](/uwp/api/windows.services.store.storeapplicense.istrial)) 的屬性。

若要存取使用者具有使用權利的目前應用程式的持久附加元件授權，請使用 [StoreAppLicense](/uwp/api/windows.services.store.storeapplicense)物件的 [AddOnLicenses](/uwp/api/windows.services.store.storeapplicense.addonlicenses) 屬性。 此屬性會傳回代表附加元件授權的集合 [StoreLicense](/uwp/api/windows.services.store.storelicense) 物件。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

如需完整的範例應用程式，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

## <a name="related-topics"></a>相關主題

* [應用程式內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [啟用應用程式和附加元件的應用程式內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作應用程式的試用版](implement-a-trial-version-of-your-app.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)