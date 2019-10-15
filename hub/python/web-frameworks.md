---
title: 在 Windows 上使用 Python 進行 Web 開發
description: 如何開始在 Windows 上使用 Python 進行網頁程式開發，包括針對 Flask 和 Django 等架構進行設定。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python、windows 10、microsoft、python on windows、python web 和 wsl、python web 應用程式、適用于 linux 的 windows 子系統、windows 上的 python 網頁程式開發、windows 上的 flask 應用程式、windows 上的 django 應用程式、python web 上的 flask web dev、windows 上的 django web dev、windows web dev 搭配 python，vs code python web dev，遠端 wsl 擴充功能，ubuntu，wsl，venv，pip，microsoft python 擴充功能，在 windows 上執行 python，在 windows 上使用 python，在 windows 上以 python 建立
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 285e5149778f2d5cb63554a5af63bb9ae23809dc
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314942"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>開始在 Windows 上使用 Python 進行網頁程式開發

以下是使用適用于 Linux 的 Windows 子系統（WSL），讓您開始在 Windows 上使用 Python 進行 網頁程式開發的逐步指南。

## <a name="set-up-your-development-environment"></a>設定開發環境

我們建議您在建立 web 應用程式時，在 WSL 上安裝 Python。 Python 網頁程式開發的許多教學課程和指示都是針對 Linux 使用者所撰寫，並使用以 Linux 為基礎的封裝和安裝工具。 大部分的 web 應用程式也會部署在 Linux 上，因此這可確保您的開發與生產環境之間具有一致性。

