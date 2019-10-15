---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314892"
---
若要安裝于 postgresql：

1. 開啟您的 WSL 終端機（ie）。Ubuntu 18.04）。
2. 更新您的 Ubuntu 套件： `sudo apt update`
3. 更新套件之後，安裝于 postgresql （以及具有一些實用公用程式的-contrib 套件）與： `sudo apt install postgresql postgresql-contrib`
4. 確認安裝並取得版本號碼： `psql --version`

安裝于 postgresql 之後，您必須知道3個命令：

1. `sudo service postgresql status`，用於檢查資料庫的狀態。
2. `sudo service postgresql start`，以開始執行您的資料庫。
3. `sudo service postgresql stop` 則停止執行您的資料庫。

### <a name="postgresql-user-setup"></a>于 postgresql 使用者設定

預設的管理使用者 `postgres`，需要指派密碼，才能連接到資料庫。 若要設定密碼：

1. 輸入命令： `sudo passwd postgres`
2. 您會收到輸入新密碼的提示。
3. 關閉並重新開啟您的終端機。

### <a name="run-postgresql-with-psql-shell"></a>使用 psql shell 執行于 postgresql

[psql](https://www.postgresql.org/docs/10/app-psql.html)是以終端機為基礎的前端到于 postgresql。 它可讓您以互動方式輸入查詢、將其發行至於 postgresql，以及查看查詢結果。 或者，可以從檔案輸入。 此外，它還提供一些中繼命令和各種類似 shell 的功能，以協助撰寫腳本及自動化各種不同的工作。

若要啟動 psql shell：

1. 啟動您的 postgres 服務： `sudo service postgresql start`
2. 連接到 postgres 服務並開啟 psql shell： `sudo -u postgres psql`

成功輸入 psql shell 後，您會看到命令列的變更，如下所示： `postgres=#`

> [!NOTE]
> 或者，您可以使用下列命令來切換至 postgres 使用者，以開啟 psql shell： `su - postgres`，然後輸入命令 . `psql`。

若要結束 postgres = # enter： `\q`，或使用快速鍵：Ctrl + D

若要查看于 postgresql 安裝上已建立的使用者帳戶，請從您的 WSL 終端機使用： `psql -c "\du"` 。或者，如果您已開啟 psql shell，則只 `\du`。 此命令將會顯示資料行：帳戶使用者名稱、角色屬性清單，以及角色群組的成員。 若要結束並返回命令列，請輸入： `q`。
