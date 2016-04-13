---
Description: 時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。
title: 時間選擇器
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: 時間選擇器
template: detail.hbs
---

# 時間選擇器

時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。 

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**TimePicker 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)
-   [**Time 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)

## 這是正確的控制項嗎？
使用時間選擇器讓使用者挑選單一時間值。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## 範例

進入點會顯示所選的時間，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 時間選擇器會重疊於其他 UI 上；其不會將其他 UI 移開。

![展開中的時間選擇器範例](images/controls_timepicker_expand.png)

## 建立時間選擇器

此範例說明如何建立含有標頭的簡易時間選擇器。

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

產生的時間選擇器外觀如下：

![時間選擇器的範例](images/time-picker-closed.png)

> **注意**&nbsp;&nbsp;如需有關日期與時間值的重要資訊，請參閱*日期和時間控制項*一文中的[DateTime 和行事曆值](date-and-time.md#datetime-and-calendar-values)。

\[本文章包含通用 Windows 平台 (UWP) app 與 Windows 10 專屬的資訊。 如需 Windows 8.1 指導方針，請下載 [Windows 8.1 指導方針 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## 相關主題

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)


<!--HONumber=Mar16_HO1-->


