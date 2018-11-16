---
author: mijacobs
Description: Use the ParallaxView control to add depth and movement to your app.
title: 如何使用 ParallaxView 控制項新增應用程式的深度和動態。
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.author: mijacobs
ms.date: 08/9/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 987202969f22a5b1b34efc9089ea7f7f08282d8c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6975009"
---
# <a name="parallax"></a>視差

視差是一種視覺效果，讓較靠近檢視者的項目移動的比背景中其他項目快。 視差能夠製造深度、透視和移動的感覺。 在 UWP 應用程式中，您可以使用 ParallaxView 控制項建立視差效果。  

> **重要 API**：[ParallaxView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview)、[VerticalShift 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift)、[HorizontalShift 屬性](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下此處以<a href="xamlcontrolsgallery:/item/ParallaxView">開啟應用程式並查看 ParallaxView 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>視差和 Fluent 設計系統

 Fluent 設計系統協助您建立結合光線、深度、動作、材質及縮放比例的現代化前衛 UI。 Parallax 是將動作、深度及縮放比例加入應用程式中的 Fluent 設計系統元件。 若要深入瞭解，請參閱[適用於 UWP 的 Fluent 設計概觀](../fluent-design-system/index.md)。

## <a name="how-it-works-in-a-user-interface"></a>使用者介面運作方式

在 UI 中建立視差效果的做法是在 UI 捲動或平移時，以不同的速率移動不同的物件。 <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> 為了示範，讓我們使用一個兩層的內容，分別是清單與背景影像。  清單位在背景影像上方，這已經造成清單可能比較靠近檢視者的錯覺。  現在，為了達到視差效果，我們要最靠近我們的物件移動速度比較遠的物件「快」。  當使用者捲動介面時，清單移動速度比背景影像快，因此造成深度錯覺。

 ![一個帶有背景影像和清單的視差範例](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>使用 ParallaxView 控制項產生視差效果

若要建立的視差效果，請使用 [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 控制項。 此控制項會將前景元素 (例如清單) 的捲動位置，繫結至背景元素 (例如影像)。 當您捲動前景元素時，背景元素會有動畫效果，以產生視差效果。 

若要使用 ParallaxView 控制項，請提供來源元素、背景元素，並將 [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (針對垂直捲動) 和/或 [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (針對水平捲動) 屬性設定為大於零的值。 
* Source 屬性會參考前景元素。 為了讓視差效果發生，前景應該是 [ScrollViewer](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 或包含 ScrollViewer 的元素，例如 [ListView](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.listview) 或 [RichTextBox](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichEditBox)。 

* 若要設定背景元素，請將該元素新增成為 ParallaxView 控制項的子系。 背景元素可以是任何的 [UIElement](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement)，例如 [Image](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.Image)，或包含其他 UI 元素的面板。 

若要建立視差效果，ParallaxView 必須在前景元素的後方。 [Grid](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.grid)與 [Canvas](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.canvas) 面板可讓您將項目以互相交疊的方式分層，讓它們能夠搭配 ParallaxView 控制項運作。  

此範例會為清單產生視差效果：
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>  
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
``` 

ParallaxView 會自動調整影像大小，讓影像能為視差作業運作，使您無須擔心影像捲動時會超出檢視範圍。

## <a name="customizing-the-parallax-effect"></a>自訂視差效果 

VerticalShift 與 HorizontalShift 屬性可讓您控制視差效果的程度。

* VerticalShift 屬性指定我們要背景在整個視差作業期間垂直移位多遠的距離。 值為 0 則表示背景完全不移動。
* HorizontalShift 屬性指定我們要背景在整個視差作業期間水平移位多遠的距離。 值為 0 則表示背景完全不移動。

值愈大，效果愈大。 

如需自訂視差方式清單，請查看 ParallaxView 類別。 

## <a name="dos-and-donts"></a>應做與不應做事項

- 在有背景影像的清單中使用視差
- 當 ListViewItems 包含影像時，考慮在 ListViewItems 中使用視差
- 不要到處使用視差，過度使用會削弱其效果

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [ParallaxView 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [適用於 UWP 的 Fluent 設計](../fluent-design-system/index.md)
- [系統中的科學：Fluent 設計和深度](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
