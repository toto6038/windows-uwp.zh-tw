---
title: Windows 封裝管理員
description: ''
author: denelon
ms.author: denelon
ms.date: 05/03/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 4f7a6533d3dea9c304e9be7d8e689ab537a46449
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580095"
---
# <a name="windows-package-manager-preview"></a>Windows 封裝管理員 (預覽)

[!INCLUDE [preview-note](../includes/package-manager-preview.md)]

Windows 封裝管理員是一套完整的[封裝管理員解決方案](#understanding-package-managers)，其中包含命令列工具，以及在 Windows 10 上安裝應用程式的一組服務。

## <a name="windows-package-manager-for-developers"></a>適用於開發人員的 Windows 封裝管理員

開發人員會使用 **winget** 命令列工具來探索、安裝、升級、移除和設定策劃的應用程式集。 安裝之後，開發人員可以透過 Windows 終端機、PowerShell 或命令提示字元存取 **winget**。

如需詳細資訊，請參閱[使用 winget 工具來安裝和管理應用程式](winget/index.md)。

## <a name="windows-package-manager-for-isvs"></a>適用於 ISV 的 Windows 封裝管理員

獨立軟體廠商 (ISV) 可以使用 Windows 封裝管理員作為軟體封裝 (包含其工具和應用程式) 的散發通道。 若要將軟體封裝 (包含 .msix、.msi 或 .exe 安裝程式) 提交至 Windows 封裝管理員，我們在 GitHub 上提供開放原始碼的 **Microsoft 社群封裝資訊清單存放庫**，可讓 ISV 上傳[封裝資訊清單](package/manifest.md)，以將其軟體封裝納入 Windows 封裝管理員。 資訊清單會自動進行驗證，也可以透過手動方式檢查。

如需詳細資訊，請參閱[將封裝提交至 Windows 封裝管理員](package/repository.md)。

## <a name="understanding-package-managers"></a>了解封裝管理員

封裝管理員是一種系統或一組用來自動安裝、升級、設定和使用軟體的工具。 大部分封裝管理員都是為了探索和安裝開發人員工具所設計的。

在理想情況下，針對指定專案的解決方案開發，開發人員會使用封裝管理員來指定所需工具的必要條件。 然後，封裝管理員會遵循陳述的指示來安裝和設定工具。 封裝管理員可減少準備環境的時間，並協助確保電腦上會安裝相同版本的封裝。

第三方封裝管理員可以利用 [Microsoft 社群封裝資訊清單存放庫](package/repository.md)來增加其軟體目錄的大小。

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理軟體封裝](winget/index.md)
* [將封裝提交至 Windows 封裝管理員](package/index.md)