---
description: 列出 Windows 執行階段的 XAML 中，針對 Common Language Runtime (CLR) 及其他程式設計語言 (例如 C++) 中特定資料類型的語言層級支援。
title: XAML 內建資料類型
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dd8bd8f80be068af92d679795431cf9a938fd5f6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372948"
---
# <a name="xaml-intrinsic-data-types"></a>XAML 內建資料類型


Windows 執行階段的 XAML 提供數種資料類型的語言層級支援，這些資料類型是 Common Language Runtime (CLR) 及其他程式設計語言 (例如 C++) 中常用的基本類型。

您最常見到 XAML 內建資料類型的使用位置是在 XAML 資源字典中定義資源。 您可能在該處定義常數，例如用於多個值的數字。 或是您可能使用透過字串或布林值處理動畫的腳本動畫，而您將需要一個 XAML 物件元素，用來代表填入 [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 定義之主要畫面格的字串或布林值。 Windows 執行階段預設 XAML 範本會同時使用這些技術。

Windows 執行階段的 XAML 提供下列類型的語言層級支援。

| XAML 基本類型 | 描述 |
|-------|-------------|
| **x:Boolean**  | 以 CLR 支援來說，會對應到 [**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean?redirectedfrom=MSDN)。 XAML 剖析 **x:Boolean** 的值時不區分大小寫。 請注意，"x:Bool" 不是可以接受的替代用法。 |
| **x:String**   | 以 CLR 支援來說，會對應到 [**String**](https://docs.microsoft.com/dotnet/api/system.string?redirectedfrom=MSDN)。 字串的編碼會預設為周圍 XML 編碼。 |
| **x:Double**   | 以 CLR 支援來說，會對應到 [**Double**](https://docs.microsoft.com/dotnet/api/system.double?redirectedfrom=MSDN)。 除了數值外，**x:Double** 的文字語法也允許語彙基元 "NaN"，這是配置行為的 "Auto" 如何儲存為資源值的方式。 處理語彙基元時會區分大小寫。 您可以使用科學記號標記法，例如 "1+E06" 代表 `1,000,000`。 |
| **x:Int32**    | 以 CLR 支援來說，會對應到 [**Int32**](https://docs.microsoft.com/dotnet/api/system.int32?redirectedfrom=MSDN)。 **x:Int32** 會視為已簽署，您可以包含減號 ("-") 符號以表示負數。 在 XAML 中，文字語法中沒有加號表示為正數的已簽署值。 |

通常您只有針對這些 XAML 語言基本類型，才會在 XAML 中定義使用 **x:** 前置詞的物件元素。 其他所有的 XAML 語言功能通常使用在屬性表單中，或是做為標記延伸。

**附註**  依照慣例，語言基本類型，以 XAML 和所有其他 XAML 語言項目會顯示 「 x 」 前置詞。 這就是 XAML 語言元素在真實世界標記中的一般用法。 XAML 的文件與 XAML 規格會依循這個慣例。

## <a name="other-xaml-primitives"></a>其他 XAML 基本類型

XAML 2009 規格有提到其他 XAML 語言層級的基本類型，如 **x:Uri** 和 **x:Single**。 除非在本主題的表格中另行列出，否則 Windows 執行階段的 XAML 目前不支援由其他 XAML 詞彙或 XAML 2009 規格所定義的其他 XAML 語言基本類型。

**附註**  日期和時間 (使用屬性[ **DateTime** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.DateTime)或是[ **DateTimeOffset**](https://docs.microsoft.com/dotnet/api/system.datetimeoffset?redirectedfrom=MSDN)， [**TimeSpan** ](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan)或是[ **System.TimeSpan**](https://docs.microsoft.com/dotnet/api/system.timespan?redirectedfrom=MSDN)) 不是以 XAML 基本可設定。 因為在 Windows 執行階段 XAML 剖析器中沒有適用於日期和時間的預設 from-string 轉換行為，所以這些屬性在 XAML 中通常完全無法設定。 若要初始化任何日期和時間屬性的值，您必須使用在頁面或元素載入時執行的程式碼後置。

## <a name="related-topics"></a>相關主題

* [XAML 概觀](xaml-overview.md)
* [XAML 語法指南](xaml-syntax-guide.md)
* [建立圖片敘述的動畫](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
 

