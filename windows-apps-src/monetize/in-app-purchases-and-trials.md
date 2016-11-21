---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "了解如何在 UWP app 中啟用 App 內購買和試用版。"
title: "App 內購買和試用版"
translationtype: Human Translation
ms.sourcegitcommit: 09a123de7b3c31e2de6a3e43d44c648e7cc94da4
ms.openlocfilehash: ef0764aaf772fc36c6a6266e103cb35cafa841c5

---

# App 內購買和試用版

Windows SDK 提供您可用來實作下列功能以從您的「通用 Windows 平台」(UWP) app 提升獲利的 API。

* 
            **App 內購買**
            &nbsp;&nbsp;無論您的 App 是否免費，您都可以直接從 App 內銷售內容或新的 App 功能 (例如將遊戲的下一個關卡解除鎖定)。

* 
            **試用功能**
            &nbsp;&nbsp;如果您將 App [在 Windows 開發人員中心儀表板中設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)，您將可藉由在試用期間排除或限制某些功能，來吸引客戶購買完整版的 App。 您也可以啟用橫幅或浮水印之類的功能，這些功能僅在客戶購買您的 App 之前的試用期間顯示。

本文提供 UWP app 中 App 內購買和試用版的運作方式概觀。

<span id="choose-namespace" />
## 選擇要使用的命名空間

您可以根據您 App 的目標是哪一個 Windows10 版本，使用兩種不同的命名空間將 App 內購買和試用版功能新增到 UWP app。 雖然這些命名空間中的 API 都是為相同的目標服務，但其設計方式截然不同，且兩個 API 之間的程式碼並不相容。

