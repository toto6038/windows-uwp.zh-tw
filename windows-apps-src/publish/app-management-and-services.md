---
Description: Manage and view details related to each of your apps in Partner Center, and configure services such as A/B testing and maps.
title: App 管理與服務
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30610cdacbd9d2be10205958688376371f0387f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260032"
---
# <a name="app-management-and-services"></a>App 管理與服務

You can manage and view details related to each of your apps in [Partner Center](https://partner.microsoft.com/dashboard), and configure services such as notifications, A/B testing, and maps.

When working with an app in Partner Center, you'll see sections in the left navigation menu for **Services** and **App management**. 您可以展開這些區段來存取如下所述的功能。

## <a name="services"></a>服務

**\[服務\]** 區段可讓您管理您 app 的數個不同服務。

## <a name="xbox-live"></a>Xbox Live

If you are publishing a game, you can enable the [Xbox Live Creators Program](https://www.xbox.com/developers/creators-program) on this page. This lets you start configuring and testing Xbox Live features, and eventually publish your Xbox Live Creators Program game.

For more info, see [Get started with the Xbox Live Creators Program](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) and [Create a new Xbox Live Creators Program title and publish to the test environment](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>實驗

利用**實驗**頁面，使用 A/B 測試為您的通用 Windows 平台 (UWP) app 建立及執行實驗。 在 A/B 測試中，您可以先在一些客戶身上測量您 app 中功能變更 (或變化) 的有效性，然後再為每個人啟用變更。

如需詳細資訊，請參閱[使用 A/B 測試執行 app 實驗](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>Maps

若要在目標為 Windows 10 或 Windows 8.x 的應用程式中使用地圖服務，請瀏覽 [Bing 地圖開發人員中心](https://www.bingmapsportal.com/)。 For info about how to request a maps authentication key from the Bing Maps Developer Center and add it to your app, see [Request a maps authentication key](../maps-and-location/authentication-key.md) for more info. 

Use the **Maps** page only for previously-published apps for Windows Phone 8.1 and earlier. To use map services in these apps, you'll need to request a map service application ID and a token to include in your app's code. When you click **Get token**, we'll generate a Map service Application ID (**ApplicationID**) and Map service Authentication Token (**AuthenticationToken**) for your app. Be sure to add these values to your code before you package and submit your app. 如需詳細資訊，請參閱[如何新增地圖控制項到頁面 (Windows Phone 8.1)](https://docs.microsoft.com/previous-versions/windows/apps/jj207033(v=vs.105)?redirectedfrom=MSDN)。

## <a name="product-collections-and-purchases"></a>產品集合與購買

To use the Microsoft Store collection API and the Microsoft Store purchase API to access ownership information for apps and add-ons, you need to enter the associated Azure AD client IDs here. 請注意，這些變更可能要花費最多 16 個小時才會生效。

如需詳細資訊，請參閱[管理服務的產品權利](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>Administrator consent

If your product integrates with Azure AD and calls APIs that request either [application permissions or delegated permissions](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) that require administrator consent, enter your Azure AD Client ID here. This lets administrators who acquire the app for their organization grant consent for your product to act on behalf of all users in the tenant.

For more info, see [Requesting consent for an entire tenant](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>應用程式管理

**\[應用程式管理\]** 區段讓您能夠檢視身分識別和套件詳細資料，並管理您的應用程式名稱。

## <a name="app-identity"></a>應用程式身分識別

此頁面顯示市集內與您應用程式唯一身分識別相關的詳細資料，包含連結至您應用程式清單的 URL。

如需詳細資訊，請參閱[檢視 app 身分識別詳細資料](view-app-identity-details.md)。

## <a name="manage-app-names"></a>管理應用程式名稱

您可以在此處檢視為應用程式保留的所有名稱。 您可以在此處保留其他名稱，或是刪除不再使用的名稱。

如需詳細資訊，請參閱[管理應用程式名稱](manage-app-names.md)。

## <a name="current-packages"></a>目前的套件

此頁面讓您能夠檢視與您所有已發行套件相關的詳細資料。

> [!NOTE]
> 在您發行應用程式之前，將無法在此處看見任何資訊。

系統會顯示每個套件的名稱、版本及架構。 按一下 **\[詳細資料\]** ，以顯示支援的語言、應用程式功能及檔案大小等其他資訊。 您看到的每個套件資訊，可能會因其目標作業系統以及其他因素有所不同。 

具備 OEM 權限的開發人員也可以從 **\[目前的套件\]** 頁面[產生預先安裝的套件](generate-preinstall-packages-for-oems.md)。

## <a name="wnsmpns"></a>WNS/MPNS

The **WNS/MPNS** section provides options to help you create and send notifications to your app's customers. 

> [!TIP]
> For UWP apps, we suggest using the **Notifications** feature in Partner Center. This feature lets you send notifications to all of your app's customers, or to a targeted subset of your Windows 10 customers who meet the criteria you’ve defined in a [customer segment](create-customer-segments.md). 如需詳細資訊，請參閱[傳送通知給您的應用程式客戶](send-push-notifications-to-your-apps-customers.md)。

Depending on your app's package type and its specific requirements, you can also use one of the following options: 

-   **Windows 推播通知服務 (WNS)** 讓您能夠從自己的雲端服務傳送快顯通知、磚、徽章及原始更新。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   **Microsoft Azure Mobile Apps** 讓您能夠傳送推播通知、驗證和管理應用程式使用者，以及在雲端中儲存應用程式資料。 如需詳細資訊，請參閱 [Mobile Apps 文件](https://docs.microsoft.com/azure/app-service-mobile/)。

-   **Microsoft Push Notifications Service (MPNS)** can be used with previously published .xap packages for Windows Phone. 儘管我們建議使用已授權的通知以避免發生節流限制，但您不需在此處進行任何設定，就能傳送有限數量的未授權通知。 If you're using MPNS, you'll need to upload a certificate to the field provided on the **WNS/MPNS** page. 如需詳細資訊，請參閱[設定已驗證的 Web 服務來傳送適用於 Windows Phone 8 的推播通知](https://docs.microsoft.com/previous-versions/windows/apps/ff941099(v=vs.105)?redirectedfrom=MSDN)。
 

 
