---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Xbox 的 Device Portal
description: 瞭解如何啟用適用于 Xbox One 的 Xbox 裝置入口網站，可讓您從遠端存取您的開發 Xbox。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: ed490b0474b919d4439e5b74b676d5974a3c6a30
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174782"
---
# <a name="device-portal-for-xbox"></a>Xbox 的 Device Portal

## <a name="set-up-device-portal-on-xbox"></a>在 Xbox 上設定 Device Portal

下列步驟顯示如何啟用 Xbox 裝置入口網站，讓您能夠遠端存取開發 Xbox。

1. 開啟 [開發人員首頁]。 這預設應該會在您啟動開發 Xbox 時開啟，但您也可以從主畫面將它開啟。

    ![Device Portal 開發人員首頁](images/device-portal-xbox-1.png)

2. 在開發人員首頁中的 **\[首頁\]** 索引標籤上，選取 **\[遠端存取\]** 底下的 **\[遠端存取設定\]**。

    ![裝置入口網站 RemoteManagement 工具](images/device-portal-xbox-15.png)

3. 選取 **\[啟用 Xbox 裝置入口網站\]** 設定。

4. 在 **\[驗證\]** 底下選取 **\[設定使用者名稱和密碼\]**。 輸入 **\[使用者名稱\]** 和 **\[密碼\]**，用來驗證從瀏覽器存取開發套件，然後 **\[儲存\]** 它們。

5. **\[關閉\]****\[遠端存取\]** 頁面，並記下**\[首頁\]** 索引標籤上 **\[遠端存取\]** 底下所列的 URL。

6. 將該 URL 輸入您的瀏覽器，然後以您設定的認證登入。

