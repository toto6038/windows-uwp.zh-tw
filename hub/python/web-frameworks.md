---
title: 在 Windows 上使用 Python 進行 Web 開發
description: 如何開始在 Windows 上使用 Python 進行 Web 開發，包括針對 Flask 和 Django 等架構進行設定。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, windows 上的 python, 搭配 wsl 的 python web, 搭配 windows 子系統 linux 版的 python web 應用程式, 在 windows 上進行 python web 開發, windows 上的 flask 應用程式, windows 上的 django 應用程式, python web, 在 windows 上進行 flask web 開發, 在 windows 上進行 django web 開發, 使用 python 進行 windows web 開發, vs code python web 開發, 遠端 wsl 延伸模組, ubuntu, wsl, venv, pip, microsoft python 延伸模組, 在 windows 上執行 python, 在 windows 上使用 python, 在 windows 上使用 python 進行建置
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 187195133dd614818d3c68473cc53b71a0b32333
ms.sourcegitcommit: 27552ed7d3d889f50d8e01776a24b8d486a8d97c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2020
ms.locfileid: "91958741"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>開始在 Windows 上使用 Python 進行 Web 開發

以下是逐步指南，引導您使用 Windows 子系統 Linux 版 (WSL)，開始在 Windows 上使用 Python 進行 Web 開發。

## <a name="set-up-your-development-environment"></a>設定開發環境

建議您在建置 Web 應用程式時，在 WSL 上安裝 Python。 許多用於開發 Python Web 應用程式的教學課程和指示都是針對 Linux 使用者所撰寫，並使用 Linux 型封裝和安裝工具。 大部分的 Web 應用程式也會部署在 Linux 上，因此這可確保您的開發與生產環境之間具有一致性。

如果您是使用 Python 進行 Web 開發以外的操作，建議您使用 Microsoft Store 直接在 Windows 10 上安裝 Python。 WSL 不支援 GUI 桌上型電腦或應用程式 (例如 PyGam、Gnome、KDE 等)。 若為這些情況，直接在 Windows 上安裝和使用 Python。 如果您是初次使用 Python，請參閱我們的指南：[開始在 Windows 上使用適用於初學者的 Python](./beginners.md)。 如果您有興趣將作業系統上的一般工作自動化，請參閱我們的指南：[開始在 Windows 上使用 Python 進行指令碼處理和自動化](./scripting.md)。 對於某些進階案例，您可能會想要考慮直接從 [python.org](https://www.python.org/downloads/windows/) 下載特定的 Python 版本，或考慮安裝[替代項目](https://www.python.org/download/alternatives)，例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。只有當您是資深 Python 程式設計人員，且有選擇替代實作的特定原因時，才建議這樣做。

## <a name="install-windows-subsystem-for-linux"></a>安裝 Windows 子系統 Linux 版

使用 WSL，您可以執行與 Windows 和您最愛的工具 (例如 Visual Studio Code、Outlook 等) 直接整合的 GNU/Linux 命令列環境。

若要啟用並安裝 WSL (或 WSL 2，視需求而定)，請遵循 [WSL 安裝文件](/windows/wsl/install-win10)中的步驟。 這些步驟會包含選擇 Linux 發行版本 (例如 Ubuntu)。

一旦您安裝了 WSL 和 Linux 發行版本，請開啟 Linux 發行版本 (位於 Windows 的 [開始] 功能表中)，然後使用下列命令來檢查版本和代號：`lsb_release -dc`。

我們建議您定期更新 Linux 發行版本 (包括安裝後立即更新)，以確保您擁有最新的套件。 Windows 不會自動處理此更新。 若要更新您的發行版本，請使用下列命令：`sudo apt update && sudo apt upgrade`。  

> [!TIP]
> 考慮[從 Microsoft Store 安裝新的 Windows 終端機](https://www.microsoft.com/store/apps/9n0dx20hk701)以啟用多個索引標籤 (在多個 Linux 命令列、Windows 命令提示字元、PowerShell、Azure CLI 等之間快速切換)、建立自訂按鍵繫結 (開啟或關閉索引標籤、複製+貼上等的快速鍵)、使用搜尋功能及設定自訂佈景主題 (色彩配置、字型樣式和大小、背景影像/柔邊/透明度)。 [深入了解](/windows/terminal)。

## <a name="set-up-visual-studio-code"></a>設定 Visual Studio Code

使用 VS Code 來利用 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)、[Linting](https://code.visualstudio.com/docs/python/linting)、[偵錯支援](https://code.visualstudio.com/docs/python/debugging)、[程式碼片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)和[單元測試](https://code.visualstudio.com/docs/python/unit-testing)。 VS Code 與 Windows 子系統 Linux 版緊密整合，提供 [內建終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)，以在程式碼編輯器與命令列之間建立順暢的工作流程，而且支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)，其中常用的 Git 命令 (add、commit、push、pull) 直接內建在 UI 中。

1. [下載並安裝 VS Code for Windows](https://code.visualstudio.com)。 VS Code 也適用於 Linux，但 Windows 子系統 Linux 版不支援 GUI 應用程式，因此我們需要將它安裝在 Windows 上。 別擔心，您仍然可以使用 Remote - WSL 延伸模組，與您的 Linux 命令列和工具整合。

2. 在 VS Code 上安裝 [Remote - WSL 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 這可讓您使用 WSL 做為整合式開發環境，並為您處理相容性和路徑。 [深入了解](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果您已安裝 VS Code，則必須確定您具有 [1.35 May 版本](https://code.visualstudio.com/updates/v1_35)或更新版本，才能安裝 [Remote - WSL 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 若沒有 Remote - WSL 延伸模組，不建議您使用 WSL，因為您會失去自動完成、偵錯、linting 等的支援。有趣的事實：此 WSL 延伸模組安裝在 $HOME/.vscode-server/extensions 中。

## <a name="create-a-new-project"></a>建立新專案

讓我們在 Linux (Ubuntu) 檔案系統上建立新的專案目錄，接著使用 VS Code 搭配 Linux 應用程式和工具來處理此目錄。

1. 關閉 VS Code 並開啟 Ubuntu 18.04 (您的 WSL 命令列)，方法是移至您的 [開始] 功能表 (左下方的 Windows 圖示)，然後鍵入："Ubuntu 18.04"。

2. 在您的 Ubuntu 命令列中，瀏覽至您要放置專案的位置，並為其建立目錄：`mkdir HelloWorld`。

![Ubuntu 終端機](../images/ubuntu-terminal.png)

> [!TIP]
> 使用 Windows 子系統 Linux 版 (WSL) 時，必須記住的重要事項是**您現在正在兩個不同的檔案系統之間工作**：1) 您的 Windows 檔案系統，以及 2) 您的 Linux 檔案系統 (WSL)，即我們範例中使用的 Ubuntu。 您必須注意安裝套件和儲存檔案的位置。 您可以在 Windows 檔案系統中安裝一個或多個版本的工具或套件，以及在 Linux 檔案系統中安裝完全不同的版本。 更新 Windows 檔案系統中的工具將不會影響 Linux 檔案系統中的工具，反之亦然。 WSL 會將您電腦上的固定磁碟機掛接在 Linux 發行版本的 `/mnt/<drive>` 資料夾底下。 例如，您的 Windows C: 磁碟機掛接在 `/mnt/c/` 底下。 您可以從 Ubuntu 終端機存取 Windows 檔案，並在這些檔案上使用 Linux 應用程式和工具，反之亦然。 建議您在適用於 Python Web 開發的 Linux 檔案系統中工作，這是因為大部分的 Web 工具原本是針對 Linux 所撰寫，並部署在 Linux 生產環境中。 它也可避免混用檔案系統語意 (例如，Windows 在檔案名稱方面不區分大小寫)。 話雖如此，WSL 現在支援在 Linux 和 Windows 檔案系統之間進行跳躍，因此您可以在其中一個檔案上裝載您的檔案。 [深入了解](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)。

## <a name="install-python-pip-and-venv"></a>安裝 Python、pip 和 venv

Ubuntu 18.04 LTS 隨附已安裝的 Python 3.6，但不隨附您預期可能會在安裝其他 Python 安裝時取得的一些模組。 我們仍然需要安裝 **pip**、適用於 Python 的標準套件管理員，以及 **venv**，這是用來建立和管理輕量型虛擬環境的標準模組。  

1. 確認已安裝 Python3，方法是開啟您的 Ubuntu 終端機並輸入：`python3 --version`。 這應該會傳回您的 Python 版本號碼。 如果您需要更新您的 Python 版本，請先輸入下列命令以更新您的 Ubuntu 版本：`sudo apt update && sudo apt upgrade`，然後使用 `sudo apt upgrade python3` 更新 Python。

2. 輸入下列命令，以安裝 **pip**：`sudo apt install python3-pip`。 Pip 可讓您安裝和管理不屬於 Python 標準程式庫的其他套件。

3. 輸入下列命令，以安裝 **venv**：`sudo apt install python3-venv`。

## <a name="create-a-virtual-environment"></a>建立虛擬環境

使用虛擬環境是針對 Python 開發專案建議的最佳做法。 藉由建立虛擬環境，您可以隔離您的專案工具，並避免與其他專案的工具發生版本衝突。 例如，您可能會維護需要 Django 1.2 Web 架構的舊版 Web 專案，但接著使用 Django 2.2 來提供令人興奮的新專案。 如果您全域更新 Django，則在虛擬環境之外，您稍後可能會遇到一些版本控制問題。 除了防止發生意外的版本控制衝突以外，虛擬環境還可讓您在沒有系統管理權限的情況下安裝和管理套件。

1. 開啟您的終端機，並在 *HelloWorld* 專案資料夾內，使用下列命令建立名為 **venv** 的虛擬環境：`python3 -m venv .venv`。

2. 若要啟用虛擬環境，請輸入：`source .venv/bin/activate`。 如果運作正常，您應該會在命令提示字元前面看到 **(.venv)** 。 您現在有一個就緒的獨立式環境，可供您撰寫程式碼及安裝套件。 當您完成虛擬環境時，請輸入下列命令將它停用：`deactivate`。

    ![建立虛擬環境](../images/wsl-venv.png)

> [!TIP]
> 建議在您計劃擁有專案的目錄內建立虛擬環境。 因為每一個專案都應該有自己的個別目錄，而且每一個都將有自己的虛擬環境，因此不需要唯一的命名。 我們的建議是使用名稱 **. venv** 來遵循 Python 慣例。 如果您安裝至您的專案目錄中，則某些工具 (例如 pipenv) 也會預設為此名稱。 您不想要使用 **.env**，因為它會與環境變數定義檔發生衝突。 通常不建議使用非點開頭的名稱，因為您不需要 `ls` 持續提醒您該目錄存在。 也建議您將 **.venv** 新增至您的 .gitignore 檔案。 (以下是[適用於 Python 的 GitHub 預設 .gitignore 範本](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106)，供您參考。)如需在 VS Code 中使用虛擬環境的詳細資訊，請參閱[在 VS Code 中使用 Python 環境](https://code.visualstudio.com/docs/python/environments)。

## <a name="open-a-wsl---remote-window"></a>開啟 WSL - Remote 視窗

VS Code 使用 Remote - WSL 延伸模組 (先前已安裝)，將您的 Linux 子系統視為遠端伺服器。 這可讓您使用 WSL 做為整合式開發環境。 [深入了解](https://code.visualstudio.com/docs/remote/wsl)。

1. 從 Ubuntu 終端機中，於 VS Code 中開啟您的專案資料夾，方法是輸入：`code .` ("." 會告訴 VS Code 開啟目前的資料夾)。

2. 從 Windows Defender 會快顯一則安全性警示，請選取 [允許存取]。 開啟 VS Code 之後，您應該會在左下角看到遠端連線主機指示器，讓您知道您正在 **WSL：Ubuntu-18.04** 上進行編輯。

    ![VS Code 遠端連線主機指示器](../images/wsl-remote-extension.png)

3. 關閉您的 Ubuntu 終端機。 接下來我們將使用整合至 VS Code 的 WSL 終端機。

4. 在 VS Code 中開啟 WSL 終端機，方法是按 **Ctrl+`** (使用倒單引號字元)，或選取 [檢視] >  [終端機]。 這會開啟一個 bash (WSL) 命令列，並開啟至您在 Ubuntu 終端機中建立的專案資料夾路徑。

    ![VS Code 與 WSL 終端機](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>安裝 Microsoft Python 延伸模組

您將需要為 Remote - WSL 安裝任何 VS Code 延伸模組。 已在 VS Code 本機安裝的延伸模組將無法自動使用。 [深入了解](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)。

1. 輸入 **Ctrl+Shift+X** (或使用功能表瀏覽至 [檢視] >  [延伸模組])，以開啟 VS Code 延伸模組視窗。

2. 在頂端 [搜尋 Marketplace 中的延伸模組] 方塊中，輸入：**Python**。

3. 尋找 **Python (ms-python.python) by Microsoft** 延伸模組，然後選取綠色 [安裝] 按鈕。

4. 延伸模組完成安裝之後，您必須選取藍色 [需要重新載入] 按鈕。 這會重新載入VS Code，並在 VS Code 延伸模組視窗中顯示 [WSL：UBUNTU-18.04 - 已安裝] 區段，其中顯示您已安裝 Python 延伸模組。

## <a name="run-a-simple-python-program"></a>執行簡單的 Python 程式

Python 是一種解譯的語言，並支援不同類型的解譯器 (Python2、Anaconda、PyPy 等)。 VS Code 應該預設為與專案相關聯的解譯器。 如果您有變更它的原因，請選取目前顯示在 VS Code 視窗底部藍色列中的解譯器，或開啟 [命令選擇區] (Ctrl+Shift+P)，然後輸入命令 **Python:Select Interpreter**。 這會顯示您目前已安裝的 Python 解譯器清單。 [深入了解如何設定 Python 環境](https://code.visualstudio.com/docs/python/environments)。

讓我們建立並執行簡單的 Python 程式作為測試，並確保已選取正確的 Python 解譯器。

1. 輸入 **Ctrl+Shift+E** (或使用功能表瀏覽至 [檢視] >  [總管])，以開啟 VS Code 檔案總管視窗。

2. 如果尚未開啟，請輸入 **Ctrl+Shift+`** 來開啟整合式 WSL 終端機，並確定已選取 **HelloWorld** python 專案資料夾。

3. 輸入下列命令來建立 python 檔案：`touch test.py`。 您應該會看到剛才建立的檔案出現在您的 [總管] 視窗中，位於已在您專案目錄的 .venv 和 .vscode 資料夾之下。

4. 選取您剛才在 [總管] 視窗中建立的 **test.py** 檔案，以在 VS Code 中開啟該檔案。 因為檔案名稱中的 .py 告訴 VS Code，這是 Python 檔案，所以您先前載入的 Python 延伸模組會自動選擇並載入 Python 解譯器，而您會看到此解譯器顯示在 VS Code 視窗的底部。

    ![選取 VS Code 中的 Python 解譯器](../images/interpreterselection.gif)

5. 將此 Python 程式碼貼至您的 test.py 檔案中，然後儲存檔案 (Ctrl+S)： 

    ```python
    print("Hello World")
    ```

6. 若要執行剛才建立的 Python "Hello World" 程式，請在 [VS Code 總管] 視窗中選取 **test.py** 檔案，然後以滑鼠右鍵按一下該檔案，以顯示選項功能表。 選取 [在終端機中執行 Python 檔案]。 或者，在您的整合式 WSL 終端機視窗中，輸入 `python test.py` 以執行 "Hello World" 程式。 Python 解譯器會在您的終端機視窗中列印 "Hello World"。

恭喜。 您已完成所有設定，現在可以建立並執行 Python 程式！ 現在，讓我們嘗試使用兩個最受歡迎的 Python Web 架構來建立 Hello World 應用程式：Flask 和 Django。

## <a name="hello-world-tutorial-for-flask"></a>Flask 的 Hello World 教學課程

[Flask](http://flask.pocoo.org/) 是適用於 Python 的 Web 應用程式架構。 在此簡短教學課程中，您將使用 VS Code 和 WSL，建立小型 "Hello World" Flask 應用程式。

1. 開啟 Ubuntu 18.04 (您的 WSL 命令列)，方法是移至您的 [開始] 功能表 (左下方的 Windows 圖示)，然後鍵入："Ubuntu 18.04"。

2. 建立專案的目錄：`mkdir HelloWorld-Flask`，然後 `cd HelloWorld-Flask` 以輸入目錄。

3. 建立虛擬環境以安裝您的專案工具：`python3 -m venv .venv`

4. 輸入下列命令，在 VS Code 中開啟您的 **HelloWorld-Flask** 專案：`code .`

5. 在 VS Code 內，輸入 **Ctrl+Shift+`** 來開啟您的整合式 WSL 終端機 (也稱為 Bash) (您的 **HelloWorld-Flask** 專案資料夾應已選取)。 *關閉您的 Ubuntu 命令列，因為我們將在與 VS Code 整合的 WSL 終端機中工作，以繼續進行。*

6. 在 VS Code 中使用您 Bash 終端機，以啟用您在步驟 #3 中建立的虛擬環境：`source .venv/bin/activate`。 如果運作正常，您應該會在命令提示字元前面看到 (.venv)。

7. 輸入下列命令，在虛擬環境中安裝 Flask：`python3 -m pip install flask`。 輸入下列命令來驗證是否已安裝它：`python3 -m flask --version`。

8. 為您的 Python 程式碼建立新的檔案：`touch app.py`

9. 在 VS Code 的 [檔案總管] 中開啟您的 **app.py** 檔案 (`Ctrl+Shift+E`，然後選取您的 app.py 檔案)。 這會啟動 Python 延伸模組來選擇解譯器。 它應該預設為 **Python 3.6.8 64 位元 ('.venv': venv)** 。 請注意，它也會偵測到您的虛擬環境。

    ![已啟動虛擬環境](../images/virtual-environment.png)

10. 在 **app.py** 中，新增程式碼以匯入 Flask，並建立 Flask 物件的執行個體：

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. 此外，在 **app.py** 中，新增可傳回內容的函式，在此情況下，指的是簡單字串。 使用 Flask 的 **app.route** 裝飾項目，將 URL 路由 "/" 對應至該函式：

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 您可以在同一函式上使用多個裝飾項目 (每行一個)，取決於您想要將多少個不同的路由對應至同一函式。

12. 儲存 **app.py** 檔案 (**Ctrl + S**)。

13. 在終端機中，輸入下列命令來執行應用程式：

    ```python
    python3 -m flask run
    ```

    這會執行 Flask 開發伺服器。 根據預設，開發伺服器會尋找 **app.py**。 執行 Flask 時，您應會看到類似以下的輸出：

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 將您的預設網頁瀏覽器開啟至轉譯的頁面，**按住 Ctrl 並按一下** 終端機中的 http://127.0.0.1:5000/ URL。 您應該會在瀏覽器中看到下列訊息：

    ![Hello, Flask!](../images/hello-flask.png)

15. 請注意，當您造訪 "/" 之類的 URL 時，有一則訊息會出現在偵錯終端機中，其中顯示 HTTP 要求：

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. 在終端機中使用 **Ctrl + C** 來停止應用程式。

> [!TIP]
> 如果您想要使用與 **app.py** 不同的檔案名稱 (例如 **program.py**)，請定義名為 **FLASK_APP** 的環境變數，並將其值設定為您選擇的檔案。 然後，Flask 的程式開發伺服器會使用 **FLASK_APP** 的值，而不是預設檔案 **app.py**。 如需詳細資訊，請參閱 [Flask 的命令列介面文件](http://flask.pocoo.org/docs/1.0/cli/)。

恭喜，您已使用 Visual Studio Code 和 Windows 子系統 Linux 版建立了 Flask Web 應用程式！ 如需使用 VS Code 和 Flask 的深入教學課程，請參閱 [Visual Studio Code中的 Flask 教學課程](https://code.visualstudio.com/docs/python/tutorial-flask)。

## <a name="hello-world-tutorial-for-django"></a>Django 的 Hello World 教學課程

[Django](https://www.djangoproject.com) 是適用於 Python 的 Web 應用程式架構。 在此簡短教學課程中，您將使用 VS Code 和 WSL，建立小型 "Hello World" Django 應用程式。

1. 開啟 Ubuntu 18.04 (您的 WSL 命令列)，方法是移至您的 [開始] 功能表 (左下方的 Windows 圖示)，然後鍵入："Ubuntu 18.04"。

2. 建立專案的目錄：`mkdir HelloWorld-Django`，然後 `cd HelloWorld-Django` 以輸入目錄。

3. 建立虛擬環境以安裝您的專案工具：`python3 -m venv .venv`

4. 輸入下列命令，在 VS Code 中開啟您的 **HelloWorld-Django** 專案：`code .`

5. 在 VS Code 內，輸入 **Ctrl+Shift+`** 來開啟您的整合式 WSL 終端機 (也稱為 Bash) (您的 **HelloWorld-Django** 專案資料夾應已選取)。 *關閉您的 Ubuntu 命令列，因為我們將在與 VS Code 整合的 WSL 終端機中工作，以繼續進行。*

6. 在 VS Code 中使用您 Bash 終端機，以啟用您在步驟 #3 中建立的虛擬環境：`source .venv/bin/activate`。 如果運作正常，您應該會在命令提示字元前面看到 (.venv)。

7. 使用下列命令，在虛擬環境中安裝 Django：`python3 -m pip install django`。 輸入下列命令來驗證是否已安裝它：`python3 -m django --version`。

8. 接著，執行下列命令以建立 Django 專案：

    ```bash
    django-admin startproject web_project .
    ```

    `startproject` 命令會假設 (藉由在尾端使用 `.`) 目前資料夾是您的專案資料夾，並在其中建立下列檔案：

    - `manage.py`:適用於專案的 Django 命令列系統管理公用程式。 您可以使用 `python manage.py <command> [options]` 來執行專案的系統管理命令。

    - 名為 `web_project` 的子資料夾，其中包含下列檔案：
        - `__init__.py`：告訴 Python 此資料夾為 Python 套件的空白檔案。
        - `wsgi.py`：WSGI 相容 Web 伺服器的進入點，用來服務您的專案。 您通常會將此檔案保持原狀，因為它會提供生產 Web 伺服器的勾點。
        - `settings.py`：包含 Django 專案的設定，您可以在開發 Web 應用程式的過程中修改這些設定。
        - `urls.py`：包含 Django 專案的目錄，您也可以在開發過程中修改此目錄。

9. 若要驗證 Django 專案，請使用命令 `python3 manage.py runserver` 來啟動 Django 的開發伺服器。 伺服器會在預設通訊埠 8000 上執行，而且您應該會在終端機視窗中看到如下輸出：

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    當您第一次執行伺服器時，它會在檔案 `db.sqlite3` 中建立預設 SQLite 資料庫，這是專供開發之用，但可用於小量 Web 應用程式的生產中。 此外，Django 的內建 Web 伺服器*僅*供本機開發之用。 不過，當您部署至 Web 主機時，Django 會改為使用主機的 Web 伺服器。 Django 專案中的 `wsgi.py` 模組負責連結至生產伺服器。

    如果您想要使用與預設 8000 不同的連接埠，請在命令列上指定連接埠號碼，例如 `python3 manage.py runserver 5000`。

10. `Ctrl+click` 終端機輸出視窗中的 `http://127.0.0.1:8000/` URL，以將預設瀏覽器開啟至該位址。 如果 Django 已正確安裝且專案有效，您會看到預設頁面。 VS Code 終端機輸出視窗也會顯示伺服器記錄檔。

11. 當您完成時，請關閉瀏覽器視窗，並使用終端機輸出視窗中所指出的 `Ctrl+C`，在 VS Code 中停止伺服器。

12. 現在，若要建立 Django 應用程式，請在您的專案資料夾 (`manage.py` 所在的位置) 中執行系統管理公用程式的 `startapp` 命令：

    ```bash
    python3 manage.py startapp hello
    ```

    此命令會建立一個名為 `hello` 的資料夾，其中包含多個程式碼檔案和一個子資料夾。 其中，您經常會使用 `views.py` (包含定義 Web 應用程式中頁面的函式) 和 `models.py` (包含定義資料物件的類別)。 Django 的系統管理公用程式會使用 `migrations` 資料夾來管理資料庫版本，如本教學課程稍後所討論。 另外，還有檔案 `apps.py` (應用程式組態)、`admin.py` (用於建立系統管理介面) 和 `tests.py` (用於測試)，此處未涵蓋這些檔案。

13. 修改 `hello/views.py` 以符合下列程式碼，這會建立應用程式首頁的單一檢視：

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 使用下列內容建立檔案 `hello/urls.py`。 `urls.py` 檔案是您指定模式的位置，可將不同的 URL 路由傳送至適當的檢視。 下列程式碼包含一個路由，可將應用程式的根 URL(`""`) 對應至您剛新增至 `hello/views.py` 的 `views.home` 函式：

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. `web_project` 資料夾也包含 `urls.py` 檔案，亦即實際處理 URL 路由的位置。 開啟 `web_project/urls.py` 並加以修改以符合下列程式碼 (您可以視需要保留啟發性的註解)。 此程式碼會使用 `django.urls.include` 來提取應用程式的 `hello/urls.py`，這會保留應用程式內含的應用程式路由。 當專案包含多個應用程式時，此分隔很有用。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 儲存所有修改的檔案。

17. 在 VS Code 終端機中，使用 `python3 manage.py runserver` 執行開發伺服器，然後將瀏覽器開啟至 `http://127.0.0.1:8000/`，以查看呈現 "Hello, Django" 的頁面。

恭喜，您已使用 VS Code 和 Windows 子系統 Linux 版建立了 Django Web 應用程式！ 如需使用 VS Code 和 Django 的深入教學課程，請參閱 [Visual Studio Code中的 Django 教學課程](https://code.visualstudio.com/docs/python/tutorial-django)。

## <a name="additional-resources"></a>其他資源

- [使用 VS Code 的 Python 教學課程](https://code.visualstudio.com/docs/python/python-tutorial)：VS Code 作為 Python 環境的簡介教學課程，主要是如何編輯、執行和偵錯程式碼。
- [VS Code 中的 Git 支援](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)：了解如何在 VS Code 中使用 Git 版本控制基本概念。  
- [了解 WSL 2 即將推出的更新！](/windows/wsl/wsl2-index)：這個新版本會變更 Linux 發行版本與 Windows 之間的互動方式，進而增加檔案系統效能，以及新增完整的系統呼叫相容性。
- [在 Windows上使用多個 Linux 發行版本](/windows/wsl/wsl-config)：了解如何在您的 Windows 電腦上管理多個不同的 Linux 發行版本。
