---
ms.assetid: 415F4107-0612-4235-9722-0F5E4E26F957
title: 感應器
description: 感應器可讓 app 得知裝置與周遭實際環境之間的關係。 感應器會將裝置的方向、指向及動作告知您的 app。
ms.date: 06/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5d86c5b0d80f83d774b1ba4f4dc297dcf1adb645
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369831"
---
# <a name="sensors"></a>感應器



感應器可讓 app 得知裝置與周遭實際環境之間的關係。 感應器會將裝置的方向、指向及動作告知您的 app。 這些感應器提供獨特的輸入形式 (例如透過裝置的動作來排列螢幕上的字元，或模擬在駕駛艙中以及將裝置當成方向盤操縱)，有助您提高遊戲、擴增實境應用程式或公用應用程式的實用性與互動性。

您應該從最開始就決定應用程式是否只仰賴感應器，或是只讓感應器提供額外的控制機制，這是一個普遍規則。 例如，將裝置當成虛擬方向盤的駕駛遊戲可以選擇透過螢幕上的 GUI 來控制。這樣一來，無論系統是否有感應器，應用程式都能夠運作。 另一方面，您撰寫彈珠傾斜迷宮的程式碼時，可讓它只能在具備適當感應器的系統上運作。 您必須進行策略抉擇，決定是否要完全倚賴感應器。 請注意，雖然滑鼠/觸控的操作方式比較不容易覺得身歷其境，但控制性較佳。

| 主題                                                       | 描述  |
|-------------------------------------------------------------|--------------|
| [校正感應器](calibrate-sensors.md)                   | 以磁力儀 (指南針、傾角計及方向感應器) 為基礎的裝置感應器會因為環境因素而需要校正。 [  <strong>MagnetometerAccuracy</strong>](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) 列舉可以在您的裝置需要校正時協助判斷可採取的步驟。 |
| [感應器方向](sensor-orientation.md)                 | 取自 [<strong>OrientationSensor</strong>](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) 類別的感應器資料是由它們的參考軸線定義的。 這些軸線是由裝置的橫式方向定義，並在使用者轉動裝置時隨著旋轉。 |
| [使用加速計](use-the-accelerometer.md)           | 了解如何使用加速計來回應使用者移動。 |
| [使用羅盤](use-the-compass.md)                       | 了解如何使用指南針來判斷目前朝向何方。 |
| [使用陀螺儀](use-the-gyrometer.md)                   | 了解如何使用陀螺儀來偵測使用者的移動變化。 | 
| [使用傾角](use-the-inclinometer.md)             | 了解如何使用傾角計來決定俯仰、翻滾及偏擺。 |
| [使用光感應器](use-the-light-sensor.md)             | 了解如何使用周遭環境光感應器來偵測光線的變化。 |
| [使用方向感應器](use-the-orientation-sensor.md) | 了解如何使用方向感應器來判斷裝置方向。|

## <a name="sensor-batching"></a>感應器批次處理

某些感應器支援批次處理的概念。 這取決於使用者所用的感應器。 當感應器執行批次處理時，會根據指定的時間間隔收集多個資料點，然後一次傳送所有的資料。 某些感應器開始讀取數據後，一有結果便會立即回報，這和此處介紹的感應器不同。 請參閱下圖，您可以了解資料的收集和傳送方式，一開始是常規的資料傳送，然後是批次方式的資料傳送。

![感應器批次收集](images/batchsample.png)

感應器批次處理的主要優點就是延長電池續航力。 不立即傳送資料的話，處理器可以節省耗電，也免去需要立即處理資料的工夫。 系統的某些組件在不用的時候，可以持續進入睡眠狀態，可大幅節省耗電

