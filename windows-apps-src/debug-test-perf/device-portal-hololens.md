---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: HoloLens 的 Device Portal
description: 了解 HoloLens 的 Windows Device Portal 如何讓您從遠端設定並管理 HoloLens 裝置。
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10, uwp, 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 059ce14f85ebe7d955ba2da8897ab47109f74a72
ms.sourcegitcommit: 1d6d05d28358e087d9ee8829d76c5fbbac0225cb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401967"
---
# <a name="device-portal-for-hololens"></a>HoloLens 的 Device Portal


## <a name="set-up-device-portal-on-hololens"></a>在 HoloLens 上設定裝置入口網站

### <a name="enable-device-portal"></a>啟用 Device Portal

1. 開啟您的 HoloLens 並將裝置戴上。
2. 執行 HoloLens (第 1 代) 的[開始手勢](https://docs.microsoft.com/hololens/hololens2-basic-usage#start-gesture)或[綻開](https://developer.microsoft.com/mixed-reality#Bloom)手勢以啟動主功能表。
3. 看一下 [設定]  磚，然後執行 HoloLens (第 1 代) 的[點選](https://developer.microsoft.com/mixed-reality#Press_and_release)手勢，或是在 HoloLens 2 上[觸碰或使用手部射線](https://docs.microsoft.com/hololens/hololens2-basic-usage)進行選取。 [設定] 應用程式會在您選取之後啟動。
4. 選取 [更新]  功能表項目。
5. 選取 [適用於開發人員]  功能表項目。
6. 啟用 [開發人員模式]  。
7. [向下捲動](https://developer.microsoft.com/mixed-reality#Navigation)並啟用 Device Portal。


### <a name="pair-your-device"></a>配對您的裝置

#### <a name="connect-over-wi-fi"></a>透過 Wi-Fi 連線 

1. 將您的 HoloLens 連線到 Wi-Fi。
2. 尋找裝置的 IP 位址。 在裝置上的 [設定] > [網路和網際網路] > [Wi-Fi] > [硬體屬性]  下尋找 IP 位址。 您也可以詢問：「嗨 Cortana，我的 IP 位址是什麼？」

3. 從您電腦上的網頁瀏覽器，移至 `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - 瀏覽器將會顯示下列訊息：「此網站的安全性憑證有問題」。 這是因為核發給 Device Portal 的憑證是測試憑證。 您可以暫時略過這個憑證錯誤並繼續。

#### <a name="connect-over-usb"></a>透過 USB 連線 

1. 安裝工具以確保您擁有 Visual Studio Update 1，並在電腦上安裝 Windows 10 開發人員工具。 這將能啟用 USB 連線能力。
2. 透過 Micro-USB 纜線將 HoloLens (第 1 代) 的 HoloLens 與電腦連線，或透過 USB-C 將 HoloLens 2 的 HoloLens 與電腦連線。
3. 從您電腦上的網頁瀏覽器，移至 `http://127.0.0.1:10080`。

> [!IMPORTANT]
> 如果您的電腦找不到裝置，請嘗試使用 HoloLens 裝置的實際網路 IP 位址，而不是 `http://127.0.0.1:10080`。

#### <a name="connect-to-an-emulator"></a>連線到模擬器 

您也可以透過模擬器使用 Device Portal。 若要連線到 Device Portal，請使用工具列。 按一下這個圖示：
- 開啟裝置入口網站：在模擬器中開啟 HoloLens OS 的 Windows 裝置入口網站。

#### <a name="create-a-username-and-password"></a>建立使用者名稱和密碼。 

首次在 HoloLens 上連線到 Device Portal 時，您必須建立使用者名稱和密碼。
1. 在您電腦上的網頁瀏覽器中，輸入 HoloLens 的 IP 位址。 [設定存取] 頁面將會開啟。
2. 按一下或點選 [要求 PIN]，並查看 HoloLens 顯示畫面以取得產生的 PIN。
3. 在 [PIN] 中輸入在您裝置文字方塊上顯示的 PIN。
4. 輸入您會用來連線到 Device Portal 的使用者名稱。 它並不需要是 Microsoft 帳戶 (MSA) 或網域名稱。
5. 輸入密碼並確認它。 密碼長度必須至少為七個字元。 它並不需要是 MSA 或網域密碼。
6. 按一下 [配對] 來在 HoloLens 上連線到 Windows Device Portal。

如果您想在任何時候變更此使用者名稱或密碼，您可以造訪裝置安全性頁面來重複此程序，方法是按一下位於右上方的 [安全性] 連結，或是瀏覽到：`https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`。

#### <a name="security-certificate"></a>安全性憑證 

如果您在瀏覽器中看見「憑證錯誤」，您可以透過和裝置建立信任關係來修正它。

每個 HoloLens 都會為其 SSL 連線產生唯一的自我簽署憑證。 根據預設，此憑證並不會受到您電腦的網頁瀏覽器信任，因此您可能會收到「憑證錯誤」。 藉由從您的 HoloLens 下載此憑證 (透過 USB 或您信任的 Wi-Fi 網路)，並在電腦上信任它，您便能安全地連線到您的裝置。
1. 請確保您是在安全的網路上 (USB 或您信任的 Wi-Fi 網路)。
2. 從裝置入口網站的 [安全性] 頁面下載此裝置的憑證。您可以按一下右上方圖示清單中的 [安全性] 連結，或是瀏覽到：`https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`

3. 安裝您電腦上「信任的根憑證授權」存放區中的憑證。在 Windows 功能表輸入：管理電腦憑證並啟動 applet。
    - 展開 [可信任的根憑證授權單位] 資料夾。
    - 按一下 [憑證] 資料夾。
    - 從 [動作] 功能表中，選取：[所有工作] > [匯入]...
    - 使用您從 Device Portal 下載的憑證檔案完成憑證匯入精靈。

4. 重新啟動瀏覽器。


## <a name="device-portal-pages"></a>Device Portal 頁面 

### <a name="home"></a>首頁 

您的 Device Portal 工作階段從首頁開始。 從首頁左側的瀏覽列中存取其他頁面。

頁面頂端的工具列能讓您存取常用狀態與功能。
- **線上**：指出裝置是否已連線到 Wi-Fi。
- **關機**：關閉裝置。
- **重新啟動**：關閉裝置電源並重新開啟。
- **安全性**：開啟 [裝置安全性] 頁面。
- **冷卻**：指出裝置的溫度。
- **A/C**：指出裝置是否已插上電源並且正在充電。
- **說明**：開啟 REST 介面文件頁面。

首頁將顯示下列資訊：
- **裝置**狀態：監視裝置的健康狀況並報告嚴重錯誤。
- **Windows 資訊**：顯示 HoloLens 的名稱，以及目前安裝的 Windows 版本。
- [喜好設定]  區段包含下列設定：
    - **IPD**：設定瞳孔間距 (IPD)，這是使用者雙眼注視前方時，兩個瞳孔中心點之間的距離 (以公釐為單位)。 此設定會立即生效。 預設值會在您設定裝置時自動計算。 **僅適用於 HoloLens (第1代)，Hololens 2 會計算眼睛位置。** 
    - **裝置名稱**：為 HoloLens 指派名稱。 在變更此值之後，必須重新啟動裝置才能生效。 按一下 [儲存] 之後，將會有對話方塊詢問您是否要立即重新啟動裝置，或稍後再重新啟動。
    - **睡眠設定**：設定裝置在接上電源及使用電池的情況下，在進入睡眠之前所需等待的時間長度。

### <a name="3d-view"></a>3D 檢視 

使用 [3D 檢視] 頁面來查看 HoloLens 解譯您周遭環境的方式。 使用滑鼠來瀏覽檢視：
- **旋轉**：按下滑鼠左鍵 + 移動滑鼠；
- **平移**：按下滑鼠右鍵 + 移動滑鼠；
- **縮放**：滑鼠滾輪。
- **追蹤選項**：選取 [強制視覺追蹤] 來開啟持續性視覺追蹤。 暫停將會停止視覺追蹤。
- **檢視選項**：設定 3D 視圖上的選項：- 追蹤：指出視覺追蹤是否為作用中。
- **顯示樓面**：顯示棋盤式的樓面規劃。
- **顯示範圍**：顯示檢視範圍。
- **顯示穩定平面**：顯示 HoloLens 用來穩定動作的平面。
- **顯示網格**：顯示代表您周遭環境的表面對應網格。
- **顯示詳細資料**：隨著下列數據變更，即時顯示雙手位置、頭部旋轉四元數，以及裝置原點向量。
- **全螢幕按鈕**：以全螢幕模式顯示 3D 檢視。 按下 ESC 以離開全螢幕檢視。

- 表面重建：按一下或點選 [更新] 以顯示裝置中最新的空間對應網格。 可能需要花費一些時間 (最多幾秒鐘) 才能完成完整作業。 網格並不會在 3D 檢視中自動更新，您必須手動按一下 [更新] 以取得來自裝置的最新網格。 按一下 [儲存] 以將目前的空間對應網格在您的電腦上儲存為 obj 檔案。

### <a name="mixed-reality-capture"></a>混合實境擷取 

使用 [混合實境擷取] 頁面來儲存來自 HoloLens 的媒體串流。
- 設定：藉由檢查下列設定來控制所擷取的媒體串流：- Holograms：在影片串流中擷取全像投影內容。 全像投影是以單聲道進行轉譯，而非立體聲。
- **PV 相機**：擷取來自相片/攝影機的影片串流。
- **麥克風音訊**：擷取來自麥克風陣列的音訊。
- **App 音訊**：擷取來自目前執行中 App 的音訊。
- **即時預覽品質**：選取即時預覽的螢幕解析度、畫面播放速率，以及串流速率。

- 按一下或點選 [即時預覽] 按鈕以顯示擷取串流。 停止即時預覽將會停止擷取串流。
- 按一下或點選 [錄製] 來開始使用指定的設定錄製混合實境串流。 停止錄製將會中止並儲存錄製內容。
- 按一下或點選 [拍攝相片] 來從擷取串流中拍攝靜止影像。
- **影片與相片**：顯示在裝置上所拍攝影片和相片擷取的清單。

請注意，HoloLens App 無法在您正在從 Device Portal 錄製或串流即時預覽時，擷取 MRC 相片或視訊。

### <a name="system-performance"></a>系統效能 

HoloLens 上的 [系統效能] 工具有額外 3 個可記錄的衡量標準。 

下列為可用的衡量標準：
- **SoC 電源**：系統單晶片電源瞬間使用率 (一分鐘內的平均值)
- **系統電源**：系統電源瞬間使用率 (一分鐘內的平均值)
- **畫面播放速率**：每秒畫面格數、每秒遺失的 VBlanks 數，以及連續遺失的 VBlanks 數

### <a name="app-crash-dumps-page"></a>App 損毀傾印頁面 

此頁面可讓您收集側載 App 的損毀傾印。 請針對您想要收集損毀傾印的 App，選取它們 [啟用的損毀傾印] 核取方塊。 返回此頁面以收集損毀傾印。 傾印檔案可以在 Visual Studio 中開啟以進行偵錯。

### <a name="kiosk-mode"></a>Kiosk 模式 

啟用 Kiosk 模式將能限制使用者啟動新 App 或變更執行中 App 的能力。 啟用 Kiosk 模式時，將會停用「盛開」手勢和 Cortana，而已放置的 App 將不會在使用者的周遭環境中顯示。

選取 [啟用 Kiosk 模式] 來使 HoloLens 進入 Kiosk 模式。 從 [啟動 App] 下拉式清單中選取要在啟動時執行的 App。 按一下或點選 [儲存] 以認可設定。

請注意，該 App 將會在啟動時執行，就算沒有啟用 Kiosk 模式也一樣。 選取 [無] 以在啟動時不執行任何 App。

### <a name="simulation"></a>模擬 

允許您記錄並播放輸入資料以進行測試。
- **擷取房間**：用來下載模擬的房間檔案，其中包含針對使用者周遭環境的空間對應網格。 為房間命名，然後按一下 [擷取] 來將資料在您的電腦上儲存為 .xef 檔案。 此房間檔案可以載入 HoloLens 模擬器。
- **錄製**：檢查要錄製的串流、為錄製命名，然後按一下或點選 [錄製] 開始錄製。 使用您的 HoloLens 執行動作，然後按一下 [停止] 來將資料在您的電腦上儲存為 .xef 檔案。 此檔案可以載入 HoloLens 模擬器或裝置。
- **播放**：按一下或點選 [上傳錄製]，從電腦中選取 xef 檔案，並將資料傳送到 HoloLens。
- **控制模式**：從下拉式清單中選取 [預設] 或 [模擬]，然後按一下或點選 [設定] 按鈕來選取 HoloLens 上的模式。 選擇 [模擬] 將會停用 HoloLens 上實際的感應器，並改為使用已上傳的模擬資料。 如果您切換到 [模擬]，您的 HoloLens 將不會對真實使用者做出回應，直到您切換回 [預設] 為止。


### <a name="virtual-input"></a>虛擬輸入 

從遠端電腦傳送鍵盤輸入到 HoloLens。

按一下或點選 [虛擬鍵盤] 下方的區域，來啟用傳送按鍵輸入到 HoloLens。 在 [輸入文字] 文字方塊中輸入，然後按一下或點選 [傳送] 來將按鍵輸入傳送到使用中的 App。

## <a name="see-also"></a>另請參閱

* [Windows 裝置入口網站概觀](device-portal.md)
* [裝置入口網站核心 API 參考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core) (所有 Windows 10 裝置通用的 API)
* [裝置入口網站混合實境 API 參考](https://docs.microsoft.com/windows/mixed-reality/device-portal-api-reference) (適用於 HoloLens 的所有 REST API 的更詳盡清單)
