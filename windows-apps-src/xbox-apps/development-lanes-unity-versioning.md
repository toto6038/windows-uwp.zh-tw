---
title&#58; Unity：針對您的 UWP 專案進行版本控制 作者：JordanEllis6809
---

# Unity：針對您的 UWP 專案進行版本控制

還沒有使用 UWP 建置適用於 Xbox 的遊戲嗎？  請先閱讀[此處](development-lanes-unity.md)。

您想要將產生的 UWP 目錄新增到版本控制的原因有幾個不同的理由，其中一個是新增相依性 (也就是 Xbox Live SDK)。  我們在本教學課程中將以此案例做為範例，希望它會有助於您解決專案的個別需求。

***免責聲明：我們將使用 Git 做為版本控制解決方案。  如果您的版本控制解決方案不同，概念仍應該可以通用。***

為了重新整理您的記憶，以下是我們的遊戲 ***ScrapyardPhoenix*** 的目錄，目前看起來如下：

![建置目的地資料夾](images/build-destination.png)

而我們的 UWP 目錄看起來像這樣：

![UWP VS 解決方案](images/uwp-vs-solution.png)

在此目錄中，我們僅需關注一個資料夾，***ScrapyardPhoenix*** (在此插入您的遊戲名稱) 資料夾。  在我們的版本控制中可以忽略所有其他項目：

***不熟悉 .gitignore 檔是什麼嗎？  在[這裡](https://git-scm.com/docs/gitignore)深入了解。***

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

我們將會從 `UWP/ScrapyardPhoenix` 中選取一些不同的檔案與資料夾，新增到我們的版本控制中。  我們先看看完整項目的詳細資訊：

![UWP 建置目錄](images/uwp-build-directory.png)  

## 資料夾  

`Assets` | 
            ***包含*** | 包含 Windows 市集影像。  
`Data`   | 
            ***忽略*** | Unity 編譯您專案的目標位置 (場景、著色器、指令碼、Prefabs 等等)。  
`Dependencies` | 
            ***包含*** | 此資料夾是建立以在其中保留所有 UWP 的相依性 (也就是 XboxLiveSDK.dll)。  
`Properties` | 
            ***包含*** | 包含開發人員可修改的進階設定。  
`Unprocessed` | 
            ***忽略*** | 包含 Unity `.dll` 和 `.pdb` 檔案。  

## 檔案  

`App.cs` | 
            ***包含*** | 您的 UWP 應用程式的進入點。  此檔案可以修改，並搭配其他來源檔案擴充。  
`Package.appxmanifest` | 
            ***包含*** | 您的 AppX 的套件資訊清單。  
`project.json` | 
            ***包含*** | 描述您的 `*.csproj` 所依存的 NuGet 套件。  
`ScrapyardPhoenix.csproj` | 
            ***包含*** | 描述您的 UWP 建置目標。  如果您將其他相依性新增到 UWP 專案，這個 `*.csproj` 檔案將會包含該資訊。  
`ScrapyardPhoenix.csproj.user` | 
            ***忽略*** | 此檔案包含本機使用者資訊。

## 產生 .gitignore

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

這樣就好了，現在您的小組成員將會與您產生的 UWP 專案同步。  現在您可以放心地將其他資產、來源和相依性新增到您的 UWP 專案。

如需一些針對 UWP 資料夾版本管理的詳細範例，可在[這些範例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)中找到。

## 將相依性新增到 UWP App

將相依性新增到 UWP 應用程式，就像您要在任何其他 Visual Studio 專案中所完成的一樣。  此處要釐清的唯一瑣碎細項，是在我們的方案中要新增相依性到哪個專案。  以下影像顯示該專案 (已加上醒目提示)：

![UWP 方案](images/uwp-solution.png)


            ***ScrapyardPhoenix (通用 Windows)*** 是您要新增參考 (例如，Xbox Live SDK) 的專案。



<!--HONumber=Jul16_HO1-->


