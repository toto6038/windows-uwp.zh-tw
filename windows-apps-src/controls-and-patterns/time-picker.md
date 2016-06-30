---
author: Jwmsft
Description: "時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。"
title: "時間選擇器"
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 5056a9f304ca21c977b9cc65b8ead007eccd4288

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

> **注意** &nbsp;&nbsp;如需有關日期和時間值的重要資訊，請參閱《日期和時間控制項》**文章中的[DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values)。



## 相關主題

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)



<!--HONumber=Jun16_HO4-->


