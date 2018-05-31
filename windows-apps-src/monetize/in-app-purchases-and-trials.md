---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: 了解如何在 UWP 應用程式中啟用 App 內購買和試用版。
title: " App 內購買和試用版"
ms.author: mcleans
ms.date: 08/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, App 內購買, IAP, 附加元件, 試用版, 消費性, 耐久性, 訂閱
ms.localizationpriority: medium
ms.openlocfilehash: 28e28c5db19d9a2b397a04cc326867605d796aac
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691237"
---
# <a name="in-app-purchases-and-trials"></a> App 內購買和試用版

Windows SDK 提供您可用來實作下列功能以從您的「通用 Windows 平台」(UWP) 應用程式提升獲利的 API。

* ** App 內購買**&nbsp;&nbsp;無論您的應用程式是否免費，您都可以直接從應用程式內銷售內容或新的應用程式功能 (例如將遊戲的下一個關卡解除鎖定)。

* **試用功能**&nbsp;&nbsp;如果您將應用程式[在 Windows 開發人員中心儀表板中設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)，您將可藉由在試用期間排除或限制某些功能，來吸引客戶購買完整版的應用程式。 您也可以啟用橫幅或浮水印之類的功能，這些功能僅在客戶購買您的應用程式之前的試用期間顯示。

本文提供 UWP 應用程式中 App 內購買和試用版的運作方式概觀。

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>選擇要使用的命名空間

您可以根據您應用程式的目標是哪一個 Windows10 版本，使用兩種不同的命名空間將 App 內購買和試用版功能新增到 UWP 應用程式。 雖然這些命名空間中的 API 都是為相同的目標服務，但其設計方式截然不同，且兩個 API 之間的程式碼並不相容。

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;從 Windows 10 版本 1607 開始，應用程式可以使用此命名空間中的 API 來實作 App 內購買和試用版。 如果您應用程式專案的目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本，建議您使用此命名空間中的成員。 這個命名空間支援最新的附加元件類型 (例如 Microsoft Store 管理的消費性附加元件)，且設計成與「Windows 開發人員中心」和「Microsoft Store」所支援的未來產品與功能類型相容。 如需有關此命名空間的詳細資訊，請參閱本文中的[使用 Windows.Services.Store 命名空間的 App 內購買和試用版](#api_intro)一節。

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;所有 Windows 10 版本也都支援一個較舊的 API 來用於此命名空間中的 App 內購買和試用版。 如需 **Windows.ApplicationModel.Store** 命名空間的相關資訊，請參閱[使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

> [!IMPORTANT]
> **Windows.ApplicationModel.Store** 命名空間不再提供新功能更新，建議您改為使用 **Windows.Services.Store** 命名空間，如果您的應用程式可這麼做的話。 **Windows.ApplicationModel.Store** 命名空間不受支援於使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)之 Windows 傳統型應用程式，或應用程式，或使用開發人員中心之開發沙箱遊戲的應用程式或遊戲 (例如與 Xbox Live 整合的任何遊戲)。

<span id="concepts" />

## <a name="basic-concepts"></a>基本概念

「Microsoft Store」中提供的每個項目通常稱為 *「產品」*。 大部分的開發人員只會使用下列類型的產品︰*應用程式*和*附加元件*。

附加元件是您在應用程式內容中提供給客戶使用的產品或功能︰例如，在應用程式或遊戲中使用的貨幣、適用於遊戲的新地圖或武器，能夠在沒有廣告的情況下使用您的應用程式，或者適用於能夠提供該類型內容之應用程式的數位內容 (例如，音樂或視訊)。 每個應用程式與附加元件都有相關的授權，可指出使用者是否有資格使用該應用程式或附加元件。 如果使用者有資格使用應用程式或附加元件，授權也會提供關於試用版的額外資訊。

若要在您的應用程式中為客戶提供附加元件，您必須[在開發人員中心儀表板中定義應用程式的附加元件](../publish/add-on-submissions.md)，讓「Microsoft Store」知道它。 接著，您的應用程式便可以使用 **Windows.Services.Store** 或 **Windows.ApplicationModel.Store** 命名空間中的 API，以 App 內購買形式提供要對使用者銷售的附加元件。

UWP 應用程式可以提供下列類型的附加元件。

| 附加元件類型 |  描述  |
|---------|-------------------|
| 耐久性  |  針對您在 [Windows 開發人員中心儀表板](../publish/enter-iap-properties.md)指定的存留期持續存在的附加元件。 <p/><p/>根據預設，耐久性附加元件永遠不會過期，在此情況下只需購買一次。 如果您為附加元件指定特定的持續時間，則使用者就能在該附加元件到期之後重新進行購買。 |
| 開發人員管理的消費性產品  |  可供購買、使用，然後再次購買的附加元件。 這種類型的附加元件通常用於應用程式內貨幣。 <p/><p/>針對這種類型的消費性產品，您需負責記錄使用者在該附加元件所代表項目的餘額，以及負責在使用者取用所有項目之後，向「Microsoft Store」回報已完全交付此附加元件的購買。 使用者必須等到您的應用程式將先前的附加元件購買回報為已完全交付之後，才能再次購買該附加元件。 <p/><p/>例如，如果您的附加元件在遊戲中代表 100 個金幣，而使用者花費了 10 個金幣，則您的應用程式或服務必須針對該使用者保留 90 個金幣的新餘額。 當使用者花光 100 個金幣之後，您的 App 必須回報該附加元件已完成，接著使用者就能再次購買 100 個金幣的附加元件。    |
| Microsoft Store 管理的消費性產品  |  可供購買、使用，然後再次購買的附加元件。 這種類型的附加元件通常用於應用程式內貨幣。<p/><p/>針對這種類型的消費性產品，「Microsoft Store」會記錄使用者在該附加元件所代表項目的餘額。 當使用者取用任何項目時，您必須負責向 Microsoft Store 回報這些項目已完成，而 Microsoft Store 會更新使用者的餘額。 您的應用程式可以隨時查詢使用者目前的餘額。 當使用者用完所有項目之後，該使用者就能再次購買該附加元件。  <p/><p/> 例如，如果您的附加元件在遊戲中代表最初的 100 個金幣數量，而使用者花費了 10 個金幣，則您的應用程式會向 Microsoft Store 回報已完成附加元件的 10 個單位，而 Microsoft Store 會更新剩下的餘額。 當使用者花光 100 個金幣之後，該使用者就能再次購買 100 個金幣的附加元件。 <p/><p/>**注意**&nbsp;&nbsp;若要使用 Microsoft Store 管理的消費性產品，您的應用程式必須以 Visual Studio 中的 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本為目標，而且必須使用 **Windows.Services.Store** 命名空間，而不是 **Windows.ApplicationModel.Store** 命名空間。  |
| 訂閱 | 耐久性附加元件會繼續向客戶收取週期性費用，才能繼續使用附加元件。 客戶隨時都可以取消訂閱，避免進一步的費用。 <p/><p/>**注意**&nbsp;&nbsp;訂閱附加元件目前僅提供給參與早期採用者計畫的開發人員。 若要使用訂閱附加元件，您的應用程式必須以 Visual Studio 中的 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本為目標，而且必須使用 **Windows.Services.Store** 命名空間，而不是 **Windows.ApplicationModel.Store** 命名空間。  |

<span />

> [!NOTE]
> 其他類型的附加元件 (例如，具有套件的耐久性附加元件，也稱為可下載內容或 DLC) 僅可供一組有限的開發人員使用，而本文件中並未涵蓋這個部分。

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>使用 Windows.Services.Store 命名空間的 App 內購買和試用版

本章節概略說明 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間的重要任務和概念。 此命名空間僅提供給目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的應用程式 (這對應到 Windows 10 (版本 1607))。 建議應用程式盡可能使用 **Windows.Services.Store** 命名空間，而不是 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空間。 如需 **Windows.ApplicationModel.Store** 命名空間的相關資訊，請參閱[本文](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)。

**本節內容**

* [影片](#video)
* [開始使用 StoreContext 類別](#get-started-storecontext)
* [實作 App 內購買](#implement-iap)
* [實作試用版功能](#implement-trial)
* [測試您的 App 內購買或試用實作。](#testing)
* [ App 內購買的收據](#receipts)
* [使用 StoreContext 類別和傳統型橋接器](#desktop)
* [產品、SKU 和可用性](#products-skus)
* [ Store 識別碼](#store-ids)

<span id="video" />

### <a name="video"></a>影片

觀看下列影片，了解如何使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空間，在您的應用程式中實作 App 內購買的概觀。
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>開始使用 StoreContext 類別

**Windows.Services.Store** 命名空間的主要進入點是 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別。 這個類別會提供一些方法，可供您用來取得目前應用程式及其可用附加元件的資訊、取得目前應用程式或其附加元件的授權資訊、為目前的使用者購買應用程式或附加元件，以及執行其他工作。 若要取得 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件，請執行下列其中一項操作︰

* 在單一使用者應用程式 (也就是只在啟動應用程式的使用者內容中執行的應用程式)，使用靜態 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) 方法來取得 **StoreContext** 物件，您可以用來為使用者存取 Microsoft Store 相關的資料。 大多數通用 Windows 平台 (UWP) 應用程式都是單一使用者應用程式。

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* 在[多使用者應用程式](../xbox-apps/multi-user-applications.md) 中，請使用靜態 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) 方法來取得 **StoreContext** 物件，您可以針對在使用應用程式時使用其 Microsoft 帳戶登入的特定使用者，使用此物件來存取其 Microsoft Store 相關資料。 下列範例會針對第一位可用的使用者取得 **StoreContext** 物件。

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> 使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)的 Windows 傳統型應用程式必須先執行額外的步驟來設定 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件，才能使用此物件。 如需詳細資訊，請參閱[本節](#desktop)。

在您具備 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件之後，您可以開始呼叫此物件的方法來取得目前應用程式及其附加元件的「Microsoft Store」產品資訊、擷取目前應用程式及其附加元件的授權資訊、為目前的使用者購買應用程式或附加元件，以及執行其他工作。 如需有關您可以使用此物件來執行常見工作的詳細資訊，請參閱下列文章：

* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [啟用應用程式的訂閱附加元件](enable-subscription-add-ons-for-your-app.md)
* [實作 App 的試用版](implement-a-trial-version-of-your-app.md)

如需示範如何在 **Windows.Services.Store** 命名空間中使用 **StoreContext** 和其他類型的範例應用程式，請參閱[ Microsoft Store 範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)。

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>實作 App 內購買

使用 **Windows.Services.Store** 命名空間在您的應用程式中為客戶提供 App 內購買：

1. 如果您的應用程式提供客戶可購買的附加元件，請[在開發人員中心儀表板中為應用程式建立附加元件提交項目](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。

2. 請在您的應用程式中撰寫程式碼以[擷取您應用程式或您應用程式所提供之附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)，然後[判斷授權是否有效](get-license-info-for-apps-and-add-ons.md) (亦即，使用者是否具有可使用應用程式或附加元件的授權)。 如果授權無效，請顯示一個 UI 來以 App 內購買形式為使用者提供要銷售的應用程式或附加元件。

3. 如果使用者選擇購買您的應用程式或附加元件，請使用適當的方法來購買該產品：

    * 如果使用者要購買您的應用程式或耐久性附加元件，請依照[啟用應用程式和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)中的指示操作。
    * 如果使用者要購買消費性附加元件，請依照[啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)中的指示操作。
    * 如果使用者要購買訂閱附加元件，請依照[啟用應用程式的訂閱附加元件](enable-subscription-add-ons-for-your-app.md)中的指示操作。

4. 依照本文中的[測試指導方針](#testing)來測試您的實作。

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>實作試用版功能

使用 **Windows.Services.Store** 命名空間在您的應用程式試用版中排除或限制功能：

1. [在 Windows 開發人員中心儀表板中將應用程式設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)。

2. 在您的應用程式中撰寫程式碼以[擷取您應用程式或您應用程式所提供之附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)，然後[判斷與該應用程式關聯的授權是否為試用版授權](get-license-info-for-apps-and-add-ons.md)。

3. 如果是試用版，請在您的應用程式中排除或限制特定功能，然後再於使用者購買完整版授權時啟用這些功能。 如需詳細資訊和程式碼範例，請參閱[實作應用程式的試用版](implement-a-trial-version-of-your-app.md)。

4. 依照本文中的[測試指導方針](#testing)來測試您的實作。

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>測試您的 App 內購買或試用實作。

如果您的應用程式使用 **Windows.Services.Store** 命名空間中的 API 來實作 App 內購買或試用版功能，您必須將應用程式發行至 Microsoft Store，並將應用程式下載至您的開發裝置以便使用它的授權來進行測試。 請依照此程序，測試您的程式碼：

1. 如果您尚未在 Microsoft Store 中發行及提供您的應用程式，請確定您的應用程式符合最低的 [Windows 應用程式認證套件](https://developer.microsoft.com/windows/develop/app-certification-kit)需求、[將您的應用程式提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)給「Windows 開發人員中心」儀表板，以及確定您的應用程式通過認證程序。 測試時您可以[將應用程式設定為不可在 Microsoft Store 中搜尋](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability)。

2. 接著，確定您已完成下列操作：

    * 在您的應用程式中撰寫使用 **Windows.Services.Store** 命名空間中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 及其他相關類型來實作 [ App 內購買](#implement-iap)或[試用功能](#implement-trial)的程式碼。
    * 如果您的應用程式提供客戶可購買的附加元件，請[在開發人員中心儀表板中為應用程式建立附加元件提交項目](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions)。
    * 如果您想在應用程式試用版中排除或限制某些功能，請[在 Windows 開發人員中心儀表板中將應用程式設定為免費試用](../publish/set-app-pricing-and-availability.md#free-trial)。

3. 在您的專案於 Visual Studio 中開啟的情況下，按一下 [專案]**** 功能表、指向 [ Microsoft Store ]****，然後按一下 [將應用程式與 Microsoft Store 建立關聯]****。 請完成精靈中的指示，將應用程式專案與「Windows 開發人員中心」帳戶中您想要用來測試的應用程式建立關聯。
    > [!NOTE]
    > 如果您不會將專案與 Microsoft Store 中的應用程式建立關聯，[StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 方法就會將其傳回值的 **ExtendedError** 屬性設定為錯誤碼值 0x803F6107。 這個值表示 Microsoft Store 沒有任何關於該應用程式的知識。
4. 如果您尚未執行此動作，從 Microsoft Store 中安裝您在上一個步驟指定的應用程式、執行一次應用程式，然後關閉此應用程式。 這可確保應用程式的有效授權已安裝於您的開發裝置上。

5. 在 Visual Studio 中，開始執行或偵錯您的專案。 您的程式碼應該從與您本機專案相關聯的 Microsoft Store 應用程式中擷取應用程式和附加元件資料。 如果系統提示您重新安裝應用程式，請依照下列指示進行，然後執行或偵錯您的專案。
    > [!NOTE]
    > 在您完成這些步驟之後，您可以繼續更新您的應用程式的程式碼，然後在您的開發電腦上偵錯更新的專案，而不用提交新的應用程式套件到 Microsoft Store。 您只需要下載您的應用程式的 Microsoft Store 版到您的開發電腦，即可取得將用於測試的本機授權。 您只需要在完成測試後提交新的應用程式套件到 Microsoft Store，並讓您的客戶能夠使用您的應用程式中的 App 內購買或試用相關功能。

如果您的應用程式使用 **Windows.ApplicationModel.Store** 命名空間，您可在應用程式中使用 [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) 類別，以便在提交 App 至 Microsoft Store 之前的測試期間模擬授權資訊。 如需詳細資訊，請參閱 [開始使用 CurrentApp 和 CurrentAppSimulator 類別] (in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes)。  

> [!NOTE]
> **Windows.Services.Store** 命名空間不提供您可在測試期間用來模擬授權資訊的類別。 如果您使用 **Windows.Services.Store** 命名空間來實作在應用程式中購買項目或試用版，您必須將應用程式發行至 Microsoft Store，並將應用程式下載至您的開發裝置以便使用它的授權來進行測試，如上所述。

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a> App 內購買的收據

**Windows.Services.Store** 命名空間不提供您可用來在應用程式程式碼中取得成功購買交易收據的 API。 這與使用 **Windows.ApplicationModel.Store** 命名空間的應用程式是不同的體驗，這些應用程式可以[使用用戶端 API 來擷取交易收據](use-receipts-to-verify-product-purchases.md)。

如果您使用 **Windows.Services.Store** 命名空間來實作 App 內購買，而您想要驗證指定的使用者是否已購買應用程式或附加元件，則您可以使用 [Microsoft Store 集合 REST API](view-and-grant-products-from-a-service.md) 中的[查詢產品方法](query-for-products.md)。 此方法的傳回資料會確認指定的客戶是否具備所指定產品的權益，並提供使用者取得該產品的交易資料。 Microsoft Store 集合 API 會使用 Azure AD 驗證來擷取此資訊。

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>使用 StoreContext 類別和傳統型橋接器

使用[傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)的傳統型應用程式可以使用 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 類別來實作 App 內購買和試用版。 不過，如果您有 Win32 傳統型應用程式，或是有具備與轉譯架構 (例如 WPF 應用程式) 關聯之視窗控制代碼 (HWND) 的傳統型應用程式，您的應用程式就必須設定 **StoreContext** 物件，以指定哪一個應用程式視窗是物件所顯示的強制回應對話方塊的主控視窗。

許多 **StoreContext** 成員 (以及透過 **StoreContext** 物件存取的其他相關類型的成員) 都會針對 Microsoft Store 相關作業 (例如購買產品) 對使用者顯示強制回應對話方塊。 如果傳統型應用程式未設定 **StoreContext** 物件來指定強制回應對話方塊的主控視窗，此物件將會傳回不正確的資料或錯誤。

若要在使用「傳統型橋接器」的傳統型應用程式中使用 **StoreContext** 物件，請依照下列步驟。

1. 執行下列其中一項，來讓您的應用程式存取 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 介面：

    * 如果您的應用程式是以受管理的語言 (例如 C# 或 Visual Basic) 撰寫的，請在您的應用程式程式碼中以 [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) 屬性宣告 **IInitializeWithWindow** 介面，如以下 C# 範例所示。 此範例假設您的程式碼檔案有 **System.Runtime.InteropServices** 命名空間的 **using** 陳述式。

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * 如果您的應用程式是以 C++ 撰寫，請在程式碼中新增對 shobjidl.h 標頭檔案的參考。 此標頭檔案包含 **IInitializeWithWindow** 介面的宣告。

2. 使用 [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) 方法 (或 [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser)，若您的應用程式是[多使用者 App](../xbox-apps/multi-user-applications.md)) 來取得 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 物件 (如本文稍早所述)，然後將此物件轉換為 [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) 物件。 接著，呼叫 [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) 方法，並傳遞要作為 **StoreContext** 方法所顯示任何強制回應對話方塊之擁有者的視窗控制代碼。 下列 C# 範例說明如何將您應用程式主要視窗的控制代碼傳遞給方法。
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>產品、SKU 和可用性

「Microsoft Store」中的每個產品都至少有一個 *SKU*，而每個 SKU 都至少會有一個「可用性」**。 這些概念對於 Windows 開發人員中心儀表板中的大多數開發人員而言是非常抽象的，而且大多數開發人員永遠都不會為他們的應用程式或附加元件定義 SKU 或可用性。 不過，由於 **Windows.Services.Store** 命名空間中適用於 Microsoft Store 產品的物件模型包含了 SKU 和可用性，因此對這些概念有基本了解在某些情況下是很有幫助的。

| 物件 |  描述  |
|---------|-------------------|
| 產品  |  *產品*是指 Microsoft Store 中提供的任何類型產品，包括應用程式或附加元件。 <p/><p/> Microsoft Store 中每個產品都有對應的 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件。 此類別提供可讓您用來存取資料 (例如產品的「Store 識別碼」、Store 清單的影像和視訊，以及定價資訊) 的屬性。 它也提供可讓您用來購買產品的方法。 |
| SKU |  *SKU* 是一個特定的產品版本，具有自己的描述、價格，以及其他獨特的產品詳細資料。 每個應用程式或附加元件都有預設的 SKU。 大多數開發人員會對一個應用程式提供多個 SKU 的唯一時刻是在其發行完整版的應用程式和試用版時 (在 Microsoft Store 型錄中，這其中每一個版本都是相同應用程式的不同 SKU)。 <p/><p/> 某些發行者可以定義自己的 SKU。 例如，大型遊戲發行者可能在不允許紅色血液的市場中使用一個顯示綠色血液的 SKU 來發佈遊戲，並在所有其他市場中使用另一個顯示紅色血液的 SKU 來發佈。 或者，銷售數位視訊內容的發行者可能針對某個視訊發佈兩個 SKU，一個 SKU 適用於高解析度版本，而另一個 SKU 適用於標準畫質版本。 <p/><p/> Microsoft Store 中每個 SKU 都有對應的 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 物件。 每個 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 都有 [Skus](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) 屬性，可讓您用來存取產品的 SKU。 |
| 可用性  |  *可用性*是一個特定的 SKU 版本，具有自己獨特的定價資訊。 每個 SKU 都有預設的可用性。 某些發行者能夠定義自己的可用性，來介紹指定 SKU 的不同價格選項。 <p/><p/> Microsoft Store 中每個可用性都有對應的 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 物件。 每個 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 都有 [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) 屬性，可讓您用來存取 SKU 的可用性。 對於大部分開發人員而言，每個 SKU 都會有一個預設的可用性。  |

<span id="store_ids" />

### <a name="store-ids"></a> Store 識別碼

Microsoft Store 中每個應用程式、附加元件或其他產品都有相關的** Store 識別碼** (這有時候也稱為*產品 Store 識別碼*)。 許多 API 需要有 Store 識別碼，才能在應用程式或附加元件上執行操作。

「Microsoft Store」中任何產品的「Store 識別碼」都是 12 個字元的英數字串，例如 ```9NBLGGH4R315```。 有幾種不同方式可在 Microsoft Store 中取得產品的 Store 識別碼：

* 若是應用程式，您可在開發人員中心儀表板的 [App 身分識別](../publish/view-app-identity-details.md) 頁面取得 Store 識別碼。
* 若是附加元件，您可以在儀表板中的 [附加元件概觀] 頁面上找到 Store 識別碼。
* 對於任何產品，您也可以使用代表產品之 [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) 物件的 [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) 屬性，以程式設計方式取得 Store 識別碼。

對於具有 SKU 和可用性的產品，SKU 和可用性也有自己的 Store 識別碼，但是格式不同。

| 物件 |   Store 識別碼格式  |
|---------|-------------------|
| SKU |  SKU 的「Store 識別碼」的格式為 ```<product Store ID>/xxxx```，其中 ```xxxx``` 是 4 個字元的英數字串，用來識別產品的 SKU。 例如，```9NBLGGH4R315/000N```。 這個識別碼是由 [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) 物件的 [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) 屬性所傳回，有時稱為 *「SKU  Store 識別碼」*。 |
| 可用性  |  可用性的「Store 識別碼」的格式為 ```<product Store ID>/xxxx/yyyyyyyyyyyy```，其中 ```xxxx``` 是 4 個字元的英數字串，用來識別產品的 SKU，而 ```yyyyyyyyyyyy``` 是 12 個字元的英數字串，用來識別 SKU 的可用性。 例如，```9NBLGGH4R315/000N/4KW6QZD2VN6X```。 這個識別碼是由 [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) 物件的 [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid) 屬性所傳回，有時稱為 *「可用性 Store 識別碼」*。  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>如何在程式碼中使用附加元件的產品識別碼

如果您想讓客戶在您的應用程式中使用附加元件，當您在開發人員中心儀表板中[建立您的附加元件提交](../publish/add-on-submissions.md)時，必須[輸入附加元件的唯一產品識別碼](../publish/set-your-add-on-product-id.md#product-id)。 您可以在程式碼中使用此產品識別碼來參考附加元件，不過您可使用產品識別碼的具體情況取決於您在應用程式中用於 App 內購買的命名空間。

> [!NOTE]
> 您在開發人員中心儀表板中為附加元件輸入的產品識別碼，不同於附加元件的[ Store 識別碼](#store-ids)。  Store 識別碼由開發人員中心產生。

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>使用 Windows.Services.Store 命名空間的應用程式

如果您的應用程式使用 **Windows.Services.Store** 命名空間，您可以使用產品識別碼輕鬆地找出代表附加元件的 [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct)，或代表附加元件授權的 [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense)。 產品識別碼由 [StoreProduct.InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) 和 [StoreLicense.InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken) 屬性公開。

> [!NOTE]
> 雖然產品識別碼是在程式碼中識別附加元件的有用方法，但 **Windows.Services.Store** 命名空間中的大部分操作使用附加元件的[ Store 識別碼](#store-ids)，而不是產品識別碼。 例如，以程式設計方式取得應用程式的一或多個已知附加元件，傳遞附加元件的 Store 識別碼 (而非產品識別碼) 到 [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync) 方法。 同樣地，在消費性附加元件完成時報告、傳遞附加元件的 Store 識別碼 (而非產品識別碼) 到 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync) 方法。

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>使用 Windows.ApplicationModel.Store 命名空間的應用程式

如果您的應用程式使用 **Windows.ApplicationModel.Store** 命名空間，針對大部分操作，您將需要使用您在開發人員中心儀表板中指派給附加元件的產品識別碼。 例如：

* 使用產品識別碼找出代表附加元件的 [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting)或代表附加元件授權的 [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense)。 產品識別碼由 [ProductListing.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) 和 [ProductLicense.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId) 屬性公開。

* 當您使用 [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 方法為使用者購買附加元件時，請指定產品識別碼。 如需詳細資訊，請參閱[啟用在應用程式內產品購買](enable-in-app-product-purchases.md)。

* 當您使用 [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync) 方法報告消費性附加元件完成時，請指定產品識別碼。 如需詳細資訊，請參閱[啟用消費性應用程式內產品購買](enable-consumable-in-app-product-purchases.md)。

## <a name="related-topics"></a>相關主題

* [取得應用程式和附加元件的產品資訊](get-product-info-for-apps-and-add-ons.md)
* [取得 App 和附加元件的授權資訊](get-license-info-for-apps-and-add-ons.md)
* [啟用 App 和附加元件的 App 內購買](enable-in-app-purchases-of-apps-and-add-ons.md)
* [啟用消費性附加元件購買](enable-consumable-add-on-purchases.md)
* [啟用應用程式的訂閱附加元件](enable-subscription-add-ons-for-your-app.md)
* [實作您應用程式的試用版](implement-a-trial-version-of-your-app.md)
* [Microsoft Store 作業的錯誤碼](error-codes-for-store-operations.md)
* [使用 Windows.ApplicationModel.Store 命名空間的 App 內購買和試用版](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
