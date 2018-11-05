---
author: stevewhims
Description: Use the Windows.Globalization.DateTimeFormatting API with custom templates and patterns to display dates and times in exactly the format you wish.
title: 使用模式來設定日期和時間的格式
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
label: Use patterns to format dates and times
template: detail.hbs
ms.author: stwhi
ms.date: 11/09/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 04a0288d0b28c12eb68cf56225747224e8df9777
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6024717"
---
# <a name="use-templates-and-patterns-to-format-dates-and-times"></a>使用範本和模式來設定日期和時間的格式

使用 [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 命名空間中的類別搭配自訂範本和模式，以您想要的格式來顯示日期和時間。

## <a name="introduction"></a>簡介

[**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 類別提供不同的方式來適當地設定世界各地語言和地區的日期和時間格式。 您可以使用年、月、日等等的標準格式。 或者您也可以將格式範本傳遞到 **DateTimeFormatter** 建構函式的 *formatTemplate* 引數，例如 "longdate" 或 "month day"。

但當您想要對所希望顯示之 [**DateTime**](/uwp/api/windows.foundation.datetime?branch=live) 物件的元件順序與格式有更多控制時，您可以將格式模式傳遞到建構函式的 *formatTemplate* 引數。 格式模式使用一種特殊的語法，可讓您取得 **DateTime** 物件的個別元件&mdash;例如，單純月份的名稱，或年份的值&mdash;以使用任何您選擇的自訂格式顯示他們。 此外，模式也可以當地語系化以配合其他語言和地區。

**注意：** 這是只有格式模式的概觀。 如需格式範本和格式模式更完整的討論，請參閱 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 類別的＜備註＞一節。

## <a name="the-difference-between-format-templates-and-format-patterns"></a>格式範本和格式模式的不同

格式範本是與文化無關的格式字串。 因此，若您使用格式範本建構 **DateTimeFormatter**，則格式器會以目前語言的正確順序顯示您的格式化元件。 相反的，格式模式則是文化限定的。 若您使用格式模式建構 **DateTimeFormatter**，則格式器會直接使用指定的模式。 因此，模式不見得在每個文化中都是有效的。

讓我們用範例說明這項差異。 我們會將一個簡單的格式範本 (並非模式) 傳遞給 **DateTimeFormatter** 建構函式。 這是格式範本 "month day"。

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

這會根據目前內容的語言與地區值來建立格式子。 格式範本中的元件順序不重要，因為格式器會使用目前語言的正確順序顯示他們。 因此，它會為英文 (美國) 顯示「January 1」，為法文 (法國) 顯示「1 janvier」，為日文顯示「1月1日」。

另一方面，格式模式則是文化限定的。 讓我們存取格式範本的格式模式。

```csharp
IReadOnlyList<string> monthDayPatterns = dateFormatter.Patterns;
```

這會根據執行階段語言和地區產生不同的結果。 不同的地區會使用不同元件、不同順序，帶有或不帶有額外的字元和間距。

```syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

在上述範例中，我們輸入無關文化的格式字串，並取得文化限定的格式字串 (即為我們呼叫 `dateFormatter.Patterns` 時發揮作用的語言和地區函式)。 因此，若您使用文化限定的格式模式建構 **DateTimeFormatter**，則它便只會在特定語言/地區中有效。

```csharp
var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

上述格式器會傳回在括號內的個別元件的文化特性值{}。 但格式模式中的元件順序則不變。 您得到的就是您想要的，而這可能不見得適用每個文化： 此格式器針對英文 (美國) 有效，但針對法文 (法國) 和日文則無效。

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol 日 is missing)
```

此外，目前正確的模式在未來可能不見得正確。 各國家或地區可能會改變他們的日曆系統，因此也會改變格式範本。 Windows 會根據格式範本更新格式器的輸出，以因應這類變更。 因此，建議您只在下列一或多個條件下使用模式語法。

-   您不倚賴特定的輸出來設定格式。
-   您不需要讓格式遵守某種文化特定標準。
-   您蓄意讓模式在各個文化特性都保持不變。
-   您意圖當地語系化實際的格式模式字串。

以下是區分格式範本和格式模式的摘要。

**格式範本，例如 "month day"。**

-   [DateTime](/uwp/api/windows.foundation.datetime?branch=live) 格式的抽象表示，包含任何順序的月份值和日期值等。
-   確定可以為 Windows 支援的所有語言-地區值傳回有效的標準格式。
-   確定可以為指定的語言-地區提供文化上適當的格式化字串。
-   並非所有元件組合都是有效的。 例如，"dayofweek day" 是無效的組合。

**格式模式，例如 "{month.full} {day.integer}"。**

-   有明確順序的字串，以完整月份名稱後面加上空格，空格後面再加上日期整數的順序表示，或任何您指定的特定格式模式。
-   可能無法對應任何語言-地區組的有效標準格式。
-   不保證文化上適當。
-   可以指定任何元件組合，且沒有一定的順序。

## <a name="examples"></a>範例

假設您想要以特定格式顯示目前時間以及目前的月份和日期。 例如，您想要讓美國英文使用者看到類似下列的格式：

``` syntax
June 25 | 1:38 PM
```

日期部分會對應 "month day" 格式範本，而時間部分則會對應 "hour minute" 格式範本。 因此，您可以為相關日期和時間的格式範本建構格式器，然後使用可當地語系化的格式字串結合他們的輸出。

```csharp
var dateToFormat = System.DateTime.Now;
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

var date = dateFormatter.Format(dateToFormat);
var time = timeFormatter.Format(dateToFormat);

string output = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), date, time);
```

`CustomDateTimeFormatString` 是指向資源檔 (.resw) 中可當地語系化資源的資源識別碼。 預設語言為英文 （美國），這會設為值為 「{0} |{1}「 以及註解指出 」{0}」 是日期和 「{1}」 是時間。 如此一來，翻譯人員便可以視需要調整格式項目。 例如，如果在某些語言或地區中，時間在日期之前是較自然的樣式，他們就可以變更項目的順序。 或者，他們可以使用某種其他分隔字元來取代 "|"。

另一種實作此範例的方法便是查詢兩個格式器以取得其格式模式，將他們結合在一起，然後使用結合後的格式模式建構第三個格式器。

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();

var dateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var timeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

string dateFormatterPattern = dateFormatter.Patterns[0];
string timeFormatterPattern = timeFormatter.Patterns[0];

string pattern = string.Format(resourceLoader.GetString("CustomDateTimeFormatString"), dateFormatterPattern, timeFormatterPattern);

var patternFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(pattern);

string output = patternFormatter.Format(System.DateTime.Now);
```

## <a name="important-apis"></a>重要 API

* [Windows.Globalization.DateTimeFormatting](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [DateTime](/uwp/api/windows.foundation.datetime?branch=live)

## <a name="related-topics"></a>相關主題

* [日期和時間格式化範例](http://go.microsoft.com/fwlink/p/?LinkId=231618)