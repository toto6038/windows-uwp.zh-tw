---
author: PatrickFarley
Description: Learn how to use the powerful Visits Tracking feature for more practical location tracking.
title: 關於使用行止動線追蹤功能的指導方針
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.author: pafarley
ms.date: 05/18/2017
ms.topic: article
keywords: windows 10, uwp, 地圖, 位置, geovisit, geovisits
ms.localizationpriority: medium
ms.openlocfilehash: 3a78b2689a10dff50696e5e65cc44f79a6f1ea7d
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5545520"
---
# <a name="guidelines-for-using-visits-tracking"></a>關於使用行止動線追蹤功能的指導方針

行止動線 (Visits) 功能簡化位置追蹤程序，使之在許多應用程式的實際用途上變得更有效率。 「行止動線」(Visit) 定義為使用者進入與離開的顯著地理區域。 行止動線與[地理柵欄](guidelines-for-geofencing.md)相似之處在於，兩者都可讓應用程式只在使用者進入或離開感興趣的特定區域時才收到通知，不再需要持續進行可能會耗盡電池使用時間的位置追蹤。 但與地理柵欄不同的是，行止動線是在平台層級上動態識別，不需要由個別應用程式明確定義。 此外，應用程式選擇追蹤哪些行止動線是依據單一細微性設定來處理，而不是依據個別地點的訂閱。

## <a name="preliminary-setup"></a>初步設定

繼續進行之前，請確定您的應用程式可以存取裝置的位置。 您必須在資訊清單中宣告 `Location`，並呼叫 **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** 方法，以確保使用者將位置權限授與應用程式。 如需此作法的詳細資訊，請參閱[取得使用者的位置](get-location.md)。 

請記得將 `Geolocation` 命名空間新增至您的類別。 本指南中所有的程式碼片段都必須這樣做才能運作。

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>檢查最新行止動線
最簡單方式是使用行止動線追蹤功能，擷取最後一次已知與行止動線相關的狀態變更。 狀態變更是平台切換事件，此時使用者進入/離開顯著位置、自上次回報後發生顯著移動，或是使用者位置遺失 (請參閱 **[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)** 列舉)。 狀態變更以 **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)** 執行個體來表示。 若要擷取上次記錄狀態變更的 **Geovisit** 執行個體，只需使用 **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)** 類別中指定的方法即可。

> [!NOTE]
> 檢查上次記錄的行止動線不保證系統目前正在追蹤行止動線。 為了在行止動線有動靜時加以追蹤，您必須在前景進行監視，或註冊進行背景追蹤 (請參閱以下各節)。

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>剖析 Geovisit 執行個體 (選用)
下列方法會將儲存在 **Geovisit** 執行個體中的所有資訊轉換成可輕鬆判讀的字串。 可用於本指南中任何的案例，以協助提供有關回報中行止動線的意見反應。

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>在前景監視行止動線

上一節中使用的 **GeovisitMonitor** 類別也會處理接聽一段時間變更狀態的案例。 您可以執行此作業，方式為具現化此類別、註冊其事件的處理常式方法，然後呼叫 `Start` 方法。

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

在此範例中，`OnVisitStateChanged` 方法會處理傳入的行止動線報告。 對應的 **Geovisit** 執行個體是透過事件參數傳入。

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
應用程式完成行止動線相關狀態變更的監視時，應該停止監視並取消註冊事件處理常式。 每當應用程式暫停或關閉時，也必須這樣做。

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>在背景監視行止動線

您還可以實作在背景工作中監視行止動線的功能，如此一來，即使應用程式未開放，也能在裝置上處理行止動線相關活動。 這是建議使用的方法，因為較為通用且更加節能。 

本指南將會使用[建立和註冊跨處理序背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)中的模型，其中主要應用程式檔案位於一個專案，而背景工作檔案取得位於相同方案的另一個專案。 如果您不熟悉實作背景工作，建議您主要依照其中指引進行，進行下面必要的替代以建立行止動線相關背景工作。

> [!NOTE]
> 為了簡單起見，下列程式碼片段省略了一些重要的功能，例如錯誤處理和本機存放區。 如需強固的背景行止動線處理實作，請參閱[範例應用程式](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)。


首先，確定應用程式已宣告背景工作權限。 在 *Package.appxmanifest* 檔案的 `Application/Extensions` 元素中，新增下列延伸模組 (新增 `Extensions` 元素，如果還不存在的話)。

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

接下來，在背景工作類別的定義中貼上下列程式碼。 此背景工作的 `Run` 方法只是將觸發程序的詳細資料 (其中包含行止動線資訊) 傳遞到不同的方法中。

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

在此同一類別的任意位置上定義 `GetVisitReports` 方法。

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

接下來，您必須在應用程式的主要專案中，執行此背景工作的註冊作業。 建立可由部分使用者動作呼叫的，或在每次啟動類別時呼叫的註冊方法。

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

這可確保 `Tasks` 命名空間中名為 `VisitBackgroundTask` 的背景工作類別會執行某個觸發類型為 `location` 的動作。 

您的應用程式現在應該可以註冊行止動線處理背景工作，而且每當裝置記錄一次行止動線相關狀態變更時，都必須啟動這項工作。 您必須在背景工作類別中填入邏輯，以判斷如何處理此狀態變更資訊。

## <a name="related-topics"></a>相關主題
* [建立和註冊跨處理序背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [取得使用者的位置](get-location.md)
* [Windows.Devices.Geolocation 命名空間](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)