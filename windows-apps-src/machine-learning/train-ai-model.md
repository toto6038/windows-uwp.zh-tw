---
author: serenaz
title: 如何在 Visual Studio 中訓練 Windows ML 模型
description: 透過本逐步教學課程，了解如何使用 Visual Studio Tools for AI 來訓練 Windows ML 模型。
ms.author: sezhen
ms.date: 03/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、uwp、Windows 機器學習、Visual Studio
ms.localizationpriority: medium
ms.openlocfilehash: 1b54b0665a2483b8a0be710f505e928c852f4dba
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842665"
---
# <a name="how-to-train-a-model-for-windows-ml-in-visual-studio"></a>如何在 Visual Studio 中訓練 Windows ML 模型

在本教學課程中，我們將會使用 [Visual Studio Tools for AI](http://aka.ms/vstoolsforai)，用於建置、測試和部署深度學習和 AI 解決方案的延伸模組開發，為 [開始使用](get-started.md) 中的 MNIST 範例應用程式訓練模型。

我們將使用 [Microsoft Cognitive Toolkit (CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit) 架構和 [MNIST 資料集](http://yann.lecun.com/exdb/mnist/)來訓練模型，其中有 60000 個範例的訓練組和 10000 個手寫數字範例的測試組。 我們接著將使用[開放類神經網路 Exchange (ONNX)](https://onnx.ai/) 格式儲存模型以利於與 Windows ML 一起使用。

## <a name="prerequisites"></a>必要條件
### <a name="install-visual-studio-tools-for-ai"></a>安裝 AI 的 Visual Studio 工具
若要開始，您需要下載並安裝 [Visual Studio](https://www.visualstudio.com/downloads/)。 打開 Visual Studio 後，啟用 **Visual Studio Tools for AI** 擴充功能：

1. 按一下 Visual Studio 中的選單列，並選取「擴充功能與更新...」
2. 按一下「線上」標籤並選取「搜尋 Visual Studio Marketplace」。
3. 搜尋「Visual Studio Tools for AI」。 
3. 按一下**下載**按鈕。 
4. 安裝後，請重新開機 Visual Studio。 

Visual Studio 重新開機後，該擴充功能將會使用中。 如果遇到問題，請查看 [尋找 Visual Studio 的擴充功能](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions)。

### <a name="download-sample-code"></a>下載範例程式碼
在 GitHub 中下載 [AI 範例](https://github.com/Microsoft/samples-for-ai)存放庫。 本範例涵蓋了取得開始使用跨 TensorFlow、CNTK、Theano 等等範圍的深入學習。

### <a name="install-cntk"></a>安裝 CNTK
安裝[適用於 Windows 的 Python CNTK](https://docs.microsoft.com/en-us/cognitive-toolkit/setup-windows-python?tabs=cntkpy24)。 請注意，如果您尚未安裝 Python，則必須安裝 Python。

另外，為了讓您的機器準備深度學習模型開發，請參閱[準備您的開發環境](https://github.com/Microsoft/samples-for-ai/blob/master/README.md)以取得安裝 Python、CNTK、TensorFlow、 NVIDIA GPU 驅動程式 (選用) 等等的簡化安裝程序。

## <a name="1-open-project"></a>1. 開啟專案

啟動 Visual Studio，然後選取**檔案 > 開啟 > 專案/方案**. 從 AI 存放庫樣本中選擇 **examples\cntk\python** 資料夾，然後打開 **CNTKPythonExamples.sln** 檔案。

![開放解決方案](images/open-solution.png)

## <a name="2-train-the-model"></a>2. 訓練模型

若要將 MNIST 專案設定為啟動專案，請按右鍵 python 專案並選擇 **\[設定為啟動專案\]**。

![開啟解決方案](images/mnist-startup.png)

接下來，透過按下 F5 或綠色的 \[執行\] 按鈕開啟 train_mnist_onnx.py 檔案並**執行**專案。

## <a name="3-view-the-model-and-add-it-to-your-app"></a>3. 檢視模型並將其新增到您的應用程式

現在，受過訓練的 **mnist.onnx** 模型檔案應在 samples-for-ai/examples/cntk/python/MNIST 資料夾中。 您可以使用這個訓練過的 **mnist.onnx** 模型檔案來組建 [入門](get-started.md)中的 MNIST 範例應用程式！ 

## <a name="4-learn-more"></a>4. 深入了解
若要了解如何使用 [Azure GPU 虛擬電腦](https://docs.microsoft.com/en-us/visualstudio/ai/tensorflow-vm) 及其他來加速訓練深度學習模型，請造訪 [Microsoft 的人工智慧](https://www.microsoft.com/ai)和 [Microsoft 機器學習技術](https://docs.microsoft.com/en-us/azure/machine-learning/#More-Microsoft-Machine-Learning-Technologies)。