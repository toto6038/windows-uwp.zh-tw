---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概觀
description: 了解 Windows Device Portal 如何讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。
ms.date: 12/12/2017
ms.topic: article
keywords: windows 10，uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 2bffdb31e9001bd0b2abe873780ef507c2073b46
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8480545"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概觀

Windows Device Portal 能讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。 它也提供進階診斷工具來協助您疑難排解和檢視您的 Windows 裝置的即時效能。

Windows Device Portal 是您可以從電腦上的網頁瀏覽器連線到您裝置上的網頁伺服器。 如果您的裝置有網頁瀏覽器，您也可以連線在本機使用該裝置上的瀏覽器。

Windows 裝置入口網站上都提供每個裝置系列，但是功能和設定而有所不同每個裝置的需求。 此文章提供 Device Portal 的一般描述，也提供有關每個裝置系統更特定資訊的文章連結。

在 Windows 裝置入口網站的功能是使用[REST Api](device-portal-api-core.md) ，您可以使用直接來存取資料並以程式設計方式控制裝置實作。

## <a name="setup"></a>安裝程式

每個裝置都有連線到 Device Portal 的特定指示，但是每個裝置都需要執行這些一般步驟：
1. （在 [設定] app 中設定） 在裝置上啟用開發人員模式與 Device Portal。
2. 將裝置與電腦連接透過區域網路或 USB。
3. 在瀏覽器中瀏覽到 Device Portal 頁面。 下表顯示連接埠與每個裝置系列使用的通訊協定。

裝置系列 | 預設開啟？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，在開發人員模式 | 80 (預設值) | 443 (預設值) | http://127.0.0.1:10080
IoT | 是，在開發人員模式 | 8080 | 透過登錄機碼啟用 | N/A
Xbox | 在開發人員模式內啟用 | 已停用 | 11443 | N/A
電腦| 在開發人員模式內啟用 | 50080\* | 50043\* | N/A
手機 | 在開發人員模式內啟用 | 80| 443 | http://127.0.0.1:10080

