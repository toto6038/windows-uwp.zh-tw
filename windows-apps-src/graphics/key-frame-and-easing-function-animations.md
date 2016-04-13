---
主要畫面格動畫和 Easing 函式動畫
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
線性主要畫面格動畫、含 KeySpline 值的主要畫面格動畫或 Easing 函式是用於類似情況的三種不同技術。
---
# 主要畫面格動畫和 Easing 函式動畫

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


線性主要畫面格動畫、含 **KeySpline** 值的主要畫面格動畫或 Easing 函式是用於類似情況的三種不同技術：建立腳本動畫比較複雜，要使用從開始狀態到結束狀態的非線性動畫行為。

## 先決條件

務必先閱讀＜[腳本動畫](storyboarded-animations.md)＞主題。 這個主題是以＜[腳本動畫](storyboarded-animations.md)＞中所說明的動畫概念為基礎，之後不會再重述這些概念。 例如，＜[腳本動畫](storyboarded-animations.md)＞會說明如何以動畫、腳本做為目標資源，[**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 屬性值，例如 [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration)、[**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior) 等等。

## 使用主要畫面格動畫設定動畫效果

主要畫面格動畫允許多個目標值，這些值會在動畫時間軸的某個時間到達。 換句話說，每個主要畫面格都可以指定不同的中繼值，最後一個到達的主要畫面格就是最終的動畫值。 在動畫中指定多個值可以製作更複雜的動畫。 主要畫面格動畫也啟用不同的內插補點邏輯，每個邏輯依據動畫類型實作為不同的 **KeyFrame** 子類別。 具體而言，每個主要畫面格動畫類型具有其 **KeyFrame** 類別的 **Discrete**、**Linear**、**Spline** 和 **Easing** 變化，用於指定其主要畫面格。 例如，若要指定針對 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 且使用主要畫面格的動畫，您可以使用 [**DiscreteDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243130)、[**LinearDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210316)、[**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446) 和 [**EasingDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210269) 宣告主要畫面格。 您可以使用單一 **KeyFrames** 集合內的任一和所有這些類型，在每一次有新的主要畫面格到達時變更內插補點。

對內插補點行為而言，每個主要畫面格都會控制內插補點，直到它的 **KeyTime** 時間到達為止。 它的 **Value** 也會在該時間到達。 如果還有更多的主要畫面格，此值就會成為序列中下一個主要畫面格的開始值。

動畫開始時，如果沒有 **KeyTime** 為 "0:0:0" 的主要畫面格，開始值會是屬性的任何非動畫值。 這種情況和 **From**/**To**/**By** 動畫在沒有 **From** 時的運作方式類似。

主要畫面格動畫的持續時間暗示此持續時間等於任一主要畫面格中設定的最高 **KeyTime** 值。 您可以視需要設定明確的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration)，但請注意，不可短於您自己的主要畫面格中的 **KeyTime**，否則會截斷部分動畫。

除了 [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) 之外，您可以在主要畫面格動畫上設定所有以 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 為基礎的屬性，和使用 **From**/**To**/**By** 動畫一樣，因為主要畫面格動畫類別也是從 **Timeline** 衍生而來。 這些分別是：

-   [
            **AutoReverse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.autoreverse)：到達最後一個主要畫面格之後，會從最後開始以反向順序重複畫面格。 這會加倍動畫的顯示持續時間。
-   [
            **BeginTime**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.begintime)：延遲開始動畫。 畫面格中 **KeyTime** 值的時間軸會在到達 **BeginTime** 時才開始計數，所以不會截斷畫面格。
-   [
            **FillBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.fillbehavior)：控制到達最後一個主要畫面格時發生的情況。 **FillBehavior** 在任何中繼主要畫面格上都沒有作用。
-   [
            **RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty)：
    -   如果設定為 **Forever**，那麼主要畫面格及其時間軸會無限重複。
    -   如果設定為某個反覆運算計數，時間軸會重複該次數。
    -   如果設定為 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)，時間軸會重複到該時間到達為止。 如果它不是時間軸隱含持續時間的整數因素，這可能會在主要畫面格序列的中途截斷動畫。
-   [
            **SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.speedratioproperty) (不常用)

### 線性主要畫面格

