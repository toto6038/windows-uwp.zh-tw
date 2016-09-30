---
Unity：將遊戲移至 Xbox One 作者：JordanEllis6809 
---

# Unity：將遊戲移至 Xbox One

在這個逐步教學課程中，我們假設您已經有 Unity 的遊戲，並已準備好建置和部署。

[本教學課程的影片版本。](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

要查看您的 Unity UWP 專案相關版本嗎？  [檢視此處](development-lanes-unity-versioning.md)。

## 步驟 0：確定 Unity 已正確安裝

安裝 Unity 時，必須選取以下元件：

![Unity 安裝元件](images/unity-install-components.png)

## 步驟 1︰建置 UWP 方案

在您的 Unity 遊戲專案中，開啟 `File -> Build Settings...` 中的 \[ Build Settings\] \(建置設定\) 視窗，移至 \[Windows Store\] \(Windows 市集\) 選項功能表，如下所示。

![建置設定視窗](images/build-settings.png)

請確定 `SDK` 設定是設為 `Universal 10`。 接下來，按下功能表底部的 \[Build\] \(建置\) 按鈕，這會啟動要求目的地資料夾的檔案總管視窗。 在專案的 `Assets` 目錄中建立名為 `UWP` 的資料夾，然後選擇此資料夾做為建置的目的地資料夾。

![建置目的地資料夾](images/build-destination.png)

Unity 現在已經建立了新的 Visual Studio 解決方案，我們將在後續步驟中用來部署 UWP 遊戲。

![UWP VS 解決方案](images/uwp-vs-solution.png)

## 步驟 2︰部署您的遊戲

開啟 `Assets/UWP` 資料夾中新產生的解決方案。  開啟後，將目標平台變更為 X64。

![x64 建置平台](images/x64-build-platform.png)

現在您已經有遊戲的 UWP Visual Studio 解決方案，[遵循下列步驟](https://msdn.microsoft.com/windows/uwp/xbox-apps/getting-started)將可讓您順利將遊戲部署到零售版 Xbox One！

## 步驟 3︰修改並重新建置

如果對指令碼以外的項目進行變更，為了讓這些變更可以在您遊戲的 UWP 建置中顯示，專案必須在編輯器中重新建置 (如__步驟 1__ 中所述)。

## 管理 UWP 專案的版本

在一些常見的情況下，將新產生之 UWP 目錄的一部份加入版本控制已成為必要。  例如，假如您正在新增新的相依性到 UWP 專案 (也就是 Xbox Live SDK)。  我們將在[這裡](development-lanes-unity-versioning.md)詳細說明此範例。



<!--HONumber=Jul16_HO2-->


