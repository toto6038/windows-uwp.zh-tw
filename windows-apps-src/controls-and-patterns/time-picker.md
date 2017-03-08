---
author: Jwmsft
Description: "時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。"
title: "時間選擇器"
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 5cd0b42f05ca246005b09faa0e926bc2e625d2c1
ms.lasthandoff: 02/07/2017

---
# <a name="time-picker"></a>時間選擇器
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。 

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**TimePicker 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)</li>
<li>[**Time 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)</li>
</ul>
</div>

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
使用時間選擇器讓使用者挑選單一時間值。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

進入點會顯示所選的時間，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 時間選擇器會重疊於其他 UI 上；其不會將其他 UI 移開。

![展開中的時間選擇器範例](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>建立時間選擇器

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

> [!NOTE]
> 如需日期及時間值的重要資訊，請參閱 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values) (位於**＜日期及時間控制項＞文章中)。



## <a name="related-topics"></a>相關主題

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)