7. 您會收到關於已提供之憑證的警告，類似下圖所示。 在 Edge 中，按一下 **\[詳細資料\]**，然後 **\[移至網頁\]** 存取 Xbox 裝置入口網站。 在彈出的對話方塊中，輸入您先前在 Xbox 上輸入的使用者名稱和密碼。

    ![裝置入口網站憑證錯誤](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>裝置入口網站頁面

Xbox 裝置入口網站提供一組標準頁面，類似 Windows 裝置入口網站上提供的，另外還有幾個特殊的頁面。 如需前者的詳細描述，請參閱 [Windows 裝置入口網站概觀](../debug-test-perf/device-portal.md)。 下列章節描述 Xbox 裝置入口網站專有的頁面。

### <a name="home"></a>家庭

類似 Windows 裝置入口網站的 **\[應用程式管理員\]** 頁面，Xbox 裝置入口網站的 **\[首頁\]** 頁面也會顯示已安裝遊戲和 app 的清單，位於 **\[我的遊戲與 app\]** 底下。 按一下遊戲或 app 名稱，即可查看與其相關的詳細資料，例如 **\[套件系列名稱\]**。 在 **\[動作\]** 下拉式清單中，您可以對遊戲或 app 執行動作，例如將它 **\[啟動\]**。

在 **\[Xbox Live 測試帳戶\]** 底下，您可以管理與 Xbox 相關的帳戶。 您可以新增使用者和訪客帳戶、建立新使用者、將使用者登入和登出，以及移除帳戶。

![家庭](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (遊戲儲存)

Windows 裝置入口網站和 Xbox 裝置入口網站都有 **\[Xbox Live\]** 頁面。 不過，Xbox 裝置入口網站有一個專用區段 **\[Xbox Live 遊戲儲存\]**，可讓您儲存 Xbox 上已安裝遊戲的資料。 輸入與標題和遊戲儲存相關的 **\[服務設定 ID (SCID)\]** (如需詳細資訊，請參閱 [Xbox Live 服務設定](/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids))、**\[Membername (MSA)\]** 和 **\[套件系列名稱 (PFN)\]**，瀏覽 **\[輸入檔案 (.json 或.xml)\]**，然後選取其中一個按鈕 (**\[重設\]**、**\[匯入\]**、**\[匯出\]** 和 **\[刪除\]**) 來操控儲存資料。

在 **\[產生\]** 區段中，您可以產生虛設資料，並儲存到指定的輸入檔案。 只要輸入 **\[容器 (預設為 2)\]**、**\[Blob (預設為 3)\]** 和 **\[Blob 大小 (預設為 1024)\]**，然後選取 **\[產生\]**。

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>HTTP 監控

HTTP 監控可讓您檢視來自 app 或遊戲的解密 HTTP 和 HTTPS 流量，當它在您的 Xbox One 上執行時。

![HTTP 監控](images/device-portal-xbox-18.png)

若要啟用它，請開啟您 Xbox One 上的開發人員首頁，移至 **\[設定\]** 索引標籤，在 **\[HTTP 監視器設定\]** 方塊中核取 **\[啟用 HTTP 監控\]**。

![開發人員首頁：網路功能](images/device-portal-xbox-14.png)

一旦啟用，在 Xbox 裝置入口網站中選取個別按鈕，即可對 HTTP 和 HTTPS 流量執行 **\[停止\]**、**\[清除\]** 和 **\[儲存到檔案\]**。

### <a name="network-fiddler-tracing"></a>網路 (Fiddler 追蹤)

Xbox 裝置入口網站中的 **\[網路\]** 頁面幾乎與 Windows 裝置入口網站中的 **\[網路功能\]** 頁面相同，例外是 **\[Fiddler 追蹤\]**，這是 Xbox 裝置入口網站專屬。 這可讓您在 PC 上執行 Fiddler 來記錄和檢查 Xbox One 與網際網路之間的 HTTP 和 HTTPS 流量。 如需詳細資訊，請參閱[如何在開發 UWP 時使用 Fiddler 搭配 Xbox One](../xbox-apps/uwp-fiddler.md)。

![網路](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>媒體擷取

在 **\[媒體擷取\]** 頁面上，您可以選取 **\[擷取螢幕擷取畫面\]** 來拍攝 Xbox One 的螢幕擷取畫面。 擷取之後，您的瀏覽器將提示您下載檔案。 您可以核取 **\[偏好 HDR\]**，如果您想要拍攝 HDR 的螢幕擷取畫面 (主機支援的話)。

![媒體擷取](images/device-portal-xbox-12.png)

### <a name="settings"></a>設定

在 **\[設定\]** 頁面上，您可以檢視和編輯 Xbox One 的數項設定。 在頂端，您可以選取 **\[匯入\]**，從檔案匯入設定，以及選取 **\[匯出\]**，將目前設定匯出為 .txt 檔案。 匯入設定可讓大量編輯變得更容易，尤其是設定多台主機時。 若要建立可匯入的設定檔案，請依喜好變更設定，然後將設定匯出。 然後您可以使用此檔案快速輕鬆地為其他主機匯入設定。

有數個不同的區段中包含不同的設定可檢視和/或編輯，說明如下。

![設定](images/device-portal-xbox-20.png)

![設定](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>裝置資訊

* **裝置名稱**：裝置的名稱。 若要編輯，請變更方塊中的名稱並選取 **\[儲存\]**。

* **作業系統版本**：唯讀。 作業系統的版本號碼。

* **作業系統版本**：唯讀。 作業系統主要版本的名稱。

* **Xbox Live 裝置 ID**：唯讀。

* **主機 ID**：唯讀。

* **序號**：唯讀。

* **主機類型**：唯讀。 Xbox One 裝置的類型 (Xbox One、Xbox One S 或 Xbox One X)。

* **開發人員模式**：唯讀。 裝置所處的開發人員模式。

#### <a name="audio-settings"></a>音訊設定

* **音訊位元流格式**：音訊資料的格式。

* **HDMI 音訊**：經由 HDMI 連接埠的音訊類型。

* **耳機格式**：經由耳機的音訊格式。

* **光纖音訊**：經由光纖連接埠的音訊類型。

* **使用 HDMI 或光纖音訊耳機**：如果您使用的耳機是透過 HDMI 或光纖連接，請核取此方塊。

#### <a name="display-settings"></a>顯示器設定

* **色彩深度**：單一像素的每個色彩元件所使用的位元數目。

* **色彩空間**：顯示器可使用的色域。

* **顯示器解析度**：顯示器的解析度。

* **顯示器連接**：顯示器的連接類型。

* **允許高動態範圍 (HDR)**：啟用顯示器的 HDR。 僅適用於相容的顯示器。

* **允許 4K**：啟用顯示器的 4K 解析度。 僅適用於相容的顯示器。

* **允許可變更新頻率 (VRR)**：啟用顯示器的 VRR。 僅適用於相容的顯示器。

#### <a name="kinect-settings"></a>Kinect 設定

Kinect 感應器必須連接至主機，才能變更這些設定。

* **啟用 Kinect**：啟用連接的 Kinect 感應器。

* **App 變更時強制 Kinect 重新載入**：只要執行不同的 app 或遊戲，就重新載入連接的 Kinect 感應器。

#### <a name="localization-settings"></a>當地語系化設定

* **地理區域**︰裝置所設定的地理區域。 必須是專用的 2 個字元國碼 (地區碼) (例如，**US** 代表美國)。

* **慣用語言**：裝置所設定的語言。

* **時區**：裝置所設定的時區。

#### <a name="network-settings"></a>網路設定

* **無線通訊裝置設定**：裝置的無線設定 (特定方面如無線 LAN 為開啟或關閉狀態)。

#### <a name="power-settings"></a>電源設定

* **閒置時，畫面變暗前經過的時間 (分鐘)**：裝置閒置超過這段時間後，畫面將會變暗。 設定為 **0**，表示永遠不讓畫面變暗。

* **閒置時，關閉前經過的時間 (分鐘)**：裝置閒置超過這段時間後將會關閉。

* **電源模式**：裝置的電源模式。 如需詳細資訊，請參閱[關於省電和立即開啟模式](https://support.xbox.com/xbox-one/console/learn-about-power-modes)。

* **連接電源時主機自動開機**：裝置連接電源時將自動開啟。

#### <a name="preference-settings"></a>喜好設定

* **預設首頁體驗**：設定裝置開啟時出現的主畫面。

* **允許來自 Xbox 應用程式的連線**：另一部裝置 (例如 Windows 10 電腦) 上的 Xbox 應用程式可以連接到此主機。

* **預設將 UWP app 視為遊戲**：遊戲和 app 在 Xbox 上會得到不同的資源配置。 如果您核取此方塊，所有 UWP 套件都會識別為遊戲，因此將獲得更多資源。

#### <a name="user-settings"></a>使用者設定

* **自動登入使用者**：裝置啟動時自動登入選取的使用者。

* **自動登入使用者控制器**：自動將特殊控制器類型與特殊使用者產生關聯。

#### <a name="xbox-live-sandbox"></a>Xbox Live 沙箱

您可以在此變更裝置所在的 Xbox Live 沙箱。 在方塊中輸入沙箱的名稱，然後選取 **\[變更\]**。

### <a name="scratch"></a>臨時

這會空白工作區，可依您的喜好自訂。 您可以使用功能表 (按一下左上方的功能表按鈕) 來新增工具 (選取 **\[新增工具至工作區\]**，然後選取您要新增的工具，再選取 **\[新增\]**)。 請注意，您可以使用此功能表新增工具至任何工作區，以及管理工作區本身。

![新增工具至工作區](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>遊戲事件資料

在 [ **遊戲事件資料** ] 頁面上，您可以觀看即時圖形，以串流處理目前記錄在 Xbox One 上的 WINDOWS (ETW) 遊戲事件的事件追蹤數目。 如果系統上有錄製的遊戲事件，您也可以在資料圖形下方的資料表中，查看詳細資料， (事件名稱、事件出現次數和遊戲標題) 描述資料表中的每個事件。 只有在有記錄的事件時，才能使用此資料表。

![遊戲事件資料](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>另請參閱

* [Windows 裝置入口網站概觀](../debug-test-perf/device-portal.md)
* [裝置入口網站核心 API 參考資料](../debug-test-perf/device-portal-api-core.md)