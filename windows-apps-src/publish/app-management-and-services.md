---
Description: 管理及檢視您的應用程式，在合作夥伴中心內的每個相關的詳細資料，並設定服務，例如 A / B 測試，並將對應。
title: 應用程式管理與服務
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ba28fb49accc213f570e470142e259c08122b704
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334576"
---
# <a name="app-management-and-services"></a>應用程式管理與服務

您可以管理及檢視您的應用程式中的每個相關的詳細資料[合作夥伴中心](https://partner.microsoft.com/dashboard/)，並設定服務，例如通知、 A / B 測試，並將對應。

當使用合作夥伴中心中的應用程式，您會看到的左側的導覽功能表中的章節**Services**並**應用程式管理**。 您可以展開這些區段來存取如下所述的功能。

## <a name="services"></a>服務

\[服務\] 區段可讓您管理您 app 的數個不同服務。

## <a name="xbox-live"></a>Xbox Live

如果您要發行的遊戲，您可以啟用[Xbox Live 創作者計劃](https://xbox.com/developers/creators-program)此頁面上。 這可讓您啟動設定及測試 Xbox Live 功能，並最終發佈您的 Xbox Live 創作者計劃的遊戲。

如需詳細資訊，請參閱 <<c0> [ 開始使用 Xbox Live 創作者計劃](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators)並[建立新的 Xbox Live 創作者計劃標題並發行至測試環境](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title)。

## <a name="experimentation"></a>實驗

利用**實驗**頁面，使用 A/B 測試為您的通用 Windows 平台 (UWP) app 建立及執行實驗。 在 A/B 測試中，您可以先在一些客戶身上測量您 app 中功能變更 (或變化) 的有效性，然後再為每個人啟用變更。

如需詳細資訊，請參閱[使用 A/B 測試執行 app 實驗](../monetize/run-app-experiments-with-a-b-testing.md)。

## <a name="maps"></a>地圖

若要在目標為 Windows 10 或 Windows 8.x 的應用程式中使用地圖服務，請瀏覽 [Bing 地圖開發人員中心](https://go.microsoft.com/fwlink/p/?LinkId=614880)。 如需如何從 Bing 地圖服務開發人員中心要求對應的驗證金鑰，並將它加入您的應用程式的資訊，請參閱 <<c0> [ 要求對應的驗證金鑰](../maps-and-location/authentication-key.md)如需詳細資訊。 

使用**Maps**只針對先前已發行應用程式適用於 Windows Phone 8.1 及更早版本的頁面。 若要使用這些應用程式中的地圖服務，您必須要求的對應服務應用程式識別碼和權杖包含在您的應用程式程式碼。 當您按一下 **取得權杖**，我們將產生的地圖服務應用程式識別碼 (**ApplicationID**)，並將對應服務驗證權杖 (**AuthenticationToken**) 應用程式。 請務必將這些值加入至您的程式碼，您就可以封裝之前，並提交您的應用程式。 如需詳細資訊，請參閱[如何新增地圖控制項到頁面 (Windows Phone 8.1)](https://go.microsoft.com/fwlink/p/?LinkId=614882)。

## <a name="product-collections-and-purchases"></a>產品集合與購買

若要使用的 Microsoft Store 收集 API 和 Microsoft Store 購買 API 來存取應用程式和附加元件的擁有權資訊，您需要輸入相關聯之 Azure AD 用戶端識別碼這裡。 請注意，這些變更可能要花費最多 16 個小時才會生效。

如需詳細資訊，請參閱[管理服務的產品權利](../monetize/view-and-grant-products-from-a-service.md)。

## <a name="administrator-consent"></a>系統管理員同意

如果您的產品與 Azure AD 整合，並呼叫其中一個要求的 Api[應用程式權限或委派的權限](https://developer.microsoft.com/graph/docs/concepts/permissions_reference)需要系統管理員同意，請輸入您的 Azure AD 用戶端識別碼。 這可讓系統管理員取得其組織授與同意，為您的產品，代表租用戶中的所有使用者的應用程式。

如需詳細資訊，請參閱 <<c0> [ 要求對整個租用戶的同意](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant)。

## <a name="app-management"></a>應用程式管理

**\[應用程式管理\]** 區段讓您能夠檢視身分識別和套件詳細資料，並管理您的應用程式名稱。

## <a name="app-identity"></a>應用程式身分識別

此頁面顯示市集內與您應用程式唯一身分識別相關的詳細資料，包含連結至您應用程式清單的 URL。

如需詳細資訊，請參閱[檢視 app 身分識別詳細資料](view-app-identity-details.md)。

## <a name="manage-app-names"></a>管理應用程式名稱

您可以在此處檢視為應用程式保留的所有名稱。 您可以在此處保留其他名稱，或是刪除不再使用的名稱。

如需詳細資訊，請參閱[管理 app 名稱](manage-app-names.md)。

## <a name="current-packages"></a>目前的套件

此頁面讓您能夠檢視與您所有已發行套件相關的詳細資料。

> [!NOTE]
> 在您發行應用程式之前，將無法在此處看見任何資訊。

系統會顯示每個套件的名稱、版本及架構。 按一下 \[詳細資料\]，以顯示支援的語言、應用程式功能及檔案大小等其他資訊。 您看到的每個套件資訊，可能會因其目標作業系統以及其他因素有所不同。 

具備 OEM 權限的開發人員也可以從 \[目前的套件\] 頁面[產生預先安裝的套件](generate-preinstall-packages-for-oems.md)。

## <a name="wnsmpns"></a>WNS/MPNS

**WNS/MPNS**區段會提供選項，可協助您建立及傳送通知給您的應用程式的客戶。 

> [!TIP]
> 針對 UWP 應用程式，我們建議使用**通知**在合作夥伴中心內的功能。 這項功能可讓您傳送通知給所有您的應用程式的客戶，或您定義中的 Windows 10 的客戶符合準則的目標子集[客群](create-customer-segments.md)。 如需詳細資訊，請參閱[傳送通知給您的應用程式客戶](send-push-notifications-to-your-apps-customers.md)。

根據您的應用程式封裝類型和其特定的需求，您也可以使用下列選項之一： 

-   **Windows 推播通知服務 (WNS)** 讓您能夠從自己的雲端服務傳送快顯通知、磚、徽章及原始更新。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md)。

-   **Microsoft Azure Mobile App** 讓您能夠傳送推播通知、驗證和管理 app 使用者，以及在雲端中儲存 app 資料。 如需詳細資訊，請參閱 [Mobile Apps 文件](https://go.microsoft.com/fwlink/p/?LinkId=221116)。

-   **Microsoft 推播通知服務 (MPNS)** 可以搭配先前發佈的.xap 套件適用於 Windows Phone。 儘管我們建議使用已授權的通知以避免發生節流限制，但您不需在此處進行任何設定，就能傳送有限數量的未授權通知。 如果您使用 MPNS，您必須將憑證上傳至上提供欄位**WNS/MPNS**頁面。 如需詳細資訊，請參閱[設定已驗證的 Web 服務來傳送適用於 Windows Phone 8 的推播通知](https://go.microsoft.com/fwlink/p/?LinkId=528736)。
 

 
