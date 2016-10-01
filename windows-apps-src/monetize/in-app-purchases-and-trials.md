---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "了解如何在 UWP app 中啟用 App 內購買和試用版。"
title: "App 內購買和試用版"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# App 內購買和試用版

Windows SDK 提供您可用來將 App 內購買和試用版功能新增到通用 Windows 平台 (UWP) App 的 API，以協助您的 App 獲利並加入新功能。 這些 API 也會提供您 App 授權資訊的存取。

對於這些案例中，Windows 10 提供兩個不同的 API：

* 所有版本的 Windows 10 都支援一個 API，可用來取得 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間中的 App 內購買和授權資訊。

* 從 Windows 10 版本 1607 開始，提供了一個替代的 API，可用來取得 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間中的 App 內購買和授權資訊。  

雖然這些命名空間中的 API 提供相同的目標，但其設計目的截然不同，且這兩個 API 之間的程式碼並不相容。 如果您的 App 目標為 Windows 10 版本 1607 或更新版本，我們建議您改用 **Windows.Services.Store** 命名空間。 這個命名空間支援最新的附加元件類型 (例如市集管理的消費性附加元件)，而設計目的是與 Windows 開發人員中心和市集所支援的未來產品與功能類型相容。 **Windows.Services.Store** 命名空間也已經改善效能。

本文介紹適用於 UWP App 的 App 內購買，並提供 **Windows.Services.Store** 命名空間的概觀，此命名空間是從 Windows 10 版本 1607 開始提供。 如需在 **Windows.ApplicationModel.Store** 命名空間中使用成員的相關資訊，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。


## UWP App 中 App 內購買的概觀

本節描述 App 內購買和試用版如何在市集中使用 UWP App 的核心概念。 這其中大部分的概念均適用於 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空間。

市集中提供的每個項目通常稱為「產品」**。 大部分的開發人員會使用下列類型的產品︰*App* 和 *附加元件* (也稱為 App 內產品或 IAP)。 附加元件是指您在 App 內容中提供給客戶使用的產品或功能。 附加元件可以代表您 App 提供給客戶的任何功能︰例如，在 App 或遊戲中使用的貨幣、適用於遊戲的新地圖或武器，能夠在沒有廣告的情況下使用您的 App，或者適用於能夠提供該類型內容之 App 的數位內容 (例如，音樂或視訊)。

每個 App 與附加元件都有相關的授權，可指出使用者是否有資格使用該 App 或附加元件。 如果使用者有資格使用 App 或附加元件，授權也會提供關於試用版的額外資訊。

若要在您的 App 中為客戶提供附加元件，可從[在開發人員中心儀表板定義 App 的附加元件](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)開始。 接著，在 App 中撰寫程式碼，以判斷使用者是否有權使用該附加元件所呈現的功能，如果使用者尚未取得該附加元件的授權，即可提供該附加元件進行銷售，讓使用者可以在 App 內購買。 如需示範在目標為 Windows 10 版本 1607 或更新版本的 App 中使用 **Windows.Services.Store** 命名空間的相關工作範例，請參閱下列文章︰

* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)

如需示範 **Windows.ApplicationModel.Store** 命名空間的相關工作範例，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

所有的開發人員都可以建立下列類型的附加元件。

| 附加元件類型 |  說明  |
|---------|-------------------|
| 耐久性  |  針對您在 [Windows 開發人員中心儀表板](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties)指定的存留期持續存在的附加元件。 <p/><p/>根據預設，耐久性附加元件永遠不會過期，在此情況下只需購買一次。 如果您為附加元件指定特定的持續時間，則使用者就能在該附加元件到期之後重新進行購買。  |
| 開發人員管理的消費性產品  |  可以購買、使用，然後再次購買的附加元件。 這種類型的附加元件通常用於 App 內貨幣。 <p/><p/>針對這種類型的消費性產品，您必須負責持續追蹤使用者對該附加元件所代表之項目的餘額，以及在使用者用完所有項目之後，向市集回報已完成附加元件的購買。 在您的 App 回報已完成先前的附加元件購買之前，使用者將無法再次購買該附加元件。 <p/><p/>例如，如果您的附加元件在遊戲中代表 100 個金幣，而使用者花費了 10 個金幣，則您的 App 或服務必須針對該使用者保留 90 個金幣的新餘額。 當使用者花光 100 個金幣之後，您的 App 必須回報該附加元件已完成，接著使用者就能再次購買 100 個金幣的附加元件。    |
| 市集管理的消費性產品  |  可以購買、使用，然後再次購買的附加元件。 這種類型的附加元件通常用於 App 內貨幣。<p/><p/>針對這種類型的消費性產品，市集會持續追蹤使用者對該附加元件所代表之項目的餘額。 當使用者取用任何項目時，您必須負責向市集回報這些項目已完成，而市集會更新使用者的餘額。 您的 App 可以隨時查詢使用者目前的餘額。 當使用者用完所有項目之後，該使用者就能再次購買該附加元件。  <p/><p/> 例如，如果您的附加元件在遊戲中代表最初的 100 個金幣數量，而使用者花費了 10 個金幣，則您的 App 會向市集回報已完成附加元件的 10 個單位，而市集會更新剩下的餘額。 當使用者花光 100 個金幣之後，該使用者就能再次購買 100 個金幣的附加元件。 <p/><p/> **市集管理的消費性產品是從 Windows 10 版本 1607 開始提供。 即將推出在 Windows 開發人員中心儀表板建立市集管理的消費性產品的能力。**  |

