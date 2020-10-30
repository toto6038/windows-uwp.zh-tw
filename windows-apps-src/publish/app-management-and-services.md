---
description: 在合作夥伴中心中管理和查看與每個應用程式相關的詳細資料，並設定例如 A/B 測試和對應的服務。
title: 應用程式管理與服務
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e973e9b3a8f3c9ba63a091f4e542e36a84c26128
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032751"
---
# <a name="app-management-and-services"></a>應用程式管理與服務

您可以在 [合作夥伴中心](https://partner.microsoft.com/dashboard)中管理和查看與每個應用程式相關的詳細資料，並設定通知、A/B 測試和對應等服務。

使用合作夥伴中心中的應用程式時，您會在左側導覽功能表中看到 **服務** 和 **應用程式管理** 的區段。 您可以展開這些區段來存取如下所述的功能。

## <a name="services"></a>服務

[ **服務** ] 區段可讓您管理應用程式的數個不同服務。

## <a name="xbox-live"></a>Xbox Live

如果您要發佈遊戲，您可以在此頁面上啟用此 [Xbox Live 創作者計畫](https://www.xbox.com/developers/creators-program) 。 這可讓您開始設定和測試 Xbox Live 功能，並最終發佈 Xbox Live 創作者計畫遊戲。

如需詳細資訊，請參閱 [開始使用 Xbox Live 創作者計畫](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) 並 [建立新的 Xbox Live 創作者計畫標題，併發布至測試環境](/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title)。

## <a name="experimentation"></a>測試

利用 **實驗** 頁面，使用 A/B 測試為您的通用 Windows 平台 (UWP) app 建立及執行實驗。 在 A/B 測試中，您可以先在一些客戶身上測量您 app 中功能變更 (或變化) 的有效性，然後再為每個人啟用變更。

如需詳細資訊，請參閱[使用 A/B 測試執行 app 實驗](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>地圖服務

若要在目標為 Windows 10 或 Windows 8.x 的應用程式中使用地圖服務，請瀏覽 [Bing 地圖開發人員中心](https://www.bingmapsportal.com/)。 如需如何從 Bing Maps 開發人員中心要求 maps 驗證金鑰，並將其新增至您的應用程式的詳細資訊，請參閱 [要求地圖服務驗證金鑰](../maps-and-location/authentication-key.md) 以取得詳細資訊。 

針對 Windows Phone 8.1 和更早版本的先前發行的應用程式，請使用 [ **對應** ] 頁面。 若要在這些應用程式中使用地圖服務，您必須要求地圖服務應用程式識別碼和權杖，以包含在應用程式的程式碼中。 當您按一下 [ **取得權杖** ] 時，我們會為您的應用程式產生 ( **ApplicationID** ) 和地圖服務驗證權杖 ( **AuthenticationToken** ) 的地圖服務應用程式識別碼。 封裝並提交您的應用程式之前，請務必將這些值新增至您的程式碼。 如需詳細資訊，請參閱[如何新增地圖控制項到頁面 (Windows Phone 8.1)](/previous-versions/windows/apps/jj207033(v=vs.105))。

## <a name="product-collections-and-purchases"></a>產品集合與購買

若要使用 Microsoft Store 收集 API 和 Microsoft Store 購買 API 來存取應用程式和附加元件的擁有權資訊，您必須在這裡輸入相關聯的 Azure AD 用戶端識別碼。 請注意，這些變更可能要花費最多 16 個小時才會生效。

如需詳細資訊，請參閱[管理服務的產品權利](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>系統管理員同意

如果您的產品與 Azure AD 整合，並且呼叫 Api 來要求需要系統管理員同意的 [應用程式許可權或委派的許可權](/graph/permissions-reference) ，請在這裡輸入您的 Azure AD 用戶端識別碼。 這可讓為其組織取得應用程式的系統管理員授與您的產品同意，以代表租使用者中的所有使用者採取行動。

如需詳細資訊，請參閱 [要求整個租使用者的同意](/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant)。

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

系統會顯示每個套件的名稱、版本及架構。 按一下 [ **詳細資料** ] 以顯示其他資訊，例如支援的語言、應用程式功能和檔案大小。 您看到的每個套件資訊，可能會因其目標作業系統以及其他因素有所不同。 

具有 OEM 許可權的開發人員也可以從目前的 **封裝** 頁面 [產生預先安裝套件](generate-preinstall-packages-for-oems.md)。

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS** 區段提供選項，可協助您建立通知並傳送給應用程式的客戶。 

> [!TIP]
> 針對 UWP 應用程式，我們建議使用合作夥伴中心中的 [ **通知** ] 功能。 這項功能可讓您將通知傳送給您的應用程式的所有客戶，或傳送給符合您在 [客戶區段](create-customer-segments.md)中定義之準則的 Windows 10 客戶的目標子集。 如需詳細資訊，請參閱[傳送通知給您的應用程式客戶](send-push-notifications-to-your-apps-customers.md)。

您也可以使用下列其中一個選項，視您的應用程式套件類型及其特定需求而定： 

-   **Windows 推播通知服務 (WNS)** 讓您能夠從自己的雲端服務傳送快顯通知、磚、徽章及原始更新。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   **Microsoft Azure Mobile Apps** 讓您能夠傳送推播通知、驗證和管理應用程式使用者，以及在雲端中儲存應用程式資料。 如需詳細資訊，請參閱 [Mobile Apps 文件](/azure/app-service-mobile/)。

-   **Microsoft 推播通知服務 (MPNS)** 可搭配先前發行的 .xap 套件用於 Windows Phone。 儘管我們建議使用已授權的通知以避免發生節流限制，但您不需在此處進行任何設定，就能傳送有限數量的未授權通知。 如果您使用的是 MPNS，則必須將憑證上傳至 [ **WNS/MPNS** ] 頁面上提供的欄位。 如需詳細資訊，請參閱[設定已驗證的 Web 服務來傳送適用於 Windows Phone 8 的推播通知](/previous-versions/windows/apps/ff941099(v=vs.105))。
 

 
