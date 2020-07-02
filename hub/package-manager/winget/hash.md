---
title: winget hash 命令
description: 產生安裝程式的 SHA256 雜湊。
ms.date: 04/28/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4b6599a2b538829c6d9107b20f5f22d22f646542
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334566"
---
# <a name="hash-command-winget"></a>hash 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 **hash** 命令會產生安裝程式的 SHA256 雜湊。 如果您需要建立[資訊清單檔案](../package/manifest.md)，以將軟體提交至 GitHub 上的 **Microsoft 社群封裝資訊清單存放庫**，則您適用此命令。 此外，**hash** 命令也支援為 MSIX 檔案產生 SHA256 憑證雜湊。

## <a name="usage"></a>使用方式

`winget hash [-f] \<file> [\<options>]`

**hash** 子命令只能在本機檔案上執行。 若要使用 **hash** 子命令，請將您的安裝程式下載到已知的位置。 然後將檔案路徑當做引數傳入 **hash** 子命令。

## <a name="arguments"></a>引數

下列是可用的引數：

| 引數  | 說明 |
|--------------|-------------|
| **-f,--file** |  針對要進行雜湊的檔案提供路徑。 |
| **-m,--msix**  | 指定雜湊命令也會建立要與 MSIX 安裝程式搭配使用的 SHA 256 SignatureSha256。 |
| **-?, --help** |  取得此命令的其他說明。 |

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理應用程式](index.md)
* [將封裝提交至 Windows 封裝管理員](../package/index.md)
