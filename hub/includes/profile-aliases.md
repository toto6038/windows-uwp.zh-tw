---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314902"
---
鍵入 `sudo service mongodb start` 或 `sudo service postgres start` 和 `sudo -u postgrest psql` 可能會很繁瑣。  不過，您可以考慮在 WSL 的 `.profile` 檔案中設定別名，可以更快速地使用這些命令且更容易記住。 

若要設定自己的自訂別名或捷徑，請執行下列命令：

1. 開啟 WSL 終端機並輸入 `cd ~`，以確定您位於根目錄中。
2. 使用終端機文字編輯器 Nano 開啟 `.profile` 檔案，控制終端機的設定：`sudo nano .profile`
3. 在檔案底部 (請勿變更 `# set PATH` 設定)，新增下列內容：

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

這可讓您輸入 `start-pg`，以開始執行 postgresql 服務，以及輸入 `run-pg` 以開啟 psql 命令介面。 您可以將 `start-pg` 和 `run-pg` 變更為想要的任何名稱，但要小心不要覆寫 postgres 已使用的命令！

4. 新增別名之後，請使用 **Ctrl + X** 結束 Nano 文字編輯器 -- 當系統提示您儲存並按 Enter 鍵 (將檔案名稱保留為 `.profile`) 時，請選取 `Y` (是)。
5. 關閉並重新開啟您的 WSL 終端機，然後嘗試新的別名命令。
