---
ms.assetid: 16AD53CA-1252-456C-8567-2263D3EC95F3
title: 使用傾角計
description: 了解如何使用傾角計來決定俯仰、翻滾及偏擺。
ms.date: 06/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bfd617c3c08cdcb7815010648c6036a5f39ee3ab
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8202665"
---
# <a name="use-the-inclinometer"></a>使用傾角計


**重要 API**

-   [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408)
-   [**傾角計**](https://msdn.microsoft.com/library/windows/apps/BR225766)

**範例**

-   如需更完整的實作，請參閱[傾角計範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Inclinometer)。

了解如何使用傾角計來決定俯仰、翻滾及偏擺。

有些 3D 遊戲需要以傾角計做為輸入裝置。 一個常見範例是飛行模擬器，它需將傾角計的三個軸 (X、Y 以及 Z) 對應至飛機的升降舵、補助翼以及方向舵輸入。

 ## <a name="prerequisites"></a>先決條件

您應該熟悉 Extensible Application Markup 語言 (XAML)、 Microsoft VisualC # 及事件。

您使用的裝置或模擬器必須支援傾角計。

 ## <a name="create-a-simple-inclinometer-app"></a>建立簡單的傾角計應用程式

本節分為兩個子區段。 第一個子區段會引導您完成從頭開始建立簡單傾角計應用程式所需的步驟。 接下來的子區段會說明您剛建立的應用程式。

###  <a name="instructions"></a>指示

-   從 **\[Visual C#\]** 專案範本中選擇 **\[空白應用程式 (通用 Windows)\]** 來建立一個新專案。

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

    using Windows.UI.Core;
    using Windows.Devices.Sensors;


    namespace App1
    {
        /// <summary>
        /// An empty page that can be used on its own or navigated to within a Frame.
        /// </summary>
        public sealed partial class MainPage : Page
        {
            private Inclinometer _inclinometer;

            // This event handler writes the current inclinometer reading to
            // the three text blocks on the app' s main page.

            private async void ReadingChanged(object sender, InclinometerReadingChangedEventArgs e)
            {
                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    InclinometerReading reading = e.Reading;
                    txtPitch.Text = String.Format("{0,5:0.00}", reading.PitchDegrees);
                    txtRoll.Text = String.Format("{0,5:0.00}", reading.RollDegrees);
                    txtYaw.Text = String.Format("{0,5:0.00}", reading.YawDegrees);
                });
            }

            public MainPage()
            {
                this.InitializeComponent();
                _inclinometer = Inclinometer.GetDefault();


                if (_inclinometer != null)
                {
                    // Establish the report interval for all scenarios
                    uint minReportInterval = _inclinometer.MinimumReportInterval;
                    uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
                    _inclinometer.ReportInterval = reportInterval;

                    // Establish the event handler
                    _inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer, InclinometerReadingChangedEventArgs>(ReadingChanged);
                }
            }
        }
    }
```

您需要將之前程式碼片段中的命名空間重新命名為您專案的名稱。 例如，如果您已建立名為 **InclinometerCS** 的專案，則應該將 `namespace App1` 取代為 `namespace InclinometerCS`。

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
            <TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
            <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
            <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
            <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
            <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>

        </Grid>
    </Page>
```

您需要將之前程式碼片段中的第一個部分的類別名稱，換成 app 的命名空間。 例如，如果您已建立名為 **InclinometerCS** 的專案，則應該將 `x:Class="App1.MainPage"` 取代為 `x:Class="InclinometerCS.MainPage"`。 您也應該將 `xmlns:local="using:App1"` 取代為 `xmlns:local="using:InclinometerCS"`。

-   按 F5 或選取 **\[偵錯\]** > **\[開始偵錯\]** 以建置、部署及執行 App。

App 開始執行之後，您就可以移動裝置或使用模擬器工具來變更傾角計值。

-   返回 Visual Studio，然後按 Shift+F5 或選取 **\[偵錯\]** > **\[停止偵錯\]** 以停止 App。

###  <a name="explanation"></a>說明

前面的範例示範了如何只需要撰寫簡短的程式碼，就可以整合 app 中的傾角計輸入。

App 會與 **MainPage** 方法中的預設傾角計建立連線。

```csharp
_inclinometer = Inclinometer.GetDefault();
```

App 會在 **MainPage** 方法內建立報告間隔。 這段程式碼會擷取裝置所支援的最短間隔，並和所要求的 16 毫秒間隔 (重新整理的速率大約是 60-Hz) 比較。 如果支援的最短間隔大於要求的間隔，程式碼會將該值設定為最小值。 否則，就會將該值設定為要求的間隔。

```csharp
uint minReportInterval = _inclinometer.MinimumReportInterval;
uint reportInterval = minReportInterval > 16 ? minReportInterval : 16;
_inclinometer.ReportInterval = reportInterval;
```

會在 **ReadingChanged** 方法中擷取新的傾角計資料。 每次感應器驅動程式收到感應器的新資料時，都會使用這個事件處理常式將值傳送給 app。 應用程式會用下行程式碼登錄這個事件處理常式。

```csharp
_inclinometer.ReadingChanged += new TypedEventHandler<Inclinometer,
InclinometerReadingChangedEventArgs>(ReadingChanged);
```

這些新的值會寫入專案 XAML 中的 TextBlock。

```xml
<TextBlock HorizontalAlignment="Left" Height="21" Margin="0,8,0,0" TextWrapping="Wrap" Text="Pitch: " VerticalAlignment="Top" Width="45" Foreground="#FFF9F4F4"/>
 <TextBlock x:Name="txtPitch" HorizontalAlignment="Left" Height="21" Margin="59,8,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="71" Foreground="#FFFDF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="23" Margin="0,29,0,0" TextWrapping="Wrap" Text="Roll:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F1F1"/>
 <TextBlock x:Name="txtRoll" HorizontalAlignment="Left" Height="23" Margin="59,29,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="50" Foreground="#FFFCF9F9"/>
 <TextBlock HorizontalAlignment="Left" Height="19" Margin="0,56,0,0" TextWrapping="Wrap" Text="Yaw:" VerticalAlignment="Top" Width="55" Foreground="#FFF7F3F3"/>
 <TextBlock x:Name="txtYaw" HorizontalAlignment="Left" Height="19" Margin="55,56,0,0" TextWrapping="Wrap" Text="TextBlock" VerticalAlignment="Top" Width="54" Foreground="#FFF6F2F2"/>
```

