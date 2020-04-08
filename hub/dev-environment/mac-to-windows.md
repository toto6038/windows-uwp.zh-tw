---
title: 協助從 Mac (Unix) 移至 Windows
description: 本指南可幫助您從 Mac (Unix) 轉換至 Windows 開發環境，包括快速鍵對應，以及 Mac 和 Windows 之間不同概念的簡要概述。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac 到 Windows, 快速鍵對應, 從 Unix 移至 Windows, 從 Mac 轉換至 Windows, 協助從 MacBook 移至 Surface, 如何為 Macintosh 使用者使用 Windows, 從 Macintosh 切換至 Windows, 協助變更開發環境, Mac OS X 至 Windows, 協助從 Mac 移至電腦
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218438"
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

## <a name="terminal-and-shell"></a>終端機和殼層

Windows 提供數個 Mac 終端機模擬器的替代方案。

1. Windows 命令列

Windows 命令列會接受 DOS 命令，而且是 Windows 最常用的命令列工具。 若要開啟，請執行下列動作：按 **WindowsKey+R** 以開啟 [執行]  方塊，然後鍵入 **cmd**，然後按一下 [確定]  。 若要開啟管理員命令列，請鍵入 **cmd**，然後按 **Ctrl+Shift+Enter**。

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) 是以工作為基礎的命令列和建置在 .NET 上的指令碼語言。 PowerShell 可協助系統管理員和使用者快速地將管理作業系統的工作自動化。 換句話說，這是功能強大的命令列，特別深受系統管理員的喜愛。

順便一提，PowerShell [也適用於 Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)。

3. 適用於 Linux 的 Windows 子系統 (WSL)

WSL 可讓您在 Windows 中執行 Linux 命令介面。 這表示您可以根據選擇和安裝的 Linux 發行版本，執行 *bash** 或其他命令介面。 使用 WSL 將提供 Mac 使用者最熟悉的環境類型。 例如，您會使用 **ls** 來列出目前目錄中的檔案，而非使用您在 Windows 命令列中使用的 **dir**。 若要了解安裝和使用 WSL 的詳細資訊，請參閱[適用於 Windows 10 的 Windows 子系統 Linux 版安裝指南](https://docs.microsoft.com/windows/wsl/install-win10)。

4. Windows 終端機 (預覽)

Windows 終端機是一種應用程式，結合各種來源的命令行工具和命令介面，包括傳統的 Windows 命令行、PowerShell，以及 Windows 子系統 Linux 版。 雖然目前仍處於預覽狀態，但已包含幾個有用的功能，例如，支援多個索引標籤、分割窗格、自訂主題和樣式，以及完整的 Unicode 支援。 可以從 [Windows 10 上的 Microsoft Store](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab) 安裝 Windows 終端機。

## <a name="apps-and-utilities"></a>應用程式和公用程式

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| 設定與偏好設定 | 系統偏好設定 | Settings |
| 工作管理員 | 活動監視器 | 工作管理員 |
| 磁碟格式化 | 磁碟公用程式 | 磁碟管理 |
| 文字編輯 | TextEdit | 記事本 |
| 事件檢視 | 主控台 | 事件檢視器 |
| 尋找檔案/應用程式 | Command+空格鍵 | Windows 按鍵 |
