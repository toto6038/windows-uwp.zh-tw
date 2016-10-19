---
author: GrantMeStrength
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: "啟用您的裝置以用於開發"
description: "設定您的 Windows 10 裝置以進行開發和偵錯。"
keywords: "啟用裝置"
translationtype: Human Translation
ms.sourcegitcommit: 6e8849b2ed067206ab14c4339f74c5219dcca16b
ms.openlocfilehash: 66413b43e5b9fd285324fd139fe14527dbee940a

---
# 啟用您的裝置以用於開發

您必須先在您的開發電腦及任何您將測試程式碼的裝置上啟用 [開發人員模式]，才能撰寫 App。

## 使用開發人員功能

### 使用 Microsoft Visual Studio 開發您的 App

您必須先在您的電腦上啟用 [開發人員模式]，才能在 Visual Studio 中開啟 UWP app 專案。 如果您開啟 UWP 專案，但未啟用 [開發人員模式]，系統將會自動開啟 [開發人員專用]**** 設定頁面。 請依照下一節中的指示來啟用 [開發人員模式]。

當您在 Windows 10 版本 1511 上的 Visual Studio 中開啟 UWP app 專案時，您會在 Visual Studio 中看到以下對話方塊。 

![Visual Studio 中顯示的 [啟用開發人員模式] 對話方塊](images/latestenabledialog.png)

當您看到這個對話方塊時，請按一下 [開發人員設定]**** 來開啟 [開發人員專用]**** 設定頁面，然後啟用 [開發人員模式]。

> 您可以隨時移至 [開發人員專用]**** 頁面來啟用或停用 [開發人員模式]：只要在工作列中的 Cortana 搜尋方塊中輸入「開發人員設定」即可。

### 啟用您的 Windows 10 裝置

您可以啟用裝置來進行開發，或直接用於側載。

