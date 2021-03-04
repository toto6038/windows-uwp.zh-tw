---
title: 協助從 Mac (Unix) 移至 Windows
description: 此指南可協助您從 Mac (Unix) 轉換至也包含快速鍵對應的 Windows 開發環境。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac 到 Windows, 快速鍵對應, 從 Unix 移至 Windows, 從 Mac 轉換至 Windows, 協助從 MacBook 移至 Surface, 如何為 Macintosh 使用者使用 Windows, 從 Macintosh 切換至 Windows, 協助變更開發環境, Mac OS X 至 Windows, 協助從 Mac 移至電腦
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: c23bceecc09677dbecbda7b4e4065377c514cc0d
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823942"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>將開發環境從 Mac 變更為 Windows 的指南

以下提示和控制項對等項目，可協助您在 Mac 和 Windows (或 WSL/Linux) 開發環境之間轉換。

若為應用程式開發，與 Xcode 最接近的對等項目是 [Visual Studio](https://visualstudio.microsoft.com)。 如果您需要返回，還有一個 [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/) 版本。 對於跨平台的原始程式碼編輯 (以及大量的外掛程式)，[Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) 是最熱門的選項。

## <a name="keyboard-shortcuts"></a>鍵盤快速鍵

| **作業** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 複製 | Command+C | Ctrl+C |
| 剪下 | Command+X | Ctrl+X |
| 貼上 | Command+V | Ctrl+V |
| 復原 | Command+Z | Ctrl+Z |
| 儲存 | Command+S | Ctrl+S |
| 開啟 | Command+O | Ctrl+O |
| 鎖定電腦 | Command+Control+Q | WindowsKey+L |
| 顯示桌面 | Command+F3 | WindowsKey+D |
| 開啟檔案瀏覽器 | Command+N | WindowsKey+E |
| 將視窗最小化 | Command+M | WindowsKey+M |
| 搜尋 | Command+空格鍵 | WindowsKey |
| 關閉使用中視窗 | Command+W | Control+W |
| 切換目前工作 | Command+Tab | Alt+Tab |
| 將視窗最大化至全螢幕 | Control+Command+F | WindowsKey+Up |
| 儲存螢幕 (螢幕擷取畫面) | Command+Shift+3 | WindowsKey+Shift+S |
| 儲存視窗 | Command+Shift+4 | WindowsKey+Shift+S |
| 檢視項目資訊或屬性 | Command+I | Alt+Enter |
 | 選取所有項目 | Command+A | Ctrl+A |
| 在清單中選取一個以上的專案 (非連續) | Command，然後按一下每個項目 | Control，然後按一下每個項目 |
| 鍵入特殊字元 | Option+ 字元鍵 | Alt+ 字元鍵|

## <a name="trackpad-shortcuts"></a>軌跡板快速鍵

注意：其中有些快捷方式需要「精確度軌跡板」，例如 Surface 裝置上的軌跡板，以及某些其他協力廠商筆記型電腦上的觸控板。

 **作業** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | 兩指垂直撥動 | 兩指垂直撥動 |
| 縮放 | 兩指向內和向外捏合 | 兩指向內和向外捏合 |
| 在檢視之間前後撥動 | 兩指橫向撥動 | 兩指橫向撥動 |
| 切換虛擬工作區 | 四指橫向撥動 | 四指橫向撥動 |
| 顯示目前開啟的應用程式 | 四指向上撥動 | 三指向上撥動 |
| 在應用程式之間切換 | 不適用 | 三指緩慢地橫向撥動 |
| 移至桌面 | 四指張開 | 三指向下撥動 |
| 開啟 Cortana / 控制中心 | 兩指從右側滑動 | 三指點選 |
| 開啟額外資訊 | 三指點選 | 不適用 |
|顯示啟動列 / 啟動應用程式 | 使用四指捏合 | 使用四指點選 |

注意：在這兩個平台上皆可設定軌跡板選項。

## <a name="command-line-shells-and-terminals"></a>命令列命令介面和終端機

Windows 支援數個命令列命令介面和終端機，其運作方式有些時候會與 Mac 的 BASH 命令介面和終端機模擬器應用程式 (例如終端機和 iTerm) 稍有不同。

### <a name="windows-shells"></a>Windows 命令介面

Windows 有兩個主要的命令列命令介面：

1. **[PowerShell](/powershell/scripting/overview?view=powershell-7)** - PowerShell 是跨平台工作自動化和組態管理架構，其組成為以 .NET 為建置基礎的命令列命令介面和指令碼語言。 使用 PowerShell，系統管理員、開發人員和進階使用者便可以快速控制並將工作自動化，這些工作可管理複雜的程序和環境與其執行所在作業系統的各個層面。 PowerShell 是[完全開放原始碼](https://github.com/powershell/powershell)，並因為它可跨平台，也[適用於 Mac 和 Linux](/powershell/scripting/install/installing-powershell?view=powershell-7)。

    **Mac 和 Linux BASH 命令介面使用者**：PowerShell 也支援您已經熟悉的許多命令別名。 例如：
    - 列出目前目錄的內容，使用：`ls`
    - 移動檔案，使用：`mv`
    - 移至新目錄，使用：`cd <path>`

    PowerShell 與 BASH 中的某些命令和引數不同。 若要深入了解，請在 PowerShell 中輸入：[`get-help`](/powershell/scripting/learn/ps101/02-help-system?view=powershell-7)，或查看文件中[相容性別名](/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7)。

    若要以系統管理員身分執行 PowerShell，請在 Windows [開始] 功能表中輸入 "PowerShell"，然後選取 [以系統管理員身分執行]。

2. **Windows 命令列 (Cmd)** ：Windows 仍會隨附傳統的命令提示字元 (和主控台，請參閱下文)，以提供與目前和舊版 MS-DOS 相容的命令和批次檔的相容性。 在執行現有/較舊的批次檔或命令列作業時，Cmd 非常有用，但一般而言，建議使用者學習並使用 PowerShell，因為 Cmd 目前維護中，而且未來將不會收到任何改良功能或新功能。

### <a name="linux-shells"></a>Linux 命令介面

現在可以安裝 Windows 子系統 Linux 版 (WSL)，以支援在 Windows 內執行 Linux 命令介面。 這表示您可以使用您所選擇的任何特定 Linux 發行版本 (直接整合在 Windows 內) 執行 **bash**。 使用 WSL 將提供 Mac 使用者最熟悉的環境類型。 例如，您會使用 **ls** 來列出目前目錄中的檔案，而非使用您在傳統 Windows Cmd 命令介面中使用的 **dir**。 若要了解安裝和使用 WSL 的詳細資訊，請參閱[適用於 Windows 10 的 Windows 子系統 Linux 版安裝指南](/windows/wsl/install-win10)。 可以使用 WSL 安裝在 Windows 上的 Linux 發行版本包括：

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [openSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

僅提供幾個例子。 您可以在 [WSL 安裝文件](/windows/wsl/install-win10#install-your-linux-distribution-of-choice)中進一步了解，並直接從 [Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools) 進行安裝。

## <a name="windows-terminals"></a>Windows 終端機

除了許多協力廠商產品，Microsoft 還提供兩個「終端機」，它們是可讓您存取命令列命令介面和應用程式的 GUI 應用程式。

1. **[Windows 終端機](/windows/terminal/)** ：「Windows 終端機」是一項全新、現代化、高度可設定的命令列終端機應用程式，提供非常高的效能、低延遲的命令列使用者體驗、多個索引標籤、分割視窗窗格、自訂主題和樣式、用於不同命令介面或命令列應用程式的多個「設定檔」，以及相當多的機會讓您設定並個人化您的命令列使用者體驗的許多層面。

    您可以使用 Windows 終端機來開啟索引標籤，其中連線至 PowerShell、WSL 命令介面 (例如 Ubuntu 或 Debian)、傳統 Windows 命令提示字元或任何其他命令列應用程式 (例如 SSH、Azure CLI、Git Bash)。

2. **[主控台](/windows/console/)** ：在 Mac 和 Linux 上，使用者通常會啟動其偏好的終端機應用程式，接著它會建立並連線至使用者的預設命令介面 (例如 BASH)。

    不過，由於長久以來的怪癖，Windows 使用者傳統上會啟動其命令介面，而 Windows 會自動啟動並連線 GUI 主控台應用程式。

    雖然使用者仍可以直接啟動命令介面並使用舊版 Windows 主控台，但強烈建議使用者改為安裝和使用 Windows 終端機，以體驗最佳、最快速、最具生產力的命令列體驗。

## <a name="apps-and-utilities"></a>應用程式和公用程式

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 設定與偏好設定 | 系統偏好設定 | Settings |
| 工作管理員 | 活動監視器 | 工作管理員 |
| 磁碟格式化 | 磁碟公用程式 | 磁碟管理 |
| 文字編輯 | TextEdit | 記事本 |
| 事件檢視 | 主控台 | 事件檢視器 |
| 尋找檔案/應用程式 | Command+空格鍵 | Windows 按鍵 |