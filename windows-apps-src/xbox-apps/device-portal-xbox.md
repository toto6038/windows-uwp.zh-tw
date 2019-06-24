---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Xbox 的 Device Portal
description: 了解如何啟用 Xbox One 的 Device Portal。
ms.date: 4/9/2019
ms.topic: article
keywords: windows 10 uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: c701dc7eef4d37f28db9f9cabd7bbb0f39a92a4c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322090"
---
# <a name="device-portal-for-xbox"></a>Xbox 的 Device Portal

## <a name="set-up-device-portal-on-xbox"></a>在 Xbox 上設定 Device Portal

下列步驟顯示如何啟用 Xbox 裝置入口網站，讓您能夠遠端存取開發 Xbox。

1. 開啟 [開發人員首頁]。 這預設應該會在您啟動開發 Xbox 時開啟，但您也可以從主畫面將它開啟。

    ![Device Portal 開發人員首頁](images/device-portal-xbox-1.png)

2. 在開發人員首頁中的 **\[首頁\]** 索引標籤上，選取 **\[遠端存取\]** 底下的 **\[遠端存取設定\]** 。

    ![裝置入口網站 RemoteManagement 工具](images/device-portal-xbox-15.png)

3. 選取 **\[啟用 Xbox 裝置入口網站\]** 設定。

4. 在 **\[驗證\]** 底下選取 **\[設定使用者名稱和密碼\]** 。 輸入 **\[使用者名稱\]** 和 **\[密碼\]** ，用來驗證從瀏覽器存取開發套件，然後 **\[儲存\]** 它們。

5. **\[關閉\]** **\[遠端存取\]** 頁面，並記下 **\[首頁\]** 索引標籤上 **\[遠端存取\]** 底下所列的 URL。

6. 將該 URL 輸入您的瀏覽器，然後以您設定的認證登入。

