---
author: awkoren
Description: "執行傳統型轉換器應用程式，將 Windows 傳統型應用程式 (例如，Win32、WPF 及 Windows Forms) 轉換為通用 Windows 平台 (UWP) 應用程式。"
Search.Product: eADQiWindows 10XVcnh
title: "傳統型應用程式轉換器預覽 (Project Centennial)"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 8a22285467005722ad6ee5bf4f129a7dfdea944c

---

# 傳統型應用程式轉換器預覽 (Project Centennial)

\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

[取得傳統型應用程式轉換器。](http://go.microsoft.com/fwlink/?LinkId=785437)

傳統型應用程式轉換器是一個發行前版本工具，可讓您將現有針對 .NET 4.6.1 或 Win32 撰寫的傳統型應用程式移至通用 Windows 平台 (UWP)。 您可以透過轉換器以自動安裝 (無訊息) 模式執行桌面安裝程式，並取得您可在開發電腦上使用 Add-AppxPackage PowerShell Cmdlet 安裝的 AppX 套件。

轉換器會在隔離的 Windows 環境中，使用全新基礎映像 (當作轉換器下載的一部分來提供) 來執行桌面安裝程式。 它會擷取桌面安裝程式所製作的任何登錄與檔案系統 I/O，並將它重新封裝為輸出的一部分。 轉換器會輸出含有套件識別資料的 AppX 套件，以及呼叫大範圍 WinRT API 的能力。

## 新功能

本節概述傳統型應用程式轉換器版本之間的變更。 

### 7/7/2016 (v0.1.22)

* 新增以下支援：自動偵測來自您傳統型應用程式的殼層擴充功能，以及在您 UWP 套件的 AppXManifest 中宣告這些殼層擴充功能。 若要深入了解傳統型擴充功能，請參閱[**已轉換傳統型應用程式擴充功能**](desktop-to-uwp-extensions.md)。 
* 針對大型應用程式集改善 AppExecutable 偵測功能。 

### 6/16/2016 (v0.1.20)

* 修正會致使無法順利在最新 Windows 10 Insider Preview 組建上成功轉換的任何問題。 
* 使用 ```–PackageArch``` 取代 ```–CreateX86Package```，這可讓您指定產生的套件架構。 

### 6/8/2016

* 已新增在執行轉換器的 AMD64 主機電腦上產生 x86 appx 套件的支援。
* 藉由移除任何先前延伸的基本映像，來減少磁碟空間使用量。
* 已新增對清理暫存檔和任何不必要的基本映像的支援。
* 已改進偵測檔案類型和通訊協定關聯的支援。
* 已改進針對一組大型 app 偵測 AppExecutable 屬性的邏輯。
* 已新增針對 MSI 型安裝程式提供額外 –InstallerArguments 的支援。
* 已修正轉換程序期間的任何 PathTooLongException 錯誤。

### 2016/5/12

- 針對 Windows 專業版支援還原功能。 
- 轉換器 ```-Setup``` 旗標現在可啟用 Windows 容器功能，以及處理基礎映像擴充。 從提升權限的 PowerShell 命令提示字元執行下列命令，以執行一次性安裝： ```PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage BaseImage-12345.wim -Verbose```
- 已新增應用程式安裝路徑的自動偵測，並將應用程式根目錄移到 VFS 外部，以便在執行階段降低任何不必要的檔案系統重新導向。
- 已新增擴充之基礎映像的自動偵測，做為轉換處理序的一部分。
- 已新增檔案類型關聯和通訊協定的自動偵測。
- 已改進偵測 [開始功能表] 捷徑的邏輯。
- 已改進檔案系統篩選，以保留應用程式安裝的 MUI 檔案。
- 已更新資訊清單中專案 Centennial 的最低支援桌面版本 (10.0.14342.0)。

## 系統需求

### 支援的作業系統
+ Windows 10 年度更新版企業版預覽 (組建 10.0.14342.0 和更新版本)

### 必要的硬體組態

您的電腦必須具備下列基本功能︰
+ 64 位元 (x64) 處理器
+ 硬體協助虛擬化
+ 第二層位址轉譯 (SLAT)

### 建議的資源
+ [適用於 Windows 10 的 Windows 軟體開發套件 (SDK)](http://go.microsoft.com/fwlink/?LinkId=615097)

## 安裝傳統型應用程式轉換器   
傳統型應用程式轉換器依賴 Windows 10 功能，此為 Windows Insider Preview 組建的一部分，是正式發行前小眾測試版。 確定您是在最新組建上運用轉換器。

1. 確定您擁有最新的 Windows 10 Insider Preview 作業系統 – 企業版或專業版 (http://insider.windows.com)。 
2. 下載與您的 Insider Preview 組建相符的 DesktopAppConverter.zip 和基本映像 .wim (http://aka.ms/converter)。 
3. 將 DesktopAppConverter.zip 擷取到本機資料夾。
4. 從系統管理員的 PowerShell 視窗︰  
```CMD
PS C:\> Set-ExecutionPolicy bypass
```
5. 從系統管理員的 PowerShell 視窗執行下列命令來安裝轉換器︰
```CMD
PS C:\> .\DesktopAppConverter.ps1 -Setup -BaseImage .\BaseImage-1XXXX.wim -Verbose
```
6. 如果執行前一個命令提示字元會提示您重新開機，請重新啟動您的電腦，並再次執行命令。

## 執行傳統型應用程式轉換器
傳統型應用程式轉換器有兩個進入點︰PowerShell 和命令介面。 您可以使用這其中一個進入點開始轉換處理序。

### 使用方式
```CMD
DesktopAppConverter.ps1
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

### 範例
下列範例示範如何透過 *&lt;publisher_name&gt;*，將命名為 *MyApp* 的傳統型應用程式轉換為 UWP 套件 (AppX)。

+ 從系統管理員的 PowerShell 視窗執行下列命令︰
```CMD
PS C:\>.\DesktopAppConverter.ps1 -Installer C:\Installer\MyApp.exe 
-InstallerArguments "/S" -Destination C:\Output\MyApp -PackageName "MyApp" 
-Publisher "CN=<publisher_name>" -Version 0.0.0.1 -MakeAppx -Verbose
```

## 部署已轉換的 AppX
使用 PowerShell 中的 [Add-AppxPackage](https://technet.microsoft.com/library/hh856048.aspx) Cmdlet，將已簽署的應用程式套件 (.appx) 部署到使用者帳戶。 若要簽署 .appx 套件，請參閱下一節＜簽署您的 .Appx 套件＞。 此外，您可以包含 Cmdlet 的 *Register*，在開發流程期間從未封裝的檔案資料夾中進行安裝。 如需詳細資訊，請參閱[部署和偵錯已轉換的 UWP App](desktop-to-uwp-deploy-and-debug.md)。

## 簽署您的 .Appx 套件

Add-AppxPackage Cmdlet 要求部署的應用程式套件 (.appx) 必須進行簽署。 使用 SignTool.exe (其隨附於 Microsoft Windows 10 SDK) 來簽署 .appx 套件。

### 範例
```CMD
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```
**注意：**當您執行 MakeCert.exe 且系統要求您輸入密碼時，請選取 [無]****。

如需憑證和簽章的詳細資訊，請參閱︰

+ [做法︰建立開發時要使用的暫時憑證](https://msdn.microsoft.com/library/ms733813.aspx)
+ [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
+ [SignTool.exe (簽署工具)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)

### 警告
1. 主機電腦上的 Windows 10 組建必須符合您在傳統型應用程式轉換器下載中取得的基礎映像。  
2. 確定桌面安裝程式是在獨立的目錄中，因為轉換器會將目錄的所有內容複製到隔離的 Windows 環境。  
3. 傳統型應用程式轉換器目前只支援在 64 位元作業系統上執行轉換處理序。 您只能將已轉換的 .appx 套件部署到 64 位元 (x64) 作業系統。  
4. 傳統型應用程式轉換器需要桌面安裝程式，以便在自動安裝模式下執行。 確保您會使用 *-InstallerArguments* 參數，將適用於安裝程式的無訊息旗標傳遞至轉換器。
5. 發行公用的 SxS Fusion 組件將無法運作。 在安裝期間，應用程式可以發行公用的並列 Fusion 組件，可存取任何其他處理序。 在處理序啟用內容建立期間，這些組件是透過名為 CSRSS.exe 的系統處理序來擷取。 針對 Centennial 處理序完成此動作之後，這些組件的啟用內容建立和模組載入將會失敗。 收件匣組件 (例如 ComCtl) 均隨附於作業系統，因此從 Centennial 處理序中取得與其的相依性是安全。 SxS Fusion 組件會登錄於下列位置︰
  + 登錄： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 檔案系統︰%windir%\SideBySide

## 已知問題

+ 如果您在先前已安裝「傳統型應用程式轉換器預覽」的開發人員電腦上收到 Windows 測試人員正式發行前小眾測試版，則您在安裝新的基本映像時可能會遇到錯誤 `New-ContainerNetwork: The object already exists`。 因應措施是從提升權限的命令提示字元執行 `Netsh int ipv4 reset` 命令，然後將電腦重新開機。 
+ 使用 "AnyCPU" 組建選項編譯的 .NET 應用程式，若其主要的可執行檔或任何相依性是放在 "Program Files" 或 "Windows\System32" 底下，安裝將會失敗。 因應措施是使用您的架構專用的傳統型安裝程式 (32 位元或 64 位元) 來成功產生 AppX 套件。

## 從傳統型應用程式轉換器進行遙測  
傳統型應用程式轉換器可能會收集您及您軟體使用方式的相關資訊，並將這項資訊傳送給 Microsoft。 您可以在產品文件和 [Microsoft 隱私權聲明](http://go.microsoft.com/fwlink/?LinkId=521839)中深入了解 Microsoft 的資料收集及使用方式。 您同意遵守 Microsoft 隱私權聲明的所有適用條款。

預設會針對傳統型應用程式轉換器啟用遙測。 新增下列登錄機碼來設定所需設定的遙測︰  
```CMD
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\DesktopAppConverter
```
+ 使用設為 1 的 DWORD 來新增或編輯 *DisableTelemetry* 值。
+ 若要啟用遙測，請移除此機碼或將值設為 0。

## 傳統型應用程式轉換器的使用方式
以下是傳統型應用程式轉換器的參數清單。 您也可以執行下列命令，在 Windows Powershell 視窗中檢視此清單︰  
```CMD
get-help .\DesktopAppConverter.ps1 -detailed
```

### 安裝程式參數  
|參數|說明|
|---------|-----------|
|```-Setup [<SwitchParameter>]``` | 在安裝模式中執行 DesktopAppConverter。 安裝模式支援展開提供的基礎映像。|
|```-BaseImage <String>``` | 未展開之基礎映像的完整路徑。 如果指定了 -Setup，則此為必要參數。|
|```-LogFile <String>``` [optional] | 指定記錄檔。 如果省略，則會建立記錄檔暫時位置。|
|```-NatSubnetPrefix <String>``` [optional] | 可供 Nat 執行個體使用的首碼值。 通常，只有在您的主機電腦附加到與轉換器的 NetNat 相同的子網路範圍時，您才會想要變更此值。 您可以使用 **Get-NetNat** Cmdlet 來查詢目前的轉換器 NetNat 組態。 |
|```-NoRestart [<SwitchParameter>]``` | 執行安裝時不顯示重新開機的提示 (需要重新開機才能啟用容器功能)。 |

### 轉換參數  
|參數|說明|
|---------|-----------|
|```-Installer <String>``` | 應用程式的安裝程式路徑 - 必須能夠自動執行或以無訊息方式執行|
|```-InstallerArguments <String>``` [optional] | 以逗號分隔的引數清單或字串，可強制您的安裝程式自動執行或以無訊息方式執行。 如果您的安裝程式是 msi，則此為選用參數。 若要取得安裝程式的記錄，請提供此處的安裝程式記錄引數，然後使用路徑 ```<log_folder>```，此為轉換器會使用適當路徑來取代的語彙基元。 <br><br>**注意︰自動/無訊息旗標和記錄引數會隨著安裝程式技術而不同。** <br><br>此參數的一個用法範例︰```-InstallerArguments "/silent /log <log_folder>\install.log"``` 另一個不會產生記錄檔的範例可能看起來如下︰```-InstallerArguments "/quiet", "/norestart"``` 同樣地，如果您想要轉換器擷取它並將它放在最後一個記錄資料夾中，就必須照字面將任何記錄引導至語彙基元路徑 ```<log_folder>```。|
|```-InstallerValidExitCodes <Int32>``` [optional] | 以逗號分隔的結束代碼清單，表示您的安裝程式已成功執行 (例如︰0, 1234, 5678)。  針對非 msi，這個值預設是 0，針對 msi 則為 0, 1641, 3010。|
|```-Destination <String>``` | 適用於轉換器 appx 輸出的所需目的地 - 如果尚未存在，DesktopAppConverter可以建立此位置。|

### Appx 識別資訊參數  
|參數|說明|
|---------|-----------|
|```-PackageName <String>``` | 通用 Windows 應用程式套件的名稱
|```-Publisher <String>``` | 通用 Windows 應用程式套件的發行者
|```-Version <Version>``` | 通用 Windows 應用程式套件的版本號碼

### 選用的 Appx 資訊清單參數  
|參數|說明|
|---------|-----------|
|```-AppExecutable <String>``` [optional] | 如果應用程式的主要執行檔已經安裝，即為其完整路徑 (但不一定要安裝於此)，例如 "C:\Program Files (x86)\MyApp\MyApp.exe"。|
|```-AppFileTypes <String>``` [optional] | 以逗號分隔的檔案類型清單，應用程式將會與其產生關聯 (例如 ".txt, .doc"，不需加上引號)。|
|```-AppId <String>``` [optional] | 在 appx 資訊清單中指定要為應用程式識別碼所設定的值。 如果未指定，它將會設定為針對 */packagename* 傳入的值。|
|```-AppDisplayName <String>``` [optional] | 指定要在 appx 資訊清單中設定應用程式顯示名稱的值。 如果未指定，它將會設定為針對 */packagename* 傳入的值。 |
|```-AppDescription <String>``` [optional] | 指定要在 appx 資訊清單中設定應用程式說明的值。 如果未指定，它將會設定為針對 */packagename* 傳入的值。|
|```-PackageDisplayName <String>``` [optional] | 指定要在 appx 資訊清單中設定套件顯示名稱的值。 如果未指定，它將會設定為針對 */packagename* 傳入的值。 |
|```-PackagePublisherDisplayName <String>``` [optional] | 指定要在 appx 資訊清單中設定套件發行者顯示名稱的值。 如果未指定，它將會設定為針對 *Publisher* 傳入的值。 |

### 其他轉換參數  
|參數|說明|
|---------|-----------|
|```-ExpandedBaseImage <String>``` [optional] | 已經展開之基礎映像的完整路徑。|
|```-MakeAppx [<SwitchParameter>]``` [optional] | 一個參數，如果顯示，即會通知這個指令碼呼叫輸出上的 MakeAppx。 |
|```-LogFile <String>``` [optional] | 指定記錄檔。 如果省略，則會建立記錄檔暫時位置。 |
|```<Common parameters>``` | 這個 Cmdlet 支援一般參數：*Verbose*、*Debug*、*ErrorAction*、*ErrorVariable*、*WarningAction*、*WarningVariable*、*OutBuffer*、*PipelineVariable* 及 *OutVariable*。 如需詳細資訊，請參閱 [about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)。 |

### 清理參數
|參數|說明|
|---------|-----------|
|```Cleanup [<Option>]``` | 執行 DesktopAppConverter 成品的清理。 Cleanup 模式有 3 個有效的選項。 |
|```Cleanup All``` | 刪除所有展開的基本映像、移除任何暫存轉換器檔案、移除容器網路，並停用選用的 Windows 功能、容器。 |
|```Cleanup WorkDirectory``` | 移除所有的暫存轉換器檔案。 |
|```Cleanup ExpandedImages``` | 刪除所有已展開且安裝在主機電腦上的基本映像。 |

### 套件架構
「傳統型應用程式轉換器預覽」現在支援建立可讓您在 x86 和 amd64 電腦上安裝與執行的 x86 與 x64 應用程式套件。 請注意，「傳統型應用程式轉換器」仍需要在 AMD64 電腦上執行才能成功轉換。

|參數|說明|
|---------|-----------|
|```-PackageArch <String>``` | 產生具有指定架構的套件。 有效的選項是 'x86' 或 'x64'；例如，-PackageArch x86。 此為選擇性參數。 如果沒有指定，傳統型應用程式轉換器將會嘗試自動偵測套件架構。 如果自動偵測失敗，將預設為 x64 套件。 |

## 另請參閱
+ [取得傳統型應用程式轉換器](http://go.microsoft.com/fwlink/?LinkId=785437)
+ [將您的傳統型應用程式移至通用 Windows 平台](https://developer.microsoft.com/windows/bridges/desktop)
+ [使用傳統型應用程式轉換器將傳統型應用程式移至 UWP](https://channel9.msdn.com/events/Build/2016/P504)
+ [專案 Centennial︰將現有的傳統型應用程式移至通用 Windows 平台](https://channel9.msdn.com/events/Build/2016/B829)  
+ [適用於桌面橋接器的 UserVoice (專案 Centennial)](http://aka.ms/UserVoiceDesktopToUwp)
+ [GitHub 上的傳統型應用程式橋接至 UWP 的程式碼範例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)



<!--HONumber=Jul16_HO2-->


