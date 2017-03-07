---
author: DBirtolo
ms.assetid: F90686F5-641A-42D9-BC44-EC6CA11B8A42
title: "使用加速計"
description: "了解如何使用加速計來回應使用者移動。"
ms.author: dbirtolo
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 8f8236a68fd7628f1f53eebc13731a72414e3217
ms.lasthandoff: 02/07/2017

---
# <a name="use-the-accelerometer"></a>使用加速計

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**重要 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**加速計**](https://msdn.microsoft.com/library/windows/apps/BR225687)

\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

了解如何使用加速計來回應使用者移動。

簡單的遊戲應用程式倚賴單一感應器 (也就是加速計) 做為輸入裝置。 這些應用程式通常只使用一或兩個軸做為輸入，但是它們也可以使用 shake 事件做為另一個輸入來源。

## <a name="prerequisites"></a>先決條件

您應該熟悉 Extensible Application Markup Language (XAML)、Microsoft Visual C# 及事件。

您使用的裝置或模擬器必須支援加速計。

## <a name="create-a-simple-accelerometer-app"></a>建立簡單的加速計應用程式

本節分為兩個子區段。 第一個子區段會引導您完成從頭開始建立簡單加速計應用程式所需的步驟。 接下來的子區段會說明您剛建立的應用程式。

### <a name="instructions"></a>指示

-   從 [Visual C#]**** 專案範本中選擇 [空白應用程式 (通用 Windows)]**** 來建立一個新專案。

-   開啟專案的 MainPage.xaml.cs 檔案，然後以下列程式碼取代現有的程式碼。

```csharp
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Windows.Foundation;
    using Windows.Foundation.Collections;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;
    using Windows.UI.Xaml.Controls.Primitives;
    using Windows.UI.Xaml.Data;
    using Windows.UI.Xaml.Input;
    using Windows.UI.Xaml.Media;
    using Windows.UI.Xaml.Navigation;

    // Required to support the core dispatcher and the accelerometer

    using Windows.UI.Core;
    using Windows.Devices.Sensors;

    namespace App1
    {

        public sealed partial class MainPage : Page
        {
            // Sensor and dispatcher variables
            private Accelerometer _accelerometer;

            // This event handler writes the current accelerometer reading to
            // the three acceleration text blocks on the app' s main page.

            private async void ReadingChanged(object sender, AccelerometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    AccelerometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AccelerationZ);

                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _accelerometer = Accelerometer.GetDefault();

                if (_accelerometer != null)
                {
                    // Establish the report interval
                    uint minReportInterval = _accelerometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _accelerometer.ReportInterval = reportInterval;

                    // Assign an event handler for the reading-changed event
                    _accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer, AccelerometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

您需要將之前程式碼片段中的命名空間重新命名為您專案的名稱。 例如，如果您已建立名為 **AccelerometerCS** 的專案，則應該將 `namespace App1` 取代為 `namespace AccelerometerCS`。

-   開啟 MainPage.xaml 檔案，並以下列 XML 取代原始內容。

```xml
        <Page
        x:Class="App1.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="#FF0C0C0C">
            <TextBlock HorizontalAlignment="Left" Height="25" Margin="8,20,0,0" TextWrapping="Wrap" Text="X-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFEDE6E6"/>
            <TextBlock HorizontalAlignment="Left" Height="27" Margin="8,49,0,0" TextWrapping="Wrap" Text="Y-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF5F2F2"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,80,0,0" TextWrapping="Wrap" Text="Z-axis:" VerticalAlignment="Top" Width="62" Foreground="#FFF6F0F0"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>

        </Grid>
    </Page>
```

您需要將之前程式碼片段中的第一個部分的類別名稱，換成 app 的命名空間。 例如，如果您已建立名為 **AccelerometerCS** 的專案，則應該將 `x:Class="App1.MainPage"` 取代為 `x:Class="AccelerometerCS.MainPage"`。 您也應該將 `xmlns:local="using:App1"` 取代為 `xmlns:local="using:AccelerometerCS"`。

-   按 F5 或選取 [偵錯]**** &gt; [開始偵錯]**** 以建置、部署及執行 app。

App 開始執行之後，您就可以移動裝置或使用模擬器工具來變更加速器值。

-   返回 Visual Studio，然後按 Shift+F5 或選取 [偵錯]**** &gt; [停止偵錯]**** 以停止應用程式。

### <a name="explanation"></a>說明

前面的範例示範了如何只需要撰寫簡短的程式碼，就可以整合 app 中的加速計輸入。

App 會與 **MainPage** 方法中的預設加速計建立連線。

```csharp
_accelerometer = Accelerometer.GetDefault();
```

App 會在 **MainPage** 方法內建立報告間隔。 這段程式碼會擷取裝置所支援的最短間隔，並和所要求的 16 毫秒間隔 (重新整理的速率大約是 60-Hz) 比較。 如果支援的最短間隔大於要求的間隔，程式碼會將該值設定為最小值。 否則，就會將該值設定為要求的間隔。

```csharp
uint minReportInterval = _accelerometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_accelerometer.ReportInterval = reportInterval;
```

會在 **ReadingChanged** 方法中擷取新的加速計資料。 每次感應器驅動程式收到感應器的新資料時，都會使用這個事件處理常式將值傳送給 app。 應用程式會用下行程式碼登錄這個事件處理常式。

```csharp
_accelerometer.ReadingChanged += new TypedEventHandler<Accelerometer,
AccelerometerReadingChangedEventArgs>(ReadingChanged);
```

這些新的值會寫入專案 XAML 中的 TextBlock。

```xml
<TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="15" Margin="70,16,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="61" Foreground="#FFF2F2F2"/>
 <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="15" Margin="70,49,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFF2EEEE"/>
 <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="15" Margin="70,80,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="53" Foreground="#FFFFF8F8"/>
```
## <a name="related-topics"></a>相關主題

* [加速計範例](http://go.microsoft.com/fwlink/p/?linkid=241377)

