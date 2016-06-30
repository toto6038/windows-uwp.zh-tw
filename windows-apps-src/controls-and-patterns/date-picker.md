---
author: Jwmsft
Description: "日期選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選當地語系化的日期值。"
title: "日期選擇器"
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: c183f7390c5b4f99cf0f31426c1431066e1bc96d
ms.openlocfilehash: c237d4bc013ad0a1d0d16f695f4332a6aac7efdc

---

# 日期選擇器

日期選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選當地語系化的日期值。 

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**DatePicker 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)
-   [**Date 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)

## 這是正確的控制項嗎？
使用日期選擇器可讓使用者挑選已知的日期，例如出生日期 (行事曆內容不重要)。

如需有關如何選擇正確日期控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## 範例

進入點會顯示所選的日期，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 日期選擇器與其他 UI 重疊；它不會將其他 UI 移開。

![展開中的日期選擇器範例](images/controls_datepicker_expand.png)

## 建立日期選擇器

這個範例示範如何建立含有標頭的簡單日期選擇器。

```xaml
<DatePicker x:Name=birthDatePicker Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

產生的日期選擇器外觀如下：

![日期選擇器的範例](images/date-picker-closed.png)

> **注意**：&nbsp;&nbsp;如需有關日期值的重要資訊，請參閱＜日期和時間控制項＞文章中的 [DateTime 和行事曆值](date-and-time.md#datetime-and-calendar-values)。



## 相關文章

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [時間選擇器](time-picker.md)



<!--HONumber=Jun16_HO4-->


