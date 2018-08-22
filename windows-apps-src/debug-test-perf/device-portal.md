---
author: PatrickFarley
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概觀
description: 了解 Windows Device Portal 如何讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。
ms.author: pafarley
ms.date: 12/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 08e7d8fcfbab0d0b22fffa3e3e0aecc38d5b095c
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "2799655"
---
# <a name="windows-device-portal-overview"></a>Windows Device Portal 概觀

Windows Device Portal 能讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。 它也會提供可協助您疑難排解並檢視您的 Windows 裝置的即時效能的進階診斷工具。

Windows 裝置入口網站是您可以從網頁瀏覽器中的電腦連線到裝置上的網頁伺服器。 如果您的裝置網頁瀏覽器中，您也可以連接本機與該裝置瀏覽器。

Windows 裝置入口網站上每個裝置系列產品，但是功能及安裝隨每個裝置的需求。 此文章提供 Device Portal 的一般描述，也提供有關每個裝置系統更特定資訊的文章連結。

您可以使用直接存取資料以及以程式設計方式控制您的裝置的[REST api （英文)](device-portal-api-core.md)中已實作 Windows 裝置入口網站的功能。

## <a name="setup"></a>安裝程式

每個裝置都有連線到 Device Portal 的特定指示，但是每個裝置都需要執行這些一般步驟：
1. 啟用開發人員模式和裝置入口網站上的裝置 （在設定應用程式中設定）。
2. 透過區域網路或 USB 連接裝置和 PC。
3. 在瀏覽器中瀏覽到 Device Portal 頁面。 此表說明的連接埠和使用的每個裝置系列通訊協定。

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
- [行動裝置的裝置入口網站](device-portal-mobile.md)
- [Xbox 的 Device Portal](device-portal-xbox.md)
- [傳統型裝置的 Device Portal](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>功能

### <a name="toolbar-and-navigation"></a>工具列與瀏覽

在頁面頂端工具列提供最常使用的功能的存取權。
- **進階**： 存取電源選項。
  - **關機**：關閉裝置。
  - **重新啟動**：關閉裝置電源並重新開啟。
- **說明**︰開啟 [說明] 頁面。

使用頁面左側瀏覽窗格中的連結，瀏覽到適用於您裝置的管理與監視工具。

以下說明不同裝置系列都通用的工具。 根據不同的裝置，可能提供其他選項。 如需詳細資訊，請參閱您的裝置類型的特定頁面。

### <a name="apps-manager"></a>應用程式管理員

應用程式管理員提供安裝/解除安裝和管理功能的應用程式封裝，並可將主機裝置上。

![裝置入口網站應用程式管理員] 頁面](images/device-portal/wdp-apps.png)

