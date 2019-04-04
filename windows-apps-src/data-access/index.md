---
ms.assetid: 76776b0f-3163-48c9-835b-3f4213968079
title: 資料存取
description: 本節討論在私人資料庫的裝置上儲存資料，以及在通用 Windows 平台 (UWP) app 中使用物件關聯對應。
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, 資料, 資料庫, 關聯式, 資料, sqlite
ms.localizationpriority: medium
ms.openlocfilehash: eb5adbdd3ae12d039d934e8d0cbe468ae5c1187c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582301"
---
# <a name="data-access"></a>資料存取

您可以使用 SQLite 資料庫，在使用者的裝置上儲存資料。 您也可以將應用程式直接連線到 SQL Server 資料庫，而不需使用任何一種服務層。

| 主題 | 描述|
|-------|------------|
| [在 UWP 應用程式中使用 SQLite 資料庫](sqlite-databases.md) | 說明如何使用 SQLite 在使用者裝置上儲存和擷取輕量資料庫中的資料。 SQLite 是無伺服器的內嵌資料庫引擎。 |
| [在 UWP 應用程式中使用 SQL Server 資料庫](sql-server-databases.md) | 說明如何直接連線到 SQL Server 資料庫，然後使用 [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) 命名空間中的類別儲存和擷取資料。 不需要服務層級。 |

## <a name="related-topics"></a>相關主題

* [客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
