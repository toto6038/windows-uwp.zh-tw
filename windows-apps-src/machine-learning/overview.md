---
author: serenaz
title: Windows ML 概觀
description: 深入了解 Windows 機器學習，以及如何使用 Windows ML 來開發。
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、Windows 機器學習
ms.localizationpriority: medium
ms.openlocfilehash: a2470cff6b5c7f07c720a38d0bff00486e4c6b27
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842868"
---
# <a name="windows-ml-overview"></a>Windows ML 概觀

![Windows 機器學習圖形](images/brain.png)

## <a name="what-is-machine-learning"></a>什麼是機器學習？

機器學習 (ML) 可讓電腦使用現有資料來預測結果和行為。 透過處理先前所收集的資料，ML 演算法組建的模型可以在有新輸入時預測正確的輸出。 例如，可以訓練模型來評估電子郵件訊息 (輸入) 為垃圾郵件或非垃圾郵件 (輸出)。

模型建置階段稱為「訓練」。 在使用現有資料進行訓練，該模型就可以用新的，以前未見過的資料進行預測，這些資料稱為「推論」、「評估」或「評分」。

訓練過的模型往往比根據一套嚴格的指令所編寫的程式更能產出更好的結果，特別是對於具有許多可能輸入及輸出組合的複雜任務。 例如，推薦的演算法為電子商務和媒體串流處理網站上的數百萬使用者提供個性化的推薦，這在沒有機器學習的情況下幾乎是不可能的。 利用機器學習的另一領域是電腦辨識，可讓電腦在對之前標記的影像進行訓練之後對影像進行分類和辨識。

