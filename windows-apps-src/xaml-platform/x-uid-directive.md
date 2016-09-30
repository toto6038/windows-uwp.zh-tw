---
author: jwmsft
description: "提供標記元素的唯一識別碼。 對通用 Windows 平台 (UWP) XAML 來說，XAML 當地語系化處理程序和工具會使用這個唯一識別碼，像是使用來自 .resw 資源檔的資源。"
title: "x&#58;Uid 指示詞"
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4f8aa553c99b6071cedc4f9d93cf8258b75eca49

---

# x&#58;Uid 指示詞

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

提供標記元素的唯一識別碼。 對通用 Windows 平台 (UWP) XAML 來說，XAML 當地語系化處理程序和工具會使用這個唯一識別碼，像是使用來自 .resw 資源檔的資源。

## XAML 屬性用法

``` syntax
<object x:Uid="stringID".../>
```

## XAML 值

| 詞彙 | 說明 |
|------|-------------|
| stringID | 這是可以在應用程式中唯一識別 XAML 元素的字串，並且會成為資源檔中資源路徑的一部分。 請參閱＜備註＞。| 

## 備註

請使用 **x:Uid** 來識別您 XAML 中的物件元素。 一般而言，這個物件元素是控制項類別或 UI 中顯示的其他元素的執行個體。 您在 **x:Uid** 中使用的字串與在資源檔中使用的字串之間的關係是，資源檔字串是 **x:Uid** 後面跟著一個 (.)，然後再跟著被當地語系化的元素的特定屬性名稱。 請參考下列範例：

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

若要指定內容來取代顯示文字 [**執行**]，您必須指定一個來自資源檔的新資源。 您的資源檔應該包含一個名稱為 "GoButton.Content" 的資源項目。 在這種情況下，[**Content**](https://msdn.microsoft.com/library/windows/apps/br209366) 會是 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 類別所繼承的特定屬性。 您也可以為這個按鈕的其他屬性提供當地語系化值，例如您可以為 "GoButton.FlowDirection" 提供一個以資源為基礎的值。 如需如何將 **x:Uid** 與資源檔搭配使用的詳細資訊，請參閱[快速入門：翻譯 UI 資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329)。

要使用哪些字串做為 **x:Uid** 值才有效，實際上取決於哪些字串在資源檔和資源路徑中可以做為合法識別碼。

因為指定的 XAML 當地語系化案例將 **x:Uid** 與 **x:Name** 分開使用，因此，用於當地語系化的識別碼與 **x:Name** 的程式撰寫模型含意沒有相依性。 此外，**x:Name** 是由 XAML 命名範圍概念所規範，而 **x:Uid** 的唯一性則是由封裝資源索引 (PRI) 系統所控制。 如需詳細資訊，請參閱[資源管理系統](https://msdn.microsoft.com/library/windows/apps/jj552947)。

UWP XAML 的 **x:Uid** 唯一性，和之前使用的 XAML-utilizing 技術有些不同的規則。 對於 UWP XAML 來說，同樣的 **x:Uid** 識別碼值可以做為多個 XAML 元素的指示詞。 不過，在解析資源檔中的資源時，每個這類元素都必須共用相同的解析邏輯。 而且，一個專案中的所有 XAML 檔案會共用單一資源範圍以進行 **x:Uid** 解析，沒有將 **x:Uid** 範圍對齊到個別 XAML 檔案的概念。

在某些情況下，您將會使用資源路徑，而不是封裝資源索引 (PRI) 系統的內建功能。 任何做為 **x:Uid** 值的字串都會定義一個開頭是 ms-resource:///Resources/ 並且包含 **x:Uid** 字串的資源路徑。 這個路徑結尾會是您在資源檔中指定的屬性的名稱，或是做為目標的屬性的名稱。

請勿將 **x:Uid** 放在屬性元素上，在 Windows 執行階段 XAML 中並不允許這樣做。




<!--HONumber=Jun16_HO4-->


