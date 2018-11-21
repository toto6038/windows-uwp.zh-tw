---
author: Jwmsft
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: XAML UI 的 3D 透視效果
description: 您可以使用透視轉換將 3D 效果套用到 Windows 執行階段 App 中的內容。 例如，您可以建立物件轉近或轉離的動畫效果，如這裡所示。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f0b50ca4cb3f74c7c8e8bccb4fceb58e0151bcf2
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434044"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>XAML UI 的 3D 透視效果


您可以使用透視轉換將 3D 效果套用到 Windows 執行階段 App 中的內容。 例如，您可以建立物件轉近或轉離的動畫效果，如這裡所示。

![透視轉換的影像](images/3dsimple.png)

透視轉換的另一個常見用法，就是將物件排列在一起以建立 3D 效果，如下所示。

![堆疊物件以建立 3D 效果](images/3dstacking.png)

除了建立靜態 3D 效果，您也可以動畫顯示透視轉換屬性，以建立移動中的 3D 效果。

前述範例是套用至影像的透視轉換，而您可以將這些效果套用至任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)，包括控制項。 例如，您可以將一種 3D 效果套用到整個控制項容器，如下所示：

![套用 3D 效果的元素容器](images/skewedstackpanel.png)

以下是建立此範例所用的 XAML 程式碼：

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

以下我們將焦點放在 [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) 屬性，其用途是在 3D 空間中旋轉和移動物件。 您可以在下一個範例中實驗這些屬性，查看它們對物件產生的效果。

## <a name="planeprojection-class"></a>PlaneProjection 類別

使用 [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) 設定 UIElement 的 [**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection) 屬性，即可將 3D 效果套用至任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911)。 **PlaneProjection** 定義如何在空間中呈現轉換。 以下範例顯示一個簡單情況。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

此圖表顯示影像呈現的情形。 x 軸、y 軸以及 z 軸以紅線顯示。 此影像是使用 [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) 屬性繞著 x 軸往後旋轉 35 度。

![RotateX 減 35 度](images/3drotatexminus35.png)

[**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) 屬性是繞著旋轉中心的 y 軸轉動。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY 減 35 度](images/3drotateyminus35.png)

[**RotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationz) 屬性是繞著旋轉中心的 z 軸轉動 (與物件平面呈直角的一條線)。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ 減 45 度](images/3drotatezminus35.png)

旋轉屬性可以指定正值或負值，朝正向或反向旋轉。 絕對數字可以大於 360，這會完全旋轉物件一圈以上。

您可以使用 [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx)、[**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) 以及 [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz) 屬性來移動旋轉中心。 根據預設，旋轉軸會直接穿過物件中心，導致物件繞著本身中心旋轉。 但是如果您將旋轉中心移出物件外部邊緣，物件就會繞著該邊緣旋轉。 **CenterOfRotationX** 與 **CenterOfRotationY** 的預設值是 0.5，**CenterOfRotationZ** 的預設值是 0。 針對 **CenterOfRotationX** 與 **CenterOfRotationY**，介於 0 與 1 的數值會將樞紐點設在物件內部的某個位置。 0 代表物件某一邊緣，1 代表正對面的另一邊緣。 值可以超出此範圍，並會據此移動旋轉中心。 因為旋轉中心 z 軸的繪製方式是穿透物件平面，所以您可以使用負值在物件背面移動旋轉中心，或使用正值在物件前面 (朝向您) 移動旋轉中心。

[**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) 會沿著與物件平行的 X 軸移動旋轉中心，而 [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) 則是沿著物件 Y 軸移動旋轉中心。 下一個圖例示範 **CenterOfRotationY** 使用不同值的效果。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.5" (預設值)**

