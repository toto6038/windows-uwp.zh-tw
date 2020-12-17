---
title: Windows 10 的 Powertoy 系統管理員模式
description: 若要讓 Powertoy 使用在更高的管理員模式下執行的應用程式，Powertoy 也必須在系統管理員模式中執行。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2eb514c25c135f8d58641232523a32f5d35153d4
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618537"
---
# <a name="powertoys-running-with-administrator-elevated-permissions"></a>以系統管理員提升許可權執行的 Powertoy

如果您是以系統管理員身分執行任何應用程式 (也稱為提高許可權) ，當提高許可權的應用程式處於焦點或嘗試與 FancyZones 等 Powertoy 功能互動時，Powertoy 可能無法正常運作。 您也可以藉由系統管理員的身分執行 Powertoy 來解決此問題。

## <a name="options"></a>選項

有兩個選項可讓您 Powertoy 以系統管理員身分執行的應用程式 (以較高的許可權) ：

- **[建議]**：當偵測到提高許可權的進程時，powertoy 會顯示提示。 開啟 [ **Powertoy 設定**]。 在 [ **一般** ] 索引標籤中，選取 [以系統管理員身分重新開機]。

- 在 **Powertoy 設定** 中啟用 [一律以系統管理員身分執行]。

## <a name="run-as-administrator-elevated-processes-explained"></a>以系統管理員提高許可權的進程執行說明

Windows 應用程式預設會在 **使用者模式下** 執行。 若要在系統 **管理模式下** 執行應用程式，或以較 *高的進程* 執行，則表示應用程式將會以額外的作業系統存取權執行。

在系統管理模式下執行應用程式或程式的最簡單方式，就是以滑鼠右鍵按一下程式，然後選取 [以 **系統管理員身分執行**]。 如果目前的使用者不是系統管理員，則 Windows 會要求系統管理員使用者名稱和密碼。

大部分的應用程式不需要以較高的許可權執行。 但需要系統管理員許可權的常見案例是執行某些 PowerShell 命令或編輯登錄。

如果您看到這個提示 (使用者存取控制提示) ，則應用程式會要求系統管理員層級提升許可權：

![Windows 提高權限提示螢幕擷取畫面](../images/pt-admin-prompt.png)

在提高許可權的命令列中，通常會將標題 "Administrator" 附加至標題列。

![Windows 管理命令列螢幕擷取畫面](../images/pt-admin-terminal.png)

## <a name="support-for-admin-mode-with-powertoys"></a>使用 Powertoy 的管理模式支援

當與以系統管理員模式執行的其他應用程式互動時，Powertoy 只需要更高的系統管理員許可權。 如果這些應用程式處於焦點，Powertoy 可能不會運作，除非它也已提高許可權。

以下是我們將無法在下列情況中使用的兩個案例：

- 攔截特定類型的鍵盤筆劃
- 調整視窗大小/移動

### <a name="affected-powertoys-utilities"></a>受影響的 Powertoy 公用程式

在下列情況下，可能需要系統管理員模式許可權：

- FancyZones
  - 貼上提高許可權的視窗 (例如，將命令提示字元) 到別致的區域
  - 將提高許可權的視窗移至不同的區域
- 快速鍵指南
  - 顯示快捷方式
- 鍵盤 remapper
  - 金鑰重新對應按鍵
  - 全域層級快捷方式重新對應
  - 以應用程式為目標的快捷方式重新對應
- Powertoy 執行
  - 顯示快捷方式
