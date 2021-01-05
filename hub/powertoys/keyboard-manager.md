---
title: Windows 10 的 Powertoy 鍵盤管理員公用程式
description: 此公用程式可讓使用者重新定義鍵盤上的按鍵
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb17cd5a7ad76728e6b063f76369c8d194a5e12c
ms.sourcegitcommit: 1a997d7e0100e58886150f9fba33d7b205f41df1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/04/2021
ms.locfileid: "97865463"
---
# <a name="keyboard-manager-utility"></a>鍵盤管理員公用程式

Powertoy 鍵盤管理員可讓您重新定義鍵盤上的按鍵。

例如，您可以在鍵盤上交換字母<kbd>D</kbd>的字母<kbd>A</kbd> 。 當您選取 <kbd>a</kbd> 鍵時，就會顯示 <kbd>D</kbd> 。

![Powertoy 鍵盤管理員重新對應金鑰螢幕擷取畫面](../images/powertoys-keyboard-remap.png)

您也可以交換快捷方式按鍵組合。 例如，快速鍵（ <kbd>Ctrl</kbd> + <kbd>C</kbd>）將會在 Microsoft Word 中複製文字。 您可以使用 powertoy 鍵盤管理員公用程式來交換<kbd>⊞ Win</kbd> + <kbd>C</kbd>) 的快捷方式。 現在， <kbd>⊞ Win</kbd> + <kbd>C</kbd>) 將複製文字。 如果您未在 Powertoy 鍵盤管理員中指定目標應用程式，則會在 Windows 之間全域套用快捷方式交換。

Powertoy 鍵盤管理員必須啟用 (Powertoy 在) 背景中執行的，才能套用重新對應的金鑰和快捷方式。 如果 Powertoy 不在執行中，則不會再套用金鑰重新對應。

![Powertoy 鍵盤管理員重新對應快速鍵螢幕擷取畫面](../images/powertoys-keyboard-shortcuts.png)

> [!NOTE]
> 有一些為作業系統保留的快速鍵，無法取代。 無法重新對應的索引鍵包括：
> - `⊞ Win`+`L`和無法重新對應， `Ctrl`  +  `Alt`  +  `Del` 因為它們是由 Windows OS 所保留。
> - `Fn`在大部分的情況下， (函式) 索引鍵不能重新對應 () 。 `F1` - `F12` (和) 的索引 `F13` - `F24` 鍵都可以對應。
> - `Pause` 只會傳送 sngle 的 keydown 事件。 例如，將它與倒退鍵和按下 + 按住的對應只會刪除單一字元。

## <a name="settings"></a>設定

若要建立與鍵盤管理員的對應，您可以選擇下列選項：

- 選取 [重新對應索引<kbd>鍵] 以</kbd>啟動重新對應的鍵盤設定視窗
- 選取 [重新對應<kbd>快捷方式</kbd>]，啟動重新對應快速鍵設定視窗

## <a name="remap-keys"></a>重新對應索引鍵

若要重新對應索引鍵，將其變更為新的值，請使用 [重新對應索引 <kbd>鍵</kbd> ] 按鈕啟動 [重新對應鍵盤設定] 視窗。 第一次啟動時，不會顯示預先定義的對應。 您必須選取 <kbd>+</kbd> 按鈕以新增重新對應。

出現新的重新對應資料列之後，請在 [金鑰] 資料行中選取您想要 ***變更** 其輸出的索引鍵。 在 [對應到] 資料行中，選取要指派的新索引鍵值。

例如，如果您想要按 <kbd>A</kbd> 並出現 <kbd>B</kbd>  ：

- 機碼： "A"
- 對應至： "B"

若要交換 "A" 和 "B" 索引鍵之間的索引鍵位置，請使用下列方式新增另一個重新對應：

- 機碼： "B"
- 對應至： "A"

![鍵盤對應鍵螢幕擷取畫面](../images/powertoys-keyboard-remap-a-b.png)

## <a name="key-to-shortcut"></a>快速鍵

若要將索引鍵重新對應到快速鍵 (組合) ，請在 [對應至] 資料行中輸入快速鍵組合。

例如，如果您想要選取 "C" 鍵，並讓它產生 "Ctrl + V"：

- 機碼： "C"
- 對應至： "Ctrl + V"

> [!NOTE]
> 即使在另一個快捷方式中使用重新對應的金鑰，也會維持金鑰重新對應。 例如，輸入快速鍵 "Alt + C" 會產生 "Alt + Ctrl + V"，因為 C 鍵已重新對應到 "Ctrl + V"。

## <a name="remap-shortcuts"></a>重新對應快速鍵

