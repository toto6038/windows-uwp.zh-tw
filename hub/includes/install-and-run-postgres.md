---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314892"
---
若要安裝 PostgreSQL：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。
2. 更新 Ubuntu 套件：`sudo apt update`
3. 更新套件之後，請使用下列命令安裝 PostgreSQL (以及具有一些實用公用程式的 -contrib 套件)：`sudo apt install postgresql postgresql-contrib`
4. 確認安裝並取得版本號碼：`psql --version`

安裝 PostgreSQL 之後，您必須知道的 3 個命令：

1. `sudo service postgresql status`，用於檢查您資料庫的狀態。
2. `sudo service postgresql start`，用來開始執行您的資料庫。
3. `sudo service postgresql stop`，用來停止執行您的資料庫。

### <a name="postgresql-user-setup"></a>PostgreSQL 使用者設定

預設系統管理員使用者 `postgres` 需要獲指派密碼，才能連線至資料庫。 若要設定密碼：

1. 輸入命令：`sudo passwd postgres`
2. 您會收到輸入新密碼的提示。
3. 關閉並重新開啟您的終端機。

### <a name="run-postgresql-with-psql-shell"></a>使用 psql 命令介面執行 PostgreSQL

[psql](https://www.postgresql.org/docs/10/app-psql.html) 是 PostgreSQL 的終端機型前端。 它可讓您以互動方式鍵入查詢、將其發給 PostgreSQL，以及查看查詢結果。 或者，輸入可以來自檔案。 此外，它還提供一些中繼命令和各種類似命令介面的功能，以協助撰寫指令碼和自動執行各種不同的工作。

若要啟動 psql 命令介面：

1. 啟動您的 postgres 服務：`sudo service postgresql start`
2. 連線至 postgres 服務並開啟 psql 命令介面：`sudo -u postgres psql`

成功進入了 psql 命令介面之後，您會看到命令列變更為如下的命令列：`postgres=#`

> [!NOTE]
> 或者，您可以使用下列命令來切換至 postgres 使用者，以開啟 psql 命令介面：`su - postgres`，然後輸入 `psql`。

若要結束 postgres=#，請輸入 `\q` 或使用快速鍵：Ctrl+D

若要查看已在 PostgreSQL 安裝上建立哪些使用者帳戶，請從您的 WSL 終端機使用：`psql -c "\du"` ...，或者，如果您已開啟 psql 命令介面，則只使用 `\du`。 此命令將會顯示資料行：帳戶使用者名稱、角色屬性清單，以及角色群組的成員。 若要結束並返回命令列，請輸入：`q`。
