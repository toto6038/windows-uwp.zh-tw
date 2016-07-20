---
author: Jwmsft
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: "腳本動畫"
description: "腳本動畫不只是視覺意義上的動畫。"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: bcb8dbd3c0b2556c3d426687eb9be02ffe7265fb

---
# 腳本動畫

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


腳本動畫不只是視覺意義上的動畫。 腳本動畫是一種變更相依性屬性值做為時間函式的方式。 您可能需要動畫庫以外之腳本動畫的其中一個主要原因，就是定義控制項的視覺狀態，做為控制項範本或頁面定義的一部分。

## Silverlight 與 WPF 的差異

如果您很熟悉 Microsoft Silverlight 或 Windows Presentation Foundation (WPF)，請閱讀本節；否則可以略過本節。

通常，在 Windows 執行階段應用程式中建立腳本動畫就和 Silverlight 或 WPF 一樣。 但是，還是有一些重要的差異：

-   腳本動畫不是以視覺化方式讓 UI 產生動畫效果的唯一方式，也不是應用程式開發人員執行這項工作最簡單的方式。 通常，比較好的設計方式是使用主題動畫和轉換動畫，而不是使用腳本動畫。 這些動畫可以快速建立建議的 UI 動畫，並不需要熟悉動畫屬性目標的複雜做法。 如需詳細資訊，請參閱[動畫概念](animations-overview.md)。
-   在 Windows 執行階段中，有許多 XAML 控制項包含主題動畫和轉換動畫做為它們內建行為的一部分。 在大部分的情況下，WPF 和 Silverlight 控制項並沒有預設動畫行為。
-   並非您建立的所有自訂動畫預設都可以在 Windows 執行階段應用程式中執行，如果動畫系統判斷動畫可能對您的 UI 造成不良的效能，就不會執行。 系統判斷可能影響效能的動畫稱為「相依式動畫」**。 它是相依式的，因為計時動畫會直接針對 UI 執行緒來運作，而作用中的使用者輸入及其他更新也會嘗試將執行階段變更套用到 UI。 在 UI 執行緒上耗用大量系統資源的相依式動畫，在特定情況下會使應用程式沒有回應。 如果您的動畫會導致配置變更，或者可能影響 UI 執行緒上的效能，您通常需要明確啟用動畫讓它執行。 這就是特定動畫類別上 **EnableDependentAnimation** 屬性的作用。 如需詳細資訊，請參閱[相依式和獨立式動畫](./storyboarded-animations.md#dependent-and-independent-animations)。
-   Windows 執行階段目前不支援自訂的 Easing 函式。

## 定義腳本動畫

腳本動畫是一種變更相依性屬性值做為時間函式的方式。 您要產生動畫效果的屬性不一定是直接影響您應用程式 UI 的屬性。 但因為 XAML 是用來定義應用程式的 UI，所以通常會產生動畫效果的都是 UI 相關屬性。 例如，您可以讓 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 的角度或按鈕背景的色彩值產生動畫效果。

定義腳本動畫的其中一個可能的主要原因是如果您是控制項作者或正在重新範本化控制項，以及您正在定義視覺狀態。 如需詳細資訊，請參閱[視覺狀態的腳本動畫](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

無論您是為應用程式定義視覺狀態或自訂動畫，本主題中所說明的腳本動畫概念和 API 大部分都適用於兩者。

為了建立動畫效果，腳本動畫的目標屬性必須是「相依性屬性」**。 相依性屬性是 Windows 執行階段 XAML 實作的主要特色。 大部分常用 UI 元素的可編寫屬性通常是當做相依性屬性來實作的，因此您可以為它們建立動畫效果、套用資料繫結值，或是套用 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) 並以 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 將屬性做為目標。 如需相依性屬性運作方式的詳細資訊，請參閱[相依性屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185583)。

通常是以編寫 XAML 的方式定義腳本動畫。 如果您使用像是 Microsoft Visual Studio 這類工具，工具會為您產生 XAML。 您也可以使用程式碼定義腳本動畫，但較不常見。

讓我們看一下簡單的範例。 在這個 XAML 範例中，是在特定的 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 物件上為 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) 屬性設定動畫效果。