<span />

>**注意**&nbsp;&nbsp;其他類型的附加元件 (例如，具有套件的耐久性附加元件，也稱為可下載內容或 DLC) 僅可供一組有限的開發人員使用，而其並未未涵蓋於本文件中。

<span id="api_intro" />
## Windows.Services.Store 命名空間的簡介

**Windows.Services.Store** 命名空間的主要進入點是 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別。 這個類別會提供您可用來取得目前 App 及其可用附加元件的資訊、購買目前使用者適用的 App 或附加元件、取得目前 App 或及附加元件的授權資訊，以及其他工作。 若要取得 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件，請執行下列其中一項︰

* 在單一使用者 App (也就是只在啟動 App 的使用者內容中執行的 App)，使用 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 方法來取得 **StoreContext** 物件，您可以用來為使用者存取和管理 Windows 市集相關的資料。 大部分的通用 Windows 平台 (UWP) App 都是單一使用者 App。

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* 在多使用者 App 中，使用 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 方法來取得 **StoreContext** 物件，您可以針對在使用 App 時，使用其 Microsoft 帳戶登入的特定使用者，用來存取和管理 Windows 市集相關的資料。 如需多使用者 App 的詳細資訊，請參閱[多使用者應用程式的簡介](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications)。 下列範例會針對第一位可用的使用者取得 **StoreContext** 物件。

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

擁有 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 之後，就可以開始呼叫方法，來購買適用於目前使用者與其他工作的 App 和附加元件。 如需詳細資訊，請參閱下列文章：

* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)

如需示範如何使用 **Windows.Services.Store** 命名空間實作試用版和 App 內購買的完整範例，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span />
### 產品、SKU 和可用性的物件模型

市集中的每個產品至少會有一個「SKU」**，而每個 SKU 至少會有一個「可用性」**。 這些概念對於 Windows 開發人員中心儀表板中的大多數開發人員而言是非常抽象的，而且大多數開發人員永遠都不會為他們的 App 或附加元件定義 SKU 或可用性。 不過，由於 **Windows.Services.Store** 命名空間中適用於市集產品的物件模型包含了 SKU 和可用性，因此對這些概念有基本了解是很有幫助的。

