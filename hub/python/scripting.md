---
title: 在 Windows 上使用 Python 進行腳本處理和自動化
description: 如何開始在 Windows 上使用 Python 進行腳本處理、自動化和系統管理。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python，windows 10，microsoft，python 系統管理，python 檔案自動化，windows 上的 python 腳本，在 windows 上設定 python，windows 上的 python 開發人員環境，windows 上的 python 開發環境，使用 powershell 的 python 腳本檔案系統工作
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 3f8a17de8121fed27e69442d5560f702a04c8e42
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314862"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>開始在 Windows 上使用 Python 進行腳本處理和自動化

以下是設定開發人員環境的逐步指南，讓您開始使用 Python 進行腳本處理，並將 Windows 上的檔案系統作業自動化。

> [!NOTE]
> 本文將說明如何設定您的環境，以使用 Python 中一些有用的程式庫，可跨平臺自動執行工作，例如搜尋您的檔案系統、存取網際網路、剖析檔案類型等，或從以 Windows 為中心的方法進行。 針對 Windows 特有的作業，請參閱[ctypes](https://docs.python.org/3/library/ctypes.html)，這是適用于 Python 的 C 相容外部函式程式庫， [Winreg](https://docs.python.org/3/library/winreg.html)，將 Windows 登錄 API 公開至 python 的函式，以及[python/WinRT](https://pypi.org/project/winrt/)，可讓您從存取 Windows 執行階段 apiPython.

## <a name="set-up-your-development-environment"></a>設定開發環境

使用 Python 撰寫執行檔案系統作業的腳本時，建議您[從 Microsoft Store 安裝 Python](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 透過 Microsoft Store 安裝會使用基本 Python3 解譯器，但會處理為目前使用者設定的路徑設定（不需要系統管理員存取權），除了提供自動更新之外。

如果您在 Windows 上使用 Python 進行**網頁程式開發**，建議您使用適用于 Linux 的 Windows 子系統來進行不同的安裝。 在我們的指南中尋找逐步解說：[開始在 Windows 上使用 Python 進行網頁程式開發](./web-frameworks.md)。 如果您是 Python 的新手，請嘗試我們的指南：[開始在 Windows 上使用適用于初學者的 Python](./beginners.md)。 針對某些先進的案例（例如需要存取/修改 Python 的已安裝檔案、複製二進位檔，或直接使用 Python Dll），您可能會想要考慮直接從[python.org](https://www.python.org/downloads/)下載特定的 Python 版本，或考慮安裝[另一個替代方法](https://www.python.org/download/alternatives)，例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。只有當您是更先進的 Python 程式設計人員，並具有選擇替代執行方式的特定原因時，才建議使用此方法。

## <a name="install-python"></a>安裝 Python

若要使用 Microsoft Store 安裝 Python：

1. 移至您的 [**開始**] 功能表（左下方的 Windows 圖示），輸入 "Microsoft Store"，然後選取連結以開啟存放區。

2. 在商店開啟後，從右上方的功能表中選取 [**搜尋**]，然後輸入 "Python"。 從 [應用程式] 底下的結果開啟 "Python 3.7"。 選取 [**取得**]。

3. Python 完成下載和安裝程式後，請使用 [**開始**] 功能表（左下角 windows 圖示）來開啟 Windows PowerShell。 PowerShell 開啟後，請輸入 `Python --version`，以確認 Python3 已安裝在您的電腦上。

4. Python 的 Microsoft Store 安裝包括**pip**，也就是標準套件管理員。 Pip 可讓您安裝和管理不屬於 Python 標準程式庫的其他套件。 若要確認您也有可用來安裝和管理封裝的 pip，請輸入 `pip --version`。

## <a name="install-visual-studio-code"></a>安裝 Visual Studio Code

藉由使用 VS Code 做為文字編輯器/整合式開發環境（IDE），您可以利用[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) （程式碼完成協助工具）、 [Linting](https://code.visualstudio.com/docs/python/linting) （協助避免在程式碼中發生錯誤）、 [Debug 支援](https://code.visualstudio.com/docs/python/debugging)（協助您尋找中的錯誤執行後的程式碼）、[程式碼片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)（適用于小型可重複使用程式碼區塊的範本）和[單元測試](https://code.visualstudio.com/docs/python/unit-testing)（使用不同類型的輸入來測試程式碼介面）。

下載適用于 Windows 的 VS Code，並遵循安裝指示： [https://code.visualstudio.com](https://code.visualstudio.com)。

## <a name="install-the-microsoft-python-extension"></a>安裝 Microsoft Python 擴充功能

您必須安裝 Microsoft Python 延伸模組，才能利用 VS Code 支援功能。 [進一步瞭解](https://code.visualstudio.com/docs/languages/python)。

1. 輸入**Ctrl + Shift + X**以開啟 [VS Code 延伸模組] 視窗（或使用功能表流覽以**查看** > **延伸**模組）。

2. 在 [Marketplace 的頂端**搜尋擴充**功能] 方塊中，輸入：**Python**。

3. 尋找 Microsoft 擴充功能的**Python （ms python python）** ，然後選取綠色的 [**安裝**] 按鈕。

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>在 VS Code 中開啟整合式 PowerShell 終端機

VS Code 包含[內建的終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機，可讓您使用 PowerShell 開啟 Python 命令列，並在程式碼編輯器和命令列之間建立順暢的工作流程。

1. 在 VS Code 中開啟終端機，選取 [ **View** > **terminal**]，或使用快速鍵**Ctrl + '** （使用倒引號字元）。

    > [!NOTE]
    > 預設終端機應該是 PowerShell，但如果您需要變更它，請使用**Ctrl + Shift + P**輸入命令面板。 輸入 **Terminal：選取 [預設 Shell @ no__t-0]，將會顯示包含 PowerShell、命令提示字元、WSL 等的終端機選項清單。選取您想要使用的帳戶，然後輸入**Ctrl + Shift + '** （使用倒引號）來建立新的終端機。

2. 在您的 VS Code 終端機中，輸入下列內容來開啟 Python： `python`

3. 輸入： `print("Hello World")` 來嘗試 Python 解譯器。 Python 會傳回您的語句「Hello World」。

    ![VS Code 中的 Python 命令列](../images/python-in-vscode.png)

4. 若要結束 Python，您可以輸入 `exit()`，`quit()`，或選取 Ctrl-Z。

## <a name="install-git-optional"></a>安裝 Git （選擇性）

如果您計畫在 Python 程式碼上與其他人共同作業，或在開放原始碼網站（如 GitHub）上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並在 UI 中建立常用的 Git 命令（add、commit、push、pull）。 您必須先安裝 Git，才能開啟 [原始檔控制] 面板。

1. 從[git-scm 網站](https://git-scm.com/download/win)下載並安裝 Git for Windows。

2. 其中包含安裝精靈，會詢問您有關 Git 安裝設定的一系列問題。 我們建議使用所有預設設定，除非您有特定原因要變更某個專案。

3. 如果您從未使用過 Git， [GitHub 指南](https://guides.github.com/)可協助您開始著手。

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>顯示檔案系統目錄結構的範例腳本

常見的系統管理工作可能需要相當長的時間，但使用 Python 腳本時，您可以將這些工作自動化，讓它們完全不需要任何時間。 例如，Python 可以讀取電腦檔案系統的內容，並執行作業，例如列印檔案和目錄的大綱、將資料夾從一個目錄移到另一個，或重新命名上百個檔案。 一般來說，如果您要手動執行這些工作，這類工作可能會耗費大量時間。 請改用 Python 腳本！

讓我們從一個簡單的腳本開始，它會逐步解說目錄樹狀結構並顯示目錄結構。

1. 使用 [**開始**] 功能表開啟 PowerShell （左下方的 Windows 圖示）。

2. 建立專案的目錄： `mkdir python-scripts`，然後開啟該目錄： `cd python-scripts`。

3. 建立幾個目錄，以搭配我們的範例腳本使用：

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. 在這些目錄內建立一些檔案，以搭配我們的腳本使用：

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. 在您的 python 腳本目錄中建立新的 python 檔案：

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. 輸入下列內容，在 VS Code 中開啟您的專案： `code .`

7. 輸入**Ctrl + Shift + E** （或使用功能表流覽至**View** > **Explorer**）以開啟 [VS Code 檔案瀏覽器] 視窗，然後選取您剛才建立的 list-directory-contents.py 檔案。 Microsoft Python 延伸模組將會自動載入 Python 解譯器。 您可以查看 VS Code 視窗底部已載入哪個解譯器。

    > [!NOTE]
    > Python 是一種轉譯的語言，這表示它會作為虛擬機器，模擬實體電腦。 有不同類型的 Python 解譯器可供您使用：Python 2、Python 3、Anaconda、PyPy 等。為了執行 Python 程式碼並取得 Python IntelliSense，您必須告訴 VS Code 要使用哪個解譯器。 除非您有特定原因要選擇不同的專案，否則建議您不要使用 VS Code 預設選擇的解譯器（在我們的案例中為 Python 3）。 若要變更 Python 解譯器，請選取 VS Code 視窗底部藍色列中目前顯示的解譯器，或開啟**命令**選擇區（Ctrl + Shift + P），然後輸入命令 **Python：選取 [解譯器 @ no__t-0]。 這會顯示您目前已安裝的 Python 解譯器清單。 [深入瞭解如何設定 Python 環境](https://code.visualstudio.com/docs/python/environments)。

    ![選取 VS Code 中的 Python 解譯器](../images/interpreterselection.gif)

8. 將下列程式碼貼到您的 list-directory-contents.py 檔案中，然後選取 [**儲存**]：

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

9. 開啟 [VS Code 整合式終端機] （**Ctrl + '** ，使用倒引號字元），然後輸入您剛才儲存 Python 腳本的 src 目錄：

    ```powershell
    cd src
    ```

10. 在 PowerShell 中使用來執行腳本：

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

11. 使用 Python，直接在您的 PowerShell 終端機中輸入下列命令，將該檔案系統目錄輸出列印至它自己的文字檔： `python3 list-directory-contents.py > food-directory.txt`

恭喜您！ 您剛撰寫了自動化系統管理腳本，它會讀取您所建立的目錄和檔案，並使用 Python 來顯示，然後將目錄結構列印到它自己的文字檔。

## <a name="example-script-to-modify-all-files-in-a-directory"></a>修改目錄中所有檔案的範例腳本

這個範例會使用您剛才建立的檔案和目錄，藉由將檔案的上次修改日期新增至檔案名開頭，來重新命名每個檔案。

1. 在您的**python 腳本**目錄中的**src**資料夾內，為您的腳本建立新的 python 檔案：

    ```powershell
    new-item update-filenames.py
    ```

2. 開啟 update-filenames.py 檔案，將下列程式碼貼到檔案中並加以儲存：

    > [!NOTE]
    > getmtime 會以刻度傳回時間戳記，這不容易閱讀。 必須先將它轉換成標準日期時間字串。

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

3. 執行下列程式碼來測試您的 update-filenames.py 腳本： `python3 update-filenames.py`，然後再次執行 list-directory-contents.py 腳本： `python3 list-directory-contents.py`

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

5. 使用 Python，直接在您的 PowerShell 終端機中輸入下列命令，以列印其本身文字檔前面修改過的新檔案系統目錄名稱： `python3 list-directory-contents.py > food-directory-last-modified.txt`

希望您瞭解使用 Python 腳本將基本系統管理工作自動化的一些有趣事項。 當然，還有很多其他東西要知道，但我們希望您從右邊開始著手。 我們已分享幾個額外的資源，以繼續進行下列課程。

## <a name="additional-resources"></a>其他資源

- @no__t 0Python 檔：檔案和目錄存取 @ no__t-0：有關使用檔案系統和使用模組來讀取檔案屬性、以可移植方式操作路徑，以及建立暫存檔案的 Python 檔。
- @no__t 0Learn Python：String_Formatting 教學課程 @ no__t-0：有關使用 "%" 運算子進行字串格式設定的詳細資訊。
- [10 您應該知道的 Python 檔案系統方法](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2)：有關使用 `os` 和 `shutil` 操作檔案和資料夾的中文。
- @no__t 0The Hitchhikers Guide to Python：系統管理 @ no__t-0：「固定指南」，提供 Python 相關主題的總覽和最佳作法。 本節涵蓋系統管理工具和架構。 本指南裝載于 GitHub，讓您可以提出問題並做出貢獻。