![CenterOfRotationY 等於 0.5](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.1"**

![CenterOfRotationY 等於 0.1](images/3dcenterofrotationy0point1.png)

請注意 [**CenterOfRotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationy) 屬性為預設值 0.5 時影像繞著中心旋轉的方式，而為 0.1 時影像在上邊緣附近旋轉的方式。 在變更 [**CenterOfRotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationx) 屬性以移動 [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) 屬性旋轉物件的位置時，會有類似行為。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.5" (預設值)**

![CenterOfRotationX 等於 0.5](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.9" (右側邊緣)**

![CenterOfRotationX 等於 0.9](images/3dcenterofrotationx0point9.png)

使用 [**CenterOfRotationZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.centerofrotationz) 將旋轉中心放在物件平面的下方或上方。 如此一來，物件繞著某點旋轉的方式，就如同行星圍繞恆星運行一般。

## <a name="positioning-an-object"></a>放置物件

目前為止，您已學會如何在空間中旋轉物件。 使用下列屬性可以將旋轉物件放在彼此之間的相對位置：

-   [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx) 使物件沿著旋轉物件平面的 x 軸移動。
-   [**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) 使物件沿著旋轉物件平面的 y 軸移動。
-   [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) 使物件沿著旋轉物件平面的 z 軸移動。
-   [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx) 使物件沿著與螢幕對齊的 x 軸移動。
-   [**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) 使物件沿著與螢幕對齊的 y 軸移動。
-   [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) 使物件沿著與螢幕對齊的 z 軸移動。

**區域位移**

[**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx)、[**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) 以及 [**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) 屬性可在物件旋轉之後，沿著物件平面的各軸平移物件。 因此，旋轉物件的方式會決定平移物件的方向。 為了示範此概念，下一個範例以動畫顯示 0 到 400 度的 **LocalOffsetX**，以及 0 到 65 度的 [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy)。

請注意，前述範例中的物件是沿著本身 x 軸移動。 動畫一開始，[**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy) 值接近零時 (與螢幕平行)，物件是以 X 方向沿著螢幕移動，但是當物件朝向您旋轉時，則是沿著朝向您之物件平面的 X 軸移動。 另一方面，如果您以動畫顯示 **RotationY** 屬性到 -65 度，則物件會以曲線路徑轉離。

[**LocalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsety) 的運作方式與 [**LocalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetx) 類似，不過是沿著垂直軸移動，因此變更 [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) 效果會影響 **LocalOffsetY** 移動物件的方向。 下一個範例會動畫顯示 0 到 400 度的 **LocalOffsetY**，以及 0 到 65 度的 **RotationX**。

[**LocalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.localoffsetz) 可平移與物件平面呈直角的物件，就像從物件背面直接穿透中心朝向您繪製一個向量。 為了示範 **LocalOffsetZ** 的運作方式，下一個範例將以動畫顯示 0 到 400 度的 **LocalOffsetZ**，以及 0 到 65 度的 [**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx)。

動畫一開始，[**RotationX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationx) 值接近零時 (與螢幕平行)，物件是直接朝向您移動，但是當物件的正面向下旋轉，就會改為向下移動。

**全域位移**

[**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx)、[**GlobalOffsetY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsety) 以及 [**GlobalOffsetZ**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetz) 屬性會沿著相對於螢幕的各軸平移物件。 也就是說，物件移動沿著的軸與物件套用的任何旋轉無關，這一點與區域位移屬性不同。 若您只想沿著螢幕的 x、y 或 z 軸移動物件，這些屬性就非常實用，可以讓您不用擔心物件所套用的旋轉。

下一個範例會以動畫顯示 0 到 400 度的 [**GlobalOffsetX**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.globaloffsetx)，以及 0 到 65 度的 [**RotationY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.planeprojection.rotationy)。

請注意，此範例中的物件在旋轉時不會變更路徑。 因為物件是沿著螢幕的 x 軸移動，與其本身的旋轉無關。

## <a name="positioning-an-object"></a>放置物件

您可以使用 [**Matrix3DProjection**](https://msdn.microsoft.com/library/windows/apps/BR210128) 與 [**Matrix3D**](https://msdn.microsoft.com/library/windows/apps/BR243266) 類型取得 [**PlaneProjection**](https://msdn.microsoft.com/library/windows/apps/BR210192) 無法達到的更複雜 semi-3D 效果。 **Matrix3DProjection** 提供一個可套用至任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 的完整 3D 轉換矩陣，讓您隨心所欲地將轉換矩陣與透視矩陣模型套用至元素。 請記住，這些 API 只含最基本的內容，如果您要使用它們，就必須寫入可正確建立 3D 轉換矩陣的程式碼。 因此，使用 **PlaneProjection** 創造簡單的 3D 效果會比較容易。
