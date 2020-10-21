---
title: 設定地理柵欄
description: 在您的 app 中設定地理柵欄，並了解如何在前景和背景中處理通知。
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, uwp, 地圖, 位置, 地理柵欄, 通知
ms.localizationpriority: medium
ms.openlocfilehash: ca6dad1a96f37e3a308ad10c84293a8d49fb0329
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297592"
---
# <a name="set-up-a-geofence"></a>設定地理柵欄

> [!NOTE]
> [**隨 mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 和地圖服務會 Requite 稱為 [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)的地圖服務驗證金鑰。 如需有關取得及設定地圖驗證金鑰的詳細資訊，請參閱[要求地圖驗證金鑰](authentication-key.md)。

在您的應用程式中設定 [**地理柵欄**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) ，並瞭解如何在前景和背景處理通知。

**提示**：若要深入了解如何在 app 中存取位置，請從 GitHub 的 [Windows-universal-samples 存放庫](https://github.com/Microsoft/Windows-universal-samples)下載下列範例。

-   [通用 Windows 平台 (UWP) 地圖範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>啟用位置功能


1.  在 \[**方案總管**\] 中按兩下 **package.appxmanifest**，然後選取 \[**功能**\] 索引標籤。
2.  在 \[**功能**\] 清單中，選取 \[**位置**\]。 這會將 `Location` 裝置功能新增至封裝資訊清單檔案中。

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>設定地理柵欄


### <a name="step-1-request-access-to-the-users-location"></a>步驟 1：要求使用者位置的存取權

**重要**：嘗試存取使用者的位置之前，您必須先使用 [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 方法要求使用者位置的存取權。 您必須從 UI 執行緒呼叫 **RequestAccessAsync** 方法，而且您的 app 必須在前景中。 在使用者將權限授與您的 app 之後，您的 app 才能存取使用者的位置資訊。

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

[**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) 方法會提示使用者提供可存取其位置的權限。 只會提示使用者一次 (每一 app)。 在使用者第一次授與或拒絕權限之後，這個方法就不會再顯示權限提示。 為了協助使用者在出現過提示之後變更位置權限，建議您提供一個位置設定連結，如本主題稍後所示範。

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>步驟 2：登錄地理柵欄狀態及位置權限的變更

在這個範例中，**switch** 陳述式是與 **accessStatus** (來自先前的範例) 搭配使用，只有在獲允許存取使用者位置的情況下才有作用。 如果獲允許存取使用者的位置，程式碼就會存取目前的地理柵欄、登錄地理柵欄狀態變更，以及登錄位置權限的變更。

**秘訣** 使用地理柵欄時，請使用來自 GeofenceMonitor 類別的 [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) 事件來監視位置權限的變更，而不要使用來自 Geolocator 類別的 StatusChanged 事件。 **Disabled** 為 [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus) 等同於已停用的 [**PositionStatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus)，兩者都指出 app 沒有存取使用者位置的權限。

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;


    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

然後，當瀏覽離開前景 app 時，取消登錄事件接聽程式。

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>步驟 3：建立地理柵欄

現在您已經準備好，可以定義並設定 [**Geofence**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 物件。 視您的需求而定，有數個不同的建構函式多載可選擇。 在最基本的地理柵欄建構函式中，請僅指定此處顯示的 [**Id**](/uwp/api/windows.devices.geolocation.geofencing.geofence.id) 與 [**Geoshape**](/uwp/api/windows.devices.geolocation.geofencing.geofence.geoshape)。

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);

```

您可以使用其中一個其他的建構函式，來進一步微調您的地理柵欄。 在下一個範例中，地理柵欄建構函式會指定這些額外的參數：

-   [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) - 指出您希望收到通知的地理柵欄事件：進入已定義的區域、離開已定義的區域，或移除地理柵欄。
-   [**SingleUse**](/uwp/api/windows.devices.geolocation.geofencing.geofence.singleuse) - 在符合所監控之地理柵欄的所有狀態之後，將會移除該地理柵欄。
-   [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) - 指出使用者必須在已定義的區域內或外多久的時間，才會觸發進入/離開事件。
-   [**StartTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.starttime) - 指出開始監控地理柵欄的時間。
-   [**Duration**](/uwp/api/windows.devices.geolocation.geofencing.geofence.duration) - 指出監控地理柵欄的期間。

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

建立後，請記得登錄您的新 [**Geofence**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) 至監視器。

```csharp
// Register the geofence
try {
   GeofenceMonitor.Current.Geofences.Add(geofence);
} catch {
   // Handle failure to add geofence
}
```

### <a name="step-4-handle-changes-in-location-permissions"></a>步驟 4：處理位置權限的變更

[**GeofenceMonitor**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitor) 物件會觸發 [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) 事件，以指出使用者的位置設定已變更。 該事件會透過引數的 **sender.Status** 屬性 (類型為 [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus)) 傳遞對應的狀態。 請注意，此方法並不是從 UI 執行緒呼叫，且 [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 物件會叫用 UI 變更。

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>設定前景通知


建立地理柵欄之後，您必須新增邏輯來處理地理柵欄事件發生時會發生的情況。 根據您已經設定的 [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates)，您可能會在下列情況下收到某個事件：

-   使用者進入相關的區域時。
-   使用者離開相關的區域時。
-   地理柵欄過期或遭到移除時。 請注意，系統不會針對移除事件啟用背景 app。

您可以在 app 執行時，直接從 app 接聽事件，或登錄背景工作，讓您可以在事件發生時，收到背景通知。

### <a name="step-1-register-for-geofence-state-change-events"></a>步驟 1：登錄地理柵欄狀態變更事件

若要讓您的 app 收到地理柵欄狀態變更的前景通知，您必須登錄事件處理常式。 這通常是在您建立地理柵欄時設定。

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>步驟 2：實作地理柵欄事件處理常式

下一步是實作事件處理常式。 此處所採取的動作取決於您 app 使用地理柵欄的目的。

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>設定背景通知


建立地理柵欄之後，您必須新增邏輯來處理地理柵欄事件發生時會發生的情況。 根據您已經設定的 [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates)，您可能會在下列情況下收到某個事件：

-   使用者進入相關的區域時。
-   使用者離開相關的區域時。
-   地理柵欄過期或遭到移除時。 請注意，系統不會針對移除事件啟用背景 app。

在背景接聽地理柵欄事件

-   在您 app 的資訊清單中宣告背景工作
-   在應用程式中登錄背景工作。 如果您的 app 需要網際網路存取 (例如存取雲端服務)，您可以在觸發事件時針對此狀況設定旗標。 您也可以設定一個旗標來確定觸發事件時使用者在場，以確定使用者確實收到通知。
-   當您的 app 在前景執行時，提示使用者將位置權限授與您的 app。

### <a name="step-1-register-for-geofence-state-change-events"></a>步驟 1：登錄地理柵欄狀態變更事件

在您 app 資訊清單的 \[**宣告**\] 索引標籤底下，新增位置背景工作的宣告。 若要這樣做：

-   新增 \[**背景工作**\] 類型的宣告。
-   設定 \[**位置**\] 的屬性工作類型。
-   在您的 app 中設定一個進入點，以便在觸發事件時呼叫。

### <a name="step-2-register-the-background-task"></a>步驟 2：登錄背景工作

此步驟中的程式碼會登錄地理柵欄背景工作。 請記住，在地理柵欄建立時，我們已檢查過位置權限。

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>步驟 3：處理背景通知

您為通知使用者所採取的動作取決於您 app 所執行的工作，但您可以顯示快顯通知、播放音效或更新動態磚。 此步驟中的程式碼會處理通知。

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>變更隱私權設定


如果位置隱私權設定不允許您的 app 存取使用者的位置，建議您提供一個可連到 \[**設定**\] app 中 \[**位置隱私權設定**\] 的便利連結。 在這個範例中，是使用「超連結」控制項來瀏覽至 `ms-settings:privacy-location` URI。

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

或者，您的 app 也可以呼叫 [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) 方法，以從程式碼啟動 \[**設定**\] app。 如需詳細資訊，請參閱[啟動 Windows 設定 app](../launch-resume/launch-settings-app.md)。

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>為您的 app 進行測試和偵錯


為地理柵欄 app 進行測試和偵錯相當具挑戰性，因為它們倚賴裝置的位置。 我們在這裡概述幾個方法，可用來測試前景和背景地理柵欄。

**為地理柵欄 app 進行偵錯**

1.  實際將裝置移到新的位置。
2.  輸入一個地理柵欄來測試，方法是建立一個包含您目前實際位置的地理柵欄區域，這樣您便位於該地理柵欄中，而「地理柵欄已輸入」事件會立即觸發。
3.  使用 Microsoft Visual Studio 模擬器來模擬裝置的位置。

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>針對在前景執行的地理柵欄 app 進行測試和偵錯

**針對在前景執行的地理柵欄 app 進行測試**

1.  在 Visual Studio 中建立您的 app。
2.  在 Visual Studio 模擬器中啟動您的應用程式。
3.  使用這些工具來模擬地理柵欄區域內外的不同位置。 請確定等待的時間夠長，應超過 [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) 屬性指定的時間才能觸發事件。 請注意，您必須接受為 app 啟用位置權限的提示。 如需有關模擬位置的詳細資訊，請參閱[設定裝置的模擬地理位置](/previous-versions/hh441475(v=vs.120)#BKMK_Set_the_simulated_geo_location_of_the_device)。
4.  您也可以使用模擬器來 預估柵欄的大小，以及要在不同速度被偵測到時所需的大約暫留時間。

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>針對在背景執行的地理柵欄 app 進行測試和偵錯

**針對在背景執行的地理柵欄 app 進行測試**

1.  在 Visual Studio 中建立您的 app。 請注意，您的應用程式應該設定 \[**位置**\] 背景工作類型。
2.  先在本機部署應用程式。
3.  關閉目前正在本機執行的應用程式。
4.  在 Visual Studio 模擬器中啟動您的應用程式。 請注意，在模擬器中，一次只支援在一個應用程式上進行背景地理柵欄模擬。 請勿在模擬器中啟動多個地理柵欄應用程式。
5.  在模擬器中，模擬您地理柵欄區域內外的不同位置。 請確定等待的時間夠長，應超過 [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) 才能觸發事件。 請注意，您必須接受為 app 啟用位置權限的提示。
6.  使用 Visual Studio 觸發位置背景工作。 如需有關在 Visual Studio 中觸發背景工作的詳細資訊，請參閱[如何觸發背景工作](/previous-versions/hh974425(v=vs.120)#BKMK_Trigger_background_tasks)。

## <a name="troubleshoot-your-app"></a>針對您的 app 進行疑難排解


必須先在裝置上啟用 \[**位置**\]，您的 app 才能存取位置。 在 \[**設定**\] app 中，確認已開啟下列 \[**位置隱私權設定**\]：

-   已將 **此裝置的位置** 設為 **開啟** \(不適用於 Windows 10 行動裝置版\)
-   已將定位服務設定的 \[**位置**\] 設為 \[**開啟**\]
-   在 \[**選擇可以使用您的位置的應用程式**\] 底下，將您的 app 設為 \[**開啟**\]

## <a name="related-topics"></a>相關主題

* [UWP 地理位置範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [地理柵欄的設計指導方針](./guidelines-for-geofencing.md)
* [定位感知應用程式的設計指導方針](./guidelines-and-checklist-for-detecting-location.md)
