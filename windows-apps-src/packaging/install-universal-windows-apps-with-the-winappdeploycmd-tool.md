---
ms.assetid: 6AA037C0-35ED-4B9C-80A3-5E144D7EE94B
title: 使用 WinAppDeployCmd.exe 工具安裝 app
description: Windows 應用程式部署 (WinAppDeployCmd.exe) 是可以用來從 Windows 10 電腦將通用 Windows 平台 (UWP) app 部署到任何 Windows 10 行動裝置版裝置的命令列工具。
---
# 使用 WinAppDeployCmd.exe 工具安裝 app

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 應用程式部署 (WinAppDeployCmd.exe) 是可以用來從 Windows 10 電腦將通用 Windows 平台 (UWP) app 部署到任何 Windows 10 行動裝置版裝置的命令列工具。 當 Windows 10 行動裝置版裝置是透過 USB 連接或可在相同的子網路上使用而不需要 Microsoft Visual Studio 或該 app 適用的方案時，您就可以使用此工具來部署 .appx 套件。 本文章說明如何使用此工具安裝 UWP app。

您只需要安裝 Windows 10 SDK，即可從命令提示字元或指令碼檔案執行 WinAppDeployCmd 工具。 當您使用 WinAppDeployCmd.exe 安裝應用程式時，此工具會使用 .appx 檔案將您的應用程式側載到 Windows 10 行動裝置。 此命令不會安裝您的應用程式所需的憑證。 若要執行應用程式，Windows 10 行動裝置必須在開發人員模式或是已安裝憑證。

在將您的 app 部署到 Windows 10 行動裝置版裝置之前，您必須先建立套件。 如需詳細資訊，請參閱 \[這裡的父連結\]。

**WinAppDeployCmd.exe** 工具位於您的 Windows 10 電腦上的下列位置：**C:\\Program Files (x86)\\Windows Kits\\10\\bin\\x86\\WinAppDeployCmd.exe** (根據您的 SDK 的安裝路徑)。 首先，將您的 Windows 10 行動裝置版裝置連接到相同的子網路，或使用 USB 連線直接將它連接到您的 Windows 10 電腦。 然後使用下列語法與本文稍後此命令的範例來部署您的 .appx 套件：

## WinAppDeployCmd 語法和選項

以下是您可以針對 **WinAppDeployCmd.exe** 使用的語法

``` syntax
WinAppDeployCmd command -option <argument> ...
    WinAppDeployCmd devices
    WinAppDeployCmd devices <x>
    WinAppDeployCmd install -file <path> -ip <address>
    WinAppDeployCmd install -file <path> -guid <address> -pin <p>
    WinAppDeployCmd install -file <path> -ip <address> -dependency <a> <b> ...
    WinAppDeployCmd install -file <path> -guid <address> -dependency <a> <b> ...
    WinAppDeployCmd uninstall -file <path>
    WinAppDeployCmd uninstall -package <name>
    WinAppDeployCmd update -file <path>
    WinAppDeployCmd list -ip <address>
    WinAppDeployCmd list -guid <address>
```

您可以在目標裝置上安裝或解除安裝 app，或者您可以更新已經安裝的 app。 若要保留已安裝的 app 儲存的資料或設定，請使用 **update** 選項，而不是 **install** 選項。

下表描述 **WinAppDeployCmd.exe** 的命令。

|             |                                                                     |
|-------------|---------------------------------------------------------------------|
| **命令** | **說明**                                                     |
| 裝置     | 顯示可用網路裝置的清單。                         |
| install     | 將 UWP 應用程式套件安裝到目標裝置。                     |
| update      | 更新已經安裝在目標裝置的 UWP 應用程式。    |
| list        | 顯示已安裝在指定的目標裝置上的 UWP 應用程式清單。 |
| uninstall   | 從目標裝置解除安裝指定的 app 套件。         |

 

下表描述 **WinAppDeployCmd.exe** 的選項。

|                  |                                                                                                                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **命令**      | **說明**                                                                                                                                                                                               |
| -h (-help)       | 顯示命令、選項和引數。                                                                                                                                                                     |
| -ip              | 目標裝置的 IP 位址                                                                                                                                                                              |
| -g (-guid)       | 目標裝置的唯一識別碼。                                                                                                                                                                       |
| -d (-dependency) | (選擇性) 指定每個套件相依性的相依性路徑。 如果未指定路徑，工具會在應用程式套件和 SDK 目錄的根目錄中搜尋相依性。 |
| -f (-file)       | 要安裝、更新或解除安裝的應用程式套件檔案路徑。                                                                                                                                                |
| -p (-package)    | 要解除安裝的應用程式套件的完整套件名稱。 (您可以使用清單命令來尋找已安裝在裝置上的套件完整名稱)。                                                   |
| -pin             | 如果與目標裝置建立連線需要的 PIN。 (如果需要驗證，系統會提示您使用 -pin 選項重試。)                                                 |

 

下表描述 **WinAppDeployCmd.exe** 的選項。

|                        |                                                                              |
|------------------------|------------------------------------------------------------------------------|
| **引數**           | **說明**                                                              |
| &lt;x&gt;              | 逾時 (秒)。 (預設值為 10)                                          |
| &lt;address&gt;        | 目標裝置的 IP 位址或唯一識別碼。                        |
| &lt;a&gt;&lt;b&gt; ... | 每個 app 套件相依性的相依性路徑。                    |
| &lt;p&gt;              | 裝置設定中顯示用於建立連線的英數字元 PIN。 |
| &lt;path&gt;           | 檔案系統路徑。                                                            |
| &lt;name&gt;           | 要解除安裝的 app 套件的完整套件名稱。                          |

 
## WinAppDeployCmd.exe 範例

以下是如何使用 **WinAppDeployCmd.exe** 的語法從命令列部署的一些範例。

顯示可供部署的裝置。 命令會在 3 秒內逾時。

``` syntax
WinAppDeployCmd devices 3
```

從您電腦的下載目錄中的 MyApp.appx 套件安裝 app 到 IP 位址是 192.168.0.1 的 Windows 10 行動裝置版裝置，使用值為 A1B2C3 的 PIN 與裝置建立連線

``` syntax
WinAppDeployCmd install -file "Downloads\MyApp.appx" -ip 192.168.0.1 -pin A1B2C3
```

從 IP 位址是 192.168.0.1 的 Windows 10 行動裝置版裝置解除安裝指定的套件 (根據其完整名稱)。 您可以使用清單命令來查看安裝在裝置上的任何套件的完整名稱。

``` syntax
WinAppDeployCmd uninstall -package Company.MyApp_1.0.0.1_x64__qwertyuiop -ip 192.168.0.1
```

使用指定的 .appx 套件更新已安裝在 IP 位址是 192.168.0.1 的 Windows 10 行動裝置版裝置上的 app。

``` syntax
WinAppDeployCmd update -file "Downloads\MyApp.appx" -ip 192.168.0.1
```



<!--HONumber=Mar16_HO1-->