```xml
<!-- Animates the rectangle's opacity. -->
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation
              Storyboard.TargetName="MyAnimatedRectangle"
              Storyboard.TargetProperty="Opacity"
              From="1.0" To="0.0" Duration="0:0:1"/>
        </Storyboard>
// a different area of the XAML
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
```
      
### 識別要設定動畫效果的物件

在上一個範例中，腳本已為 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) 的 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) 屬性設定動畫效果。 您不是在物件本身宣告動畫， 而是在腳本的動畫定義內宣告。 腳本通常定義於 XAML 中，該 XAML 不在要設定動畫效果的物件 XAML UI 定義附近。 通常設定為 XAML 資源。

若要將動畫連接到目標，您要透過它的識別程式名稱參考目標。 您應該一律在 XAML UI 定義中套用 [x:Name 屬性](https://msdn.microsoft.com/library/windows/apps/Mt204788)，以命名要設定動畫效果的物件。 然後在動畫定義內設定 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/Hh759823)，針對物件設定動畫效果。 **Storyboard.TargetName** 的值則使用目標物件的名稱字串，就是您先前在別處以 x:Name 屬性所設定的名稱。

### 針對相依性屬性設定動畫效果

您在動畫中設定 [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824) 的值。 這會決定要針對哪一個目標物件的特定屬性設定動畫效果。

有時您需要將不是目標物件之直接屬性的屬性設成目標，該目標屬性巢狀於物件屬性關係的較深處。 您通常需要這麼做才能向下切入到一組參與物件和屬性值，直到可以參考可設定動畫效果的屬性類型 ([**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)、[**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723))。 這個概念稱為「間接目標」**，而使用這種方式設定目標屬性的語法稱為「屬性路徑」**。

這裡提供一個範例。 在腳本動畫最常做的一件事就是變更部分應用程式 UI 或控制項的色彩，以便表示該控制項處於特定的狀態。 假設您要為 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) 設定動畫效果，讓它從紅色變成綠色。 您預期和 [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066) 有關，這是正確的。 不過，影響物件色彩的 UI 元素上沒有任何屬性真正是 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 類型， 而是 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 類型。 所以，動畫真正需要針對的是 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 類別的 [**Color**](https://msdn.microsoft.com/library/windows/apps/BR242963) 屬性，這是 **Brush** 衍生的類型，通常用於這些與色彩相關的 UI 屬性。 以下是為動畫的屬性目標產生屬性路徑的樣子：

```xml
<Storyboard x:Name="myStoryboard">
    <ColorAnimation Storyboard.TargetName="tb1" From="Red" To="Green"
      Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
   />
    </Storyboard>
```

以下是如何從它的各部分來思考這個語法：

-   每一組 () 括號都包含一個屬性名稱。
-   屬性名稱內有一個點，這個點區分類型名稱和屬性名稱，所以您識別的屬性非常明確。
-   位於中間，不在括號內的點則是步驟。 語法將此解讀為，採用第一個屬性 (此為物件) 的值，逐步執行它的物件模型，然後將第一個屬性值的特定子屬性做為目標。

以下清單為您可能使用間接屬性目標的動畫目標案例，以及接近您使用的語法的一些屬性路徑字串：

-   為 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) 值設定動畫效果，如同套用到 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)： `(UIElement.RenderTransform).(TranslateTransform.X)`
-   為 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 的 [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 設定 [**Color**](https://msdn.microsoft.com/library/windows/apps/BR242963) 動畫效果，如同套用到 [**Fill**](https://msdn.microsoft.com/library/windows/apps/BR243378)： `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
-   為 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) ([**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022) 中四個轉換的其中一個) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) 值設定動畫效果，如同套用到 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)：`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

您可能注意到這些範例有些會在數字周圍使用方括弧。 這是索引子。 它表示方括弧前的屬性名稱有集合做為值，而您需要該集合內的項目 (以零為基底的索引識別)。

您也可以讓 XAML 附加屬性產生動畫效果。 一律將完整的附加屬性名稱放在括號內，例如 `(Canvas.Left)`。 如需詳細資訊，請參閱[讓 XAML 附加屬性產生動畫效果](./storyboarded-animations.md#animating-xaml-attached-properties)。

如需如何使用屬性路徑讓屬性的間接目標產生動畫效果的詳細資訊，請參閱 [Property-path 語法](https://msdn.microsoft.com/library/windows/apps/Mt185586)或 [**Storyboard.TargetProperty 附加屬性**](https://msdn.microsoft.com/library/windows/apps/Hh759824)。

### 動畫類型

Windows 執行階段動畫系統有三種腳本動畫適用的特定類型：

-   [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)，可以使用任何 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 設定動畫效果
-   [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)，可以使用任何 [**PointAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210346) 設定動畫效果
-   [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)，可以使用任何 [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066) 設定動畫效果

還有適用於物件參考值的一般化 [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) 動畫類型，稍後會再討論。

### 指定動畫值

到目前為止，我們已經說明了如何針對物件和屬性設定動畫效果，但還沒有說明動畫在執行時對屬性值的影響。

我們所說明的動畫類型有時是指 **From**/**To**/**By** 動畫。 這表示動畫會使用這些來自動畫定義的一或多個輸入，隨時間變更屬性的值：

-   此值從 **From** 值開始。 如果您沒有指定 **From** 值，開始值會是動畫執行之前動畫屬性所具備的值。 這可能是預設值、來自樣式或範本的值，或是特別由 XAML UI 定義或應用程式程式碼套用的值。
-   動畫最後的值是 **To** 值。
-   或是，若要指定相對於開始值的結束值，請設定 **By** 屬性。 您應該設定這個屬性而不是 **To** 屬性。
-   如果您沒有指定 **To** 值或 **By** 值，結束值會是動畫執行之前動畫屬性所具備的值。 在這種情況下，最好要有 **From** 值，因為如果沒有這個值，動畫就完全不會變更值；開始和結束值會相同。
-   動畫通常至少會有 **From**、**By** 或 **To** 其中一個，但不會三個同時存在。

讓我們回到稍早的 XAML 範例，再看一次 **From** 和 **To** 值，以及 **Duration**。 這個範例是設定 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) 屬性的動畫效果，**Opacity** 的屬性類型為 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)。 所以這裡使用的動畫是 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)。

`From="1.0" To="0.0"` 指定當動畫執行時，[**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) 屬性從值 1 開始，然後動畫效果結束在 0。 換句話說，這些 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值對 **Opacity** 屬性而言，這個動畫會使物件從不透明開始，然後淡化成透明。

```xml
...
    <Storyboard x:Name="myStoryboard">
    <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
