---
title: winget validate 命令
description: 驗證資訊清單檔案，以將軟體提交至 GitHub 上的 Microsoft 社群封裝資訊清單存放庫。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a5772e144a4226be9fbb4a4949aaac1e3d4408e6
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824929"
---
# <a name="validate-command-winget"></a>validate 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 **validate**命令會驗證[資訊清單檔案](../package/manifest.md)，以將軟體提交至 GitHub 上的 **Microsoft 社群封裝資訊清單存放庫**。 資訊清單必須是遵循[規格](https://github.com/microsoft/winget-pkgs/YamlSpec.md)的 YAML 檔案。

## <a name="usage"></a>使用方式

`winget validate [--manifest] \<manifest>`

## <a name="arguments"></a>引數

下列是可用的引數。

| 引數  | 說明 |
|--------------|-------------|
| **--manifest** |  要驗證的資訊清單路徑。 |
| **-?, --help** |  取得此命令的其他說明 |

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理應用程式](index.md)
* [將封裝提交至 Windows 封裝管理員](../package/index.md)
