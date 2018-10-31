---
author: laurenhughes
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: 使用 WinAppDeployCmd.exe 工具安裝 App
description: Windows 應用程式部署 (WinAppDeployCmd.exe) 是一個命令列工具，可以用來部署到任何 windows 10 裝置從 windows 10 電腦的通用 Windows 平台 (UWP) 應用程式。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 13468ce3b74992c026d94223b5e67aea99d79991
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "5830647"
---
# <a name="install-apps-with-the-winappdeploycmdexe-tool"></a>使用 WinAppDeployCmd.exe 工具安裝應用程式


Windows 應用程式部署 (WinAppDeployCmd.exe) 是一個命令列工具，可以用來部署到任何 windows 10 裝置從 windows 10 電腦的通用 Windows 平台 (UWP) 應用程式。 您可以使用此工具來部署應用程式套件，windows 10 裝置時透過 USB 連接或可在相同的子網路上使用而不需要針對該應用程式的 Microsoft Visual Studio 或方案。 您也可以不用封裝就將 App 部署到遠端的電腦或 Xbox One。 本文章說明如何使用此工具安裝 UWP App。

您只需要從命令提示字元或指令碼檔案執行 WinAppDeployCmd 工具安裝 windows 10 SDK。 當您使用 WinAppDeployCmd.exe 安裝應用程式時，這會使用.appx/.msix 檔案或 AppxManifest （針對鬆散檔案） 將側載到 windows 10 裝置上的應用程式。 此命令不會安裝您 App 所需的憑證。 若要執行應用程式，windows 10 裝置必須處於開發人員模式，或已安裝憑證。

