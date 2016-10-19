---
author: Jwmsft
Description: "行事曆日期選擇器是一種下拉式控制項，最適合用來從行事曆檢視中挑選單一日期，然後取得各種重要的相關資訊，例如天次或行事曆行程密度。"
title: "行事曆日期選擇器"
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: d32c643db84eeb7c81304467a326566938e726c0

---
# 行事曆日期選擇器

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

行事曆日期選擇器是一種下拉式控制項，最適合用來從行事曆檢視中挑選單一日期，然後取得各種重要的相關資訊，例如天次或行事曆行程密度。 您可以修改行事曆來提供其他內容，或限制可用的日期。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.aspx"><strong>CalendarDatePicker 類別</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx"><strong>Date 屬性</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx"><strong>DateChanged 事件</strong></a></li>
</ul>

</div>
</div>






## 這是正確的控制項嗎？
使用者可以使用**行事曆日期選擇器**，從內容相關行事曆檢視中挑選單一日期。 它的用途包括選擇約會或出發日期。

若要讓使用者挑選已知的日期，例如出生日期 (行事曆內容不重要)，請考慮使用[**日期選擇器**](date-picker.md)。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## 範例

如果尚未設定日期，進入點會顯示預留位置文字；否則，它會顯示所選的日期。 當使用者選取進入點時，行事曆檢視會展開，方便使用者選擇日期。 行事曆檢視與其他 UI 重疊；它不會將其他 UI 移開。

![行事曆日期選擇器的範例](images/calendar-date-picker-2-views.png)

## 建立日期選擇器

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

產生的行事曆日期選擇器看起來像這樣︰

![行事曆日期選擇器的範例](images/calendar-date-picker-closed.png)

行事曆日期選擇器有一個內部 [**CalendarView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.aspx)，可用來挑選日期。 CalendarView 屬性的子集 (例如 [**IsTodayHighlighted**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted.aspx) 和 [**FirstDayOfWeek**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.firstdayofweek.aspx)) 存在於 CalendarDatePicker 上面，而且會被轉送到內部 CalendarView 供您修改。 

不過，您無法變更內部 CalendarView 的 [**SelectionMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendarview.selectionmode.aspx) 以允許複選。 如果您想讓使用者挑選多個日期或一律顯示行事曆，請考慮使用行事曆檢視，而不要使用行事曆日期選擇器。 請參閱[行事曆檢視](calendar-view.md)文章，以取得有關如何修改行事曆顯示方式的詳細資訊。

### 選取日期

使用 [**Date**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.date.aspx) 屬性來取得日期或設定選取的日期。 根據預設，Date 屬性是 **null**。 當使用者在行事曆檢視中選取一個日期時，便會更新這個屬性。 使用者可以按一下行事曆檢視中選取的日期，以取消選取該日期。 

您可以在程式碼中按照以下方式設定日期。

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

當您在程式碼中設定日期時，值會受到 [**MinDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.mindate.aspx) 和 [**MaxDate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.maxdate.aspx) 屬性的約束。
- 如果 **Date** 小於 **MinDate**，值會設定為 **MinDate**。
- 如果 **Date** 大於 **MaxDate**，值會設定為 **MaxDate**。

您可以處理 [**DateChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.datechanged.aspx) 事件，以便在 Date 值變更時收到通知。

> **注意**&nbsp;&nbsp;如需有關日期值的重要資訊，請參閱＜日期和時間控制項＞文章中的 [DateTime 與 Calendar 值](date-and-time.md#datetime-and-calendar-values)。

### 設定標頭與預留位置文字

您可以新增 [**Header**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.header.aspx) (或標籤) 與 [**PlaceholderText**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.placeholdertext.aspx) (或浮水印) 到行事曆日期選擇器，以告知使用者其用途。 若要自訂標頭的外觀，您可以設定 [**HeaderTemplate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.calendardatepicker.headertemplate.aspx) 屬性，而不是 Header。

預設預留位置文字是「選取日期」。 您可以將 PlaceholderText 屬性設定為空字串以移除預設文字，或者您可以提供自訂文字，如下所示。

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```


## 相關文章

- [日期和時間控制項](date-and-time.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)
- [時間選擇器](time-picker.md)



<!--HONumber=Aug16_HO3-->


