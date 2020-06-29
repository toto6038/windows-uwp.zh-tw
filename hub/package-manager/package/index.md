---
title: 將封裝提交至 Windows 封裝管理員
description: 您可以使用 Windows 封裝管理員作為軟體封裝 (包含工具和應用程式) 的散發通道。
ms.date: 04/29/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: ae9c9039154e2a576a691a01d64abcf8c9029c1c
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334604"
---
# <a name="submit-packages-to-windows-package-manager"></a>將封裝提交至 Windows 封裝管理員

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

如果您是獨立軟體廠商 (ISV)，您可以使用 Windows 封裝管理員作為軟體封裝 (包含工具和應用程式) 的散發通道。 Windows 封裝管理員目前支援下列格式的安裝程式：MSIX、MSI 和 EXE。

若要將軟體封裝提交至 Windows 封裝管理員，請遵循下列步驟：

1. [建立封裝資訊清單，以提供應用程式的相關資訊](manifest.md)。 資訊清單是遵循 Windows 封裝管理員結構描述的 YAML 檔案。
2. [將資訊清單提交至 Windows 封裝管理員存放庫](repository.md)。 這是 GitHub 上的開放原始碼存放庫，其中包含 **winget** 工具可存取的資訊清單集合。

## <a name="related-topics"></a>相關主題

* [使用 winget 工具](../winget/index.md)
* [建立封裝資訊清單](manifest.md)
* [將您的資訊清單提交至存放庫](repository.md)