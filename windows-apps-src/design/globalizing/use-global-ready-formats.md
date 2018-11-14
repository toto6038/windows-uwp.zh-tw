---
author: stevewhims
Description: Design your app to be global-ready by appropriately formatting dates, times, numbers, phone numbers, and currencies. You'll then be able later to adapt your app for additional cultures, regions, and languages in the global market.
title: 全球化您的日期/時間/數字格式
ms.assetid: 6ECE8BA4-9A7D-49A6-81EE-AB2BE7F0254F
template: detail.hbs
ms.author: stwhi
ms.date: 11/07/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 173198c2c61530704dad02e2e92e6a7e47aae420
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6249592"
---
# <a name="globalize-your-datetimenumber-formats"></a>全球化您的日期/時間/數字格式

藉由為日期、時間、數字、電話號碼及貨幣進行適當的格式設定，將您的應用程式設計為可全球通用。 您的應用程式在將來便可因應全球市場中的其他文化特性、地區及語言。

## <a name="introduction"></a>簡介

在建立您的應用程式時，若您思考的範圍超越單一語言和文化，則當您的應用程式成長進入新的市場時，便會遭遇到更少未預期的問題 (若有的話)。 例如，日期、時間、數字、行事曆、貨幣、電話號碼、度量單位和紙張大小等項目，全部都可以根據不同的文化特性或語言以不同方式顯示。

不同的地區和文化使用不同的日期和時間格式。 這包含使用不同的慣例來排列日期的日、月順序、時間的小時和分鐘區隔方式，即使是做為分隔符號的標點符號也會有所不同。 此外，日期可能會因為不同文化而顯示為各種長格式 (「Wednesday, March 28, 2012」) 或短格式 (「3/28/12」)。 不同語言之間的星期幾與月份的名稱和縮寫當然也會有所不同。

您可以預覽不同語言使用的格式。 移至 **[設定]** > **[時間與語言]** > **[地區與語言]**，然後按一下 **[其他日期、時間及區域設定]** > **[變更日期、時間或數字格式]**。 在 **[格式]** 索引標籤上，從 **[格式]** 下拉式功能表中選取語言，然後在 **[範例]** 中預覽格式。

此主題會使用「使用者設定檔語言清單」、「應用程式資訊清單語言清單」及「應用程式執行階段語言清單」等三個術語。 如需取得這些術語意義和存取其值的詳細資訊，請參閱[了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)。

## <a name="format-dates-and-times-for-the-app-runtime-language-list"></a>為應用程式執行階段語言清單格式化日期和時間

如果需要讓使用者選擇日期或選取時間，請使用標準[日曆、日期和時間控制項](../controls-and-patterns/date-and-time.md)。 這些會自動使用最適合應用程式執行階段語言清單的日期和時間格式。

若您需要自行顯示日期或時間，您可以使用 [**DateTimeFormatter**](/uwp/api/windows.globalization.datetimeformatting?branch=live) 類別。 根據預設，**DateTimeFormatter** 會自動使用最適合應用程式執行階段語言清單的日期和時間格式。 因此，以下程式碼會將特定 **DateTime** 格式化為最適合該清單的格式。 例如，假設您的應用程式資訊清單語言清單包含您預設語言的英文 (美國) 和德文 (德國)。 若目前的日期是 2017 年 11 月 6 日，使用者設定檔語言清單首先包含德文 (德國)，則格式器將為 "06.11.2017"。 若使用者設定檔語言清單首先包含英文 (美國) (或並未包含英文或德文)，則格式器將為 "11/6/2017" (因為 "en-US" 符合，或作為預設值使用)。

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate");
    var shortTimeFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shorttime");

    var dateTimeToFormat = DateTime.Now;

    var shortDate = shortDateFormatter.Format(dateTimeToFormat);
    var shortTime = shortTimeFormatter.Format(dateTimeToFormat);

    var results = "Short Date: " + shortDate + "\n" +
                  "Short Time: " + shortTime;
