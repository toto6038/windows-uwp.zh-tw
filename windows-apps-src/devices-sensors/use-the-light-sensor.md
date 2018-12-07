---
ms.assetid: 15BAB25C-DA8C-4F13-9B8F-EA9E4270BCE9
title: 使用光感應器
description: 了解如何使用周遭環境光感應器來偵測光線的變化。
ms.date: 06/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 47092c128fe3a3855d7e32706451545b357c39c4
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8780709"
---
# <a name="use-the-light-sensor"></a>使用光感應器


**重要 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**LightSensor**](https://msdn.microsoft.com/library/windows/apps/BR225790)

**範例**

-   如需更完整的實作，請參閱[光感應器範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LightSensor)。

了解如何使用周遭環境光感應器來偵測光線的變化。

周遭環境光感應器是數種環境感應器的其中一種，可讓應用程式回應使用者環境中的變化。

## <a name="prerequisites"></a>先決條件

您應該熟悉 Extensible Application Markup 語言 (XAML)、 Microsoft VisualC # 及事件。

您使用的裝置或模擬器必須支援周遭環境光感應器。

## <a name="create-a-simple-light-sensor-app"></a>建立簡單的光感應器應用程式

本節分為兩個子區段。 第一個子區段會引導您完成從頭開始建立簡單光感應器應用程式所需的步驟。 接下來的子區段會說明您剛建立的應用程式。

###  <a name="instructions"></a>指示

-   從 **\[Visual C#\]** 專案範本中選擇 **\[空白應用程式 (通用 Windows)\]** 來建立一個新專案。

-   開啟專案的 BlankPage.xaml.cs 檔案，然後以下列程式碼取代現有的程式碼。

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
    using Windows.Devices.Sensors; // Required to access the sensor platform and the ALS

    // The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?linkid=234238

    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class BlankPage : Page
        {
            private LightSensor _lightsensor; // Our app' s lightsensor object

            // This event handler writes the current light-sensor reading to
            // the textbox named "txtLUX" on the app' s main page.

            private void ReadingChanged(object sender, LightSensorReadingChangedEventArgs e)
            {
                Dispatcher.RunAsync(CoreDispatcherPriority.Normal, (s, a) =>
                {
                    LightSensorReading reading = (a.Context as LightSensorReadingChangedEventArgs).Reading;
                    txtLuxValue.Text = String.Format("{0,5:0.00}", reading.IlluminanceInLux);
                });
            }

            public BlankPage()
            {
                InitializeComponent();
                _lightsensor = LightSensor.GetDefault(); // Get the default light sensor object

                // Assign an event handler for the ALS reading-changed event
                if (_lightsensor != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _lightsensor.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _lightsensor.ReportInterval = reportInterval;

                    // Establish the even thandler
                    _lightsensor.ReadingChanged += new TypedEventHandler<LightSensor, LightSensorReadingChangedEventArgs>(ReadingChanged);
                }

            }

        }
    }
```

您需要將之前程式碼片段中的命名空間重新命名為您專案的名稱。 例如，如果您已建立名為 **LightingCS** 的專案，則應該將 `namespace App1` 取代為 `namespace LightingCS`。

-   開啟 MainPage.xaml 檔案，並以下列 XML 取代原始內容。

```xml
    <Page
        x:Class="App1.BlankPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:App1"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid x:Name="LayoutRoot" Background="Black">
            <TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
            <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>


        </Grid>

    </Page>
```

您需要將之前程式碼片段中的第一個部分的類別名稱，換成 app 的命名空間。 例如，如果您已建立名為 **LightingCS** 的專案，則應該將 `x:Class="App1.MainPage"` 取代為 `x:Class="LightingCS.MainPage"`。 您也應該將 `xmlns:local="using:App1"` 取代為 `xmlns:local="using:LightingCS"`。

-   按 F5 或選取 **\[偵錯\]** > **\[開始偵錯\]** 以建置、部署及執行 App。

App 開始執行之後，您就可以改變感應器可用的光線或使用模擬器工具來變更光感器值。

-   返回 Visual Studio，然後按 Shift+F5 或選取 **\[偵錯\]** > **\[停止偵錯\]** 以停止 App。

###  <a name="explanation"></a>說明

前面的範例示範了如何只需要撰寫簡短的程式碼，就可以整合 app 中的光感應器輸入。

App 會與 **BlankPage** 方法中的預設感應器建立連線。

```csharp
_lightsensor = LightSensor.GetDefault(); // Get the default light sensor object
```

App 會在 **BlankPage** 方法內建立報告間隔。 這段程式碼會擷取裝置所支援的最短間隔，並和所要求的 16 毫秒間隔 (重新整理的速率大約是 60-Hz) 比較。 如果支援的最短間隔大於要求的間隔，程式碼會將該值設定為最小值。 否則，就會將該值設定為要求的間隔。

```csharp
uint minReportInterval = _lightsensor.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_lightsensor.ReportInterval = reportInterval;
```
會在 **ReadingChanged** 方法中擷取新的光感應器資料。 每次感應器驅動程式收到感應器的新資料時，都會使用這個事件處理常式將值傳送給 app。 應用程式會用下行程式碼登錄這個事件處理常式。

```csharp
_lightsensor.ReadingChanged += new TypedEventHandler<LightSensor,
LightSensorReadingChangedEventArgs>(ReadingChanged);
```

這些新的值會寫入專案 XAML 中的 TextBlock。

```xml
<TextBlock HorizontalAlignment="Left" Height="44" Margin="52,38,0,0" TextWrapping="Wrap" Text="LUX Reading" VerticalAlignment="Top" Width="150"/>
 <TextBlock x:Name="txtLuxValue" HorizontalAlignment="Left" Height="44" Margin="224,38,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="217"/>
```