---
title: 適用于初學者的 Windows 10 上的 Python
description: 如果您初次在 Windows 上使用 Python，本指南可協助開始。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, 學習 python, windows 上適用於初學者的 python, 利用 microsoft store 安裝 python, python 搭配 vs code, windows 上的 pygame
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 9978daa746f1ee4c0dd11739af836177fb03cd7a
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254578"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>開始在 Windows 上使用適用於初學者的 Python

下列是逐步指南，適用於有興趣使用 Windows 10 學習 Python 的初學者。

## <a name="set-up-your-development-environment"></a>設定開發環境

對於初次使用 Python 的初學者，我們建議您[從 Microsoft Store安裝 Python](https://www.microsoft.com/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab)。 透過 Microsoft Store 進行安裝會使用基本 Python3 解譯器，但會處理目前使用者的 PATH 設定 (避免需要系統管理員存取權)，而且還會提供自動更新。 如果您是在教育環境中，或是組織中限制您電腦上權限或系統管理存取權的部門，則這項功能特別有用。

如果您是在 Windows 上使用 Python 進行 **Web 開發**，建議您針對開發環境採用不同的設定。 建議您透過 Windows 子系統 Linux 版安裝和使用 Python，而不是直接安裝在 Windows 上。 如需說明，請參閱：[開始在 Windows 上使用 Python 進行 Web 開發](./web-frameworks.md)。 如果您有興趣將作業系統上的一般工作自動化，請參閱我們的指南：[開始在 Windows 上使用 Python 進行指令碼處理和自動化](./scripting.md)。 對於某些進階案例 (例如，需要存取/修改 Python 的安裝檔案、製作二進位檔的複本，或直接使用 Python DL)，您可能會想要考慮直接從 [python.org](https://www.python.org/downloads/) 下載特定的 Python 版本，或考慮安裝[替代項目](https://www.python.org/download/alternatives)，例如 Anaconda、Jython、PyPy、WinPython、IronPython 等。只有當您是資深 Python 程式設計人員，且有選擇替代實作的特定原因時，才建議這樣做。

## <a name="install-python"></a>安裝 Python

若要使用 Microsoft Store 安裝 Python：

1. 移至 [開始]  功能表 (左下方的 Windows 圖示)，鍵入 "Microsoft Store"，選取連結以開啟市集。

2. 市集開啟之後，從右上方功能表中選取 [搜尋]  ，然後輸入 "Python"。 從 [應用程式] 底下的結果開啟 "Python 3.9"。 選取 [取得]  。

3. 當 Python 下載結束並完成安裝程序之後，請使用 [開始]  功能表 (左下方的 Windows 圖示) 開啟 Windows PowerShell。 開啟 PowerShell 之後，請輸入 `Python --version` 以確認 Python3 已安裝在您的電腦上。

4. Python 的 Microsoft Store 安裝包括 **pip** (標準套件管理員)。 Pip 可讓您安裝和管理不屬於 Python 標準程式庫的其他套件。 若要確認您也有可用來安裝和管理套件的 pip，請輸入 `pip --version`。

## <a name="install-visual-studio-code"></a>安裝 Visual Studio Code

藉由使用 VS Code 做為文字編輯器/整合式開發環境 (IDE)，您可以利用 [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (協助完成程式碼)、[Linting](https://code.visualstudio.com/docs/python/linting) (協助避免在程式碼中發生錯誤)、[偵錯支援](https://code.visualstudio.com/docs/python/debugging) (在執行程式碼之後，協助您在其中找出錯誤)、[程式碼片段](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (小型可重覆使用程式碼區塊的範本)，以及[單元測試](https://code.visualstudio.com/docs/python/unit-testing) (以不同類型的輸入來測試程式碼的介面)。

VS Code 也包含[內建終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)，可讓您使用 Windows 命令提示字元、PowerShell 或您偏好的任何方式開啟 Python 命令列，以在程式碼編輯器與命令列之間建立順暢的工作流程。

1. 若要安裝 VS Code，請下載 VS Code for Windows：[https://code.visualstudio.com](https://code.visualstudio.com)。

2. 安裝 VS Code 後，您也必須安裝 Python 擴充功能。 若要安裝 Python 擴充功能，您可以選取 [VS Code Marketplace 連結](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 或開啟 VS Code，然後在 [擴充功能] 功能表 (Ctrl+Shift+X) 中搜尋 **Python**。

3. Python 是一種解譯的語言，若要執行 Python 程式碼，您必須告訴 VS Code 使用哪個解譯器。 建議您堅持使用 Python 3.7，除非您有特定原因，非選擇不同版本不可。 安裝 Python 擴充功能後，請開啟 [命令選擇區]  (Ctrl+Shift+P) 以選取 Python 3 解譯器，然後開始鍵入命令 **Python:Select Interpreter** 進行搜尋，然後選取命令。 您也可以使用底部狀態列上的 [選取 Python 環境]  選項 (如果有的話)，它可能已顯示選取的解譯器。 此命令會呈現 VS Code 可以自動尋找的可用解譯器清單，包括虛擬環境。 如果您未看到所需的解譯器，請參閱[設定 Python 環境](https://code.visualstudio.com/docs/python/environments)。

    ![選取 VS Code 中的 Python 解譯器](../images/interpreterselection.gif)

4. 若要在 VS Code 中開啟終端機，請選取 [檢視]   >  [終端機]  ，或者使用快速鍵 **Ctrl+`** (使用倒單引號字元)。 預設終端機為 PowerShell。

5. 在 VS Code 終端機內，直接輸入下列命令來開啟 Python：`python`

6. 輸入下列命令來試用 Python 解譯器：`print("Hello World")`。 Python 會傳回您的陳述式 "Hello World"。

    ![VS Code 中的 Python 命令列](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>安裝 Git (選用)

如果您計畫在 Python 程式碼上與其他人合作，或在開放原始碼網站 (如 GitHub) 上裝載您的專案，VS Code 支援[使用 Git 進行版本控制](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support)。 VS Code 中的 [原始檔控制] 索引標籤會追蹤您所有的變更，並讓常用的 Git 命令 (add、commit、push、pull) 直接內建在 UI 中。 您首先必須安裝 Git，才能強化 [原始檔控制] 面板。

1. 從 [git-scm 網站](https://git-scm.com/download/win)下載並安裝適用於 Windows 的 Git。

2. 隨附的安裝精靈會詢問您有關 Git 安裝設定的一系列問題。 建議您使用所有預設設定，除非您有特定原因，非變更某些設定不可。

3. 如果您之前從未使用過 Git，[GitHub 指南](https://guides.github.com/)可以協助您開始使用。

## <a name="hello-world-tutorial-for-some-python-basics"></a>適用於某些 Python 基本概念的 Hello World 教學課程

根據建立者 Guido van Rossum 所說，「Python 是一種「高階程式設計語言」，其核心設計原理在於程式碼可讀性和語法，可讓程式設計人員以幾行程式碼表達概念。」

Python 是一種解譯的語言。 與編譯的語言相較之下，您撰寫的程式碼必須轉譯成機器碼，才能由電腦的處理器執行，而 Python 程式碼則直接傳遞至解譯器並直接執行。 您只要鍵入程式碼並加以執行即可。 讓我們來試試看！

1. 在開啟 PowerShell 命令列後，輸入 `python` 以執行 Python 3 解譯器。 (有些指示會偏好使用命令 `py` 或 `python3`，這些命令應該也可以執行)。 您會知道您已成功，因為有三個大於符號的 >>> 提示字元會顯示。

2. 有數個內建方法可讓您修改 Python 中的字串。 利用下列命令建立變數：`variable = 'Hello World!'`。 按 Enter 鍵以換行。

3. 利用下列命令列印您的變數：`print(variable)`。 這會顯示 "Hello World!" 文字。

4. 使用下列命令，找出字串變數的長度、使用的字元數：`len(variable)`。 這會顯示已使用 12 個字元。 (請注意，在總長度中會將空格算成一個字元。)

5. 將您的字串變數轉換成大寫字母：`variable.upper()`。 現在將您的字串變數轉換成小寫字母：`variable.lower()`。

6. 計算在字串變數中使用字母 "l" 的次數：`variable.count("l")`。

7. 在字串變數中搜尋特定字元，讓我們使用下列命令尋找驚嘆號：`variable.find("!")`。 這會顯示在字串的第 11 個位置字元中找到驚嘆號。

8. 將驚嘆號取代為問號：`variable.replace("!", "?")`。

9. 若要結束 Python，您可以輸入 `exit()`、`quit()`，或選取 Ctrl-Z。

![本教學課程的 PowerShell 螢幕擷取畫面](../images/hello-world-basics.png)

希望您在使用某些 Python 的內建字串修改方法時感到有趣。 現在，請嘗試使用 VS Code 建立 Python 程式檔案，並執行它。

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>使用 Python 搭配 VS Code 的 Hello World 教學課程

VS Code 團隊結合了絕佳的 [開始使用 Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) 教學課程，逐步解說如何使用 Python 建立 Hello World 程式、執行程式檔案、設定和執行偵錯工具，以及安裝 *matplotlib* 和 *numpy* 之類的套件，以在虛擬環境內建立圖形化繪圖。

1. 開啟 PowerShell 並建立名為 "hello" 的空資料夾、瀏覽至此資料夾，然後在 VS Code 中開啟它：

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. 開啟 VS Code 之後，會在左側 [總管]  視窗中顯示新的 **hello** 資料夾，請按 **Ctrl+`** (使用倒單引號字元) 或選取 [檢視]   >  [終端機]  ，在 VS Code 的底部面板中開啟命令列視窗。 藉由在資料夾中啟動 VS Code，該資料夾就會變成您的「工作區」。 VS Code 會將該工作區特定的設定儲存在..vscode/settings.json 中，這些設定與全域儲存的使用者設定不同。

3. 繼續進行 VS Code 文件中的教學課程：[建立 Python Hello World 原始程式碼檔案](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file)。

## <a name="create-a-simple-game-with-pygame"></a>使用 Pygame 建立簡單的遊戲

![執行範例遊戲的 Pygame](../images/pygame-shmup.jpg)

Pygame 是用於撰寫遊戲的熱門 Python 套件 - 鼓勵學生學習程式設計，同時建立一些有趣的東西。 Pygame 會在新視窗中顯示圖形，因此它不會在僅限命令列的 WSL 方法下使用。 不過，如果您已依本教學課程所述，透過 Microsoft Store 安裝 Python，則它會正常執行。

1. 安裝了 Python 之後，請輸入 `python -m pip install -U pygame --user`，從命令列 (或 VS Code 內的終端機)安裝 pygame。

2. 執行範例遊戲來測試安裝：`python -m pygame.examples.aliens`

3. 一切都很順利，遊戲將會開啟一個視窗。 當您玩完遊戲時，請關閉視窗。

以下說明如何開始撰寫您自己的遊戲。

1. 開啟 PowerShell (或 Windows 命令提示字元)，並建立名為 "bounce" 的空資料夾。 瀏覽至此資料夾，並建立名為 "bounce.py" 的檔案。 在 VS Code 中開啟資料夾：

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. 使用 VS Code 輸入下列 Python 程式碼 (或複製並貼上)：

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

3. 將其另存為：`bounce.py`。

4. 從 PowerShell 終端機，輸入下列命令來執行它：`python bounce.py`。

    ![接著執行 Pygame](../images/pygame.jpg)

嘗試調整一些數字，看看它們對彈跳球有何影響。

在 [pygame.org](http://www.pygame.org) 深入閱讀如何使用 pygame 撰寫遊戲。

## <a name="resources-for-continued-learning"></a>持續學習的資源

建議下列資源，以支援您繼續了解如何在 Windows 上進行 Python 開發。

### <a name="online-courses-for-learning-python"></a>學習 Python 的線上課程

- [Microsoft Learn 上的 Python 簡介](/learn/modules/intro-to-python/)：試用互動式 Microsoft Learn 平台，並獲得完成此模組的體驗點數，其中涵蓋如何撰寫基本 Python 程式碼、宣告變數，以及使用主控台輸入和輸出的基本概念。 互動式沙箱環境讓此平台成為尚未設定 Python 開發環境的人員開始的絕佳位置。

- [Pluralsight 上的 Python：8堂課程，29 小時](https://app.pluralsight.com/paths/skills/python)：Pluralsight 上的 Python 學習路徑提供線上課程，其中涵蓋各種與 Python 相關的主題，包括測量您技能及找出差距的工具。

- [LearnPython.org 教學課程](https://www.learnpython.org/)：開始學習 Python，而不需要安裝或設定任何項目，這些免費互動式 Python 教學課程都是 DataCamp 上人員提供的。

- [Python.org 教學課程](https://docs.python.org/3/tutorial/index.html)：非正式地向讀者介紹 Python 語言和系統的基本概念和功能。

- [在 Lynda.com 上學習 Python](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html)：Python 基本簡介。

### <a name="working-with-python-in-vs-code"></a>在 VS Code 中使用 Python

- [在 VS Code中編輯 Python](https://code.visualstudio.com/docs/python/editing)：深入了解如何利用 VS Code 的自動完成和 Python 的 IntelliSense 支援，包括如何自訂其行為...，或只是將其關閉。

- [Linting Python](https://code.visualstudio.com/docs/python/linting)：Linting 是執行程式的過程，其會分析程式碼找出潛在錯誤。 了解 VS Code 針對 Python 提供的各種不同形式的 linting 支援，以及如何進行設定。

- [偵錯 Python](https://code.visualstudio.com/docs/python/debugging)：偵錯工具是指從電腦程式識別及移除錯誤的過程。 本文涵蓋如何使用 VS Code 初始化和設定 Python 的偵錯、如何設定和驗證中斷點、附加本機指令碼、針對不同的應用程式類型或在遠端電腦上執行偵錯，以及一些基本的疑難排解。

- [對 Python 進行單元測試](https://code.visualstudio.com/docs/python/unit-testing)：涵蓋部分背景，說明單元測試的意義、範例逐步解說、啟用測試架構、建立和執行測試、偵錯測試，以及測試組態設定。
