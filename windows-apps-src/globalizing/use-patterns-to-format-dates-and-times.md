---
使用 Windows.Globalization.DateTimeFormatting API 搭配自訂模式，以您想要的格式來顯示日期和時間。
使用模式來設定日期和時間的格式
ms.assetid: 012028B3-9DA2-4E72-8C0E-3E06BEC3B3FE
使用模式來設定日期和時間的格式
template: detail.hbs
---

# 使用模式來設定日期和時間的格式


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)
-   [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)

使用 [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) API 搭配自訂模式，以您想要的格式來顯示日期和時間。

## <span id="Introduction"> </span> <span id="introduction"> </span> <span id="INTRODUCTION"> </span>簡介


[
            **Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859) 提供不同的方式來適當地設定世界各地語言和地區的日期和時間格式。 您可以針對年、月、日等使用標準格式，或是使用標準字串範本，例如 "longdate" 或 "month day"。

但是當您想要對所希望顯示之 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 字串的組成要素順序與格式有更多控制時，您可以針對字串範本參數使用特殊語法，稱為「模式」。 模式語法可讓您取得 **DateTime** 物件的個別要素 — 例如，只有月份名稱或只有年份值 — 以便使用您選擇的自訂格式來顯示日期和時間。 此外，模式也可以當地語系化以配合其他語言和地區。

**注意**這是格式模式的概觀。 如需格式範本和格式模式更完整的討論，請參閱 [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828) 類別的＜備註＞一節。

 

## <span id="What_you_need_to_know"> </span> <span id="what_you_need_to_know"> </span> <span id="WHAT_YOU_NEED_TO_KNOW"> </span>您需要知道的事項


在使用模式時要特別注意，您建立的自訂格式不保證在所有文化中都是有效的。 例如，以 "month day" 範本為例：

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
```

這會根據目前內容的語言與地區值來建立格式子。 因此，一律會以適當的全球格式一起顯示月與日。 例如，英文 (美國) 會顯示「January 1」、法文 (法國) 會顯示「1 janvier」，日文會顯示「1月1日」。 這是因為此範本是以文化特定模式字串為依據，此字串可以透過模式屬性來存取：

**C#**
```CSharp
var monthdaypattern = datefmt.Patterns;
```
**JavaScript**
```JavaScript
var monthdaypattern = datefmt.patterns;
```

這會根據格式子的語言和地區產生不同的結果。 請注意，不同地區可能會使用不同的組成要素、不同的順序，以及包含 (或不含) 其他字元和間距：

``` syntax
En-US: "{month.full} {day.integer}"
Fr-FR: "{day.integer} {month.full}"
Ja-JP: "{month.integer}月{day.integer}日"
```

您可以使用模式來建構自訂的 [**DateTimeFormatter**](https://msdn.microsoft.com/library/windows/apps/br206828)，例如以下是以美國英文模式為依據：

**C#**
```CSharp
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```
**JavaScript**
```JavaScript
var datefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("{month.full} {day.integer}");
```

Windows 會針對括弧 {} 內部的個別要素傳回文化特定值。 但若使用模式語法，要素順序不會因文化特性而異。 您得到的就是您想要的，而這可能不見得適用每個文化：

``` syntax
En-US: January 1
Fr-FR: janvier 1 (inappropriate for France; non-standard order)
Ja-JP: 1月1 (inappropriate for Japan; the day symbol is missing)
```

此外，模式不保證會一直保持不變。 各國家或地區可能會改變他們的行事曆系統，因此也會改變格式範本。 Windows 會更新格式子的輸出，以因應這類變更。 因此，您應該只在下列情況下，才使用模式語法來設定 [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 的格式：

-   您不倚賴特定的輸出來設定格式。
-   您不需要讓格式遵守某種文化特定標準。
-   您蓄意讓模式在各個文化特性都保持不變。
-   您想要將模式當地語系化

總結標準字串範本與非標準字串範本之間的差異：

**字串範本，例如 "month day"。**

-   [
            **DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) 格式的抽象表示包含某種順序的月份值和日期值。
-   確定可以為 Windows 支援的所有語言-地區值傳回有效的標準格式。
-   確定可以為指定的語言-地區提供文化上適當的格式化字串。
-   並非所有要素組合都是有效的。 例如，沒有可用於 "dayofweek day" 的字串範本。

**字串模式，例如 "{month.full} {day.integer}"。**

-   有明確順序的字串，以完整月份名稱後面加上空格，空格後面再加上日期整數的順序表示。
-   可能無法對應任何語言-地區組的有效標準格式。
-   不保證文化上適當。
-   可以指定任何組成要素組合，且沒有一定的順序。

## <span id="Tasks"> </span> <span id="tasks"> </span> <span id="TASKS"> </span>工作


假設您想要以特定格式顯示目前時間以及目前的月份和日期。 例如，您想要讓美國英文使用者看到類似下列的格式：

``` syntax
June 25 | 1:38 PM
```

日期部分會對應 "month day" 範本，而時間部分則會對應 "hour minute" 範本。 因此，您可以建立串連組成那些範本之模式的自訂格式。

首先，取得相關日期和時間範本的格式子，然後取得這些範本的模式：

**C#**
```CSharp
// Get formatters for the date part and the time part.
var mydate = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("month day");
var mytime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.Patterns[0];
var mytimepattern = mytime.Patterns[0];
```
**JavaScript**
```JavaScript
// Get formatters for the date part and the time part.
var dtf = Windows.Globalization.DateTimeFormatting;
var mydate = dtf.DateTimeFormatter("month day");
var mytime = dtf.DateTimeFormatter("hour minute");

// Get the patterns from these formatters.
var mydatepattern = mydate.patterns[0];
var mytimepattern = mytime.patterns[0];
```

您應將自訂格式儲存為可當地語系化的資源字串。 例如，英文 (美國) 字串會是 "{date} | {time}"。 當地語系化人員可視需要調整此字串。 例如，如果在某些語言或地區中，時間在日期之前是較自然的樣式，他們就可以變更要素的順序。 或者，他們可以使用某種其他分隔字元來取代 "|"。 在執行階段，您可以使用相關模式來取代字串的 {date} 和 {time} 部分：

**C#**
```CSharp
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var resourceLoader = new Windows.ApplicationModel.Resources.ResourceLoader();
var mydateplustime = resourceLoader.GetString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```
**JavaScript**
```JavaScript
// Assemble the custom pattern. This string comes from a resource, and should be localizable. 
var mydateplustime = WinJS.Resources.getString("date_plus_time");
mydateplustime = mydateplustime.replace("{date}", mydatepattern);
mydateplustime = mydateplustime.replace("{time}", mytimepattern);
```

然後您就可以根據自訂模式來建構新的格式子：

**C#**
```CSharp
// Get the custom formatter.
var mydateplustimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(mydateplustime);
```
**JavaScript**
```JavaScript
// Get the custom formatter.
var mydateplustimefmt = new dtf.DateTimeFormatter(mydateplustime);
```

## <span id="related_topics"> </span>相關主題


* [日期和時間格式化範例](http://go.microsoft.com/fwlink/p/?LinkId=231618)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Foundation.DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576)
 

 





<!--HONumber=Mar16_HO1-->


