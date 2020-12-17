---
title: Windows 10 的 Powertoy 執行公用程式
description: 適用于 power 使用者的快速啟動程式，其中包含一些額外的功能，而不會犧牲效能。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9be1d54946ec2286d95dbe7d4518a631efd471e9
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618527"
---
# <a name="powertoys-run-utility"></a>Powertoy 執行公用程式

Powertoy Run 是適用于 power users 的快速啟動程式，其中包含一些額外的功能，而不會犧牲效能。 它是開放原始碼，而且是適用于其他外掛程式的模組化。

若要使用 powertoy 執行，請選取<kbd>Alt</kbd> + <kbd>空格鍵</kbd>，然後開始鍵入！

*如果快捷方式不是您想要的，別擔心，它可在設定中完整設定。*

![Powertoy 執行示範開啟應用程式](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>需求

- Windows 10 版本1903或更高版本
- 安裝之後，必須在背景中啟用和執行 Powertoy，此公用程式才能運作

## <a name="features"></a>特性

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
  | 停用磁片磁碟機偵測警告 | 警告：如果您所有的磁片磁碟機都未建立索引，將不再顯示 |

## <a name="keyboard-shortcuts"></a>鍵盤快速鍵

  | **捷徑** | **動作** |
  | --- | --- |
  | Alt + 空格鍵 | 開啟或隱藏 Powertoy 執行 |
  | Esc | 隱藏 Powertoy 執行 |
  | Ctrl+Shift+Enter |  (僅適用于應用程式) 以系統管理員身分開啟選取的應用程式 |
  | CTRL+SHIFT+E |  (僅適用于應用程式和檔案) 在檔案總管中開啟包含資料夾 |
  | Ctrl+C |  (僅適用于) 複製路徑位置的資料夾和檔案 |
  | 索引標籤 | 流覽搜尋結果和內容功能表按鈕 |

## <a name="action-key"></a>動作金鑰

這些會強制 Powertoy 只執行到目標外掛程式。

  | **動作金鑰** | **動作** |
  | --- | --- |
  | `=` | 只有計算機。 範例 `=2+2` |
  | `?` | 僅搜尋檔案。 `?road`要尋找的範例`roadmap.txt` |
  | `.` | 只搜尋已安裝的應用程式。 `.code`取得 Visual Studio Code 的範例 |
  | `//` | 僅限 Url。 `//docs.microsoft.com`讓您的預設瀏覽器移至的範例https://docs.microsoft.com |
  | `<` | 僅執行進程。 `<outlook`尋找包含 outlook 的所有進程的範例 |
  | `>` | 僅限 Shell 命令。 `>ping localhost`執行 ping 查詢的範例 |

## <a name="indexer-settings"></a>索引子設定

如果索引子設定未設定為涵蓋所有磁片磁碟機，您將會收到下列警告：

![Powertoy 執行索引子警告](../images/pt-run-warning.png)

您可以關閉 [Powertoy] 設定中的警告，或選取警告來展開正在編制索引的磁片磁碟機。 選取警告之後，會開啟 [搜尋 Windows] Windows 10 設定] 選項。

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
