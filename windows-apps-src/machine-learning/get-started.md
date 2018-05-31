---
author: serenaz
title: 開始使用 Windows ML。
description: 使用此逐步教學課程建立您的第一個 Windows ML 應用程式。
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、Windows 機器學習、winml、Windows ML
ms.localizationpriority: medium
ms.openlocfilehash: e30786f775a66bcf5c8e6dce0b4aab4f1f239be6
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816583"
---
# <a name="get-started-with-windows-ml"></a>開始使用 Windows ML。

在本教學課程中，我們會建置簡單的 UWP 應用程式，使用經過訓練的機器學習模型來辨識使用者繪製的數字。 本教學課程主要著重於如何在您的應用程式中載入和使用 Windows ML。

## <a name="prerequisites"></a>必要條件

- [Windows SDK - 組建 17110+](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK)
- [Visual Studio (版本 15.7 - 預覽版 1)](https://www.visualstudio.com/vs/preview/) 

    **注意**：在 Visual Studio 安裝程式，您需要查看檢查選用的 Windows 10 預覽版 SDK (10.0.17110.0)。

## <a name="1-download-the-sample"></a>1. 下載範例

首先，您需要從 GitHub 下載我們的 [MNIST_GetStarted 範例](https://github.com/Microsoft/Windows-Machine-Learning)。 我們已提供搭配實作 XAML 控制項和事件的樣板，包括：

- [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) 繪製數字。
- [按鈕](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button) 解釋數字並清除畫布。
- 協助程式常式將 InkCanvas 輸出轉換成 [VideoFrame](https://docs.microsoft.com/uwp/api/windows.media.videoframe)。

完整的 MNIST 範例也可從 GitHub 下載。

## <a name="2-open-project-in-visual-studio-preview"></a>2. 在 Visual Studio 預覽中開啟專案。

啟動 Visual Studio 預覽版，並打開 MNIST 應用程式範例。 (如果解決方案顯示為無法使用，則需要按右鍵，然後選取「重新載入專案。」)

在方案總管中，該專案有三個主要的程式碼檔案：

- `MainPage.xaml` - 我們所有的 XAML 的程式碼，用於為 InkCanvas、按鈕及標籤建立 UI。
- `MainPage.xaml.cs` - 我們的應用程式程式碼所在之處。
- `Helper.cs` - 協助程式常式裁剪和轉換影像格式。

![具有專案檔案的 Visual Studio 方案總管](images/get-started1.png)

## <a name="3-build-and-run-project"></a>3. 建置和執行專案

在 Visual Studio 工具列上，將解決方案平台從「ARM」變更為「X64」，以在本機電腦上執行專案。

若要執行專案，按一下工具列上的**開始偵錯**按鈕，或按下 F5。 應用程式應該會顯示 InkCanvas，使用者可以在其中寫入數字，「辨識」按鈕解釋數字，解釋數字的空白標籤欄位將顯示為文字，「清除數字」按鈕清除 InkCanvas。

![應用程式螢幕擷取畫面](images/get-started2.png)

**注意**：如果您下載的組建高於 17110，則可能需要變更專案的部署目標版本。 在專案方案下，請移至**屬性**。 在 [應用程式] 索引標籤中，設定目標版本以符合您的作業系統和 SDK。

## <a name="4-download-a-model"></a>4. 下載模型

接下來，取得機器學習模型以新增到我們的應用程式中。 本教學課程中，我們會使用經由 [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) 和 [匯出為 ONNX 格式](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb) 訓練的預先訓練 **MNIST 模型**。

如果您使用 GitHub 的 MNIST_GetStarted 範例，則 MNIST 模型已包含在您的資產資料夾中，您需要將其視為現有項目新增到您的應用程式中。 您也可以從 GitHub上的 [ONNX 模型動物園](https://github.com/onnx/models) 下載預先訓練模型。

如果您對訓練自己模型感興趣，則可以遵循我們用來訓練 MNIST 模型的 [tutorial](train-ai-model.md)。

## <a name="5-add-the-model"></a>5. 新增模型

下載 MNIST 模型後，在方案總管中的資產資料夾按右鍵，然後選擇「**Add** > **Existing Item**」。 將檔案選擇器指向 ONNX 模型的位置，然後按一下新增。 

該專案現在應該有兩個新的檔案：

- `MNIST.onnx` - 您的訓練模型。
- `MNIST.cs` - Windows ML 產生的程式碼。

![具有新檔案的方案總管](images/get-started3.png)

在編譯應用程式時為了確保模型組建，請按右鍵 `mnist.onnx` 檔案，然後選擇 **Properties**。 對於 **建置動作**，請選擇**內容**。

現在，我們來看看在 `MNIST.cs` 檔案中新產生的程式碼。 我們有三類：

- **MNISTModel** 建立機器學習模型代表，將特定輸入和輸出繫結到模型，並非同步評估模型。 
- **MNISTModelInput** 初始化模型期望的輸入類型。 此時，請輸入 VideoFrame。
- **MNISTModelOutput** 初始化模型將輸出的類型。 在這種情況下，輸出將是稱為「classLabel」類型 `<long>` 的清單和稱為「預測」類型的字典 `<long, float>`

現在我們將使用這些類別，以便在我們的專案中載入、繫結，以及評估模型。

## <a name="6-load-bind-and-evaluate-the-model"></a>6. 載入、繫結，以及評估模型

對於 Windows ML 應用程式，我們想要遵循的模式是：載入 > 繫結 > 評估。

- 載入機器學習模型。
- 將輸入和輸出繫結到模型。
- 評估模型和檢視結果。

我們將使用 `MNIST.cs` 中產生的介面的程式碼，在我們的應用程式中載入、繫結，以及評估模型。

首先，在 `MainPage.xaml.cs` 中，讓我們初始化模型、輸入，以及輸出。

```csharp
namespace MNIST_Demo
{
    public sealed partial class MainPage : Page
    {
        private MNISTModel ModelGen = new MNISTModel();
        private MNISTModelInput ModelInput = new MNISTModelInput();
        private MNISTModelOutput ModelOutput = new MNISTModelOutput();
        ...
    }
}
```

然後，在 LoadModel()，我們將會載入模型。

```csharp
private async void LoadModel()
{
    //Load a machine learning model
    StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
    ModelGen = await MNISTModel.CreateMNISTModel(modelFile);
}
```

接下來，我們想要將我們的輸入與輸出繫結至模型。 

在這種情況下，我們模型需要 VideoFrame 類型的輸入。 在 `helper.cs` 中使用我們隨附的協助程式函數，我們將會複製 InkCanvas 的內容，將其轉換成 VideoFrame 類型，並將其繫結到我們的模型。

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
     //Bind model input with contents from InkCanvas
     ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
}
```

對於輸出，我們只需使用指定的輸入來呼叫 EvaluateAsync()。 由於模型以對應的機率傳回數字清單，我們需要剖析傳回的清單以判斷哪個數字具有最大的機率並顯示該數字。

```csharp
private async void recognizeButton_Click(object sender, RoutedEventArgs e)
{
    //Bind model input with contents from InkCanvas
    ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
    
    //Evaluate the model
    ModelOutput = await ModelGen.EvaluateAsync(ModelInput);
            
    //Iterate through evaluation output to determine highest probability digit
    float maxProb = 0;
    int maxIndex = 0;
    for (int i = 0; i < 10; i++)
    {
        if (ModelOutput.Plus214_Output_0[i] > maxProb)
        {
            maxIndex = i;
            maxProb = ModelOutput.Plus214_Output_0[i];
        }
    }
    numberLabel.Text = maxIndex.ToString();
}
```

最後，我們要清除 InkCanvas 以讓使用者繪製另一個數字。

```csharp
private void clearButton_Click(object sender, RoutedEventArgs e)
{
    inkCanvas.InkPresenter.StrokeContainer.Clear();
    numberLabel.Text = "";
}
```

## <a name="7-launch-the-app"></a>7. 啟動應用程式

在我們組建並啟動應用程式之後，我們就能辨識 InkCanvas 上繪製的數字。

![完整的應用程式](images/get-started4.png)

## <a name="8-next-steps"></a>8. 後續步驟

這樣就完成了，您已經製作了您的第一個 Windows ML 應用程式！ 如需更多範例示範如何使用 Windows ML，請查看 [應用程式範例](samples.md)。