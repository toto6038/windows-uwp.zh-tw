---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314882"
---
安裝 MongoDB：

1. 開啟您的 WSL 終端機（ie）。Ubuntu 18.04）。
2. 更新您的 Ubuntu 套件： `sudo apt update`
3. 更新套件之後，請使用下列內容安裝 MongoDB： `sudo apt-get install mongodb`
4. 確認安裝並取得版本號碼： `mongod --version`

安裝 MongoDB 之後，您必須知道3個命令：

1. `sudo service mongodb status`，用於檢查資料庫的狀態。
2. `sudo service mongodb start`，以開始執行您的資料庫。
3. `sudo service mongodb stop` 則停止執行您的資料庫。

> [!NOTE]
> 您可能會看到在教學課程或文章中使用 `sudo systemctl status mongodb` 命令。 為了保持輕量，WSL 不包含 `systemd` （Linux 中的服務管理系統）。 相反地，它會使用 SysVinit 在您的電腦上啟動服務。 您不應該注意到差異，但如果教學課程建議使用 `sudo systemctl`，請改用： `sudo /etc/init.d/`。 例如，`sudo systemctl status mongodb`，WSL 的會是 `sudo /etc/inid.d/mongodb status` .。。或者，您也可以使用 `sudo service mongodb status`。

### <a name="run-your-mongo-database-in-a-local-server"></a>在本機伺服器中執行您的 Mongo 資料庫

1. 檢查資料庫的狀態：`sudo service mongodb status` 您應該會看到 [Fail] 回應，除非您已經啟動資料庫。

2. 啟動您的資料庫：`sudo service mongodb start` 您現在應該會看到 [確定] 回應。

3. 藉由連接到資料庫伺服器並執行診斷命令來確認：`mongo --eval 'db.runCommand({ connectionStatus: 1 })'`，這會輸出目前的資料庫版本、伺服器位址和埠，以及狀態命令的輸出。 回應中「確定」欄位的 @no__t 值為0，表示伺服器正在運作。

4. 若要停止您的 MongoDB 服務執行，請輸入： `sudo service mongodb stop`

> [!NOTE]
> MongoDB 有數個預設參數，包括將資料儲存在/data/db 中，並在埠27017上執行。 此外，`mongod` 是背景程式（資料庫的主機進程），而 `mongo` 是連接到特定 `mongod` 實例的命令列 shell。