```

您可以在您自己的電腦上以這種方式測試。

- 確認您專案中的資源檔同時限定 "en-US" 和 "de-DE" (請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md))。
- 在 **[設定]** > **[時間與語言]** > **[地區與語言]** > **[語言]** 中變更您的使用者設定檔語言清單。 新增德文 (德國)，將其設為預設值，然後重新執行程式碼。

## <a name="format-dates-and-times-for-the-user-profile-language-list"></a>為使用者設定檔語言清單格式化日期和時間

請記住，根據預設，**DateTimeFormatter** 會比對應用程式執行階段語言清單。 如此一來，若您顯示字串，例如「日期是 &lt;date&gt;」，則語言便會符合日期格式。

若您因為任何理由，想要只根據使用者設定檔語言清單格式化日期和/或時間，您可以使用程式碼執行此作葉，如以下範例所示。 但是若您決定執行此作業，請了解使用者可能會選擇您的應用程式不具備翻譯字串的語言。 例如，若您的應用程式並未當地語系化成德文 (德國)，但使用者選擇該語言作為其慣用語言，這可能會導致奇怪的字串顯示結果，例如「日期是 06.11.2017」。

```csharp
    // Use the DateTimeFormatter class to display dates and times using basic formatters.

    var userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;

    var shortDateFormatter = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("shortdate", userLanguages);

    var results = "Short Date: " + shortDateFormatter.Format(DateTime.Now);
```

## <a name="format-numbers-and-currencies-appropriately"></a>以適當方式格式化數字及貨幣

不同文化會以不同方式來格式化數字。 格式的差異可能是要顯示到小數第幾位、小數分隔符號要使用哪個字元，以及要使用哪個貨幣符號。 使用 [**NumberFormatting**](/uwp/api/windows.globalization.numberformatting?branch=live) 命名空間中的類別來顯示小數點、百分比或千分比數字及貨幣。 在大多數的時候，您會希望這些格式器類別使用最適合使用者設定檔的格式。 但您也可以使用格式器來顯示任何區域或格式的貨幣。

這個範例顯是如何根據使用者設定檔和給定的特定貨幣系統顯示貨幣。

```csharp
    // This scenario uses the CurrencyFormatter class to format a number as a currency.

    var userCurrency = Windows.System.UserProfile.GlobalizationPreferences.Currencies[0];

    var valueToBeFormatted = 12345.67;

    var userCurrencyFormatter = new Windows.Globalization.NumberFormatting.CurrencyFormatter(userCurrency);
    var userCurrencyValue = userCurrencyFormatter.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency,
    // in this case US Dollar (specified as an ISO 4217 code) 
    // but with the default number formatting for the current user.
    var currencyFormatUSD = new Windows.Globalization.NumberFormatting.CurrencyFormatter("USD");
    var currencyValueUSD = currencyFormatUSD.Format(valueToBeFormatted);

    // Create a formatter initialized to a specific currency.
    // In this case it's the Euro with the default number formatting for France.
    var currencyFormatEuroFR = new Windows.Globalization.NumberFormatting.CurrencyFormatter("EUR", new[] { "fr-FR" }, "FR");
    var currencyValueEuroFR = currencyFormatEuroFR.Format(valueToBeFormatted);

    // Results for display.
    var results = "Fixed number (" + valueToBeFormatted + ")\n" +
                    "With user's default currency: " + userCurrencyValue + "\n" +
                    "Formatted US Dollar: " + currencyValueUSD + "\n" +
                    "Formatted Euro (fr-FR defaults): " + currencyValueEuroFR;
