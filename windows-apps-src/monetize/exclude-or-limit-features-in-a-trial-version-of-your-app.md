---
author: Xansky
Description: If you enable customers to use your app for free during a trial period, you can entice your customers to upgrade to the full version of your app by excluding or limiting some features during the trial period.
title: 在試用版本中排除或限制某些功能
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: Windows 10, UWP, 試用版, app 內購買, IAP, Windows.ApplicationModel.Store
ms.author: mhopkins
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18d9fb3ba5b0fbd1e964450a75d8e0e417265e7f
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5404697"
---
# <a name="exclude-or-limit-features-in-a-trial-version"></a>在試用版本中排除或限制某些功能

如果您讓客戶在試用期間免費使用 App，您可以在試用期間排除或限制某些功能，吸引客戶升級成完整版的 App。 開始撰寫程式碼之前應先決定要受到限制的功能，然後確定應用程式只有在購買完整授權後，才允許這些功能運作。 您也可以啟用橫幅或浮水印之類的功能，這些功能僅在客戶購買您的應用程式之前的試用期間顯示。

> [!IMPORTANT]
> 這篇文章示範如何使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間的成員來實作試用版功能。 此命名空間不再提供新功能更新，建議您改為使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間。 **Windows.Services.Store** 命名空間支援最新的附加元件類型 (例如 Microsoft Store 管理的消費性附加元件及訂閱)，並且設計成與 Windows 開發人員中心和 Microsoft Store 所支援的未來產品與功能類型相容。 **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如需使用 **Windows.Services.Store** 命名空間實作試用功能的詳細資訊，請參閱[本文](implement-a-trial-version-of-your-app.md)。

## <a name="prerequisites"></a>先決條件

要新增功能讓客戶購買的 Windows 應用程式。

## <a name="step-1-pick-the-features-you-want-to-enable-or-disable-during-the-trial-period"></a>步驟 1：挑選要在試用期間啟用或停用的功能

App 目前的授權狀態會儲存為 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 類別的屬性。 一般而言，您會將依存於授權狀態的函式放在條件性區塊中，如下個步驟所述。 考量這些功能時，請確定您實作功能的方式，可在所有授權狀態下運作。

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

如果您想要偵測授權變更並在 App 中執行一些動作，您必須按照下個步驟中的做法，新增事件處理常式。

## <a name="step-2-initialize-the-license-info"></a>步驟 2：初始化授權資訊

當您的 App 初始化時，為您的 App 取得 [LicenseInformation](https://msdn.microsoft.com/library/windows/apps/br225157) 物件，如此範例中所示。 我們假設 **licenseInformation** 是類型 **LicenseInformation** 的全域變數或欄位。

現在，您將要使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 而不是 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 取得模擬的授權資訊。 將 App 的發行版本提交到** Microsoft Store **之前，您必須將程式碼中所有的 **CurrentAppSimulator** 參考取代為 **CurrentApp**。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTest)]

接下來新增事件處理常式，以在 App 執行時接收授權變更的通知。 例如，如果試用期到期，或是客戶透過 Microsoft Store 購買 App，則 App 的授權會有所變更。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseTestWithEvent)]

## <a name="step-3-code-the-features-in-conditional-blocks"></a>步驟 3：以條件性區塊撰寫功能程式碼

授權變更事件觸發時，您的 App 必須呼叫授權 API 來判斷試用狀態是否有所變更。 此步驟中的程式碼顯示如何為此事件建構處理常式。 此時，如果使用者購買應用程式，最好可以對使用者提供授權狀態有所變更的回應。 根據程式碼的撰寫方式，您可能必須要求使用者重新啟動應用程式。 但請盡可能讓轉換流暢、輕鬆。

此範例顯示如何評估 App 的授權狀態，據以啟用或停用您 App 的功能。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#ReloadLicense)]

## <a name="step-4-get-an-apps-trial-expiration-date"></a>步驟 4：取得 App 的試用版到期日

納入可決定 App 試用版到期日的程式碼。

此範例中的程式碼定義的函式可以取得應用程式試用版授權的到期日。 如果授權仍然有效，就會顯示到期日與試用版到期之前的剩餘天數。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#DisplayTrialVersionExpirationTime)]

## <a name="step-5-test-the-features-using-simulated-calls-to-the-license-api"></a>步驟 5：透過模擬呼叫授權 API 來測試功能

現在使用模擬的資料來測試您的 App。 **CurrentAppSimulator** 會從位於 %UserProfile%\\AppData\\local\\packages\\&lt;套件名稱&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData 中，稱為 WindowsStoreProxy.xml 的 XML 檔案取得測試特定的授權資訊。 您可以編輯 WindowsStoreProxy.xml，變更您 App 與其功能的模擬到期日。 測試所有可能的到期日與授權組態，確認一切無誤。 如需詳細資訊，請參閱[使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。

如果這個路徑和檔案不存在，您必須在安裝或執行階段期間建立它們。 如果您嘗試存取 [CurrentAppSimulator.LicenseInformation](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.licenseinformation) 屬性，但該特定位置中卻沒有 WindowsStoreProxy.xml，則會發生錯誤。

## <a name="step-6-replace-the-simulated-license-api-methods-with-the-actual-api"></a>步驟 6：以實際的 API 取代模擬的授權 API 方法

以模擬的授權伺服器測試您的 App 之後，並在將 App 提交至 Microsoft Store 進行認證之前，請以 **CurrentApp** 取代 **CurrentAppSimulator**，如下一個程式碼範例所示。

> [!IMPORTANT]
> 在將您的 App 提交至 Microsoft Store 時，該 App 必須使用 **CurrentApp** 物件，否則將無法通過認證。

> [!div class="tabbedCodeSnippets"]
[!code-cs[TrialVersion](./code/InAppPurchasesAndLicenses/cs/TrialVersion.cs#InitializeLicenseRetailWithEvent)]

## <a name="step-7-describe-how-the-free-trial-works-to-your-customers"></a>步驟 7：為客戶說明免費試用版的運作方式

務必對客戶說明您的 App 在免費試用期間或到期之後的行為，客戶才不會對 App 的行為感到意外。

如需有關描述 app 的詳細資訊，請參閱[建立 app 描述](https://msdn.microsoft.com/library/windows/apps/mt148529)。

## <a name="related-topics"></a>相關主題

* [Microsoft Store 範例 (示範試用版和 app 內購買)](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [設定 app 價格與可用性](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 
