---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: 啟用您的裝置以用於開發
description: 針對 Windows 10 裝置的開發已有不同的方法。
關鍵字：入門
關鍵字：開發人員授權
關鍵字：Visual Studio、開發人員授權
關鍵字 ︰啟用裝置
---
# 啟用您的裝置以用於開發

針對 Windows 10 裝置的開發已有不同的方法。 對於您要用來開發、安裝或測試 app 的每個裝置，您不再需要開發人員授權。 您只需從裝置的設定針對這些工作啟用一次即可。 就這麼簡單。 不需要再每隔 30 或 90 天更新您的開發人員授權。

若您仍使用 Windows 8.1 裝置，利用 Microsoft Visual Studio 2013 或 Microsoft Visual Studio 2015 開發或測試 app，則還是需要[取得開發人員授權](https://msdn.microsoft.com/library/windows/apps/Hh974578)或[註冊您的 Windows Phone](https://msdn.microsoft.com/library/windows/apps/Dn614128)。

## 使用開發人員功能

### 使用 Microsoft Visual Studio 開發您的 app

如果您在 Windows 10 裝置上使用 Microsoft Visual Studio，且開啟 Windows 8.1 或 Windows 10 app 的方案，系統會提示您使用此對話方塊來啟用您的裝置。 您需要啟用裝置，才能使用設計工具以及為您的應用程式偵錯。

![Visual Studio 中顯示的 [啟用開發人員模式] 對話方塊](images/latestenabledialog.png)

當您看到這個對話方塊時，按一下 [**開發人員的設定**] 直接前往 [**更新與安全性**] 頁面，如下所示。 或按一下 [確定]****，然後依照下列步驟，啟用您的 Windows 10 裝置來進行開發。

### 啟用您的 Windows 10 裝置

針對 Windows 10，您可以選擇要在裝置上啟用的開發人員功能。 這包括任何裝置：Windows 10 桌上型電腦、平板電腦與手機。 您可以啟用裝置進行開發，或直接側載。

-   「側載」**是安裝並執行或測試未經 Windows 市集認證的 app。 例如，僅供公司內部使用的 app。
-   「開發人員模式」**可讓您側載 app，也可從 Visual Studio 的偵錯模式中執行 app。

**注意**：如果您要側載 app，您仍只應安裝來自受信任來源的 app。 當您安裝未經 Windows 市集認證的側載 app 時，表示您同意您已具備側載這些 app 所需的所有權利，並為安裝和執行 app 造成的任何損害負全責。 請參閱這份[隱私權聲明](http://go.microsoft.com/fwlink/?LinkId=521839)的 Windows &gt; Windows 市集一節。

**使用開發人員功能**

1.  在您要啟用的裝置上，移至 [設定]****。 選擇 [更新與安全性]****，然後選擇 [適用於開發人員]****。
2.  選擇您需要的存取層級。 如需更多有關選項的詳細資訊，請參閱[應該選擇哪一個設定：側載 app 或開發人員模式？](#WhichSettings)
3.  閱讀您選擇的設定免責聲明，然後按一下 [是]**** 接受變更。

以下是在傳統型裝置系列上的 [設定] 頁面。

![請移至 [設定] 並選擇 [更新與安全性]，然後選擇 [適用於開發人員] 以檢視您的選項](images/devmode-pc-options.png)

以下是在行動裝置系列上的 [設定] 頁面。

![從手機上的 [設定]，選擇 [更新與安全性]](images/devmode-mob.png)

### 應該選擇哪一個設定：側載 app 或開發人員模式？

根據預設，您只能從 Windows 市集安裝通用 Windows 平台 (UWP) app。 變更這些設定來使用開發人員功能，可以變更裝置的安全性層級。 您不應該從未驗證的來源安裝 app。

**側載 app**

通常是需要在受管理裝置上安裝自訂 app 而不經過 Windows 市集的公司或學校才會使用側載 app 設定。 在這個案例中，常見的情況是組織要強制執行停用「Windows 市集 app」**設定的原則，如先前在手機 [設定] 頁面的影像中所示。 組織也會提供側載 app 所需的憑證和安裝位置。 如需詳細資訊，請參閱 TechNet 文章[在 Windows 10 中側載 App](https://technet.microsoft.com/library/mt269549.aspx) 和[在 Microsoft Intune 中開始使用 App 部署](https://technet.microsoft.com/library/dn646955.aspx)。

裝置系列特定的資訊

-   在傳統型裝置系列上：您可以執行使用套件 ("Add-AppDevPackage.ps1") 建立的 Windows PowerShell 指令碼，來安裝 app 套件 (.appx) 以及執行該 app 所需的任何憑證。

-   在行動裝置系列上：如果已經安裝所需的憑證，您可以點選檔案來安裝任何要透過電子郵件傳送給您或位於 SD 記憶卡上的 .appx。

因為您無法在不具受信任憑證的裝置上安裝 app，所以**側載 app** 會是比開發人員模式更安全的選項。

**開發人員模式**

除了側載功能，開發人員模式設定還會啟用偵錯和其他部署選項。 它取代了 Windows 8.1 對開發人員授權的需求。

裝置系列特定的資訊

-   在傳統型裝置系列上：

    啟用開發人員模式，在 Visual Studio 中開發和偵錯 app。 如先前所述，若未啟用開發人員模式，系統將會在 Visual Studio 中提示您。

-   在行動裝置系列上：

    啟用開發人員模式，從 Visual Studio 部署 app 並在裝置上進行偵錯。

    您可以點選檔案來安裝透過電子郵件傳送給您或 SD 記憶卡上的任何 .appx。 請勿從未驗證的來源安裝應用程式。

**提示**  
有數個工具可讓您用來將應用程式從 Windows 10 電腦部署到 Windows 10 行動裝置。 這兩個裝置都必須透過有線或無線連線連接到網路的同一個子網路，或者必須透過 USB 來連接它們。 列出的任一個方法只會安裝 app 套件 (.appx)。它們不會安裝憑證。

-   使用 Windows 10 應用程式部署 (WinAppDeployCmd) 工具。 深入了解 [WinAppDeployCmd 工具](http://msdn.microsoft.com/library/windows/apps/mt203806.aspx)。
-   從 Windows 10 版本 1511 開始，您可以使用 [Device Portal](#device_portal)，從您的瀏覽器部署到執行 Windows 10 版本 1511 或更新版本的行動裝置。 使用 Device Portal (&lt;IP&gt;/appmanager.md) 中的 [應用程式]**** 頁面來上傳 app 套件 (.appx)，並將其安裝在裝置上。

 

### 設定群組原則或登錄機碼

您也可以使用群組原則或登錄機碼，做為啟用您的 Windows 10 傳統型裝置來進行開發的替代方法。

**在傳統型裝置系列上**

除非您有 Windows 10 家用版，否則請使用 gpedit.msc 設定群組原則來啟用您的裝置。 如果您有 Windows 10 家用版，您需要使用 regedit 或 PowerShell 命令，直接設定登錄機碼來啟用您的裝置。

**使用 gpedit 啟用您的裝置**

1.  執行 **Gpedit.msc**。
2.  移至 Local Computer Policy &gt; Computer Configuration &gt; Administrative Templates &gt; Windows Components &gt; App Package Deployment
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

## 開發人員模式功能

每個裝置系列可能都會有額外的開發人員功能。 只有在裝置上啟用 [**開發人員模式**] 時才能使用這些功能，而且可能會根據 OS 版本而不同。

此圖顯示 Windows 10 1511 版上行動裝置系列的開發人員功能。

![適用於行動裝置的開發人員模式選項](images/devmode-mob-options.png)

### <span id="device-discovery-and-pairing"> </span>裝置探索和 Device Portal

若要深入了解裝置探索和 Device Portal，請參閱 [Windows Device Portal 概觀](../debug-test-perf/device-portal.md)。

如需裝置特定的安裝指示，請參閱︰
- [HoloLens 的 Device Portal](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [IoT 的 Device Portal](http://ms-iot.github.io/content/en-US/win10/tools/DevicePortal.htm)
- [行動裝置的 Device Portal](../debug-test-perf/device-portal-mobile.md)
- [Xbox 的 Device Portal](../debug-test-perf/device-portal-xbox.md)

### 錯誤報告

設定此值，以指定手機上儲存了多少損毀傾印。

收集手機上的損毀傾印，讓您能夠在發生當機之後，即時直接存取重要的毀損資訊。 系統只會針對開發人員簽署的 app 收集傾印。 您可以在手機儲存體的 Documents\Debug 資料夾中找到傾印。 如需傾印檔案的詳細資訊，請參閱[使用傾印檔案](https://msdn.microsoft.com/library/d5zhxt22.aspx)。

## 將裝置從 Windows 8.1 升級至 Windows 10

在 Windows 8.1 裝置上建立或側載 app 時，您必須安裝開發人員授權。 如果您將裝置從 Windows 8.1 升級為 Windows 10，則會保留此資訊。 執行下列命令，從升級後的 Windows 10 裝置移除此資訊。 如果您直接從 Windows 8.1 升級為 Windows 10 版本 1511 或更新版本，就不需要執行此步驟。

**取消註冊開發人員授權**

1.  使用系統管理員權限執行 PowerShell。
2.  執行此命令：**unregister-windowsdeveloperlicense**。

在這之後，您需要啟用裝置來進行開發 (如本主題所述)，讓您能夠繼續在此裝置上進行開發。 如果您沒有這樣做，在偵錯您的 app 或嘗試為它建立套件時可能會收到錯誤。 以下是此錯誤的範例：

錯誤：DEP0700：app 的註冊失敗。




<!--HONumber=Mar16_HO5-->


