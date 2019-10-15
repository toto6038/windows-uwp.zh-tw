---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 0e144789531e43e3561e7b82830c2b78249c294b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314872"
---
資料庫系統有兩個[熱門的選擇](https://insights.stackoverflow.com/survey/2019#technology-_-databases)： [MongoDB](https://www.mongodb.com/what-is-mongodb)和[于 postgresql](https://www.postgresql.org/about/)。 

MongoDB 是 NoSQL 檔資料庫，設計用來與 JSON 搭配使用，並儲存無架構的資料。 這適用于彈性和非結構化資料、快取即時分析和水準調整。 

于 postgresql （有時也稱為 Postgres）是一種 SQL 關係資料庫，著重于擴充性和標準合規性。 它現在也可以處理 JSON，但通常適用于結構化資料、垂直調整和符合 ACID 規範的需求，例如電子商務和財務交易。

架構

**于 postgresql**資料表 |資料行 |值 |記錄.

**MongoDB （NoSQL）：** 集合 |金鑰 |值 |記錄.

您選擇的資料庫類型應該取決於您將使用資料庫的應用程式類型。 我們建議您查閱結構化和非結構化資料庫的優缺點，並根據您的使用案例加以選擇。 另外還有其他幾個資料庫系統需要考慮于 postgresql 和 MongoDB 以外的範圍。