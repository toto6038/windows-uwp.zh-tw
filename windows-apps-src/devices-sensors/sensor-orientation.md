---
author: muhsinking
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: 感應器方向
description: 取自 Accelerometer、Gyrometer、Compass、Inclinometer 以及 OrientationSensor 類別的感應器資料是由它們的參考軸線定義的。 這些軸線是由裝置的橫式方向定義，並在使用者轉動裝置時隨著旋轉。
ms.author: mukin
ms.date: 05/24/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f3205bfa2da1b83fe2c341b1c810f155e796b804
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5758234"
---
# <a name="sensor-orientation"></a>感應器方向


**重要 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**Windows.Devices.Sensors.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn895032)

取自 [**Accelerometer**](https://msdn.microsoft.com/library/windows/apps/BR225687)、[**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/BR225718)、[**Compass**](https://msdn.microsoft.com/library/windows/apps/BR225705)、[**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/BR225766) 以及 [**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 類別的感應器資料是由它們的參考軸線定義的。 這些軸線是由裝置的參照畫面定義，並在使用者轉動裝置時隨著旋轉。 如果您的 app 支援自動旋轉，而會隨著使用者旋轉時調整自己的方向來適應裝置，您就必須在使用前先行針對旋轉調整感應器資料。

## <a name="display-orientation-vs-device-orientation"></a>顯示方向和裝置方向

為了了解感應器的參考軸線，您必須區分顯示方向與裝置方向。 顯示方向就是螢幕上顯示的方向文字與影像，而裝置方向則是裝置的實際位置。 在下圖中，裝置和顯示方向都是**橫向** (請注意，顯示的感應器軸線只適用於橫向優先裝置)。

![顯示方向與裝置方向為 Landscape](images/sensor-orientation-a.PNG)

下圖說明顯示方向與裝置方向都是 **LandscapeFlipped**。

![顯示方向與裝置方向為 LandscapeFlipped](images/sensor-orientation-b.PNG)

下一張圖說明顯示方向為 Landscape，而裝置方向為 LandscapeFlipped。

![顯示方向為 Landscape，而裝置方向為 LandscapeFlipped](images/sensor-orientation-c.PNG)

使用 [**GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.getforcurrentview.aspx) 方法搭配 [**CurrentOrientation**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.display.displayinformation.currentorientation.aspx) 屬性，即可透過 [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/Dn264258) 類別查詢方向值。 接著與 [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/BR226142) 列舉比較就能建立邏輯。 記住您支援哪些方向，就必須支援將參考軸線轉換為該方向。

## <a name="landscape-first-vs-portrait-first-devices"></a>橫向優先裝置與直向優先裝置

製造商會同時生產橫向優先裝置與直向優先裝置。 橫向優先裝置 (例如桌上型電腦和膝上型電腦) 與直向優先裝置 (例如手機和某些平板電腦) 之間的參考框架並不同。 下表顯示橫向優先裝置與直向優先裝置的感應器軸線。

| 方向 | 橫向優先 | 直向優先 |
|-------------|-----------------|----------------|
| **橫向** | ![方向為 Landscape 的橫向優先裝置](images/sensor-orientation-0.PNG) | ![方向為 Landscape 的直向優先裝置](images/sensor-orientation-1.PNG) |
| **直向** | ![方向為 Portrait 的橫向優先裝置](images/sensor-orientation-2.PNG) | ![方向為 Portrait 的直向優先裝置](images/sensor-orientation-3.PNG) |
| **LandscapeFlipped** | ![方向為 LandscapeFlipped 的橫向優先裝置](images/sensor-orientation-4.PNG) | ![方向為 LandscapeFlipped 的直向優先裝置](images/sensor-orientation-5.PNG) | 
| **PortraitFlipped** | ![方向為 PortraitFlipped 的橫向優先裝置](images/sensor-orientation-6.PNG)| ![方向為 PortraitFlipped 的直向優先裝置](images/sensor-orientation-7.PNG) |

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
| **LandscapeFlipped**  | -X | -Y | Z |
| **PortraitFlipped**   | -Y |  X | Z |

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

[**OrientationSensor**](https://msdn.microsoft.com/library/windows/apps/BR206371) 資料必須以不同方式變更。 想像這些不同的方向，如逆時針旋轉到 Z 軸，所以我們需要讓旋轉反轉以回到使用者的方向。 對於四元數資料，我們可以使用尤拉公式來定義參考四元數旋轉，也可以使用參考旋轉矩陣。

![尤拉公式](images/eulers-formula.png)

若要取得您想要的相對方向，請將參考物件比對絕對物件相乘。 請注意，這個數學公式不可以交換。

![將參考物件比對絕對物件相乘](images/orientation-formula.png)

在前面的運算式中，絕對物件是由感應器資料傳回。


| 顯示方向  | 延著 Z 軸逆時針旋轉 | 參考四元數 (反向旋轉) | 參考旋轉矩陣 (反向旋轉) | 
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **橫向**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **直向**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |

