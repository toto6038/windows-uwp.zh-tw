---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314902"
---
輸入 `sudo service mongodb start` 或 `sudo service postgres start` 和 `sudo -u postgrest psql` 可能會變得繁瑣。  不過，您可以考慮在 WSL 的 @no__t 0 檔案中設定別名，讓這些命令更快速地使用且更容易記住。 

若要設定您自己的自訂別名或快捷方式，以執行下列命令：

1. 開啟 WSL 終端機，並輸入 `cd ~`，以確定您位於根目錄中。
2. 開啟 `.profile` 檔案，以控制終端機的設定，並使用終端機文字編輯器 Nano： `sudo nano .profile`
3. 在檔案底部（請勿變更 [`# set PATH`] 設定），新增下列內容：

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

這可讓您輸入 `start-pg` 以開始執行于 postgresql 服務，並 `run-pg` 以開啟 psql shell。 您可以將 `start-pg` 和 `run-pg` 變更為您想要的任何名稱，只要小心不要覆寫 postgres 已經使用的命令！

4. 新增新的別名之後，請使用**Ctrl + X**來結束 Nano 文字編輯器--當系統提示您儲存並輸入時，請選取 [`Y` （是）] （將檔案名保留為 `.profile`）。
5. 關閉並重新開啟您的 WSL 終端機，然後嘗試新的別名命令。
