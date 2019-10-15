---
title: 開始在 Windows 上使用適用于初學者的 Python
description: 協助您開始使用 Windows 上的 Python 的全新指南。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python，windows 10，microsoft，學習 python，windows 上適用于初學者的 python，使用 microsoft store 安裝 python，使用 vs code 的 python，在 windows 上 pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d4c1cb6d65eb38a93e8bf9f0c34afd9e28f20129
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314922"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>開始在 Windows 上使用適用于初學者的 Python

以下是對使用 Windows 10 學習 Python 感興趣的新手的逐步指南。

## <a name="set-up-your-development-environment"></a>設定開發環境

對於 Python 新手的新手，建議您[從 Microsoft Store 安裝 python](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 透過 Microsoft Store 安裝會使用基本 Python3 解譯器，但會處理為目前使用者設定的路徑設定（不需要系統管理員存取權），除了提供自動更新之外。 如果您是在教育環境中，或是組織中限制了電腦上的許可權或系統管理存取權，這項功能特別有用。

如果您在 Windows 上使用 Python 進行**網頁程式開發**，建議您針對開發環境採用不同的設定。 我們建議您透過適用于 Linux 的 Windows 子系統來安裝和使用 Python，而不是直接在 Windows 上安裝。 如需協助，請參閱：[開始在 Windows 上使用 Python 進行網頁程式開發](./web-frameworks.md)。 如果您想要將作業系統上的一般工作自動化，請參閱我們的指南：[開始在 Windows 上使用 Python 進行腳本處理和自動化](./scripting.md)。 針對某些先進的案例（例如需要存取/修改 Python 的已安裝檔案、複製二進位檔，或直接使用 Python Dll），您可能會想要考慮直接從[python.org](https://www.python.org/downloads/)下載特定的 Python 版本，或考慮安裝[另一個替代方法](https://www.python.org/download/alternatives)，例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。只有當您是更先進的 Python 程式設計人員，並具有選擇替代執行方式的特定原因時，才建議使用此方法。

## <a name="install-python"></a>安裝 Python

若要使用 Microsoft Store 安裝 Python：

1. 移至您的 [**開始**] 功能表（左下方的 Windows 圖示），輸入 "Microsoft Store"，然後選取連結以開啟存放區。

2. 在商店開啟後，從右上方的功能表中選取 [**搜尋**]，然後輸入 "Python"。 從 [應用程式] 底下的結果開啟 "Python 3.7"。 選取 [**取得**]。

3. Python 完成下載和安裝程式後，請使用 [**開始**] 功能表（左下角 windows 圖示）來開啟 Windows PowerShell。 PowerShell 開啟後，請輸入 `Python --version`，以確認 Python3 已安裝在您的電腦上。

4. Python 的 Microsoft Store 安裝包括**pip**，也就是標準套件管理員。 Pip 可讓您安裝和管理不屬於 Python 標準程式庫的其他套件。 若要確認您也有可用來安裝和管理封裝的 pip，請輸入 `pip --version`。

## <a name="install-visual-studio-code"></a>安裝 Visual Studio Code

藉由使用 VS Code 做為文字編輯器/整合式開發環境（IDE），您可以利用[IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) （程式碼完成協助工具）、 [Linting](https://code.visualstudio.com/docs/python/linting) （協助避免在程式碼中發生錯誤）、 [Debug 支援](https://code.visualstudio.com/docs/python/debugging)（協助您尋找中的錯誤執行後的程式碼）、[程式碼片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets)（適用于小型可重複使用程式碼區塊的範本）和[單元測試](https://code.visualstudio.com/docs/python/unit-testing)（使用不同類型的輸入來測試程式碼介面）。

VS Code 也包含[內建的終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機，可讓您使用 Windows 命令提示字元、PowerShell 或您偏好的任何方式開啟 Python 命令列，並在程式碼編輯器和命令列之間建立順暢的工作流程。

1. 若要安裝 VS Code，請下載適用于 Windows 的 VS Code： [https://code.visualstudio.com](https://code.visualstudio.com)。

2. Python 是一種轉譯的語言，若要執行 Python 程式碼，您必須告訴 VS Code 要使用的解譯器。 我們建議您堅持使用 Python 3.7，除非您有特定原因要選擇不同的專案。 開啟**命令**選擇區（Ctrl + Shift + P）以選取 Python 3 解譯器，開始輸入命令 **Python：選取 [解譯器 @ no__t-0] 進行搜尋，然後選取命令。 您也可以使用底部狀態列上的 [**選取 Python 環境**] 選項（如果有的話）（可能已經顯示選取的解譯器）。 命令會顯示 VS Code 可以自動尋找的可用解譯器清單，包括虛擬環境。 如果您沒有看到所需的解譯器，請參閱設定[Python 環境](https://code.visualstudio.com/docs/python/environments)。

    ![選取 VS Code 中的 Python 解譯器](../images/interpreterselection.gif)

3. 若要在 VS Code 中開啟終端機，請選取 [ **View** > **terminal**]，或使用快速鍵**Ctrl + '** （使用倒引號字元）。 預設終端機是 PowerShell。

4. 在您的 VS Code 終端機中，直接輸入命令來開啟 Python： `python`

5. 輸入： `print("Hello World")` 來嘗試 Python 解譯器。 Python 會傳回您的語句「Hello World」。

    ![VS Code 中的 Python 命令列](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>安裝 Git （選擇性）

如果您計畫在 Python 程式碼上與其他人共同作業，或在開放原始碼網站（如 GitHub）上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並在 UI 中建立常用的 Git 命令（add、commit、push、pull）。 您必須先安裝 Git，才能開啟 [原始檔控制] 面板。

1. 從[git-scm 網站](https://git-scm.com/download/win)下載並安裝 Git for Windows。

2. 其中包含安裝精靈，會詢問您有關 Git 安裝設定的一系列問題。 我們建議使用所有預設設定，除非您有特定原因要變更某個專案。

3. 如果您從未使用過 Git， [GitHub 指南](https://guides.github.com/)可協助您開始著手。

## <a name="hello-world-tutorial-for-some-python-basics"></a>適用于某些 Python 基本概念的 Hello World 教學課程

根據它的建立者 Guido van Rossum，Python 是一種「高階程式設計語言」，其核心設計原理在於程式碼可讀性和語法，可讓程式設計人員以幾行程式碼表達概念。」

Python 是一種已轉譯的語言。 與編譯的語言相較之下，您撰寫的程式碼必須轉譯成機器碼，才能由電腦的處理器執行，而 Python 程式碼則直接傳遞至解譯器並直接執行。 您只要輸入您的程式碼並加以執行即可。 讓我們試試看吧！

1. 開啟 PowerShell 命令列後，輸入 `python`，以執行 Python 3 解譯器。 （有些指示會偏好使用命令 `py` 或 `python3`，這些都應該也能正常執行）。 您會知道您已成功，因為有三個大於符號的 > > > 提示字元會顯示。

2. 有數個內建的方法可讓您修改 Python 中的字串。 建立變數，並使用： `variable = 'Hello World!'`。 針對新行按 Enter 鍵。

3. 使用下列程式來列印您的變數： `print(variable)`。 這會顯示 "Hello World！" 文字。

4. 找出字串變數的長度、使用的字元數，以及： `len(variable)`。 這會顯示有12個字元使用。 （請注意，它會在總長度中計算為字元的空格）。

5. 將您的字串變數轉換成大寫字母： `variable.upper()`。 現在將您的字串變數轉換成小寫字母： `variable.lower()`。

6. 計算在字串變數中使用字母 "l" 的次數： `variable.count("l")`。

7. 在您的字串變數中搜尋特定字元，讓我們找出驚嘆號，其中包含： `variable.find("!")`。 這會顯示在字串的第11個位置字元中找到驚嘆號。

8. 以問號： `variable.replace("!", "?")` 取代驚嘆號。

9. 若要結束 Python，您可以輸入 `exit()`，`quit()`，或選取 Ctrl-Z。

![本教學課程的 PowerShell 螢幕擷取畫面](../images/hello-world-basics.png)

希望您在使用某些 Python 的內建字串修改方法時有樂趣。 現在，請嘗試建立 Python 程式檔案，並使用 VS Code 執行它。

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>使用 Python 搭配 VS Code 的 Hello World 教學課程

VS Code 小組結合了 Python 的絕佳[消費者入門](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder)教學課程逐步解說如何使用 python 建立 Hello World 程式、執行程式檔、設定和執行偵錯工具，以及安裝*matplotlib*和*這類套件numpy*在虛擬環境內建立圖形化繪圖。

1. 開啟 PowerShell 並建立名為 "hello" 的空資料夾，流覽至此資料夾，然後在 VS Code 中開啟它：

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. 一旦 VS Code 開啟，在左側的**Explorer**視窗中顯示新的*hello*資料夾，按**Ctrl + '** （使用倒引號字元）或選取**View** >  **，即可在 VS Code 底部面板中開啟命令列視窗。終端**機。 藉由在資料夾中啟動 VS Code，該資料夾就會變成您的「工作區」。 VS Code 會將該工作區特定的設定儲存在. vscode/settings. json 中，這與全域儲存的使用者設定不同。

3. 繼續進行 VS Code 檔中的教學課程：[建立 Python Hello World 原始碼](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file)檔案。

## <a name="create-a-simple-game-with-pygame"></a>使用 Pygame 建立簡單的遊戲

![執行範例遊戲的 Pygame](../images/pygame-shmup.jpg)

Pygame 是熱門的 Python 套件，可讓您撰寫遊戲，鼓勵學生學習程式設計，同時建立一些有趣的東西。 Pygame 會在新視窗中顯示圖形，因此它不會在僅限命令列的 WSL 方法下使用。 不過，如果您已依照本教學課程中的詳細說明，透過 Microsoft Store 安裝 Python，則會正常執行。

1. 安裝 Python 之後，請輸入 `python -m pip install -U pygame --user`，從命令列（或 VS Code 中的終端機）安裝 pygame。

2. 執行範例遊戲來測試安裝： `python -m pygame.examples.aliens`

3. 這一切都很順利，遊戲就會開啟一個視窗。 當您完成播放時，請關閉視窗。

以下說明如何開始撰寫您自己的遊戲。

1. 開啟 PowerShell （或 Windows 命令提示字元），並建立名為 "彈跳" 的空資料夾。 流覽至此資料夾，並建立名為 "bounce.py" 的檔案。 在 VS Code 中開啟資料夾：

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. 使用 VS Code 輸入下列 Python 程式碼（或複製並貼上）：

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. 將它儲存為： `bounce.py`。

4. 從 PowerShell 終端機，輸入： `python bounce.py` 來執行。

    ![Pygame 執行下一大事](../images/pygame.jpg)

請嘗試調整一些數位，以查看它們在您跳動球上的效果。

若要深入瞭解如何使用 pygame 撰寫遊戲，請參閱[pygame.org](http://www.pygame.org)。

## <a name="resources-for-continued-learning"></a>持續學習的資源

我們建議下列資源，以支援您繼續瞭解 Windows 上的 Python 開發。

### <a name="online-courses-for-learning-python"></a>學習 Python 的線上課程

- [Microsoft Learn 上的 Python 簡介](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/)：試用互動式 Microsoft Learn 平臺，並獲得完成此課程模組的經驗點，其中涵蓋如何撰寫基本 Python 程式碼、宣告變數，以及使用主控台輸入和輸出的基本概念。 互動式沙箱環境讓這成為開始使用尚未設定 Python 開發環境之人員的絕佳位置。

- 在 Pluralsight 上 @no__t 0Python：8堂課程，29小時 @ no__t-0：Pluralsight 上的 Python 學習路徑提供線上課程，涵蓋各種與 Python 相關的主題，包括測量技能及找出差距的工具。

- [LearnPython.org 教學](https://www.learnpython.org/)課程：開始學習 Python，而不需要安裝或設定任何專案，這些都是來自 DataCamp 的人員提供的免費互動式 Python 教學課程。

- [Python.org 教學](https://docs.python.org/3/tutorial/index.html)課程：介紹 Python 語言和系統的基本概念和功能，非正式的讀者。

- [瞭解 Lynda.com 上的 Python](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html)：Python 的基本簡介。

### <a name="working-with-python-in-vs-code"></a>在 VS Code 中使用 Python

- [編輯 VS Code 中的 Python](https://code.visualstudio.com/docs/python/editing)：深入瞭解如何利用適用于 Python 的 VS Code 自動完成和 IntelliSense 支援，包括如何自訂其 behvior .。。或只是關閉它們。

- [Linting Python](https://code.visualstudio.com/docs/python/linting)：Linting 是執行程式的程式，會針對可能發生的錯誤分析程式碼。 瞭解 VS Code 針對 Python 提供的各種不同形式的 linting 支援，以及如何進行設定。

- [調試 Python](https://code.visualstudio.com/docs/python/debugging)：「偵錯工具」是指從電腦程式識別及移除錯誤的過程。 本文說明如何使用 VS Code 初始化和設定 Python 的偵錯工具、如何設定和驗證中斷點、附加本機腳本、針對不同的應用程式類型或遠端電腦執行檢查，以及一些基本的疑難排解。

- [單元測試 Python](https://code.visualstudio.com/docs/python/unit-testing)：涵蓋部分背景說明單元測試的意義、範例逐步解說、啟用測試架構、建立和執行測試、偵錯工具和測試設定。 
