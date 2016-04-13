---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Windows Device Portal 概觀
description: 了解 Windows Device Portal 如何讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。
---
# Windows Device Portal 概觀

Windows Device Portal 能讓您從遠端透過網路或 USB 連線來設定及管理您的裝置。 它也提供進階診斷工具來協助您疑難排解和檢視您 Windows 裝置的即時效能。 

Device Portal 是您裝置上的網頁伺服器，您可以從電腦上的網頁瀏覽器與它連線。 若您的裝置有網頁瀏覽器，您也可以使用裝置上的瀏覽器與它進行本機連線。

每個裝置系列上都提供 Windows Device Portal，但是功能和設定會依據裝置的需求而各有差異。 此文章提供 Device Portal 的一般描述，也提供有關每個裝置系統更特定資訊的文章連結。

Windows Device Portal 中的所有項目都是以 [REST API](device-portal-api-core.md) (可讓您用來存取資料並以程式設計方式控制裝置) 為基礎所建置。

## 設定

每個裝置都有連線到 Device Portal 的特定指示，但是每個裝置都需要執行這些一般步驟。
1. 在您的裝置上啟用開發人員模式與 Device Portal 。
2. 透過區域網路或 USB 連接裝置與電腦。
3. 在瀏覽器中瀏覽到 Device Portal 頁面。 此表顯示每個裝置系列使用的連接埠與通訊協定。

裝置系列 | 預設開啟？ | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | 是，在開發人員模式 | 80 (預設值) | 443 (預設值) | localhost:10080
IoT | 是，在開發人員模式 | 8080 | 透過登錄機碼啟用 | N/A
Xbox | 在開發人員模式內啟用 | 已停用 | 11443 | N/A
電腦| 在開發人員模式內啟用 | 隨機 > 50,000 (xx080) | 隨機 > 50,000 (xx443) | N/A
手機 | 在開發人員模式內啟用 | 80| 443 | localhost:10080

如需裝置特定的安裝指示，請參閱︰
- [HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [IoT 的 Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [行動裝置的 Device Portal](device-portal-mobile.md#setup)
- [Xbox 的 Device Portal](device-portal-xbox.md)

## 功能

### 工具列與瀏覽

頁面頂端的工具列能讓您存取常用狀態與功能。
- **關機**：關閉裝置。
- **重新啟動**：關閉裝置電源並重新開啟。
- **說明**︰開啟 [說明] 頁面。

使用頁面左側瀏覽窗格中的連結，瀏覽到適用於您裝置的管理與監視工具。

這裡描述各種裝置上常見的工具。 根據不同的裝置，可能提供其他選項。 如需詳細資訊，請參閱您裝置的特定頁面。

### 首頁

您的 Device Portal 工作階段從首頁開始。 首頁頁面通常會有裝置的相關資訊，例如名稱與 OS 版本，也包含可為裝置設定的喜好設定。

### App

提供在裝置上安裝/解除安裝 AppX 套件與套件組合及其管理功能。

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-apps.png)

- **已安裝的 App**︰移除及啟動 App。
- **執行中 App**︰列出目前的執行中 App。
- **安裝 App**︰從電腦或網路上的資料夾，選取安裝的應用程式套件。
- **相依性**︰新增將要安裝之 App 的相依性。
- **部署**︰將所選取的 App 與相依性部署到您的裝置。

**安裝 App**

1.  [建立應用程式套件](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx)之後，您可以從遠端將它安裝到您的裝置上。 在 Visual Studio 中建置後，會產生輸出資料夾。 

    ![App 安裝](images/device-portal/iot-installapp0.png)    
2.  按一下 [瀏覽] 並尋找您的應用程式套件 (.appx)。
3.  按一下 [瀏覽] 並尋找憑證檔案 (.cer) (並非所有裝置都需要)。
4.  新增相依性。 如果有一個以上的相依性，請個別新增每個相依性。     
5.  在 [部署] 下，按一下 [執行]。 
6.  若要安裝另一個 App，請按一下 [重設] 按鈕以清除欄位。


**解除安裝 App**

1.  請確定您的應用程式不在執行中。 
2.  若它正在執行，請移至 [執行中的 App] 並將它關閉。 若您在 App 仍在執行時將它解除安裝，那麼重新安裝 App 時會造成問題。 
3.  準備好之後，請按一下 [解除安裝]。

### 處理程序

顯示有關目前正在執行之處理程序的詳細資訊。 這包括 App 與系統處理程序。

此頁面與電腦上的「工作管理員」類似，可讓您查看哪些處理程序目前在執行中及其記憶體使用量。  在某些平台 (電腦、IoT 與 HoloLens) 上，您可以終止處理程序。 

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-processes.png)

### 效能

顯示系統診斷資訊的即時圖表，例如電源使用量、畫面播放速率與 CPU 負載。

下列為可用的衡量標準：
- **CPU**︰可用總計的百分比
- **記憶體**：總計、使用中、已認可的可用空間、分頁與非分頁
- **GPU**：GPU 引擎使用率、可用總計的百分比
- **I/O**：讀取與寫入
- **網路**︰已接收與已傳送

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-perf.png)

