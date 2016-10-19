---
author: payzer
title: "如何停用滑鼠模式"
description: "停用預設滑鼠模式的指示。"
translationtype: Human Translation
ms.sourcegitcommit: b4df1f944d909640791e4ed7e3bcf8d8bdf7a0d1
ms.openlocfilehash: 91e530a3313d53c4e693b88a64b849f3188a72de

---

# 如何停用滑鼠模式
滑鼠模式針對所有應用程式預設皆為開啟，這代表所有未選擇不使用的應用程式都將會看到滑鼠指標 (類似於主機上 Edge 瀏覽器的指標)。 我們強烈建議您關閉此功能，並針對方向控制器瀏覽進行最佳化。   
   
## HTML   
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

## XAML    
若要關閉滑鼠模式，在您的建構函式後加上下列項目：   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
}
```

## C++/DirectX   
如果您正在撰寫 C++/DirectX app，則不須執行任何動作。 滑鼠模式僅適用於 HTML 和 XAML 應用程式。

## 另請參閱
- [Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)




<!--HONumber=Aug16_HO3-->


