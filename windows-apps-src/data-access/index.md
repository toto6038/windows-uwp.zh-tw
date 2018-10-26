---
author: normesta
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: 資料存取
description: 本節討論在私人資料庫的裝置上儲存資料，以及在通用 Windows 平台 (UWP) 應用程式中使用物件關聯對應。
ms.author: normesta
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, 資料, 資料庫, 關聯式, 表格, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: beca20d358430ecd82cd1bc57459a6f6af36be77
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5569860"
---
# <a name="data-access"></a>資料存取

您可以在使用者裝置上儲存資料，藉由使用 SQLite 資料庫。 您也可以直接到 SQL Server 資料庫連接您的應用程式，而不需要使用任何一種服務層級。

| 主題 | 描述|
|-------|------------|
| [在 UWP app 中使用 SQLite 資料庫](sqlite-databases.md) | 示範如何使用 SQLite 來儲存和擷取輕量資料庫在使用者裝置上的資料。 SQLite 是無伺服器的內嵌資料庫引擎。 |
| [UWP app 中使用 SQL server 資料庫](sql-server-databases.md) | 示範如何直接連接到 SQL Server 資料庫，然後儲存和擷取資料[System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)命名空間中使用的類別。 不需要服務層級。 |

## <a name="related-topics"></a>相關主題

* [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