您可以調整延遲時間，即可控制感應器傳送批次資料的頻率。 例如，[**Accelerometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 的 [**ReportLatency**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.reportlatency) 屬性。 為應用程式設定這個屬性之後，感應器就會在指定的時間之後傳送資料。 您可以設定 [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.reportinterval)，這樣就可以在規定的延遲時間內，控制可以累積的資料數量。

設定延遲性時，有幾點重要聲明，請您牢記於心。 第一點重要聲明是：根據感應器自身情況，它們可以接受的 [**MaxBatchSize**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.maxbatchsize) 各自不同。 這是指感應器最多可以快取的事件數量，超過此限之後，就必須傳送出去。 將 **MaxBatchSize** 乘以 [**ReportInterval**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.reportinterval) 之後，就會得出 [**ReportLatency**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.reportlatency) 最大值。 如果指定的值大於這個最大值，就會使用延遲時間上限，以免您遺失資料。 此外，即使有多個應用程式，每一個還是可以設定自己的延遲時間。 為了滿足所有的應用程式的需求，系統會採用最短的延遲時間。 因為這些種種的因素，所以您在應用程式中設定的延遲時間，有可能會與觀察到的延遲時間不一致。

感應器使用批次報告時，呼叫 [**GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) 即可清除目前的批次資料，然後開始新的延遲時間。

## <a name="accelerometer"></a>加速計

[  **Accelerometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) 感應器可測量裝置沿著 X 軸、Y 軸及 Z 軸的重力值，很適合簡單動作應用程式。 請注意，所謂的重力值包括因重力而產生的加速度。 如果裝置在桌面上的 **FaceUp** 為 [**SimpleOrientation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientation)，加速計的 Z 軸讀數就是 -1 G。 因此，加速計不見得只會測量座標加速度 (速度的變動率)。 當使用加速計時，請務必區別重力向量與重力的區隔，以及線性加速向量與動作的區隔。 請注意，靜止裝置的重力向量應該正規化為 1。

下圖說明：

-   V1 = 向量 1 = 因重力而產生的力
-   V2 = 向量 2 = 裝置底座的 -Z 軸 (自螢幕背面向外指)
-   Θi = 斜度 (傾斜角度) = 裝置底座 –Z 軸與重力向量之間的角度

![加速計](images/accelerometer1.png)![加速計測量](images/accelerometer2.png)

可使用加速計感應器的應用程式，包括以您傾斜裝置的方向 (重力向量) 讓彈珠在螢幕上滾動的遊戲。 這類功能會密切對應到 [**Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) 的功能，且也可利用該感應器或結合使用俯仰與翻滾動作來完成。 使用加速計的重力向量，為裝置傾斜提供輕鬆以數學方式操控的向量，可稍微簡化此動作。 另一個範例是使用者在空中揮動裝置 (線性加速向量) 時，讓鞭子發出霹啪聲的 app。

如需範例實作，請參閱[加速計範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Accelerometer)。

## <a name="activity-sensor"></a>活動感應器

[  **Activity**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.ActivitySensor) 感應器會判斷連接至感應器的裝置目前的狀態。 此感應器通常用於健身 app，可在攜帶裝置的使用者跑步或步行時進行追蹤。 若想了解此感應器 API 可偵測到哪些可能的活動，請參閱 [**ActivityType**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.ActivityType)。

如需範例實作，請參閱[活動感應器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ActivitySensor)。

## <a name="altimeter"></a>高度表

[  **Altimeter**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Altimeter) 感應器會傳回一個值，指出感應器的高度。 這可讓您追蹤距離海平面的高度變化 (以公尺為單位)。 舉例來說，會在跑步期間追蹤高度變化以計算卡路里燃燒量的跑步 app，就是可能會使用此感應器的 app 之一。 在此案例中，這個感應器的資料可與 [**Activity**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.ActivitySensor) 感應器相結合，以提供更精確的追蹤資訊。

如需範例實作，請參閱[高度表範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Altimeter)。

## <a name="barometer"></a>氣壓計

[  **Barometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Barometer) 感應器可讓 app 取得氣壓計讀數。 天氣 app 可使用這項資訊提供目前的氣壓。 這可以用來提供更詳細的資訊，並預測可能的天氣變化。

如需範例實作，請參閱[氣壓計範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Barometer)。

## <a name="compass"></a>指南針

[  **Compass**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Compass) 感應器可根據地球水平面傳回磁北的 2D 指向。 指南針感應器不應該用來判斷特定裝置指向，或用來代表 3D 空間中的任何事物。 地理功能會導致指向形成自然偏角，因此有些系統同時支援 [**HeadingMagneticNorth**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.compassreading.headingmagneticnorth) 與 [**HeadingTrueNorth**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.compassreading.headingtruenorth)。 請想想您的應用程式偏好哪一種；但請記住，並非所有系統都會回報真北值。 結合陀螺儀與磁力儀 (測量磁力強度的裝置) 感應器兩者的資料以產生指南針朝向，而其淨影響就是可穩定資料 (磁場強度會因電力系統設備而極不穩定)。

![關於磁北極的指南針讀數](images/compass.png)

想要顯示羅盤或巡覽地圖的 app，通常都會使用指南針感應器。

如需範例實作，請參閱[指南針範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Compass)。

## <a name="gyrometer"></a>陀螺儀

[  **Gyrometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Gyrometer) 感應器可測量沿著 X 軸、Y 軸及 Z 軸的角速度。 這在簡單動作應用程式中非常有用，因為這些應用程式不受裝置指向影響，但會受到裝置以不同速度旋轉所影響。 陀螺儀會因為資料中的雜訊或沿著一或多軸的常數偏差而受到影響。 您應該查詢加速計以確認裝置是否正在移動，以判斷陀螺儀是否受到偏差所影響，然後據此在應用程式中加以補償。

![陀螺儀：俯仰、翻滾及偏擺](images/gyrometer.png)

使用陀螺儀感應器的 app 範例，就是將裝置猛然快速旋轉來旋轉輪盤的遊戲。

如需範例實作，請參閱[陀螺儀範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Gyrometer)。

## <a name="inclinometer"></a>傾角計

[  **Inclinometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Inclinometer) 感應器可指定裝置的偏擺、俯仰及翻滾的值，最適合以裝置在空間之定位方式為基準的應用程式。 俯仰與翻滾是採用加速計的重力向量以及整合陀螺儀提供的資料所衍生。 偏擺則是以磁力儀與陀螺儀 (類似指南針朝向) 的資料建立。 傾角計以易於解讀和理解的方式提供高階指向資料。 當您需要裝置指向但不需要操控感應器資料時，可以使用傾角計。

![傾角計：俯仰、翻滾以及偏擺資料](images/inclinometer.png)

本身會變更檢視方式以符合裝置指向的應用程式，都可以使用傾角計感應器。 再者，本身會根據裝置偏擺、俯仰及翻滾而顯示飛機動作的 app，也可以使用傾角計讀數。

如需範例實作，請參閱傾角範例[ https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer)。

## <a name="light-sensor"></a>光感應器

[  **Light**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.LightSensor) 感應器可判斷感應器周圍的周遭環境光線。 這可讓 app 判斷裝置周圍的光線設定何時有所變化。 例如，平板電腦的使用者可能會在晴天從室內移到室外去。 智慧型 app 可以使用此值，來提高背景與呈現的字型之間的對比。 如此，即使是在較亮的室外設定下也可閱讀內容。

如需範例實作，請參閱[光感應器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor)。

## <a name="orientation-sensor"></a>方向感應器

裝置指向可透過四元數與旋轉矩陣來表達。 [  **OrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.OrientationSensor) 提供的高精確度可用來判斷裝置在空間中相對於絕對指向的定位方式。 **OrientationSensor** 資料衍生自加速計、陀螺儀及磁力儀。 因此，傾角計感應器與指南針感應器都可以從四元數的值衍生。 四元數與旋轉矩陣對於高階數學操作很有助益，通常用於圖形程式設計。 因為許多轉換方式都是以四元數與旋轉矩陣為基礎，所以使用複雜操作的應用程式應該會偏好使用方向感應器。

![方向感應器資料](images/orientation-sensor.png)

方向感應器通常用於高階擴增實境 app，這種 app 會根據裝置背面所指的方向，對您的周遭環境繪製覆疊。

如需範例實作，請參閱[方向感應器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/OrientationSensor)。

## <a name="pedometer"></a>計步器

[  **Pedometer**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Pedometer) 感應器會追蹤攜帶連線裝置的使用者行走的步數。 感應器依設定會追蹤一段指定時間內的步數。 有些健身 app 會追蹤使用者的行走步數，以幫助他們設定並達到各種目標。 後續可以收集並儲存這項資訊，以顯示一段時間的進度。

如需範例實作，請參閱[計步器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Pedometer)。

## <a name="proximity-sensor"></a>鄰近性感測器

[  **Proximity**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.ProximitySensor) 感應器可用來指出感應器是否偵測到物件。 除了可判斷物件是否在裝置的範圍內、鄰近性感測器也可判斷與偵測的物件之間相隔多少距離。 舉例來說，想要在使用者進入指定範圍時從睡眠狀態啟動的 app，就可能會使用此感應器。 在鄰近性感測器偵測到物件之前，裝置會處於低耗電的睡眠狀態，之後則可進入較活躍的狀態。

如需範例實作，請參閱[鄰近性感測器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ProximitySensor)。

## <a name="simple-orientation"></a>簡單方向

[  **SimpleOrientationSensor**](https://docs.microsoft.com/uwp/api/windows.devices.sensors.simpleorientationsensor) 可偵測特定裝置目前的象限指向，以及它面朝上或朝下。 這有六種可能的 [**SimpleOrientation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientation) 狀態 (**NotRotated**、**Rotated90**、**Rotated180**、**Rotated270**、**FaceUp**、**FaceDown**)。

根據裝置是平行於地面或與地面成直角而變更其顯示方式的閱讀程式，都可以使用 SimpleOrientationSensor 的值來判斷裝置的手持姿勢。

如需範例實作，請參閱[簡單方向感應器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleOrientationSensor)。
