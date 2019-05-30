---
Description: 無論您的 app 是否免費，都可以直接從 app 內銷售內容、其他 app 或新的 app 功能 (例如解除鎖定遊戲的下一個關卡)。 以下示範如何在 app 中啟用這些產品。
title: 啟用應用程式內產品購買
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: UWP, 附加元件, app 內購買, IAP, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 44cc0674e98c2fdf1bf8ecd2fbf6f859dfe25e62
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371851"
---
# <a name="enable-in-app-product-purchases"></a>啟用應用程式內產品購買

無論您的 app 是否免費，都可以直接從 app 內銷售內容、其他 app 或新的 app 功能 (例如解除鎖定遊戲的下一個關卡)。 以下示範如何在 app 中啟用這些產品。

> [!IMPORTANT]
> 這篇文章示範如何使用 [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) 命名空間的成員來啟用在應用程式內產品購買。 此命名空間不再提供新功能更新，建議您改為使用 [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) 命名空間。 **Windows.Services.Store**命名空間支援最新的附加元件類型，例如存放區管理需求的可取用的附加元件和訂用帳戶，而且設計成與未來的產品和協力廠商所支援功能的類型相容Center 和存放區。 **Windows.Services.Store** 命名空間在 Windows 10 (版本 1607) 中引進，只適用於目標為 Visual Studio 中 **Windows 10 Anniversary Edition (10.0；組建 14393)** 或更新版本的專案。 如需有關使用 **Windows.Services.Store** 命名空間來啟用 app 內產品購買的詳細資訊，請參閱[本文](enable-in-app-purchases-of-apps-and-add-ons.md)。

> [!NOTE]
> 試用版的 App 無法提供應用程式內產品。 使用試用版 App 的客戶只有在購買 App 的完整版本後，才能購買應用程式內產品。

## <a name="prerequisites"></a>必要條件

-   要新增功能讓客戶購買的 Windows 應用程式。
-   初次撰寫並測試新應用程式內產品的程式碼時，您必須使用 [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 物件，而不是 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 物件。 如此一來，您就可以利用對授權伺服器進行模擬呼叫來驗證授權邏輯，而不是呼叫使用中的伺服器。 若要這樣做，您需要自訂檔案 %userprofile%中名為 WindowsStoreProxy.xml\\AppData\\本機\\封裝\\&lt;封裝名稱&gt;\\LocalState\\Microsoft\\Windows 市集\\ApiData。 Microsoft Visual Studio 模擬器會在您第一次執行您的 App 時建立這個檔案，或者您也可以在執行階段載入自訂的檔案。 如需詳細資訊，請參閱[使用 WindowsStoreProxy.xml 檔案搭配 CurrentAppSimulator](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。
-   本主題也會參照[市集範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)中提供的程式碼範例。 這個範例非常適合用來體驗實機操作針對通用 Windows 平台 (UWP) app 提供的不同貨幣選項。

## <a name="step-1-initialize-the-license-info-for-your-app"></a>步驟 1：初始化您的應用程式的授權資訊

在您的 App 進行初始化時，請透過初始化 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 或 [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 來為應用程式取得 [LicenseInformation](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.LicenseInformation) 物件，以啟用購買應用程式內產品的功能。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#InitializeLicenseTest)]

## <a name="step-2-add-the-in-app-offers-to-your-app"></a>步驟 2：將應用程式內的供應項目新增至您的應用程式

針對您想要透過應用程式內產品提供的每項功能，建立一個購買選項，然後新增至您的 App。

> [!IMPORTANT]
> 在將您的 App 送出至市集之前，您必須先把要呈現給客戶的所有應用程式內產品新增至 App。 如果您想要稍後再新增應用程式內產品，您就必須更新 App，然後重新送出新版本。

1.  **建立應用程式內的供應項目權杖**

    您可以依權杖識別您應用程式中的每個應用程式內產品。 這個權杖是您定義的字串，並在應用程式和市集中用來識別特定的應用程式內產品。 請指定一個既唯一 (對您的應用程式來說) 又有意義的名稱，以便在撰寫程式碼時，可以快速地識別其正確功能。 以下是一些名稱的範例：

    * "SpaceMissionLevel4"
    * "ContosoCloudSave"
    * "RainbowThemePack"

  > [!NOTE]
  > 必須符合您在您的程式碼中使用的應用程式內的供應項目語彙基元[產品識別碼](../publish/set-your-add-on-product-id.md#product-id)值，指定何時您[定義對應的附加元件，您的應用程式在合作夥伴中心](../publish/add-on-submissions.md)。

2.  **程式碼的條件式區塊中的功能**

    針對與應用程式內產品關聯的每個功能，您必須將其程式碼放到條件性區塊中來測試，以查看客戶是否擁有可使用該功能的授權。

    下列範例示範如何在授權專屬的條件性區塊中，撰寫 **featureName** 產品功能的程式碼。 **featureName** 字串是可在 App 內唯一識別這個產品的權杖，同時也可用來在市集中識別此產品。

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#CodeFeature)]

3.  **加入這項功能在購買 UI**

    您的應用程式也必須提供購買機制，讓客戶能夠購買應用程式內產品所提供的產品或功能。 客戶無法透過市集以購買完整應用程式的方式來購買這些產品或功能。

    以下是如何測試以查看客戶是否已經擁有應用程式內產品，如果沒有，就顯示購買對話方塊來讓客戶購買。 請以您為購買對話方塊自訂的程式碼取代 "show the purchase dialog" 註解 (例如提供一個含有親切提醒「購買此應用程式！」 按鈕的頁面)。

    > [!div class="tabbedCodeSnippets"]
    [!code-csharp[EnableInAppPurchases](./code/InAppPurchasesAndLicenses/cs/EnableInAppPurchases.cs#BuyFeature)]

## <a name="step-3-change-the-test-code-to-the-final-calls"></a>步驟 3：將測試程式碼變更為最終的呼叫

這個步驟很簡單：在您 App 的程式碼中，將 [CurrentAppSimulator](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 的每個參考變更成 [CurrentApp](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp)。 您不再需要提供 WindowsStoreProxy.xml 檔案，因此可以將該檔案從您 App 的路徑中移除 (不過您可以加以儲存，以供在下個步驟中設定應用程式內的購買選項時參考)。

## <a name="step-4-configure-the-in-app-product-offer-in-the-store"></a>步驟 4：設定存放區中的應用程式內產品的供應項目

在合作夥伴中心瀏覽至您的應用程式和[建立附加元件](../publish/add-on-submissions.md)符合您的應用程式內產品供應項目。 定義產品識別碼、類型、價格與附加元件的其他屬性。 請確定在測試時，將它設定為與 WindowsStoreProxy.xml 相同的設定。

  > [!NOTE]
  > 必須符合您在您的程式碼中使用的應用程式內的供應項目語彙基元[產品識別碼](../publish/set-your-add-on-product-id.md#product-id)指定對應的附加元件，在合作夥伴中心內的值。

## <a name="remarks"></a>備註

如果您想要為客戶提供消費性應用程式內產品選項 (可購買、耗盡，並在想要時再次購買的項目)，請前往[啟用消費性應用程式內產品購買](enable-consumable-in-app-product-purchases.md)主題。

如果您需要使用收據來確認使用者已進行 App 內購買，請務必檢閱[使用收據來驗證產品購買](use-receipts-to-verify-product-purchases.md)。

## <a name="related-topics"></a>相關主題


* [啟用消費性 App 內產品購買](enable-consumable-in-app-product-purchases.md)
* [管理大型的應用程式內產品目錄](manage-a-large-catalog-of-in-app-products.md)
* [若要確認產品採購使用回條](use-receipts-to-verify-product-purchases.md)
* [（示範試用版，以及在應用程式內購買） 的存放區範例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