-   「側載」**是安裝並執行或測試未經 Windows 市集認證的 app。 例如，僅供公司內部使用的 app。
-   「開發人員模式」**可讓您側載應用程式，也可從 Visual Studio 以偵錯模式執行應用程式。 

    當您啟用 [開發人員模式] 時，系統會安裝一套選項，包括︰
    - 安裝 Windows Device Portal。 只有在已開啟 [啟用 Device Portal]**** 選項時，才會啟用 Device Portal 並為其設定防火牆規則。
    - 為 SSH 服務安裝、啟用及設定允許遠端安裝 App 的防火牆規則。
    - (僅限傳統型裝置) 允許啟用適用於 Linux 的 Windows 子系統。 如需詳細資訊，請參閱[關於 Windows 上 Ubuntu 上的 Bash](https://msdn.microsoft.com/commandline/wsl/about)

如需更多有關選項的詳細資訊，請參閱[應該選擇哪一個設定：側載 app 或開發人員模式？](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#which-settings-should-i-choose-sideload-apps-or-developer-mode)

**使用開發人員功能**

1.  在您要啟用的裝置上，移至 [設定]****。 選擇 [更新與安全性]****，然後選擇 [開發人員專用]****。
2.  選擇您需要的存取層級 - 若要開發 UWP app，請選擇 [開發人員模式]****。 
3.  閱讀您所選設定的免責聲明，然後按一下 [是]**** 來接受變更。

> [!NOTE]
> 如果您的裝置是由組織所擁有，您組織可能會停用某些選項，如以下所示。

以下是在傳統型裝置系列上的 [設定] 頁面。

![請移至 [設定] 並選擇 [更新與安全性]，然後選擇 [適用於開發人員] 以檢視您的選項](images/devmode-pc-options.png)

以下是在行動裝置系列上的 [設定] 頁面。

![從手機上的 [設定]，選擇 [更新與安全性]](images/devmode-mob.png)

## [開發人員模式] 功能

每個裝置系列可能都會有額外的開發人員功能。 只有已在裝置上啟用 [開發人員模式] 時，才可使用這些功能，而且可能依 OS 版本而不同。

此圖顯示 Windows 10 版本 1511 上行動裝置系列的開發人員功能。

![適用於行動裝置的開發人員模式選項](images/devmode-mob-options.png) 

### <span id="device-discovery-and-pairing"></span>Device Portal

若要深入了解裝置探索和 Device Portal，請參閱 [Windows Device Portal 概觀](../debug-test-perf/device-portal.md)。

如需裝置特定的安裝指示，請參閱︰
- [傳統型裝置的 Device Portal](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [IoT 的 Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [行動裝置的 Device Portal](../debug-test-perf/device-portal-mobile.md)
- [Xbox 的 Device Portal](../debug-test-perf/device-portal-xbox.md)

如果您在啟用 [開發人員模式] 或 Device Portal 時遇到問題，請參閱[已知問題](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)論壇來尋找這些問題的因應措施。 

###SSH

當您在裝置上啟用 [開發人員模式] 時，系統會啟用 SSH 服務。  當您的裝置是 UWP 應用程式的部署目標時，就會用到此服務。   服務的名稱是 'SSH Server Broker' 和 'SSH Server Proxy'。

> [!NOTE]
> 這不是 Microsoft 的 OpenSSH 實作 (可在 [GitHub](https://github.com/PowerShell/Win32-OpenSSH) 中找到)。

為了利用 SSH 服務，您可以啟用裝置探索來允許 PIN 配對。 如果您想要執行另一個 SSH 服務，您可以在不同的連接埠上進行設定，或是關閉 [開發人員模式] SSH 服務。 若要關閉 SSH 服務，只要停用 [開發人員模式] 即可。  

### 裝置探索

當您啟用裝置探索時，會讓網路上的其他裝置能夠透過 mDNS 看見您的裝置。  這個功能也可讓您取得可與此裝置配對的 SSH PIN。  

![PIN 配對](images/devmode-pc-pinpair.PNG)

您應該只有在您想要讓裝置成為部署目標時，才啟用裝置探索。 例如，如果您使用 Device Portal 將 App 部署到手機來進行測試，您就必須在該手機上啟用裝置探索，而不是在您的開發電腦上啟用。

### 錯誤報告 (僅限行動裝置)

設定此值以指定在手機上儲存多少損毀傾印。

收集手機上的損毀傾印，讓您能夠在發生當機之後，即時直接存取重要的毀損資訊。 系統只會針對開發人員簽署的 app 收集傾印。 您可以在手機儲存體的 Documents\Debug 資料夾中找到傾印。 如需有關傾印檔案的詳細資訊，請參閱[使用傾印檔案](https://msdn.microsoft.com/library/d5zhxt22.aspx)。

### Windows Explorer、遠端桌面及 PowerShell 的最佳化 (僅限傳統型裝置)

 在傳統型裝置系列上，[開發人員專用]**** 設定頁面含有您可用來將電腦最佳化以進行開發工作的設定捷徑。 針對每個設定，您都可以選取核取方塊並按一下 [套用]****，或按一下 [顯示設定]**** 連結來開啟該選項的設定頁面。 

## 應該選擇哪一個設定：側載應用程式或開發人員模式？

根據預設，您只能從 Windows 市集安裝通用 Windows 平台 (UWP) app。 變更這些設定來使用開發人員功能，可以變更裝置的安全性層級。 您不應該從未驗證的來源安裝 app。

### 側載 app

通常是需要在受管理裝置上安裝自訂應用程式而不透過「Windows 市集」的公司或學校，才會使用 [側載應用程式] 設定。 在此案例中，組織強制執行會停用 [Windows 市集應用程式]** 設定的原則相當常見，如先前 [設定] 頁面的圖中所示。 組織也會提供側載應用程式所需的憑證和安裝位置。 如需詳細資訊，請參閱 TechNet 文章[在 Windows 10 中側載 App](https://technet.microsoft.com/library/mt269549.aspx) 和[在 Microsoft Intune 中開始使用 App 部署](https://technet.microsoft.com/library/dn646955.aspx)。

裝置系列特定的資訊

-   在傳統型裝置系列上：您可以執行使用套件 ("Add-AppDevPackage.ps1") 建立的 Windows PowerShell 指令碼，來安裝應用程式套件 (.appx) 以及執行該應用程式所需的任何憑證。 如需詳細資訊，請參閱[封裝 UWP app](../packaging/packaging-uwp-apps.md)。

-   在行動裝置系列上：如果已經安裝所需的憑證，您便可以點選檔案來安裝任何透過電子郵件傳送給您或位於 SD 記憶卡上的 .appx。

因為您無法在不具受信任憑證的裝置上安裝應用程式，所以 [側載應用程式]**** 會是比 [開發人員模式] 更安全的選項。

> [!NOTE]
> 如果您要側載應用程式，您應該仍然只安裝來自受信任來源的應用程式。 當您安裝未經「Windows 市集」認證的側載應用程式時，即表示您同意您已具備側載該應用程式所需的一切權限，並且為安裝和執行該應用程式所造成的任何損害負全責。 請參閱這份[隱私權聲明](http://go.microsoft.com/fwlink/?LinkId=521839)的 Windows &gt; ＜Windows 市集＞小節。

### 開發人員模式

[開發人員模式] 取代了 Windows 8.1 對開發人員授權的需求。  除了側載功能，[開發人員模式] 設定還會啟用偵錯和其他部署選項。 這包括啟動 SSH 服務來允許此裝置成為部署目標。 若要停用此服務，您必須停用 [開發人員模式]。

裝置系列特定的資訊

-   在傳統型裝置系列上：

    啟用 [開發人員模式] 以在 Visual Studio 中進行 App 開發和偵錯。 如先前所述，若未啟用 [開發人員模式]，系統將會在 Visual Studio 中提示您。

    允許啟用適用於 Linux 的 Windows 子系統。 如需詳細資訊，請參閱[關於 Windows 上 Ubuntu 上的 Bash](https://msdn.microsoft.com/commandline/wsl/about)

-   在行動裝置系列上：

    啟用開發人員模式，從 Visual Studio 部署 app 並在裝置上進行偵錯。

    您可以點選檔案來安裝透過電子郵件傳送給您或 SD 記憶卡上的任何 .appx。 請勿從未驗證的來源安裝應用程式。

**提示**  
有數個工具可讓您用來將應用程式從 Windows 10 電腦部署到 Windows 10 行動裝置。 這兩個裝置都必須透過有線或無線連線連接到網路的同一個子網路，或者必須透過 USB 來連接它們。 列出的任一個方法只會安裝 app 套件 (.appx)。它們不會安裝憑證。

-   使用 Windows 10 應用程式部署 (WinAppDeployCmd) 工具。 深入了解 [WinAppDeployCmd 工具](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx)。
-   從 Windows 10 版本 1511 開始，您可以使用 [Device Portal](#device_portal)，從您的瀏覽器部署到執行 Windows 10 版本 1511 或更新版本的行動裝置。 使用 Device Portal 中的 **[App](../debug-test-perf/device-portal.md#apps)** 頁面來上傳應用程式套件 (.appx)，並將它安裝在裝置上。

## 使用群組原則或登錄機碼來啟用裝置

對大多數開發人員來說，您會想要使用 [設定] App 來啟用您的裝置以進行偵錯。 在某些案例 (例如自動化測試) 中，您可以使用其他方法來啟用您的 Windows 10 傳統型裝置以進行開發。

除非您擁有的是「Windows 10 家用版」，否則請使用 gpedit.msc 來設定群組原則以啟用您的裝置。 如果您確實擁有「Windows 10 家用版」，則需要使用 regedit 或 PowerShell 命令來直接設定登錄機碼，以啟用您的裝置。

**使用 gpedit 啟用您的裝置**

1.  執行 **Gpedit.msc**。
2.  移至 [本機電腦原則] &gt; [電腦設定] &gt; [系統管理範本] &gt; [Windows 元件] &gt; [應用程式套件部署]
3.  若要啟用側載功能，請編輯原則來啟用：

    -   **允許安裝所有受信任的 app**

    - 或 -

    若要啟用開發人員模式，請編輯原則來啟用這兩者：

    -   **允許安裝所有受信任的 app**
    -   **允許開發 Windows 市集 app，並從整合式開發環境 (IDE) 安裝它們。**

4.  將電腦重新開機。

**使用 regedit 啟用您的裝置**

1.  執行 **regedit**。
2.  若要啟用側載功能，請將此 DWORD 的值設定為 1：

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - 或 -

    若要啟用開發人員模式，請將此 DWORD 的值設定為 1：

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**使用 PowerShell 啟用您的裝置**

1.  使用系統管理員權限執行 PowerShell。
2.  若要啟用側載功能，請執行下列命令：

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - 或 -

    若要啟用開發人員模式，請執行下列命令：

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## 將裝置從 Windows 8.1 升級至 Windows 10

在 Windows 8.1 裝置上建立或側載應用程式時，您必須安裝開發人員授權。 如果您將裝置從 Windows 8.1 升級為 Windows 10，則會保留此資訊。 執行下列命令，從升級後的 Windows 10 裝置移除此資訊。 如果您直接從 Windows 8.1 升級為 Windows 10 版本 1511 或更新版本，就不需要執行此步驟。

**取消註冊開發人員授權**

1.  使用系統管理員權限執行 PowerShell。
2.  執行此命令：**unregister-windowsdeveloperlicense**。

在這之後，您需要啟用裝置來進行開發 (如本主題所述)，讓您能夠繼續在此裝置上進行開發。 如果您沒有這樣做，在偵錯您的 app 或嘗試為它建立套件時可能會收到錯誤。 以下是此錯誤的範例：

錯誤：DEP0700：app 的註冊失敗。



<!--HONumber=Aug16_HO5-->


