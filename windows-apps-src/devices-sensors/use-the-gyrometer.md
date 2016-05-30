---
author: DBirtolo
ms.assetid: 454953E1-DD8F-44B7-A614-7BAD8C683536
title: 使用陀螺儀
description: 了解如何使用陀螺儀來偵測使用者的移動變化。
---
# 使用陀螺儀

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** 重要 API **

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**陀螺儀**](https://msdn.microsoft.com/library/windows/apps/BR225718)

\[正式發行前可能會進行大幅度修改之發行前版本產品的一些相關資訊。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

了解如何使用陀螺儀來偵測使用者的移動變化。

陀螺儀與加速計可互補做為遊戲控制器。 加速計可測量線性動作，而陀螺儀可測量角速度或旋轉動作。

## 先決條件

您應該熟悉 Extensible Application Markup Language (XAML)、Microsoft Visual C# 及事件。

您使用的裝置或模擬器必須支援陀螺儀。

## 建立簡單的陀螺儀應用程式

本節分為兩個子區段。 第一個子區段會引導您完成從頭開始建立簡單陀螺儀應用程式所需的步驟。 接下來的子區段會說明您剛建立的應用程式。

###  指示

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

    using Windows.UI.Core; // Required to access the core dispatcher object
    using Windows.Devices.Sensors; // Required to access the sensor platform and the gyrometer


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Gyrometer _gyrometer; // Our app' s gyrometer object
     
            // This event handler writes the current gyrometer reading to 
            // the three textblocks on the app' s main page.

            private async void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    GyrometerReading reading = e.Reading;
                    txtXAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityX);
                    txtYAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityY);
                    txtZAxis.Text = String.Format("{0,5:0.00}", reading.AngularVelocityZ);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
                
                if (_gyrometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _gyrometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _gyrometer.ReportInterval = reportInterval;

                    // Assign an event handler for the gyrometer reading-changed event
                    _gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, GyrometerReadingChangedEventArgs>(ReadingChanged);
                }

            }
        }
    }
```

您需要將之前程式碼片段中的命名空間重新命名為您專案的名稱。 例如，如果您已建立名為 **GyrometerCS** 的專案，則應該將 `namespace App1` 取代為 `namespace GyrometerCS`。

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
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
            <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
            <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
            <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
            <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>

        </Grid>
    </Page>
```

您需要將之前程式碼片段中的第一個部分的類別名稱，換成 app 的命名空間。 例如，如果您已建立名為 **GyrometerCS** 的專案，則應該將 `x:Class="App1.MainPage"` 取代為 `x:Class="GyrometerCS.MainPage"`。 您也應該將 `xmlns:local="using:App1"` 取代為 `xmlns:local="using:GyrometerCS"`。

-   按 F5 或選取 [偵錯]****  >  [開始偵錯]**** 以建置、部署及執行 App。

App 開始執行之後，您就可以移動裝置或使用模擬器工具來變更陀螺儀值。

-   返回 Visual Studio，然後按 Shift+F5 或選取 [偵錯]****  >  [停止偵錯]**** 以停止 App。

###  說明

前面的範例示範了如何只需要撰寫簡短的程式碼，就可以整合 app 中的陀螺儀輸入。

App 會與 **MainPage** 方法中的預設陀螺儀建立連線。

```csharp
_gyrometer = Gyrometer.GetDefault(); // Get the default gyrometer sensor object
```

App 會在 **MainPage** 方法內建立報告間隔。 這段程式碼會擷取裝置所支援的最短間隔，並和所要求的 16 毫秒間隔 (重新整理的速率大約是 60-Hz) 比較。 如果支援的最短間隔大於要求的間隔，程式碼會將該值設定為最小值。 否則，就會將該值設定為要求的間隔。

```csharp
uint minReportInterval = _gyrometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_gyrometer.ReportInterval = reportInterval;
```

會在 **ReadingChanged** 方法中擷取新的陀螺儀資料。 每次感應器驅動程式收到感應器的新資料時，都會使用這個事件處理常式將值傳送給 app。 應用程式會用下行程式碼登錄這個事件處理常式。

```csharp
_gyrometer.ReadingChanged += new TypedEventHandler<Gyrometer, 
GyrometerReadingChangedEventArgs>(ReadingChanged);
```

這些新的值會寫入專案 XAML 中的 TextBlock。

```xml
        <TextBlock HorizontalAlignment="Left" Height="23" Margin="8,8,0,0" TextWrapping="Wrap" Text="X-Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFDFDFD"/>
        <TextBlock x:Name="txtXAxis" HorizontalAlignment="Left" Height="23" Margin="67,8,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="88" Foreground="#FFFDFAFA"/>
        <TextBlock HorizontalAlignment="Left" Height="20" Margin="8,52,0,0" TextWrapping="Wrap" Text="Y Axis:" VerticalAlignment="Top" Width="46" Foreground="White"/>
        <TextBlock x:Name="txtYAxis" HorizontalAlignment="Left" Height="24" Margin="54,48,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="80" Foreground="#FFFBFBFB"/>
        <TextBlock HorizontalAlignment="Left" Height="21" Margin="8,93,0,0" TextWrapping="Wrap" Text="Z Axis:" VerticalAlignment="Top" Width="46" Foreground="#FFFEFBFB"/>
        <TextBlock x:Name="txtZAxis" HorizontalAlignment="Left" Height="21" Margin="54,93,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="63" Foreground="#FFF8F3F3"/>
```

 ## 相關主題

* [陀螺儀範例](http://go.microsoft.com/fwlink/p/?linkid=241379)



<!--HONumber=May16_HO2-->