...
```

`Duration="0:0:1"` 指定動畫持續時間，也就是矩形淡化的速度。 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 屬性是以 *hours*:*minutes*:*seconds* 的形式指定值。 因此，這個範例的時間期間為 1 秒。

如需 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 值和 XAML 語法的詳細資訊，請參閱 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)。

**注意** 對於我們所說明的範例，如果您確定設定動畫效果的物件其開始狀態的 [**Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962) 永遠等於 1，無論是透過預設或明確設定，都可以省略 **From** 值，動畫會使用隱含的開始值，結果會相同。

 

### From/To/By 的型別為 Nullable

我們之前提過，您可以省略 **From**、**To** 或 **By**，而使用目前的非動畫值替代遺漏值。 動畫的 **From**、**To** 或 **By** 屬性不是您可以猜得到的型別。 例如，[**DoubleAnimation.To**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) 屬性的型別不是 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)， 而是 **Double** 的 [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)。 另外，它的預設值是 **null** 而不是 0。 該 **null** 值是動畫系統用來分辨您並未特別針對 **From**、**To** 或 **By** 屬性設定值。 Visual C++ 元件延伸 (C++/CX) 沒有 **Nullable** 型別，所以改用 [**IReference**](https://msdn.microsoft.com/library/windows/apps/BR225864)。

### 其他動畫屬性

本節後續所說明的屬性都是選擇性屬性，具備預設值，且適用於大部分的動畫。

### **AutoReverse**

如果您沒有在動畫指定 [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) 或 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211)，動畫會執行一次，然後執行 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 指定的時間。

[**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) 屬性指定是否在時間軸到達其 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的最後時反向播放。 如果將它設定為 **true**，動畫會在到達宣告的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 最後時反向，將值從結束值 (**To**) 變更回開始值 (**From**)。 這表示該動畫實際上是執行 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的兩倍時間。

### **RepeatBehavior**

[**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 屬性指定時間軸播放的次數，或是時間軸在該期間內應重複的較長持續時間。 根據預設，時間軸有一個「1x」的反覆運算計數，這表示在它的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 會播放一次，不會重複。

您可以讓動畫執行多個反覆運算，例如，「3x」的值會讓動畫執行三次。 或是，您可以為 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 指定不同的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)。 該 **Duration** 應該比動畫本身的 **Duration** 長，才會生效。 例如，如果您將 **RepeatBehavior** 指定為 0:0:10，[**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 為 0:0:2 的動畫會重複五次。 如果除不盡，動畫會在到達 **RepeatBehavior** 時間時截斷，可能正好在動畫的中間。 最後，您可以指定特殊值「Forever」，這會使動畫無限的執行，直到您主動停止為止。

如需 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411) 值和 XAML 語法的詳細資訊，請參閱 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411)。

### **FillBehavior="Stop"**

根據預設，當動畫結束時，動畫會將屬性值留在最後的 **To** 或 **By** 修改的值，即使在它的持續時間已經超過也是一樣。 不過，如果您將 [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) 屬性的值設定為 [**FillBehavior.Stop**](https://msdn.microsoft.com/library/windows/apps/BR210306)，動畫值的值會還原為套用動畫之前的值，或是更精確的說，是相依性屬性系統所決定的目前有效值 (如需這個區別的詳細資訊，請參閱[相依性屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185583))。

### **BeginTime**

根據預設，動畫的 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) 是「0:0:0」，所以會在包含的 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 執行時立即開始。 如果 **Storyboard** 包含多個動畫，且您希望錯開其他動畫與初始動畫的開始時間，或是要刻意製造短時間的延遲，可以變更這個項目。

### **SpeedRatio**

如果在 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 中有多個動畫，您可以變更一或多個動畫相對於 **Storyboard** 的時間速率。 父項 **Storyboard** 才是控制動畫執行時如何使用 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 時間的主要項目。 不常使用這個屬性。 如需詳細資訊，請參閱 [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/BR243213)。

## 在 **Storyboard** 定義多個動畫

[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 的內容可以有多個動畫定義。 如果您將相關動畫套用到相同目標物件的兩個屬性，就可能有多個動畫。 例如，您可以變更做為 UI 元素 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 之 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 的 [**TranslateX**](https://msdn.microsoft.com/library/windows/apps/BR228122) 和 [**TranslateY**](https://msdn.microsoft.com/library/windows/apps/BR228124) 屬性，這會使得元素對角平移。 您需要兩個不同的動畫才能完成這個動作，但您可能希望這兩個動畫屬於相同的 **Storyboard**，因為您希望這兩個動畫永遠一起執行。

動畫不一定要是相同的類型，或是針對相同的目標。 它們可以有不同的持續時間，不需要共用任何屬性值。

當父項 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 執行時，其中的各個動畫也都會執行。

[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 類別實際上有許多和動畫類型相同的動畫屬性，因為兩者共用 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 基底類別。 因此，**Storyboard** 可以有 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 或 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204)。 不過您通常不會在 **Storyboard** 上設定這些項目，除非您希望所有包含的動畫都有該行為。 一般來說，在 **Storyboard** 上設定的任何 **Timeline** 屬性都會套用到它的所有子動畫。 如果未設定，**Storyboard** 會有隱含的持續時間，從所包含動畫的最長 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 值開始計算。 在 **Storyboard** 上明確設定短於其中一個子動畫的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 會使動畫被截斷，通常不希望發生這種情形。

腳本不能包含嘗試針對相同物件上的相同屬性設定動畫效果的兩個動畫。 如果您嘗試這麼做，會在腳本嘗試執行時發生執行階段錯誤。 即使動畫因為刻意設定不同的 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) 值和持續時間而沒有時間上的重疊，也同樣適用上述限制。 如果您真的希望在單一腳本中的相同屬性套用較複雜的動畫時間軸，應該要使用主要畫面格動畫。 請參閱[主要畫面格和 Easing 函式動畫](key-frame-and-easing-function-animations.md)。

動畫系統可以將多個動畫套用到屬性的值，前提是這些輸入必須來自多個腳本。 特別為同時執行的腳本使用這個行為並不常見。 不過，套用到控制項屬性的應用程式定義的動畫，將會修改之前以控制項視覺狀態模型執行之動畫的 **HoldEnd** 值，是有可能的。

## 將腳本定義為資源

[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 是您放置動畫物件的容器。 您通常在頁面層級的 [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) 或 [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/BR242338) 將 **Storyboard** 定義為資源，供想要設定動畫效果的物件使用。

下一個範例說明前一個範例 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 如何包含在頁面層級的 [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) 定義中，其中 **Storyboard** 是根 [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503) 的索引資源。 請注意 [x:Name 屬性](https://msdn.microsoft.com/library/windows/apps/Mt204788)。 這個屬性是您為 **Storyboard** 定義變數名稱的方式，以便讓 XAML 中的其他元素和程式碼之後可以參考 **Storyboard**。

```xml
<Page ...>
  <Page.Resources>
        <!-- Storyboard resource: Animates a rectangle's opacity. -->
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation
              Storyboard.TargetName="MyAnimatedRectangle"
              Storyboard.TargetProperty="Opacity"
              From="1.0" To="0.0" Duration="0:0:1"/>
        </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <StackPanel>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </StackPanel>
