---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: 資料存取
description: 本節討論在私人資料庫的裝置上儲存資料，以及在通用 Windows 平台 (UWP) app 中使用物件關聯對應。
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, 資料, 資料庫, 關聯式, 資料, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: d324c5b17e167b841761b92507eec2134e4b7009
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173642"
---
# <a name="data-access"></a>資料存取

您可以使用 SQLite 資料庫，在使用者的裝置上儲存資料。 您也可以將應用程式直接連線到 SQL Server 資料庫，而不需使用任何一種服務層。

| 主題 | 描述|
|-------|------------|
| [在 UWP 應用程式中使用 SQLite 資料庫](sqlite-databases.md) | 說明如何使用 SQLite 在使用者裝置上儲存和擷取輕量資料庫中的資料。 SQLite 是無伺服器的內嵌資料庫引擎。 |
| [在 UWP 應用程式中使用 SQL Server 資料庫](sql-server-databases.md) | 說明如何直接連線到 SQL Server 資料庫，然後使用 [System.Data.SqlClient](/dotnet/api/system.data.sqlclient) 命名空間中的類別儲存和擷取資料。 不需要服務層級。 |

## <a name="related-topics"></a>相關主題

* [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)