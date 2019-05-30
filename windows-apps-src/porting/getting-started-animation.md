---
title: 動畫入門
ms.assetid: C1C3F5EA-B775-4700-9C45-695E78C16205
description: 在這個專案中，我們會移動一個矩形，套用淡出效果，然後再將它帶回檢視中。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b30f2e9d08fd36686045523c54180829570cbd2d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372877"
---
# <a name="getting-started-animation"></a>開始使用：動畫


## <a name="adding-animations"></a>新增動畫

在 iOS 中，您經常會以程式設計的方式來建立動畫效果。 例如，您可以使用區塊型 **UIView** 類別的 **animateWithDuration** 方法或舊版非區塊型方法所提供的動畫。 或者，您可以明確使用 **CALayer** 類別將圖層製作成動畫。 Windows 應用程式中的動畫也可以透過程式設計的方式建立，但是也可以透過 Extensible Application Markup Language (XAML) 以宣告的方式進行定義。 您可以使用 Microsoft Visual Studio 直接編輯 XAML 程式碼，但是 Visual Studio 另外隨附了一個稱為 **Blend** 的工具，當您在設計工具中處理動畫時，它可以為您建立 XAML 程式碼。 事實上，Blend 讓您能夠以圖形方式開啟、設計、建置以及執行完整的 Visual Studio 專案。 下列逐步解說可讓您嘗試一下。

建立一個新的通用 Windows 平台(UWP) app，並將它命名為「SimpleAnimation」之類的名稱。 在這個專案中，我們會移動一個矩形，套用淡出效果，然後再將它帶回檢視中。 XAML 中的動畫是以「*腳本*」(請勿與 iOS 腳本混淆) 的概念為基礎。 腳本使用「*主要畫面格*」產生屬性變更的動畫效果。

在專案開啟的時候，於 [**方案總管**] 中，以滑鼠右鍵按一下專案名稱，然後點取 [**在 Blend 中開啟**] 或 [**在 Blend 中設計**]，如下圖中所示。 Visual Studio 會繼續在背景執行。

![[在 Blend 中開啟] 功能表命令](images/ios-to-uwp/vs-open-in-blend.png)

Blend 啟動之後，您應該會看到和下圖類似的畫面。

![Blend 開發環境](images/ios-to-uwp/blend-1.png)

於左側的 [**方案總管**] 視窗中按兩下 **MainPage.xaml**。 接下來，在中央邊緣的 [**設計檢視**] 的垂直帶狀工具中，選取 [**矩形**] 工具，然後在 [**設計檢視**] 中繪製一個矩形，如下圖中所示。

![將矩形新增到設計檢視](images/ios-to-uwp/blend-2.png)

若要讓矩形填滿綠色，請看到 [**屬性**] 視窗的 [**筆刷**] 區域，按一下 [**單色筆刷**] 按鈕，然後按一下 [**色彩滴管**] 圖示。 按一下綠色色調區段的任意位置。

若要開始建立矩形的動畫效果，在 [**物件與時間軸**] 視窗中，點選加號 ([**新增**]) 按鈕，如下圖中所示，然後點選 [**確定**]。

![新增腳本](images/ios-to-uwp/blend-3.png)

[**物件與時間軸**] 視窗中就會顯示腳本 (您可能需要調整檢視，才能正確看到腳本)。 [**設計檢視**] 會顯示變更，以顯示 [**Storyboard1 時間軸錄製中**]。 若要捕捉矩形的目前狀態，請在 [**物件與時間軸**] 視窗中，點選黃色箭頭正上方的 [**錄製主要畫面格**] 按鈕，如下圖中所示。

![錄製主要畫面格](images/ios-to-uwp/blend-4.png)

現在我們要移動矩形並讓它淡出。 若要這樣做，將橘免/黃色箭頭拖曳到 2 秒的位置，然後將矩形向右移一點點。 接下來，在 [**屬性**] 視窗的 [**外觀**] 區域中，將 [**不透明度**] 屬性變更為 [**0**]，如下圖中所示。 若要預覽動畫，請點選 [腳本] 面板中的 [**播放**] 按鈕。

![[屬性] 視窗及 [播放] 按鈕](images/ios-to-uwp/blend-5.png)

接下來，我們將矩形帶回檢視中。 在 [**物件與時間軸**] 視窗中，按兩下 [**Storyboard1**]。 然後，在 [**屬性**] 視窗的 [**通用**] 區域中，選取 [**自動反轉**]，如下圖中所示。

![選取腳本](images/ios-to-uwp/blend-6.png)

最後，按一下 [**播放**] 按鈕看看發生了什麼事。

您可以按一下視窗頂端的綠色執行按鈕 (或只按下 F5) 來建置和執行專案。 如果您這樣做，您將看到您的專案會確實地建置和執行，但綠色矩形卻動也不動，就像在超市走道看到糖果賴著不走的小孩一樣。 若要開始動畫，則需要在專案新增一行程式碼。 方法如下。

開啟 [**檔案**] 功能表，然後選取 [**儲存 MainPage.xaml**] 來儲存專案。 返回 Visual Studio。 如果 Visual Studio 顯示一個對話方塊，詢問您是否要重新載入已修改過的檔案，請選取 [**是**]。 按兩下隱藏在 **MainPage.xaml** 底下的 **MainPage.xaml.cs** 檔案以將它開啟，並在 public MainPage() 方法正上方加入下列程式碼：

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Add the following line of code.
    Storyboard1.Begin();
}
```

重新執行專案，看看矩形的動畫效果。 萬歲！

如果開啟 MainPage.xaml 檔案，在 [**XAML**] 檢視中，您會看到當您在設計工具中工作時 Blend 為您新增的 XAML 程式碼。 請特別看看 `<Storyboard>` 和 `<Rectangle>` 元素中的程式碼。 下列程式碼顯示範例。 橢圓形表示為簡潔而省略的不相關程式碼；為了便於閱讀程式碼，我們加入了斷行符號。

```xml
...
<Storyboard 
        x:Name="Storyboard1" 
        AutoReverse="True">
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateX)"
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="185.075"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateY)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="2.985"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.Opacity)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="1"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2"
                Value="0"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
...
<Rectangle 
        x:Name="rectangle" 
        Fill="#FF00FF63" 
        HorizontalAlignment="Left" 
        Height="122" 
        Margin="151,312,0,0" 
        Stroke="Black" 
        VerticalAlignment="Top" 
        Width="239" 
        RenderTransformOrigin="0.5,0.5">
    <Rectangle.RenderTransform>
        <CompositeTransform/>
    </Rectangle.RenderTransform>
</Rectangle>
...
```

您可以以手動方式編輯這個 XAML，或返回 Blend 以繼續處理工作。 Blend 會以好玩的方式建立有趣的使用者介面，而使用圖形工具來製作介面動畫的功能將會大幅縮短開發時間。 如需動畫的詳細資訊，請參閱[動畫概觀](https://docs.microsoft.com/windows/uwp/graphics/animations-overview)。

**附註**  如需動畫的詳細資訊<span class="legacy-term">使用 JavaScript 和 HTML 的 UWP 應用程式</span>，請參閱[以動畫顯示您的 UI (HTML)](https://docs.microsoft.com/previous-versions/windows/apps/hh465165(v=win.10))。

### <a name="next-step"></a>後續步驟

[開始使用：接下來呢？](getting-started-what-next.md)
