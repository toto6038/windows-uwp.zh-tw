---
author: wschin
title: 將現有的 ML 模型轉換成 ONNX
description: 程式碼範例示範如何使用 WinMLTools 將 scikit-learn 和 Core ML 中的現有模型轉換成 ONNX 格式。
ms.author: sezhen
ms.date: 3/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10、Windows 機器學習、WinML、WinMLTools、ONNX、ONNXMLTools、 scikit-learn、Core ML
ms.localizationpriority: medium
ms.openlocfilehash: 882efca26730c990093a89a5ed3ff4b5587e05bf
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832676"
---
# <a name="convert-existing-ml-models-to-onnx"></a>將現有的 ML 模型轉換成 ONNX

[WinMLTools](https://aka.ms/winmltools)，可讓使用者將其他架構中訓練的模型轉換成 ONNX 格式。 以下示範如何安裝 WinMLTools 套件以及如何透過 Python 的程式碼將 scikit-learn 和 Core ML 中的現有模型轉換成 ONNX。

## <a name="install-winmltools"></a>安裝 WinMLTools

WinMLTools 是一個 Python 套件 (winmltools)，可支援 Python 2.7 和 3.6 版本。 如果您正在從事資料科學專案，我們推薦安裝科學 Python 發行版，例如 Anaconda。

WinMLTools 遵循[標準 Python 套件安裝程序](https://packaging.python.org/installing/)。 從您的 python 環境中執行：

```
pip install winmltools
```

WinMLTools 有下列相依性：

- numpy v1.10.0+
- onnxmltools 0.1.0.0+
- protobuf v.3.1.0+

若要更新相關套件，請使用 '-U' 引數執行 pip 命令。

```
pip install -U winmltools
```

請遵循 GitHub 上的 [onnxmltools](https://github.com/onnx/onnxmltools)，以取得更多有關 onnxmltools 相依性的資訊。

有關如何使用 WinMLTools 的其他詳細資料，見於包含輔助說明功能的套件專用檔案。

```
help(winmltools)
```

## <a name="scikit-learn-models"></a>Scikit-learn 模型

下列程式碼片段在 scikit-learn 中訓練線性支援向量電腦，並將模型轉換成 ONNX。

~~~python
# First, we create a toy data set.
# The training matrix X contains three examples, with two features each.
# The label vector, y, stores the labels of all examples.
X = [[0.5, 1.], [-1., -1.5], [0., -2.]]
y = [1, -1, -1]

# Then, we create a linear classifier and train it.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()
linear_svc.fit(X, y)

# To convert scikit-learn models, we need to specify the input feature's name and type for our converter. 
# The following line means we have a 2-D float feature vector, and its name is "input" in ONNX.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
linear_svc_onnx = convert_sklearn(linear_svc, name='LinearSVC', input_features=[('input', 'float', 2)])    

# Now, we save the ONNX model into binary format.
from winmltools.utils import save_model
save_model(linear_svc_onnx, 'linear_svc.onnx')

# If you'd like to load an ONNX binary file, our tool can also help.
from winmltools.utils import load_model
linear_svc_onnx = load_model('linear_svc.onnx')

# To see the produced ONNX model, we can print its contents or save it in text format.
print(linear_svc_onnx)
from winmltools.utils import save_text
save_text(linear_svc_onnx, 'linear_svc.txt')

# The conversion of linear regression is very similar. See the example below.
from sklearn.svm import LinearSVR
linear_svr = LinearSVR()
linear_svr.fit(X, y)
linear_svr_onnx = convert_sklearn(linear_svr, name='LinearSVR', input_features=[('input', 'float', 2)])   
~~~

使用者可以使用其他 scikit-learn 模型取代 `LinearSVC`，例如 `RandomForestClassifier`。 請注意，[automatic code generator](overview.md#automatic-interface-code-generation) 使用  `name` 參數來產生課程名稱及變數。 如果沒有提供 `name`，則會產生 GUID，這將不符合像 C++/C# 此類語言的變數命名慣例。 

## <a name="scikit-learn-pipelines"></a>Scikit-learn 管線

接下來，我們會示範如何將 scikit-learn 管線轉換成 ONNX。

~~~python
# First, we create a toy data set.
# Notice that the first example's last feature value, 300, is large.
X = [[0.5, 1., 300.], [-1., -1.5, -4.], [0., -2., -1.]]
y = [1, -1, -1]

# Then, we declare a linear classifier.
from sklearn.svm import LinearSVC
linear_svc = LinearSVC()

# One common trick to improve a linear model's performance is to normalize the input data.
from sklearn.preprocessing import Normalizer
normalizer = Normalizer()

# Here, we compose our scikit-learn pipeline. 
# First, we apply our normalization. 
# Then we feed the normalized data into the linear model.
from sklearn.pipeline import make_pipeline
pipeline = make_pipeline(normalizer, linear_svc)
pipeline.fit(X, y)

# Now, we convert the scikit-learn pipeline into ONNX format. 
# Compared to the previous example, notice that the specified feature dimension becomes 3.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from winmltools import convert_sklearn
pipeline_onnx = convert_sklearn(linear_svc, name='NormalizerLinearSVC', input_features=[('input', 'float', 3)])   

# We can print the fresh ONNX model.
print(pipeline_onnx)

# We can also save the ONNX model into binary file for later use.
from winmltools.utils import save_model
save_model(pipeline_onnx, 'pipeline.onnx')

# Our conversion framework provides limited support of heterogeneous feature values.
# We cannot have numerical types and string type in one feature vector. 
# Let's assume that the first two features are floats and the last feature is integer (encoded a categorical attribute).
X_heter = [[0.5, 1., 300], [-1., -1.5, 400], [0., -2., 100]]

# One popular way to represent categorical is one-hot encoding.
from sklearn.preprocessing import OneHotEncoder
one_hot_encoder = OneHotEncoder(categorical_features=[2])

# Let's initialize a classifier. 
# It will be right after the one-hot encoder in our pipeline.
linear_svc = LinearSVC()

# Then, we form a two-stage pipeline.
another_pipeline = make_pipeline(one_hot_encoder, linear_svc)
another_pipeline.fit(X_heter, y)

# Now, we convert, save, and load the converted model. 
# For heterogeneous feature vectors, we need to seperately specify their types for all homogeneous segments.
# The automatic code generator (mlgen) uses the name parameter to generate class names.
another_pipeline_onnx = convert_sklearn(another_pipeline, name='OneHotLinearSVC', input_features=[('input', 'float', 2), ('another_input', 'int64', 1)])
save_model(another_pipeline_onnx, 'another_pipeline.onnx')
from winmltools.utils import load_model
loaded_onnx_model = load_model('another_pipeline.onnx')

# Of course, we can print the ONNX model to see if anything went wrong.
print(another_pipeline_onnx)
~~~

## <a name="core-ml-models"></a>Core ML 模型

我們假設範例 Core ML 模型檔案的路徑是 *example.mlmodel*。

~~~python
from coremltools.models.utils import load_spec
# Load model file
model_coreml = load_spec('example.mlmodel')
from winmltools import convert_coreml
# Convert it!
# The automatic code generator (mlgen) uses the name parameter to generate class names.
model_onnx = convert_coreml(model_coreml, name='ExampleModel')   
~~~

`model_onnx` 是 ONNX [ModelProto](https://github.com/onnx/onnxmltools/blob/0f453c3f375c1ae928b83a4c7909c82c013a5bff/onnxmltools/proto/onnx-ml.proto#L176) 物件。 我們可以將它儲存為兩種不同格式。

~~~python
from winmltools.utils import save_model
# Save the produced ONNX model in binary format
save_model(model_onnx, 'example.onnx')
# Save the produced ONNX model in text format
from winmltools.utils import save_text
save_text(model_onnx, 'example.txt')
~~~

**注意**：CoreMLTools 是 Apple 提供的 Python 套件，但在 Windows 上不可用。 如果您需要在 Windows 上安裝套件，請直接從存放庫安裝套件：

```
pip install git+https://github.com/apple/coremltools
```

## <a name="core-ml-models-with-image-inputs-or-outputs"></a>具有影像輸入或輸出的 Core ML 模型

由於在 ONNX 中缺少影像類型，轉換 Core ML 影像模型 (即，使用影像作為輸入或輸出的模型。) 需要一些前置處理和後續處理步驟。

前置處理的目標是確保輸入影像能正確的格式化為 ONNX 張量。 假設 *X* 是 Core ML 中圖形為 [C, H, W] 的影像輸入。 在 ONNX 中，變數 *X* 將是具有相同圖形的浮點張量，而 *X*[0, :, :]/*X*[1, :, :]/*X*[2, :, :] 儲存影像的紅色/綠色/藍色頻道。 在 Core ML 中的灰階影像，其格式是 ONNX 中的 [1, H, W]-張量，因為我們只會有一個頻道。

如果原始 Core ML 模型輸出影像，則以手動方式將 ONNX 的浮點輸出張量轉換成影像。 有兩個基本步驟。 第一個步驟是截斷大於 255 到 255 的值，並將所有負值變更為 0。 第二個步驟是將所有像素值四捨五入至整數 (透過加上 0.5，然後截斷小數點)。 在 Core ML 模型中指定輸出頻道順序 (例如 RGB 或 BGR)。 若要產生適當的影像輸出，我們需要轉置或隨機播放以恢復所需的格式。

我們認為從 [GitHub](https://github.com/likedan/Awesome-CoreML-Models) 下載的 Core ML 模型、FNS-Candy，可作為示範 ONNX 和 Core ML 格式之間差異的具體轉換範例。 請注意，所有後續命令都是在 Python 環境中執行的。

首先，我們載入 Core ML 模型並檢查其輸入和輸出格式。

~~~python
from coremltools.models.utils import load_spec
model_coreml = load_spec('FNS-Candy.mlmodel')
model_coreml.description # Print the content of Core ML model description
~~~

螢幕輸出：

~~~
...
input {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
output {
    ...
      imageType {
      width: 720
      height: 720
      colorSpace: BGR
    ...
}
...
~~~

此時，輸入和輸出都是 720 x 720 BGR 影像。 接下來就是要將 Core ML 模型與 WinMLTools 進行轉換。

~~~python
# The automatic code generator (mlgen) uses the name parameter to generate class names.
from onnxmltools import convert_coreml
model_onnx = convert_coreml(model_coreml, name='FNSCandy')    
~~~

在 ONNX 中檢視模型輸入和輸出格式的另一種方法是使用下列命令：

~~~python
model_onnx.graph.input # Print out the ONNX input tensor's information
~~~

螢幕輸出：

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

在 ONNX 中產生的輸入 (以 *X*表示) 是一個 4-D 張量。 最後 3 個軸分別是 C-軸、H-軸 和 W-軸。 第一個維度是「無」，表示這個 ONNX 模型可以套用到任意數量的影像。 要套用此模型處理一批 2 張影像，第一張影像對應至 *X*[0, :, :, :]，而 *X*[1, :, :, :] 對應至第二張影像。 第一張影像的藍色/綠色/紅色頻道為 *X*[0, 0, :, :]/*X*[0, 1, :, :]/*X*[0, 2, :, :]，而第二張影像的頻道類似。

~~~python
model_onnx.graph.output # Print out the ONNX output tensor's information
~~~

螢幕輸出：

~~~
...
  tensor_type {
    elem_type: FLOAT
    shape {
      dim {
        dim_param: "None"
      }
      dim {
        dim_value: 3
      }
      dim {
        dim_value: 720
      }
      dim {
        dim_value: 720
      }
    }
  }
...
~~~

您可以看到產生的格式與原始模型輸入格式相同。 不過，在這種情況下，它不是影像，因為像素值是整數，而不是浮點數。 要還原成影像，請將大於 255 到 255 的值截斷，將負值變更為 0，並將所有小數四捨五入為整數。