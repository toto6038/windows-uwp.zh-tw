---
author: PatrickFarley
Description: Follow these best practices for geofencing in your app.
title: 地理柵欄應用程式的指導方針
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, 地圖, 位置, 地理柵欄
ms.localizationpriority: medium
ms.openlocfilehash: 86104f00ed0189290fd0cd718042573d9d592cc3
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "5822158"
---
# <a name="guidelines-for-geofencing-apps"></a>地理柵欄應用程式的指導方針




**重要 API**

-   [**Geofence 類別 (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn263587)
-   [**Geolocator 類別 (XAML)**](https://msdn.microsoft.com/library/windows/apps/br225534)

在應用程式中遵循這些適用於[**地理柵欄**](https://msdn.microsoft.com/library/windows/apps/dn263744)的最佳做法。

## <a name="recommendations"></a>建議


-   如果您的應用程式在發生 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 事件時需要網際網路存取，在建立地理柵欄之前請先檢查是否有網際網路存取。
    -   如果應用程式目前沒有網際網路存取，在設定地理柵欄之前，您可以提示使用者先連線到網際網路。
    -   如果無法使用網際網路存取，請避免耗用地理柵欄位置檢查所需的電源。
-   當地理柵欄事件指出 [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) 或 **Exited** 狀態的變更，請檢查時間戳記和目前的位置，確保地理柵欄通知的相關性。 如需詳細資訊，請參閱下方的**檢查時間戳記和目前位置**。
下方 (#timestamp) 以取得詳細資訊。
-   建立例外狀況，以管理裝置無法存取位置資訊的情況，並在必要時通知使用者。 位置資訊可能因為權限已關閉、裝置不包含 GPS 無線電、GPS 訊號被阻擋或 Wi-Fi 訊號不夠強而無法使用。
-   通常不需要同時在前景和背景接聽地理柵欄事件。 不過，如果應用程式需要同時在前景和背景中接聽地理柵欄事件：

    -   呼叫 [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) 方法，查明事件是否已經發生。
    -   使用者看不到您的應用程式時，取消登錄前景事件接聽程式，在重新看得到時重新登錄。

    如需程式碼範例和詳細資訊，請參閱[背景與前景接聽程式](#background-and-foreground-listeners)。

-   每個應用程式不要使用 1000 個以上的地理柵欄。 實際上，系統在每個應用程式支援數千個地理柵欄，使用低於 1000 個地理柵欄，可協助降低應用程式的記憶體使用量，進而讓應用程式維持良好效能。
-   不要建立半徑小於 50 公尺的地理柵欄。 如果您的 app 需要使用半徑較小的地理柵欄，請建議使用者在具有 GPS 無線電的裝置上使用您的 app，以確保最佳效能。

## <a name="additional-usage-guidance"></a>其他用法指導方針

### <a name="checking-the-time-stamp-and-current-location"></a>檢查時間戳記和目前位置

如果某個事件指出 [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) 或 **Exited** 狀態的變更，請檢查事件的時間戳記和您目前的位置。 系統沒有足夠的資源啟動背景工作、使用者沒有注意到通知，或裝置處於待機狀態 (在 Windows 上) 等因素，都可能影響使用者實際處理事件的時機。 例如，可能發生以下順序的情況：

-   您的應用程式建立地理柵欄並監視地理柵欄的進入和退出事件。
-   使用者移動地理柵欄內的裝置，導致觸發進入事件。
-   您的應用程式傳送通知給使用者，告知他們現在已位於地理柵欄內。
-   使用者太忙碌，10 分鐘後才注意到通知。
-   在這 10 分鐘延遲過程中，使用者已經離開地理柵欄。

您可以從時間戳記看出之前有動作發生。 您可以從目前位置看出使用者現在已經離開地理柵欄。 視您應用程式的功能而定，您可能希望過濾掉此事件。

### <a name="background-and-foreground-listeners"></a>背景與前景接聽程式

一般而言，您的應用程式不需要同時接聽前景與背景工作中的 [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) 事件。 在碰到可能兩者都需要的狀況，最簡潔的處理方法是讓背景工作處理通知。 如果您同時設定了前景和背景地理柵欄接聽程式，則無法保證哪一個會先觸發，因此您必須一律呼叫 [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) 方法來查明是否有事件發生。

如果您已設定前景和背景地理柵欄接聽程式，則當使用者看不到 app 時，應該取消登錄前景事件接聽程式，並在 app 再次看得見時重新登錄 app。 這裡是登錄可見度事件的部分範例程式碼。

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

當可見度改變時，您就可以啟用或停用前景事件處理常式，如下所示。

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>調整地理柵欄大小

雖然 GPS 可以提供最準確的位置資訊，不過地理柵欄也可以使用 Wi-Fi 或其他定位感應器來判斷使用者的目前位置。 不過，使用那些其他方法可能會影響您可建立的地理柵欄大小。 如果正確性很低，建立小型地理柵欄不會有用處。 一般而言，建議您不要建立半徑小於 50 公尺的地理柵欄。 此外，Windows 上只會定期執行地理柵欄背景工作；如果您使用小型地理柵欄，您有可能會完全錯失 [**Enter**](https://msdn.microsoft.com/library/windows/apps/dn263660) 或 **Exit** 事件。

如果您的 app 需要使用半徑較小的地理柵欄，請建議使用者在具有 GPS 無線電的裝置上使用您的 app，以確保最佳效能。

## <a name="related-topics"></a>相關主題


* [設定地理柵欄](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [取得目前的位置](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [UWP 位置範例 (地理位置)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