線性主要畫面格會產生值的簡單線性內插補點，直到畫面格的 **KeyTime** 到達為止。 這個內插補點行為和＜[腳本動畫](storyboarded-animations.md)＞主題中說明的較簡單 **From**/**To**/**By** 動畫最為類似。

以下說明如何使用主要畫面格動畫的線性主要畫面格調整矩形的呈現高度。 這個範例所執行的動畫，在前 4 秒以線性方式微幅增加矩形的高度，然後在最後迅速調整，直到矩形高度成為開始時的兩倍為止。

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### 離散主要畫面格

離散主要畫面格完全不使用任何內插補點。 當 **KeyTime** 到達時，就直接套用新的 **Value**。 視製作成動畫的 UI 屬性而定，這通常會產生看起來像在「跳躍」的動畫。 請確定這真的是您所想要的美學行為。 您可以增加宣告的主要畫面格數量以減少這種明顯的跳躍，但如果順暢的動畫才是您的目標，最好改用線性或曲線主要畫面格。

**注意** 離散主要畫面格是將含有 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) 的 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 和 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 類型以外的值製作成動畫的唯一方式。 本主題稍後會更詳細的討論這個部分。

 

### 曲線主要畫面格

曲線主要畫面格會根據 **KeySpline** 屬性的值，在值之間建立可變動的轉換。 這個屬性會指定貝茲曲線的第一個和第二個控制點，以說明動畫的加速方式。 基本上，[**KeySpline**](https://msdn.microsoft.com/library/windows/apps/BR210307) 會定義一段時間的函式關係，而此函式-時間圖形就是該貝茲曲線的形狀。 通常您會在 XAML shorthand 屬性字串中指定 **KeySpline** 值，共有四個 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值，以空格或逗號分隔。 這些值是貝茲曲線兩個控制點的 "X,Y" 組。 "X" 是時間，而 "Y" 是值的函式修飾詞。 每個值都應該介於 0 到 1 (含) 之間。 如果沒有對 **KeySpline** 進行控制點修改，則從 0,0 到 1,1 的直線就代表線性內插補點一段時間的函式。 您的控制點會變更該曲線的形狀，因此變更曲線動畫一段時間的函式行為。 這要以圖形來看最為清楚。 您可以在瀏覽器執行 [Silverlight 主要曲線視覺化檢視範例](http://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample)，看看控制點如何修改曲線，以及用它當作 **KeySpline** 值時，範例動畫如何執行。

下一個範例示範套用到動畫的三個不同主要畫面格，最後一個是 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值 ([**SplineDoubleKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR210446)) 的主要曲線動畫。 請注意 **KeySpline** 所套用的字串 "0.6,0.0 0.9,0.00"。 這會產生一個曲線，動畫一開始看起來執行得很慢，但會在 **KeyTime** 到達之前迅速到達該值。

```xml
<Storyboard x:Name="myStoryboard">

            <!-- Animate the TranslateTransform's X property
             from 0 to 350, then 50,
             then 200 over 10 seconds. -->
            <DoubleAnimationUsingKeyFrames
          Storyboard.TargetName="MyAnimatedTranslateTransform"
          Storyboard.TargetProperty="X"
          Duration="0:0:10" EnableDependentAnimation="True">

                <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
                 steadily from its starting position to 500 over 
                 the first 3 seconds.  -->
                <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

                <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
                 appears at 400 after the fourth second of the animation. -->
                <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

                <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
                 back to its starting point. The
                 animation starts out slowly at first and then speeds up. 
                 This KeyFrame ends after the 6th
                 second. -->
                <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>

            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
        ```

### Easing key frames

An easing key frame is a key frame where interpolation being applied, and the function over time of the interpolation is controlled by several pre-defined mathematical formulas. You can actually produce much the same result with a spline key frame as you can with some of the easing function types, but there are also some easing functions such as [**BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) that you can't reproduce with a spline.

To apply an easing function to an easing key frame, you set the **EasingFunction** property as a property element in XAML for that key frame. For the value, specify an object element for one of the easing function types.

This example applies a [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126) and then a [**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) as successive key frames to a [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) to create a bouncing effect.

```xml
<Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames Duration="0:0:10"
             Storyboard.TargetProperty="Height"
             Storyboard.TargetName="myEllipse">

                <!-- This keyframe animates the ellipse up to the crest 
                     where it slows down and stops. -->
                <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
                    <EasingDoubleKeyFrame.EasingFunction>
                        <CubicEase/>
                    </EasingDoubleKeyFrame.EasingFunction>
                </EasingDoubleKeyFrame>

                <!-- This keyframe animates the ellipse back down and makes
                     it bounce. -->
                <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
                    <EasingDoubleKeyFrame.EasingFunction>
                        <BounceEase Bounces="5"/>
                    </EasingDoubleKeyFrame.EasingFunction>
                </EasingDoubleKeyFrame>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
```

這只是一個 Easing 函式範例， 下一節會有更詳細的說明。

## Easing 函式

Easing 函式可讓您將自訂的數學公式套用至動畫。 對於製作以 2-D 座標系統模擬真實世界物理的動畫來說，數學運算操作通常很有用。 例如，您可以讓物件寫實地彈跳，或是表現得就像放在彈簧上面。 您可以使用主要畫面格或甚至是 **From**/**To**/**By** 動畫來模擬這些效果，但工作量會大幅增加，而且動畫無法像使用數學公式一樣精準。

Easing 函式可用三種方式套用到動畫：

-   如先前小節所述，在主要畫面格動畫中使用 Easing 主要畫面格。 使用 [**EasingColorKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210267)、[**EasingDoubleKeyFrame.EasingFunction**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction.aspx) 或 [**EasingPointKeyFrame.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210279)。
-   在其中一個 **From**/**To**/**By** 動畫類型設定 **EasingFunction** 屬性。 使用 [**ColorAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR243075)、[**DoubleAnimation.EasingFunction**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) 或 [**PointAnimation.EasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR210354)。
-   將 [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) 設定為 [**VisualTransition**](https://msdn.microsoft.com/library/windows/apps/BR209034) 的一部分。 這是專用於定義控制項的視覺狀態；如需詳細資訊，請參閱 [**GeneratedEasingFunction**](https://msdn.microsoft.com/library/windows/apps/BR209037) 或[視覺狀態的腳本](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

以下是 Easing 函式的清單：

-   [
            **BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049)：稍微倒退動畫的動作，再開始沿指示的路徑開始動畫。
-   [
            **BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057)：建立彈跳效果。
-   [
            **CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063)：使用循環函式建立加速或減速的動畫。
-   [
            **CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126)：使用公式 f(t) = t3 建立加速或減速的動畫。
-   [
            **ElasticEase**](https://msdn.microsoft.com/library/windows/apps/BR210282)：建立類似來回振動直到停止的彈簧動畫。
-   [
            **ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294)：使用指數公式建立加速或減速的動畫。
-   [
            **PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399)：使用公式 f(t) = tp (其中 p 等於 [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) 屬性) 建立加速或減速的動畫。
-   [
            **QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403)：使用公式 f(t) = t2 建立加速或減速的動畫。
-   [
            **QuarticEase**](https://msdn.microsoft.com/library/windows/apps/BR210405)：使用公式 f(t) = t4 建立加速或減速的動畫。
-   [
            **QuinticEase**](https://msdn.microsoft.com/library/windows/apps/BR210407)：使用公式 f(t) = t5 建立加速或減速的動畫。
-   [
            **SineEase**](https://msdn.microsoft.com/library/windows/apps/BR210439)：使用正弦公式建立加速或減速的動畫。

有些 Easing 函式有自己的屬性。 例如，[**BounceEase**](https://msdn.microsoft.com/library/windows/apps/BR243057) 有兩個屬性 [**Bounces**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounces.aspx) 和 [**Bounciness**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.media.animation.bounceease.bounciness.aspx)，可修改該特定 **BounceEase** 的 function-over-time 行為。 其他 Easing 函式 (例如 [**CubicEase**](https://msdn.microsoft.com/library/windows/apps/BR243126)) 並沒有所有 Easing 函式共用之 [**EasingMode**](https://msdn.microsoft.com/library/windows/apps/BR210275) 屬性以外的屬性，而且一律產生相同的 function-over-time 行為。

視您在具備屬性的 Easing 函式上設定屬性的方式而定，這些 Easing 函式中有部分會有一些重疊。 例如，[**QuadraticEase**](https://msdn.microsoft.com/library/windows/apps/BR210403) 和 [**PowerEase**](https://msdn.microsoft.com/library/windows/apps/BR210399) 完全一樣，它們的 [**Power**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.powerease.power) 都等於 2。 而 [**CircleEase**](https://msdn.microsoft.com/library/windows/apps/BR243063) 基本上是預設值 [**ExponentialEase**](https://msdn.microsoft.com/library/windows/apps/BR210294)。

[
            **BackEase**](https://msdn.microsoft.com/library/windows/apps/BR243049) Easing 函式是獨一無二的，因為它可以變更 **From**/**To** 或主要畫面格值所設定的一般範圍以外的值。 它啟動動畫的方式是以一般 **From**/**To** 行為的相反方向變更值，回到 **From** 或再次開始值，然後按一般方式執行動畫。

在先前的範例中，我們示範了如何宣告主要畫面格動畫的 Easing 函式。 下一個範例會將 Easing 函式套用到 **From**/**To**/**By** 動畫。

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

Easing 函式套用到 **From**/**To**/**By** 動畫時，它會變更如何在動畫之 [**Duration**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.duration) 上的 **From** 和 **To** 值之間插入值的 function- over-time 特性。 如果沒有 Easing 函式，這會是一個線性內插補點。

## <span id="Discrete_object_value_animations"> </span> <span id="discrete_object_value_animations"> </span> <span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"> </span>離散物件值動畫

這是值得特別提及的動畫，因為這是唯一的一種方式，讓您可以將動畫值套用到 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 或 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 類型以外的屬性。 這是主要畫面格動畫 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)。 使用 [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) 值製作動畫有所不同，因為無法在畫面格之間插入值。 當畫面格的 [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR210342) 到達時，動畫值會立即設定為主要畫面格的 **Value** 中指定的值。 因為沒有內插補點，所以在 **ObjectAnimationUsingKeyFrames** 主要畫面格集合中只使用一個主要畫面格：[**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)。

[
            **DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132) 的 [**Value**](https://msdn.microsoft.com/library/windows/apps/BR210344) 通常使用屬性 (Property) 元素語法進行設定，因為您嘗試設定的物件值通常無法以字串的形式表示，所以無法在屬性語法中填入 **Value**。 如果您使用 [StaticResource](https://msdn.microsoft.com/library/windows/apps/Mt185588) 這類的參考，還是可以使用屬性語法。

您會在一個地方看到預設範本使用 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320)，那就是當範本屬性參考 [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) 資源。 這些資源是 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 物件，不僅只是 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723) 值，它們會使用定義為系統佈景主題的資源 ([**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/BR208807))。 它們可以直接指派給 **Brush** 類型的值，例如 [**TextBlock.Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665)，而且不需要使用間接目標。 但因為 **SolidColorBrush** 不是 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) 或 **Color**，所以您必須使用 **ObjectAnimationUsingKeyFrames** 才能使用資源。

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="Button">
                    <Grid Background="Transparent">
                        <TextBlock x:Name="Text"
                            Text="{TemplateBinding Content}"/>
                        <VisualStateManager.VisualStateGroups>
                            <VisualStateGroup x:Name="CommonStates">
                                <VisualState x:Name="Normal"/>
                                <VisualState x:Name="PointerOver">
                                    <Storyboard>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                        </ObjectAnimationUsingKeyFrames>
                                    </Storyboard>
                                </VisualState>
                                <VisualState x:Name="Pressed">
                                    <Storyboard>
                                        <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                            <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                        </ObjectAnimationUsingKeyFrames>
                                    </Storyboard>
                                </VisualState>
...
                           </VisualStateGroup>
                        </VisualStateManager.VisualStateGroups>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

You also might use [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) to animate properties that use an enumeration value. Here's another example from a named style that comes from the Windows Runtime default templates. Note how it sets the [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) property that takes a [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR209006) enumeration constant. In this case you can set the value using attribute syntax. You only need the unqualified constant name from an enumeration for setting a property with an enumeration value, for example "Collapsed".

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

一個 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) 畫面格組可以使用多個 [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/BR243132)。 透過讓 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760) 的值產生動畫效果來建立「投影片放映」動畫是個很有趣的方式，可用來舉例說明適用多個物件值的案例。

 ## 相關主題

* [Property-path 語法](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [相依性屬性概觀](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)
 

 






<!--HONumber=Mar16_HO1-->