若要重新對應快速鍵組合（例如 "Ctrl + v"），請選取 [重新對應 <kbd>快捷</kbd> 方式以啟動重新對應快速鍵設定] 視窗。

第一次啟動時，不會顯示預先定義的對應。 您必須選取 <kbd>+</kbd> 按鈕以新增重新對應。

出現新的重新對應資料列之後，請在 [快速鍵] 資料行中選取您想要 _*_變更_*_ 其輸出的索引鍵。 在 [對應到] 資料行中，選取要指派的新快速鍵值。

例如， <kbd>Ctrl +</kbd>的快捷方式會 + <kbd></kbd>複製您選取的文字。 若要重新對應該快速鍵以使用 <kbd>Alt</kbd> 鍵，而不是 <kbd>Ctrl</kbd> 鍵：

- 快速鍵： "Ctrl" + "C"
- 對應至： "Alt" + "C"

![鍵盤對應的快捷方式螢幕擷取畫面](../images/powertoys-keyboard-remap-shortcut.png)

重新對應快速鍵時，有幾個要遵循的規則 (這些規則只適用于 [快速鍵] 資料行) ：

- 快速鍵必須以輔助按鍵開頭： <kbd>Ctrl</kbd>、 <kbd>Shift</kbd>、 <kbd>Alt</kbd>或 <kbd>⊞ Win</kbd>
- 快速鍵的結尾必須是動作機碼 (所有的非輔助按鍵) ： A、B、C、1、2、3等等。
- 快速鍵不可超過3個索引鍵  

## <a name="remap-a-shortcut-to-a-single-key"></a>將快捷方式重新對應至單一索引鍵

您可以將快捷方式 (按鍵組合) 重新對應到單鍵按鍵組合。

例如，若要取代快速鍵<kbd>⊞ Win</kbd>  +  <kbd>D</kbd> (顯示/隱藏 Windows 桌面應用程式) 使用單鍵按鍵， <kbd>Alt</kbd>：

- 快速鍵： "⊞ Win" (Windows 鍵) + "D"
- 對應至： "Alt"

> [!NOTE]
> 即使在另一個快捷方式中使用重新對應的金鑰，也會維持快捷方式重新對應。 例如，輸入快捷方式 "Alt" + "Tab"，在重新對應上述的 "Alt" 鍵之後，會產生 "⊞ Win" + "D" + "Tab"。

## <a name="app-specific-shortcuts"></a>應用程式特定的快捷方式

鍵盤管理員可讓您將特定應用程式的快捷方式重新對應 (而不是跨 Windows) 。

例如，在 Outlook 電子郵件應用程式中，預設會設定 "Ctrl + E" 快速鍵來搜尋電子郵件。 如果您想要改為設定 "Ctrl + F" 來搜尋您的電子郵件 (而不是依預設將電子郵件轉寄) ，您可以將快捷方式設定為 [Outlook]，並將其設定為「目標應用程式」。

鍵盤管理員會使用進程名稱 (不是應用程式名稱) 到目標應用程式。 例如，Microsoft Edge 設定為 "x-msedge-clientid" (進程名稱) ，而不是 "Microsoft Edge" (應用程式名稱) 。 若要尋找應用程式的處理常式名稱，請開啟 PowerShell 並輸入命令， `get-process` 或開啟命令提示字元，然後輸入命令 `tasklist` 。 這會針對您目前開啟的所有應用程式，產生進程名稱的清單。 以下是一些熱門應用程式進程名稱的清單。

  | _ *應用程式**   | **程序名稱**|
  | ------------------| --------------|
  | Microsoft Edge    |  msedge.exe   |
  | OneNote           |  onenote.exe  |
  | Outlook           |  outlook.exe  |
  | Teams             |  Teams.exe    |
  | Adobe Photoshop   |  Photoshop.exe|
  | 檔案總管     |  explorer.exe |
  | Spotify 音樂     |  spotify.exe  |
  | Google Chrome     |  chrome.exe   |
  | Excel             |  excel.exe    |
  | Word              |  winword.exe  |
  | Powerpoint        |  powerpnt.exe |

### <a name="keys-that-cannot-be-remapped"></a>無法重新對應的索引鍵

某些快速鍵不允許重新對應。 其中包含：

- <kbd>Ctrl</kbd> +<kbd>Alt</kbd> + <kbd>Del</kbd> (interupt 命令) 
- <kbd>⊞勝利</kbd> +<kbd>L</kbd> (鎖定您的電腦) 
- 在大部分的情況下，無法將函式索引鍵（ <kbd>Fn</kbd>）重新對應 () 但可以對應<kbd>F1</kbd> - <kbd>F12</kbd> 。

