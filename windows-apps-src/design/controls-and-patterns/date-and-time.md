---
description: 日期和時間控制項可讓您檢視並設定日期和時間。 此文章提供設計指導方針，並協助您挑選適當的控制項。
title: 日期和時間控制項的指導方針
ms.assetid: 4641FFBB-8D82-4290-94C1-D87617997F61
label: Calendar, date, and time controls
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 239296cfb2de5b52b9f9ef1498b5c2c22e2be372
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286607"
---
# <a name="calendar-date-and-time-controls"></a>行事曆、日期和時間控制項

 

日期和時間控制項為您提供一個標準、當地語系化的方式，讓使用者在您的 app 中檢視以及設定日期和時間值。 此文章提供設計指導方針，並協助您挑選適當的控制項。

> **重要 API**：[CalendarView 類別](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)、[CalendarDatePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)、[DatePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)、[TimePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/category/DataInput">開啟應用程式並查看這些控制項的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="which-date-or-time-control-should-you-use"></a>您應該使用哪一個日期或時間控制項？

有四個日期和時間控制項可供選擇。您使用的控制項取決於您的案例。 使用此資訊，以選擇適合在您 app 中使用的控制項。

| 控制 | 範例 | 描述 |
| ------- | :-----: | ----------- |
| 行事曆檢視 | ![行事曆檢視的範例](images/controls_calendar_monthview_small.png) | 用來從一律顯示的行事曆中挑選單一日期或日期範圍。 |
| 行事曆日期選擇器 | ![行事曆日期選擇器的螢幕擷取畫面。](images/calendar-date-picker-closed.png) | 用來從內容相關的行事曆挑選單一日期。 |
| 日期選擇器 | ![日期選擇器的範例](images/date-picker-closed.png) | 當內容相關的資訊並不重要時，用來挑選單一已知日期。 |
| 時間選擇器 | ![時間選擇器的範例](images/time-picker-closed.png) | 用來挑選單一時間值。 |

<!-- This table seems redundant, not sure it's needed.-->

### <a name="calendar-view"></a>行事曆檢視

**行事曆檢視** 可讓使用者檢視行事曆並與其互動，以便依月份、年份或 10 年瀏覽行事曆。 使用者可以選取單一日期或日期範圍。 它沒有選擇器介面，而且行事曆一律會顯示。

行事曆檢視是由 3 個個別檢視所組成：月份檢視、年份檢視和十年份檢視。 行事曆啟動時預設以月份檢視開啟，但是您可以指定任何檢視做為啟動檢視。

![三個行事曆檢視的螢幕擷取畫面，其中顯示月份檢視、年份檢視和十年份檢視。](images/calendar-view-3-views.png)

- 若您需要讓使用者選取多個日期，則必須使用 **CalendarView**。
- 若您需要讓使用者只挑選單一日期，而且不需要一律顯示行事曆，請考慮使用 **CalendarDatePicker** 或 **DatePicker** 控制項。

### <a name="calendar-date-picker"></a>行事曆日期選擇器

**CalendarDatePicker** 是一種下拉式控制項，最適合用來從行事曆檢視中挑選單一日期，然後取得各種重要的相關資訊，例如星期幾或行事曆行程密度。 您可以修改行事曆來提供其他內容，或限制可用的日期。

如果尚未設定日期，進入點會顯示預留位置文字；否則，它會顯示所選的日期。 當使用者選取進入點時，行事曆檢視會展開，方便使用者選擇日期。 行事曆檢視與其他 UI 重疊；它不會將其他 UI 移開。

![行事曆日期選擇器的螢幕擷取畫面，其中顯示一個空白的 [選取日期] 文字方塊，然後是一個底下填入行事曆的文字方塊。](images/calendar-date-picker-2-views.png)

- 行事曆日期選擇器的用途包括選擇約會或出發日期。 

### <a name="date-picker"></a>日期選擇器

**DatePicker** 控制項提供一個選擇特定日期的標準化方式。 

進入點會顯示所選的日期，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 日期選擇器與其他 UI 重疊；它不會將其他 UI 移開。

![展開中的日期選擇器範例](images/controls_datepicker_expand.png)

- 使用日期選擇器可讓使用者挑選已知的日期，例如出生日期 (行事曆內容不重要)。

### <a name="time-picker"></a>時間選擇器

**TimePicker** 可用來選取單一時間值，例如約會或出發時間。 它是一種由使用者或在程式碼中設定的靜態顯示，但不會更新以顯示目前的時間。

進入點會顯示所選的時間，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 時間選擇器會重疊於其他 UI 上；其不會將其他 UI 移開。

![展開中的時間選擇器範例](images/controls_timepicker_expand.png)

- 使用時間選擇器讓使用者挑選單一時間值。

## <a name="create-a-date-or-time-control"></a>建立日期或時間控制項

如需每個日期和時間控制項的特定資訊和範例，請參閱以下文章。

- [行事曆檢視](calendar-view.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [日期選擇器](date-picker.md)
- [時間選擇器](time-picker.md)

### <a name="use-a-date-picker-and-time-picker-together"></a>同時使用日期選擇器和時間選擇器

這個範例示範如何 `DatePicker` 搭配使用， `TimePicker` 讓使用者選取他們的抵達日期和時間。 您可以處理 `SelectedDateChanged` 和 `SelectedTimeChanged` 事件，以更新名為的單一 [DateTime](/uwp/api/windows.foundation.datetime) 實例 `arrivalDateTime` 。 使用者也可以在設定之後清除日期和時間選擇器。

:::image type="content" source="images/date-time/date-and-time-picker.png" alt-text="日期選擇器、時間選擇器、按鈕和文字標籤。":::

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="ArrivalDatePicker_SelectedDateChanged"/>
    <StackPanel Orientation="Horizontal">
        <TimePicker x:Name="arrivalTimePicker" Header="Arrival time"
                MinuteIncrement="15"
                SelectedTimeChanged="ArrivalTimePicker_SelectedTimeChanged"/>
        <Button Content="Clear" Click="ClearDateButton_Click"
                VerticalAlignment="Bottom" Height="30" Width="54"/>
    </StackPanel>
    <TextBlock x:Name="arrivalText" Margin="0,12"/>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    DateTime arrivalDateTime;

    public MainPage()
    {
        this.InitializeComponent();

        // Set minimum to the current year and maximum to five years from now.
        arrivalDatePicker.MinYear = DateTimeOffset.Now;
        arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
    }

    private void ArrivalTimePicker_SelectedTimeChanged(TimePicker sender, TimePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalTimePicker.SelectedTime != null)
        {
            arrivalDateTime = new DateTime(arrivalDateTime.Year, arrivalDateTime.Month, arrivalDateTime.Day,
                                           args.NewTime.Value.Hours, args.NewTime.Value.Minutes, args.NewTime.Value.Seconds);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ArrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            if (VerifyDateIsFuture((DateTimeOffset)arrivalDatePicker.SelectedDate) == true)
            {
                arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day,
                                               arrivalDateTime.Hour, arrivalDateTime.Minute, arrivalDateTime.Second);
                arrivalText.Text = arrivalDateTime.ToString();
            }
            else
            {
                arrivalDatePicker.SelectedDate = null;
                arrivalText.Text = "Arrival date must be later than today.";
            }
        }
    }

    private bool VerifyDateIsFuture(DateTimeOffset date)
    {
        if (date > DateTimeOffset.Now)
        {
            return true;
        }
        return false;
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDateTime = new DateTime();
        arrivalDatePicker.SelectedDate = null;
        arrivalTimePicker.SelectedTime = null;
        arrivalText.Text = string.Empty;
    }
}
```

### <a name="globalization"></a>全球化

XAML 日期控制項支援 Windows 所支援的每個日曆系統。 這些行事曆是在 [Windows.Globalization.CalendarIdentifiers](/uwp/api/Windows.Globalization.CalendarIdentifiers) 類別中指定。 每個控制項會針對您 app 的預設語言使用正確的行事曆，或者您也可以設定 **CalendarIdentifier** 屬性，以使用特定的行事曆系統。

時間選擇器控制項支援 [Windows.Globalization.ClockIdentifiers](/uwp/api/Windows.Globalization.ClockIdentifiers) 類別中指定的每個時鐘系統。 您可以設定 [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) 屬性，以使用 12 小時制或 24 小時制的時鐘。 屬性的類型是字串，但您必須使用對應到 ClockIdentifiers 類別的靜態字串屬性的值。 這些是︰TwelveHour ("12HourClock" 字串) 和 TwentyFourHour ("24HourClock" 字串)。 "12HourClock" 是預設值。

### <a name="datetime-and-calendar-values"></a>DateTime 與 Calendar 值

XAML 日期和時間控制項中使用的日期物件有不同的呈現方式，取決於您的程式設計語言。

- C# 和 Visual Basic 使用屬於 .NET 一部分的 [System.DateTimeOffset](/dotnet/api/system.datetimeoffset) 結構。 
- C++/CX 使用 [Windows::Foundation::DateTime](/windows/desktop/api/windows.foundation/ns-windows-foundation-datetime) 結構。 

相關的概念是 Calendar 類別，這會影響在內容中如何解譯日期。 所有 Windows 執行階段應用程式都可以使用 [Windows.Globalization.Calendar](/uwp/api/Windows.Globalization.Calendar) 類別。 C# 和 Visual Basic 應用程式也可以使用 [System.Globalization.Calendar](/dotnet/api/system.globalization.calendar) 類別，它包含非常類似的功能。 (Windows 執行階段 app 可使用.NET 行事曆的基底類別，但非特定的實作；例如：GregorianCalendar。)

.NET 亦支援名為 [DateTime](/dotnet/api/system.datetime) 的類型，隱含可轉換為 [DateTimeOffset](/dotnet/api/system.datetimeoffset)。 因此，您可能會在 .NET 程式碼中看到用來設定值的 "DateTime" 類型，其實是 DateTimeOffset。 如需 DateTime 和 DateTimeOffset 之間差異的詳細資訊，請參閱 [DateTimeOffset](/dotnet/api/system.datetimeoffset) 類別中的「備註」。

> [!NOTE]
> 採用日期物件的屬性不能設定為 XAML 屬性字串，因為 Windows 執行階段 XAML 剖析器沒有可將字串轉換為 DateTime/DateTimeOffset 物件形式之日期的轉換邏輯。 您通常會在程式碼中設定這些值。 另一個可能的技術是定義可做為資料物件或在資料內容中使用的日期，然後將屬性設定為 XAML 屬性，參考可存取日期做為資料的 [\{Binding\}標記延伸](../../xaml-platform/binding-markup-extension.md)運算式。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML UI 基本知識範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)
- [行事曆範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Calendar)
- [日期和時間格式化範例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/DateTimeFormatting)

## <a name="related-topics"></a>相關主題

### <a name="for-developers-xaml"></a>適用於開發人員 (XAML)

- [CalendarView 類別](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)
- [CalendarDatePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)
- [DatePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)
- [TimePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)
