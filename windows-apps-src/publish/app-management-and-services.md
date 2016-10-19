---
author: jnHs
Description: "您可以在 Windows 開發人員中心儀表板中，管理和檢視與每個應用程式相關的詳細資料，並設定像是推播通知和地圖等服務。"
title: "App 管理與服務"
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: 9787ef724622a95d291b4631196b3e13bcb1298a

---

# App 管理與服務

您可以在 Windows 開發人員中心儀表板中，管理和檢視與每個應用程式相關的詳細資料，並設定像是推播通知和地圖等服務。

當您在儀表板中使用 app 時，將會在 [服務] 與 [App 管理] 的左導覽功能表中看見一些區段。 您可以展開這些區段來存取如下所述的功能。

## 服務

[服務]**** 區段可讓您管理您 app 的數個不同服務。

### 推播通知

根據應用程式的套件類型及其特定需求而定，您可以針對推播通知使用下列其中一個選項：

-   **Windows 推播通知服務 (WNS)** 讓您能夠從自己的雲端服務傳送快顯通知、磚、徽章及原始更新。 如需詳細資訊，請參閱 [Windows 推播通知服務 (WNS) 概觀](https://msdn.microsoft.com/library/windows/apps/mt187203)。

-   **Microsoft Azure Mobile App** 讓您能夠傳送推播通知、驗證和管理 app 使用者，以及在雲端中儲存 app 資料。 如需詳細資訊，請參閱 [Mobile Apps 文件](http://go.microsoft.com/fwlink/p/?LinkId=221116)。

-   **Microsoft 推播通知服務 (MPNS)** 可以與您適用於 Windows Phone 的 .xap 套件搭配使用。 儘管我們建議使用已授權的通知以避免發生節流限制，但您不需在此處進行任何設定，就能傳送有限數量的未授權通知。 如果使用的是 MPNS，就需要將憑證上傳至 [推播通知]**** 頁面上提供的欄位。 如需詳細資訊，請參閱[設定已驗證的 Web 服務來傳送適用於 Windows Phone 8 的推播通知](http://go.microsoft.com/fwlink/p/?LinkId=528736)。

### 實驗

利用**實驗**頁面，使用 A/B 測試為您的通用 Windows 平台 (UWP) app 建立及執行實驗。 在 A/B 測試中，您可以先在一些客戶身上測量您 app 中功能變更 (或變化) 的有效性，然後再為每個人啟用變更。

如需詳細資訊，請參閱[使用 A/B 測試執行 app 實驗](../monetize/run-app-experiments-with-a-b-testing.md)。

### 地圖

若要在適用於 Windows Phone 8.1 和較舊版本的 app 中使用地圖服務，您需要在 app 程式碼中包含地圖服務 app 識別碼和權杖。 您可以在 [地圖]**** 頁面的 [服務]**** 區段中取得這個權杖。

> **注意：**若要在目標為其他作業系統的 app 中使用地圖服務，請瀏覽 [Bing 地圖開發人員中心](http://go.microsoft.com/fwlink/p/?LinkId=614880)。 如需詳細資訊，請參閱[要求地圖驗證金鑰](https://msdn.microsoft.com/library/windows/apps/mt219694)。

如需詳細資訊，請參閱[使用地圖服務](use-map-services.md)。

### 產品集合與購買

若要使用 Windows 市集集合 API 與 Windows 市集購買 API 來存取 App 及附加元件的擁有權資訊，您必須在這裡輸入相關聯的 Azure AD 用戶端識別碼。 請注意，這些變更可能要花費最多 16 個小時才會生效。

如需詳細資訊，請參閱[從服務檢視與授與產品](https://msdn.microsoft.com/library/windows/apps/mt609002)。

## 應用程式管理

[App 管理]**** 區段讓您能夠檢視身分識別和套件詳細資料，並管理您的 app 名稱。

### 應用程式身分識別

此頁面顯示市集內與您應用程式唯一身分識別相關的詳細資料，包含連結至您應用程式清單的 URL。

如需詳細資訊，請參閱[檢視 app 身分識別詳細資料](view-app-identity-details.md)。

### 管理 app 名稱

您可以在此處檢視為應用程式保留的所有名稱。 您可以在此處保留其他名稱，或是刪除不再使用的名稱。

如需詳細資訊，請參閱[管理 app 名稱](manage-app-names.md)。

### 目前的套件

此頁面讓您能夠檢視與您所有已發行套件相關的詳細資料。

> **注意：**在您發行 app 之前，將無法在此處看見任何資訊。

系統會顯示每個套件的名稱、版本及架構。 按一下 [詳細資料]****，以顯示支援的語言、應用程式功能及檔案大小等其他資訊。

您對每個套件可以看到的精確資訊，可能會因其目標作業系統以及其他因素有所不同。 例如，如果您已新增 [Windows 廣告流量分配](https://msdn.microsoft.com/library/windows/apps/mt219691)到您的套件，您會在這裡發現設定該套件的流量分配的連結。

具備 OEM 權限的開發人員也可以從 [目前的套件]**** 頁面[產生預先安裝的套件](generate-preinstall-packages-for-oems.md)。

 

 



<!--HONumber=Aug16_HO3-->


