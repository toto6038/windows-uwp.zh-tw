---
title: show 命令
description: 顯示指定應用程式的詳細資料，包括應用程式來源的詳細資料，以及與應用程式相關聯的中繼資料。
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 9443bb31133ee9643571a4861af7027272b35d94
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334509"
---
# <a name="show-command-winget"></a>show 命令 (winget)

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

[winget](index.md) 工具的 **show** 命令會顯示指定應用程式的詳細資料，包括應用程式來源的詳細資訊，以及與應用程式相關聯的中繼資料。

**show** 命令只會顯示隨著應用程式提交的中繼資料。 如果已提交的應用程式排除了某些中繼資料，則不會顯示該資料。

## <a name="usage"></a>使用方式

`winget show [[-q] \<query>] [\<options>]`

![show 命令](images\show.png)

## <a name="arguments"></a>引數

下列是可用的引數。

| 引數  | 說明 |
|--------------|-------------|
| **-q,--query** |  用來搜尋應用程式的查詢。 |
| **-?, --help** |  取得此命令的其他說明。 |

## <a name="options"></a>選項

可用選項如下。

| 選項  | 說明 |
|--------------|-------------|
| **-m,--manifest** | 要安裝的應用程式資訊清單路徑。 |
| **--id**         |  依據識別碼篩選結果。 |
| **--name**   |      依據名稱篩選結果。 |
| **--moniker**   |  依據應用程式別名篩選結果。 |
| **-v,--version** |  使用指定的版本。 預設值為最新版本。 |
| **-s,--source** |   使用指定的[來源](source.md)尋找應用程式。 |
| **-e,--exact**     | 使用「完全相符」準則來尋找應用程式。 |
| **--versions**    | 顯示可用的應用程式版本。 |

## <a name="multiple-selections"></a>多個選取項目

如果提供給 **winget** 的查詢不會產生單一應用程式，則 **winget** 會顯示搜尋結果。 這會提供您額外的資料，讓您更精確地進行搜尋。

## <a name="results-of-show"></a>show 的結果

如果偵測到單一應用程式，將會顯示下列資料。

### <a name="metadata"></a>中繼資料

| 值  | 說明 |
|--------------|-------------|
| **識別碼**   | 應用程式的識別碼。 |
| **名稱**  | 應用程式的名稱。 |
| **發行者** | 應用程式的發行者。 |
| **版本** | 應用程式的版本。 |
| **作者**  | 應用程式的作者。 |
| **AppMoniker** | 應用程式的 AppMoniker。 |
| **描述** | 應用程式的描述。 |
| **授權**  | 應用程式的授權。 |
| **LicenseUrl** | 應用程式授權檔案的 URL。 |
| **首頁**  | 應用程式的首頁。 |
| **標籤** | 用於協助搜尋的標籤。  |
| **命令** | 應用程式支援的命令。 |
| **通道**  | 有關應用程式是預覽版本或發行版本的詳細資料。  |
| **OS 最低版本** | 應用程式所支援的 OS 最低版本。 |

### <a name="installer-details"></a>安裝程式詳細資料

| 值  | 說明 |
|--------------|-------------|
| **架構**   | 安裝程式的架構。 |
| **語言**  | 安裝程式的語言。 |
| **安裝程式類型**  | 安裝程式的類型。 |
| **下載 URL** | 安裝程式的 URL。 |
| **雜湊** | 安裝程式的 Sha-256。  |
| **範圍** | 顯示安裝程式是以每部電腦，還是每位使用者為單位。 |

## <a name="related-topics"></a>相關主題

* [使用 winget 工具來安裝和管理應用程式](index.md)
