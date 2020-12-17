---
title: 安裝 PowerToys
description: 安裝 Powertoy，這是使用可執行檔或封裝管理員 (WinGet、Chocolatey、內幕) 自訂 Windows 10 的一組公用程式。
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: 0f051d44609653029a1bab40d2978eca91a4499c
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97612099"
---
# <a name="install-powertoys"></a>安裝 PowerToys

有多種方式可安裝 Powertoy：

- **[Windows 可執行檔 .exe](#install-with-windows-executable-file)** 檔 *(建議的)*
- [Windows 封裝管理員](#install-with-windows-package-manager-preview) *(Preview)*
- *(未正式支援* 的 [社區驅動安裝工具](#community-driven-install-tools)) 

## <a name="requirements"></a>需求

- Windows 10 1803 (組建 17134) 或更新版本。
- [.Net Core 3.1 桌面運行](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer)時間。 Powertoy 安裝程式將會處理這項需求。

若要確定您的電腦符合這些需求，請選取 [ **⊞ Win** *(Windows 金鑰])*  +  **D**，然後輸入 **winver**，選取 **[確定]**，以檢查您的 Windows 10 版本和組建編號。 (或在 Windows 命令提示字元中輸入 `ver` 命令)。 您可以在 [**設定**] 功能表中 [更新至最新的 Windows 版本](ms-settings:windowsupdate)。

## <a name="install-with-windows-executable-file"></a>使用 Windows 可執行檔安裝

若要使用 Windows 可執行檔來安裝 Powertoy：

1. 造訪 [Microsoft Powertoy GitHub 版本頁面](https://github.com/microsoft/PowerToys/releases/)。
2. 流覽可用 Powertoy 穩定和實驗版本的清單。
3. 選取 [ **資產** ] 下拉式功能表，以顯示發行的檔案。
4. 選取檔案 `PowerToysSetup-0.##.#-x64.exe` 以下載 powertoy 可執行檔安裝程式。
5. 下載之後，請開啟可執行檔，並遵循安裝提示。

**目前這是建議的安裝方法。**

## <a name="install-with-windows-package-manager-preview"></a>使用 Windows 封裝管理員 (Preview 安裝) 

若要使用 Windows 封裝管理員 (WinGet) preview 來安裝 Powertoy：

1. 從 [Windows 封裝管理員](https://github.com/microsoft/winget-cli/releases)下載 powertoy。
2. 從命令列/PowerShell 執行下列命令：

```powershell
WinGet install powertoys
```

## <a name="community-driven-install-tools"></a>社區驅動的安裝工具

這些以社區驅動的替代安裝方法未正式支援，而且 Powertoy 團隊不會更新或管理這些套件。

### <a name="install-with-chocolatey"></a>使用 Chocolatey 安裝

若要使用 [Chocolatey](https://chocolatey.org/)安裝 powertoy，請從命令列/PowerShell 執行下列命令：

```powershell
choco install powertoys
```

若要升級 Powertoy，請執行：

```powershell
choco upgrade powertoys
```

如果您在安裝/升級時遇到問題，請流覽 [Chocolatey.org 上的 powertoy 套件](https://chocolatey.org/packages/powertoys) ，並依照 [Chocolatey](https://chocolatey.org/docs/package-triage-process)分級程式進行操作。

### <a name="install-with-scoop"></a>使用內幕安裝

若要使用 [內幕](https://scoop.sh/)安裝 powertoy，請從命令列/PowerShell 執行下列命令：

```powershell
scoop install powertoys
```

若要更新 Powertoy，請從命令列/PowerShell 執行下列命令：

```powershell
scoop update powertoys
```

如果您在安裝/更新時遇到問題，請在 [GitHub 上的內幕](https://github.com/lukesampson/scoop/issues)存放庫中提出問題。