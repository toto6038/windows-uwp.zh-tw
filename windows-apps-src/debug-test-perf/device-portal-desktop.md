---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Windows 桌面的裝置入口網站
description: 了解 Windows Device Portal 如何在 Windows 桌面上開啟診斷與自動化功能。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10 uwp，裝置入口網站
ms.localizationpriority: medium
ms.openlocfilehash: 00cf497d5d57f5a3cdc5c52ecfeead7885ff7d56
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713804"
---
# <a name="device-portal-for-windows-desktop"></a>Windows 桌面的裝置入口網站

Windows 裝置入口網站可讓您檢視診斷資訊，並透過 HTTP 從瀏覽器視窗與桌面互動。 您可以使用 Device Portal 來執行下列動作：
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

* Localhost:`http://127.0.0.1:<PORT>`或 `http://localhost:<PORT>`
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
- Location
- 臨時

## <a name="more-device-portal-options"></a>更多裝置入口網站選項

### <a name="registry-based-configuration-for-device-portal"></a>裝置入口網站的登錄為主設定

若您想要選取 Device Portal 的連接埠號碼 (例如 80 和 443)，則可設定下列登錄機碼︰

- 在 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`:必要的 DWORD。 將此設為 0，以保留選擇的連接埠號碼。
    - `HttpPort`:必要的 DWORD。 包含 Device Portal 針對 HTTP 連線開啟接聽的連接埠號碼。    
    - `HttpsPort`:必要的 DWORD。 包含 Device Portal 用來接聽 HTTPS 連線的連接埠號碼。
    
在相同的 regkey 路徑底下，您也可以關閉驗證需求：
- `UseDefaultAuthorizer` - `0` 為停用，`1`啟用。  
    - 這可控制每個連線以及從 HTTP 重新導向至 HTTPS 的基本驗證要求。  
    
### <a name="command-line-options-for-device-portal"></a>裝置入口網站的命令列選項
從系統管理命令提示字元中，您可以啟用及設定裝置入口網站的幾個部分。 若要查看最新的組建上支援的命令集，您可以執行 `webmanagement /?`

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

## <a name="common-errors-and-issues"></a>常見的錯誤和問題

以下是設定裝置入口網站時，可能會遇到的一些常見錯誤。

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbseinvalidwindowsupdatecount"></a>WindowsUpdateSearch 傳回無效的更新數目 (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

嘗試發行前組建的 Windows 10 上安裝開發人員套件時，可能會發生此錯誤。 這些功能 on Demand (FoD) 套件裝載於 Windows Update，並在發行前版本組建上下載它們需要您選擇加入試驗。 若您的安裝不選擇正確的組建和信號組合試驗，將無法下載承載。 再次檢查下列項目：

1. 瀏覽至**設定 > 更新與安全性 > Windows 測試人員計畫**並確認**Windows 測試人員帳戶**區段具有正確的帳戶資訊。 如果您沒有看到該區段，選取**連結 Windows 測試人員帳戶**中，新增您的電子郵件帳戶，並確認它在底下會出現**Windows 測試人員帳戶**標題 (您可能需要選取**Windows 測試人員帳戶連結**實際連結時間第二個新增的帳戶)。
 
2. 底下**您要接收何種內容？** ，請確定**進行開發的 Windows**已選取。
 
3. 底下**您想要取得新的組建步調為何？** ，請確定**快速 Windows Insider**已選取。
 
4. 您現在應該能夠安裝 FoDs。 如果您已確認，在 快速 Windows 測試人員和仍然無法安裝 FoDs、 請提供意見反應和附加記錄檔底下**C:\Windows\Logs\CBS**。

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC]StartService:OpenService 1060 失敗：指定的服務並不是已安裝的服務

如果未安裝開發人員套件，您可能會發生此錯誤。 而不需要開發人員套件中，沒有任何 web 管理服務。 嘗試再次安裝開發人員套件。

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbsemeterednetwork"></a>CBS 無法啟動下載，因為系統已在計量付費網路 (CBS_E_METERED_NETWORK)

如果您是在計量付費網際網路連線，您可能會發生此錯誤。 您將無法下載付費連線上的開發人員套件。

## <a name="see-also"></a>另請參閱

* [Windows Device Portal 概觀](device-portal.md)
* [裝置入口網站 core API 參考](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
