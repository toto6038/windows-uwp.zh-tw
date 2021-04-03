---
description: 日期選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選當地語系化的日期值。
title: 日期選擇器
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 51fa98908fa4a2c1c026ce5a4c924b8f800aee8b
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286597"
---
# <a name="date-picker"></a>日期選擇器

日期選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選當地語系化的日期值。

![日期選擇器的範例](images/date-picker-closed.png)

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Windows UI 程式庫 2.2 或更新版本中有這個控制項使用圓角的新範本。 如需詳細資訊，請參閱[圓角半徑](../style/rounded-corner.md)。 WinUI 是 NuGet 套件，其中包含適用於 Windows 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **平臺 api：** [DatePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)、 [SelectedDate 屬性](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用日期選擇器可讓使用者挑選已知的日期，例如出生日期 (行事曆內容不重要)。

如果行事曆的內容很重要，請考慮使用行事 [曆日期選擇器](calendar-date-picker.md) 或 [日曆視圖](calendar-view.md)。

如需有關如何選擇正確日期控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/DatePicker">開啟應用程式，並查看 DatePicker 的運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

進入點會顯示所選的日期，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 日期選擇器與其他 UI 重疊；它不會將其他 UI 移開。

![展開中的日期選擇器範例](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>建立日期選擇器

這個範例示範如何建立含有標頭的簡單日期選擇器。

```xaml
<DatePicker x:Name="birthDatePicker" Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

產生的日期選擇器外觀如下：

![日期選擇器的範例](images/date-picker-closed.png)

### <a name="formatting-the-date-picker"></a>格式化日期選擇器

依預設，日期選擇器會顯示 day、month 和 year。 如果您的日期選擇器案例不需要所有欄位，您可以隱藏不需要的欄位。 若要隱藏欄位，請將其對應的 *欄位* Visible 屬性設定為 `false` ： [DayVisible](/uwp/api/windows.ui.xaml.controls.datepicker.dayvisible)、 [MonthVisible](/uwp/api/windows.ui.xaml.controls.datepicker.monthvisible)或 [YearVisible](/uwp/api/windows.ui.xaml.controls.datepicker.yearvisible)。

在這裡只需要年份，因此會隱藏日期和月份欄位。

```xaml
<DatePicker x:Name="yearDatePicker" Header="In what year was Microsoft founded?" 
            MonthVisible="False" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-year-only.png" alt-text="日期選擇器，其中包含隱藏的日期和月份欄位。":::

中每個的字串內容 `ComboBox` `DatePicker` 是由 [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting.datetimeformatter)所建立。 您可以藉 `DateTimeFormatter` 由提供 *格式範本* 或 *格式模式* 的字串，來通知您如何格式化日期值。 如需詳細資訊，請參閱 [DayFormat](/uwp/api/windows.ui.xaml.controls.datepicker.dayformat)、 [MonthFormat](/uwp/api/windows.ui.xaml.controls.datepicker.monthformat)和 [YearFormat](/uwp/api/windows.ui.xaml.controls.datepicker.yearformat) 屬性。

在此， *格式模式* 會用來將月份顯示為整數和縮寫。 您可以將常值字串新增至格式模式，例如在月份縮寫的括弧： `({month.abbreviated})` 。

```xaml
<DatePicker MonthFormat="{}{month.integer(2)} ({month.abbreviated})" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-day-hidden.png" alt-text="天欄位隱藏的日期選擇器。":::

### <a name="date-values"></a>日期值

日期選擇器控制項同時具有[日期](/uwp/api/windows.ui.xaml.controls.datepicker.date) / [DateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.datechanged)和[SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate) / [SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged) api。 這兩者之間的差異在於不可 `Date` 為 null，而可為 `SelectedDate` null。

的值 `SelectedDate` 是用來填入日期選擇器，而且 `null` 預設為。 如果 `SelectedDate` 為 `null` ，則 `Date` 屬性會設定為 12/31/1600; 否則， `Date` 值會與 `SelectedDate` 值同步。 當 `SelectedDate` 為時，選擇器為「取消設定」， `null` 並顯示功能變數名稱而非日期。

:::image type="content" source="images/date-time/date-picker-no-selected-date.png" alt-text="未選取日期的日期選擇器。":::

您可以設定 [MinYear](/uwp/api/windows.ui.xaml.controls.datepicker.minyear) 和 [MaxYear](/uwp/api/windows.ui.xaml.controls.datepicker.maxyear) 屬性，以限制選擇器中的日期值。 依預設， `MinYear` 會在目前日期之前設定為100年，並 `MaxYear` 設定為目前日期之後的100年份。

如果您只設定 `MinYear` 或 `MaxYear` ，您必須確定有效的日期範圍是由您設定的日期和其他日期的預設值所建立，否則選擇器中將不會有日期可供選取。 例如，設定只會 `yearDatePicker.MaxYear = new DateTimeOffset(new DateTime(900, 1, 1));` 使用預設值來建立不正確日期範圍 `MinYear` 。

#### <a name="initializing-a-date-value"></a>初始化日期值

日期屬性無法設定為 XAML 屬性字串，因為 Windows 執行階段 xaml 剖析器沒有將字串轉換為日期的轉換邏輯，以做為[DateTime](/uwp/api/windows.foundation.datetime)  /  [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true)物件。 以下是這些物件可在程式碼中定義並設定為目前日期以外日期的一些建議方式。

- [DateTime](/uwp/api/windows.foundation.datetime)：具現化 [Windows.](/uwp/api/windows.globalization.calendar) (的物件會初始化為目前的日期) 。 設定 [年份](/uwp/api/windows.globalization.calendar.year)或呼叫 [AddYears](/uwp/api/windows.globalization.calendar.addyears)，以調整日期。 然後，呼叫 [Calendar. GetDateTime](/uwp/api/windows.globalization.calendar.getdatetime) ，並使用傳回的 `DateTime` 來設定日期屬性。
- [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true)：呼叫函式。 若 [為內部 system.string](/dotnet/api/system.datetime?view=dotnet-uwp-10.0&preserve-view=true)，請使用此函式簽章。 或者，您也可以建立預設的 [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true) (它會初始化為目前的日期) 並呼叫 [AddYears](/dotnet/api/system.datetimeoffset.addyears?view=dotnet-uwp-10.0&preserve-view=true)。

另一種可能的方法是定義可作為資料物件或資料內容中的日期，然後將 date 屬性設定為參考 [{Binding} 標記延伸](/windows/uwp/xaml-platform/binding-markup-extension) 的 XAML 屬性，該擴充功能可以存取日期做為資料。

> [!NOTE]
> 如需日期值的重要資訊，請參閱＜日期和時間控制項＞文章中的 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values)。

這個範例示範如何 `SelectedDate` `MinYear` `MaxYear` 在不同的控制項上設定、和屬性 `DatePicker` 。

```xaml
<DatePicker x:Name="yearDatePicker" MonthVisible="False" DayVisible="False"/>
<DatePicker x:Name="arrivalDatePicker" Header="Arrival date"/>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set minimum year to 1900 and maximum year to 1999.
    yearDatePicker.SelectedDate = new DateTimeOffset(new DateTime(1950, 1, 1));
    yearDatePicker.MinYear = new DateTimeOffset(new DateTime(1900, 1, 1));
    // Using a different DateTimeOffset constructor.
    yearDatePicker.MaxYear = new DateTimeOffset(1999, 12, 31, 0, 0, 0, new TimeSpan());

    // Set minimum to the current year and maximum to five years from now.
    arrivalDatePicker.MinYear = DateTimeOffset.Now;
    arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
}
```

### <a name="using-the-date-values"></a>使用日期值

若要在您的應用程式中使用日期值，您通常會使用 [SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate) 屬性的資料系結，或處理 [SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged) 事件。

> 如需使用 `DatePicker` 和 `TimePicker` 一起更新單一值的範例 `DateTime` ，請參閱行事 [曆、日期和時間控制項-同時使用日期選擇器和時間選擇器](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together)。

在這裡，您會使用 `DatePicker` 來讓使用者選取其抵達日期。 您可以處理 `SelectedDateChanged` 事件，以更新名為的 [日期時間](/uwp/api/windows.foundation.datetime) 實例 `arrivalDateTime` 。

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="arrivalDatePicker_SelectedDateChanged"/>
    <Button Content="Clear" Click="ClearDateButton_Click"/>
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

    private void arrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDatePicker.SelectedDate = null;
        arrivalText.Text = string.Empty;
    }
}
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [時間選擇器](time-picker.md)
