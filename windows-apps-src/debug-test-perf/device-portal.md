---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概觀
description: 了解 Windows Device Portal 如何讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "74254766"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概觀

Windows Device Portal 能讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。 它也提供進階診斷工具來協助您疑難排解和檢視您 Windows 裝置的即時效能。

Windows 裝置入口網站是您裝置上的網頁伺服器，您可以從電腦上的網頁瀏覽器與它連線。 若您的裝置有網頁瀏覽器，您也可以與該裝置上的瀏覽器進行本機連線。

每個裝置系列上都提供 Windows 裝置入口網站，但是功能和設定會依據各裝置的需求而有所不同。 此文章提供 Device Portal 的一般描述，也提供有關每個裝置系統更特定資訊的文章連結。

Windows 裝置入口網站中的功能是透過 [REST API](device-portal-api-core.md) (可用來直接存取資料並以程式設計方式控制裝置) 所實作。

## <a name="setup"></a>安裝程式

每個裝置都有連線到 Device Portal 的特定指示，但是每個裝置都需要執行這些一般步驟：

1. 在您的裝置上啟用開發人員模式與裝置入口網站 (在 [設定] 應用程式中設定)。

2. 透過區域網路或 USB，將裝置與電腦連線。

3. 在瀏覽器中瀏覽到 Device Portal 頁面。 此表顯示每個裝置系列使用的連接埠與通訊協定。

裝置系列 | 預設開啟？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，在開發人員模式 | 80 (預設值) | 443 (預設值) | http://127.0.0.1:10080
IoT | 是，在開發人員模式 | 8080 | 透過登錄機碼啟用 | 不適用
Xbox | 在開發人員模式內啟用 | 停用 | 11443 | 不適用
[桌上型電腦]| 在開發人員模式內啟用 | 50080\* | 50043\* | 不適用
電話 | 在開發人員模式內啟用 | 80| 443 | http://127.0.0.1:10080

