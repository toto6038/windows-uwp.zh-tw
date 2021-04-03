---
description: 時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。
title: 時間選擇器
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d5391e3d738e7de81362a5fd0e42dc6d007dd
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270227"
---
# <a name="time-picker"></a>時間選擇器
 
時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。

![時間選擇器的範例](images/time-picker-closed.png)

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

> **平臺 api**： [TimePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)、 [SelectedTime 屬性](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
使用時間選擇器讓使用者挑選單一時間值。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/TimePicker">開啟應用程式並查看 TimePicker 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

進入點會顯示所選的時間，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 時間選擇器會重疊於其他 UI 上；其不會將其他 UI 移開。

![展開中的時間選擇器範例](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>建立時間選擇器

此範例說明如何建立含有標頭的簡易時間選擇器。

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

產生的時間選擇器外觀如下：

![時間選擇器的範例](images/time-picker-closed.png)

### <a name="formatting-the-time-picker"></a>格式化時間選擇器

根據預設，時間選擇器會顯示具有 AM/PM 選取器的12小時制。 您可以將 [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) 屬性設定為 "24HourClock"，改為顯示24小時制。

```xaml
<TimePicker Header="12HourClock" SelectedTime="14:30"/>
<TimePicker Header="24HourClock" SelectedTime="14:30" ClockIdentifier="24HourClock"/>
```

:::image type="content" source="images/date-time/time-picker-clocks.png" alt-text="時間選擇器顯示12小時制，而選擇器顯示24小時制。":::

您可以設定 [MinuteIncrement](/uwp/api/windows.ui.xaml.controls.timepicker.minuteincrement) 屬性，以指出分鐘選擇器中顯示的時間增量。 例如，15指定 `TimePicker` 分鐘控制項只顯示選項00、15、30、45。

```xaml
<TimePicker MinuteIncrement="15"/>
```

:::image type="content" source="images/date-time/time-picker-minute-increment.png" alt-text="時間選擇器顯示15分鐘的增量。":::

### <a name="time-values"></a>時間值

時間選擇器控制項同時具有[time](/uwp/api/windows.ui.xaml.controls.timepicker.time) / [TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged)和[SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime) / [SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged) api。 這兩者之間的差異在於不可 `Time` 為 null，而可為 `SelectedTime` null。

的值 `SelectedTime` 是用來填入時間選擇器，而且 `null` 預設為。 如果 `SelectedTime` 為 `null` ，則 `Time` 屬性會設定為時間[](/dotnet/api/system.timespan?view=dotnet-uwp-10.0&preserve-view=true)範圍 0; 否則， `Time` 值會與 `SelectedTime` 值同步。 當 `SelectedTime` 為時，選擇器為「取消設定」， `null` 並顯示功能變數名稱而不是時間。

:::image type="content" source="images/date-time/time-picker-no-selected-time.png" alt-text="未選取時間的時間選擇器。":::

#### <a name="initializing-a-time-value"></a>初始化時間值

在程式碼中，您可以將時間屬性初始化為類型的值 `TimeSpan` ：

```csharp
TimePicker timePicker = new TimePicker
{
    SelectedTime = new TimeSpan(14, 15, 00) // Seconds are ignored.
};
```

您可以在 XAML 中將時間值設定為屬性。 如果您已 `TimePicker` 在 XAML 中宣告物件，而不使用時間值的系結，這可能是最簡單的方法。 使用格式為 *hh： Mm* 的字串，其中 *hh* 是小時，而且可以介於0到23之間， *Mm* 是分鐘，而且可以介於0到59之間。

```xaml
<TimePicker SelectedTime="14:15"/>
```

> [!NOTE]
> 如需日期及時間值的重要資訊，請參閱 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values) (位於  ＜日期及時間控制項＞文章中)。

### <a name="using-the-time-values"></a>使用時間值

若要在您的應用程式中使用 time 值，您通常會使用 [SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime) 或 [time](/uwp/api/windows.ui.xaml.controls.timepicker.time) 屬性的資料系結，直接在程式碼中使用 time 屬性，或處理 [SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged) 或 [TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged) 事件。

> 如需使用 `DatePicker` 和 `TimePicker` 一起更新單一值的範例 `DateTime` ，請參閱行事 [曆、日期和時間控制項-同時使用日期選擇器和時間選擇器](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together)。

在此， `SelectedTime` 屬性會用來比較所選時間與目前時間。

請注意，因為 `SelectedTime` 此屬性可為 null，所以您必須明確地將它轉換成 `DateTime` ，如下所示： `DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);` 。 `Time`不過，您可以在沒有轉換的情況下使用屬性，如下所示： `DateTime myTime = DateTime.Today + checkTimePicker.Time;` 。

:::image type="content" source="images/date-time/time-picker-check.png" alt-text="時間選擇器、按鈕和文字標籤。":::

```xaml
<StackPanel>
    <TimePicker x:Name="checkTimePicker"/>
    <Button Content="Check time" Click="{x:Bind CheckTime}"/>
    <TextBlock x:Name="resultText"/>
</StackPanel>
```

```csharp
private void CheckTime()
{
    // Using the Time property.
    // DateTime myTime = DateTime.Today + checkTimePicker.Time;
    // Using the SelectedTime property (nullable requires cast to DateTime).
    DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);
    if (DateTime.Now >= myTime)
    {
        resultText.Text = "Your selected time has already past.";
    }
    else
    {
        string hrs = (myTime - DateTime.Now).Hours.ToString();
        string mins = (myTime - DateTime.Now).Minutes.ToString();
        resultText.Text = string.Format("Your selected time is {0} hours, {1} minutes from now.", hrs, mins);
    }
}
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)
