---
author: muhsinking
Description: The date picker gives you a standardized way to let users pick a localized date value using touch, mouse, or keyboard input.
title: 日期選擇器
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1f258ff63d2cf9badfc1066f46c97ecc14b90f5f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038897"
---
# <a name="date-picker"></a>日期選擇器

 

日期選擇器為您提供一個標準化的方式，可以讓使用者利用觸控、滑鼠或鍵盤輸入來挑選當地語系化的日期值。 

> **重要 API**：[DatePicker 類別](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.aspx)、[Date 屬性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.datepicker.date.aspx)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
使用日期選擇器可讓使用者挑選已知的日期，例如出生日期 (行事曆內容不重要)。

如需有關如何選擇正確日期控制項的詳細資訊，請參閱[日期和時間控制項](date-and-time.md)文章。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/DatePicker">開啟應用程式並查看 DatePicker 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

進入點會顯示所選的日期，當使用者選取進入點時，選擇器介面就會從中央垂直展開供使用者選取。 日期選擇器與其他 UI 重疊；它不會將其他 UI 移開。

![展開中的日期選擇器範例](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>建立日期選擇器

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

> **注意**&nbsp;&nbsp;如需有關日期值的重要資訊，請參閱＜日期和時間控制項＞文章中的 [DateTime 與 Calendar 值](date-and-time.md#datetime-and-calendar-values)。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [日期和時間控制項](date-and-time.md)
- [行事曆日期選擇器](calendar-date-picker.md)
- [行事曆檢視](calendar-view.md)
- [時間選擇器](time-picker.md)
