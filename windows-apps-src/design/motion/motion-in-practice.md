---
description: 瞭解如何在您的應用程式中將流暢的動畫基礎，例如計時、簡化、方向和引力。
title: 實務動畫-Windows 應用程式中的動畫
label: Motion in practice
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cebc072a7b358aedfdd2320fa47f238712d7ee92
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220401"
---
# <a name="bringing-it-together"></a>組合在一起

時間、加減速、方向以及重力搭配，構成了 Fluent 動作的基礎。 每一個在其他應用程式中應予以考量，並在應用程式內容中適當地套用。

以下是 3 種在應用程式中套用 Fluent 動作基礎的方式。

:::row:::
    :::column:::
**隱含動畫** 參數變更中值之間的自動補間和計時會變更，以使用標準化值達成非常簡單的流暢動作。
    :::column-end:::
    :::column:::
**內建動畫** 系統元件（例如通用控制項和共用的動作）是「預設為流暢」。 基本概念的套用方式與隱含使用方式一致。
    :::column-end:::
    :::column:::
**遵循指引建議的自訂動畫** 有時候系統可能尚未針對您的案例提供確切的動作解決方案。 在這些情況下，請使用基準基本建議作為您體驗的起點。
    :::column-end:::
:::row-end:::

**轉換範例**

![功能性動畫](images/pageRefresh.gif)

:::row:::
    :::column:::
<b>向前方向：</b><br>
淡出： 150m;緩和：預設 <b>向前加速方向：</b><br>
向上滑150px：300毫秒;簡化：預設減速
    :::column-end:::
    :::column:::
<b>反向方向：</b><br>
向下滑動150px： 150ms;緩和：預設的向前加速 <b>方向：</b><br>
淡化：300毫秒;簡化：預設減速
    :::column-end:::
:::row-end:::

**物件範例**

 ![300ms 動作](images/control.gif)

:::row:::
    :::column:::
<b>方向展開：</b><br>
成長：300毫秒;簡化：標準
    :::column-end:::
    :::column:::
<b>方向合約：</b><br>
成長： 150ms;簡化：預設加速
    :::column-end:::
:::row-end:::

## <a name="examples"></a>範例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項資源庫</strong> 應用程式，請按一下這裡 <a href="xamlcontrolsgallery:/item/ImplicitTransition">開啟應用程式，並查看隱含轉換的實際</a>運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="implicit-animations"></a>隱含動畫

> 隱含動畫需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更新版本。

隱含動畫是一種簡單的方式，可在參數變更期間自動插插新的值與新值，以達成流暢的移動。

您可以隱含地以動畫顯示下列屬性的變更：

- [UIElement](/uwp/api/windows.ui.xaml.uielement)
  - **不透明度**
  - **旋轉**
  - **調整**
  - **翻譯**

- [框線](/uwp/api/windows.ui.xaml.controls.border)、 [ContentPresenter](/uwp/api/windows.ui.xaml.controls.contentpresenter)或 [面板](/uwp/api/windows.ui.xaml.controls.panel)
  - **背景**

每個可以有隱含動畫變更的屬性都有對應的 _轉換_ 屬性。 若要以動畫顯示內容，請將轉換類型指派給對應的 _轉換_ 屬性。 下表顯示 _轉換_ 屬性和要用於每一個的轉換類型。

| 動畫屬性 | 轉換屬性 | 隱含轉換類型 |
| -- | -- | -- |
| [UIElement。不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | [OpacityTransition](/uwp/api/windows.ui.xaml.uielement.opacitytransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Rotation](/uwp/api/windows.ui.xaml.uielement.rotation) | [RotationTransition](/uwp/api/windows.ui.xaml.uielement.rotationtransition) | [ScalarTransition](/uwp/api/windows.ui.xaml.scalartransition) |
| [UIElement.Scale](/uwp/api/windows.ui.xaml.uielement.scale) | [ScaleTransition](/uwp/api/windows.ui.xaml.uielement.scaletransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [UIElement.Translation](/uwp/api/windows.ui.xaml.uielement.translation) | [TranslationTransition](/uwp/api/windows.ui.xaml.uielement.translationtransition) | [Vector3Transition](/uwp/api/windows.ui.xaml.vector3transition) |
| [框線. 背景](/uwp/api/windows.ui.xaml.controls.border.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.border.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [ContentPresenter 背景](/uwp/api/windows.ui.xaml.controls.contentpresenter.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.contentpresenter.backgroundtransition) | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |
| [Panel。背景](/uwp/api/windows.ui.xaml.controls.panel.background) | [BackgroundTransition](/uwp/api/windows.ui.xaml.controls.panel.backgroundtransition)  | [BrushTransition](//uwp/api/windows.ui.xaml.uielement.brushtransition) |

這個範例示範如何使用不透明度屬性和轉換，讓按鈕在控制項啟用時淡入，並在停用時淡出。

```xaml
<Button x:Name="SubmitButton"
        Content="Submit"
        Opacity="{x:Bind OpaqueIfEnabled(SubmitButton.IsEnabled), Mode=OneWay}">
    <Button.OpacityTransition>
        <ScalarTransition />
    </Button.OpacityTransition>
</Button>
```

```csharp
public double OpaqueIfEnabled(bool IsEnabled)
{
    return IsEnabled ? 1.0 : 0.2;
}
```

## <a name="related-articles"></a>相關文章

- [動作概觀](index.md)
- [計時和加/減速](timing-and-easing.md)
- [方向性和重力](directionality-and-gravity.md)