</Page>
```

若要組織 XAML 中的索引資源，一般做法是在 XAML 檔案 (例如 page.xaml 或 app.xaml) 的 XAML 根項目定義資源。 您也可以把資源分到不同的檔案，然後將它們合併到應用程式或頁面中。 如需詳細資訊，請參閱 [ResourceDictionary 與 XAML 資源參考](https://msdn.microsoft.com/library/windows/apps/Mt187273)。

**注意** Windows 執行階段 XAML 可支援使用 [x:Key](https://msdn.microsoft.com/library/windows/apps/Mt204787) 屬性 或 [x:Name](https://msdn.microsoft.com/library/windows/apps/Mt204788) 屬性識別資源。 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 較常使用 x:Name 屬性，因為您最後還是希望依變數名稱參考它，以便可以呼叫它的 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 方法和執行動畫。 如果您使用 [x:Key](https://msdn.microsoft.com/library/windows/apps/Mt204787) 屬性，就需要使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 方法 (例如 [**Item**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.item) 索引子) 抓取它做為索引資源，然後將抓取的物件轉換為 **Storyboard** 以使用 **Storyboard** 方法。

 

直到為控制項視覺外觀宣告視覺狀態動畫之前，您也可以將動畫放置在 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 單元內。 在該情況下，您定義的 **Storyboard** 元素會進入 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 容器中，它會在 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) 更深層巢狀中 (這是索引鍵資源 **Style**)。 在這種情況下，您的 **Storyboard** 不需要索引鍵或名稱，因為它是 **VisualState**，具有 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) 可以叫用的目標名稱。 控制項的樣式通常會分到不同的 XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 檔案中，而不是放置在頁面或應用程式 **Resources** 集合內。 如需詳細資訊，請參閱[視覺狀態的腳本動畫](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

## 相依式和獨立式動畫

現在，我們需要介紹動畫系統運作方式的一些重點。 尤其是，動畫與 Windows 執行階段應用程式如何呈現到畫面的基本互動，以及該呈現使用處理執行緒的方式。 Windows 執行階段應用程式都有一個主要 UI 執行緒，這個執行緒負責以目前的資訊更新畫面。 此外，Windows 執行階段應用程式還有一個合成執行緒，用於在顯示配置之前立即預先計算。 當您設定 UI 的動畫效果時，可能會為 UI 執行緒產生大量的工作。 系統必須在每次重新整理的極短時間間隔內重繪大量的畫面區域。 這對於擷取動畫屬性最新的屬性值來說是必要的工作。 如果不小心處理，動畫很有可能使 UI 回應速度變慢，或影響位於相同 UI 執行緒上其他應用程式功能的效能。

有可能使 UI 執行緒變慢的各種動畫稱為「*相依式動畫*」， 沒有前述可能的動畫稱為「*獨立式動畫*」。 相依式和獨立式動畫的區別不只在於我們前面所說明的動畫類型 ([**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 等等)。 而是，在於您所設定動畫效果的特定屬性，以及控制項的繼承和組合等其他因素。 也有可能即使動畫變更了 UI，但動畫對於 UI 執行緒只有極小的影響，而且可以改由合成執行緒做為獨立式動畫處理。

如果動畫具備以下任一特性，就是獨立式動畫：

-   動畫的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 為 0 秒 (請參閱警告)
-   動畫針對 [**UIElement.Opacity**](https://msdn.microsoft.com/library/windows/apps/BR208962)
-   動畫針對這些 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 屬性的子屬性值：[**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)、[**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)、[**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection.aspx)、[**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)
-   動畫針對 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) 或 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772)
-   動畫針對 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 值且使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)，設定其 [**Color**](https://msdn.microsoft.com/library/windows/apps/BR242963) 的動畫效果
-   動畫是 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)

**警告** 如果要讓您的動畫被視為是獨立式動畫，您必須明確地設定 `Duration="0"`。 例如，如果您從這個 XAML 移除 `Duration="0"`，動畫都會被視為是相依式動畫，即使畫面的 [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR243169) 是「0:0:0」。

 

```xml
<Storyboard>
    <DoubleAnimationUsingKeyFrames
         Duration="0"
         Storyboard.TargetName="Button2"
         Storyboard.TargetProperty="Width">
         <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200" />
     </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

