---
author: DelfCo
Description: "藉由為日期、時間、數字、電話號碼及貨幣進行適當的格式設定，即可開發全球通用的應用程式。"
title: "使用全球通用格式"
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
label: Use global-ready formats
template: detail.hbs
ms.author: bobdel
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a733f3d87f2e6598e49d8926a7b10797e335c79c
ms.lasthandoff: 02/07/2017

---

# <a name="use-global-ready-formats"></a>使用全球通用格式

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

藉由為日期、時間、數字、電話號碼及貨幣進行適當的格式設定，即可開發全球通用的應用程式。 這可讓您的應用程式在將來因應全球市場中的其他文化特性、地區及語言。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)</li>
<li>[**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)</li>
<li>[**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)</li>
<li>[**Windows.Globalization.PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting)</li>
</ul>
</div>


## <a name="introduction"></a>簡介

許多應用程式開發人員在建立應用程式時，都會自然而然地只顧慮到自己的語言和文化。 但是，當應用程式開始跨足其他市場時，調整應用程式以適應新語言和新區域就可能會遭遇意外的困難。 例如，日期、時間、數字、行事曆、貨幣、電話號碼、度量單位和紙張大小等項目，全部都可以根據不同的文化特性或語言以不同方式顯示。

只要在開發應用程式時將幾個事項納入考量，就可以簡化適應新市場的程序。

## <a name="format-dates-and-times-appropriately"></a>以適當方式格式化日期和時間

有許多方法可以正確顯示日期和時間。 不同的區域與文化會使用不同的慣例來排列日期的日、月順序、時間的小時和分鐘區隔方式，即使是做為分隔符號的標點符號也會有所不同。 此外，日期可能會因為不同文化而顯示為各種長格式 (「Wednesday, March 28, 2012」) 或短格式 (「3/28/12」)。 每種語言的星期幾與月份的名稱和縮寫當然也會有所不同。