```

您可以藉由在 **[設定]** > **[時間與語言]** > **[地區與語言]** > **[國家或地區]** 中變更國家或地區，來在您自己的電腦上測試上述程式碼。 選擇國家或地區 (假設是冰島)，然後重新執行程式碼。

## <a name="use-a-culturally-appropriate-calendar"></a>使用符合當地文化的日曆

不同地區及語言的行事曆也有所不同。 公曆 (西曆) 不是每個地區的預設行事曆。 有些地區的使用者可能會選擇其他日曆，像是日本年號年曆或阿拉伯陰曆。 不同的時區及日光節約時間也會對日曆上的日期和時間有顯著影響。

若要確保使用的是慣用日曆格式，您可以使用標準[日曆、日期和時間控制項](../controls-and-patterns/date-and-time.md)。 如果遇到更複雜的案例，需要在日曆日期上直接使用操作，**Windows.Globalization** 可以提供 [**Calendar**](/uwp/api/windows.globalization.calendar?branch=live) 類別，對特定文化、地區及日曆類型，提供適當的日曆表示法。

## <a name="format-phone-numbers-appropriately"></a>以適當方式格式化電話號碼

不同地區格式化電話號碼的方式也有所不同。 數字位數、數字分組方式及電話號碼特定部分的意義，都會隨每個國家/地區而有所不同。 從 Windows 10 版本 1607 開始，您可以使用 [**PhoneNumberFormatting**](/uwp/api/windows.globalization.phonenumberformatting?branch=live) 命名空間中的類別來針對目前地區適當地格式化電話號碼。

[**PhoneNumberInfo**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberinfo?branch=live) 會剖析數字字串，並可讓您：判斷數字是否為目前地區中的有效電話號碼、比較兩個號碼是否相等、並擷取電話號碼不同的功能部分，例如國碼 (地區碼)。

[**PhoneNumberFormatter**](/uwp/api/windows.globalization.phonenumberformatting.phonenumberformatter?branch=live) 會格式化數字字串或 **PhoneNumberInfo** 以供顯示，即使數字字串代表部分的電話號碼。 您可以使用此部分的號碼格式，在使用者輸入號碼時格式化號碼。

以下範例示範如何使用 **PhoneNumberFormatter**，在輸入電話號碼時格式化電話號碼。 每當名為 phoneNumberInputTextBox 的 **TextBox** 內有文字變更時，就會使用目前預設地區設定來格式化文字方塊內容，並顯示在名為 phoneNumberOutputTextBlock 的 **TextBlock** 中。 為了便於示範，字串也使用紐西蘭的地區設定格式化，並顯示在名為 phoneNumberOutputTextBlockNZ 的 TextBlock 中。
  
```csharp
    using Windows.Globalization.PhoneNumberFormatting;

    PhoneNumberFormatter currentFormatter, NZFormatter;

    public MainPage()
    {
        this.InitializeComponent();

        // Use the default formatter for the current region
        this.currentFormatter = new PhoneNumberFormatter();

        // Create an explicit formatter for New Zealand. 
        PhoneNumberFormatter.TryCreate("NZ", out this.NZFormatter);
    }

    private void phoneNumberInputTextBox_TextChanged(object sender, TextChangedEventArgs e)
    {
        // Format for the default region.
        this.phoneNumberOutputTextBlock.Text = currentFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);

        // If the NZFormatter was created successfully, format the partial string for the NZ TextBlock.
        if(this.NZFormatter != null)
        {
            this.phoneNumberOutputTextBlockNZ.Text = this.NZFormatter.FormatPartialString(this.phoneNumberInputTextBox.Text);
        }
    }
```    

您可以藉由在 **[設定]** > **[時間與語言]** > **[地區與語言]** > **[國家或地區]** 中變更國家或地區，來在您自己的電腦上測試上述程式碼。 選擇國家或地區 (假設是紐西蘭，以確認該格式相符)，然後重新執行程式碼。 如需取得測試資料，您可以使用 Web 搜尋位於紐西蘭公司的電話號碼。

## <a name="the-users-language-and-cultural-preferences"></a>使用者語言及文化喜好設定

如果您想要只根據使用者的語言、地區或文化喜好設定提供不同功能，Windows 可以讓您透過 [**Windows.System.UserProfile.GlobalizationPreferences**](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live) 存取這些喜好設定。 如有需要，可以使用 **GlobalizationPreferences** 類別取得使用者目前地理區域、慣用語言、慣用貨幣等項目的值。 但請記得，若您應用程式的字串/影像並未針對使用者的慣用語言進行當地語系化，則日期、時間和其他為該慣用語言格式化的資料可能不會與您顯示的字串相符。

## <a name="important-apis"></a>重要 API

* [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting?branch=live)
* [NumberFormatting](/uwp/api/windows.globalization.numberformatting?branch=live)
* [Calendar](/uwp/api/windows.globalization.calendar?branch=live)
* [PhoneNumberFormatting](/uwp/api/windows.globalization.phonenumberformatting?branch=live)
* [GlobalizationPreferences](/uwp/api/windows.system.userprofile.globalizationpreferences?branch=live)

## <a name="related-topics"></a>相關主題

* [日曆、日期和時間控制項](../controls-and-patterns/date-and-time.md)
* [了解使用者設定檔語言和應用程式資訊清單語言](manage-language-and-region.md)
* [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](../../app-resources/tailor-resources-lang-scale-contrast.md)

## <a name="samples"></a>範例

* [行事曆詳細資料及數學範例](http://go.microsoft.com/fwlink/p/?linkid=231636)
* [日期和時間格式化範例](http://go.microsoft.com/fwlink/p/?linkid=231618)
* [全球化喜好設定範例](http://go.microsoft.com/fwlink/p/?linkid=231608)
* [數字格式化及剖析範例](http://go.microsoft.com/fwlink/p/?linkid=231620)
