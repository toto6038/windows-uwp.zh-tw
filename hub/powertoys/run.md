---
title: Windows 10 的 Powertoy 執行公用程式
description: 適用于 power 使用者的快速啟動程式，其中包含一些額外的功能，而不會犧牲效能。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d9c67b38f9a7c7729c0f4839a327a2527aa116d
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2021
ms.locfileid: "107218133"
---
# <a name="powertoys-run-utility"></a>Powertoy 執行公用程式

Powertoy Run 是適用于 power users 的快速啟動程式，其中包含一些額外的功能，而不會犧牲效能。 其為開放原始碼，並可針對其他外掛程式進行模組化。

若要使用 powertoy 執行，請選取<kbd>Alt</kbd> + <kbd>空格鍵</kbd>，然後開始鍵入！

*如果快捷方式不是您想要的，別擔心，它可在設定中完整設定。*

![Powertoy 執行示範開啟應用程式](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>規格需求

- Windows 10 版本1903或更高版本
- 安裝之後，必須在背景中啟用和執行 Powertoy，此公用程式才能運作

## <a name="features"></a>功能

Powertoy 執行功能包括：

- 搜尋應用程式、資料夾或檔案

- 搜尋正在執行的進程 (先前稱為 [WindowWalker](https://github.com/betsegaw/windowwalker/)) 

- 具有鍵盤快速鍵的可點擊按鈕 (例如 [ *以系統管理員身分開啟* ] 或 [ *開啟包含的資料夾* ]) 

- 例如，使用 (叫用 Shell 外掛程式 `>` `> Shell:startup` 將會開啟 Windows 開機檔案夾) 

- 使用計算機進行簡單的計算

## <a name="settings"></a>設定

您可以在 [Powertoy 設定] 功能表中找到下列執行選項。

  | **設定** |**動作** |
  | --- | --- |
  | 開啟 Powertoy 執行 | 定義用來開啟/隱藏 Powertoy 執行的鍵盤快速鍵 |
  | 略過全螢幕模式的快捷方式 |  在全螢幕 (F11) 的情況下，執行將不會與快捷方式一起參與 |
  | 結果數目上限 |  顯示而不需要滾動的結果數目上限 |
  | 在啟動時清除先前的查詢 | 啟動時，不會反白顯示先前的搜尋 |
  | 停用磁片磁碟機偵測警告 | 警告：如果您所有的磁片磁碟機都未建立索引，將不再顯示。 |

## <a name="keyboard-shortcuts"></a>鍵盤快速鍵

  | **捷徑** | **動作** |
  | --- | --- |
  | Alt + 空格鍵 | 開啟或隱藏 Powertoy 執行 |
  | Esc | 隱藏 Powertoy 執行 |
  | Ctrl+Shift+Enter |  (僅適用于應用程式) 以系統管理員身分開啟選取的應用程式 |
  | CTRL+SHIFT+E |  (僅適用于應用程式和檔案) 在檔案總管中開啟包含資料夾 |
  | Ctrl+C |  (僅適用于) 複製路徑位置的資料夾和檔案 |
  | 索引標籤 | 流覽搜尋結果和內容功能表按鈕 |

## <a name="action-keys"></a>動作按鍵

這些預設啟用片語會強制 Powertoy 只執行到目標外掛程式。

  | **動作金鑰** | **動作** |
  | --- | --- |
  | `=` | 只有計算機。 範例 `=2+2` 。 |
  | `?` | 僅搜尋檔案。 `?road`要尋找 `roadmap.txt` 的範例。 |
  | `.` | 僅限已安裝的程式。 `.code`取得 Visual Studio Code 的範例。 請參閱 [程式參數](#program-parameters) ，以取得將參數加入程式啟動時的選項。 |
  | `//` | 僅限 Url。 `//docs.microsoft.com`讓您的預設瀏覽器移至的範例 https://docs.microsoft.com 。 |
  | `<` | 僅執行進程。 `<outlook`尋找包含 outlook 的所有進程的範例。 |
  | `>` | 僅限 Shell 命令。 `>ping localhost`執行 ping 查詢的範例。 |
  | `:` | 僅限登錄機碼。 搜尋 HKEY_CURRENT_USER 登錄機碼的範例 `:hkcu` 。 |
  | `!` | 僅限 Windows 服務。 `!alg`搜尋要啟動或停止的應用層閘道服務的範例。 |
  | `{` | Visual Studio Code 先前開啟的工作區、遠端電腦 (SSH 或 Codespaces) 和容器。 `{powertoys`搜尋路徑中包含 ' powertoy ' 之工作區的範例。 此外掛程式預設為關閉。

## <a name="system-commands"></a>系統命令

Powertoy 執行會啟用一組可以執行的系統層級動作。

  | **動作金鑰**   |   **動作** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | 關閉電腦 |
  | `Restart` | 重新開機電腦 |
  | `Sign Out` | 將目前的使用者登入 |
  | `Lock` | 鎖定電腦 |
  | `Sleep` | 睡眠電腦 |
  | `Hibernate` | 休眠電腦 |
  | `Empty Recycle Bin` | 清空回收站 |

## <a name="plugin-manager"></a>外掛程式管理員

[Powertoy 回合設定] 功能表包含外掛程式管理員，可讓您啟用/停用目前可用的各種外掛程式。 藉由選取和展開區段，您可以自訂每個外掛程式所使用的啟用片語。 此外，您可以選取外掛程式是否出現在全域結果中，以及設定可用的其他外掛程式選項。 

## <a name="program-parameters"></a>程式參數

Powertoy 執行程式外掛程式可讓您在啟動應用程式時加入程式引數。 程式引數必須遵循程式的命令列介面所定義的預期格式。

例如，當啟動 Visual Studio Code 時，您可以指定要開啟的資料夾：

`Visual Studio Code -- C:\myFolder`

Visual Studio Code 也支援一組 [命令列參數](https://code.visualstudio.com/docs/editor/command-line)，可搭配 powertoy Run to 中的對應引數使用，例如，查看檔案之間的差異：

`Visual Studio Code -d C:\foo.txt C:\bar.txt` 

如果未選取 [在全域結果中包含] 程式外掛程式的選項，請務必在預設的情況下包含啟用片語， `.` 以叫用外掛程式的行為：

`.Visual Studio Code -- C:\myFolder`

## <a name="monitor-positioning"></a>監視定位

如果有多個監視正在使用中，您可以在 [設定] 功能表中設定適當的啟動行為，以在所需的監視器上啟動 Powertoy 回合。 選項包括開啟：

- 主要監視
- 使用滑鼠游標進行監視
- 以焦點視窗監視

![Powertoy 執行監視選取專案](../images/pt-run-monitor.png)

## <a name="windows-search-settings"></a>Windows Search 設定

如果 Windows Search 外掛程式未設定為涵蓋所有磁片磁碟機，您將會收到下列警告：

![Powertoy 執行索引子警告](../images/pt-run-warning.png)

您可以關閉 Windows Search 的 [Powertoy 執行外掛程式管理員] 選項中的警告，或選取警告來展開正在編制索引的磁片磁碟機。 選取警告之後，會開啟 [搜尋 Windows] 選項功能表 Windows 10 設定]。

![編制索引設定](../images/pt-run-indexing.png)

在此 [搜尋視窗] 功能表中，您可以：

- 選取 [增強] 模式可讓您在 Windows 10 機上的所有磁片磁碟機上進行索引編制。
- 指定要排除的檔案夾路徑。
- 選取功能表選項底部附近的 [先進搜尋索引子設定] (，) 以設定 Advanced index 設定、新增或移除搜尋位置、編制加密檔案的索引等。

![Advanced 索引設定](../images/pt-run-indexing-advanced.png)

## <a name="known-issues"></a>已知問題

如需所有已知問題和建議的清單，請參閱 [GitHub 上的 powertoy 產品](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3AProduct-Launcher)存放庫問題。

## <a name="attribution"></a>Attribution

- [Wox](https://github.com/Wox-launcher/Wox/)

- [Beta Tadele 的視窗遍歷程式](https://github.com/betsegaw/windowwalker)