* 
            **
              [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)
            **
            &nbsp;&nbsp;從 Windows10 版本 1607 開始，App 可以使用此命名空間中的 API 來實作 App 內購買和試用版。 如果您 App 的目標為 Windows10 版本 1607 或更新版本，建議您使用此命名空間中的成員。 這個命名空間支援最新的附加元件類型 (例如市集管理的消費性附加元件)，且設計成與「Windows 開發人員中心」和「市集」所支援的未來產品與功能類型相容。 如需有關此命名空間的詳細資訊，請參閱本文中的[使用 Windows.Services.Store 命名空間](#api_intro)一節。

* 
            **
              [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)
            **
            &nbsp;&nbsp;所有 Windows10 版本也都支援一個較舊的 API 來用於此命名空間中的 App 內購買和試用版。 雖然 Windows10 的任何 UWP app 都可以使用此命名空間，但此命名空間無法被更新來支援未來「開發人員中心」和「市集」中新類型的產品與功能。 如需有關此命名空間的資訊，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

<span id="concepts" />
## 基本概念

本節介紹 UWP app 中 App 內購買和試用版的基本概念。 這其中大部分的概念除非另有註明，否則均同時適用於 **Windows.Services.Store** 和 **Windows.ApplicationModel.Store** 命名空間。

「市集」中提供的每個項目通常稱為「產品」。 大部分的開發人員會使用下列類型的產品︰*App* 和 *附加元件* (也稱為 App 內產品或 IAP)。

附加元件是指您在 App 內容中提供給客戶使用的產品或功能。 附加元件可以代表您 App 提供給客戶的任何功能︰例如，在 App 或遊戲中使用的貨幣、適用於遊戲的新地圖或武器，能夠在沒有廣告的情況下使用您的 App，或者適用於能夠提供該類型內容之 App 的數位內容 (例如，音樂或視訊)。 每個 App 與附加元件都有相關的授權，可指出使用者是否有資格使用該 App 或附加元件。 如果使用者有資格使用 App 或附加元件，授權也會提供關於試用版的額外資訊。

若要在您的 App 中為客戶提供附加元件，您必須[在開發人員中心儀表板中定義 App 的附加元件](../publish/iap-submissions.md)，讓「市集」知道它。 接著，您的 App 便可以使用 **Windows.Services.Store** 或 **Windows.ApplicationModel.Store** 命名空間中的 API，以 App 內購買形式提供要對使用者銷售的附加元件。

UWP app 可以提供下列類型的附加元件。

| 附加元件類型 |  說明  |
|---------|-------------------|
| 耐久性  |  針對您在 [Windows 開發人員中心儀表板](../publish/enter-iap-properties.md)指定的存留期持續存在的附加元件。 <p/><p/>根據預設，耐久性附加元件永遠不會過期，在此情況下只需購買一次。 如果您為附加元件指定特定的持續時間，則使用者就能在該附加元件到期之後重新進行購買。 |
| 開發人員管理的消費性產品  |  可供購買、使用，然後再次購買的附加元件。 這種類型的附加元件通常用於 App 內貨幣。 <p/><p/>針對這種類型的消費性產品，您需負責記錄使用者在該附加元件所代表項目的餘額，以及負責在使用者取用所有項目之後，向「市集」回報已完全交付此附加元件的購買。 使用者必須等到您的 App 將先前的附加元件購買回報為已完全交付之後，才能再次購買該附加元件。 <p/><p/>例如，如果您的附加元件在遊戲中代表 100 個金幣，而使用者花費了 10 個金幣，則您的 App 或服務必須針對該使用者保留 90 個金幣的新餘額。 當使用者花光 100 個金幣之後，您的 App 必須回報該附加元件已完成，接著使用者就能再次購買 100 個金幣的附加元件。    |
| 市集管理的消費性產品  |  可供購買、使用，然後再次購買的附加元件。 這種類型的附加元件通常用於 App 內貨幣。<p/><p/>針對這種類型的消費性產品，「市集」會記錄使用者在該附加元件所代表項目的餘額。 當使用者取用任何項目時，您必須負責向市集回報這些項目已完成，而市集會更新使用者的餘額。 您的 App 可以隨時查詢使用者目前的餘額。 當使用者用完所有項目之後，該使用者就能再次購買該附加元件。  <p/><p/> 例如，如果您的附加元件在遊戲中代表最初的 100 個金幣數量，而使用者花費了 10 個金幣，則您的 App 會向市集回報已完成附加元件的 10 個單位，而市集會更新剩下的餘額。 當使用者花光 100 個金幣之後，該使用者就能再次購買 100 個金幣的附加元件。 <p/><p/>
            **注意**
            &nbsp;&nbsp;市集管理的消費性產品是從 Windows10 版本 1607 開始提供。 若要使用市集管理的消費性產品，您的 App 必須以 Windows10 版本 1607 或更新版本為目標，而且必須使用 **Windows.Services.Store** 命名空間中的 API，而不是 **Windows.ApplicationModel.Store** 命名空間中的 API。  |

<span />

>
            **注意**
            &nbsp;&nbsp;其他類型的附加元件 (例如，具有套件的耐久性附加元件，也稱為可下載內容或 DLC) 僅可供一組有限的開發人員使用，而本文件中並未涵蓋這個部分。

<span id="api_intro" />
## 使用 Windows.Services.Store 命名空間

本文的其餘部分將說明如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間來實作 App 內購買和試用版。 此命名空間僅適用於目標為 Windows10 版本 1607 或更新版本的 App，建議您儘可能讓 App 使用此命名空間，而不使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間。

如需您正在尋找 **Windows.ApplicationModel.Store** 命名空間的相關資訊，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

### 開始使用 StoreContext 類別


            **Windows.Services.Store** 命名空間的主要進入點是 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別。 這個類別會提供一些方法，可供您用來取得目前 App 及其可用附加元件的資訊、取得目前 App 或其附加元件的授權資訊、為目前的使用者購買 App 或附加元件，以及執行其他工作。 若要取得 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件，請執行下列其中一項操作︰

* 在單一使用者 App (也就是只在啟動 App 的使用者內容中執行的 App)，使用 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) 方法來取得 **StoreContext** 物件，您可以用來為使用者存取和管理 Windows 市集相關的資料。 大多數「通用 Windows 平台」(UWP) app 都是單一使用者 App。

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* 在[多使用者 App](../xbox-apps/multi-user-applications.md) 中，請使用 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) 方法來取得 **StoreContext** 物件，您可以針對在使用 App 時使用其 Microsoft 帳戶來登入的特定使用者，使用此物件來存取和管理其「Windows 市集」相關資料。 下列範例會針對第一位可用的使用者取得 **StoreContext** 物件。

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```
>
            **注意**
            &nbsp;&nbsp;使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)的 Windows 傳統型應用程式必須先執行額外的步驟來設定 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件，才能使用此物件。 如需詳細資訊，請參閱[在使用傳統型橋接器的傳統型應用程式中使用 StoreContext 類別](#desktop)。

在您具備 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件之後，您可以開始呼叫方法來取得目前 App 及其附加元件的「市集」產品資訊、擷取目前 App 及其附加元件的授權資訊、為目前的使用者購買 App 或附加元件，以及執行其他工作。 如需有關您可以使用此命名空間來執行之常見工作的詳細資訊，請參閱下列文章：

* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)

如需示範如何使用 **Windows.Services.Store** 命名空間的範例 App，請參閱[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span id="implement-iap" />
### 實作 App 內購買

使用 **Windows.Services.Store** 命名空間在您的 App 中為客戶提供 App 內購買：

1. 如果您的 App 提供客戶可購買的附加元件，請[在開發人員中心儀表板中為 App 建立附加元件提交項目](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。

2. 請在您的 App 中撰寫程式碼以[擷取您 App 或您 App 所提供之附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)，然後[判斷授權是否有效](get-license-info-for-apps-and-add-ons.md) (亦即，使用者是否具有可使用 App 或附加元件的授權)。 如果授權無效，請顯示一個 UI 來以 App 內購買形式為使用者提供要銷售的 App 或附加元件。

3. 如果使用者選擇購買您的 App 或附加元件，請使用適當的方法來購買該產品。 如果使用者要購買您的 App 或耐久性附加元件，請依照[啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)中的程序操作。 如果使用者要購買消費性附加元件，請依照[啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)中的指示操作。

4. 依照本文中的[測試指導方針](#testing)來測試您的實作。

<span id="implement-trial" />
### 實作試用版功能

使用 **Windows.Services.Store** 命名空間在您的 App 試用版中排除或限制功能：

1. 
            [在 Windows 開發人員中心儀表板中將 App 設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)。

2. 在您的 App 中撰寫程式碼以[擷取您 App 或您 App 所提供之附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)，然後[判斷與該 App 關聯的授權是否為試用版授權](get-license-info-for-apps-and-add-ons.md)。

3. 如果是試用版，請在您的 App 中排除或限制特定功能，然後再於使用者購買完整版授權時啟用這些功能。 如需詳細資訊，請參閱[實作 App 的試用版](implement-a-trial-version-of-your-app.md)。

4. 依照本文中的[測試指導方針](#testing)來測試您的實作。

<span id="testing" />
### 測試您的實作


            **Windows.Services.Store** 命名空間不提供您可在測試期間用來模擬授權資訊的類別。 您必須改為將 App 發行到「市集」，並將該 App 下載到您的開發裝置，才能使用它的授權進行測試。 這與使用 **Windows.ApplicationModel.Store** 命名空間的 App 是不同的體驗，這些 App 可以使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別，在測試期間模擬授權資訊。

如果您的 App 使用 **Windows.Services.Store** 命名空間中的 API 來存取 App 及其附加元件的資訊，請依照下列程序來測試您的程式碼：

1. 如果您尚未在「市集」中發行及提供您的 App，請確定您的 App 符合最低 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)需求、[將您的 App 提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)給「Windows 開發人員中心」儀表板，以及確定您的 App 通過認證程序，如此才能在「市集」中將它列出。 您可以選擇性地[從市集隱藏 App](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)，讓使用者在您測試時無法取得該 App。

2. 接著，確定您已完成下列操作：

  * 在您的 App 中撰寫使用 **Windows.Services.Store** 命名空間中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 及其他相關類型來實作 [App 內購買](#implement-iap)或[試用功能](#implement-trial)的程式碼。

  * 如果您的 App 提供客戶可購買的附加元件，請[在開發人員中心儀表板中為 App 建立附加元件提交項目](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。

  * 如果您想在 App 試用版中排除或限制某些功能，請[在 Windows 開發人員中心儀表板中將 App 設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)。

3. 在您的專案於 Visual Studio 中開啟的情況下，按一下 [專案] 功能表、指向 [市集]，然後按一下 [將應用程式與市集建立關聯]。 請完成精靈中的指示，將 App 專案與「Windows 開發人員中心」帳戶中您想要用來測試的 App 建立關聯。

  >
            **注意**
            &nbsp;&nbsp;如果您不會將專案與市集中的 App 建立關聯，[StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法就會將其傳回值的 **ExtendedError** 屬性設定為錯誤碼值 0x803F6107。 這個值表示市集沒有任何關於該 App 的知識。

4. 如果您尚未執行此動作，從市集中安裝您在上一個步驟指定的 App、執行一次 App，然後關閉此 App。 這可確保 App 的有效授權已安裝於您的開發裝置上。

5. 在 Visual Studio 中，開始執行或偵錯您的專案。 您的程式碼應該從與您本機專案相關聯的市集 App 中擷取 App 和附加元件資料。 如果系統提示您重新安裝 App，請依照下列指示進行，然後執行或偵錯您的專案。

<span id="receipts" />
### App 內購買的收據


            **Windows.Services.Store** 命名空間不提供您可用來在 App 程式碼中取得成功購買交易收據的 API。 這與使用 **Windows.ApplicationModel.Store** 命名空間的 App 是不同的體驗，這些 App 可以[使用用戶端 API 來擷取交易收據](use-receipts-to-verify-product-purchases.md)。

如果您使用 **Windows.Services.Store** 命名空間來實作 App 內購買，而您想要驗證指定的使用者是否已購買 App 或附加元件，則您可以使用 [Windows 市集集合 REST API](view-and-grant-products-from-a-service.md) 中的[查詢產品方法](query-for-products.md)。 此方法的傳回資料會確認指定的客戶是否具備所指定產品的權益，並提供使用者取得該產品的交易資料。 「Windows 市集」集合 API 會使用 Azure AD 驗證來存取此資訊。

<span id="desktop" />
### 在使用傳統型橋接器的應用程式中使用 StoreContext 類別

使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)的傳統型應用程式可以使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別來實作 App 內購買和試用版。 不過，如果您有 Win32 傳統型應用程式，或是有具備與轉譯架構 (例如 WPF 應用程式) 關聯之視窗控制代碼 (HWND) 的傳統型應用程式，您的應用程式就必須設定 **StoreContext** 物件，以指定哪一個應用程式視窗是物件所顯示的強制回應對話方塊的主控視窗。

許多 **StoreContext** 成員 (以及透過 **StoreContext** 物件存取的其他相關類型的成員) 都會針對市集相關作業 (例如購買產品) 對使用者顯示強制回應對話方塊。 如果傳統型應用程式未設定 **StoreContext** 物件來指定強制回應對話方塊的主控視窗，此物件將會傳回不正確的資料或錯誤。

若要在使用「傳統型橋接器」的傳統型應用程式中使用 **StoreContext** 物件，請依照下列步驟。

  1. 如果您的應用程式是以受管理的語言 (例如 C# 或 Visual Basic) 撰寫的，請在您的 App 程式碼中以 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 屬性宣告 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 介面，如以下範例所示。 此範例假設您的程式碼檔案有 **System.Runtime.InteropServices** 命名空間的 **using** 陳述式。

    ```csharp
    [ComImport]
    [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface IInitializeWithWindow
    {
        void Initialize(IntPtr hwnd);
    }
    ```

    如果您的應用程式是以原生程式碼 (例如 C++) 撰寫的，您就不需要匯入 **IInitializeWithWindow** 介面。 只要在您的程式碼中新增對 shobjidl.h 標頭檔案的參考即可。

  2. 使用 [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) (或 [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx)) 方法來取得 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件 (如本文稍早所述)，然後將此物件轉換 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 物件。 接著，呼叫 [Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 方法，並傳遞要作為 **StoreContext** 方法所顯示任何強制回應對話方塊之主控視窗的視窗控制代碼。 下列範例說明如何將您 App 主要視窗的控制代碼傳遞給方法。

    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />
### 產品、SKU 和可用性

「市集」中的每個產品都至少有一個 *SKU*，而每個 SKU 都至少會有一個「可用性」。 這些概念對於 Windows 開發人員中心儀表板中的大多數開發人員而言是非常抽象的，而且大多數開發人員永遠都不會為他們的 App 或附加元件定義 SKU 或可用性。 不過，由於 **Windows.Services.Store** 命名空間中適用於市集產品的物件模型包含了 SKU 和可用性，因此對這些概念有基本了解是很有幫助的。

| 物件類型 |  描述  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  此類別代表「市集」中提供的任何類型產品，包括 App 或附加元件。 此類別提供可讓您用來存取資料 (例如產品的「市集識別碼」、「市集」清單的影像和視訊，以及定價資訊) 的屬性。 它也提供可讓您用來購買產品的方法。 |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  此類別代表產品的 *SKU*。 SKU 是一個特定的產品版本，具有自己的描述、價格，以及其他獨特的產品詳細資料。 每個 App 或附加元件都有預設的 SKU。 大多數開發人員會對一個 App 提供多個 SKU 的唯一時刻是在其發行完整版的 App 和試用版時 (在市集型錄中，這其中每一個版本都是相同 App 的不同 SKU)。 <p/><p/> 某些發行者可以定義自己的 SKU。 例如，大型遊戲發行者可能在不允許紅色血液的市場中使用一個顯示綠色血液的 SKU 來發佈遊戲，並在所有其他市場中使用另一個顯示紅色血液的 SKU 來發佈。 或者，銷售數位視訊內容的發行者可能針對某個視訊發佈兩個 SKU，一個 SKU 適用於高解析度版本，而另一個 SKU 適用於標準畫質版本。 <p/><p/> 每個產品都有 [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) 屬性，可讓您用來存取 SKU。 |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  此類別代表 SKU 的「可用性」。 可用性是一個特定的 SKU 版本，具有自己獨特的定價資訊。 每個 SKU 都有預設的可用性。 某些發行者能夠定義自己的可用性，來介紹指定 SKU 的不同價格選項。 <p/><p/> 每個 SKU 都會有 [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) 屬性，可讓您用來存取可用性。 對於大部分開發人員而言，每個 SKU 都會有一個預設的可用性。  |

<span id="store_ids" />
### 市集識別碼

市集中的每個 App 與附加元件都會有一個相關聯的**市集識別碼**。 
            **Windows.Services.Store** 命名空間中的許多 API 需要有市集識別碼，才能在 App 或附加元件上執行操作。 產品、SKU 和可用性具有不同的市集識別碼格式。

| 物件類型 |  市集識別碼格式  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  「市集」中任何產品的「市集識別碼」都是 12 個字元的英數字串，例如 ```9NBLGGH4R315```。 此市集識別碼適用於 App 或附加元件的 Windows 開發人員中心儀表板頁面，而且是由 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) 屬性所傳回。 這個識別碼有時也稱為「產品市集識別碼」。 |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  就 SKU 而言，「市集識別碼」的格式為 ```<product Store ID>/xxxx```，其中 ```xxxx``` 是 4 個字元的英數字串，用來識別產品的 SKU。 例如，```9NBLGGH4R315/000N```。 這個識別碼是由 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 物件的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) 屬性所傳回，有時稱為「SKU 市集識別碼」。 |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  就可用性而言，「市集識別碼」的格式為 ```<product Store ID>/xxxx/yyyyyyyyyyyy```，其中 ```xxxx``` 是 4 個字元的英數字串，用來識別產品的 SKU，而 ```yyyyyyyyyyyy``` 是 12 個字元的英數字串，用來識別 SKU 的可用性。 例如，```9NBLGGH4R315/000N/4KW6QZD2VN6X```。 這個識別碼是由 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 物件的 [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) 屬性所傳回，有時稱為「可用性市集識別碼」。  |

## 相關主題

* [取得 App 和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)
* [使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Nov16_HO1-->


