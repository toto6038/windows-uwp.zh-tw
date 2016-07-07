---
author: payzer
title: "如何停用滑鼠模式"
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: bd7780f105f86d7d292c5db2535ad40af07d6e4f

---

# 如何停用滑鼠模式
滑鼠模式現在針對所有應用程式預設為開啟。 這代表所有未選擇不使用的應用程式都將會看到滑鼠指標 (類似於主機上 Edge 瀏覽器的指標)。 強烈建議您關閉此功能，並針對方向控制器瀏覽最佳化。   
   
## HTML   
若要關閉滑鼠模式，請將下列項目加入到應用程式中的 JavaScript 檔案：   
   
```code
navigator.gamepadInputEmulation = "keyboard";
```   

## XAML    
若要關閉滑鼠模式，請將下列項目加入到應用程式中的 JavaScript 檔案：   
   
```code
public App() {
        this.InitializeComponent();
        this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
        this.Suspending += OnSuspending;
        }
```

## C++/DirectX   
如果您正在撰寫 C++/DirectX app，則不須執行任何動作。 滑鼠模式僅適用於 HTML 和 XAML 應用程式。



<!--HONumber=Jun16_HO5-->


