---
author: serenaz
title: 使用 Windows ML 將模型整合到您的應用程式中
description: 透過下列載入、繫結，以及評估模式，將模型整合到您的應用程式中。
ms.author: sezhen
ms.date: 03/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、winml、Windows 機器學習
ms.localizationpriority: medium
ms.openlocfilehash: 8631c07b45a8642a1de700e424d3ca4f147456fc
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842575"
---
# <a name="integrate-a-model-into-your-app-with-windows-ml"></a>使用 Windows ML 將模型整合到您的應用程式中

Windows ML 的 [自動程式碼產生](overview.md#automatic-interface-code-generation) 為您建立呼叫 [Windows ML APIs](/uwp/api/windows.ai.machinelearning.preview) 介面，讓您輕鬆與您的模型進行互動。 使用介面產生的包裝函式類別，請遵循載入、繫結和評估模式，以將 ML 模型整合到您的應用程式中。

![載入、繫結、評估](images/load-bind-evaluate.png)

我們將會在本文中，使用來自[開始](get-started.md)的 MNIST 模型來示範如何在應用程式中載入、繫結和評估模型。

## <a name="add-the-model"></a>新增模型

首先，您需要將 ONNX 模型新增到 Visual Studio 專案的資產中。 如果您使用 [Visual Studio (版本 15.7 - 預覽版 1)](https://www.visualstudio.com/vs/preview/)建置 UWP 應用程式，則 Visual Studio 會自動在新檔案中產生包裝函式類別。 對於其他工作流程，您需要使用 [mlgen](overview.md#automatic-interface-code-generation) 工具來產生包裝函式類別。

以下是為 MNIST 模型產生的 Windows ML 包裝函式類別。 我們將會使用本文的其餘部分來解釋如何在您的應用程式中使用這些類別。

```csharp
public sealed class MNISTModelInput
{
    public VideoFrame Input3 { get; set; }
}

public sealed class MNISTModelOutput
{
    public IList<float> Plus214_Output_0 { get; set; }
    public MNISTModelOutput()
    {
        this.Plus214_Output_0 = new List<float>();
    }
}

public sealed class MNISTModel
{
    private LearningModelPreview learningModel;
    public static async Task<MNISTModel> CreateMNISTModel(StorageFile file)
    {
        LearningModelPreview learningModel = await LearningModelPreview.LoadModelFromStorageFileAsync(file);
        MNISTModel model = new MNISTModel();
        model.learningModel = learningModel;
        return model;
    }
    public async Task<MNISTModelOutput> EvaluateAsync(MNISTModelInput input) {
        MNISTModelOutput output = new MNISTModelOutput();
        LearningModelBindingPreview binding = new LearningModelBindingPreview(learningModel);
        binding.Bind("Input3", input.Input3);
        binding.Bind("Plus214_Output_0", output.Plus214_Output_0);
        LearningModelEvaluationResultPreview evalResult = await learningModel.EvaluateAsync(binding, string.Empty);
        return output;
    }
}
```

**注意**：為了確保在編譯應用程式時組建模型，請按右鍵 `.onnx` 檔案，然後選擇 **Properties**。 對於 **建置動作**，請選擇**內容**。

## <a name="load"></a>載入

產生包裝函式類別之後，您就可以在您的應用程式中載入模型。

MNISTModel 類別代表 MNIST 模型，為了載入模型，我們會呼叫 CreateMNISTModel 方法，將 ONNX 檔案作為參數傳遞。

```csharp
// Load the model
StorageFile modelFile = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/MNIST.onnx"));
MNISTModel model = MNISTModel.CreateMNISTModel(modelFile);
```

## <a name="bind"></a>繫結

產生的程式碼也包含輸入和輸出包裝函式類別。 輸入類代表模型的預期輸入，輸出類代表模型的預期輸出。

若要初始化模型的輸入物件，請呼叫輸入類建構函數，傳遞應用程式的資料，並確保輸入資料符合您的模型所期望的輸入類型。 MNISTModelInput 類想要一個 VideoFrame，所以我們使用輔助方法來取得輸入的 VideoFrame。

```csharp
//Bind the input data to the model
MNISTModelInputs ModelInput = new MNISTModelInputs();
ModelInput.Input3 = await helper.GetHandWrittenImage(inkGrid);
```

輸出物件初始化為*空*值，並將會在下一步「評估」之後使用模型結果進行更新。

## <a name="evaluate"></a>評估

在您的輸入初始化後，呼叫模型的 EvaluateAsync 方法來評估輸入資料上的模型。 EvaluateAsync 將您的輸入和輸出繫結到模型物件，並在輸入上評估模型。

```csharp
// Evaluate the model
MNISTModelOuput ModelOutput = model.EvaluateAsync(ModelInput);
```

經過評估後，您的輸出包含模型的結果，您現在可以檢視和分析結果。 由於 MNIST 模型輸出清單的機率，因此我們遍歷清單來尋找並顯示最大的機率數字。

```csharp
 //Iterate through output to determine highest probability digit
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
```

## <a name="using-the-windows-ml-apis-directly"></a>直接使用 Windows ML APIs

雖然 Windows ML 自動程式碼產生器提供了與模型進行互動的方便介面，但您不需要使用包裝函式類別。 而是可以直接在您的應用程式中呼叫 Windows ML API。
如果您想這樣做，您將會遵循相同的模式：載入模型、繫結輸入和輸出，並進行評估。

如需使用 API​​ 的詳細資訊，請參閱 [Windows ML API 參照](/uwp/api/windows.ai.machinelearning.preview)。
