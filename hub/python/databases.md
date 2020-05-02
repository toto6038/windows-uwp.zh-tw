---
title: 在 Windows 上開始使用 Python 與資料庫搭配
description: 本指南可協助您在 Windows 上開始使用 PostgreSQL 或 MongoDB 與 Python 搭配。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, postgresql, mongodb, postgres, mongo, microsoft, windows 上的 python, 在 windows 上安裝 postgresql, 在 windows 上安裝 mongodb, 使用 postgresql 與 python 搭配, 使用 mongodb 與 python 搭配, WSL 上的 postgresql, WSL 上的 mongodb
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9b1bdea86739f3d58b39cf7f0e6b8090474886f3
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517782"
---
# <a name="get-started-using-postgresql-or-mongodb-with-python-on-windows"></a>在 Windows 上開始使用 PostgreSQL 或 MongoDB 與 Python 搭配

此逐步指南將協助您開始將 Python 應用程式連線至資料庫。 我們選擇專注於兩個熱門選項：PostgreSQL 和 MongoDB。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 與 PostgreSQL 之間的差異

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 您也可能想要考慮如何將您所使用的架構和工具與特定資料庫系統整合。 [Django Web 架構](./web-frameworks.md#hello-world-tutorial-for-django)似乎與 PostgreSQL 更緊密結合 (請參閱 [Django 文件](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/)和 [psycopg2](https://github.com/psycopg/psycopg2))。 [Flask Web 架構](./web-frameworks.md#hello-world-tutorial-for-flask)似乎與 MongoDB 更緊密結合 (請參閱 [MongoEngine](https://github.com/MongoEngine/flask-mongoengine) 和 [PyMongo](https://github.com/dcrosta/flask-pymongo))。

## <a name="install-postgresql"></a>安裝 PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL 支援 VS Code

VS Code 支援透過 [PostgreSQL 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)使用 PostgreSQL 資料庫。您可以從 VS Code 內建立、連線至、管理和查詢 PostgreSQL 資料庫。

## <a name="install-mongodb"></a>安裝 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB 支援 VS Code

VS Code 支援透過 [Azure CosmosDB 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)使用 MongoDB 資料庫。您可以從 VS Code 內建立、連線至、管理和查詢 MongoDB 資料庫。

若要深入了解，請造訪 VS Code 文件：[使用 MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

## <a name="set-up-profile-aliases"></a>設定設定檔別名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
