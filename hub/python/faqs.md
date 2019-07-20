---
title: 在 Windows 上使用 Python 的常見問題
description: 在 Windows 上使用 Python 的常見問題
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: python, windows 10, microsoft, pip, .py, 檔案路徑, PYTHONPATH, python 部署, python 封裝
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 4f7f5c325dfd114093e1434259489459a8c78151
ms.sourcegitcommit: 161eac985af11faaff78797d86343d4fa7d6a05f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2019
ms.locfileid: "68366736"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>在 Windows 上使用 Python 的常見問題

## <a name="why-cant-i-pip-install-a-certain-package"></a>為什麼我無法「pip 安裝」某個封裝？

安裝失敗的原因有很多, 在大多數情況下, 正確的解決方案是與封裝開發人員聯繫。

問題最常見的原因是嘗試將安裝到您沒有許可權修改的位置。 例如, 預設安裝位置可能需要系統管理許可權, 但根據預設, Python 將不會有它們。 最佳的解決方法是建立虛擬環境並安裝。

某些套件包含機器碼, 需要 C 或C++編譯器才能安裝。 一般來說, 封裝開發人員應該發行預先編譯的版本, 但通常不會。 如果您[安裝 Visual Studio 的組建工具](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019)並選取C++選項, 其中有些封裝可能會正常執行, 不過, 在大部分情況下, 您需要洽詢封裝開發人員。

[請遵循 StackOverflow 上的討論](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379)。

## <a name="what-is-pyexe"></a>什麼是 .py？

您的電腦上可能會安裝多個版本的 Python, 因為您正在處理不同類型的 Python 專案。 因為這些全都使用命令`python` , 所以您可能不會察覺到您使用的是哪一個。 [.Py 啟動器](https://docs.python.org/3/using/windows.html#launcher)會自動選取您已安裝的最新 Python 版本。 您也可以使用命令 ( `py -3.7`例如) 來選取特定版本, `py --list`或查看可以使用的版本。 **不過**, 只有在您使用從[Python.org](https://www.python.org/downloads/windows/)安裝的 Python 版本時, .py 啟動器才會生效。當您從 Microsoft Store 安裝 Python 時, `py` **不會包含**命令。 針對 Linux、macOS、WSL 和 Microsoft Store 版本的 Python, 您應該使用`python3`命令。

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>當我複製並貼上檔案路徑時, 為何不能在 Python 中使用它們？

Python 字串會針對特殊字元使用「轉義」。 例如, 若要在字串中插入新的行字元, 請輸入`\n`。 由於 Windows 上的檔案路徑使用反斜線, 因此某些部分可能會轉換成特殊字元。

若要將路徑貼入 Python 中的字串, 請新增`r`前置詞。 這表示它是`raw`字串, 而且不會使用任何換用字元, 除了 \ "(您可能需要移除路徑中的最後一個反斜線)。 因此您的路徑可能如下所示: r "C:\Users\MyName\Documents\Document.txt"

使用 Python 中的路徑時, 建議使用標準 pathlib 模組。 這可讓您將字串轉換成豐富路徑物件, 以一致的方式執行路徑操作, 不論其是否使用正斜線或反斜線, 讓您的程式碼在不同的作業系統上都能更好地運作。

## <a name="what-is-pythonpath"></a>什麼是 PYTHONPATH？

Python 會使用 PYTHONPATH 環境變數來指定可從中匯入模組的目錄清單。 執行時, 您可以檢查`sys.path`變數, 以查看當您匯入某個專案時, 將會搜尋哪些目錄。

若要從命令提示字元設定此變數, 請`set PYTHONPATH=list;of;paths`使用:。

若要從 PowerShell 設定此變數, 請`$env:PYTHONPATH=’list;of;paths’`使用: 在啟動 Python 之前。

**不**建議您透過**環境變數**設定全域設定此變數, 因為它可供任何版本的 Python 使用, 而不是您想要使用的。

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>哪裡可以找到封裝和部署的協助？

[Docker](https://code.visualstudio.com/docs/azure/docker):[VSCode 擴充](https://code.visualstudio.com/docs/azure/docker)功能可協助您快速封裝和部署 Dockerfile 和 docker-compose.dev.debug.yml yml 範本 (為您的專案產生適當的 docker 檔案)。

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/)可讓您部署和管理容器化應用程式, 同時依需求調整資源。

## <a name="what-if-i-need-to-work-across-different-machines"></a>如果我需要在不同的電腦上工作, 該怎麼辦？

[設定同步](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)可讓您使用 GitHub 同步處理不同安裝中的 VS Code 設定。 如果您在不同的電腦上工作, 這有助於讓您的環境在其上保持一致。

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>如果我習慣使用 PyCharm、Atom、Sublime Text、Emacs 或 Vim, 該怎麼辦？

VSCode 擴充功能[Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads)可以協助您的環境在家。

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>Mac 快速鍵如何對應至 Windows 快捷方式的金鑰？

Windows 電腦與 Macintosh 之間有些鍵盤按鈕和系統快捷方式稍有不同。 Microsoft 支援服務的這個[鍵盤對應指南](https://support.microsoft.com/help/970299/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh)涵蓋了基本概念。
