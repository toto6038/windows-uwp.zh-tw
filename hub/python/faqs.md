---
title: 在 Windows 上使用 Python 的常見問題
description: 在 Windows 上使用 Python 的常見問題
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, pip, py.exe, 檔案路徑, PYTHONPATH, python 部署, python 封裝
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 6dbf86e0f9435e44140159ebb2bcbc3d67928999
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/29/2020
ms.locfileid: "74663555"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>在 Windows 上使用 Python 的常見問題

## <a name="why-cant-i-pip-install-a-certain-package"></a>為什麼我無法「pip 安裝」某個套件？

安裝失敗的原因有很多，在大部分情況下，正確的解決方案就是與套件開發人員連絡。

問題最常見的原因是嘗試安裝至您沒有權限修改的位置。 例如，預設安裝位置可能需要系統管理權限，但根據預設，Python 沒有該權限。 最佳的解決方案是建立虛擬環境並在該處安裝。

某些套件包括需要 C 或C++編譯器才能安裝的機器碼。 一般來說，套件開發人員應該發佈預先編譯的版本，但通常不會。 如果您[安裝 Build Tools for Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)，並選取 C++ 選項，則其中部分套件可能會運作，但是在大部分情況下，您需要連絡套件開發人員。

[跟隨 Stack Overflow 上的討論](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379)。

## <a name="what-is-pyexe"></a>什麼是 py.exe？

您最終可能會在電腦上安裝多個版本的 Python，因為您正在處理不同類型的 Python 專案。 因為這些全都使用 `python` 命令，所以您使用的 Python 版本可能不明顯。 做為標準，建議使用 `python3` 命令 (或 `python3.7` 來選取特定版本)。

[py.exe 啟動器](https://docs.python.org/3/using/windows.html#launcher)會自動選取您已安裝的最新 Python 版本。 您也可以使用 `py -3.7` 之類的命令來選取特定版本，或使用 `py --list` 來查看可以使用的版本。 **不過**，只有在您使用從 [python.org](https://www.python.org/downloads/windows/) 安裝的 Python 版本時，.py 啟動器才會運作。從 Microsoft Store 安裝 Python 時，**不會**包含 `py` 命令。 若為 Linux、macOS、WSL 和 Microsoft Store 版本的 Python，您應該使用 `python3` (或 `python3.7`) 命令。

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>為什麼執行 python .exe 會開啟 Microsoft Store？

為了協助新使用者尋找妥善安裝 Python 的方式，我們已新增捷徑至 Windows，將您直接帶往 Microsoft Store 中發佈的最新社群套件版本。 此套件可以輕鬆地安裝，而不需要系統管理員權限，而且會將預設 `python` 和 `python3` 命令取代為真正的命令。

執行搭配任何命令列引數的捷徑可執行檔將會傳回錯誤碼，指出尚未安裝 Python。 這是為了防止批次檔和指令碼在可能非預期的情況下開啟 Store 應用程式。

如果您使用 [python.org](https://www.python.org/downloads/windows/) 中的安裝程式來安裝 Python，並選取 [新增至路徑] 選項，新的 `python` 命令會優先於捷徑。 請注意，其他安裝程式可能會以_低於_內建捷徑的優先順序來新增 `python`。

您可以停用捷徑而不安裝 Python，方法是從 [開始] 開啟 [管理應用程式執行別名]、尋找 [應用程式安裝程式] Python 項目，並將它們切換為 [關閉]。

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>當我複製並貼上檔案路徑時，為何不能在 Python 中使用它們？

Python 字串會針對特殊字元使用「逸出」。 例如，若要在字串中插入換行字元，請鍵入 `\n`。 由於 Windows 上的檔案路徑使用反斜線，因此某些部分可能會轉換為特殊字元。

若要將路徑貼入為 Python 中的字串，請新增 `r` 前置詞。 這表示它是 `raw` 字串，而且除了 \” 以外，不會使用任何逸出字元 (您可能需要移除路徑中的最後一個反斜線)。 因此，路徑可能如下：`r"C:\Users\MyName\Documents\Document.txt"`

在 Python 中使用路徑時，建議使用標準 pathlib 模組。 這可讓您將字串轉換為 Rich 格式的 Path 物件，以一致方式執行路徑操作，不論其是否使用正斜線或反斜線，都可讓您的程式碼在不同的作業系統上更好地運作。

## <a name="what-is-pythonpath"></a>什麼是 PYTHONPATH？

Python 會使用 PYTHONPATH 環境變數來指定可從中匯入模組的目錄清單。 執行時，您可以檢查 `sys.path` 變數，以查看當匯入某個模組時，將會搜尋哪些目錄。

若要從命令提示字元設定此變數，請使用：`set PYTHONPATH=list;of;paths`。

若要從 PowerShell 設定此變數，請在啟動 Python 之前，才使用 `$env:PYTHONPATH=’list;of;paths’`。

**不**建議透過**環境變數**設定全域設定此變數，因為任何版本的 Python 都可使用它，而不是您想要使用的版本。

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>哪裡可以找到封裝和部署方面的協助？

[Docker](https://code.visualstudio.com/docs/azure/docker)：[VSCode 延伸模組](https://code.visualstudio.com/docs/azure/docker)可協助您快速封裝和部署 Dockerfile 和 docker-compose.dev.debug.yml 範本 (為您的專案產生適當的 Docker 檔案)。

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/) 可讓您部署和管理容器化應用程式，同時依需求調整資源。

## <a name="what-if-i-need-to-work-across-different-machines"></a>如果我需要在不同的電腦上工作，該怎麼辦？

[設定同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)可讓您使用 GitHub 同步處理不同安裝之間的 VS Code 設定。 如果您在不同的電腦上工作，這有助於讓您的環境在其上保持一致。

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>如果我習慣使用 PyCharm、Atom、Sublime Text、Emacs 或 Vim，該怎麼辦？

VSCode 延伸模組 [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) 可以協助您的環境在家一樣沒有問題。

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac 快速鍵如何對應至 Windows 快速鍵？

在 Windows 電腦與 Macintosh 之間有些鍵盤按鈕和系統快速鍵稍有不同。 此 [Mac 到 Windows 轉換指南](../dev-environment/mac-to-windows.md)涵蓋基本概念。
