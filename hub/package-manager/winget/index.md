---
title: 使用 winget 工具來安裝和管理應用程式
description: winget 命令列工具可讓開發人員在 Windows 10 電腦上探索、安裝、升級、移除和設定應用程式。
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 3b3f108de117fb937a7a670497a4a1a1d5810aca
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334531"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>使用 winget 工具來安裝和管理應用程式

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

**winget** 命令列工具可讓開發人員在 Windows 10 電腦上探索、安裝、升級、移除和設定應用程式。 此工具是 Windows 封裝管理員服務的用戶端介面。

**winget** 工具目前為預覽版，因此並非所有規劃的功能都可供使用。

## <a name="install-winget"></a>安裝 winget

安裝 **winget** 工具的方法有好幾種：

* **winget** 工具包含在正式發行前的 [Windows 應用程式安裝程式](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab)小眾測試版或預覽版中。 您必須安裝**應用程式安裝程式**的預覽版本，才能使用 **winget**。 若要取得早期版本的存取權，請將您的要求提交至 [Windows 封裝管理員的測試人員計畫](https://aka.ms/AppInstaller_InsiderProgram)。 參與正式發行前小眾測試版的更新通道，可確保您能看到最新的預覽版更新。

* 參與 [Windows 測試人員的正式發行前小眾測試版更新通道](https://insider.windows.com)。

* 安裝 [winget 存放庫](https://github.com/microsoft/winget-cli)發行資料夾中的 Windows 桌面應用程式安裝程式套件。

> [!NOTE]
> **winget** 工具需要 Windows 10 1709 版 (10.0.16299) 或更新版本的 Windows 10。

## <a name="administrator-considerations"></a>系統管理員考量

安裝程式行為可能會根據您是否以系統管理員權限執行 **winget**，而有所不同。

* 如果在沒有系統管理員權限的情況下執行 **winget**，則可能需要提供某些應用程式的[權限](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/)才能安裝。 當安裝程式執行時，Windows 會提示您[提高權限](https://docs.microsoft.com/windows/security/identity-protection/user-account-control)。 如果您選擇不提高權限，應用程式將無法安裝。  

* 在系統管理員命令提示字元中執行 **winget** 時，您將不會看到應用程式需要的[提高權限提示](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/how-user-account-control-works)。 以系統管理員身分執行命令提示字元時，請務必小心，並且只安裝您信任的應用程式。

## <a name="use-winget"></a>使用 winget

安裝**應用程式安裝程式**之後，您可以在命令提示字元中輸入 'winget' 來執行 **winget**。

其中一個最常見的使用案例是搜尋並安裝最愛的工具。

1. 若要[搜尋](search.md)工具，請輸入 `winget search \<appname>`。
2. 確認您想要的工具可供使用之後，您就可以輸入 `winget install \<appname>`來[安裝](install.md)工具。 **winget** 工具會啟動安裝程式，並將應用程式安裝在您的電腦上。
    ![winget 命令列](images\install.png)

3. 除了安裝和搜尋之外，**winget** 還提供一些其他命令，可讓您[顯示應用程式的詳細資料](show.md)、[變更來源](source.md)和[驗證封裝](validate.md)。 如需完整的命令清單，請輸入：`winget --help`。
    ![winget 說明](images\help.png)

### <a name="commands"></a>命令

目前的 **winget** 工具預覽版支援下列命令。

| 命令 | 描述 |
|---------|-------------|
| [hash](hash.md) | 產生安裝程式的 SHA256 雜湊。 |
| [說明](help.md) | 顯示 **winget** 工具命令的說明。 |
| [install](install.md) | 安裝指定的應用程式。 |
| [search](search.md) | 搜尋應用程式。 |
| [show](show.md) | 顯示指定應用程式的詳細資料。 |
| [source](source.md) | 新增、移除及更新 **winget** 工具所存取的 Windows 封裝管理員存放庫。 |
| [validate](validate.md) | 驗證資訊清單檔案以提交至 Windows 封裝管理員存放庫。 |

### <a name="options"></a>選項

目前的 **winget** 工具預覽版支援下列選項。

| 選項 | 描述 |
|--------------|-------------|
| **-v,--version** | 此選項會傳回目前的 winget 版本。 |
| **--info** |  info 會提供您有關 winget 的所有詳細資訊，包括授權和隱私權聲明的連結。 |
| **-?, --help** |  取得 winget 的其他說明 |

## <a name="supported-installer-formats"></a>支援的安裝程式格式

目前的 **winget** 工具預覽版支援下列類型的安裝程式。

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>以指令碼撰寫 winget

您可以撰寫批次指令碼和 powershell 指令碼來安裝多個應用程式。

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> 撰寫指令碼後，**winget** 會依照指定的順序啟動應用程式。 當安裝程式傳回成功或失敗時，**winget** 將會啟動下一個安裝程式。 如果安裝程式啟動另一個程序，其可能會提前傳回 **winget**。 這會導致 **winget** 在先前的安裝程式完成之前，就安裝下一個安裝程式。

## <a name="missing-tools"></a>缺少工具

如果[社群存放庫](../package/repository.md)未包含您的工具或應用程式。 請將封裝提交至我們的[存放庫](https://github.com/microsoft/winget-pkgs)。 新增您最愛的工具，以便您和其他人使用。

## <a name="open-source-details"></a>開放原始碼詳細資訊

**winget** 工具是 GitHub 存放庫 [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/) 中提供的開放原始碼軟體。 用於建立用戶端的原始碼位於 [src 資料夾](https://github.com/microsoft/winget-cli/tree/master/src)。

**winget** 的原始碼包含在 Visual Studio 2019 C++ 解決方案中。 若要正確建立解決方案，請安裝[搭配 C++ 工作負載的 Visual Studio](https://visualstudio.microsoft.com/downloads/)。

我們歡迎您參與撰寫 GitHub 上的 **winget** 原始碼。 您必須先同意並簽署 Microsoft CLA。