如果您使用 Python 做為 網頁程式開發以外的專案，建議您使用 Microsoft Store 直接在 Windows 10 上安裝 Python。 WSL 不支援 GUI 桌面或應用程式（例如 PyGame、Gnome、KDE 等）。 在這些情況下，直接在 Windows 上安裝和使用 Python。 如果您不熟悉 Python，請參閱我們的指南：[開始在 Windows 上使用適用于初學者的 Python](./beginners.md)。 如果您想要將作業系統上的一般工作自動化，請參閱我們的指南：[開始在 Windows 上使用 Python 進行腳本處理和自動化](./scripting.md)。 針對某些先進的案例，您可能會想要考慮直接從[python.org](https://www.python.org/downloads/windows/)下載特定的 Python 版本，或考慮安裝[替代](https://www.python.org/download/alternatives)方案，例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。只有當您是更先進的 Python 程式設計人員，並具有選擇替代執行方式的特定原因時，才建議使用此方法。

## <a name="enable-windows-subsystem-for-linux"></a>啟用適用于 Linux 的 Windows 子系統

WSL 可讓您執行 GNU/Linux 環境（包括大部分的命令列工具、公用程式和應用程式），直接在 Windows 上進行修改，並與您的 Windows 檔案系統和我的最愛工具（例如 Visual Studio Code）完全整合。 啟用 WSL 之前，請先檢查您是否有[最新版本的 Windows 10](https://www.microsoft.com/software-download/windows10)。

若要在您的電腦上啟用 WSL，您需要：

1. 移至您的 [開始] 功能表（左下方的 Windows 圖示），輸入 **「** 開啟或關閉 windows 功能」，然後選取 [**控制台**] 的連結以開啟 [ **windows 功能**] 快顯功能表。 在清單中尋找「適用于 Linux 的 Windows 子系統」，然後選取核取方塊以開啟此功能。

2. 出現提示時，重新啟動您的電腦。

## <a name="install-a-linux-distribution"></a>安裝 Linux 散發套件

有數個 Linux 散發套件可在 WSL 上執行。 您可以在 Microsoft Store 中尋找並安裝我的最愛。 我們建議從[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)開始，因為它是最新、熱門且受到妥善支援。

1. 開啟此[Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q)連結，開啟 [Microsoft Store]，然後選取 [**取得**]。 *（這是相當大的下載，可能需要一些時間才能安裝）。*

2. 下載完成之後，從 [**開始**] 功能表中，選取 [從 Microsoft Store**啟動**] 或 [啟動]，方法是輸入 "Ubuntu 18.04 LTS"。

3. 當您第一次執行散發套件時，系統會要求您建立帳戶名稱和密碼。 在此之後，預設會自動以此使用者身分登入。 您可以選擇任何使用者名稱和密碼。 它們不會影響您的 Windows 使用者名稱。

您可以藉由輸入下列內容來檢查您目前使用的 Linux 散發套件： `lsb_release -d`。 若要更新您的 Ubuntu 散發套件，請使用： `sudo apt update && sudo apt upgrade`。 我們建議定期更新，以確保您擁有最新的套件。 Windows 不會自動處理這種更新。 如需 Microsoft Store 中可用的其他 Linux 發行版本連結、替代安裝方法或疑難排解，請參閱適用于[windows 10 的 Windows 子系統 For Linux 安裝指南](https://docs.microsoft.com/windows/wsl/install-win10)。

## <a name="set-up-visual-studio-code"></a>設定 Visual Studio Code

藉由使用 VS Code，利用[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense)、 [Linting](https://code.visualstudio.com/docs/python/linting)、[偵錯工具支援](https://code.visualstudio.com/docs/python/debugging)、[程式碼片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)和[單元測試](https://code.visualstudio.com/docs/python/unit-testing)。 VS Code 與適用于 Linux 的 Windows 子系統完美整合，提供[內建的終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機，讓您在程式碼編輯器與命令列之間建立順暢的工作流程，以及支援 git 以搭配一般 git[進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)直接內建在 UI 中的命令（新增、認可、推送、提取）。

1. [下載並安裝適用于 Windows 的 VS Code](https://code.visualstudio.com)。 VS Code 也適用于 Linux，但適用于 Linux 的 Windows 子系統不支援 GUI 應用程式，因此我們需要將它安裝在 Windows 上。 別擔心，您仍然可以使用遠端 WSL 擴充功能，與您的 Linux 命令列和工具整合。

2. 在 VS Code 上安裝[遠端 WSL 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 這可讓您使用 WSL 做為整合式開發環境，並為您處理相容性和路徑。 [進一步瞭解](https://code.visualstudio.com/docs/remote/remote-overview)。

> [!IMPORTANT]
> 如果您已安裝 VS Code，則必須確定您有[1.35 版](https://code.visualstudio.com/updates/v1_35)或更新版本，才能安裝[遠端 WSL 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)。 不建議您在沒有遠端 WSL 擴充功能的 VS Code 中使用 WSL，因為您會失去自動完成、偵錯工具、linting 等的支援。有趣的事實：此 WSL 延伸模組安裝在 $HOME/.vscode-server/extensions。

## <a name="create-a-new-project"></a>建立新專案

讓我們在我們的 Linux （Ubuntu）檔案系統上建立新的專案目錄，我們接著會使用 VS Code 來處理 Linux 應用程式和工具。

1. 關閉 VS Code 並開啟 Ubuntu 18.04 （您的 WSL 命令列），方法是移至您的 [**開始**] 功能表（左下方的 Windows 圖示），然後輸入："Ubuntu 18.04"。

2. 在您的 Ubuntu 命令列中，流覽至您要放置專案的位置，並為其建立目錄： `mkdir HelloWorld`。

![Ubuntu 終端機](../images/ubuntu-terminal.png)

> [!TIP]
> 使用適用于 Linux 的 Windows 子系統（WSL）時必須記住的重要事項是，**您現在可以在兩個不同的檔案系統之間運作**：1）您的 Windows 檔案系統，以及2）您的 Linux 檔案系統（WSL），這是適用于我們範例的 Ubuntu。 您必須注意安裝封裝和儲存檔案的位置。 您可以在 Windows 檔案系統中安裝一個或多個版本的工具或套件，以及 Linux 檔案系統中完全不同的版本。 更新 Windows 檔案系統中的工具將不會影響 Linux 檔案系統中的工具，反之亦然。 WSL 會將您電腦上的固定磁片磁碟機掛接在 Linux 散發套件的 @no__t 0 資料夾底下。 例如，您的 Windows C：磁片磁碟機會裝載在 `/mnt/c/` 之下。 您可以從 Ubuntu 終端機存取 Windows 檔案，並在這些檔案上使用 Linux 應用程式和工具，反之亦然。 我們建議您在適用于 Python 網頁程式開發的 Linux 檔案系統中工作，這是因為大部分的 web 工具原本是針對 Linux 所撰寫，並部署在 Linux 生產環境中。 它也可避免混用檔案系統的語義（例如，Windows 在檔案名方面並不區分大小寫）。 話雖如此，WSL 現在支援在 Linux 和 Windows 檔案系統之間進行跳躍，因此您可以在其中一個檔案上裝載您的檔案。 [進一步瞭解](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/)。 我們也很高興分享該[WSL2 即將推出 Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) ，並提供一些絕佳的改良功能。 您可以[在 Windows 測試人員組建18917上立即試用](https://docs.microsoft.com/windows/wsl/wsl2-install)。

## <a name="install-python-pip-and-venv"></a>安裝 Python、pip 和 venv

Ubuntu 18.04 LTS 隨附已安裝的 Python 3.6，但不包含您可能會預期會透過其他 Python 安裝取得的一些模組。 我們仍然需要安裝**pip**、適用于 Python 的標準套件管理員，以及**venv**，這是用來建立和管理輕量虛擬環境的標準模組。  

1. 開啟您的 Ubuntu 終端機，並輸入： `python3 --version`，確認已安裝 Python3。 這應該會傳回您的 Python 版本號碼。 如果您需要更新您的 Python 版本，請先輸入下列內容以更新您的 Ubuntu 版本： `sudo apt update && sudo apt upgrade`，然後使用 `sudo apt upgrade python3` 更新 Python。

2. 輸入： `sudo apt install python3-pip` 來安裝**pip** 。 Pip 可讓您安裝和管理不屬於 Python 標準程式庫的其他套件。

3. 輸入下列程式來安裝**venv** ： `sudo apt install python3-venv`。

## <a name="create-a-virtual-environment"></a>建立虛擬環境

使用虛擬環境是 Python 開發專案的建議最佳作法。 藉由建立虛擬環境，您可以隔離您的專案工具，並避免與其他專案的工具進行版本設定衝突。 例如，您可能會維護需要 Django 1.2 web 架構的舊版 Web 專案，但接著會使用 Django 2.2 來提供令人興奮的新專案。 如果您全域更新 Django，在虛擬環境之外，您稍後可能會遇到一些版本設定問題。 除了防止意外的版本控制衝突以外，虛擬環境可讓您在沒有系統管理許可權的情況下，安裝和管理套件。

1. 開啟您的終端機，並在您的*HelloWorld*專案資料夾內，使用下列命令建立名為 **. venv**： `python3 -m venv .venv` 的虛擬環境。

2. 若要啟用虛擬環境，請輸入： `source .venv/bin/activate`。 如果運作正常，您應該會在命令提示字元之前看到 **（. venv）** 。 您現在有一個獨立環境，可供撰寫程式碼及安裝套件。 當您完成虛擬環境時，請輸入下列命令將它停用： `deactivate`。

    ![建立虛擬環境](../images/wsl-venv.png)

> [!TIP]
> 建議您在想要擁有專案的目錄內建立虛擬環境。 因為每個專案都應該有自己的個別目錄，每個都有自己的虛擬環境，因此不需要唯一的命名。 我們的建議是使用**venv**來遵循 Python 慣例。 如果您將安裝在您的專案目錄中，某些工具（例如 pipenv）也會預設為此名稱。 您不想要使用**env**做為與環境變數定義檔衝突的。 我們通常不建議以非點符開頭的名稱，因為您不需要 `ls` 會持續提醒您該目錄是否存在。 我們也建議您將**venv**新增至您的 .gitignore 檔案。 （這裡是適用于[Python 的 GitHub 預設 .gitignore 範本](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106)，供您參考）。如需在 VS Code 中使用虛擬環境的詳細資訊，請參閱[在 VS Code 中使用 Python 環境](https://code.visualstudio.com/docs/python/environments)。

## <a name="open-a-wsl---remote-window"></a>開啟 WSL-遠端視窗

VS Code 使用遠端 WSL 擴充功能（先前已安裝）將您的 Linux 子系統視為遠端伺服器。 這可讓您使用 WSL 做為整合式開發環境。 [進一步瞭解](https://code.visualstudio.com/docs/remote/wsl)。 

1. 輸入下列內容，在您的 Ubuntu 終端機 VS Code 中開啟您的專案資料夾： `code .` （"." 會告訴 VS Code 開啟目前的資料夾）。

2. 系統會從 Windows Defender 彈出一則安全性警示，並選取 [允許存取]。 一旦 VS Code 開啟，您應該會看到位於左下角的遠端連線主機指示器，讓您知道您正在編輯 @no__t 0WSL：Ubuntu-18.04 @ no__t-0。

    ![VS Code 遠端連線主機指標](../images/wsl-remote-extension.png)

3. 關閉您的 Ubuntu 終端機。 接下來我們將使用整合到 VS Code 的 WSL 終端機。

4. 按**Ctrl + '** （使用倒引號字元）或選取**View** > **終端**機，在 VS Code 中開啟 WSL 終端機。 這會開啟一個 bash （WSL）命令列，並開啟您在 Ubuntu 終端機中建立的專案資料夾路徑。

    ![使用 WSL 終端機 VS Code](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>安裝 Microsoft Python 擴充功能

您將需要為您的遠端 WSL 安裝任何 VS Code 延伸模組。 已在 VS Code 本機安裝的延伸模組將無法自動使用。 [進一步瞭解](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions)。

1. 輸入**Ctrl + Shift + X**以開啟 [VS Code 延伸模組] 視窗（或使用功能表流覽以**查看** > **延伸**模組）。

2. 在 [Marketplace 的頂端**搜尋擴充**功能] 方塊中，輸入：**Python**。

3. 尋找 Microsoft 擴充功能的**Python （ms python python）** ，然後選取綠色的 [**安裝**] 按鈕。

4. 延伸模組完成安裝之後，您必須選取藍色的 [**需要重載**] 按鈕。 這會重載 VS Code 並顯示 @no__t 0WSL：UBUNTU-18.04-[VS Code 擴充功能] 視窗中的 [已安裝 @ no__t-0] 區段，其中顯示您已安裝 Python 延伸模組。

## <a name="run-a-simple-python-program"></a>執行簡單的 Python 程式

Python 是一種轉譯的語言，支援不同類型的 interpretors （Python2、Anaconda、PyPy 等）。 VS Code 應該預設為與專案相關聯的解譯器。 如果您有需要變更它的原因，請選取目前顯示在 VS Code 視窗底部藍色列中的解譯器，或開啟**命令**選擇區（Ctrl + Shift + P），然後輸入命令 **Python：選取 [解譯器 @ no__t-0]。 這會顯示您目前已安裝的 Python 解譯器清單。 [深入瞭解如何設定 Python 環境](https://code.visualstudio.com/docs/python/environments)。

讓我們建立並執行簡單的 Python 程式作為測試，並確定已選取正確的 Python 解譯器。

1. 輸入**Ctrl + Shift + E** （或使用功能表流覽至**View** > **Explorer**），以開啟 [VS Code 檔案瀏覽器] 視窗。

2. 如果尚未開啟，請輸入**Ctrl + Shift + '** 來開啟您的整合式 WSL 終端機，並確定已選取您的**HelloWorld** python 專案資料夾。

3. 輸入： `touch test.py` 來建立 python 檔案。 您應該會看到剛才建立的檔案會出現在您的 [venv] 和 [vscode] 資料夾底下的專案目錄中。

4. 選取您剛才在 [瀏覽器] 視窗中建立的**test.py**檔案，以在 VS Code 中開啟它。 因為 .py 在我們的檔案名中告訴 VS Code 這是 Python 檔案，所以您先前載入的 Python 擴充功能會自動選擇並載入 Python 解譯器，您會看到它顯示在 VS Code 視窗的底部。

    ![選取 VS Code 中的 Python 解譯器](../images/interpreterselection.gif)

5. 將此 Python 程式碼貼到您的 test.py 檔案中，然後儲存檔案（Ctrl + S）： 

    ```python
    print("Hello World")
    ```

6. 若要執行我們剛才建立的 Python "Hello World" 程式，請在 [VS Code Explorer] 視窗中選取**test.py**檔案，然後以滑鼠右鍵按一下該檔案，以顯示選項功能表。 選取 [**在終端機中執行 Python**檔案]。 或者，在您的整合式 WSL 終端機視窗中，輸入： `python test.py` 來執行您的「Hello World」程式。 Python 解譯器會在您的終端機視窗中列印「Hello World」。

那麼. 您已準備好建立和執行 Python 程式！ 現在，讓我們試著使用兩個最受歡迎的 Python web 架構來建立 Hello World 應用程式：Flask 和 Django。

## <a name="hello-world-tutorial-for-flask"></a>Flask 的 Hello World 教學課程

[Flask](http://flask.pocoo.org/)是適用于 Python 的 web 應用程式架構。 在此簡短教學課程中，您將使用 VS Code 和 WSL 建立小型的「Hello World」 Flask 應用程式。

1. 移至 [**開始**] 功能表（左下方的 Windows 圖示）並輸入下列命令，以開啟 Ubuntu 18.04 （您的 WSL 命令列）："Ubuntu 18.04"。

2. 建立專案的目錄： `mkdir HelloWorld-Flask`，然後 `cd HelloWorld-Flask` 輸入目錄。

3. 建立虛擬環境以安裝您的專案工具： `python3 -m venv .venv`

4. 輸入下列命令，在 VS Code 中開啟您的**HelloWorld Flask**專案： `code .`

5. 在 VS Code 中，輸入**Ctrl + Shift + '** 來開啟您的整合式 WSL 終端機（也稱為 Bash）（您的**HelloWorld-Flask**專案資料夾應已選取）。 *關閉您的 Ubuntu 命令列，因為我們將在與 VS Code 整合的 WSL 終端機中工作。*

6. 使用 VS Code 中的 Bash 終端機，啟用您在步驟 #3 中建立的虛擬環境： `source .venv/bin/activate`。 如果運作正常，您應該會在命令提示字元之前看到（. venv）。

7. 輸入下列內容，在虛擬環境中安裝 Flask： `python3 -m pip install flask`。 輸入下列內容來確認是否已安裝： `python3 -m flask --version`。

8. 為您的 Python 程式碼建立新的檔案： `touch app.py`

9. 在 VS Code 的檔案瀏覽器中開啟您的**app.py**檔案（`Ctrl+Shift+E`，然後選取您的 app.py 檔案）。 這會啟動 Python 延伸模組來選擇解譯器。 它應該預設為**Python 3.6.8 64 位（'. venv '： venv）** 。 請注意，它也偵測到您的虛擬環境。

    ![已啟用虛擬環境](../images/virtual-environment.png)

10. 在**app.py**中，新增程式碼以匯入 Flask，並建立 Flask 物件的實例：

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. 此外，在**app.py**中，新增可傳回內容的函式，在此案例中為簡單的字串。 使用 Flask 的**應用程式。路由**裝飾專案會將 URL 路由 "/" 對應至該函式：

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > 您可以在相同的函式上使用多個裝飾專案（每行一個），視您想要對應至相同函式的不同路由數目而定。

12. 儲存**app.py**檔案（**Ctrl + S**）。

13. 在終端機中，輸入下列命令來執行應用程式：

    ```python
    python3 -m flask run
    ```

    這會執行 Flask 開發伺服器。 根據預設，開發伺服器會尋找**app.py** 。 當您執行 Flask 時，您應該會看到如下所示的輸出：

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. 將您的預設網頁瀏覽器開啟至轉譯的頁面， **Ctrl + 按一下**終端機中的 [http://127.0.0.1:5000/ URL]。 您應該會在瀏覽器中看到下列訊息：

    ![您好，Flask！](../images/hello-flask.png)

15. 請注意，當您造訪 "/" 之類的 URL 時，會在顯示 HTTP 要求的 [偵錯工具] 終端機中出現一則訊息：

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. 在終端機中使用**Ctrl + C**來停止應用程式。

> [!TIP]
> 如果您想要使用不同于**app.py**的檔案名，例如**program.py**，請定義名為**FLASK_APP**的環境變數，並將其值設定為您所選擇的檔案。 然後，Flask 的程式開發伺服器會使用**FLASK_APP**的值，而不是預設的 file **app.py**。 如需詳細資訊，請參閱[Flask 的命令列介面檔](http://flask.pocoo.org/docs/1.0/cli/)。

恭喜，您已使用 Visual Studio Code 和適用于 Linux 的 Windows 子系統建立了 Flask web 應用程式！ 如需使用 VS Code 和 Flask 的更深入教學課程，請參閱[Visual Studio Code 中的 Flask 教學](https://code.visualstudio.com/docs/python/tutorial-flask)課程。

## <a name="hello-world-tutorial-for-django"></a>Django 的 Hello World 教學課程

[Django](https://www.djangoproject.com)是適用于 Python 的 web 應用程式架構。 在此簡短教學課程中，您將使用 VS Code 和 WSL 建立小型的「Hello World」 Django 應用程式。

1. 移至 [**開始**] 功能表（左下方的 Windows 圖示）並輸入下列命令，以開啟 Ubuntu 18.04 （您的 WSL 命令列）："Ubuntu 18.04"。

2. 建立專案的目錄： `mkdir HelloWorld-Django`，然後 `cd HelloWorld-Django` 輸入目錄。

3. 建立虛擬環境以安裝您的專案工具： `python3 -m venv .venv`

4. 輸入下列命令，在 VS Code 中開啟您的**HelloWorld DJango**專案： `code .`

5. 在 VS Code 中，輸入**Ctrl + Shift + '** 來開啟您的整合式 WSL 終端機（也稱為 Bash）（您的**HelloWorld-Django**專案資料夾應已選取）。 *關閉您的 Ubuntu 命令列，因為我們將在與 VS Code 整合的 WSL 終端機中工作。*

6. 使用 VS Code 中的 Bash 終端機，啟用您在步驟 #3 中建立的虛擬環境： `source .venv/bin/activate`。 如果運作正常，您應該會在命令提示字元之前看到（. venv）。

7. 使用下列命令在虛擬環境中安裝 Django： `python3 -m pip install django`。 輸入下列內容來確認是否已安裝： `python3 -m django --version`。

8. 接下來，執行下列命令來建立 Django 專案：

    ```bash
    django-admin startproject web_project .
    ```

    @No__t-0 命令會假設目前的資料夾是您的專案資料夾，並在其中建立下列內容，以在結尾使用 `.`）：

    - `manage.py`:專案的 Django 命令列系統管理公用程式。 您可以使用 `python manage.py <command> [options]` 來執行專案的系統管理命令。

    - 名為 `web_project` 的子資料夾，其中包含下列檔案：
        - `__init__.py`：指出 Python 此資料夾為 Python 套件的空白檔案。
        - `wsgi.py`： WSGI 相容 web 伺服器的進入點，以服務您的專案。 您通常會將此檔案保持原狀，因為它會提供生產 web 伺服器的勾點。
        - `settings.py`：包含 Django 專案的設定，您可以在開發 web 應用程式的過程中加以修改。
        - `urls.py`：包含 Django 專案的目錄，您也可以在開發過程中修改。

9. 若要驗證 Django 專案，請使用命令 `python3 manage.py runserver` 來啟動 Django 的程式開發伺服器。 伺服器會在預設通訊埠8000上執行，您應該會在終端機視窗中看到類似下列輸出的輸出：

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    當您第一次執行伺服器時，它會在檔案中建立預設的 SQLite 資料庫 `db.sqlite3`，適用于開發用途，但可用於適用于低容量 web 應用程式的生產環境。 此外，Django 的內建 web 伺服器*僅*供本機開發之用。 不過，當您部署到 web 主機時，Django 會改為使用主機的 web 伺服器。 Django 專案中的 @no__t 0 模組會負責連結到實際執行伺服器。

    如果您想要使用與預設8000不同的埠，請在命令列上指定埠號碼，例如 `python3 manage.py runserver 5000`。

10. `Ctrl+click` [終端機輸出] 視窗中的 `http://127.0.0.1:8000/` URL，以開啟您的預設瀏覽器至該位址。 如果 Django 已正確安裝且專案有效，您會看到預設頁面。 [VS Code 終端機輸出] 視窗也會顯示伺服器記錄檔。

11. 當您完成時，請關閉瀏覽器視窗，並在 VS Code 中使用 `Ctrl+C` 來停止伺服器，如 [終端機輸出] 視窗中所示。

12. 現在，若要建立 Django 應用程式，請在您的專案資料夾（其中 `manage.py` 所在的位置）中執行系統管理公用程式的 `startapp` 命令：

    ```bash
    python3 manage.py startapp hello
    ```

    此命令會建立名為 `hello` 的資料夾，其中包含多個程式碼檔案和一個子資料夾。 在這些情況下，您經常會使用 `views.py` （其中包含定義 web 應用程式中頁面的函式）和 `models.py` （其中包含定義您的資料物件的類別）。 Django 的系統管理公用程式會使用 @no__t 0 資料夾來管理資料庫版本，如本教學課程稍後所述。 另外還有檔案 `apps.py` （應用程式設定），`admin.py` （用於建立系統管理介面），而 `tests.py` （適用于測試），此處未涵蓋。

13. 修改 `hello/views.py` 以符合下列程式碼，這會為應用程式的首頁建立單一視圖：

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. 使用下列內容建立檔案，`hello/urls.py`。 @No__t 0 檔案是您指定模式的位置，可將不同的 Url 路由傳送至適當的視圖。 下列程式碼包含一個路由，可將應用程式的根 URL （`""`）對應到您剛才新增至 `hello/views.py` 的 `views.home` 函數：

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. @No__t 0 資料夾也包含 @no__t 1 檔案，也就是 URL 路由的實際處理位置。 開啟 `web_project/urls.py`，並加以修改以符合下列程式碼（您可以視需要保留有意義的批註）。 這段程式碼會使用 `django.urls.include` 提取應用程式的 `hello/urls.py`，這會保留應用程式中包含的應用程式路由。 當專案包含多個應用程式時，這項分隔會很有説明。

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. 儲存所有已修改的檔案。

17. 在 VS Code 終端機中，執行具有 `python manage.py runserver` 的程式開發伺服器，然後開啟瀏覽器以 `http://127.0.0.1:8000/`，以查看呈現 "Hello，Django" 的頁面。

恭喜，您已使用 VS Code 和適用于 Linux 的 Windows 子系統建立了 Django web 應用程式！ 如需使用 VS Code 和 Django 的更深入教學課程，請參閱[Visual Studio Code 中的 Django 教學](https://code.visualstudio.com/docs/python/tutorial-django)課程。

## <a name="additional-resources"></a>其他資源

- [使用 VS Code 的 Python 教學](https://code.visualstudio.com/docs/python/python-tutorial)課程：以 Python 環境 VS Code 的簡介教學課程，主要是如何編輯、執行和偵錯工具代碼。
- [VS Code 中的 Git 支援](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)：瞭解如何在 VS Code 中使用 Git 版本控制基本概念。  
- [深入瞭解 WSL 2！即將推出的更新](https://docs.microsoft.com/windows/wsl/wsl2-index)：這個新版本會變更 Linux 散發與 Windows 之間的互動方式、增加檔案系統效能，以及新增完整的系統呼叫相容性。
- [在 Windows 上使用多個 Linux 發行](https://docs.microsoft.com/windows/wsl/wsl-config)版：瞭解如何在您的 Windows 電腦上管理多個不同的 Linux 發行版本。
