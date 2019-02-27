---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows 桌面的裝置入口網站
description: 了解 Windows 裝置入口網站如何在 Windows 桌面上開啟診斷與自動化功能。
ms.date: 02/6/2019
ms.topic: article
keywords: windows 10，uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 4fe1f2a51199dd12cd1d285c17c5d48c9a25b969
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117798"
---
# <a name="device-portal-for-windows-desktop"></a>Windows 桌面的裝置入口網站

Windows 裝置入口網站可讓您檢視診斷資訊，並透過 HTTP 從瀏覽器視窗與桌面互動。 您可以使用裝置入口網站來執行下列動作：
- 請參閱和操作執行中處理程序清單
- 安裝、刪除、啟動和終止 app
- 變更 Wi-Fi 設定檔、檢視訊號強度和查看 ipconfig
- 檢視 CPU、記憶體、I/O、網路及 GPU 使用量的動態圖形
- 收集處理程序傾印
- 收集 ETW 追蹤 
- 操作側載應用程式隔離的儲存裝置

## <a name="set-up-device-portal-on-windows-desktop"></a>在 Windows 桌面上設定裝置入口網站

### <a name="turn-on-developer-mode"></a>開啟開發人員模式

從 Windows 10 版本 1607 開始，某些適用於桌上型電腦的新功能只有在啟用開發人員模式時才提供。 如需有關如何啟用開發人員模式的資訊，請參閱[啟用您的裝置以用於開發](../get-started/enable-your-device-for-development.md)。

> [!IMPORTANT]
> 有時會因網路或相容性問題，致使開發人員模式無法正確安裝在您的裝置上。 如需協助進行這些問題的疑難排解，請參閱[相關的＜啟用您的裝置以用於開發＞一節](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package)。

### <a name="turn-on-device-portal"></a>開啟裝置入口網站

您可以在 **\[設定\]** 的 **\[適用於開發人員\]** 區段中啟用裝置入口網站。 啟用時，您也必須建立對應的使用者名稱與密碼。 請勿使用您的 Microsoft 帳戶或其他 Windows 認證。 

![設定 app 的裝置入口網站區段](images/device-portal/device-portal-desk-settings.png) 

一旦啟用裝置入口網站，您會在區段的底端看見 Web 連結。 記下附加至所列 URL 結尾的連接埠號碼：啟用裝置入口網站時會隨機產生此號碼，但其應與桌面重新開機時保持一致。 

這些連結提供兩種連線至裝置入口網站的方式：透過區域網路 (包括 VPN) 或透過本機主機。

### <a name="connect-to-device-portal"></a>連接到裝置入口網站

若要透過本機主機連接，開啟瀏覽器視窗並輸入此處所顯示您要用於連線的網址。

* 本機主機：`http://127.0.0.1:<PORT>` 或 `http://localhost:<PORT>`
* 區域網路： `https://<IP address of the desktop>:<PORT>`

HTTPS 需要進行驗證和安全通訊。

如果您是在受保護的環境中使用裝置入口網站 (例如測試實驗室)，您可以在此環境中信任區域網路上的每個人、裝置上沒有個人資訊，而且有獨特的需求，則您可以停用 \[驗證\] 選項。 這會啟用未加密的通訊，並允許具有您電腦 IP 位址的任何人進行連線並對其進行控制。

## <a name="device-portal-content-on-windows-desktop"></a>Windows 桌面上的裝置入口網站內容

Windows 桌面上的裝置入口網站提供標準頁面集。 如需這些項目的詳細描述，請參閱 [Windows 裝置入口網站概觀](device-portal.md)。

- 應用程式管理員
- 檔案總管
- 執行中處理程序
- 效能
- 偵錯
- Windows 事件追蹤 (ETW)
- 效能追蹤
- 裝置管理員
- 網路功能
- 當機資料
- 功能
- 混合實境
- 串流安裝偵錯工具
- 位置
- 臨時

## <a name="more-device-portal-options"></a>更多裝置入口網站選項

### <a name="registry-based-configuration-for-device-portal"></a>裝置入口網站的登錄為主設定

若您想要選取裝置入口網站的連接埠號碼 (例如 80 和 443)，則可設定下列登錄機碼︰