## <a name="how-to-select-a-key"></a>如何選取金鑰

若要選取要重新對應的索引鍵或快捷方式，您可以：

- 使用 [ <kbd>類型金鑰</kbd> ] 按鈕。
- 使用下拉式功能表。

一旦您選取 [ <kbd>類型金鑰]/[快捷方式</kbd> ] 按鈕，就會出現一個對話方塊，讓您可以使用鍵盤來輸入按鍵或快捷方式。 當您滿意輸出之後，請按住 <kbd>Enter</kbd> 以繼續。 如果您想要離開對話，請按住 <kbd>Esc</kbd> 按鈕。

使用下拉式功能表，您可以使用索引鍵名稱進行搜尋，而其他下拉式值則會在進行時顯示。 不過，您不能在下拉式功能表開啟時使用類型金鑰功能。

## <a name="orphaning-keys"></a>損壞索引鍵

損壞金鑰表示您會將它對應到另一個索引鍵，而不再有任何對應的索引鍵。

例如，如果索引鍵從-> B 重新對應，則鍵盤上就不會再有產生的金鑰。

若要修正此問題，請使用 + 建立另一個對應的重新對應金鑰，以產生。為了確保不會發生這種情況，系統會顯示任何孤立金鑰的警告。

![Powertoy 鍵盤管理員孤立金鑰](../images/powertoys-keyboard-remap-orphaned.png)

## <a name="frequently-asked-questions"></a>常見問題集

### <a name="i-remapped-the-wrong-keys-how-can-i-stop-it-quickly"></a>我重新對應了錯誤的按鍵，如何快速地停止？

若要讓按鍵重新對應正常運作，Powertoy 必須在背景中執行，而且必須啟用鍵盤管理員。 若要停止重新對應的金鑰，請關閉 Powertoy 或停用 Powertoy 設定中的鍵盤管理員。

### <a name="can-i-use-keyboard-manager-at-my-log-in-screen"></a>我可以在我的登入畫面中使用鍵盤管理員嗎？

否，只有當 Powertoy 正在執行，而且沒有在任何密碼畫面（包括以系統管理員身分執行）上運作時，才可使用鍵盤管理員。

### <a name="do-i-have-to-turn-off-my-computer-for-the-remapping-to-take-effect"></a>我是否必須關閉電腦，重新對應才會生效？

否，重新對應應該會在按下 **Apply** 時立即進行。

### <a name="where-are-the-maclinux-profiles"></a>什麼是 Mac/Linux 設定檔？

目前不包含 Mac 和 Linux 設定檔。

### <a name="will-this-work-on-video-games"></a>這是否適用于影片遊戲？

這取決於遊戲存取您金鑰的方式。 某些鍵盤 Api 無法與鍵盤管理員搭配使用。

### <a name="will-remapping-work-if-i-change-my-input-language"></a>如果我變更輸入語言，會重新對應工作嗎？

是的。 現在，如果您在英文 (US) 鍵盤上重新 <kbd>對應到</kbd> <kbd>B</kbd> ，然後將語言設定變更為法文， <kbd>請在法文鍵盤上輸入，</kbd> <kbd>在法文 (鍵盤</kbd> 上輸入，) 會產生 <kbd>B</kbd>，這與 Windows 處理多語系輸入的方式一致。

## <a name="troubleshooting"></a>疑難排解

如果您嘗試重新對應機碼或快捷方式，而且發生問題，可能是下列其中一個問題：

- **以系統管理員身分執行：** 如果該視窗在系統管理員中執行 (提升) 模式，且 Powertoy 不是以系統管理員身分執行，則重新對應將無法在應用程式/視窗上運作。 請嘗試以系統管理員身分執行 Powertoy。

- **不要攔截金鑰：** 鍵盤管理員會攔截鍵盤勾點，以重新對應您的金鑰。 某些也會進行這種情況的應用程式可能會干擾鍵盤管理員。 若要修正此問題，請移至 [設定] 並停用，然後再重新啟用鍵盤管理員。

## <a name="known-issues"></a>已知問題

- [未正確切換 Caps light 指標](https://github.com/microsoft/PowerToys/issues/1692)

- [重新對應無法運作 FancyZones 和快捷方式指南](https://github.com/microsoft/PowerToys/issues/3079)

- [重新對應按鍵（例如 Win、Ctrl、Alt 或 Shift）可能會中斷手勢和某些特殊按鈕](https://github.com/microsoft/PowerToys/issues/3703)

請參閱開啟的 [鍵盤管理員問題](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3A%22Product-Keyboard+Shortcut+Manager%22)清單。
