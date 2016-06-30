---
title: "將 Unity 遊戲移到 Xbox One"
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# 將 Unity 遊戲移到 Xbox One

在這個逐步教學課程中，我們假設您已經有 Unity 的遊戲，並已準備好建置和部署。

[本教學課程的影片版本。](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## 步驟 0：確定 Unity 已正確安裝

安裝 Unity 時，必須選取以下元件：

![Unity 安裝元件](images/unity-install-components.png)

## 步驟 1︰建置 UWP 方案

在您的 Unity 遊戲專案中，開啟  中的 \[ Build Settings\] (建置設定) 視窗，移至 \[Windows Store\] (Windows 市集) 選項功能表，如下所示。

![建置設定視窗](images/build-settings.png)

請確定 `SDK` 設定是設為 `Universal 10`。 接下來，按下功能表底部的 \[Build\] (建置) 按鈕，這會啟動要求目的地資料夾的檔案總管視窗。 在專案的 `Assets` 目錄中建立名為 `UWP` 的資料夾，然後選擇此資料夾做為建置的目的地資料夾。

![建置目的地資料夾](images/build-destination.png)

Unity 現在已經建立了新的 Visual Studio 解決方案，我們將在後續步驟中用來部署 UWP 遊戲。

![UWP VS 解決方案](images/uwp-vs-solution.png)

## 步驟 2︰部署您的遊戲

開啟 `Assets/UWP` 資料夾中新產生的解決方案。  開啟後，將目標平台變更為 X64。

![x64 建置平台](images/x64-build-platform.png)

現在您已經有遊戲的 UWP Visual Studio 解決方案，[遵循下列步驟](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)將可讓您順利將遊戲部署到零售版 Xbox One！

## 開發人員注意事項

- 建議您在版本控制中忽略 UWP 資料夾。 如果您想要將其他 XAML 元素新增到專案中，並且需要對 UWP 資料夾中的某些資產進行版本控制，請參閱[進行該工作的範例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)。

- 如果您在 Unity 中對包含在遊戲建置內的任何內容 (除了指令碼以外) 進行變更，您必須重新建置 UWP 解決方案，才會在下一次部署時看到那些變更。 這是因為在 Unity 的建置步驟期間，您專案的所有資產會編譯到一個資源檔案中。 當部署遊戲時，UWP 解決方案會參考所產生的資源檔案。




<!--HONumber=Jun16_HO4-->


