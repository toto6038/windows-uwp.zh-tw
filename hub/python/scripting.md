---
title: 在 Windows 上使用 Python 進行指令碼處理和自動化
description: 如何開始在 Windows 上使用 Python 進行指令碼處理、自動化和系統管理。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python 系統管理, python 檔案自動化, windows 上的 python 指令碼, 在 windows 上設定 python, windows 上的 python 開發人員環境, windows 上的 python 開發環境, python 搭配 powershell, 適用於檔案系統工作的 python 指令碼
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d465d46a0524345a45dff9b1cc7c425e4cb468a4
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "79208993"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>開始在 Windows 上使用 Python 進行指令碼處理和自動化

以下是逐步指南，指引您設定開發人員環境，並開始使用 Python 進行指令碼處理，以及將 Windows 上的檔案系統作業自動化。

> [!NOTE]
> 本文將涵蓋如何設定您的環境，以使用 Python 中一些有用的程式庫，跨平臺自動執行工作，例如搜尋您的檔案系統、存取網際網路、剖析檔案類型等，這些動作都是採取以 Windows 為中心的方法。 針對 Windows 特有的作業，請參閱 [ctypes](https://docs.python.org/3/library/ctypes.html) (適用於 Python 的 C 相容外部函式程式庫)、[winreg](https://docs.python.org/3/library/winreg.html) (將 Windows 登錄 API 公開至 Python 的函式)，以及 [Python/WinRT](https://pypi.org/project/winrt/) (可讓您從 Python 存取 Windows 執行階段 API)。

## <a name="set-up-your-development-environment"></a>設定開發環境

使用 Python 撰寫執行檔案系統作業的指令碼時，建議您從 [Microsoft Store 安裝 Python](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 透過 Microsoft Store 進行安裝會使用基本 Python3 解譯器，但會處理目前使用者的 PATH 設定 (避免需要系統管理員存取權)，而且還會提供自動更新。

如果您在 Windows 上使用 Python 進行  **Web 開發**，建議您使用 Windows 子系統 Linux 版進行不同的設定。 在我們的指南中尋找逐步解說：[開始在 Windows 上使用 Python 進行 Web 開發](./web-frameworks.md)。 如果您初次使用 Python，請試用我們的指南：[開始在 Windows 上使用適用於初學者的 Python](./beginners.md)。 對於某些進階案例 (例如，需要存取/修改 Python 的安裝檔案、製作二進位檔的複本，或直接使用 Python DL)，您可能會想要考慮直接從 [python.org](https://www.python.org/downloads/) 下載特定的 Python 版本，或考慮安裝[替代項目](https://www.python.org/download/alternatives)，例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。只有當您是資深 Python 程式設計人員，且有選擇替代實作的特定原因時，才建議這樣做。

## <a name="install-python"></a>安裝 Python

若要使用 Microsoft Store 安裝 Python：

1. 移至 [開始]  功能表 (左下方的 Windows 圖示)，鍵入 "Microsoft Store"，選取連結以開啟市集。

2. 市集開啟之後，從右上方功能表中選取 [搜尋]  ，然後輸入 "Python"。 從 [應用程式] 底下的結果開啟 "Python 3.7"。 選取 [取得]  。

3. 當 Python 下載結束並完成安裝程序之後，請使用 [開始]  功能表 (左下方的 Windows 圖示) 開啟 Windows PowerShell。 開啟 PowerShell 之後，請輸入 `Python --version` 以確認 Python3 已安裝在您的電腦上。

4. Python 的 Microsoft Store 安裝包括 **pip** (標準套件管理員)。 Pip 可讓您安裝和管理不屬於 Python 標準程式庫的其他套件。 若要確認您也有可用來安裝和管理套件的 pip，請輸入 `pip --version`。

## <a name="install-visual-studio-code"></a>安裝 Visual Studio Code

藉由使用 VS Code 做為文字編輯器/整合式開發環境 (IDE)，您可以利用 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (協助完成程式碼)、[Linting](https://code.visualstudio.com/docs/python/linting) (協助避免在程式碼中發生錯誤)、[偵錯支援](https://code.visualstudio.com/docs/python/debugging) (在執行程式碼之後，協助您在其中找出錯誤)、[程式碼片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (小型可重覆使用程式碼區塊的範本)，以及[單元測試](https://code.visualstudio.com/docs/python/unit-testing) (以不同類型的輸入來測試程式碼的介面)。

下載 VS Code for Windows，並遵循安裝指示：[https://code.visualstudio.com](https://code.visualstudio.com)。

## <a name="install-the-microsoft-python-extension"></a>安裝 Microsoft Python 延伸模組

您必須安裝 Microsoft Python 延伸模組，才能利用 VS Code 支援功能。 [深入了解](https://code.visualstudio.com/docs/languages/python)。

1. 輸入 **Ctrl+Shift+X** (或使用功能表瀏覽至 [檢視]   >  [延伸模組]  )，以開啟 VS Code 延伸模組視窗。

2. 在頂端 [搜尋 Marketplace 中的延伸模組]  方塊中，輸入：**Python**。

3. 尋找 **Python (ms-python.python) by Microsoft** 延伸模組，然後選取綠色 [安裝]  按鈕。

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>在 VS Code 中開啟整合式 PowerShell 終端機

VS Code 包含[內建終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)，可讓您使用 PowerShell 開啟 Python 命令列，以在程式碼編輯器與命令列之間建立順暢的工作流程。

1. 在 VS Code 中開啟終端機、選取 [檢視]   >  [終端機]  ，或者使用快速鍵 **Ctrl+`** (使用倒單引號字元)。

    > [!NOTE]
    > 預設終端機應該是 PowerShell，但如果您需要變更它，請使用 **Ctrl+Shift+P** 以進入命令面板。 輸入 **Terminal:Select Default Shell**，此時將會顯示終端機選項清單，其中包含 PowerShell、命令提示字元、WSL 等。選取您想要使用的終端機選項，並輸入 **Ctrl+Shift+`** (使用倒單引號) 來開啟新的終端機。

2. 在 VS Code 終端機內，輸入下列命令來開啟 Python：`python`

3. 輸入下列命令來試用 Python 解譯器：`print("Hello World")`。 Python 會傳回您的陳述式 "Hello World"。

    ![VS Code 中的 Python 命令列](../images/python-in-vscode.png)

4. 若要結束 Python，您可以輸入 `exit()`、`quit()`，或選取 Ctrl-Z。

## <a name="install-git-optional"></a>安裝 Git (選用)

如果您計畫在 Python 程式碼上與其他人合作，或在開放原始碼網站 (如 GitHub) 上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並讓常用的 Git 命令 (add、commit、push、pull) 直接內建在 UI 中。 您首先必須安裝 Git，才能強化 [原始檔控制] 面板。

1. 從 [git-scm 網站](https://git-scm.com/download/win)下載並安裝適用於 Windows 的 Git。

2. 隨附的安裝精靈會詢問您有關 Git 安裝設定的一系列問題。 建議您使用所有預設設定，除非您有特定原因，非變更某些設定不可。

3. 如果您之前從未使用過 Git，[GitHub 指南](https://guides.github.com/)可以協助您開始使用。

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>顯示檔案系統目錄結構的範例指令碼

常見的系統管理工作可能需要相當長的時間，但使用 Python 指令碼時，您可以將這些工作自動化，讓它們完全不需要任何時間。 例如，Python 可以讀取電腦檔案系統的內容並執行作業，例如列印檔案和目錄的大綱、將資料夾從某個目錄移到另一個目錄，或重新命名數百個檔案。 一般來說，如果您要手動執行這類工作，可能需要大量時間。 請改用 Python 指令碼！

讓我們從一個簡單的指令碼開始，它會逐步解說目錄樹狀結構並顯示目錄結構。

1. 使用 [開始]  功能表 (左下方的 Windows 圖示) 來開啟 PowerShell。

2. 建立專案的目錄：`mkdir python-scripts`，然後開啟該目錄：`cd python-scripts`。

3. 建立幾個目錄，以搭配我們的範例指令碼使用：

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. 在這些目錄內建立一些檔案，以搭配我們的指令碼使用：

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. 在您的 python 指令碼目錄中建立新的 python 檔案：

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. 輸入下列命令，在 VS Code 中開啟您的專案：`code .`

7. 輸入 **Ctrl+Shift+E** 來開啟 [VS Code 檔案總管] 視窗 (或使用功能表瀏覽至 [檢視]   >  [總管]  )，然後選取您剛建立的 list-directory-contents.py 檔案。 Microsoft Python 延伸模組將會自動載入 Python 解譯器。 您可以 VS Code 視窗底部查看已載入哪個解譯器。

    > [!NOTE]
    > Python 是一種解譯的語言，這表示它會作為虛擬機器，模擬實體電腦。 有不同類型的 Python 解譯器可供您使用：Python 2、Python 3、Anaconda、PyPy 等。若要執行 Python 程式碼，並取得 Python IntelliSense，您必須告訴 VS Code 使用哪個解譯器。 除非您有特定原因，非選擇不同的解譯器不可，否則建議您堅持使用 VS Code 預設選擇的解譯器 (我們案例中的 Python 3)。 若要變更 Python 解譯器，請選取目前顯示在 VS Code 視窗底部藍色列中的解譯器，或開啟 [命令選擇區]  (Ctrl+Shift+P)，然後輸入命令 **Python:Select Interpreter**。 這會顯示您目前已安裝的 Python 解譯器清單。 [深入了解如何設定 Python 環境](https://code.visualstudio.com/docs/python/environments)。

    ![選取 VS Code 中的 Python 解譯器](../images/interpreterselection.gif)

8. 將下列程式碼貼至您的 list-directory-contents.py 檔案中，然後選取 [儲存]  ：

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. 開啟 VS Code 整合式終端機 (**Ctrl+`** ，使用倒單引號字元)，然後輸入您剛才儲存 Python 指令碼的 src 目錄：

    ```powershell
    cd src
    ```

10. 使用下列命令，在 PowerShell 中執行指令碼：

    ```powershell
    python3 .\list-directory-contents.py
    ```

    您應該會看到如下所示的輸出：

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. 直接在 PowerShell 終端機中輸入下列命令，以使用 Python 將該檔案系統目錄輸出列印至它自己的文字檔：`python3 list-directory-contents.py > food-directory.txt`

恭喜！ 您剛撰寫了自動化系統管理指令碼，它會讀取您所建立的目錄和檔案，並使用 Python 來顯示它們，然後將目錄結構列印至它自己的文字檔。

## <a name="example-script-to-modify-all-files-in-a-directory"></a>修改目錄中所有檔案的範例指令碼

此範例會使用您剛才建立的檔案和目錄，藉由將檔案的上次修改日期新增至檔案名稱開頭，來重新命名每個檔案。

1. 在 **python-scripts** 目錄的 **src** 資料夾內，為您的指令碼建立新的 Python 檔案：

    ```powershell
    new-item update-filenames.py
    ```

2. 開啟 update-filenames.py 檔案、將下列程式碼貼至檔案中，然後加以儲存：

    > [!NOTE]
    > getmtime 會以刻度傳回時間戳記，這不容易讀取。 必須先將它轉換成標準日期時間字串。

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. 執行下列命令來測試您的 update-filenames.py 指令碼：`python3 update-filenames.py`，然後再次執行 list-directory-contents.py 指令碼：`python3 list-directory-contents.py`

4. 您應該會看到如下所示的輸出：

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. 直接在 PowerShell 終端機中輸入下列命令，以使用 Python 將新的檔案系統目錄名稱列印至它自己的文字檔，而此目錄名稱前面會加上次修改時間戳記：`python3 list-directory-contents.py > food-directory-last-modified.txt`

希望您了解使用 Python 指令碼將基本系統管理工作自動化的一些趣事。 當然，還有很多其他東西需要知道，但我們希望您稳步開始。 我們已在下面分享幾個其他資源以繼續學習。

## <a name="additional-resources"></a>其他資源

- [Python 文件：檔案和目錄存取](https://docs.python.org/3.7/library/filesys.html)：此 Python 文件描述如何使用檔案系統和模組，來讀取檔案屬性、以可攜方式操作路徑，以及建立暫存檔案。
- [了解 Python：String_Formatting 教學課程](https://www.learnpython.org/en/String_Formatting)：有關使用 "%" 運算子進行字串格式化的詳細資訊。
- [10 種您應該知道的 Python 檔案系統方法](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2)：有關使用 `os` 和 `shutil` 操作檔案和資料夾的中級文章。
- [Python 的 Hitchhikers 指南：系統管理](https://docs.python-guide.org/scenarios/admin/)：一種「武斷指南」，提供 Python 相關主題的概觀和最佳做法。 本節涵蓋系統管理工具和架構。 本指南裝載於 GitHub，讓您可以提出問題並投稿。
