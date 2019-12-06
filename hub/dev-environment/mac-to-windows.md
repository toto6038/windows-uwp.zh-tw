---
title: 協助從 Mac （Unix）移至 Windows
description: 協助您從 Mac （Unix）轉換到 Windows 開發環境的指南，包括快速鍵對應和 Mac 和 Windows 之間不同概念的簡短總覽。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac 到 Windows、快速鍵對應、從 Unix 移至 Windows、從 Mac 轉換到 Windows、從 MacBook 移至介面、如何將 Windows 用於 Macintosh 使用者、從 Macintosh 切換到 Windows、協助變更開發環境、Mac OS X 到 Windows、協助從 Mac 移至電腦
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a4e71143730184db094df2a7e8f1416cbaf244c4
ms.sourcegitcommit: f5bb4e35d1373b982259e61547b3b1765da0e78c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881274"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>將您的開發環境從 Mac 變更為 Windows 的指南

下列秘訣和控制項對等專案應該可以協助您在 Mac 和 Windows （或 WSL/Linux）開發環境之間進行轉換。

針對應用程式開發，最接近 Xcode 的相當於[Visual Studio](https://visualstudio.microsoft.com)。 如果您覺得需要返回，還有一個[Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)版本。 針對跨平臺原始程式碼編輯（和大量外掛程式） [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432)是最受歡迎的選擇。

## <a name="keyboard-shortcuts"></a>鍵盤快速鍵

| **運算** | **Mac** | **視窗** |
|---------------|--------------------|---------------------|
| [複製] | 命令 + C | Ctrl+C |
| 剪下 | 命令 + X | Ctrl+X |
| 貼上 | 命令 + V | Ctrl+V |
| 復原 | 命令 + Z | Ctrl+Z |
| [儲存] | 命令 + S | Ctrl+S |
| 開啟 | 命令 + O | Ctrl+O |
| 鎖定電腦 | Command + Control + Q | WindowsKey + L |
| 顯示桌面 | 命令 + F3 | WindowsKey + D |
| 最小化視窗 | 命令 + M | WindowsKey + M |
| [搜尋] | 命令 + 空格鍵 | WindowsKey |
| 關閉使用中視窗 | 命令 + W | Control + W |
| 切換目前的工作 | 命令 + Tab | Alt+Tab |
| 儲存畫面（螢幕擷取畫面） | 命令 + Shift + 3 | WindowsKey + Shift + S |
| 儲存視窗 | 命令 + Shift + 4 | WindowsKey + Shift + S |
| 視圖專案資訊或屬性 | 命令 + I | Alt+Enter |
 | 選取所有項目 | 命令 + A | Ctrl+A |
| 在清單中選取一個以上的專案（非連續） | 命令，然後按一下每個專案 | 控制項，然後按一下每個專案 |
| 輸入特殊字元 | Option + 字元鍵 | Alt + 字元鍵|

## <a name="trackpad-shortcuts"></a>軌跡板快速鍵

注意：其中有些快捷方式需要「精確度軌跡板」，例如 Surface 裝置上的軌跡板，以及一些其他的協力廠商膝上型電腦。

 **運算** | **Mac** | **視窗** |
|---------------|--------------------|---------------------|
| Scroll | 雙手指垂直滑動 | 雙手指垂直滑動 |
| 縮放 | 兩個手指縮小和放大 | 兩個手指縮小和放大 |
| 在 views 之間向前和向後滑動 | 兩個手指橫向滑動 | 兩個手指橫向滑動 |
| 切換虛擬工作區 | 四個手指橫向滑動 | 四個手指橫向滑動 |
| 顯示目前開啟的應用程式 | 四個手指向上滑動 | 三個手指向上滑動 |
| 在應用程式之間切換 | 無 | 緩慢的三手指側刷 |
| 前往桌面 | 散佈四個手指 | 三向下輕觸 |
| 開啟 Cortana/行動中心 | 向右滑動兩個手指 | 三碰點 |
| 開啟額外資訊 | 三碰點 | 無 |
|顯示啟動控制板/啟動應用程式 | 使用四個手指縮小 | 利用四個手指 |

注意：在這兩個平臺上都可設定軌跡板選項。

## <a name="terminal-and-shell"></a>終端機和殼層

Windows 提供數個 Mac 終端機模擬器的替代方案。

1. Windows 命令列

Windows 命令列會接受 DOS 命令，而是 Windows 最常使用的命令列工具。 若要開啟它：按**WindowsKey + R**開啟 [**執行**] 方塊，然後輸入**Cmd** ，再按一下 **[確定]** 。 若要開啟系統管理員命令列，請輸入**cmd** ，然後按**Ctrl + Shift + enter**。

2. PowerShell

[Powershell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6)是一種以工作為基礎的命令列 shell 和指令碼語言，建置於 .net 上。 PowerShell 可協助系統管理員和使用者快速地將管理作業系統的工作自動化。 換句話說，這是一項功能強大的命令列，特別是系統管理員的愛。

順便一提，PowerShell[也適用于 Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6)。

3. 適用於 Linux 的 Windows 子系統 (WSL)

WSL 可讓您在 Windows 內執行 Linux shell。 這表示您可以根據選擇和安裝的特定 Linux 散發版本，執行*bash** 或其他 shell。 使用 WSL 可提供 Mac 使用者最熟悉的環境類型。 例如，**您將會**在目前目錄中列出檔案，而不是依照 Windows 命令列的方式來列出**目錄**。 若要深入瞭解安裝和使用 WSL，請參閱[適用于 windows 10 的 Windows 子系統 For Linux 安裝指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="apps-and-utilities"></a>應用程式和公用程式

 **應用程式** | **Mac** | **視窗** |
|---------------|--------------------|---------------------|
| 設定和喜好設定 | 系統喜好設定 | [設定] |
| 工作管理員 | 活動監視器 | [工作管理員] |
| 磁片格式化 | 磁片公用程式 | 磁碟管理 |
| 文字編輯 | TextEdit | 記事本 |
| 事件查看 | Console | 事件檢視器 |
| 尋找檔案/應用程式 | 命令 + 空格鍵 | Windows 鍵 |