- 在 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`：所需的 DWORD。 將此設為 0，以保留選擇的連接埠號碼。
    - `HttpPort`：所需的 DWORD。 包含裝置入口網站針對 HTTP 連線開啟接聽的連接埠號碼。    
    - `HttpsPort`：所需的 DWORD。 包含裝置入口網站用來接聽 HTTPS 連線的連接埠號碼。
    
在相同的 regkey 路徑底下，您也可以關閉驗證需求：
- `UseDefaultAuthorizer` - `0` 代表停用；`1` 代表啟用。  
    - 這可控制每個連線以及從 HTTP 重新導向至 HTTPS 的基本驗證要求。  
    
### <a name="command-line-options-for-device-portal"></a>裝置入口網站的命令列選項
從系統管理命令提示字元中，您可以啟用及設定裝置入口網站的幾個部分。 若要查看組建上支援的最新命令集，您可以執行 `webmanagement /?`

- `sc start webmanagement` 或 `sc stop webmanagement` 
    - 開啟或關閉服務。 這仍需要啟用開發人員模式。 
- `-Credentials <username> <password>` 
    - 設定裝置入口網站的使用者名稱和密碼。 使用者名稱必須符合基本驗證標準，因此不能包含冒號 (:) 且應使用標準 ASCII 字元建置，例如 [a-zA-Z0-9]，因為瀏覽器無法以標準方式剖析完整字元集。  
- `-DeleteSSL` 
    - 這樣會重設用於 HTTPS 連線的 SSL 憑證快取。 如果您遇到 TLS 連線錯誤，而無法略過 (與預期的憑證警告不同)，此選項可修正問題。 
- `-SetCert <pfxPath> <pfxPassword>`
    - 如需詳細資料，請參閱[使用自訂 SSL 憑證佈建裝置入口網站](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl)。  
    - 這可讓您安裝自己的 SSL 憑證，以修正通常在裝置入口網站中看見的 SSL 警告頁面。 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - 執行具有特定組態並且會顯示偵錯訊息的單機版裝置入口網站。 這對於建置[封裝外掛程式](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin)來說最實用。 
    - 如需如何以 System 身分執行此操作以完整測試封裝外掛程式的詳細資料，請參閱 [MSDN Magazine 文章](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx)。

## <a name="common-errors-and-issues"></a>常見的錯誤以及問題

以下是一些設定 Device Portal 時，您可能會遇到的常見錯誤。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbseinvalidwindowsupdatecount"></a>WindowsUpdateSearch 傳回不正確的更新數目 (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

嘗試安裝開發人員套件上的 Windows 10 發行前組建時，您可能會發生這個錯誤。 這些功能隨選 (FoD) 套件會裝載於 Windows Update，並下載發行前組建會要求您選擇到正式發行前小眾。 如果您的安裝不選擇加入的正確的組建和更新步調組合正式發行前小眾，將無法下載承載。 仔細檢查下列項目：

1. 瀏覽至 [**設定 > 更新 & 安全性 > Windows 測試人員計畫**，並確認**Windows 測試人員帳戶**\] 區段都有您正確的帳戶資訊。 如果您沒有看到該區段中，選取 [ **Windows 測試人員帳戶連結**，新增您的電子郵件帳戶，並確認它 （您可能需要選取**Windows 測試人員帳戶連結**至第二次**Windows 測試人員帳戶**標題下方顯示實際連結新增的帳戶）。
 
2. 下方**何種內容您想要收到？**，請確定已選取**的 Windows 開發**。
 
3. 下方**您想要取得新組建哪些步調？**，請確定已選取 [ **Windows 測試人員-快**。
 
4. 您現在應該安裝 Fod。 如果您已經確認您是在 Windows 測試人員快速，並且仍然無法安裝 Fod，請提供意見反應並附加**C:\Windows\Logs\CBS**底下的記錄檔。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC]StartService: OpenService 失敗 1060年： 指定的服務並不是已安裝的服務

如果不會安裝開發人員套件，您可能會發生這個錯誤。 不需要開發人員套件中，沒有任何 web 管理服務。 再次嘗試安裝開發人員套件。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbsemeterednetwork"></a>CBS 無法啟動下載，因為系統會在計量付費網路 (CBS_E_METERED_NETWORK)

如果您是在計量付費的網際網路連線，您可能會發生這個錯誤。 您將無法下載開發人員套件上的計量付費連線。

## <a name="see-also"></a>請參閱

* [Windows 裝置入口網站概觀](device-portal.md)
* [裝置入口網站核心 API 參考資料](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
