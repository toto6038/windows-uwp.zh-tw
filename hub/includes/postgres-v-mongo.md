---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314872"
---
資料庫系統的[熱門選擇](https://insights.stackoverflow.com/survey/2019#technology-_-databases)是 [MongoDB](https://www.mongodb.com/what-is-mongodb) 和 [PostgreSQL](https://www.postgresql.org/about/)。 

MongoDB 是 NoSQL 文件資料庫，其設計旨在使用 JSON，並儲存無結構描述的資料。 這非常適合於彈性和非結構化資料、快取即時分析和水平調整。 

PostgreSQL (有時也稱為 Postgres) 是一種 SQL 關聯式資料庫，著重於擴充性和標準合規性。 它現在也可以處理 JSON，但通常更適用於結構化資料、垂直調整和符合 ACID 規範的需求，例如電子商務和金融交易。

結構描述：

**PostgreSQL：** 資料表 |資料行 |值 | 記錄。

**MongoDB (NoSQL)：** 集合 | 索引鍵 | 值 | 文件。

您選擇的資料庫種類應該取決於您將使用資料庫與其搭配的應用程式類型。 建議您查閱結構化和非結構化資料庫的優缺點，並根據使用案例進行選擇。 除了 PostgreSQL 和 MongoDB，另外還有其他幾個資料庫系統需要考慮。