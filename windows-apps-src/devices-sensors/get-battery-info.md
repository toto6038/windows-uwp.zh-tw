---
ms.assetid: 90BB59FC-90FE-453E-A8DE-9315E29EB98C
title: 取得電池資訊
description: 了解如何使用 Windows.Devices.Power 命名空間中的 API 取得詳細的電池資訊。
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 81f4232d038b89f2c49cf584346d632911fb70e2
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7841270"
---
# <a name="get-battery-information"></a>取得電池資訊


** 重要 API **

-   [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017)
-   [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432)

了解如何使用 [**Windows.Devices.Power**](https://msdn.microsoft.com/library/windows/apps/Dn895017) 命名空間中的 API 取得詳細的電池資訊。 *電池報告* ([**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)) 描述電池或電池組的充電、容量和狀態。 這個主題示範您的 app 如何取得電池報告和收到變更通知。 程式碼範例來自於本主題結尾列出的基本電池 app。

## <a name="get-aggregate-battery-report"></a>取得彙總電池報告


有些裝置有一個以上的電池，每個電池對裝置整體電源容量貢獻多少電力不一定顯而易見。 這時就需要使用 [**AggregateBattery**](https://msdn.microsoft.com/library/windows/apps/Dn895011) 類別。 *彙總電池*代表所有連接到裝置的電池控制器，可以提供單一整體的 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 物件。

**注意：**[**電池**](https://msdn.microsoft.com/library/windows/apps/Dn895004)類別實際上對應到電池控制器。 根據裝置而定，控制器有時連接到實體電池，有時連接到裝置外殼。 因此，即使沒有電池，也有可能可以建立電池物件。 但有些時候，電池物件可能是 **Null**。

一旦您有彙總電池物件，請呼叫 [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) 取得對應的 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)。

```csharp
private void RequestAggregateBatteryReport()
{
    // Create aggregate battery object
    var aggBattery = Battery.AggregateBattery;

    // Get report
    var report = aggBattery.GetReport();

    // Update UI
    AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
}
```

## <a name="get-individual-battery-reports"></a>取得個別的電池報告

您也可以為個別電池建立 [**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 物件。 使用 [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getdeviceselector.aspx) 與 [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/BR225432) 方法來取得一組 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 物件，代表任何已連接到裝置的電池控制器。 然後，使用所需 **DeviceInformation** 物件的 **Id** 屬性，透過 [**FromIdAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.fromidasync.aspx) 方法建立對應的 [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004)。 最後，呼叫 [**GetReport**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.getreport) 以取得個別的電池報告。

這個範例示範如何為所有連接到裝置的電池建立電池報告。

```csharp
async private void RequestIndividualBatteryReports()
{
    // Find batteries 
    var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
    foreach(DeviceInformation device in deviceInfo)
    {
        try
        {
        // Create battery object
        var battery = await Battery.FromIdAsync(device.Id);

        // Get report
        var report = battery.GetReport();

        // Update UI
        AddReportUI(BatteryReportPanel, report, battery.DeviceId);
        }
        catch { /* Add error handling, as applicable */ }
    }
}
```

## <a name="access-report-details"></a>存取報告詳細資料

[**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005) 物件提供許多電池資訊。 如需詳細資訊，請參閱 API 參考中的屬性：**Status** ([**BatteryStatus**](https://msdn.microsoft.com/library/windows/apps/Dn818458) 列舉)、[**ChargeRateInMilliwatts**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.chargerateinmilliwatts.aspx)、[**DesignCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.designcapacityinmilliwatthours.aspx)、[**FullChargeCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.fullchargecapacityinmilliwatthours.aspx) 和 [**RemainingCapacityInMilliwattHours**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.batteryreport.remainingcapacityinmilliwatthours)。 這個範例顯示基本電池 app (本主題稍後提供) 使用的一些電池報告屬性。

```csharp
...
TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };
...
...
```

## <a name="request-report-updates"></a>要求報告更新

當電池的充電、容量或狀態變更時，[**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 物件會觸發 [**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) 事件。 通常會在狀態變更時立即觸發，至於其他所有變更，則是定期觸發。 這個範例示範如何註冊以取得電池報告更新。

```csharp
...
Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
...
```

## <a name="handle-report-updates"></a>處理報告更新

當電池更新時，[**ReportUpdated**](https://msdn.microsoft.com/library/windows/apps/windows.devices.power.battery.reportupdated) 事件會傳遞對應的 [**Battery**](https://msdn.microsoft.com/library/windows/apps/Dn895004) 物件給事件處理常式方法。 但是，這個事件處理常式不是從 UI 執行緒呼叫。 您需要使用 [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 物件來叫用任何 UI 變更，如這個範例中所示。

```csharp
async private void AggregateBattery_ReportUpdated(Battery sender, object args)
{
    if (reportRequested)
    {

        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }
        });
    }
}
```

## <a name="example-basic-battery-app"></a>範例：基本電池 app

在 Microsoft Visual Studio 中建立下列基本電池 app 來測試這些 API。 從 Visual Studio 起始頁，按一下 **\[新增專案\]**，然後在 **\[Visual C# &gt; Windows &gt; 通用\]** 範本下，使用 **\[空白應用程式\]** 範本建立新的 app。

接下來，開啟 **MainPage.xaml** 檔案，然後將以下的 XML 複製到這個檔案中 (取代原來的內容)。

```xml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" >
        <StackPanel VerticalAlignment="Center" Margin="15,30,0,0" >
            <RadioButton x:Name="AggregateButton" Content="Aggregate results" GroupName="Type" IsChecked="True" />
            <RadioButton x:Name="IndividualButton" Content="Individual results" GroupName="Type" IsChecked="False" />
        </StackPanel>
        <StackPanel Orientation="Horizontal">
        <Button x:Name="GetBatteryReportButton" 
                Content="Get battery report" 
                Margin="15,15,0,0" 
                Click="GetBatteryReport"/>
        </StackPanel>
        <StackPanel x:Name="BatteryReportPanel" Margin="15,15,0,0"/>
    </StackPanel>
</Page>
```

如果您的 app 名稱不是 **App1**，您需要將之前程式碼片段中的類別名稱開頭部分，換成 app 的命名空間。 例如，如果您建立名為 **BasicBatteryApp** 的專案，則應該將 `x:Class="App1.MainPage"` 取代為 `x:Class="BasicBatteryApp.MainPage"`。 您也應該將 `xmlns:local="using:App1"` 取代為 `xmlns:local="using:BasicBatteryApp"`。

接下來，開啟專案的 **MainPage.xaml.cs** 檔案，然後以下列程式碼取代現有的程式碼。

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Devices.Enumeration;
using Windows.Devices.Power;
using Windows.UI.Core;

namespace App1
{
    public sealed partial class MainPage : Page
    {
        bool reportRequested = false;
        public MainPage()
        {
            this.InitializeComponent();
            Battery.AggregateBattery.ReportUpdated += AggregateBattery_ReportUpdated;
        }


        private void GetBatteryReport(object sender, RoutedEventArgs e)
        {
            // Clear UI
            BatteryReportPanel.Children.Clear();


            if (AggregateButton.IsChecked == true)
            {
                // Request aggregate battery report
                RequestAggregateBatteryReport();
            }
            else
            {
                // Request individual battery report
                RequestIndividualBatteryReports();
            }

            // Note request
            reportRequested = true;
        }

        private void RequestAggregateBatteryReport()
        {
            // Create aggregate battery object
            var aggBattery = Battery.AggregateBattery;

            // Get report
            var report = aggBattery.GetReport();

            // Update UI
            AddReportUI(BatteryReportPanel, report, aggBattery.DeviceId);
        }

        async private void RequestIndividualBatteryReports()
        {
            // Find batteries 
            var deviceInfo = await DeviceInformation.FindAllAsync(Battery.GetDeviceSelector());
            foreach(DeviceInformation device in deviceInfo)
            {
                try
                {
                // Create battery object
                var battery = await Battery.FromIdAsync(device.Id);

                // Get report
                var report = battery.GetReport();

                // Update UI
                AddReportUI(BatteryReportPanel, report, battery.DeviceId);
                }
                catch { /* Add error handling, as applicable */ }
            }
        }


        private void AddReportUI(StackPanel sp, BatteryReport report, string DeviceID)
        {
            // Create battery report UI
            TextBlock txt1 = new TextBlock { Text = "Device ID: " + DeviceID };
            txt1.FontSize = 15;
            txt1.Margin = new Thickness(0, 15, 0, 0);
            txt1.TextWrapping = TextWrapping.WrapWholeWords;

            TextBlock txt2 = new TextBlock { Text = "Battery status: " + report.Status.ToString() };
            txt2.FontStyle = Windows.UI.Text.FontStyle.Italic;
            txt2.Margin = new Thickness(0, 0, 0, 15);

            TextBlock txt3 = new TextBlock { Text = "Charge rate (mW): " + report.ChargeRateInMilliwatts.ToString() };
            TextBlock txt4 = new TextBlock { Text = "Design energy capacity (mWh): " + report.DesignCapacityInMilliwattHours.ToString() };
            TextBlock txt5 = new TextBlock { Text = "Fully-charged energy capacity (mWh): " + report.FullChargeCapacityInMilliwattHours.ToString() };
            TextBlock txt6 = new TextBlock { Text = "Remaining energy capacity (mWh): " + report.RemainingCapacityInMilliwattHours.ToString() };

            // Create energy capacity progress bar & labels
            TextBlock pbLabel = new TextBlock { Text = "Percent remaining energy capacity" };
            pbLabel.Margin = new Thickness(0,10, 0, 5);
            pbLabel.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            ProgressBar pb = new ProgressBar();
            pb.Margin = new Thickness(0, 5, 0, 0);
            pb.Width = 200;
            pb.Height = 10;
            pb.IsIndeterminate = false;
            pb.HorizontalAlignment = HorizontalAlignment.Left;

            TextBlock pbPercent = new TextBlock();
            pbPercent.Margin = new Thickness(0, 5, 0, 10);
            pbPercent.FontFamily = new FontFamily("Segoe UI");
            pbLabel.FontSize = 11;

            // Disable progress bar if values are null
            if ((report.FullChargeCapacityInMilliwattHours == null)||
                (report.RemainingCapacityInMilliwattHours == null))
            {
                pb.IsEnabled = false;
                pbPercent.Text = "N/A";
            }
            else
            {
                pb.IsEnabled = true;
                pb.Maximum = Convert.ToDouble(report.FullChargeCapacityInMilliwattHours);
                pb.Value = Convert.ToDouble(report.RemainingCapacityInMilliwattHours);
                pbPercent.Text = ((pb.Value / pb.Maximum) * 100).ToString("F2") + "%";
            }

            // Add controls to stackpanel
            sp.Children.Add(txt1);
            sp.Children.Add(txt2);
            sp.Children.Add(txt3);
            sp.Children.Add(txt4);
            sp.Children.Add(txt5);
            sp.Children.Add(txt6);
            sp.Children.Add(pbLabel);
            sp.Children.Add(pb);
            sp.Children.Add(pbPercent);
        }

        async private void AggregateBattery_ReportUpdated(Battery sender, object args)
        {
            if (reportRequested)
            {

                await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
                {
                    // Clear UI
                    BatteryReportPanel.Children.Clear();


                    if (AggregateButton.IsChecked == true)
                    {
                        // Request aggregate battery report
                        RequestAggregateBatteryReport();
                    }
                    else
                    {
                        // Request individual battery report
                        RequestIndividualBatteryReports();
                    }
                });
            }
        }
    }
}
```

如果您的 app 名稱不是 **App1**，您需要將之前範例中的命名空間，重新命名為您的專案名稱。 例如，如果您建立名為 **BasicBatteryApp** 的專案，則應該將命名空間 `App1` 取代為命名空間 `BasicBatteryApp`。

最後，若要執行此基本電池 app：在 [**偵錯**] 功能表中，按一下 [**開始偵錯**] 來測試方案。

**提示：** 若要從[**BatteryReport**](https://msdn.microsoft.com/library/windows/apps/Dn895005)物件接收數值，偵錯您的應用程式在**本機電腦**或外部**裝置**（例如 Windows Phone) 上。 在裝置模擬器上偵錯時，**BatteryReport** 物件會傳回 **Null** 到容量和速率屬性。

 

