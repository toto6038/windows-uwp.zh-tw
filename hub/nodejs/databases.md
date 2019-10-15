---
title: 開始將 node.js 應用程式連接到資料庫
description: 開始將 node.js 應用程式連接到 Windows 上的資料庫。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS，node.js，windows 10，microsoft，學習 NodeJS，windows 上的節點，wsl 上的節點，在 windows 上安裝節點，使用 vs code 的 NodeJS，在 windows 上以節點進行開發，在 windows 上使用 NodeJS 進行開發，在 windows 上的 wsl 上安裝節點適用于 Linux 的子系統
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: bdc3e3c944c4aeb25f5cf880fc4d31df1019da5a
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315112"
---
# <a name="get-started-connecting-nodejs-apps-to-a-database"></a>開始將 node.js 應用程式連接到資料庫

Node.js 應用程式通常需要保存資料，這可能會透過檔案、本機儲存體、雲端服務或資料庫來進行。 此逐步指南將協助您開始將 node.js 應用程式連接到資料庫。 我們選擇將焦點放在兩個熱門的選項：MongoDB 和于 postgresql。

## <a name="prerequisites"></a>必要條件

本指南假設您已完成[使用 WSL 2 設定 node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建18932或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 散發套件（適用于我們的範例的 Ubuntu 18.04）。 您可以使用下列方式來檢查： `wsl lsb_release -a`
- 請確定您的 Ubuntu 18.04 散發套件在 WSL 2 模式下執行。 （WSL 可以在 v1 或 v2 模式中執行散發）。您可以開啟 PowerShell 並輸入： `wsl -l -v` 來檢查此項
- 使用 PowerShell，將 Ubuntu 18.04 設定為預設散發，並使用： `wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 和于 postgresql 之間的差異

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>安裝 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB 的 VS Code 支援

VS Code 支援透過[Azure CosmosDB 擴充](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)功能來使用 mongodb 資料庫，您可以從 VS Code 內建立、管理和查詢 mongodb 資料庫。

若要深入瞭解，請造訪 VS Code 檔：使用[MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

在 MongoDB 檔中深入瞭解：

- [使用 MongoDB 簡介](https://docs.mongodb.com/manual/introduction/)
- [建立使用者](https://docs.mongodb.com/manual/tutorial/create-users/)
- [連接至遠端主機上的 MongoDB 實例](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD：Create、Read、Update、Delete @ no__t-0
- [參考檔](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>安裝于 postgresql

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>于 postgresql 的 VS Code 支援

VS Code 支援透過[于 postgresql 擴充](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)功能來使用於 postgresql 資料庫，您可以從 VS Code 內建立、連接、管理和查詢于 postgresql 資料庫。

## <a name="set-up-profile-aliases"></a>設定設定檔別名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