\* 情況並非總是如此，因為電腦上的 Device Portal 會宣告位於暫時性範圍內的連接埠 (&gt;50000)，以避免與裝置上現有的連接埠宣告相衝突。 若要深入了解，請參閱適用於電腦的[連接埠設定](device-portal-desktop.md#registry-based-configuration-for-device-portal)一節。  

如需裝置特定的安裝指示，請參閱︰
- [HoloLens 的 Device Portal](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [IoT 的裝置入口網站](https://go.microsoft.com/fwlink/?LinkID=616499)
- [行動裝置的 Device Portal](device-portal-mobile.md)
- [Xbox 的 Device Portal](device-portal-xbox.md)
- [傳統型裝置的 Device Portal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具列與瀏覽

在頁面頂端的工具列提供常用功能的存取權。
- **電源**： 存取 [電源選項。
  - **關機**：關閉裝置。
  - **重新啟動**：關閉裝置電源並重新開啟。
- **說明**︰開啟 [說明] 頁面。

使用頁面左側瀏覽窗格中的連結，瀏覽到適用於您裝置的管理與監視工具。

這裡描述跨裝置系列是常見的工具。 根據不同的裝置，可能提供其他選項。 如需詳細資訊，請參閱您的裝置類型的特定頁面。

### <a name="apps-manager"></a>應用程式管理員

應用程式管理員提供安裝/解除安裝和管理功能的應用程式套件和套件組合主機裝置上。

![裝置入口網站應用程式管理員] 頁面](images/device-portal/wdp-apps.png)

- **已安裝應用程式**： 使用下拉式功能表中移除，或開始在裝置安裝的應用程式。 按一下 [**新增**安裝新的應用程式。 這會起始安裝已封裝的應用程式從本機部署的 UX、 網路或 web 裝載並登錄鬆散檔案從網路共用。
- **執行應用程式**： 取得目前正在執行，且在必要時關閉它們的應用程式的相關資訊。

#### <a name="install-an-app"></a>安裝 App

1.  建立應用程式套件之後，您可以從遠端將它安裝到您的裝置上。 在 Visual Studio 中建置後，會產生輸出資料夾。
  ![App 安裝](images/device-portal/iot-installapp0.png)
2.  在裝置入口網站的應用程式管理員區段中，按一下 [**新增**]，然後選取**安裝應用程式套件從本機存放區**。
3.  按一下 [**瀏覽**，並尋找您的應用程式套件。
3.  按一下 [**瀏覽]** 並尋找憑證 (_.cer_) 檔案 （不需要在所有裝置上）。
4.  檢查個別的方塊，如果您想要安裝選用或架構套件，以及應用程式安裝。 如果有一個以上的相依性，請個別新增每個相依性。     
5.  按一下 [**下一步**將移至下一個步驟和**安裝**來初始化安裝。 

#### <a name="uninstall-an-app"></a>解除安裝 App
1.  請確定您的應用程式不在執行中。 
2.  如果是，請移至**執行應用程式**，並將它關閉。 如果您嘗試解除安裝應用程式正在執行時，它將會造成問題，當您嘗試重新安裝應用程式。 
3.  從下拉式清單中選取應用程式，然後按一下 [**移除**。

### <a name="running-processes"></a>執行處理程序

此頁面會顯示有關目前在主機裝置上執行的處理程序的詳細資料。 這包括 App 與系統處理程序。 在某些平台 （桌面、 IoT、 與 HoloLens），您可以終止處理程序。

![裝置入口網站執行處理頁面](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>檔案總管

此頁面可讓您檢視和操作任何側載 app 所儲存的檔案。 請參閱[使用應用程式檔案總管](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)部落格文章以了解有關 [檔案總管] 中，以及如何使用它。 

![裝置入口網站檔案總管] 頁面](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>效能

效能頁面就會顯示系統診斷資訊，例如電源使用量、 畫面播放速率的即時圖表，及 CPU 負載。

下列為可用的衡量標準：
- **CPU**： 的總可用的 CPU 使用率百分比
- **記憶體**： 總數，在使用中，可用、 已認可、 分頁，與非分頁
- **I/O**： 讀取和寫入資料數量
- **網路**： 收到並傳送資料
- **GPU**: %的總可用 GPU 引擎使用率


![裝置入口網站效能頁面](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>事件 Windows 追蹤 (ETW) 記錄

ETW 記錄頁面管理裝置上的即時事件 Windows 追蹤 (ETW) 資訊。

![裝置入口網站 ETW 記錄頁面](images/device-portal/mob-device-portal-etw.png)

選取 **\[隱藏提供者\]** 以只顯示 [事件] 清單。
- **已登錄提供者**： 選取事件提供者與追蹤等級。 追蹤等級是其中一個值：
  1. 異常結束或終止
  2. 嚴重錯誤
  3. 警告
  4. 非錯誤警告
  5. 追蹤的詳細的資料

  按一下或點選 **\[啟用\]** 以開始追蹤。 提供者已新增到 **\[啟用的提供者\]** 下拉式清單中。
- **自訂提供者**︰選取自訂 ETW 提供者與追蹤等級。 依 GUID 識別提供者。 不要在 GUID 中包含括號。
- **已啟用的提供者**： 這會列出已啟用的提供者。 從下拉式清單選取提供者，然後按一下或點選 **\[停用\]** 以停止追蹤。 按一下或點選 **\[全部停止\]** 以暫停所有追蹤。
- **提供者歷程記錄**： 這會顯示目前的工作階段期間已啟用的 ETW 提供者。 按一下或點選 **\[啟用\]** 以啟用已停用的提供者。 按一下或點選 **\[清除\]** 以清除歷程記錄。
- **篩選 / 事件**: [**事件**] 區段會列出來自以表格格式所選提供者的 ETW 事件。 表格會即時更新。 若要設定自訂篩選事件將會顯示為其使用**篩選器**功能表。 按一下 [**清除**] 按鈕以從表格中刪除所有 ETW 事件。 這不會停用任何提供者。 您可以按一下 [**儲存到檔案**來將目前收集的 ETW 事件匯出到本機的 CSV 檔案。

如需使用 ETW 記錄的詳細資訊，請參閱[使用裝置入口網站，以檢視偵錯記錄](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)部落格文章。 

### <a name="performance-tracing"></a>效能追蹤

效能追蹤頁面可讓您檢視主機裝置從[Windows Performance Recorder (WPR)](https://msdn.microsoft.com/library/hh448205.aspx)追蹤。

![裝置入口網站效能追蹤] 頁面](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用的設定檔**︰從下拉式清單選取 WPR 設定檔，然後按一下或點選 **\[開始\]** 以開始追蹤。
- **自訂設定檔**︰按一下或點選 **\[瀏覽\]**，以從您的電腦選擇 WPR 設定檔。 按一下或點選 **\[上傳並開始\]** 以開始追蹤。

若要停止追蹤，請按一下 **\[停止\]**。 停留在此頁面上，直到追蹤檔案 (。ETL) 完成下載。

擷取。可以開啟 ETL 檔案以進行分析[Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/desktop/hh448170.aspx)中。

### <a name="device-manager"></a>裝置管理員

[裝置管理員] 頁面會列舉連接到您的裝置的所有周邊。 您可以按一下設定圖示，以檢視每個屬性。

![裝置入口網站裝置管理員] 頁面](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>網路

網路功能頁面管理裝置上的網路連線。 除非您已連線到 Device Portal，透過 USB，否則變更這些設定會可能會中斷連線您從 Device Portal。
- **可用的網路**： 會顯示可供裝置使用的 WiFi 網路。 按一下或點選網路可讓您與它連線，而且您必須視需要提供密碼。 Device Portal 還不支援企業驗證。 您也可以使用 [**設定檔**] 下拉式清單來嘗試連線到任何已知為裝置的 WiFi 設定檔。
- **IP 設定**： 裝置的網路連接埠會顯示有關每個主機的地址資訊。

![裝置入口網站的網路功能頁面](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>服務功能和附註

### <a name="dns-sd"></a>DNS-SD

Device Portal 會使用 DNS-SD 在區域網路上公告其目前狀態。 所有的 Device Portal 執行個體都會在 "WDP._wdp._tcp.local" 底下公告，不論其裝置類型為何。 服務執行個體的 TXT 記錄會提供下列項目：

索引鍵 | 類型 | 說明 
----|------|-------------
S | int | Device Portal 的安全連接埠。 如果為 0 (零)，Device Portal 不會接聽 HTTPS 連線。 
D | string | 裝置類型。 格式將為 "Windows.*"，例如 Windows.Xbox 或 Windows.Desktop
A | string | 裝置架構。 這將是 ARM、x86 或 AMD64。  
T | null 字元字串分隔清單 | 裝置的使用者套用標記。 請參閱＜標記 REST API＞以了解如何使用。 清單是以兩個 NULL 結束。  

建議連接 HTTPS 連接埠，因為並非所有的裝置都會接聽由 DNS-SD 記錄公告的 HTTP 埠。 

### <a name="csrf-protection-and-scripting"></a>CSRF 保護和指令碼處理

為了防止 [CSRF 攻擊](https://wikipedia.org/wiki/Cross-site_request_forgery)，所有非 GET 要求都需要唯一權杖。 這個 X-CSRF-Token 要求標頭的權杖是衍生自工作階段 Cookie，CSRF-Token。 在 Device Portal Web UI 中，CSRF-Token Cookie 會複製到各個要求的 X-CSRF-Token 標頭中。

> [!IMPORTANT]
> 此保護可防止使用 REST Api 從獨立用戶端 （例如命令列公用程式）。 這可以 3 種方式解決： 
> - 使用"auto-"使用者名稱。 使用者名稱前面加上 "auto-" 的用戶端將略過 CSRF 保護。 請務必注意，不可以透過瀏覽器使用此使用者名稱來登入 Device Portal，因為它會將服務開放給 CSRF 攻擊。 範例︰如果 Device Portal 的使用者名稱為 "admin"，```curl -u auto-admin:password <args>``` 應該用來略過 CSRF 保護。 
> - 在用戶端中實作 cookie-to-header 配置。 這需要 GET 要求建立工作階段 Cookie，然後在所有後續要求同時包含標頭與 Cookie。 
> - 停用驗證，並使用 HTTP。 CSRF 保護僅適用於 HTTPS 端點，因此 HTTP 端點上的連線不需要執行上述各項。 

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨網站 WebSocket 攔截 (CSWSH) 保護

若要防止 [CSWSH 攻擊](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，開啟 WebSocket 連接到裝置入口網站的所有用戶端，必須也提供符合 Host 標頭的 Origin 標頭。 這可向裝置入口網站證明要求是來自裝置入口網站 UI 或有效的用戶端應用程式。 沒有 Origin 標頭您的要求將會被拒絕。 
