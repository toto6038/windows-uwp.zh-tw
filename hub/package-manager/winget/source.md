---
title: source 命令
description: 管理 Windows 封裝管理員所存取的存放庫。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: b44f20021a0fa33da862e2361be60b730b041b49
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/19/2020
ms.locfileid: "83824969"
---
# <a name="source-command-winget"></a>source 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 **source** 命令會管理 Windows 封裝管理員所存取的存放庫。 您可以使用 **source** 命令來**新增**、**移除**、**列出**及**更新**存放庫。

來源會提供讓您用來探索及安裝應用程式的資料。 您新增的來源必須是您信任的安全位置。

## <a name="usage"></a>使用方式

`winget source \<sub command> \<options>`

![來源影像](images\source.png)

## <a name="arguments"></a>引數

下列是可用的引數。

| 引數  | 說明 |
|--------------|-------------|
| **-?, --help** |  取得此命令的其他說明。 |

## <a name="sub-commands"></a>子命令

來源支援下列用於操作來源的子命令。

| 子命令  | 說明 |
|--------------|-------------|
|  **add** |  新增來源。 |
|  **list** | 列舉已啟用的來源清單。 |
|  **update** | 更新來源。 |
|  **remove** | 移除來源。 |
|  **reset** | 將 **winget** 重設回初始設定。  |

## <a name="options"></a>選項

**source** 命令支援下列選項。

| 選項  | 說明 |
|--------------|-------------|
|  **-n,--name** | 要用來識別來源的名稱。 |
|  **-a,--arg** | 來源的 URL 或 UNC。 |
|  **-t,--type** | 來源的類型。 |
| **-?, --help** |  取得此命令的其他說明。 |

## <a name="add"></a>新增

**add** 子命令會新增來源。 此子命令需要 **--name** 選項和 **name** 引數。

使用方式：`winget source add [-n, --name] \<name> [-a] \<url> [[-t] \<type>]`

範例：`winget source add --name Contoso  https://www.contoso.com/cache`

**add** 子命令也支援選擇性的 **type** 參數。 **type** 參數會向用戶端說明其正在連線的存放庫類型。 支援下列類型。

| 類型  | 說明 |
|--------------|-------------|
| **Microsoft.PreIndexed.Package** | 來源類型 \<預設>。 |

## <a name="list"></a>list

**list** 子命令會列舉目前啟用的來源。 此子命令也會提供特定來源的詳細資料。

使用方式：`winget list [-n, --name] \<name>`

### <a name="list-all"></a>全部列出

**list** 子命令本身會顯示所支援的完整來源清單。 例如：

```CMD
> C:\winget list
> Current sources:
>     Contoso ->  https://www.contoso.com/cache
```

### <a name="list-source-details"></a>列出來源詳細資料

為了取得來源的完整詳細資料，請傳入用來識別來源的名稱。 例如：

```CMD
> C:\winget source list --name contoso  
> Name   : contoso  
> Type   : Microsoft.PreIndexed.Package  
> Arg    : https://pkgmgr-int.azureedge.net/cache  
> Data   : AppInstallerSQLiteIndex-int_g4ype1skzj3jy  
> Updated: 2020-4-14 17:45:32.000
```

**Name** 顯示用來識別來源的名稱。
**Type** 顯示存放庫的類型。
**Arg** 顯示來源所使用的 URL 或路徑。
**Data** 顯示使用的選擇性封裝名稱 (如果有的話)。
**Updated** 顯示上次更新來源的日期和時間。

## <a name="update"></a>update

**update**子命令會強制更新個別來源或所有來源。

使用方式：`winget update [-n, --name] \<name>`

### <a name="update-all"></a>更新全部

**update**子命令本身會要求並更新每個存放庫。 例如：`C:\winget update`

### <a name="update-source"></a>更新來源

**update** 子命令與 **--name** 選項結合後，可以直接更新個別來源。 例如：`C:\winget update --name contoso`

## <a name="remove"></a>移除

**remove** 子命令會移除來源。 此子命令需要 **--name** 選項和 **name 引數**，才能識別來源。

使用方式：`winget source add [-n, --name] \<name>`

例如：`winget source remove --name Contoso`

## <a name="reset"></a>reset

**reset** 子命令會將用戶端重設回其原始設定。 **reset** 子命令會移除所有來源，並將來源設定為預設值。 使用此命令的情況應相當罕見。

使用方式：`winget source reset`

例如：`winget source reset`

## <a name="default-repository"></a>預設存放庫

Windows 封裝管理員會指定預設存放庫。 您可以使用 **list**命令來識別存放庫。 例如：`winget source list`

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理應用程式](index.md)
