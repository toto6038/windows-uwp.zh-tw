---
title: 開始在 Windows 上搭配使用 Python 與資料庫
description: 協助您開始在 Windows 上搭配使用於 postgresql 或 MongoDB 與 Python 的指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python、windows 10、于 postgresql、mongodb、postgres、mongo、microsoft、windows 上的 python、在 windows 上安裝、在 windows 上安裝 mongodb、使用於 postgresql 搭配 python、搭配 python 使用 mongodb、于 postgresql 上的 WSL、mongodb on WSL
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 42a257361cffec974d060a6518dfdf5254d62082
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314852"
---
# <a name="get-started-using-databases-with-python-on-windows"></a>開始在 Windows 上搭配使用資料庫與 Python

Python 應用程式通常需要保存資料，這可能會透過檔案、本機儲存體、雲端服務或資料庫來進行。 此逐步指南將協助您開始將 Python 應用程式連接至資料庫。 我們選擇將焦點放在兩個熱門的選項：于 postgresql 和 MongoDB。

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 和于 postgresql 之間的差異

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

> [!NOTE]
> 您也可能想要考慮如何整合您所使用的架構和工具與特定的資料庫系統。 [Django web 架構](./web-frameworks.md#hello-world-tutorial-for-django)似乎與于 postgresql 更緊密整合（請參閱[Django](https://docs.djangoproject.com/en/2.2/ref/contrib/postgres/)檔和[psycopg2](https://github.com/psycopg/psycopg2)）。 [Flask web 架構](./web-frameworks.md#hello-world-tutorial-for-flask)似乎與 MongoDB 更緊密整合（請參閱[MongoEngine](https://github.com/MongoEngine/flask-mongoengine)和[PyMongo](https://github.com/dcrosta/flask-pymongo)）。

## <a name="install-postgresql"></a>安裝于 postgresql

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>于 postgresql 的 VS Code 支援

VS Code 支援透過[于 postgresql 擴充](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)功能來使用於 postgresql 資料庫，您可以從 VS Code 內建立、連接、管理和查詢于 postgresql 資料庫。

## <a name="install-mongodb"></a>安裝 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB 的 VS Code 支援

VS Code 支援透過[Azure CosmosDB 擴充](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)功能來使用 mongodb 資料庫，您可以從 VS Code 內建立、連接、管理及查詢 mongodb 資料庫。

若要深入瞭解，請造訪 VS Code 檔：使用[MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

## <a name="set-up-profile-aliases"></a>設定設定檔別名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