- **已安裝的應用程式**： 若要移除或啟動裝置上所安裝之應用程式使用的下拉式清單功能表。 按一下 [**新增**安裝新的應用程式。 這起始 UX 部署封裝應用程式從本機安裝、 網路或 web 主控及登錄鬆散從網路共用的檔案。
- **執行應用程式**： 取得目前正在執行並關閉其所需的應用程式的詳細資訊。

#### <a name="install-an-app"></a>安裝 App

1.  建立應用程式套件之後，您可以從遠端將它安裝到您的裝置上。 在 Visual Studio 中建置後，會產生輸出資料夾。
  ![App 安裝](images/device-portal/iot-installapp0.png)
2.  在裝置入口網站的應用程式管理員] 區段中，按一下 [**新增**] 並選取**安裝從本機儲存的應用程式套件**。
3.  按一下 [**瀏覽**並尋找應用程式套件。
3.  按一下 [**瀏覽**並尋找憑證 (_.cer_) 檔案 （不需要的所有裝置）。
4.  如果您想要安裝選用的個別方塊或 framework 套件以及應用程式安裝檢查。 如果有一個以上的相依性，請個別新增每個相依性。     
5.  按 [**下一步**將移至下一個步驟並**安裝**以啟動安裝。 

#### <a name="uninstall-an-app"></a>解除安裝 App
1.  請確定您的應用程式不在執行中。 
2.  如果是，移至**執行應用程式**並將它關閉。 如果您嘗試解除安裝應用程式執行時，它會造成問題當您嘗試重新安裝應用程式。 
3.  從下拉式清單中選取應用程式並按一下 [**移除**]。

### <a name="running-processes"></a>執行程序

此頁面會顯示關於目前在主機裝置上執行的程序的詳細資訊。 這包括 App 與系統處理程序。 您可以在某些平台 （桌面、 IoT、 和 HoloLens） 終止程序。

![裝置入口網站執行程序頁面](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>檔案總管

此頁面可讓您檢視及操作任何 sideloaded 應用程式所儲存的檔案。 請參閱[使用應用程式檔案總管](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/)部落格文章以深入了解檔案總管]，以及如何使用它。 

![裝置入口網站檔案總管] 頁面上](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>效能

[效能] 頁面上會顯示類似 power 流量、 播放速率系統診斷資訊的即時圖表和 CPU 負載。

下列為可用的衡量標準：
- **CPU**： 的總可用的 CPU 使用率百分比
- **記憶體**： 總在使用中，提供、 認可、 分頁、 及非分頁
- **I/O**： 讀取與寫入資料之訂單數量
- **網路**： 收到和送出資料
- **GPU**: %的總可用 GPU 引擎使用率


![裝置入口網站效能頁面](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>事件追蹤 for Windows (ETW) 記錄

ETW 記錄] 頁面上管理裝置上的即時事件追蹤 for Windows (ETW) 資訊。

![裝置入口網站 ETW 記錄頁面](images/device-portal/mob-device-portal-etw.png)

選取 **\[隱藏提供者\]** 以只顯示 [事件] 清單。
- **Registered 提供者**： 選取事件提供者與追蹤層級。 追蹤層級是下列值之一：
  1. 異常結束或終止
  2. 嚴重錯誤
  3. 警告
  4. 非錯誤警告
  5. 詳細的追蹤

  按一下或點選 **\[啟用\]** 以開始追蹤。 提供者已新增到 **\[啟用的提供者\]** 下拉式清單中。
- **自訂提供者**︰選取自訂 ETW 提供者與追蹤等級。 依 GUID 識別提供者。 不包括括號 GUID。
- **已啟用提供者**： 這會列出已啟用的提供者。 從下拉式清單選取提供者，然後按一下或點選 **\[停用\]** 以停止追蹤。 按一下或點選 **\[全部停止\]** 以暫停所有追蹤。
- **提供者歷程記錄**： 這會顯示已啟用目前工作階段期間的 ETW 提供者。 按一下或點選 **\[啟用\]** 以啟用已停用的提供者。 按一下或點選 **\[清除\]** 以清除歷程記錄。
- **篩選 / 事件**：**事件**章節將列出從選取的提供者以表格格式 ETW 事件。 即時更新表格。 使用 [**篩選**] 功能表中將其顯示事件的自訂篩選設定。 按一下 [**清除**] 按鈕以從表格刪除所有 ETW 事件。 這不會停用任何提供者。 您可以按一下 [**儲存至檔案**到本機的 CSV 檔案匯出目前收集的 ETW 事件。

如需使用 ETW 記錄的詳細資訊，請參閱[使用裝置入口網站檢視偵錯記錄檔](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/)部落格文章。 

### <a name="performance-tracing"></a>效能追蹤

效能追蹤] 頁面可讓您檢視從主機裝置的[Windows 效能錄製 (WPR)](https://msdn.microsoft.com/library/hh448205.aspx)追蹤。

![裝置入口網站效能追蹤] 頁面](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用的設定檔**︰從下拉式清單選取 WPR 設定檔，然後按一下或點選 **\[開始\]** 以開始追蹤。
- **自訂設定檔**︰按一下或點選 **\[瀏覽\]**，以從您的電腦選擇 WPR 設定檔。 按一下或點選 **\[上傳並開始\]** 以開始追蹤。

若要停止追蹤，請按一下 **\[停止\]**。 直到追蹤檔案留在此頁面 (。ETL) 已完成下載。

擷取。可開啟[Windows 效能 Analyzer](https://msdn.microsoft.com/library/windows/desktop/hh448170.aspx)分析的 ETL 檔案。

### <a name="device-manager"></a>裝置管理員

[裝置管理員] 頁面會列舉所有周邊附加至您的裝置。 您可以按一下檢視的每個屬性的設定圖示。

![裝置入口網站裝置管理員] 頁面上](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>網路

[網路] 頁面上管理裝置上的網路連線。 除非您已連線至透過 USB 裝置入口網站、 變更這些設定將可能中斷您裝置入口網站。
- **可用的網路**： 顯示可用的裝置的 WiFi 網路。 按一下或點選網路可讓您與它連線，而且您必須視需要提供密碼。 裝置入口網站還不支援企業驗證。 您也可以使用 [**設定檔**] 下拉式清單來嘗試連線至任何裝置已知的 WiFi 設定檔。
- **IP 設定**： 裝置的網路連接埠會顯示每個主機位址資訊。

![裝置網路入口網站頁面](images/device-portal/mob-device-portal-network.png)

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
> 此保護防止使用方式的 REST Api 從獨立用戶端 （例如命令列公用程式）。 這可以 3 種方式解決： 
> - 使用 「 自動-"使用者名稱。 使用者名稱前面加上 "auto-" 的用戶端將略過 CSRF 保護。 請務必注意，不可以透過瀏覽器使用此使用者名稱來登入 Device Portal，因為它會將服務開放給 CSRF 攻擊。 範例︰如果 Device Portal 的使用者名稱為 "admin"，```curl -u auto-admin:password <args>``` 應該用來略過 CSRF 保護。 
> - 在用戶端中實作 cookie-to-header 配置。 這需要 GET 要求建立工作階段 Cookie，然後在所有後續要求同時包含標頭與 Cookie。 
> - 停用驗證，並使用 HTTP。 CSRF 保護僅適用於 HTTPS 端點，因此 HTTP 端點上的連線不需要執行上述各項。 

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>跨網站 WebSocket 攔截 (CSWSH) 保護

若要防止 [CSWSH 攻擊](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html)，開啟 WebSocket 連接到裝置入口網站的所有用戶端，必須也提供符合 Host 標頭的 Origin 標頭。 這可向裝置入口網站證明要求是來自裝置入口網站 UI 或有效的用戶端應用程式。 沒有 Origin 標頭您的要求將會被拒絕。 