如果您的動畫不符合這些條件，就可能是相依式動畫。 根據預設，動畫系統不會執行相依式動畫。 所以在開發和測試的程序期間，您可能不會看到您的動畫執行。 您還是可以使用這種動畫，但必須特別啟用每一個相依式動畫。 若要啟用動畫，將動畫物件的 **EnableDependentAnimation** 屬性設定為 **true** (代表動畫的每一個 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 子類別都有不同的屬性實作，但都命名為 `EnableDependentAnimation`)。

對於使用相依式動畫的需求，應用程式開發人應謹慎構思動畫系統和開發經驗的設計概念。 我們希望開發人員注意到動畫對於 UI 的回應速度確實有效能方面的影響。 在全方位的應用程式中，很難針對效能不佳的動畫隔離和偵錯。 所以，最好只開啟應用程式的 UI 體驗確實需要的相依式動畫。 我們不希望因為使用大量循環的裝飾動畫而輕易地降低應用程式的效能。 如需動畫效能祕訣的詳細資訊，請參閱[最佳化動畫和媒體](https://msdn.microsoft.com/library/windows/apps/Mt204774)。

做為應用程式開發人員，您也可以選擇套用一律停用相依式動畫的全應用程式設定，即使 **EnableDependentAnimation** 為 **true** 也可以。 請參閱 [**Timeline.AllowDependentAnimations**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.allowdependentanimations)。

**祕訣** 如果您使用 Visual Studio 撰寫控制項的視覺狀態，設計工具就會在您嘗試將相依式動畫套用到視覺狀態屬性時產生警告。

 

## 啟動和控制動畫

到目前為止，我們說明的所有內容都還不能讓動畫實際執行或套用！ 在動畫啟動和執行之前，動畫在 XAML 中宣告的值變更都還看不見，還不會發生。 您必須以應用程式週期或使用者經驗相關的一些方式明確啟動動畫。 以最簡單的方式來說，您可以在 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) (這是該動畫的父項) 上呼叫 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 方法，啟動動畫。 您無法直接從 XAML 呼叫方法，所以無論如何啟用動畫，都要從程式碼啟用。 可能是頁面的程式碼後置或應用程式的元件，如果您是定義自訂控制項類別，也可能是控制項的邏輯。

