---
title: search 命令
description: 查詢可以可安裝的應用程式來源
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 7038f9b31c4c0446e3af56cac2d118598347d4d3
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493253"
---
# <a name="search-command-winget"></a>search 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 **search** 命令會查詢可安裝的應用程式來源。  

**search** 命令可以顯示所有可用應用程式，也可以向下篩選到特定應用程式。 **search** 命令通常用來識別用來安裝特定應用程式的字串。

## <a name="usage"></a>使用方式

`winget search [[-q] \<query>] [\<options>]`

![搜尋](images\search.png)

## <a name="arguments"></a>引數

下列是可用的引數。

| 引數  | 說明 |
 --------------|-------------|
| **-q,--query** |  用來搜尋應用程式的查詢。 |
| **-?, --help** |  取得此命令的其他說明。 |

## <a name="show-all"></a>全部顯示

如果 search 命令未包含任何篩選或選項，則會在預設來源中顯示所有可用的應用程式。 如果您只傳入 **source** 選項，您也可以搜尋另一個來源中的所有應用程式。

## <a name="search-strings"></a>搜尋字串

您可以使用下列選項來篩選搜尋字串。

| 選項  | 說明 |
 --------------|-------------|
| **--id**        |   將搜尋限制為應用程式的識別碼。 識別碼包含發行者和應用程式名稱。 |
| **--name**      |  將搜尋限制為應用程式的名稱。 |
| **--moniker**  |    將搜尋限制為指定的別名。 |
| **--tag**    |  將搜尋限制為針對應用程式所列出的標籤。 |
| **--command**   |   將搜尋限制為針對應用程式所列出的命令。 |

該字串將被視為子字串。 根據預設，搜尋也不會區分大小寫。 例如，`winget search micro` 可能會傳回下列內容：

* Microsoft
* microscope
* MyMicro

## <a name="search-options"></a>搜尋選項

搜尋命令支援多個選項或篩選準則，可協助您限制結果。

| 選項  | 說明 |
 --------------|-------------|
| **-e, --exact**  |     在查詢中使用確切字串，包括檢查是否區分大小寫。 其不會使用子字串的預設行為。  |  
| **-n, --count**      |  將顯示的輸出限制為指定的計數。 |
| **-s, --source**     |  將搜尋限制為指定的[來源](source.md)名稱。  |

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理應用程式](index.md)
