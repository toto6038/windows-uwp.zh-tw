---
author: normesta
Description: "執行 Desktop Converter App，將 Windows 傳統型應用程式 (例如，Win32、WPF 及 Windows Forms) 轉換為通用 Windows 平台 (UWP) app。"
Search.Product: eADQiWindows 10XVcnh
title: "傳統型轉 UWP 橋接器 Desktop App Converter"
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74c84eb6-4714-4e12-a658-09cb92b576e3
ms.openlocfilehash: 4daf45a4637ac846c550430cbc7518238ebd5bd8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-desktop-app-converter"></a>傳統型轉 UWP 橋接器：Desktop App Converter

[取得 Desktop App Converter](https://aka.ms/converter)

Desktop Converter App 是一個可讓您將現有針對 .NET 4.6.1 或 Win32 撰寫的傳統型應用程式移至通用 Windows 平台 (UWP) 的工具。 您可以透過轉換器以自動安裝 (無訊息) 模式執行桌面安裝程式，並取得您可在開發電腦上使用 Add-AppxPackage PowerShell Cmdlet 安裝的 Windows 應用程式套件。

Desktop App Converter 現在已可在 [Windows 市集](https://aka.ms/converter)中取得。

這個轉換器會在隔離的 Windows 環境中，使用全新基礎映像 (當作轉換器下載的一部分來提供) 來執行桌面安裝程式。 它會擷取桌面安裝程式所製作的任何登錄與檔案系統 I/O，並將它重新封裝為輸出的一部分。 轉換器會輸出含有套件識別資料的 Windows 應用程式套件，以及呼叫大範圍 WinRT API 的能力。

## <a name="whats-new"></a>新功能

最新版的 DAC 是 v1.0.9.0。 此更新的新增內容：

* 無安裝程式轉換：如果您的 App 是使用 xcopy 所安裝，或是熟悉 App 安裝程式對系統所做的變更，您可以將 -Installer 參數設定為 App 檔案的根目錄，在沒有使用安裝程式的情況執行轉換。
* 應用程式套件驗證︰使用新的 `-Verify` 旗標，根據傳統型橋接器及市集的需求驗證已轉換的應用程式套件


## <a name="system-requirements"></a>系統需求

### <a name="operating-system"></a>作業系統

+ Windows10 年度更新版 (10.0.14393.0 和更新版本) 專業版或企業版。

### <a name="hardware-configuration"></a>硬體設定

+ 64 位元 (x64) 處理器
+ 硬體協助虛擬化
+ 第二層位址轉譯 (SLAT)

### <a name="required-resources"></a>所需的資源

+ [適用於 Windows 10 的 Windows 軟體開發套件 (SDK)](https://go.microsoft.com/fwlink/?linkid=821375)

## <a name="set-up-the-desktop-app-converter"></a>安裝 Desktop App Converter

*(無安裝程式轉換不需要這些步驟)*

Desktop App Converter 依賴最新的 Windows 10 功能。 請確定您執行的是 Windows10 年度更新版 (14393.0) 或更新組建。

1.    從 [Windows 市集下載 DesktopAppConverter](https://aka.ms/converter) 和[符合您組建的基礎映像 .wim 檔案](https://aka.ms/converterimages)。  
2.    以系統管理員身分執行 DesktopAppConverter。 您可以從開始功能表以滑鼠右鍵按一下磚，然後選取 *\[更多\]* 下的 *\[以系統管理員身分執行\]*，或是從工作列以滑鼠右鍵按一下磚，再以滑鼠右鍵按一下快顯顯示的應用程式名稱，然後選取 *\[以系統管理員身分執行\]*，來執行此動作。
3.    從 App 主控台視窗中，執行 ```Set-ExecutionPolicy bypass```。
4.    從 App 主控台視窗執行 ```DesktopAppConverter.exe -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose``` 以安裝轉換器。
5.    如果系統在您執行前一個命令提示字元時提示您重新開機，請重新啟動您的電腦。

## <a name="run-the-desktop-app-converter"></a>執行 Desktop App Converter

+ **市集下載**︰使用 ```DesktopAppConverter.exe``` 以執行轉換器。

### <a name="usage"></a>使用方式

```CMD
DesktopAppConverter.exe
-Installer <String> [-InstallerArguments <String>] [-InstallerValidExitCodes <Int32>]
-Destination <String>
-PackageName <String>
-Publisher <String>
-Version <Version>
[-ExpandedBaseImage <String>]
[-AppExecutable <String>]
[-AppFileTypes <String>]
[-AppId <String>]
[-AppDisplayName <String>]
[-AppDescription <String>]
[-PackageDisplayName <String>]
[-PackagePublisherDisplayName <String>]
[-MakeAppx]
[-LogFile <String>]
[<CommonParameters>]  
```

### <a name="examples"></a>範例

下列範例示範如何將名為 *MyApp* by *MyPublisher* 的傳統型應用程式轉換為 Windows 應用程式套件。

#### <a name="no-installer-conversion"></a>無安裝程式轉換

使用無安裝程式轉換時，`-Installer` 參數會指向應用程式檔案的根目錄，而 `-AppExecutable` 參數為必要項。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyApp\ -AppExecutable MyApp.exe -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

#### <a name="installer-based-conversion"></a>安裝程式型轉換

使用安裝程式型轉換時，`-Installer` 會指向 App 的 Setup 安裝程式。

```cmd
DesktopAppConverter.exe -Installer C:\Installer\MyAppSetup.exe -InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" -Publisher "CN=MyPublisher" -Version 0.0.0.1 -MakeAppx -Sign -Verbose
```

## <a name="deploy-your-converted-windows-app-package"></a>部署您的已轉換 Windows 應用程式套件

使用 PowerShell 中的 [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) Cmdlet，將已簽署的應用程式套件 (.appx) 部署到使用者帳戶。

您可以在 Desktop App Converter (v0.1.24) 中使用 ```-Sign``` 旗標，自動簽署已轉換的應用程式。 或者，請參閱[簽署已轉換的傳統型應用程式](desktop-to-uwp-signing.md)，以了解如何自我簽署 AppX 套件。

您也可以包含 Add-AppXPackage PowerShell Cmdlet 的 ```-Register``` 參數，在開發流程期間從未封裝檔案的資料夾進行安裝。

如需部署和偵錯已轉換應用程式的詳細資訊，請參閱[部署和偵錯已轉換的 UWP app](desktop-to-uwp-deploy-and-debug.md)。

## <a name="sign-your-windows-app-package"></a>登入您的 Windows 應用程式套件

Add-AppxPackage Cmdlet 要求部署的 Windows 應用程式套件 (.appx) 必須進行簽署。 使用 ```-Sign``` 旗標做為轉換器命令列的一部分或 SignTool.exe (其隨附於 Microsoft Windows10 SDK) 來簽署 Windows 應用程式套件。

如需如何簽署 Windows 應用程式套件的詳細資訊，請參閱[簽署已轉換的傳統型應用程式](desktop-to-uwp-signing.md)。

注意：如果您嘗試使用自動產生的憑證來簽署套件，則需要使用預設密碼 "123456"。

## <a name="modify-vfs-folder-and-registry-hive-optional"></a>修改 VFS 資料夾和登錄區 (選用)

Desktop App Converter 採用極為保守的方式來篩選出容器中的檔案和系統雜訊。  這不是必要作業，但轉換之後您便可：

1. 檢閱 VFS 資料夾，並刪除安裝程式不需要的任何檔案。
2. 檢閱 Reg.dat 內容，並刪除應用程式未安裝/不需要的任何機碼。

如果您對已轉換的應用程式進行任何變更 (包含上述項目)，則不需要再次執行轉換器；您可以使用 MakeAppx 工具以及 DAC 為您應用程式產生的 appxmanifest.xml 檔案，手動重新封裝應用程式。 如需說明，請參閱[使用傳統型橋接器將您的應用程式手動轉換為 UWP](desktop-to-uwp-manual-conversion.md)。

## <a name="caveats"></a>警告

1. 主機電腦上的 Windows 10 組建必須符合您在 Desktop App Converter 下載中取得的基礎映像。  
2. 確定桌面安裝程式是在獨立的目錄中，因為轉換器會將目錄的所有內容複製到隔離的 Windows 環境。  
3. Desktop App Converter 目前只支援在 64 位元作業系統上執行轉換處理序。 您只能將已轉換的 Windows 應用程式套件部署到 64 位元 (x64) 作業系統。  
4. Desktop App Converter 需要桌面安裝程式，以便在自動安裝模式下執行。 確保您會使用 *-InstallerArguments* 參數，將適用於安裝程式的無訊息旗標傳遞至轉換器。
5. 發行公用的 SxS Fusion 組件將無法運作。 在安裝期間，應用程式可以發行公用的並列 Fusion 組件，可存取任何其他處理序。 在處理序啟用內容建立期間，這些組件是透過名為 CSRSS.exe 的系統處理序來擷取。 針對已轉換的處理序完成此動作之後，這些組件的啟用內容建立和模組載入將會失敗。 收件匣組件 (例如 ComCtl) 均隨附於作業系統，因此取得和它們的相依性是安全的。 SxS Fusion 組件會登錄於下列位置︰
  + 登錄： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 檔案系統︰%windir%\SideBySide

## <a name="known-issues"></a>已知問題

* 目前正在調查部分 OS 組建上發生的下列錯誤：

    * ```E_CREATTING_ISOLATED_ENV_FAILED```
    * ```E_STARTING_ISOLATED_ENV_FAILED```

    如果您遇到其中任一項錯誤，請確定使用的是[下載中心](https://aka.ms/converterimages)內的有效基礎映像。 若您使用的 .wim 有效，請透過 converter@microsoft.com 將您的記錄檔傳送給我們，協助我們進行調查。

* 如果您在先前已安裝 Desktop App Converter 的開發人員電腦上收到 Windows 測試人員正式發行前小眾測試版，則您在安裝新的基礎映像時可能會遇到錯誤 `New-ContainerNetwork: The object already exists`。 因應措施是從提升權限的命令提示字元執行 `Netsh int ipv4 reset` 命令，然後將電腦重新開機。

* 使用 "AnyCPU" 組建選項編譯的 .NET 應用程式，若其主要的可執行檔或任何相依性是放在 "Program Files" 或 "Windows\System32" 底下，安裝將會失敗。 因應措施是使用您的架構專用的傳統型安裝程式 (32 位元或 64 位元) 來成功產生 Windows 應用程式套件。

## <a name="telemetry-from-desktop-app-converter"></a>從 Desktop App Converter 進行遙測

傳統型應用程式轉換器可能會收集您及您軟體使用方式的相關資訊，並將這項資訊傳送給 Microsoft。 您可以在產品文件和 [Microsoft 隱私權聲明](http://go.microsoft.com/fwlink/?LinkId=521839)中深入了解 Microsoft 的資料收集及使用方式。 您同意遵守 Microsoft 隱私權聲明的所有適用條款。

預設會針對傳統型應用程式轉換器啟用遙測。 新增下列登錄機碼來設定所需設定的遙測︰  

```cmd
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 使用設為 1 的 DWORD 來新增或編輯 *DisableTelemetry* 值。
+ 若要啟用遙測，請移除此機碼或將值設為 0。

## <a name="desktop-app-converter-usage"></a>傳統型應用程式轉換器的使用方式

以下是 Desktop App Converter 的參數清單。 您也可以執行下列命令來檢視此清單：   

```CMD
Get-Help DesktopAppConverter.exe -detailed
```

### <a name="setup-parameters"></a>安裝程式參數  

|參數|說明|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | 在安裝模式中執行 DesktopAppConverter。 安裝模式支援展開提供的基礎映像。|
|```-BaseImage <String>``` | 未展開之基礎映像的完整路徑。 如果指定了 -Setup，則此為必要參數。|
|```-LogFile <String>``` [optional] | 指定記錄檔。 如果省略，則會建立記錄檔暫時位置。|
|```-NatSubnetPrefix <String>``` [optional] | 可供 Nat 執行個體使用的首碼值。 通常，只有在您的主機電腦附加到與轉換器的 NetNat 相同的子網路範圍時，您才會想要變更此值。 您可以使用 **Get-NetNat** Cmdlet 來查詢目前的轉換器 NetNat 組態。 |
|```-NoRestart [<SwitchParameter>]``` | 執行安裝時不顯示重新開機的提示 (需要重新開機才能啟用容器功能)。 |

### <a name="conversion-parameters"></a>轉換參數  

|參數|描述|
|---------|-----------|
|```-AppInstallPath <String> [optional]``` | 您的應用程式在安裝之後，其已安裝檔案根資料夾的完整路徑 (例如 "C:\Program Files (x86)\MyApp")。|
|```-Destination <String>``` | 適用於轉換器 appx 輸出的所需目的地，如果尚未存在，DesktopAppConverter可以建立此位置。|
|```-Installer <String>``` | 應用程式的安裝程式路徑，必須能夠自動執行或以無訊息方式執行。 無安裝程式碼轉換，這是 App 檔案根目錄的路徑。 |
|```-InstallerArguments <String>``` [optional] | 以逗號分隔的引數清單或字串，可強制您的安裝程式自動執行或以無訊息方式執行。 如果您的安裝程式是 msi，則此為選用參數。 若要取得安裝程式的記錄，請提供此處的安裝程式記錄引數，然後使用路徑 ```<log_folder>```，此為轉換器會使用適當路徑來取代的語彙基元。 <br><br>**注意︰自動/無訊息旗標和記錄引數會隨著安裝程式技術而不同。** <br><br>此參數的一個用法範例︰```-InstallerArguments "/silent /log <log_folder>\install.log"``` 另一個不會產生記錄檔的範例可能看起來如下︰```-InstallerArguments "/quiet", "/norestart"``` 同樣地，如果您想要轉換器擷取它並將它放在最後一個記錄資料夾中，就必須照字面將任何記錄引導至語彙基元路徑 ```<log_folder>```。|
|```-InstallerValidExitCodes <Int32>``` [optional] | 以逗號分隔的結束代碼清單，表示您的安裝程式已成功執行 (例如︰0, 1234, 5678)。  針對非 msi，這個值預設是 0，針對 msi 則為 0, 1641, 3010。|

### <a name="windows-app-package-identity-parameters"></a>Windows 應用程式套件身分識別參數  

|參數|說明|
|---------|-----------|
|```-PackageName <String>``` | 通用 Windows 應用程式套件的名稱
|```-Publisher <String>``` | 通用 Windows 應用程式套件的發行者
|```-Version <Version>``` | 通用 Windows 應用程式套件的版本號碼

### <a name="optional-windows-app-package-manifest-parameters"></a>選用 Windows 應用程式套件資訊清單參數  

|參數|說明|
|---------|-----------|
|```-AppExecutable <String> [optional]``` [optional] | 應用程式主要可執行檔 (例如 "MyApp.exe") 的名稱。 此參數是無安裝程式轉換的必要參數。 |
|```-AppFileTypes <String>``` [optional] | 以逗號分隔的檔案類型清單，應用程式將會與其產生關聯 (例如 ".txt, .doc"，不需加上引號)。|
|```-AppId <String>``` [optional] | 在 Windows 應用程式套件資訊清單中指定要為應用程式識別碼所設定的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。|
|```-AppDisplayName <String>``` [optional] | 指定要在 Windows 應用程式套件資訊清單中設定應用程式顯示名稱的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。 |
|```-AppDescription <String>``` [optional] | 在 Windows 應用程式套件資訊清單中指定要為應用程式描述所設定的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。|
|```-PackageDisplayName <String>``` [optional] | 指定要在 Windows 應用程式套件資訊清單中設定套件顯示名稱的值。 如果未指定，它將會設定為針對 *PackageName* 傳入的值。 |
|```-PackagePublisherDisplayName <String>``` [optional] | 指定要在 Windows 應用程式套件資訊清單中設定套件發行者顯示名稱的值。 如果未指定，它將會設定為針對 *Publisher* 傳入的值。 |

### <a name="other-conversion-parameters"></a>其他轉換參數  

|參數|說明|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [optional] | 已經展開之基礎映像的完整路徑。|
|```-MakeAppx [<SwitchParameter>]``` [optional] | 一個參數，如果顯示，即會通知這個指令碼呼叫輸出上的 MakeAppx。 |
|```-LogFile <String>``` [optional] | 指定記錄檔。 如果省略，則會建立記錄檔暫時位置。 |
| ```-Sign [<SwitchParameter>] [optional]``` | 指示此指令碼簽署輸出 Windows 應用程式套件。 這個參數應該出現在參數 ```-MakeAppx``` 旁邊。
|```<Common parameters>``` | 這個 Cmdlet 支援一般參數：*Verbose*、*Debug*、*ErrorAction*、*ErrorVariable*、*WarningAction*、*WarningVariable*、*OutBuffer*、*PipelineVariable* 及 *OutVariable*。 如需詳細資訊，請參閱 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)。 |
| ```-Verify [<SwitchParameter>] [optional]``` | 一個參數，如果存在，這個參數會指示 DAC 根據傳統型橋接器及 Windows 市集需求來驗證已轉換的 App 套件。 結果會是驗證報告「VerifyReport.xml」，這在瀏覽器中呈現最佳視覺效果。 這個參數應該出現在參數 `-MakeAppx` 旁邊。

### <a name="cleanup-parameters"></a>清理參數

|參數|說明|
|---------|-----------|
|```Cleanup [<Option>]``` | 執行 DesktopAppConverter 成品的清理。 Cleanup 模式有 3 個有效的選項。 |
|```Cleanup All``` | 刪除所有展開的基本映像、移除任何暫存轉換器檔案、移除容器網路，並停用選用的 Windows 功能、容器。 |
|```Cleanup WorkDirectory``` | 移除所有的暫存轉換器檔案。 |
|```Cleanup ExpandedImage``` | 刪除所有已展開且安裝在主機電腦上的基本映像。 |

### <a name="package-architecture"></a>套件架構

Desktop App Converter 現在支援建立可讓您在 x86 和 amd64 電腦上安裝與執行的 x86 與 x64 應用程式套件。 請注意，Desktop App Converter 仍需要在 AMD64 電腦上執行，才能成功轉換。

|參數|說明|
|---------|-----------|
|```-PackageArch <String>``` | 產生具有指定架構的套件。 有效的選項是 'x86' 或 'x64'；例如，-PackageArch x86。 此為選擇性參數。 如果沒有指定，傳統型應用程式轉換器將會嘗試自動偵測套件架構。 如果自動偵測失敗，將預設為 x64 套件。

### <a name="running-the-peheadercertfixtool"></a>執行 PEHeaderCertFixTool

轉換過程中，DesktopAppConverter 會自動執行 PEHeaderCertFixTool，以修正任何損毀的 PE 標頭。 不過，您也可以在 UWP Windows 應用程式套件、鬆散檔案或特定的二進位檔上執行 PEHeaderCertFixTool。 使用範例：

```CMD
PEHeaderCertFixTool.exe <binary file>|<.appx package>|<folder> [/c] [/v]
 /c   -- check for corrupted certificate but do not fix (optional)
 /v   -- verbose (optional)
example1: PEHeaderCertFixTool app.exe
example2: PEHeaderCertFixTool c:\package.appx /c
example3: PEHeaderCertFixTool c:\myapp /c /v
```

## <a name="language-support"></a>語言支援

Desktop App Converter 不支援 Unicode；因此，無法在工具上使用中文字元或非 ASCII 字元。

## <a name="see-also"></a>另請參閱

+ [使用 Desktop App Converter 將傳統型應用程式移至 UWP](https://channel9.msdn.com/events/Build/2016/P504)
+ [Project Centennial︰將現有的傳統型應用程式移至通用 Windows 平台](https://channel9.msdn.com/events/Build/2016/B829)  