\* 情況並非總是如此，因為電腦上的裝置入口網站會宣告位於暫時性範圍內的連接埠 (>50,000)，以避免與裝置上現有的連接埠宣告相衝突。 若要深入了解，請參閱適用於電腦的[連接埠設定](device-portal-desktop.md#registry-based-configuration-for-device-portal)一節。  

如需裝置特定的安裝指示，請參閱︰

- [HoloLens 的裝置入口網站](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [IoT 的裝置入口網站](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [行動裝置的行動裝置](device-portal-mobile.md)
- [Xbox 的裝置入口網站](../xbox-apps/device-portal-xbox.md)
- [傳統型裝置的裝置入口網站](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具列與瀏覽

頁面頂端的工具列能讓您存取常用功能。

- **電源**：存取電源選項。
  - **關機**：關閉裝置。
  - **重新啟動**：關閉裝置電源並重新開啟。
- **說明**：開啟說明頁面。

使用頁面左側瀏覽窗格中的連結，瀏覽到適用於您裝置的管理與監視工具。

這裡描述各種裝置系列常見的工具。 根據不同的裝置，可能提供其他選項。 如需詳細資訊，請參閱您裝置類型的特定頁面。

### <a name="apps-manager"></a>應用程式管理員

應用程式管理員對於主機裝置上的應用程式套件與套件組合，提供了安裝/解除安裝及管理功能。

![裝置入口網站應用程式管理員頁面](images/device-portal/WDP_AppsManager2.png)

* **部署應用程式**：從本機、網路或 Web 主機部署套件應用程式，並從網路共用註冊鬆散的檔案。
* **已安裝的應用程式**：使用下拉式功能表移除或啟動安裝在裝置上的應用程式。
* **執行中應用程式**：取得目前執行中應用程式的相關資訊，並視需要加以關閉。

#### <a name="install-sideload-an-app"></a>安裝 (側載) 應用程式

您可以使用 Windows 裝置入口網站，在開發期間側載應用程式：

1. 當您建立應用程式套件後，可以從遠端將其安裝到您的裝置上。 在 Visual Studio 中建置後，會產生輸出資料夾。

    ![App 安裝](images/device-portal/iot-installapp0.png)

2. 在 Windows 裝置入口網站中，瀏覽至 [應用程式管理員]  頁面。

3. 在 [部署應用程式]  區段中，選取 [本機存放區]  。

4. 在 [選取應用程式套件]  下，選取 [選擇檔案]  ，並瀏覽至您要側載的應用程式套件。

5. 在 [選取用於簽署應用程式套件的憑證檔案 (.cer)]  下，選取 [選擇檔案]  ，並瀏覽至與應用程式套件相關聯的憑證。

6. 如果您在安裝應用程式時，要一起安裝選用項目或架構套件，請勾選對應的方塊，並選取 [下一步]  加以選擇。

7. 選取 [安裝]  ，即可開始安裝。

8. 如果裝置是以 S 模式執行 Windows 10，而且是第一次在裝置上安裝指定的憑證，請重新啟動裝置。

#### <a name="install-a-certificate"></a>安裝憑證

或者也可以透過 Windows 裝置入口網站安裝憑證，並以其他方式安裝應用程式：

1. 在 Windows 裝置入口網站中，瀏覽至 [應用程式管理員]  頁面。

2. 在 [部署應用程式]  區段中，選取 [安裝憑證]  。

3. 在 [選取用於簽署應用程式套件的憑證檔案 (.cer)]  下，選取 [選擇檔案]  ，並瀏覽至與您要側載的應用程式套件相關聯的憑證。

4. 選取 [安裝]  ，即可開始安裝。

5. 如果裝置是以 S 模式執行 Windows 10，而且是第一次在裝置上安裝指定的憑證，請重新啟動裝置。

#### <a name="uninstall-an-app"></a>解除安裝 App

1. 請確定您的應用程式不在執行中。
2. 若正在執行，請移至 [執行中應用程式]  ，並將其關閉。 若您在應用程式仍在執行時將其解除安裝，會在嘗試重新安裝應用程式時導致問題。
3. 從下拉式清單中選取應用程式，並按一下 [移除]  。

### <a name="running-processes"></a>執行中的處理程序

此頁面顯示目前主機裝置上執行中處理程序的詳細資訊。 這包括 App 與系統處理程序。 在某些平台 (電腦、IoT 與 HoloLens) 上，您可以終止處理程序。

![裝置入口網站執行中處理程序頁面](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>檔案總管

您可在此頁面檢視和操作側載應用程式所存放的檔案。 請參閱[使用應用程式檔案總管](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)部落格文章 (英文)，深入了解檔案總管以及使用方法。

![裝置入口網站檔案總管頁面](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>效能

[效能] 頁面顯示系統診斷資訊的即時圖表，例如電源使用量、畫面播放速率與 CPU 負載。

下列為可用的衡量標準：

- **CPU**：可用 CPU 使用率總計百分比
- **記憶體**：總計、使用中、可用、已認可、分頁、非分頁
- **I/O**：讀取與寫入資料數量
- **網路**：接收與傳送資料
- **GPU**：可用 GPU 引擎使用率總計百分比

![裝置入口網站效能頁面](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Windows 事件追蹤 (ETW) 記錄

ETW 記錄頁面會管理裝置上的即時 Windows 事件追蹤 (ETW) 資訊。

![裝置入口網站 ETW 記錄頁面](images/device-portal/mob-device-portal-etw.png)

選取 [隱藏提供者]  ，便只會顯示 [事件] 清單。

- **已註冊的提供者**：選取事件提供者與追蹤層級。 追蹤層級是下列其中一個值：
  1. 異常結束或終止
  2. 嚴重錯誤
  3. 警告
  4. 非錯誤警告
  5. 追蹤的詳細資料

  按一下或點選 [啟用]  以開始追蹤。 提供者已新增到 [啟用的提供者]  下拉式清單中。
- **自訂提供者**：選取自訂 ETW 提供者與追蹤層級。 依 GUID 識別提供者。 不要在 GUID 中包含括號。
- **啟用的提供者**：會列出已啟用的提供者。 從下拉式清單選取提供者，然後按一下或點選 [停用]  以停止追蹤。 按一下或點選 [全部停止]  以暫停所有追蹤。
- **提供者歷程記錄**：會顯示目前工作階段期間已啟用的 ETW 提供者。 按一下或點選 [啟用]  以啟用已停用的提供者。 按一下或點選 [清除]  以清除歷程記錄。
- **篩選條件/事件**:[事件]  區段會以表格格式列出所選提供者的 ETW 事件。 此表格會即時更新。 使用 [篩選條件]  功能表可設定自訂篩選條件，篩選出要顯示的事件。 按一下 [清除]  按鈕，即可將所有 ETW 事件從表格中刪除。 這不會停用任何提供者。 您可以按一下 [儲存到檔案]  ，在本機將目前收集的 ETW 事件匯出為本機 CSV 檔案。

如需使用 ETW 記錄的詳細資訊，請參閱[使用裝置入口網站以檢視偵錯記錄](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)部落格文章 (英文)。

### <a name="performance-tracing"></a>效能追蹤

[效能追蹤] 頁面可供檢視來自主機裝置的 [Windows Performance Recorder (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) 追蹤。

![裝置入口網站效能追蹤頁面](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用的設定檔**：從下拉式清單選取 WPR 設定檔，然後按一下或點選 [開始]  以開始追蹤。
- **自訂設定檔**：按一下或點選 [瀏覽]  ，以從您的電腦選擇 WPR 設定檔。 按一下或點選 [上傳並開始]  以開始追蹤。

若要停止追蹤，請按一下 [停止]  。 留在此頁面上，直到追蹤檔案 (.ETL) 下載完成。

擷取的 ETL 檔案可以在 [Windows 效能分析器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10))中開啟以進行分析。

### <a name="device-manager"></a>裝置管理員

[裝置管理員] 頁面會列舉連結至您裝置的所有周邊設備。 您可以按一下設定圖示，檢視每個屬性。

![裝置入口網站裝置管理員頁面](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>網路功能

[網路功能] 頁面會管理裝置上的網路連線。 除非您透過 USB 連線到裝置入口網站，否則變更這些設定很可能會中斷您與裝置入口網站的連線。

- **可用網路**：顯示裝置可用的 WiFi 網路。 按一下或點選網路可讓您與它連線，而且您必須視需要提供密碼。 裝置入口網站尚未支援企業驗證。 您也可以使用 [設定檔]  下拉式清單，嘗試連線到裝置已知的 WiFi 設定檔。
- **IP 設定**：顯示每個主機裝置網路連接埠位址的相關資訊。

![裝置入口網站網路功能頁面](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>服務功能和附註

### <a name="dns-sd"></a>DNS-SD

Device Portal 會使用 DNS-SD 在區域網路上公告其目前狀態。 所有的 Device Portal 執行個體都會在 "WDP._wdp._tcp.local" 底下公告，不論其裝置類型為何。 服務執行個體的 TXT 記錄會提供下列項目：

按鍵 | 類型 | 說明
----|------|-------------
S | int | Device Portal 的安全連接埠。 如果為 0 (零)，Device Portal 不會接聽 HTTPS 連線。
D | 字串 | 裝置類型。 格式將為「Windows.*」，例如 Windows.Xbox 或 Windows.Desktop
A | 字串 | 裝置架構。 這將是 ARM、x86 或 AMD64。  
T | null 字元字串分隔清單 | 裝置的使用者套用標記。 請參閱＜標記 REST API＞以了解如何使用。 清單是以兩個 NULL 結束。  

建議連接 HTTPS 連接埠，因為並非所有的裝置都會接聽由 DNS-SD 記錄公告的 HTTP 埠。

### <a name="csrf-protection-and-scripting"></a>CSRF 保護和指令碼處理

為了防止 [CSRF 攻擊](https://en.wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 要求都需要唯一權杖。 這個 X-CSRF-Token 要求標頭的權杖是衍生自工作階段 Cookie，CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 會複製到各個要求的 X-CSRF-Token 標頭中。

> [!IMPORTANT]
> 這個保護功能可防止從獨立用戶端 (例如，命令列公用程式) 使用 REST API。 這可以 3 種方式解決：
> - 使用 "auto-" 使用者名稱。 使用者名稱前面加上 "auto-" 的用戶端將略過 CSRF 保護。 請務必注意，不可以透過瀏覽器使用此使用者名稱來登入 Device Portal，因為它會將服務開放給 CSRF 攻擊。 範例：如果裝置入口網站使用者名稱為 "admin"，應使用 ```curl -u auto-admin:password <args>``` 略過 CSRF 保護。
> - 在用戶端中實作 cookie-to-header 配置。 這需要 GET 要求建立工作階段 Cookie，然後在所有後續要求同時包含標頭與 Cookie。
> - 停用驗證，並使用 HTTP。 CSRF 保護僅適用於 HTTPS 端點，因此 HTTP 端點上的連線不需要執行上述各項。

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨網站 WebSocket 攔截 (CSWSH) 保護

若要防止 [CSWSH 攻擊](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，開啟 WebSocket 連接到裝置入口網站的所有用戶端，必須也提供符合 Host 標頭的 Origin 標頭。 這可向裝置入口網站證明要求是來自裝置入口網站 UI 或有效的用戶端應用程式。 沒有 Origin 標頭您的要求將會被拒絕。