機器學習的可能性和應用是無止境的，如需研究和方案的詳細資訊，請造訪 [Microsoft 的 人工智慧](https://www.microsoft.com/ai) 和 [Microsoft AI 平台](https://azure.microsoft.com/en-us/overview/ai-platform/)。 如果您想要組建機器學習和 AI 模型，您還可以查看 [Azure Machine Learning Services](https://docs.microsoft.com/azure/machine-learning/preview/overview-what-is-azure-ml)。

## <a name="what-is-windows-ml"></a>什麼是 Windows ML？

Windows ML 是在 Windows 10 裝置上評估訓練過的機器學習模型的平台，允許開發人員在其 Windows 應用程式中使用機器學習。

Windows ML 的一些醒目提示包括：

- **硬體加速**
    
    在支援 DirectX12 的裝置上，Windows ML 使用 GPU 加速對深入學習模型的評估。 此外 CPU 最佳化可實現傳統 ML 和深度學習這兩種演算法的高效能評估。

- **本機評估**

    Windows ML 評估本機硬體，消除連線性、頻寬和資料隱私權的問題。 本機評估還可啟用低延遲和高效能，以取得快速評估結果。

- **影像處理**

    對於電腦視覺案例，Windows ML 透過處理模型輸入的框架預處理和提供相機管線設定來簡化和最佳化影像、視訊和相機資料的使用。

## <a name="how-to-develop-with-windows-ml"></a>如何使用 Windows ML 進行開發

> [!VIDEO https://www.youtube.com/embed/8MCDSlm326U]

### <a name="system-requirements"></a>系統需求

若要組建使用 Windows ML 的應用程式，您需要 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) - 組建 17110 或更高版本。

### <a name="onnx-models"></a>ONNX 模型

要使用 Windows ML，您需要使用 [開放類神經網路 Exchange (ONNX)](https://onnx.ai) 格式的預先訓練的機器學習模型。 Windows ML 支援 ONNX 格式的 v1.0 版本，該版本可讓開發人員使用由不同訓練架構產生的模型。

如需公開可用 ONNX 模型的清單，請參閱 GitHub 上的 [ONNX 模型](https://github.com/onnx/models)。

若要了解如何使用 Visual Studio Tools for AI 來訓練 ONNX 模型，請參閱 [訓練模型](train-ai-model.md)。

### <a name="convert-existing-models-to-onnx"></a>將現有的模型轉換成 ONNX

許多訓練架構已經原生支援 ONNX 模型，並且有許多架構和程式庫的轉換工具。 若要了解如何從架構 (如 Caffe 2、PyTorch、CNTK、Chainer 等等) 中匯出，請參閱 GitHub 上的 [ONNX 教學課程](https://github.com/onnx/tutorials)。

您也可以使用 [WinMLTools](https://pypi.org/project/winmltools/) 將訓練過的機器學習模型轉換到 Windows ML 接受的 ONNX 格式。 WinMLTools 支援這些格式的轉換：

- Core ML
- Scikit-Learn
- XGBoost
- LibSVM

若要了解如何安裝和使用 WinMLTools，請參閱 [轉換模型](conversion-samples.md)。

有了 Visual Studio Tools for AI 延伸，您也可以在 Visual Studio IDE 內使用 WinMLTools 感受更簡便的點擊體驗，將您的機型轉換成 ONNX 格式。 若要進一步瞭解，請參閱 [VS Tools for AI](https://github.com/Microsoft/vs-tools-for-ai/)。

### <a name="onnx-operators"></a>ONNX 運算子

Windows ML 支援 CPU 上的 100+ ONNX 運算子，並加速 DirectX12 相容 GPU上的計算。 若要電信業者簽章的完整清單，請參閱 [ai.onnx](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators.md) (預設) 和 [ai.onnx.ml](https://github.com/onnx/onnx/blob/rel-1.0/docs/Operators-ml.md) 命名空間的 ONNX 電信業者的結構描述文件。

Windows ML 支援 ONNX v1.0 文件中所定義的所有運算子，其區別如下：

- Windows ML 支援的標記為「實驗性」的運算子：
    - 仿射
    - 裁切
    - FC
    - 身份識別
    - ImageScaler
    - MeanVarianceNormalization
    - ParametricSoftplus
    - ScaledTanh
    - ThresholdedRelu
    - Upsample
- MatMul - 目前不支援大於 2D 矩陣乘法，只支援 CPU
- Cast - 只支援 CPU
- 目前不支援下列運算子：
    - RandomUniform
    - RandomUniformLike
    - RandomNormal
    - RandomNormalLike

### <a name="automatic-interface-code-generation"></a>介面程式碼自動產生器

使用 ONNX 模型檔案，Windows ML 的程式碼產生器會建立介面，以便您的應用程式中的模型進行互動。 產生的介面包含代表模型、輸入和輸出的包裝函式類別。 產生的程式碼為您呼叫 [Windows ML API](/uwp/api/windows.ai.machinelearning.preview)，讓您輕鬆地在專案中載入、繫結和評估模型。 程式碼產生器目前支援 C# 和 C++/CX。

對於 UWP 開發人員，Windows ML 的自動程式碼產生本身與 [Visual Studio](https://developer.microsoft.com/windows/downloads) 原生整合。 在 Visual Studio 專案內部，只要將您 ONNX 檔案新增為現有的項目，VS 將會在新介面檔案中產生 Windows ML 包裝函式類別。

您還可以使用 Windows SDK 隨附的命令列工具 `mlgen.exe` 來產生 Windows ML 包裝函式類別。 該工具位於 `(SDK_root)\bin\<version>\x64` 或 `(SDK_root)\bin\<version>\x86` 中，其中 SDK_root 是 SDK 安裝目錄。 若要執行該工具，請使用下列命令。

```
mlgen -i INPUT-FILE -l LANGUAGE -n NAMESPACE [-o OUTPUT-FILE]
```

輸入參數定義：

- `INPUT-FILE`：ONNX 模型檔案
- `LANGUAGE`：CPPCX 或 CS
- `NAMESPACE`：產生程式碼的命名空間
- `OUTPUT-FILE`：將產生的程式碼寫入到檔案路徑。 如果不指定 OUTPUT-FILE (輸出檔案)，則產生的程式碼將寫入到標準輸出

若要了解如何在您的應用程式中使用產生的程式碼，請參閱 [整合模型](integrate-model.md)。

## <a name="next-steps"></a>後續步驟

嘗試使用 [開始](get-started.md) 中的逐步教學課程建立您的您的第一個 Windows ML 應用程式。