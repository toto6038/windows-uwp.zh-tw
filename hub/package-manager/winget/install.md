---
title: install 命令
description: 安裝指定的應用程式。
author: KevinLaMS
ms.author: kevinla
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 8c460ccd18bb1bb12e5322e0e08a17edbd9692f7
ms.sourcegitcommit: 5a145eda92b5915393e58006867cdd8b98e922f5
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2020
ms.locfileid: "84166237"
---
# <a name="install-command-winget"></a>install 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 **install** 命令會安裝指定的應用程式。 使用 [**search**](search.md) 命令來識別您想要安裝的應用程式。  

**install** 命令需要您指定要安裝的確切字串。 如果有任何不明確的情況，系統會提示您進一步將 **install** 命令篩選到確切的應用程式。

## <a name="usage"></a>使用方式

`winget install [[-q] \<query>] [\<options>]`

![search 命令](images\install.png)

## <a name="arguments"></a>引數

下列是可用的引數。

| 引數      | 說明 |
|-------------|-------------|  
| **-q,--query**  |  用來搜尋應用程式的查詢。 |
| **-?, --help** |  取得此命令的其他說明。 |

## <a name="options"></a>選項

這些選項可讓您自訂安裝體驗，以符合您的需求。

| 選項      | 說明 |
|-------------|-------------|  
| **-m, --manifest** |   後面必須接著資訊清單 (YAML) 檔案的路徑。 您可以從 [本機 YAML 檔案](#local-install)中使用資訊清單執行安裝體驗。 |
| **--id**    |  將安裝限制為應用程式的識別碼。   |  
| **--name**   |  將搜尋限制為應用程式的名稱。 |  
| **--moniker**   | 將搜尋限制為針對應用程式列出的別名。 |  
| **-v, --version**  |  讓您指定要安裝的確切版本。 如果未指定，則會安裝目前最高版本的應用程式。 |  
| **-s, --source**   |  將搜尋限制為提供的來源名稱。 後面必須加上來源名稱。 |  
| **-e, --exact**   |   在查詢中使用確切字串，包括檢查是否區分大小寫。 其不會使用子字串的預設行為。 |  
| **-i, --interactive** |  在互動模式中執行安裝程式。 預設體驗會顯示安裝程式的進度。 |  
| **-h, --silent** |  以無訊息模式執行安裝程式。 這會隱藏所有 UI。 預設體驗會顯示安裝程式的進度。 |  
| **-o, --log**  |  將記錄導向至記錄檔。 您必須提供檔案路徑，而且您必須有該檔案的寫入權限。 |
| **--override** | 將直接傳遞至安裝程式的字串。    |
| **-l, --location** |    要安裝的位置 (如果支援的話)。 |

### <a name="example-queries"></a>範例查詢

下列範例會安裝特定版本的應用程式。

```CMD
winget install powertoys --version 0.15.2
```

下列範例會從其識別碼安裝應用程式。

```CMD
winget install --id Microsoft.PowerToys
```

下列範例會依版本和識別碼來安裝應用程式。

```CMD
winget install --id Microsoft.PowerToys --version 0.15.2
```

## <a name="multiple-selections"></a>多個選取項目

如果提供給 **winget** 的查詢不會產生單一應用程式，則 **winget** 會顯示搜尋結果。 這會提供您額外的資料，讓您更精確地搜尋，以進行正確安裝。

## <a name="local-install"></a>本機安裝

**資訊清單**選項可讓您直接將 YAML 檔案傳入用戶端，進而安裝應用程式。 **資訊清單**選項具有下列使用方式。

使用方式：`winget install --manifest \<file>`

| 選項  | 說明 |
|-------------|-------------|  
|  **-m, --manifest** | 要安裝的應用程式資訊清單路徑。 |

### <a name="log-files"></a>記錄檔

除非重新導向，否則 winget 的記錄檔會位於下列資料夾中： **\%temp%\\AICLI\\*.log**

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理應用程式](index.md)
