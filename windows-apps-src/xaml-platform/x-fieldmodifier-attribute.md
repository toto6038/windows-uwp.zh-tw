---
author: jwmsft
description: 修改 XAML 編譯行為，以 public 存取而不是 private 預設行為來定義具名物件參考的欄位。
title: xFieldModifier 屬性
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
---

# x&#58;FieldModifier 屬性

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

修改 XAML 編譯行為，以 **public** 存取而不是 **private** 預設行為來定義具名物件參考的欄位。

## XAML 屬性用法

``` syntax
<object x:FieldModifier="public".../>
```

## 相依性

相同元素上也必須提供 [x:Name 屬性](x-name-attribute.md)。

## 備註

**x:FieldModifier** 屬性的值會依程式設計語言而有所不同。 要使用的字串取決於每種語言實作其 **CodeDomProvider** 的方式，以及它所傳回用於定義 **TypeAttributes.Public** 和 **TypeAttributes.NotPublic** 意義的類型轉換器。 以 C#、Microsoft Visual Basic 或 Visual C++ 元件延伸 (C++/CX) 來說，您可以提供字串值 "public" 或 "Public"；剖析器對這個屬性值並沒有強制大小寫。

您也可以指定 **NonPublic** (在 C# 或 C++/CX 中為 **internal**，在 Visual Basic 中為 **Friend**)，但這並不常用。 內部存取並不適用於 Windows 執行階段 XAML 程式碼產生模型。 私用存取是預設值。

**x:FieldModifier** 只與具有 [x:Name 屬性](x-name-attribute.md)的元素相關，因為該名稱會在欄位是公用時用來參考該欄位。

**注意** Windows 執行階段 XAML 不支援 **x:ClassModifier** 或 **x:Subclass**。



<!--HONumber=May16_HO2-->


