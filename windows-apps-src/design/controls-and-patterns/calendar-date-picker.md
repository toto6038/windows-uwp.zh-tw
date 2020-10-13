---
description: 行事曆日期選擇器是一種下拉式控制項，最適合用來從行事曆檢視中挑選單一日期，然後取得各種重要的相關資訊，例如星期幾、行事曆行程密度。
title: 行事曆日期選擇器
ms.assetid: 9e0213e0-046a-4906-ba86-0b49be51ca99
label: Calendar date picker
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c7061de6098c77214136f3441b43abbf35a10686
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829614"
---
# <a name="calendar-date-picker"></a>行事曆日期選擇器

行事曆日期選擇器是一種下拉式控制項，最適合用來從行事曆檢視中挑選單一日期，然後取得各種重要的相關資訊，例如星期幾、行事曆行程密度。 您可以修改行事曆來提供其他內容，或限制可用的日期。

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

> **平台 API**：[CalendarDatePicker 類別](/uwp/api/Windows.UI.Xaml.Controls.CalendarDatePicker)、[Date 屬性](/uwp/api/windows.ui.xaml.controls.calendardatepicker.date)、[DateChanged 事件](/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用者可以使用**行事曆日期選擇器**，從內容相關行事曆檢視中挑選單一日期。 它的用途包括選擇約會或出發日期。

若要讓使用者挑選已知的日期，例如出生日期 (行事曆內容不重要)，請考慮使用[日期選擇器](date-picker.md)。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/CalendarDatePicker">開啟應用程式並查看 CalendarDatePicker 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

如果尚未設定日期，進入點會顯示預留位置文字；否則，它會顯示所選的日期。 當使用者選取進入點時，行事曆檢視會展開，方便使用者選擇日期。 行事曆檢視與其他 UI 重疊；它不會將其他 UI 移開。

![行事曆日期選擇器的螢幕擷取畫面，其中顯示一個空白的 [選取日期] 文字方塊，然後是一個底下填入行事曆的文字方塊。](images/calendar-date-picker-2-views.png)

## <a name="create-a-date-picker"></a>建立日期選擇器

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date"/>
```

```csharp
CalendarDatePicker arrivalCalendarDatePicker = new CalendarDatePicker();
arrivalCalendarDatePicker.Header = "Arrival date";
```

產生的行事曆日期選擇器看起來像這樣︰

![已填入行事曆日期選擇器的螢幕擷取畫面，其標籤顯示 [抵達日期]。](images/calendar-date-picker-closed.png)

行事曆日期選擇器有一個內部 [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView)，可用來挑選日期。 CalendarView 屬性的子集 (例如 [IsTodayHighlighted](/uwp/api/windows.ui.xaml.controls.calendardatepicker.istodayhighlighted) 和 [FirstDayOfWeek](/uwp/api/windows.ui.xaml.controls.calendardatepicker.firstdayofweek)) 存在於 CalendarDatePicker 上面，而且會被轉送到內部 CalendarView 供您修改。 

不過，您無法變更內部 CalendarView 的 [SelectionMode](/uwp/api/windows.ui.xaml.controls.calendarview.selectionmode) 以允許複選。 如果您想讓使用者挑選多個日期或一律顯示行事曆，請考慮使用行事曆檢視，而不要使用行事曆日期選擇器。 請參閱[行事曆檢視](calendar-view.md)文章，以取得有關如何修改行事曆顯示方式的詳細資訊。

### <a name="selecting-dates"></a>選取日期

使用 [Date](/uwp/api/windows.ui.xaml.controls.calendardatepicker.date) 屬性來取得日期或設定選取的日期。 根據預設，Date 屬性是 **null**。 當使用者在行事曆檢視中選取一個日期時，便會更新這個屬性。 使用者可以按一下行事曆檢視中選取的日期，以取消選取該日期。 

您可以在程式碼中按照以下方式設定日期。

```csharp
myCalendarDatePicker.Date = new DateTime(1977, 1, 5);
```

當您在程式碼中設定日期時，值會受到 [MinDate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.mindate) 和 [MaxDate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.maxdate) 屬性的約束。
- 如果 **Date** 小於 **MinDate**，值會設定為 **MinDate**。
- 如果 **Date** 大於 **MaxDate**，值會設定為 **MaxDate**。

您可以處理 [DateChanged](/uwp/api/windows.ui.xaml.controls.calendardatepicker.datechanged) 事件，以便在 Date 值變更時收到通知。

> [!NOTE]
> 如需日期值的重要資訊，請參閱＜日期和時間控制項＞文章中的 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values)。

### <a name="setting-a-header-and-placeholder-text"></a>設定標頭與預留位置文字

您可以新增 [Header](/uwp/api/windows.ui.xaml.controls.calendardatepicker.header) (或標籤) 與 [PlaceholderText](/uwp/api/windows.ui.xaml.controls.calendardatepicker.placeholdertext) (或浮水印) 到行事曆日期選擇器，以告知使用者其用途。 若要自訂標頭的外觀，您可以設定 [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.calendardatepicker.headertemplate) 屬性，而不是 Header。

預設預留位置文字是「選取日期」。 您可以將 PlaceholderText 屬性設定為空字串以移除預設文字，或者您可以提供自訂文字，如下所示。

```xaml
<CalendarDatePicker x:Name="arrivalCalendarDatePicker" Header="Arrival date" 
                    PlaceholderText="Choose your arrival date"/>
```

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Xaml-Controls-Gallery) (英文) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [日期和時間控制項](date-and-time.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)
- [時間選擇器](time-picker.md)
