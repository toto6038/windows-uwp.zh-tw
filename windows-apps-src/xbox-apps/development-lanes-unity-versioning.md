---
title: Unity：針對您的 UWP 專案進行版本控制
description: 管理您 Unity UWP 專案的版本。
ms.localizationpriority: medium
ms.openlocfilehash: 9fe59b0c63f536822ec50a225bbdc11e68b97713
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7711630"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity：針對您的 UWP 專案進行版本控制

尚未使用通用 Windows 平台 (UWP) 針對 Xbox 建置您的 Unity 遊戲嗎？  請先參閱[將 Unity 遊戲移到 Xbox 上的 UWP](development-lanes-unity.md)。

有幾個不同的理由會讓您想要將產生的 UWP 目錄新增到版本控制，其中一個是新增相依性 (例如 Xbox Live SDK)。  我們會在本教學課程中以此案例做為範例，希望它會有助於您解決專案的個別需求。

***免責聲明：我們將使用 Git 做為版本控制解決方案。  如果您的版本控制解決方案不同，概念仍應該可以通用。***

為了重新整理您的記憶，以下是我們的遊戲 ***ScrapyardPhoenix*** 的目錄，目前看起來如下：

![建置目的地資料夾](images/build-destination.png)

而我們的 UWP 目錄看起來像這樣：

![UWP VS 解決方案](images/uwp-vs-solution.png)

在此目錄中，我們只需關注 ***ScrapyardPhoenix*** (請以您的遊戲名稱取代) 資料夾。  在我們的版本控制中，所有其他項目都可以忽略。

***不熟悉 .gitignore 檔是什麼嗎？  請參閱 [gitignore](https://git-scm.com/docs/gitignore)。***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

我們會從 **UWP/ScrapyardPhoenix** 資料夾中選取幾個不同的檔案與資料夾，以新增到我們的版本控制。  我們先看看完整的詳細資料：

![UWP 建置目錄](images/uwp-build-directory.png)  

## <a name="folders"></a>資料夾  

`Assets` | ***包含***|包含 Microsoft Store 的影像  
`Data`   | ***忽略***|其中 Unity 編譯您的專案 （場景、 著色器、 指令碼、 Prefab 等等。）  
`Dependencies` | ***包含***|此資料夾是我建立來保留所有的 UWP 相依性 (例如 XboxLiveSDK.dll)  
`Properties` | ***包含***|包含可由開發人員修改的更多進階的設定  
`Unprocessed` | ***忽略***|包含 Unity`.dll`和`.pdb`檔案  

## <a name="files"></a>檔案  

`App.cs` | ***包含***|您的 UWP 應用程式; 進入點這可以修改和延伸透過其他來源檔案  
`Package.appxmanifest` | ***包含***|針對您的 AppX 應用程式套件資訊清單來源檔案  
`project.json` | ***包含***|描述的 NuGet 套件您`*.csproj`取決於  
`ScrapyardPhoenix.csproj` | ***包含***|描述您的 UWP 建置目標;如果您將其他相依性新增到您的 UWP 專案，此`*.csproj`檔案將會包含該資訊  
`ScrapyardPhoenix.csproj.user` | ***忽略***|這個檔案包含本機使用者資訊

## <a name="resulting-gitignore"></a>產生的 .gitignore

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

這樣就好了，現在您的小組成員將會與您產生的 UWP 專案同步。 現在您可以放心地將其他資產、來源和相依性新增到您的 UWP 專案。

如需更多針對 UWP 資料夾進行版本管理的範例，可在[這些範例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)中找到。

## <a name="adding-dependencies-to-your-uwp-app"></a>將相依性新增到 UWP App

若要將相依性新增到 DLL 和 WINMD，請將它們放到您 **Unity Assets** 資料夾中的 **Plugins** 資料夾內，然後在檢查程式中選取它們並做出適當的目標平台設定。

![UWP 方案](images/uwp-solution.PNG)

***ScrapyardPhoenix (通用 Windows)*** 是您會將參考新增至如 Xbox Live SDK 等的專案。

## <a name="see-also"></a>另請參閱
- [將現有的遊戲移到 Xbox](development-lanes-landing.md)
- [Xbox One 上的 UWP](index.md)
