---
title: 如何停用滑鼠模式
description: 停用預設滑鼠模式的指示。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e57ee4e6-7807-4943-a933-c2b4dc80fc01
ms.localizationpriority: medium
ms.openlocfilehash: 1e4b8868f416494daf978d65d4a4ccde02d6ccf5
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740609"
---
# <a name="how-to-disable-mouse-mode"></a>如何停用滑鼠模式
滑鼠模式針對所有應用程式預設皆為開啟，這代表所有未選擇不使用的應用程式都將會看到滑鼠指標 (類似於主機上 Edge 瀏覽器的指標)。 我們強烈建議您關閉此功能，並針對方向控制器瀏覽進行最佳化。   
   
## <a name="html"></a>HTML   
若要在 JavaScript 通用 Windows 平台 (UWP) App 中開啟方向控制器瀏覽，請使用 [TVHelpers 方向瀏覽](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation) JavaScript 程式庫。 在您的應用程式套件中包含方向瀏覽 JavaScript 檔案，並在所有需要方向控制器瀏覽的 HTML 頁面中新增該檔案的參考：

```code
<script src="directionalnavigation-1.0.0.0.js"></script>
```
如需詳細資料，請參閱[方向瀏覽 wiki](https://github.com/Microsoft/TVHelpers/wiki/Using-DirectionalNavigation)。

如果您想要關閉滑鼠模式，並改為直接使用 DOM 或 WinRT 控制器 API，請針對每個需要的頁面執行下列項目： 
   
```code
navigator.gamepadInputEmulation = "gamepad";
```   

   這個屬性預設為 `mouse`，並會啟用滑鼠模式。 將它設定為 `keyboard` 會關閉滑鼠模式，而控制器輸入將會改為產生 DOM 鍵盤事件。 將它設定為 `gamepad` 會關閉滑鼠模式，並且不會產生 DOM 鍵盤事件，允許您僅使用 DOM 或 WinRT 控制器 API。

## <a name="xaml"></a>XAML    
若要關閉滑鼠模式，在您的建構函式後加上下列項目：   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## <a name="cdirectx"></a>C++/DirectX   
如果您正在撰寫 C++/DirectX app，則不須執行任何動作。 滑鼠模式僅適用於 HTML 和 XAML 應用程式。

## <a name="see-also"></a>另請參閱
- [Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)

