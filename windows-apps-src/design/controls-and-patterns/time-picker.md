---
author: muhsinking
Description: The time picker gives you a standardized way to let users pick a time value using touch, mouse, or keyboard input.
title: 時間選擇器
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: mukin
ms.date: 05/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b54716798b41bb7f7ac0da53f895cf91735e4e31
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494065"
---
# <a name="time-picker"></a>時間選擇器
 

時間選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選時間值。 

> **重要 API**：[TimePicker 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)、[Time 屬性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
使用時間選擇器讓使用者挑選單一時間值。

如需有關如何選擇正確控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/TimePicker">開啟應用程式並查看 TimePicker 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

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
> 如需日期及時間值的重要資訊，請參閱 [DateTime 和 Calendar 值](date-and-time.md#datetime-and-calendar-values) (位於＜*日期及時間控制項*＞文章中)。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)：以互動式格式查看所有 XAML 控制項。

## <a name="related-topics"></a>相關主題

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [日期選擇器](date-picker.md)
