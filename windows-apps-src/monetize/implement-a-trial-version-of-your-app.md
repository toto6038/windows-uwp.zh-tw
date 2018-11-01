---
author: Xansky
ms.assetid: 571697B7-6064-4C50-9A68-1374F2C3F931
description: 了解如何使用 Windows.Services.Store 命名空間來實作 App 的試用版。
title: 實作應用程式的試用版
keywords: Windows 10, UWP, 試用版, App 內購買, Windows.Services.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b078f0573d94e80b590f473f7c9a9b0395115ee
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5876841"
---
# <a name="implement-a-trial-version-of-your-app"></a>實作應用程式的試用版

如果您將 App [在 Windows 開發人員中心儀表板中設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)，讓客戶可在試用期間免費使用您的 App，您將可藉由在試用期間排除或限制某些功能，來吸引客戶升級成完整版的 App。 開始撰寫程式碼之前，請先決定哪些功能應受到限制，然後確定只有在客戶購買完整授權後，App 才會允許這些功能運作。 您也可以啟用橫幅或浮水印之類的功能，這些功能僅在客戶購買您的 App 之前的試用期間顯示。

本文顯示如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別的成員，以判斷使用者是否有您應用程式的試用授權，而且如果授權狀態在應用程式執行期間變更，則會收到通知。 

> [!NOTE]
> **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如果您的 app 目標為較早版本的 Windows 10，您必須使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間，而不是 **Windows.Services.Store** 命名空間。 如需詳細資訊，請參閱[這篇文章](exclude-or-limit-features-in-a-trial-version-of-your-app.md)。

## <a name="guidelines-for-implementing-a-trial-version"></a>實作試用版的指導方針

App 目前的授權狀態會儲存為 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 類別的屬性。 一般而言，您會將依存於授權狀態的函式放在條件性區塊中，如下個步驟所述。 考量這些功能時，請確定您實作功能的方式，可在所有授權狀態下運作。

此外，決定您在應用程式執行時要如何處理應用程式授權的變更。 您的試用版應用程式可具備完整功能，但應用程式內會有付費版本所沒有的廣告橫幅。 或者，試用版應用程式可以停用特定功能，或是定期顯示訊息，詢問使用者是否要購買。

考慮您正在製作的應用程式類型，以及適合採用哪種試用或到期策略。 對於遊戲試用版，採用的策略最好是限制使用者可玩的遊戲內容。 對於試用版公用程式，您可考慮設定到期日，或是限制潛在買家會使用的功能。

對於大部分非遊戲類型的應用程式，設定到期日是一種很好的做法，因為使用者可以對完整的應用程式有比較深入的了解。 這裡提供幾個常見的到期日案例以及您如何處理的選項。

-   **試用授權在應用程式執行時到期**

    如果您的應用程式正在執行時試用到期，應用程式可以：

    -   什麼也不做。
    -   對客戶顯示訊息。
    -   關閉。
    -   提示客戶購買應用程式。

    最佳做法是顯示一個提示購買應用程式的訊息；如果客戶購買應用程式，就啟用所有功能讓他們繼續使用。 如果客戶決定不要購買，就關閉應用程式，或定期提示他們購買應用程式。

-   **試用授權在應用程式啟動之前到期**

    如果試用期在使用者啟動應用程式之前就已到期，應用程式將無法啟動。 使用者將會看到一個對話方塊，提供他們從 Microsoft Store 購買應用程式的選項。

-   **客戶在應用程式執行時購買應用程式**

    如果客戶在您的應用程式執行時購買它，這裡是您應用程式可以採取的動作。

    -   什麼也不做，讓客戶在試用模式下繼續使用，直到重新啟動應用程式。
    -   感謝他們購買，或是顯示一則訊息。
    -   不顯示訊息直接啟用完整授權的所有功能 (或停用試用版通知)。

務必對客戶說明您的 App 在免費試用期間或到期之後的行為，客戶才不會對 App 的行為感到意外。 如需有關描述 App 的詳細資訊，請參閱[建立 App 描述](https://msdn.microsoft.com/library/windows/apps/mt148529)。

## <a name="prerequisites"></a>先決條件

這個範例包含下列先決條件：
* 適用於目標為 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的通用 Windows 平台 (UWP) App 的 Visual Studio 專案。
* 您已在 Windows 開發人員中心儀表板中建立應用程式，並將其設定為沒有時間限制的[免費試用](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)，而且已在 Microsoft Store 中發佈此應用程式。 測試時您也可以選擇將應用程式設定為不可在 Microsoft Store 中搜尋。 如需詳細資訊，請參閱我們的[測試指南](in-app-purchases-and-trials.md#testing)。

這個範例中的程式碼假設：
* 程式碼會在 [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx) 的內容中執行，其中包含名為 ```workingProgressRing``` 的 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx) 和名為 ```textBlock``` 的 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)。 這些物件可個別用來表示發生非同步作業，以及顯示輸出訊息。
* 程式碼檔案含有適用於 **Windows.Services.Store** 命名空間的 **using** 陳述式。
* App 是單一使用者 app，僅會在啟動 app 的使用者內容中執行。 如需詳細資訊，請參閱 [App 內購買和試用版](in-app-purchases-and-trials.md#api_intro)。

> [!NOTE]
> 如果您的傳統型應用程式使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，您可能需要新增此範例中未顯示的額外程式碼來設定 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](in-app-purchases-and-trials.md#desktop)。

## <a name="code-example"></a>程式碼範例

當您的 App 進行初始化時，請取得 App 的 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 物件，並處理 [OfflineLicensesChanged](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.offlinelicenseschanged) 事件，以在授權於 App 執行期間變更時收到通知。 例如，如果試用期到期，或是客戶透過 Microsoft Store 購買 App，則 App 的授權會有所變更。 當授權變更時，取得新的授權，並據以啟用或停用您的 App 功能。

此時，如果使用者購買 App，最好可以對使用者提供授權狀態有所變更的回應。 根據程式碼的撰寫方式，您可能必須要求使用者重新啟動應用程式。 但請盡可能讓轉換流暢、輕鬆。

> [!div class="tabbedCodeSnippets"]
[!code-cs[ImplementTrial](./code/InAppPurchasesAndLicenses_RS1/cs/ImplementTrialPage.xaml.cs#ImplementTrial)]

如需完整的範例應用程式，請參閱 [Microsoft Store 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

## <a name="related-topics"></a>相關主題

* [App 內購買和試用版](in-app-purchases-and-trials.md)
* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