7. 您會收到關於已提供之憑證的警告，類似下圖所示。 在 Edge 中，按一下 **\[詳細資料\]** ，然後 **\[移至網頁\]** 存取 Xbox 裝置入口網站。 在彈出的對話方塊中，輸入您先前在 Xbox 上輸入的使用者名稱和密碼。

    ![Device Portal 憑證錯誤](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Device Portal 頁面

Xbox 裝置入口網站提供一組標準頁面，類似 Windows 裝置入口網站上提供的，另外還有幾個特殊的頁面。 如需前者的詳細描述，請參閱 [Windows 裝置入口網站概觀](../debug-test-perf/device-portal.md)。 下列章節描述 Xbox 裝置入口網站專有的頁面。

### <a name="home"></a>首頁

類似 Windows 裝置入口網站的 **\[應用程式管理員\]** 頁面，Xbox 裝置入口網站的 **\[首頁\]** 頁面也會顯示已安裝遊戲和 app 的清單，位於 **\[我的遊戲與 app\]** 底下。 按一下遊戲或 app 名稱，即可查看與其相關的詳細資料，例如 **\[套件系列名稱\]** 。 在 **\[動作\]** 下拉式清單中，您可以對遊戲或 app 執行動作，例如將它 **\[啟動\]** 。

在 **\[Xbox Live 測試帳戶\]** 底下，您可以管理與 Xbox 相關的帳戶。 您可以新增使用者和訪客帳戶、建立新使用者、將使用者登入和登出，以及移除帳戶。

![首頁](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (遊戲儲存)

Windows 裝置入口網站和 Xbox 裝置入口網站都有 **\[Xbox Live\]** 頁面。 不過，Xbox 裝置入口網站有一個專用區段 **\[Xbox Live 遊戲儲存\]** ，可讓您儲存 Xbox 上已安裝遊戲的資料。 輸入與標題和遊戲儲存相關的 **\[服務設定 ID (SCID)\]** (如需詳細資訊，請參閱 [Xbox Live 服務設定](https://docs.microsoft.com/gaming/xbox-live/xbox-live-service-configuration.md#get-your-ids))、 **\[Membername (MSA)\]** 和 **\[套件系列名稱 (PFN)\]** ，瀏覽 **\[輸入檔案 (.json 或.xml)\]** ，然後選取其中一個按鈕 ( **\[重設\]** 、 **\[匯入\]** 、 **\[匯出\]** 和 **\[刪除\]** ) 來操控儲存資料。

在 **\[產生\]** 區段中，您可以產生虛設資料，並儲存到指定的輸入檔案。 只要輸入 **\[容器 (預設為 2)\]** 、 **\[Blob (預設為 3)\]** 和 **\[Blob 大小 (預設為 1024)\]** ，然後選取 **\[產生\]** 。

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>HTTP 監控

HTTP 監控可讓您檢視來自 app 或遊戲的解密 HTTP 和 HTTPS 流量，當它在您的 Xbox One 上執行時。

![HTTP 監控](images/device-portal-xbox-18.png)

若要啟用它，請開啟您 Xbox One 上的開發人員首頁，移至 **\[設定\]** 索引標籤，在 **\[HTTP 監視器設定\]** 方塊中核取 **\[啟用 HTTP 監控\]** 。

![開發人員的首頁：網路功能](images/device-portal-xbox-14.png)

一旦啟用，在 Xbox 裝置入口網站中選取個別按鈕，即可對 HTTP 和 HTTPS 流量執行 **\[停止\]** 、 **\[清除\]** 和 **\[儲存到檔案\]** 。

### <a name="network-fiddler-tracing"></a>網路 (Fiddler 追蹤)

Xbox 裝置入口網站中的 **\[網路\]** 頁面幾乎與 Windows 裝置入口網站中的 **\[網路功能\]** 頁面相同，例外是 **\[Fiddler 追蹤\]** ，這是 Xbox 裝置入口網站專屬。 這可讓您在 PC 上執行 Fiddler 來記錄和檢查 Xbox One 與網際網路之間的 HTTP 和 HTTPS 流量。 如需詳細資訊，請參閱[如何在開發 UWP 時使用 Fiddler 搭配 Xbox One](../xbox-apps/uwp-fiddler.md)。

![Network](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>媒體擷取

在 **\[媒體擷取\]** 頁面上，您可以選取 **\[擷取螢幕擷取畫面\]** 來拍攝 Xbox One 的螢幕擷取畫面。 擷取之後，您的瀏覽器將提示您下載檔案。 您可以核取 **\[偏好 HDR\]** ，如果您想要拍攝 HDR 的螢幕擷取畫面 (主機支援的話)。

![媒體擷取](images/device-portal-xbox-12.png)

### <a name="settings"></a>設定

在 **\[設定\]** 頁面上，您可以檢視和編輯 Xbox One 的數項設定。 在頂端，您可以選取 **\[匯入\]** ，從檔案匯入設定，以及選取 **\[匯出\]** ，將目前設定匯出為 .txt 檔案。 匯入設定可讓大量編輯變得更容易，尤其是設定多台主機時。 若要建立可匯入的設定檔案，請依喜好變更設定，然後將設定匯出。 然後您可以使用此檔案快速輕鬆地為其他主機匯入設定。

有數個不同的區段中包含不同的設定可檢視和/或編輯，說明如下。

![設定](images/device-portal-xbox-20.png)

![設定](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>裝置資訊

* **裝置名稱**:裝置的名稱。 若要編輯，請變更方塊中的名稱並選取 **\[儲存\]** 。

* **OS 版本**:唯讀的。 作業系統的版本號碼。

* **OS 版本**:唯讀的。 作業系統主要版本的名稱。

* **Xbox Live 的裝置識別碼**:唯讀的。

* **主控台識別碼**:唯讀的。

* **序號**:唯讀的。

* **主控台類型**:唯讀的。 Xbox One 裝置的類型 (Xbox One、Xbox One S 或 Xbox One X)。

* **開發人員模式**:唯讀的。 裝置所處的開發人員模式。

#### <a name="audio-settings"></a>音訊設定

* **音訊位元資料流格式**:音訊資料格式。

* **HDMI 音訊**:音訊透過 HDMI 連接埠類型。

* **耳機格式**:來自耳機的音訊格式。

* **光學音訊**:音訊，透過光學的連接埠類型。

* **使用 HDMI 或光學音訊耳機**:如果您要使用耳機連接透過 HDMI 或光碟，請核取此方塊。

#### <a name="display-settings"></a>顯示器設定

* **色彩深度**:使用每個色彩元件的單一像素位元數。

* **色彩空間**:可用的顯示色彩範圍。

* **顯示器解析度**:顯示器的解析度。

* **顯示連線**:要顯示的連接類型。

* **允許高動態範圍 (HDR)** :顯示畫面上，可讓 HDR。 僅適用於相容的顯示器。

* **允許 4k**:可讓 4k 解析度的顯示器上。 僅適用於相容的顯示器。

* **允許變數的重新整理的頻率 (VRR)** :啟用 VRR 顯示器上。 僅適用於相容的顯示器。

#### <a name="kinect-settings"></a>Kinect 設定

Kinect 感應器必須連接至主機，才能變更這些設定。

* **啟用 Kinect**:啟用附加的 Kinect 感應器。

* **強制 Kinect 重新載入應用程式變更**:每次執行不同的應用程式或遊戲時，請重新載入附加的 Kinect 感應器。

#### <a name="localization-settings"></a>當地語系化設定

* **地理區域**:裝置設定為地理區域。 必須是專用的 2 個字元國碼 (地區碼) (例如，**US** 代表美國)。

* **慣用語言**:裝置設定為語言。

* **時區**:裝置設定為時區。

#### <a name="network-settings"></a>網路設定

* **無線選項設定**:（例如無線區域網路的某些方面是否開啟或關閉） 之裝置的無線設定。

#### <a name="power-settings"></a>電源設定

* **當處於閒置狀態時之後 （分鐘）, 的維度螢幕**:裝置已閒置這段時間後，會變暗的螢幕。 設定為 **0**，表示永遠不讓畫面變暗。

* **當處於閒置狀態時，完成之後關閉**:已閒置的這段時間之後，就會關閉裝置。

* **電源模式**:裝置的電源模式。 如需詳細資訊，請參閱[關於省電和立即開啟模式](https://support.xbox.com/xbox-one/console/learn-about-power-modes)。

* **自動啟動主控台時連線到 power**:裝置會自動開啟連接到電源時。

#### <a name="preference-settings"></a>喜好設定

* **預設的居家體驗**:開啟裝置時，會出現的主畫面的設定。

* **允許從 Xbox 應用程式的連線**:在另一個裝置 （例如 Windows 10 電腦） 上的 Xbox 應用程式可以連接到這個主控台。

* **UWP 應用程式視為預設的遊戲**:遊戲和應用程式取得不同的資源，在 Xbox 上配置給他們。 如果您核取此方塊，所有 UWP 套件都會識別為遊戲，因此將獲得更多資源。

#### <a name="user-settings"></a>使用者設定

* **自動登入使用者**:當您開啟裝置時，會自動登入選取的使用者。

* **自動登入使用者的控制器**:自動將特定控制器的型別與特定使用者。

#### <a name="xbox-live-sandbox"></a>Xbox Live 沙箱

您可以在此變更裝置所在的 Xbox Live 沙箱。 在方塊中輸入沙箱的名稱，然後選取 **\[變更\]** 。

### <a name="scratch"></a>臨時

這會空白工作區，可依您的喜好自訂。 您可以使用功能表 (按一下左上方的功能表按鈕) 來新增工具 (選取 **\[新增工具至工作區\]** ，然後選取您要新增的工具，再選取 **\[新增\]** )。 請注意，您可以使用此功能表新增工具至任何工作區，以及管理工作區本身。

![新增工具至工作區](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>遊戲事件資料

在 [**遊戲事件資料**] 頁面上，您可以檢視串流的即時圖形中的事件追蹤的 Windows (ETW) 目前記錄在您的 Xbox One 遊戲的事件數目。 如果有記錄在系統上的遊戲事件，您也可以檢視詳細資料 （事件名稱、 事件發生和遊戲的標題） 描述的資料圖形下方的資料表中每一個事件。 資料表才可以使用記錄的事件。

![遊戲事件資料](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>另請參閱

* [Windows Device Portal 概觀](../debug-test-perf/device-portal.md)
* [裝置入口網站 core API 參考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