若要部署到行動裝置，您必須先建立套件。 如需詳細資訊，請參閱[此處](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

**WinAppDeployCmd.exe**工具位於 windows 10 電腦上： **C:\\Program Files (x86) kits\\10\\bin\\<SDK Version>\\x86\\WinAppDeployCmd.exe** （根據您的安裝路徑 sdk）。 
> [!NOTE]
> 在 SDK 15063 和更新版本，SDK 在版本特定的資料夾中並排安裝。  舊版 SDK（14393 之前版本）直接寫入上層資料夾。

首先，您的 windows 10 裝置連接到相同的子網路，或將它連接到您 windows 10 的電腦使用 USB 連線直接。 然後使用下列語法與本文稍後此命令的範例來部署您的 UWP App：

## <a name="winappdeploycmd-syntax-and-options"></a>WinAppDeployCmd 語法和選項

**WinAppDeployCmd.exe** 使用的一般語法如下：
```syntax
WinAppDeployCmd command -option <argument>
```

以下是一些使用各種命令的其他語法範例︰
```syntax
WinAppDeployCmd devices
WinAppDeployCmd devices <x>
WinAppDeployCmd install -file <path> -ip <address>
WinAppDeployCmd install -file <path> -guid <address> -pin <p>
WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> 
WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b>
WinAppDeployCmd uninstall -file <path>
WinAppDeployCmd uninstall -package <name>
WinAppDeployCmd update -file <path>
WinAppDeployCmd list -ip <address>
WinAppDeployCmd list -guid <address>
WinAppDeployCmd deployfiles -file <path> -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd registerfiles -remotedeploydir <remoterelativepath> -ip <address>
WinAppDeployCmd addcreds -credserver <server> -credusername <username> -credpassword <password> -ip <address>
WinAppDeployCmd getcreds -credserver <server> -ip <address>
WinAppDeployCmd deletecreds -credserver <server> -ip <address>
```

您可以在目標裝置上安裝或解除安裝 App，也可以更新已經安裝的 App。 若要保留已安裝的 app 儲存的資料或設定，請使用 **update** 選項，而不是 **install** 選項。

下表描述 **WinAppDeployCmd.exe** 的命令。


| **命令**  | **說明**                                                     |
|--------------|---------------------------------------------------------------------|
| 裝置      | 顯示可用網路裝置的清單。                         |
| install      | 將 UWP 應用程式套件安裝到目標裝置。                     |
| update       | 更新已經安裝在目標裝置的 UWP 應用程式。    |
| list         | 顯示已安裝在指定的目標裝置上的 UWP 應用程式清單。 |
| uninstall    | 從目標裝置解除安裝指定的 App 套件。         |
| deployfiles  | 將位於目標路徑的鬆散檔案 App 複製到遠端裝置上的相對路徑。|
| registerfiles| 在遠端部署目錄註冊鬆散檔案 App。         |
| addcreds     | 將認證新增到 Xbox 以允許它存取 App 註冊的網路位置。|
| getcreds     | 取得目標從網路共用執行應用程式時所使用的網路認證。|
| deletecreds  | 刪除目標從網路共用執行應用程式時所使用的網路認證。|


下表描述 **WinAppDeployCmd.exe** 的選項。


| **命令**  | **說明**  |
|--------------|------------------|
| -h (-help)       | 顯示命令、選項和引數。 |
| -ip              | 目標裝置的 IP 位址 |
| -g (-guid)       | 目標裝置的唯一識別碼。|
| -d (-dependency) | (選擇性) 指定每個套件相依性的相依性路徑。 如果未指定路徑，工具會在應用程式套件和 SDK 目錄的根目錄中搜尋相依性。|
| -f (-file)       | 要安裝、更新或解除安裝的應用程式套件檔案路徑。|
| -p (-package)    | 要解除安裝的應用程式套件的完整套件名稱。 (您可以使用清單命令來尋找已安裝在裝置上的套件完整名稱) |
| -pin             | 如果與目標裝置建立連線需要的 PIN。 (如果需要驗證，系統會提示您使用 -pin 選項重試) |
| -credserver      | 目標使用之網路認證的伺服器名稱。 |
| -credusername    | 目標使用之網路認證的使用者名稱。 |
| -credpassword    | 目標使用之網路認證的密碼。 |
| -connecttimeout  | 連線到裝置時的逾時 (以秒為單位)。 |
| -remotedeploydir | 在遠端裝置上要將檔案複製到的相對目錄路徑/名稱。這會是已知且自動決定的遠端部署資料夾。 |
| -deleteextrafile | 切換來指示是否要清除遠端目錄中的現有檔案以符合來源目錄。 |


下表描述 **WinAppDeployCmd.exe** 的選項。

| **引數**           | **說明**                                                              |
|------------------------|------------------------------------------------------------------------------|
| &lt;x&gt;              | 逾時 (秒)。 (預設值為 10)                                          |
| &lt;位址&gt;        | 目標裝置的 IP 位址或唯一識別碼。                        |
| &lt;a&gt;&lt;b&gt; ... | 每個應用程式套件相依性的相依性路徑。                    |
| &lt;p&gt;              | 裝置設定中顯示用於建立連線的英數字元 PIN。 |
| &lt;path&gt;           | 檔案系統路徑。                                                            |
| &lt;name&gt;           | 要解除安裝之應用程式套件的完整套件名稱。                          |
| &lt;server&gt;         | 檔案網路上的伺服器。                                                  |
| &lt;username&gt;       | 具有檔案網路上伺服器存取權限之認證的使用者名稱。      |
| &lt;password&gt;       | 具有檔案網路上伺服器存取權限之認證的密碼。 |
| &lt;remotedeploydir&gt;| 裝置上相對於部署位置的目錄。                      |

 
## <a name="winappdeploycmdexe-examples"></a>WinAppDeployCmd.exe 範例

以下是如何使用 **WinAppDeployCmd.exe** 的語法從命令列部署的一些範例。

顯示可供部署的裝置。 命令會在 3 秒後逾時。

``` syntax
WinAppDeployCmd devices 3
```

從 windows 10 裝置 IP 位址是 192.168.0.1 a1b2c3 的 PIN 的建立與裝置連線到您的電腦 [下載] 目錄中的 MyApp.appx 套件安裝應用程式

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

從 IP 位址是 192.168.0.1 的 Windows 10 裝置解除安裝指定的套件 (根據其完整名稱)。 您可以使用清單命令來查看安裝在裝置上任何套件的完整名稱。

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

更新已安裝在 IP 位址是 192.168.0.1 使用指定的應用程式套件的 windows 10 裝置的 app。

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```

將 App 的檔案部署到 IP 位址為 192.168.0.1 的電腦或 Xbox 上；從和 AppxManifest 相同的資料夾，部署到該裝置部署路徑下的 app1_F5 目錄。

``` syntax
WinAppDeployCmd deployfiles -file "C:\apps\App1\AppxManifest.xml" -remotedeploydir app1_F5 -ip 192.168.0.1
```

在位於 192.168.0.1 的電腦或 Xbox 部署路徑下的 app1_F5 目錄註冊 App。

``` syntax
WinAppDeployCmd registerfiles -file app1_F5 -ip 192.168.0.1
```

## <a name="using-winappdeploycmd-to-set-up-run-from-pc-deployment-on-xbox-one"></a>使用 WinAppDeployCmd 在 Xbox One 上設定 [從電腦執行] 部署

[從電腦執行] 可讓您將 UWP 應用程式部署到 Xbox One，且不複製其上的二進位檔，而是將二進位檔裝載於與 Xbox 相同網路中的網路共用上。  若要這樣做，您需要開發人員已解除鎖定的 Xbox One，以及 Xbox 可存取之網路磁碟機上的鬆散檔案 UWP 應用程式。

執行下列程式碼來註冊 App︰
``` syntax
WinAppDeployCmd registerfiles -ip <Xbox One IP> -remotedeploydir <location of app> -username <user for network> -password <password for user>

ex. WinAppDeployCmd register files -ip 192.168.0.1 -remotedeploydir \\driveA\myAppLocation -username admin -password A1B2C3
```
