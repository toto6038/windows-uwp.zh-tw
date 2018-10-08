---
author: jnHs
Description: Manage and view details related to each of your apps in the Windows Dev Center dashboard, and configure services such as A/B testing and maps.
title: 應用程式管理與服務
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d0e4be450aa972ad8561f27a8d4749050458520a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2018
ms.locfileid: "4415270"
---
# <a name="app-management-and-services"></a>應用程式管理與服務

在 Windows 開發人員中心儀表板中，您可以管理和檢視與每個 App 相關的詳細資料，並設定像是通知、A/B 測試和地圖等服務。

當您在儀表板中使用 app 時，將會在 **\[服務\]** 與 **\[App 管理\]** 的左導覽功能表中看見一些區段。 您可以展開這些區段來存取如下所述的功能。

## <a name="services"></a>服務

**\[服務\]** 區段可讓您管理您 app 的數個不同服務。

## <a name="xbox-live"></a>Xbox Live

如果您正在發佈遊戲，您可以啟用此頁面上的[Xbox Live 創作者計畫](http://xbox.com/developers/creators-program)。 這可讓您啟動設定並測試 Xbox Live 功能，及最後發佈您的 Xbox Live 創作者計畫遊戲。

如需詳細資訊，請參閱[開始使用 Xbox Live 創作者計畫](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)，並[建立新的 Xbox Live 創作者計畫遊戲並發佈至測試環境](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md)。

## <a name="experimentation"></a>實驗

利用**實驗**頁面，使用 A/B 測試為您的通用 Windows 平台 (UWP) app 建立及執行實驗。 在 A/B 測試中，您可以先在一些客戶身上測量您 app 中功能變更 (或變化) 的有效性，然後再為每個人啟用變更。

如需詳細資訊，請參閱[使用 A/B 測試執行 app 實驗](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>地圖

若要在適用於 Windows Phone 8.1 和較舊版本的 app 中使用地圖服務，您需要在 app 程式碼中包含地圖服務 app 識別碼和權杖。 您可以在 **\[地圖\]** 頁面的 **\[服務\]** 區段中取得這個權杖。

> [!NOTE]
> 若要在目標為 Windows 10 或 Windows 8.x 的應用程式中使用地圖服務，請瀏覽 [Bing 地圖開發人員中心](http://go.microsoft.com/fwlink/p/?LinkId=614880)。 如需詳細資訊，請參閱[要求地圖驗證金鑰](https://docs.microsoft.com/windows/uwp/maps-and-location/authentication-key)。

如需詳細資訊，請參閱[使用地圖服務](use-map-services.md)。

## <a name="product-collections-and-purchases"></a>產品集合與購買

若要使用 Microsoft Store 集合 API 及 Microsoft Store 購買 API 來存取 app 和附加元件的擁有權資訊，您需要輸入相關聯 Azure AD 用戶端識別碼以下。 請注意，這些變更可能要花費最多 16 個小時才會生效。

如需詳細資訊，請參閱[管理服務的產品權利](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>系統管理員同意

f 與 Azure AD 整合您的產品，並呼叫的 Api，要求一個[應用程式的權限或委派權限](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)需要系統管理員的同意下，輸入您的 Azure AD 用戶端識別碼。 這可讓您以使它代表租用戶中的所有使用者的產品用於其組織授與同意取得的應用程式的系統管理員。

如需詳細資訊，請參閱[整個的租用戶同意的要求](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant)。

## <a name="app-management"></a>應用程式管理

**\[應用程式管理\]** 區段讓您能夠檢視身分識別和套件詳細資料，並管理您的應用程式名稱。

## <a name="app-identity"></a>應用程式身分識別

此頁面顯示市集內與您應用程式唯一身分識別相關的詳細資料，包含連結至您應用程式清單的 URL。

如需詳細資訊，請參閱[檢視 app 身分識別詳細資料](view-app-identity-details.md)。

## <a name="manage-app-names"></a>管理 app 名稱

您可以在此處檢視為應用程式保留的所有名稱。 您可以在此處保留其他名稱，或是刪除不再使用的名稱。

如需詳細資訊，請參閱[管理 app 名稱](manage-app-names.md)。

## <a name="current-packages"></a>目前的套件

此頁面讓您能夠檢視與您所有已發行套件相關的詳細資料。

> [!NOTE]
> 在您發行應用程式之前，將無法在此處看見任何資訊。

系統會顯示每個套件的名稱、版本及架構。 按一下 **\[詳細資料\]**，以顯示支援的語言、應用程式功能及檔案大小等其他資訊。 您看到的每個套件資訊，可能會因其目標作業系統以及其他因素有所不同。 

具備 OEM 權限的開發人員也可以從 **\[目前的套件\]** 頁面[產生預先安裝的套件](generate-preinstall-packages-for-oems.md)。

## <a name="wnsmpns"></a>WNS/MPNS\

[ **WNS/MPNS\** ] 區段會提供選項以協助您建立和傳送通知給您的應用程式客戶。 

> [!TIP]
> 適用於 UWP app 中，我們建議您在儀表板中使用**通知**選項。 這項功能可讓您傳送通知給所有您的應用程式的客戶，或您的 Windows 10 客戶符合條件的特定對象子集您所定義[的消費者區隔](create-customer-segments.md)中。 如需詳細資訊，請參閱[傳送通知給您的應用程式客戶](send-push-notifications-to-your-apps-customers.md)。

根據您的應用程式套件類型及其特定需求，您也可以使用其中一個下列選項： 

-   **Windows 推播通知服務 (WNS)** 讓您能夠從自己的雲端服務傳送快顯通知、磚、徽章及原始更新。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   **Microsoft Azure Mobile Apps** 讓您能夠傳送推播通知、驗證和管理應用程式使用者，以及在雲端中儲存應用程式資料。 如需詳細資訊，請參閱 [Mobile Apps 文件](http://go.microsoft.com/fwlink/p/?LinkId=221116)。

-   **Microsoft 推播通知服務 (MPNS)** 可以與您適用於 Windows Phone 的 .xap 套件搭配使用。 儘管我們建議使用已授權的通知以避免發生節流限制，但您不需在此處進行任何設定，就能傳送有限數量的未授權通知。 如果您使用的是 MPNS，您將需要將憑證上傳至 \ [ **WNS/MPNS\** ] 頁面上提供的欄位。 如需詳細資訊，請參閱[設定已驗證的 Web 服務來傳送適用於 Windows Phone 8 的推播通知](http://go.microsoft.com/fwlink/p/?LinkId=528736)。
 

 
