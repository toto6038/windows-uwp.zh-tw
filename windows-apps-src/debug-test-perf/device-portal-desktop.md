---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: "傳統型裝置的 Device Portal"
description: "了解 Windows Device Portal 如何在 Windows 桌面上開啟診斷與自動化功能。"
translationtype: Human Translation
ms.sourcegitcommit: b5d259172a7e3975d48a5ba669cfbe345869aebf
ms.openlocfilehash: 3436a95124071045c8ec89ed8ddf644ccc80c29f

---
# 傳統型裝置的 Device Portal

從 Windows 10 版本 1607 開始，即針對傳統型裝置提供額外的開發人員功能。 只有在啟用開發人員模式時，才能使用這些功能。

如需有關如何啟用開發人員模式的資訊，請參閱[啟用您的裝置以用於開發](../get-started/enable-your-device-for-development.md)。

Device Portal 可讓您檢視診斷資訊，並透過 HTTP 從您的瀏覽器與桌面互動。 您可以使用 Device Portal 來執行下列動作：
- 請參閱和操作執行中處理程序清單
- 安裝、刪除、啟動和終止 app
- 變更 Wi-Fi 設定檔、檢視訊號強度和查看 ipconfig
- 檢視 CPU、記憶體、I/O、網路及 GPU 使用量的動態圖形
- 收集處理程序傾印
- 收集 ETW 追蹤 
- 操作側載應用程式隔離的儲存裝置

## 在 Windows 桌面上設定 Device Portal

### 開啟 Device Portal

在 [開發人員設定]**** 功能表中 (啟用 [開發人員模式])，您可啟用 Device Portal。  

啟用 Device Portal 時，您也必須為 Device Portal 建立使用者名稱與密碼。 請勿使用您的 Microsoft 帳戶或其他 Windows 認證。  

啟用 Device Portal 之後，您會在 [設定]**** 區段的底端看見其連結。 記下套用至 URL 結尾的連接埠號碼：啟用 Device Portal 時會隨機產生此連接埠號碼，但其應與桌面重新開機時保持一致。 若您想要手動設定永久連接埠號碼，請參閱[設定連接埠號碼](device-portal-desktop.md#setting-port-numbers)。

您可以從兩種連線至 Device Portal 的方式中進行選擇：本機主機以及透過區域網路 (包括 VPN)。

**連接到 Device Portal**

1. 在瀏覽器中，針對您使用的連接類型輸入位址 (如下所示)。

    - 本機主機：`http://127.0.0.1:PORT` 或 `http://localhost:PORT`

    使用此位址在本機檢視 Device Portal。
    
    - 區域網路： `https://<The IP address of the desktop>:PORT`

    使用這個位址來透過區域網路連線。

HTTPS 需要進行驗證和安全通訊。

如果您是在受保護的環境中使用 Device Portal (例如測試實驗室)，您可以在此環境中信任區域網路上的每個人、裝置上沒有個人資訊，而且有獨特的需求，則您可以停用驗證。 這會啟用未加密的通訊，並允許具有您電腦 IP 位址的任何人對其進行控制。

## Device Portal 頁面

桌面上的 Device Portal 提供標準頁面集。 如需詳細描述，請參閱 [Windows Device Portal 概觀](device-portal.md)。

- App
- 處理程序
- 效能
- 偵錯
- Windows 事件追蹤 (ETW)
- 效能追蹤
- 裝置
- 網路功能
- App 檔案總管 

## 設定連接埠號碼

若您想要選取 Device Portal 的連接埠號碼 (例如 80 和 443)，則可設定下列登錄機碼︰

- 在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service 下
    - UseDynamicPorts：所需的 DWORD。 將此設為 0，以保留選擇的連接埠號碼。
    - HttpPort：所需的 DWORD。 包含 Device Portal 針對 HTTP 連線開啟接聽的連接埠號碼。  
    - HttpsPort：所需的 DWORD。 包含 Device Portal 用來接聽 HTTPS 連線的連接埠號碼。

## 無法安裝開發人員模式套件或啟動 Device Portal
有時會因網路或相容性問題，致使「開發人員模式」無法正確安裝。 「遠端」****部署 (Device Portal 和 SSH) 必須使用「開發人員模式」套件，但本機開發則不需要。  即使您遇到這些問題，您仍可使用 Visual Studio 來部署您的 App。 

若要尋找這些問題及其他問題的因應措施，請參閱[已知問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)。 

### 找不到套件

「在 Windows Update 中找不到開發人員模式套件。 錯誤程式碼 0x001234 深入了解」   

發生此錯誤的原因，可能在於網路連線問題、企業設定或是套件已遺失。 

解決此問題：

1. 確認您的電腦是否已連線至網際網路。 
2. 若您位於加入網域的電腦上，請說出您的網路系統管理員。 
3. 在 [設定] &gt; [更新與安全性] &gt; [Windows Updates](ms-settings:windowsupdate) 中，檢查 Windows 更新。
4. 確認 Windows 開發人員模式套件顯示於 [設定] &gt; [系統] &gt; [應用程式與功能] &gt; [管理選用功能](ms-settings:optionalfeatures) &gt; [新增功能]。 若該套件遺失，Windows 即無法找到適用於您電腦的正確套件。 

執行上述步驟之後，停用和重新啟用開發人員模式確認已修正問題。 


### 無法安裝套件

「無法安裝開發人員模式套件。 錯誤程式碼 0x001234 深入了解」

由於您的 Windows 組建與開發人員模式套件不相容，因此發生此錯誤。 

解決此問題：

1. 在 [設定] &gt; [更新與安全性] &gt; [Windows Updates](ms-settings:windowsupdate) 中，檢查 Windows 更新。
2. 重新啟動電腦，以確保套用所有的更新。



<!--HONumber=Aug16_HO5-->