如果需要讓使用者選擇日期或選取時間，請使用標準[日期和時間選擇器](https://msdn.microsoft.com/library/windows/apps/hh465466)控制項。 這些控制項會自動使用使用者慣用語言及區域的日期和時間格式。

如果需要自行顯示日期或時間，請使用 [**Date/Time**](https://msdn.microsoft.com/library/windows/apps/br206859) 及 [**Number**](https://msdn.microsoft.com/library/windows/apps/br226136) 格式器，自動以使用者慣用的格式顯示日期、時間及數字。 以下程式碼將使用目前的慣用語言和區域，來格式化指定的 DateTime。 例如，假設目前日期是 2012 年 6 月 3 日。如果使用者慣用英文 (美國)，則格式器會產生「6/3/2012」；但如果使用者慣用德文 (德國)，則會產生「03.06.2012」：

```CSharp
    // Use the Windows.Globalization.DateTimeFormatting.DateTimeFormatter class
    // to display dates and times using basic formatters.

    // Formatters for dates and times, using shortdate format.
    var sdatefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var stimefmt = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    // Obtain the date that will be formatted.
    var dateToFormat = DateTime.Now;

    // Perform the actual formatting.
    var sdate = sdatefmt.Format(dateToFormat);
    var stime = stimefmt.Format(dateToFormat);

    // Results for display.
    var results = "Short Date: " + sdate + "\n" +
                  "Short Time: " + stime;
```

## <a name="format-numbers-and-currencies-appropriately"></a>以適當方式格式化數字及貨幣

不同文化會以不同方式來格式化數字。 格式的差異可能是要顯示到小數第幾位、小數分隔符號要使用哪個字元，以及要使用哪個貨幣符號。 使用 [**NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136) 來顯示小數點、百分比或千分比數字及貨幣。 在大部分情況下，您只要根據使用者目前的喜好設定來顯示數字或貨幣即可。 不過您也可以使用格式器來顯示特定區域或格式的貨幣。

以下程式碼提供的範例是如何依照使用者慣用的語言及地區顯示貨幣，或針對特定貨幣系統顯示貨幣：

```CSharp
    // This scenario uses the Windows.Globalization.NumberFormatting.CurrencyFormatter class
    // to format a number as a currency.

    // Determine the current user's default currency.
    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    // Number to be formatted.
    var fractionalNumber = 12345.67;

    // Currency formatter using the current user's preference settings for number formatting.
    var userCurrencyFormat = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var currencyDefault = userCurrencyFormat.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD"); 
    var currencyUSD = currencyFormatUSD.Format(fractionalNumber);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyEuroFR = currencyFormatEuroFR.Format(fractionalNumber);

    // Results for display.
    var results = "Fixed number (" + fractionalNumber + ")\n" +
                  "With user's default currency: " + currencyDefault + "\n" +
                  "Formatted US Dollar: " + currencyUSD + "\n" +
                  "Formatted Euro (fr-FR defaults): " + currencyEuroFR;
```

## <a name="use-a-culturally-appropriate-calendar"></a>使用符合當地文化的行事曆

不同地區及語言的行事曆也有所不同。 公曆 (西曆) 不是每個地區的預設行事曆。 有些地區的使用者可能會選擇其他行事曆，像是日本年號年曆或阿拉伯陰曆。 不同的時區及日光節約時間也會對行事曆上的日期和時間有顯著影響。

使用標準[日期和時間選擇器](https://msdn.microsoft.com/library/windows/apps/hh465466)控制項可讓使用者選擇日期，確保使用慣用的行事曆格式。 如果遇到更複雜的案例，需要在行事曆日期上直接使用操作，Windows.Globalization 可以提供 [**Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724) 類別，針對特定文化、地區及行事曆類型，提供適當的行事曆表示法。

## <a name="format-phone-numbers-appropriately"></a>以適當方式格式化電話號碼
不同地區格式化電話號碼的方式也有所不同。 數字位數、數字分組方式及電話號碼特定部分的意義，都會隨每個國家/地區而有所不同。 從 Windows 10 版本 1607 開始，您可以使用 [**PhoneNumberFormatting**](https://msdn.microsoft.com/library/windows/apps/Windows.Globalization.PhoneNumberFormatting) 來針對目前地區適當地格式化電話號碼。

[**PhoneNumberInfo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberinfo.aspx) 會剖析數字字串，並可讓您判斷數字是否為目前地區中的有效電話號碼、比較兩個號碼是否相等、並擷取電話號碼不同的功能部分，例如國碼 (地區碼)。

[**PhoneNumberFormatter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.globalization.phonenumberformatting.phonenumberformatter.aspx) 會格式化數字字串或 PhoneNumberInfo 以供顯示，即使數字字串代表部分的電話號碼。 (您可以使用此部分的號碼格式，在使用者輸入號碼時格式化號碼。) 

下列程式碼示範如何使用 PhoneNumberFormatter，在輸入電話號碼時格式化電話號碼。 每當名為 gradualInput 的 TextBox 內有文字變更時，就會使用目前預設地區設定來格式化文字方塊內容，並顯示在名為 outBox 的 TextBlock 中。 為了便於示範，字串也使用紐西蘭的地區設定格式化，並顯示在名為 NZOutBox 的 TextBlock 中。
    
```csharp
    using Windows.Globalization;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        // Note that you must check the results of TryCreate before you use the formatter.
        PhoneNumberFormatter.TryCreate("NZ", out NZFormatter);

    }

    private void gradualInput_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region into outBox.
        outBox.Text = currentFormatter.FormatPartialString(gradualInput.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZOutBox.
        if(NZFormatter != null)
        {
            NZOutBox.Text = NZFormatter.FormatPartialString(gradualInput.Text);
        }
    }
```    

## <a name="respect-the-users-language-and-cultural-preferences"></a>尊重使用者的語言及文化喜好設定

如果要根據使用者的語言、地區或文化喜好設定提供不同功能，Windows 可以讓您透過 [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825) 存取這些喜好設定。 如有需要，可以使用 **GlobalizationPreferences** 類別取得使用者目前地理區域、慣用語言、慣用貨幣等項目的值。

## <a name="related-topics"></a>相關主題

* [針對全球市場進行規劃](https://msdn.microsoft.com/library/windows/apps/hh465405)
* [日期和時間控制項的指導方針](https://msdn.microsoft.com/library/windows/apps/hh465466)

**參考資料**
* [**Windows.Globalization.Calendar**](https://msdn.microsoft.com/library/windows/apps/br206724)
* [**Windows.Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
* [**Windows.Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
* [**Windows.System.UserProfile.GlobalizationPreferences**](https://msdn.microsoft.com/library/windows/apps/br241825)

**範例**
* [行事曆詳細資料及數學範例](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [日期和時間格式化範例](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [全球化喜好設定範例](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [數字格式化及剖析範例](http://go.microsoft.com/fwlink/p/?linkid=231620)