### Windows 事件追蹤 (ETW)

管理裝置上的即時 Windows 事件追蹤 (ETW)

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-etw.png)

選取 [隱藏提供者] 以只顯示 [事件] 清單。
- **已登錄的提供者**︰選取 ETW 提供者與追蹤等級。 追蹤等級是下列其中一個值 ︰
    1. 異常結束或終止
    2. 嚴重錯誤
    3. 警告
    4. 非錯誤警告
    5. 追蹤的詳細資料 (*)

按一下或點選 [啟用] 以開始追蹤。 提供者已新增到 [啟用的提供者] 下拉式清單中。
- **自訂提供者**︰選取自訂 ETW 提供者與追蹤等級。 依 GUID 識別提供者。 不要在 GUID 中包含括號。
- **啟用的提供者**：列出已啟用的提供者。 從下拉式清單選取提供者，然後按一下或點選 [停用] 以停止追蹤。 按一下或點選 [全部停止] 以暫停所有追蹤。
- **提供者歷程記錄**︰顯示目前工作階段期間已啟用的 ETW 提供者。 按一下或點選 [啟用] 以啟用已停用的提供者。 按一下或點選 [清除] 以清除歷程記錄。
- **事件**︰以表格格式列出所選提供者的 ETW 事件。 此表格會即時更新。 在表格下方，按一下 [清除] 按鈕以刪除表格中的所有 ETW 事件。 這不會停用任何提供者。 您可以按一下 [儲存到檔案]，將目前收集的 ETW 事件匯出到本機的 CSV 檔案。

### 效能追蹤

從您的裝置擷取 [Windows Performance Recorder](https://msdn.microsoft.com/library/windows/hardware/hh448205.aspx) (WPR)。

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-perf-tracing.png)

- **可用的設定檔**︰從下拉式清單選取 WPR 設定檔，然後按一下或點選 [開始] 以開始追蹤。
- **自訂設定檔**︰按一下或點選 [瀏覽]，以從您的電腦選擇 WPR 設定檔。 按一下或點選 [上傳並開始] 以開始追蹤。

若要停止追蹤，按一下 [停止] 連結。 留在此頁面上，直到追蹤檔案 (.ETL) 下載完成。

擷取的 ETL 檔案可以在 [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx) 中開啟以進行分析。

### 裝置

列舉與您的裝置連接的所有周邊裝置。

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-devices.png)

### 網路功能

管理裝置上的網路連線。  除非您透過 USB 連線到 Device Portal，否則變更這些設定很可能會中斷您與 Device Portal 的連線。 
- **設定檔**：選取要使用的其他 WiFi 設定檔。  
- **可用的網路**︰可供裝置使用的 WiFi 網路。  按一下或點選網路可讓您與它連線，而且您必須視需要提供密碼。  注意︰Device Portal 還不支援企業驗證。 

![行動裝置的 Device Portal](images/device-portal/mob-device-portal-network.png)



<!--HONumber=Mar16_HO5-->