| 物件類型 |  說明  |
|---------|-------------------|
| 產品  |  [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 類別代表可在市集中使用的任何類型產品，包括 App 或附加元件。 這個類別所提供的屬性，可讓您用來存取產品市集識別碼等資料、適用於市集清單的影像和視訊，以及定價資訊。 它也提供您可以用來購買該產品的方法。 |
| SKU |  [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 類別代表產品的 *SKU*。 SKU 是一個特定版本的產品，具有自己的描述、價格，以及其他獨特的產品詳細資料。 每個 App 或附加元件都有預設的 SKU。 大多數開發人員會對一個 App 提供多個 SKU 的唯一時刻是在其發行完整版的 App 和試用版時 (在市集型錄中，這其中每一個版本都是相同 App 的不同 SKU)。 <p/><p/> 某些發行者可以定義自己的 SKU。 例如，大型遊戲發行者可能在不允許紅色血液的市場中使用一個顯示綠色血液的 SKU 來發佈遊戲，並在所有其他市場中使用另一個顯示紅色血液的 SKU 來發佈。 或者，銷售數位視訊內容的發行者可能針對某個視訊發佈兩個 SKU，一個 SKU 適用於高解析度版本，而另一個 SKU 適用於標準畫質版本。 <p/><p/> 每個產品都會有 [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) 屬性，讓您可用來存取 SKU。 |
| 可用性  |  [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 類別代表 SKU 的「可用性」**。 可用性是一個特定版本的 SKU，具有自己獨特的定價資訊。 每個 SKU 都有預設的可用性。 某些發行者能夠定義自己的可用性，來介紹指定 SKU 的不同價格選項。 <p/><p/> 每個 SKU 都會有 [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) 屬性，可讓您用來存取可用性。 對於大部分開發人員而言，每個 SKU 都會有一個預設的可用性。  |

<span id="store_ids" />
### 市集識別碼

市集中的每個 App 與附加元件都會有一個相關聯的**市集識別碼**。 **Windows.Services.Store** 命名空間中的許多 API 需要有市集識別碼，才能在 App 或附加元件上執行操作。 產品、SKU 和可用性具有不同的市集識別碼格式。

| 物件類型 |  市集識別碼格式  |
|---------|-------------------|
| 產品  |  市集中任何產品的市集識別碼都是 12 個字元的英數字串，例如 ```9NBLGGH4R315```。 此市集識別碼適用於 App 或附加元件的 Windows 開發人員中心儀表板頁面，而且是由 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) 屬性所傳回。 這個識別碼有時也稱為「產品市集識別碼」**。 |
| SKU |  針對 SKU，市集識別碼的格式為 ```<product Store ID>/xxxx```，其中 ```xxxx``` 是 4 個字元的英數字串，可識別產品的 SKU。 例如，```9NBLGGH4R315/000N```。 這個識別碼是由 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 物件的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) 屬性所傳回，而且有時稱為「SKU 市集識別碼」**。 |
| 可用性  |  針對可用性，市集識別碼的格式為 ```<product Store ID>/xxxx/yyyyyyyyyyyy```，其中 ```xxxx``` 是 4 個字元的英數字串，可識別產品的 SKU，而 ```yyyyyyyyyyyy``` 是 12 個字元的英數字串，可識別 SKU 的可用性。 例如，```9NBLGGH4R315/000N/4KW6QZD2VN6X```。 這個識別碼是由 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 物件的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) 屬性所傳回，而且有時稱為「可用性市集識別碼」**。  |

<span id="testing" />
### 測試使用 Windows.Services.Store 命名空間的 App

**Windows.Services.Store** 命名空間不提供您可在測試期間用來模擬授權資訊的類別。 您必須改為將 App 發行到市集，並將該 App 下載到您的開發裝置，以使用它的授權進行測試。 這與使用 **Windows.ApplicationModel.Store** 命名空間的 App 是不同的體驗，因為這些 App 可以使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別，在測試期間模擬授權資訊

如果您的 App 使用 **Windows.Services.Store** 命名空間中的 API 來存取 App 及其附加元件的資訊，請遵循下列程序來測試您的程式碼：

1. 如果您的 App 已經發行且可在市集中取得，而您想要更新此 App 以使用 **Windows.Services.Store** 命名空間中的 API，則您已經準備好開始使用。 如果您想要為 App 提供附加元件，請確定您會在開發人員中心儀表板[定義 App 的附加元件](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)。

  如果您尚未發行 App 且無法在市集中取得，則建置基本 App 以符合最低 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)需求，並[提交這個 App](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) 到 Windows 開發人員中心儀表板。 如果您想要為 App 提供附加元件，請確定您會[定義 App 的附加元件](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions)。 您可以在測試期間選擇性地[從市集隱藏 App](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)。

2. 在您的 App 中撰寫程式碼，使用 **Windows.Services.Store** 命名空間的其中一個 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法來執行工作，例如[取得目前 App 可用的附加元件](get-product-info-for-apps-and-add-ons.md)、[購買 App 或附加元件](enable-in-app-purchases-of-apps-and-add-ons.md)或[取得 App 的授權資訊](get-license-info-for-apps-and-add-ons.md)。 如需其他範例，請參閱下方的＜相關主題＞一節。

3. 在 Visual Studio 中，按一下 [專案] 功能表****、指向 [市集]****，然後按一下 [將應用程式與市集建立關聯]****。 完成精靈中的指示，將 App 專案與您想要在 Windows 開發人員中心帳戶中用來測試的 App 建立關聯。

  >**注意**&nbsp;&nbsp;如果您不會將專案與市集中的 App 建立關聯，[StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法就會將其傳回值的 **ExtendedError** 屬性設定為錯誤碼值 0x803F6107。 這個值表示市集沒有任何關於該 App 的知識。

4. 如果您尚未執行此動作，從市集中安裝您在上一個步驟指定的 App、執行一次 App，然後關閉此 App。 這可確保 App 的有效授權已安裝於您的開發裝置上。

5. 在 Visual Studio 中，開始執行或偵錯您的專案。 您的程式碼應該從與您本機專案相關聯的市集 App 中擷取 App 和附加元件資料。 如果系統提示您重新安裝 App，請依照下列指示進行，然後執行或偵錯您的專案。

## 相關主題

* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)
* [使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


