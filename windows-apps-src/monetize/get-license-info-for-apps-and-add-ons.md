---
author: mcleanbyron
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: "了解如何使用 Windows.Services.Store 命名空間，來取得目前 app 及其附加元件的授權資訊。"
title: "取得 App 和附加元件的授權資訊"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 0482cc192eeff4d3633898b6fa677805c635c6e1

---

# <a name="get-license-info-for-apps-and-add-ons"></a>取得 App 和附加元件的授權資訊

目標為 Windows&nbsp;10 版本 1607 或更新版本的 App 可以使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的方法，來取得目前 App 及其附加元件 (也稱為 App 內產品或 IAP) 的授權資訊。 例如，您可以使用這項資訊來判斷 app 或其附加元件的授權是否有效，或者它們是否為試用版授權。

>**注意** &nbsp;&nbsp;本文適用於目標為 Windows&nbsp;10 版本 1607 或更新版本的 app。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)

## <a name="prerequisites"></a>先決條件

這個範例包含下列先決條件：
* 適用於目標為 Windows&nbsp;10 版本 1607 或更新版本的通用 Windows 平台 (UWP) app 的 Visual Studio 專案。
* 您已在 Windows 開發人員中心儀表板中建立 app，而且已在市集中發佈此 app 且可供使用。 這可以是您想要釋出給客戶的 app，或者可以是符合最低 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)需求的基本 app，而您只能基於測試目的加以使用。 如需詳細資訊，請參閱[測試指導方針](in-app-purchases-and-trials.md#testing)。

這個範例中的程式碼假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) 和名為 ```textBlock``` 的 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

>**注意**&nbsp;&nbsp;如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增此範例中未顯示的額外程式碼來設定 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

## <a name="code-example"></a>程式碼範例

若要取得目前 App 的授權資訊，請使用 [GetAppLicenseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getapplicenseasync.aspx) 方法。 這是一個非同步方法，會傳回 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 物件來提供 App 的授權資訊，包括指出使用者是否具備使用 App 的授權 ([IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.isactive.aspx)) 及該授權是否是試用版授權 ([IsTrial](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.istrial.aspx)) 的屬性。

若要擷取 App 的附加元件授權，請使用 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 物件的 [AddOnLicenses](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.addonlicenses.aspx) 屬性。 此屬性會傳回代表 App 附加元件授權的集合 [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) 物件。 若要判斷使用者是否具備使用附加元件的授權，請使用 [IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.isactive.aspx) 屬性。

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

如需完整的範例應用程式，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)



<!--HONumber=Dec16_HO1-->


