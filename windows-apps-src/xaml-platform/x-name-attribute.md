---
author: jwmsft
description: "唯一識別用於從程式碼後置或一般程式碼中存取具現化物件的物件元素。"
title: "xName 屬性"
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
translationtype: Human Translation
ms.sourcegitcommit: ebda34ce4d9483ea72dec3bf620de41c98d7a9aa
ms.openlocfilehash: 1a70bffd6e6990ece4565b919846503b95ae8f61

---

# x:Name 屬性

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

唯一識別用於從程式碼後置或一般程式碼中存取具現化物件的物件元素。 一旦套用到支援程式撰寫模型，在建構函示傳回時，**x:Name** 可以視為等同於含有物件參考的變數。

## XAML 屬性用法

``` syntax
<object x:Name="XAMLNameValue".../>
```

## XAML 值

| 詞彙 | 說明 |
|------|-------------|
| XAMLNameValue | 符合 XamlName 文法限制的字串。 |

##  XamlName 文法

下列是字串的基準文法，該字串在這個 XAML 實作中被當做是一個金鑰：

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   字元限制在低 ASCII 範圍，更具體地說，僅限羅馬字母大寫與小寫字元、數字以及底線 (\_) 字元。
-   不支援 Unicode 字元範圍。
-   名稱開頭不可以是數字。 如果使用者提供數字做為初始字元，則某些工具實作會在字串前面加上底線 (\_)，或者工具會依據包含數字的其他值自動產生 **x:Name** 值。

## 備註

在處理 XAML 時，指定的 **x:Name** 會成為在基礎程式碼中建立的欄位名稱，並且該欄位含有物件參考。 建立此欄位的處理程序是由 MSBuild 目標步驟執行的，它們也負責加入 XAML 檔案的部分類別和其程式碼後置。 這個行為不一定是 XAML 語言指定的，它是 XAML 的通用 Windows 平台 (UWP) 程式撰寫為了在其程式撰寫模型或應用程式模型中使用 **x:Name** 所套用的特定實作。

每個定義的 **x:Name** 在 XAML 命名範圍內都必須是唯一的。 一般而言，XAML 命名範圍是定義在載入的頁面的根元素層級，並且包含單一 XAML 頁面該元素底下的所有元素。 其他 XAML 命名範圍是由在該頁面上定義的任何控制項範本或資料範本所定義。 在執行階段，會針對從套用的控制項範本建立的物件樹根目錄建立另一個 XAML 命名範圍，也會由透過呼叫 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 所建立的物件樹建立另一個 XAML 命名範圍。 如需詳細資訊，請參閱 [XAML 命名範圍](xaml-namescopes.md)。

當元素被引入到設計介面時，設計工具通常會自動為元素產生 **x:Name** 值。 自動產生配置會依據您使用的設計工具而不同，但典型的配置是產生一個以支援元素的類別名稱開頭的字串，後面則是一個遞增的整數。 例如，如果您將第一個 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 元素引入設計工具，您會發現在 XAML 中，這個元素含有 "Button1" 的 **x:Name** 屬性值。

**x:Name** 不能設定在 XAML 屬性元素語法中，或是使用 [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) 的程式碼中。 **x:Name** 只能使用 XAML 屬性語法設定在元素中。

**注意** 特別是 C++/CX app，**x:Name** 參考的支援欄位不是為 XAML 檔案或頁面的根元素建立的。 如果您需要從 C++ 程式碼後置參考根物件，請使用其他 API 或樹狀目錄周遊。 例如，您可以呼叫 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) 以取得已知名稱的子元素，然後再呼叫 [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739)。

### x:Name 和其他 Name 屬性

UWP XAML 中使用的一些類型也具有名為 **Name** 的屬性。 例如，[**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) 和 [**TextElement.Name**](https://msdn.microsoft.com/library/windows/apps/hh702125)。

如果 **Name** 可做為元素上的可設定屬性，則在 XAML 中就可以交替使用 **Name** 和 **x:Name**，但如果在同一個元素上同時指定這兩個屬性，就會發生錯誤。 也有情況是有 **Name** 屬性，但是為唯讀屬性 (例如 [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031))。 如果是這種情況，請一律使用 **x:Name** 在 XAML 中命名該元素，而唯讀 **Name** 則是用於一些較不常見的程式碼案例。

**注意** 一般而言，不應該使用 [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) 來變更原先由 **x:Name** 所設定的值，雖然有些案例是這項一般規則的例外。 在典型的案例中，建立與定義 XAML 命名範圍屬於 XAML 處理器作業。 在執行階段修改 **FrameworkElement.Name** 會導致 XAML 命名範圍/私用欄位命名無法對齊，這會讓您的程式碼後置難以記錄。

### x:Name 和 x:Key

**x:Name** 可以當作屬性套用到 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 內的元素，用來替代 [x:Key 屬性](x-key-attribute.md)。 (**ResourceDictionary** 中的所有元素都必須具有 x:Key 或 x:Name 屬性，是一項常規)。這通用於[腳本動畫](https://msdn.microsoft.com/library/windows/apps/mt187354)。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/mt187273)一節。




<!--HONumber=Aug16_HO3-->


