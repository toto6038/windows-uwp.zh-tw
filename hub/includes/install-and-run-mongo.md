---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314882"
---
若要安裝 MongoDB：

1. 開啟 WSL 終端機 (即 Ubuntu 18.04)。
2. 更新 Ubuntu 套件：`sudo apt update`
3. 更新套件之後，請使用下列命令安裝 MongoDB：`sudo apt-get install mongodb`
4. 確認安裝並取得版本號碼：`mongod --version`

安裝 MongoDB 之後，您必須知道的 3 個命令：

1. `sudo service mongodb status`，用於檢查資料庫的狀態。
2. `sudo service mongodb start`，用來開始執行您的資料庫。
3. `sudo service mongodb stop`，用來停止執行您的資料庫。

> [!NOTE]
> 您可能會看到用於教學課程或文章中的 `sudo systemctl status mongodb` 命令。 為了保持輕量型工作，WSL 不包括 `systemd` (Linux 中的服務管理系統)。 反之，它會使用 SysVinit 在您的電腦上啟動服務。 您應該不會注意到差異，但如果教學課程建議使用 `sudo systemctl`，請改用：`sudo /etc/init.d/`。 例如，`sudo systemctl status mongodb`，若為 WSL，將是 `sudo /etc/inid.d/mongodb status` ...，或者，也可以使用 `sudo service mongodb status`。

### <a name="run-your-mongo-database-in-a-local-server"></a>在本機伺服器中執行您的 Mongo 資料庫

1. 檢查您資料庫的狀態：`sudo service mongodb status` 您應該會看到 [Fail] 回應，除非您已經啟動資料庫。

2. 啟動您的資料庫：`sudo service mongodb start` 您現在應該會看到 [OK] 回應。

3. 藉由連線至資料庫伺服器並執行診斷命令來確認：`mongo --eval 'db.runCommand({ connectionStatus: 1 })'` 這會輸出目前的資料庫版本、伺服器位址和連接埠，以及狀態命令的輸出。 回應中 "ok" 欄位的 `1` 值表示伺服器正在運作。

4. 若要停止執行您的 MongoDB 服務，請輸入：`sudo service mongodb stop`

> [!NOTE]
> MongoDB 有數個預設參數，包括將資料儲存在 /data/db 中，並在連接埠 27017 上執行。 此外，`mongod` 是精靈 (資料庫的主機處理程序)，而 `mongo` 是連線至特定 `mongod` 執行個體的命令列命令介面。