通常，您會呼叫 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin)，並讓動畫執行到持續時間結束。 但是，您也可以使用 [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx)、[**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) 和 [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) 方法，在執行階段控制 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)，還可以使用更進階的動畫控制項案例適用的其他 API。

當您在包含無限重複的動畫 (`RepeatBehavior="Forever"`) 的腳本上呼叫 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 時，動畫會執行到包含它的頁面卸載或您特別呼叫 [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) 或 [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) 為止。

### 從應用程式程式碼啟動動畫

您可以自動啟動動畫，或回應使用者的動作啟動動畫。 如果是自動啟動，通常會使用物件存留期事件 (例如 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723)) 做為動畫觸發程序。 **Loaded** 事件就是一個適合這個目的使用的事件，因為這個時候 UI 已經準備好進行互動，而且因為 UI 的另一個部分還在載入，所以一開始時，動畫不會被截斷。

在這個範例中，[**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed) 事件會附加到矩形，當使用者按一下矩形時，動畫就會開始。

```xml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
  ```

事件處理常式會使用 **Storyboard** 的 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 方法來啟動 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) (動畫)。

> [!div class="tabbedCodeSnippets"]
``` csharp
myStoryboard.Begin();
```
``` cpp
myStoryboard->Begin();
```
``` vb
myStoryBoard.Begin()
```

