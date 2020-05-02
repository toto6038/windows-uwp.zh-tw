---
title: 開始將 Node.js 應用程式連線到資料庫
description: 開始將 Node.js 應用程式連線到 Windows 上的資料庫。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, 了解 nodejs, windows 上的 Node, wsl 上的 Node, linux 或 windows 上的 Node, 在 windows 上安裝 Node, nodejs 與 vs code, 在 windows 上使用 Node 進行開發, 在 windows 上使用 nodejs 進行開發, 在 WSL 上安裝 Node, Windows 子系統上適用於 Linux 的 NodeJS
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 63c47107538d8744201f83ea1be24cfaf3193f4f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "72517823"
---
# <a name="get-started-using-mongodb-or-postgresql-with-nodejs-on-windows"></a>開始在 Windows 上搭配 Node.js 使用 MongoDB 或 PostgreSQL

Node.js 應用程式通常需要保存資料，這可能透過檔案、本機儲存體、雲端服務或資料庫進行。 此逐步指南將協助您開始將 Node.js 應用程式連線至資料庫。 我們選擇專注於兩個熱門選項：MongoDB 與 PostgreSQL。

## <a name="prerequisites"></a>必要條件

本指南假設您已經完成[使用 WSL 2 設定您的 Node.js 開發環境](./setup-on-wsl2.md)的步驟，包括：

- 安裝 Windows 10 Insider Preview 組建 18932 或更新版本。
- 在 Windows 上啟用 WSL 2 功能。
- 安裝 Linux 發行版本 (本範例適用 Ubuntu 18.04)。 可透過下列方式進行檢查：`wsl lsb_release -a`
- 確保 Ubuntu 18.04 發行版本是在 WSL 2 模式下執行。 (WSL 可以在 v1 或 v2 模式中執行發行版本。)可開啟 PowerShell 並輸入下列內容，以此方式進行檢查：`wsl -l -v`
- 使用 PowerShell，將 Ubuntu 18.04 設定為預設發行版本，請透過：`wsl -s ubuntu 18.04`

## <a name="differences-between-mongodb-and-postgresql"></a>MongoDB 與 PostgreSQL 之間的差異

[!INCLUDE [Postgres vs Mongo](../includes/postgres-v-mongo.md)]

## <a name="install-mongodb"></a>安裝 MongoDB

[!INCLUDE [Install and run Mongo](../includes/install-and-run-mongo.md)]

### <a name="vs-code-support-for-mongodb"></a>MongoDB 支援 VS Code

VS Code 支援透過 [Azure CosmosDB 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)使用 MongoDB 資料庫。您可以在 VS Code 中建立、管理及查詢 MongoDB 資料庫。

若要深入了解，請造訪 VS Code 文件：[使用 MongoDB](https://code.visualstudio.com/docs/azure/mongodb)。

在 MongoDB 文件中深入了解：

- [使用 MongoDB 的簡介](https://docs.mongodb.com/manual/introduction/)
- [建立使用者](https://docs.mongodb.com/manual/tutorial/create-users/)
- [連線至遠端主機上的 MongoDB 執行個體](https://docs.mongodb.com/manual/mongo/#mongodb-instance-on-a-remote-host)
- [CRUD：建立、讀取、更新、刪除](https://docs.mongodb.com/manual/crud/)
- [參考文件](https://docs.mongodb.com/manual/reference/)

## <a name="install-postgresql"></a>安裝 PostgreSQL

[!INCLUDE [Install and run PostgresQL](../includes/install-and-run-postgres.md)]

### <a name="vs-code-support-for-postgresql"></a>PostgreSQL 支援 VS Code

VS Code 支援透過 [PostgreSQL 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql)使用 PostgreSQL 資料庫。您可以從 VS Code 內建立、連線至、管理和查詢 PostgreSQL 資料庫。

## <a name="set-up-profile-aliases"></a>設定設定檔別名

[!INCLUDE [Set up profile aliases](../includes/profile-aliases.md)]
