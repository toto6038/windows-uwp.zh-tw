---
title: Unity：針對您的 UWP 專案進行版本控制
description: 瞭解如何使用通用 Windows 平臺 (UWP) ，搭配適用于 Xbox 的 Unity 遊戲使用版本控制。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 67c4ec927d83ecba1257eb73fec451e3281333b2
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254163"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity：針對您的 UWP 專案進行版本控制

尚未使用通用 Windows 平台 (UWP) 針對 Xbox 建置您的 Unity 遊戲嗎？  請先參閱[將 Unity 遊戲移到 Xbox 上的 UWP](development-lanes-unity.md)。

有幾個不同的理由會讓您想要將產生的 UWP 目錄新增到版本控制，其中一個是新增相依性 (例如 Xbox Live SDK)。  我們會在本教學課程中以此案例做為範例，希望它會有助於您解決專案的個別需求。

***免責聲明：我們將使用 Git 作為版本控制解決方案。 如果您有不同的概念，這些概念仍應轉譯。** _

若要重新整理您的記憶體，這是我們的遊戲的目錄 _*_ScrapyardPhoenix_*_，目前看起來像這樣：

![建置目的地資料夾](images/build-destination.png)

而我們的 UWP 目錄看起來像這樣：

![UWP VS 解決方案](images/uwp-vs-solution.png)

在這個目錄中，我們只在意一個資料夾， _*_ScrapyardPhoenix_*_ (在這裡插入您的遊戲名稱) 資料夾。  在我們的版本控制中，所有其他項目都可以忽略。

_*_不熟悉 .gitignore 檔案是什麼？ 請參閱 [.gitignore](https://git-scm.com/docs/gitignore)。_*_

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep... (this line will be modified and removed further down)
!/UWP/ScrapyardPhoenix/
```

我們會從 **UWP/ScrapyardPhoenix** 資料夾中選取幾個不同的檔案與資料夾，以新增到我們的版本控制。  我們先看看完整的詳細資料：

![UWP 建置目錄](images/uwp-build-directory.png)  

## <a name="folders"></a>資料夾  

| 資料夾名稱 | 設定 | 說明 |
|-------------|---------|-------------|
| `Assets` | **_包含_* _ | 包含 Microsoft Store 映射 |
| `Data` | _*_忽略_*_ | Unity 會將您的專案編譯成 (場景、著色器、腳本、Prefabs 等 )  |
| `Dependencies` | _*_包括_*_ | 這是我建立的資料夾，用來保留所有 UWP 相依性 (例如 XboxLiveSDK.dll)  |
| `Properties` | _*_包括_*_ | 包含其他可由開發人員修改的 advanced 設定 |
| `Unprocessed` | _*_忽略_*_ | 包含 Unity `.dll` 和 `.pdb` 檔案 |

## <a name="files"></a>檔案儲存體  

| 資料夾名稱 | 設定 | 說明 |
|-------------|---------|-------------|
| `App.cs` | _*_包含_*_ | UWP 應用程式的進入點;您可以使用其他原始檔來修改和延伸此檔案 |
| `Package.appxmanifest` | _*_包括_*_ | Msix 或 .appx 封裝的應用程式套件資訊清單原始程式檔 |
| `project.json` | _*_包括_*_ | 描述您相依的 NuGet 套件 `_.csproj` |
| `ScrapyardPhoenix.csproj` | ***包含** _ | 描述您的 UWP 組建目標;如果您將其他相依性新增至 UWP 專案，此檔案 `_.csproj` 將包含該資訊 |
| `ScrapyardPhoenix.csproj.user` | ***忽略** _ | 此檔案包含本機使用者資訊 |

## <a name="resulting-gitignore"></a>產生的 .gitignore

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep...
!/UWP/ScrapyardPhoenix/Assets/*
!/UWP/ScrapyardPhoenix/Dependencies/*
!/UWP/ScrapyardPhoenix/Properties/*
!/UWP/ScrapyardPhoenix/App.cs
!/UWP/ScrapyardPhoenix/Package.appxmanifest
!/UWP/ScrapyardPhoenix/project.json
!/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj
```

這樣就好了，現在您的小組成員將會與您產生的 UWP 專案同步。 現在您可以放心地將其他資產、來源和相依性新增到您的 UWP 專案。

如需更多針對 UWP 資料夾進行版本管理的範例，可在[這些範例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)中找到。

## <a name="adding-dependencies-to-your-uwp-app"></a>將相依性新增到 UWP App

若要將相依性新增到 DLL 和 WINMD，請將它們放到您 **Unity Assets** 資料夾中的 **Plugins** 資料夾內，然後在檢查程式中選取它們並做出適當的目標平台設定。

![UWP 方案](images/uwp-solution.PNG)

**_ScrapyardPhoenix (通用 Windows)_** 是您要新增參考的專案，例如 Xbox Live SDK。

## <a name="see-also"></a>另請參閱

- [將現有的遊戲移到 Xbox](development-lanes-landing.md)
- [Xbox One 上的 UWP](index.md)
