---
author: payzer
title: "如何關閉縮放比例"
description: "關閉預設縮放比例的指示。"
translationtype: Human Translation
ms.sourcegitcommit: 582f5677c15f7cd62c398103b48743ba4bea6c5b
ms.openlocfilehash: 8079be9685558277565766fa8d0ebbfd4a555904

---

# 如何關閉縮放比例   
根據預設，若為 XAML，應用程式會調整為 200%，若為 HTML app 則為 150%。 您可以關閉預設縮放比例。 這會導致應用程式使用裝置的實際像素尺寸 (1910 x 1080 像素)。   
   
## HTML   
您可以使用下列程式碼片段，選擇不使用縮放比例︰ 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

或者，您可以使用 Web 適用的方法：   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## XAML
您可以使用下列程式碼片段，選擇不使用縮放比例︰   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## DirectX/C++   
DirectX/C++ 應用程式不會進行縮放。 自動縮放僅適用於 HTML 與 XAML 應用程式。  

## 另請參閱
- [Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)



<!--HONumber=Aug16_HO3-->


