---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: 感應器方向
description: 取自 Accelerometer、Gyrometer、Compass、Inclinometer 以及 OrientationSensor 類別的感應器資料是由它們的參考軸線定義的。 這些軸線是由裝置的橫式方向定義，並在使用者轉動裝置時隨著旋轉。
ms.date: 07/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8836753778b1dd5dcbc8856b0df5ec1f11d8e753
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159802"
---
# <a name="sensor-orientation"></a>感應器方向

[**加速**](/uwp/api/Windows.Devices.Sensors.Accelerometer)計、[**陀螺儀**](/uwp/api/Windows.Devices.Sensors.Gyrometer)、[**羅盤**](/uwp/api/Windows.Devices.Sensors.Compass)、[**傾角羅盤**](/uwp/api/Windows.Devices.Sensors.Inclinometer)和[**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor)類別的感應器資料是由其參考軸所定義。 這些軸線是由裝置的參照畫面定義，並在使用者轉動裝置時隨著旋轉。 如果您的 app 支援自動旋轉，而會隨著使用者旋轉時調整自己的方向來適應裝置，您就必須在使用前先行針對旋轉調整感應器資料。

### <a name="important-apis"></a>重要 API

- [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
- [**Windows. 感應器. 自訂**](/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>顯示方向和裝置方向

為了了解感應器的參考軸線，您必須區分顯示方向與裝置方向。 顯示方向就是螢幕上顯示的方向文字與影像，而裝置方向則是裝置的實際位置。

> [!NOTE]
> 正 Z 軸會從裝置畫面中擴充，如下圖所示。
> :::image type="content" source="images/sensor-orientation-zaxis-1-small.png" alt-text="Z 軸的膝上型電腦":::

在下列圖表中，裝置和顯示方向都是 [橫向](/uwp/api/Windows.Graphics.Display.DisplayOrientations) (感應器軸顯示為橫向) 特定。


下圖顯示 [橫向](/uwp/api/Windows.Graphics.Display.DisplayOrientations)的顯示和裝置方向。

:::image type="content" source="images/sensor-orientation-a-small.jpg" alt-text="顯示方向與裝置方向為 Landscape":::

下圖顯示 [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations)中顯示和裝置的方向。

:::image type="content" source="images/sensor-orientation-b-small.jpg" alt-text="顯示方向與裝置方向為 LandscapeFlipped":::

此最後的圖表會顯示橫向的顯示方向，而裝置方向為 [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations)。

:::image type="content" source="images/sensor-orientation-c-small.jpg" alt-text="顯示方向為 Landscape，而裝置方向為 LandscapeFlipped":::

使用 [**GetForCurrentView**](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) 方法搭配 [**CurrentOrientation**](/uwp/api/windows.graphics.display.displayinformation.currentorientation) 屬性，即可透過 [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation) 類別查詢方向值。 接著與 [**DisplayOrientations**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) 列舉比較就能建立邏輯。 記住您支援哪些方向，就必須支援將參考軸線轉換為該方向。

## <a name="landscape-first-vs-portrait-first-devices"></a>橫向優先裝置與直向優先裝置

製造商會同時生產橫向優先裝置與直向優先裝置。 橫向優先裝置 (例如桌上型電腦和膝上型電腦) 與直向優先裝置 (例如手機和某些平板電腦) 之間的參考框架並不同。 下表顯示橫向優先裝置與直向優先裝置的感應器軸線。

| 方向 | 橫向優先 | 直向優先 |
|-------------|-----------------|----------------|
| **橫向** | :::image type="content" source="images/sensor-orientation-0-small.jpg" alt-text="方向為 Landscape 的橫向優先裝置"::: | :::image type="content" source="images/sensor-orientation-1-small.jpg" alt-text="方向為 Landscape 的直向優先裝置"::: |
| **直向** | :::image type="content" source="images/sensor-orientation-2-small.jpg" alt-text="方向為 Portrait 的橫向優先裝置"::: | :::image type="content" source="images/sensor-orientation-3-small.jpg" alt-text="方向為 Portrait 的直向優先裝置"::: |
| **LandscapeFlipped** | :::image type="content" source="images/sensor-orientation-4-small.jpg" alt-text="方向為 LandscapeFlipped 的橫向優先裝置"::: | :::image type="content" source="images/sensor-orientation-5-small.jpg" alt-text="方向為 LandscapeFlipped 的直向優先裝置":::
| **PortraitFlipped** | :::image type="content" source="images/sensor-orientation-6-small.jpg" alt-text="方向為 PortraitFlipped 的橫向優先裝置"::: | :::image type="content" source="images/sensor-orientation-7-small.jpg" alt-text="方向為 PortraitFlipped 的直向優先裝置"::: |

## <a name="devices-broadcasting-display-and-headless-devices"></a>廣播顯示畫面和無周邊裝置的裝置

有些裝置能夠將顯示畫面廣播至另一個裝置。 例如，您可以使用平板電腦，並將顯示畫面廣播至以橫向顯示的投影機。 在此情況下，請務必記住裝置方向是以原始裝置為準，而不是以呈現顯示畫面的裝置為準。 因此加速計會報告平板電腦的資料。

此外，有些裝置沒有顯示畫面。 這些裝置的預設方向會設為直向。

## <a name="display-orientation-and-compass-heading"></a>顯示方向和指南針朝向

指南針朝向取決於參考軸線，所以它會隨著裝置方向變更。 您要根據下表進行調整 (假設使用者面向北方)。

| 顯示方向 | 指南針朝向的參考軸線 | 面向北方的 API 指南針朝向 (先橫向) | 面向北方的 API 指南針朝向 (先直向) |指南針朝向調整 (先橫向) | 指南針朝向調整 (先直向) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| 橫向           | -Z | 0   | 270 | 朝向               | (朝向 + 90) % 360  |
| 直向            |  Y | 90  | 0   | (朝向 + 270) % 360 |  朝向              |
| LandscapeFlipped    |  Z | 180 | 90  | (朝向 + 180) % 360 | (朝向 + 270) % 360 |
| PortraitFlipped     |  Y | 270 | 180 | (朝向 + 90) % 360  | (朝向 + 180) % 360 |

如表中所示修改指南針朝向，以便正確顯示朝向。 下列程式碼片段示範其操作方法。

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>包含加速計和陀螺儀的顯示方向

本表轉換顯示方向的加速計和陀螺儀資料。

| 參考軸線        |  X |  Y | Z |
|-----------------------|----|----|---|
| **橫向**         |  X |  Y | Z |
| **直向**          |  Y | -X | Z |
| **LandscapeFlipped**  | -X | -y | Z |
| **PortraitFlipped**   | -y |  X | Z |

以下是將這些轉換套用到陀螺儀的程式碼範例。

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>顯示方向和裝置方向

[**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) 資料必須以不同方式變更。 您可以將這些不同的方向視為逆時針旋轉至 Z 軸的方向，因此我們必須反轉旋轉，以取回使用者的方向。 對於四元數資料，我們可以使用尤拉公式來定義參考四元數旋轉，也可以使用參考旋轉矩陣。

:::image type="content" source="images/eulers-formula.png" alt-text="尤拉公式":::

若要取得您想要的相對方向，請將參考物件比對絕對物件相乘。 請注意，這個數學公式不可以交換。

:::image type="content" source="images/orientation-formula.png" alt-text="將參考物件比對絕對物件相乘":::

在前面的運算式中，絕對物件是由感應器資料傳回。

| 顯示方向  | 延著 Z 軸逆時針旋轉 | 參考四元數 (反向旋轉) | 參考旋轉矩陣 (反向旋轉) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **橫向**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **直向**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |

## <a name="see-also"></a>另請參閱

[整合動作與方向感應器](/windows-hardware/design/whitepapers/integrating-motion-and-orientation-sensors)