如果您想要動畫在完成套用值之後執行其他邏輯，可以處理 [**Completed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.completed.aspx) 事件。 此外，對於疑難排解屬性系統/動畫互動，[**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/BR242358) 方法很有用。

**祕訣** 當您為從應用程式程式碼啟動動畫的應用程式案例撰寫程式碼時，可能希望再次檢查動畫或轉換是否已經在您 UI 案例的動畫庫中。 動畫庫的動畫可以為所有 Windows 執行階段應用程式提供更一致的 UI 體驗，且更容易使用。

 

### 視覺狀態的動畫

用來定義控制項視覺狀態的 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 的執行行為和應用程式直接執行腳本的方式有所不同。 就像套用到 XAML 的視覺狀態定義，**Storyboard** 是包含 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 的元素，而整體狀態是使用 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) API 所控制。 其中的所有動畫都會在控制項使用包含的 **VisualState** 時，根據它們的動畫值和 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 屬性執行。 如需詳細資訊，請參閱[視覺狀態的腳本](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。 至於視覺狀態，顯示的 [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) 並不相同。 如果視覺狀態變更為其他狀態，即使新的視覺狀態並未特別將新動畫套用到屬性，仍會取消先前視覺狀態及其動畫所套用的所有屬性變更。

### **Storyboard** 和 **EventTrigger**

有一種方法可以啟動完全以 XAML 宣告的動畫。 不過，這項技術已不被廣泛使用。 它是來自 WPF 和 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) 支援前之 Silverlight 早期版本的舊版語法。 由於匯入/相容性的需要，這個 [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) 語法仍然可以在 Windows 執行階段 XAML 中運作，但僅能用於以 [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) 事件為依據的觸發程式行為；嘗試觸發其他事件會擲回例外狀況或無法編譯。 如需詳細資訊，請參閱 [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) 或 [**BeginStoryboard**](https://msdn.microsoft.com/library/windows/apps/BR243053)。

## 讓 XAML 附加屬性產生動畫效果

這不是常見的案例，但是您可以將動畫值套用到 XAML 附加屬性。 如需何謂附加屬性及其運作方式的詳細資訊，請參閱[附加屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185579)。 設定附加屬性目標需要使用 [property-path 語法](https://msdn.microsoft.com/library/windows/apps/Mt185586)，此語法可將屬性名稱包在括號內。 您可以使用套用不連續整數值的 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/Hh759773)，讓 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/BR210320) 之類的內建附加屬性產生動畫效果。 不過，目前 Windows 執行階段 XAML 實作的侷限是您無法讓自訂附加屬性產生動畫效果。

## 其他動畫類型以及了解設定 UI 動畫效果的後續步驟

到目前為止，我們已經說明了在兩個值之間產生動畫效果的自訂動畫，以及在動畫執行時視需要以線性方式插入值。 這些稱為 **From**/**To**/**By** 動畫。 但是還有另一種動畫類型，可讓您宣告位於開始和結束之間的中繼值。 這些稱為「*主要畫面格動畫*」。 還有一種方式可以改變 **From**/**To**/**By** 動畫或主要畫面格動畫上的內插補點邏輯。 這牽涉到套用 Easing 函式。 如需這些概念的詳細資訊，請參閱[主要畫面格和 Easing 函式動畫](key-frame-and-easing-function-animations.md)。

## 相關主題

* [Property-path 語法](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [相依性屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [主要畫面格和 Easing 函式動畫](key-frame-and-easing-function-animations.md)
* [視覺狀態的腳本動畫](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)
* [控制項範本](https://msdn.microsoft.com/library/windows/apps/Mt210948)
* [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)
 

 







<!--HONumber=Jul16_HO2-->


