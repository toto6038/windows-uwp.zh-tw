---
description: 本主題提供 Windows Phone Silverlight API 與其通用 Windows 平台 (UWP) 對等 API 的完整對應。
title: WPSL 至 UWP 命名空間和類別對應
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ab6bdace41041f03bd4316b1f65de1a5aeb60835
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167492"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>Windows Phone Silverlight UWP API 對應


本主題提供 Windows Phone Silverlight API 與其通用 Windows 平台 (UWP) 對等 API 的完整對應。 不過，通常沒有一對一的功能對應：這兩個平台在命名空間與類別中都可能有比對方多或少的功能。

當您在 UWP 專案中工作，並重複使用來自 Windows Phone Silverlight 專案的原始程式碼時，對應表格將會對您有幫助。 兩個平台之間的命名空間和類別 (包括 UI 控制項) 的名稱有所不同。 在許多情況下，相當簡單，只要變更命名空間名稱，您的程式碼就會進行編譯。 有時候，除了命名空間名稱之外，類別或 API 名稱也會變更。 其他時候，則需要更費工夫來進行對應，而只有在少數情況下會需要變更方法。

**如何使用資料表：  ** 首先，搜尋您所使用的類別名稱。 當只變更命名空間名稱無法達成對應時，就會列出類別。 如果未列出您的類別，則表示只需要變更命名空間即可達成對應。 因此，請尋找您類別的命名空間名稱，然後您就可以找到對等的 UWP 命名空間名稱。 您的類別會在該命名空間中。 如果未列出您的命名空間，則表示它的名稱未變更。

**注意**   Windows 10 支援比 Windows Phone Store 應用程式更多的 .NET Framework。 例如，Windows 10 有數個 System.servicemodel。 \* 命名空間以及 System.Net、System System.net.networkinformation.networkinformationexception 和 System .Net。
此外，在 Windows 10 app 中，您將可以從 .NET 原生 (一種事先編譯技術，其將 MSIL 轉換為原生可執行機器碼) 獲得助益。 .NET 原生 app 比其對應的 MSIL 啟動更快、使用更少的記憶體，而且消耗較少的電池電力。

| Windows Phone Silverlight | Windows 執行階段 |
| ------------------------- | --------------- |
| 廣告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 類別 | [AdControl](../monetize/display-ads-in-your-app.md) 類別 |
| 鬧鐘、提醒及背景代理程式 | |
| **Microsoft.Phone.BackgroundAgent** 類別 | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 類別 |
| **Microsoft.Phone.Scheduler** 命名空間 | [**Windows.ApplicationModel.Background**](/uwp/api/Windows.ApplicationModel.Background) 命名空間 |
| **Microsoft.Phone.Scheduler.Alarm** 類別 | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 和 [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) 類別 |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** 類別 | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 類別 |
| **Microsoft.Phone.Scheduler.Reminder** 類別 | [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 和 [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) 類別 |
| **Microsoft.Phone.PictureDecoder** 類別 | [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 類別 |
| **Microsoft.Phone.BackgroundAudio** 命名空間 | [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback) 命名空間 |
| **Microsoft.Phone.BackgroundTransfer** 命名空間 | [**Windows.Networking.BackgroundTransfer**](/uwp/api/Windows.Networking.BackgroundTransfer) 命名空間 |
| app 模型和環境 | |
| **System.AppDomain** 類別 | 沒有直接的對等。 請參閱 [**Application**](/uwp/api/Windows.UI.Xaml.Application)、[**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication) 類別 |
| **System.Environment** 類別 | 沒有直接的對等項目 |
| **System.ComponentModel.Annotations** 類別  | 沒有直接的對等項目 |
| **System.ComponentModel.BackgroundWorker** 類別 | [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) 類別 |
| **System.ComponentModel.DesignerProperties** 類別 | [**DesignMode**](/uwp/api/Windows.ApplicationModel.DesignMode) 類別 |
| **System.Threading.Thread**、**System.Threading.ThreadPool** 類別 | [**ThreadPool**](/uwp/api/Windows.System.Threading.ThreadPool) 類別 |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 方法 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 方法 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 屬性 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 屬性 |
| **System.Threading.Timer** 類別 | [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) 類別 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 類別 | [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 類別 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 類別 | [**DispatcherTimer**](/uwp/api/Windows.UI.Xaml.DispatcherTimer) 類別 |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** 類別 | 沒有直接的對等項目 |
| **Microsoft.Expression.Interactivity** 命名空間 | [Microsoft.Xaml.Interactivity](/previous-versions/dn458193(v=vs.120)) 命名空間 |
| **Microsoft.Expression.Interactivity.Core** 命名空間 | [Microsoft.Xaml.Interactions.Core](/previous-versions/dn458182(v=vs.120)) 命名空間 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** 類別 | 沒有直接的對等項目 |
| **Microsoft.Expression.Interactivity.Input** 命名空間 | 沒有直接的對等項目 |
| **Microsoft.Expression.Interactivity.Media** 命名空間 | [Microsoft.Xaml.Interactions.Media](/previous-versions/dn458186(v=vs.120)) 命名空間 |
| **Microsoft.Expression.Shapes** 命名空間 | 沒有直接的對等項目 |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** 介面 | 沒有直接的對等項目 |
| 連絡人和行事曆資料 | |
| **Microsoft.Phone.UserData** 命名空間 | [**Windows.ApplicationModel.Contacts**](/uwp/api/Windows.ApplicationModel.Contacts)、[**Windows.ApplicationModel.Appointments**](/uwp/api/Windows.ApplicationModel.Appointments) 命名空間 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** 類別 | [**Contact**](/uwp/api/Windows.ApplicationModel.Contacts.Contact) 類別 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 類別 | [**AppointmentCalendar**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentCalendar) 類別 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 類別 | [**ContactStore**](/uwp/api/Windows.ApplicationModel.Contacts.ContactStore) 類別 |
| 控制項和 UI 基礎結構 | |
| **ControlTiltEffect. TiltEffect** 類別 | 來自 Windows 執行階段動畫庫的動畫會內建至通用控制項的預設「樣式」中。 請參閱[動畫](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **Microsoft.Phone.Controls** 命名空間 | （[**控制項**](/uwp/api/Windows.UI.Xaml.Controls)命名空間） |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 類別 | [**PopupMenu**](/uwp/api/Windows.UI.Popups.PopupMenu) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 類別 | [**DatePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.DatePickerFlyout) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 類別 | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 類別 | [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 類別 | [**SystemProtection**](/uwp/api/Windows.Phone.System.SystemProtection)、[**WindowActivatedEventArgs**](/uwp/api/Windows.UI.Core.WindowActivatedEventArgs) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 類別 | [**Hub**](/uwp/api/Windows.UI.Xaml.Controls.Hub) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 類別 | [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 類別 | [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 類別 | [**PointerDownThemeAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.PointerDownThemeAnimation) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 類別 | [**TimePickerFlyout**](/uwp/api/Windows.UI.Xaml.Controls.TimePickerFlyout) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 類別 | [**Web**](/uwp/api/Windows.UI.Xaml.Controls.WebView) 類型類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 類別 | 沒有直接的對等項目 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 類別 | 沒有針對一般配置目的的直接對等項目。 [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) 和 [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) 可以用在項目控制項的項目面板範本中。 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 命名空間 | 沒有直接的對等項目 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 命名空間 | 沒有直接的對等項目 |
| **Microsoft.Phone.Globalization** 命名空間 | 沒有直接的對等項目 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** 類別 | [**EasClientDeviceInformation**](/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning.EasClientDeviceInformation)、[**MemoryManager**](/uwp/api/Windows.System.MemoryManager) 類別。 如需更多詳細資料，請參閱[裝置狀態](wpsl-to-uwp-input-and-sensors.md)。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 類別 | 沒有直接的對等項目 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 類別 | [**AdvertisingManager**](/uwp/api/Windows.System.UserProfile.AdvertisingManager) 類別 |
| **System.Windows** 命名空間 | [**Windows.UI.Xaml**](/uwp/api/Windows.UI.Xaml) 命名空間 |
| **System.Windows.Automation** 命名空間 | [**Windows.UI.Xaml.Automation**](/uwp/api/Windows.UI.Xaml.Automation) 命名空間 |
| **System.Windows.Controls**、**System.Windows.Input** 命名空間 | [**Windows.UI.Core**](/uwp/api/Windows.UI.Core)、[**Windows.UI.Input**](/uwp/api/Windows.UI.Input)、[**Windows.UI.Xaml.Controls**](/uwp/api/Windows.UI.Xaml.Controls) 命名空間 |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** 類別 | [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 類別 |
| **System.Windows.Controls.RichTextBox** 類別 | [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) 類別 |
| **System.Windows.Controls.WrapPanel** 類別 | 沒有針對一般配置目的的直接對等項目。 [**ItemsWrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.ItemsWrapGrid) 和 [**WrapGrid**](/uwp/api/Windows.UI.Xaml.Controls.WrapGrid) 可以用在項目控制項的項目面板範本中。 |
| **System.Windows.Controls.Primitives** 命名空間 | [**Windows.UI.Xaml.Controls.Primitives**](/uwp/api/Windows.UI.Xaml.Controls.Primitives) 命名空間 |
| **System.Windows.Controls.Shapes** 命名空間 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 命名空間 |
| **System.Windows.Data** 命名空間 | [**Windows.UI.Xaml.Data**](/uwp/api/Windows.UI.Xaml.Data) 命名空間 |
| **System.Windows.Documents** 命名空間 | [**Windows.UI.Xaml.Documents**](/uwp/api/Windows.UI.Xaml.Documents) 命名空間 |
| **System.Windows.Ink** 命名空間 | 沒有直接的對等項目 |
| **System.Windows.Markup** 命名空間 | [**Windows.UI.Xaml.Markup**](/uwp/api/Windows.UI.Xaml.Markup) 命名空間 | 
| **System.Windows.Navigation** 命名空間 | [**Windows.UI.Xaml.Navigation**](/uwp/api/Windows.UI.Xaml.Navigation) 命名空間 |
| **System.Windows.UIElement.Tap** 事件、**EventHandler&lt;GestureEventArgs&gt;** 委派 | [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped) 事件、[**TappedEventHandler**](/uwp/api/windows.ui.xaml.input.tappedeventhandler) 委派 |
| 資料和服務 |  |
| **System.Data.Linq.DataContext** 類別 | 沒有直接的對等項目 |
| **System.Data.Linq.Mapping.ColumnAttribute** 類別 | 沒有直接的對等項目 |
| **System.Data.Linq.SqlClient.SqlHelpers** 類別 | 沒有直接的對等項目 |
| 裝置 | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 命名空間 | [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)、[**Windows.Devices.Enumeration.Pnp**](/uwp/api/Windows.Devices.Enumeration.Pnp)、[**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input)、[**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) 命名空間 |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** 類別 | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 類別。 還有 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 類別 (僅限 Windows)。 |
| **Microsoft.Devices.CameraButtons** 類別 | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 類別 |
| **Microsoft.Devices.CameraVideoBrushExtensions** 類別 | [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) 類別 |
| **Microsoft.Devices.Environment** 類別 | 沒有直接的對等。 為了因應此情況，請使用條件式編譯並定義自訂符號。 或者，您也可以使用 [IsAttached](/dotnet/api/system.diagnostics.debugger.isattached#System_Diagnostics_Debugger_IsAttached) 屬性來設計一個因應措施。 |
| **Microsoft.Devices.MediaHistory** 類別 | 沒有直接的對等項目 |
| **Microsoft.Devices.VibrateController** 類別 | [**VibrationDevice**](/uwp/api/Windows.Phone.Devices.Notification.VibrationDevice) 類別 |
| **Microsoft.Devices.Radio.FMRadio** 類別 | 沒有直接的對等項目 |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** 類別 | 在 [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors) 命名空間中 |
| **Microsoft.Devices.Sensors.Gyroscope** 類別 | [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer) 類別 |
| **Microsoft.Devices.Sensors.Motion** 類別 | [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) 類別 |
| 全球化 | |
| **System.object** 命名空間 | [**Windows. 全球化**](/uwp/api/Windows.Globalization) 命名空間 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 屬性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 屬性 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 屬性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 屬性 |
| 圖形與動畫 | |
| Node.js、[類型名稱架構類別庫](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41))、[內容管線類別庫](/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) ** \* ** | 沒有直接的對等。 一般而言，請使用 [Microsoft DirectX](/windows/desktop/directx) 搭配 C++。 請參閱[開發遊戲](/previous-versions/windows/apps/hh452744(v=win.10))和 [DirectX 與 XAML 互通性](/previous-versions/windows/apps/hh825871(v=win.10))。 |
| **Microsoft.Xna.Framework.Audio.Microphone** 類別 | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 類別 |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 類別 | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 類別 |
| **Microsoft.Xna.Framework.GamerServices** 命名空間 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](/uwp/api/Windows.Phone.System.UserProfile.GameServices.Core) 命名空間 |
| **Microsoft.Xna.Framework.GamerServices.Guide** 類別 | 沒有直接的對等項目 |
| **Microsoft.Xna.Framework.Input.GamePad** 類別 | [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 類別 |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 類別 | [**GestureRecognizer**](/uwp/api/Windows.UI.Input.GestureRecognizer) 類別 |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** 類別 | [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) 類別 |
| **Microsoft.Xna.Framework.Media.MediaQueue** 類別 | [**SystemMediaTransportControls**](/uwp/api/Windows.Media.SystemMediaTransportControls) 類別 |
| **Microsoft.Xna.Framework.Media.Playlist** 類別 | [**BackgroundMediaPlayer**](/uwp/api/Windows.Media.Playback.BackgroundMediaPlayer) 類別 |
| **System.Windows.Media** 命名空間 | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) 命名空間 |
| **System.Windows.Media.RadialGradientBrush** 類別 | 沒有直接的對等。 請參閱[媒體和圖形](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **System.Windows.Media.Animation** 命名空間 | [**Windows.UI.Xaml.Media.Animation**](/uwp/api/Windows.UI.Xaml.Media.Animation) 命名空間 |
| **System.Windows.Media.Effects** 命名空間 | 沒有直接的對等項目 |
| **System.Windows.Media.Imaging** 命名空間 | [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging) 命名空間 |
| **System.Windows.Media.Media3D** 命名空間 | [**Windows.UI.Xaml.Media.Media3D**](/uwp/api/Windows.UI.Xaml.Media.Media3D) 命名空間 |
| **System.Windows.Shapes** 命名空間 | [**App.xaml**](/uwp/api/Windows.UI.Xaml.Shapes) 命名空間 |
| 啟動程式與選擇器 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** 類別 | [**ContactPicker**](/uwp/api/Windows.ApplicationModel.Contacts.ContactPicker) 類別 |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** 類別 | [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) 命名空間 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 類別 | [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) 類別。 還有 [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI) 類別 (僅限 Windows)。 |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 類別 ([**RequestAppPurchaseAsync**](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) 方法) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** 類別 | [**Launcher**](/uwp/api/Windows.System.Launcher) 類別 |
| **Microsoft.Phone.Tasks.EmailComposeTask** 類別 | [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) 類別 |
| **Microsoft.Phone.Tasks.GameInviteTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.PhoneCallTask** 類別 | [**PhoneCallManager**](/uwp/api/Windows.ApplicationModel.Calls.PhoneCallManager) 類別 |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 類別 | [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 類別 |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 類別 | [**AppointmentManager**](/uwp/api/Windows.ApplicationModel.Appointments.AppointmentManager) 類別 |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** 類別 | [**StoredContact**](/uwp/api/Windows.Phone.PersonalInformation.StoredContact) 類別 (僅限 Windows Phone) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** 類別 | [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 類別 |
| Location | |
| **System.Device.Location** 命名空間 | [**Windows. 地理位置**](/uwp/api/Windows.Devices.Geolocation) 命名空間 |
| **System.Device.GeoCoordinateWatcher** 類別 | [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 類別 |
| 地圖 | |
| **Microsoft.Phone.Maps** 命名空間 | [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間 |
| **Microsoft.Phone.Maps.Controls** 命名空間 | [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps) 命名空間 |
| **Microsoft.Phone.Maps.Controls.Map** 類別 | [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) 類別 |
| **Microsoft.Phone.Maps.Services** 命名空間 | [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps) 命名空間 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** 類別 | [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) 類別 |
| **System.Device.Location.GeoCoordinate** 類別 | [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) 類別 |
| **Microsoft.Phone.Maps.Services.Route** 類別 | [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) 類別 |
| **Microsoft.Phone.Maps.Services.RouteQuery** 類別 | [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) 類別 |
| 創造營收 | |
| **Microsoft.Phone.Marketplace** 命名空間 | [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store) 命名空間 |
| 媒體 | |
| **Microsoft.Phone.Media** 命名空間 | [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) 類別 |
| 網路功能 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 類別 | [**Hostname**](/uwp/api/Windows.Networking.HostName)、[**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 類別 | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 類別 | [**ConnectionProfile**](/uwp/api/Windows.Networking.Connectivity.ConnectionProfile) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 類別 | [**NetworkInformation**](/uwp/api/Windows.Networking.Connectivity.NetworkInformation) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 類別 | 沒有直接的對等項目 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Networking.Voip** 命名空間 | 沒有直接的對等項目 |
| **System.Net.CookieCollection** 類別 | 仍受支援，但缺少某些屬性 (例如 IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** 類別，和與 **System.Net.WebClient** 相關的類似類別 | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) 。 衍生自 [System.Net.Http.StreamContent](/previous-versions/visualstudio/hh138119(v=vs.118)) 以測量進度。 |
| **System.Net.DnsEndPoint**、**IPAddress** 類別 | 這些類別仍受支援，但缺少某些屬性。 或者，移植到 [**HostName**](/uwp/api/Windows.Networking.HostName) 類別。 |
| **System.Net.HttpUtility** 類別 | [**HtmlFormatHelper**](/uwp/api/Windows.ApplicationModel.DataTransfer.HtmlFormatHelper) 類別 |
| **System.Net.HttpWebRequest** 類別 | 部分支援，但具前瞻性的建議替代方案為 [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118)))。 這些 API 使用 [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) 來代表 HTTP 要求。 |
| **System.Net.HttpWebResponse** 類別 | 仍受支援，但使用 Dispose() 而不是 Close()。 但具前瞻性的建議替代方案為 [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118)))。 這些 API 使用 [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) 來代表 HTTP 回應。 |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 類別 | 仍受支援，但建構函式除外。 |
| **System.Net.OpenReadCompletedEventArgs** 類別，和與 **System.Net.WebClient** 相關的類似類別 | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.Sockets.Socket** 類別 | 仍受支援，但使用 Dispose() 而不是 Close()。 或者，移植到 [**StreamSocket**](/uwp/api/Windows.Networking.Sockets.StreamSocket) 類別。 |
| **System.Net.Sockets.SocketException** 類別 | 仍受支援，但使用 SocketErrorCode 屬性而不是 ErrorCode。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** 類別 | [**DatagramSocket**](/uwp/api/Windows.Networking.Sockets.DatagramSocket) 類別 |
| **System.Net.UploadProgressChangedEventArgs** 類別，和與 **System.Net.WebClient** 相關的類似類別 | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebClient** 類別 | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.Net.WebRequest** 類別 | 部分支援 (不同組的屬性)，但具前瞻性的建議替代方案為 [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118)))。 這些 API 使用 [System.Net.Http.HttpRequestMessage](/previous-versions/visualstudio/hh159020(v=vs.118)) 來代表 HTTP 要求。 |
| **System.Net.WebResponse** 類別 | 仍受支援，但使用 Dispose() 而不是 Close()。 但具前瞻性的建議替代方案為 [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118)))。 這些 API 使用 [System.Net.Http.HttpResponseMessage](/dotnet/api/system.net.http.httpresponsemessage) 來代表 HTTP 回應。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 類別 | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 類別 | [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient) 類別 (或 [System.Net.Http.HttpClient](/previous-versions/visualstudio/hh193681(v=vs.118))) |
| **System.UriFormatException** 類別 | **System.FormatException** 類別 |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 命名空間 | [**Windows.UI.Notifications**](/uwp/api/Windows.UI.Notifications)、[**Windows.Networking.PushNotifications**](/uwp/api/Windows.Networking.PushNotifications) 命名空間 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 類別 | [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification) 類別 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 類別 | [**PushNotificationChannel**](/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannel) 類別 |
| 程式設計 | |
| **System** 命名空間 | [**Windows Foundation**](/uwp/api/Windows.Foundation) 命名空間 |
| **System.Diagnostics.StackFrame**、**StackTrace** 類別 | 沒有直接的對等項目 |
| **System.Diagnostics** 命名空間 | [**Windows.Foundation.Diagnostics**](/uwp/api/Windows.Foundation.Diagnostics) 命名空間 |
| **System.ICloneable** 介面 | 傳回適當類型的自訂方法。 |
| **System.Reflection.Emit.ILGenerator** 類別 | 沒有直接的對等項目 |
| 反應擴充功能 | |
| **Microsoft.Phone.Reactive** 命名空間 | 沒有直接的對等項目 |
| 反射 | |
| **System.Type** 類別 | **System.Reflection.TypeInfo** 類別。 請參閱 [.NET Framework 中適用於 UWP apps 的反映](/dotnet/framework/reflection-and-codedom/reflection-for-windows-store-apps)。 |
| 資源 | |
| **System.Resources.ResourceManager** 類別 | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) 和 [**WA.Resources**](/uwp/api/Windows.ApplicationModel.Resources) 命名空間、[**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) 類別。 請參閱[在 Windows 執行階段 app 中建立及擷取資源](/previous-versions/windows/apps/hh694557(v=vs.140))。 |
| 防護晶片 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** 類別 | [**SmartCardConnection**](/uwp/api/Windows.Devices.SmartCards.SmartCardConnection) 類別 |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 類別 | [**SmartCardReader**](/uwp/api/Windows.Devices.SmartCards.SmartCardReader) 類別 |
| 安全性 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** 類別 | [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** 類別 | [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 類別 | [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 類別 | [**CryptographicBuffer**](/uwp/api/Windows.Security.Cryptography.CryptographicBuffer) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 類別 | [**CertificateEnrollmentManager**](/uwp/api/Windows.Security.Cryptography.Certificates.CertificateEnrollmentManager) 類別 |
| Shell | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 類別 | [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 類別 | [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) 類別 (在 [**PrimaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.primarycommands) 屬性內使用時) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 類別 | [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton) 類別 (在 [**SecondaryCommands**](/uwp/api/windows.ui.xaml.controls.commandbar.secondarycommands) 屬性內使用時) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** 類別 | [**TileTemplateType**](/uwp/api/Windows.UI.Notifications.TileTemplateType) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 類別 | [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication)、[**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 類別 | [**StatusBarProgressIndicator**](/uwp/api/Windows.UI.ViewManagement.StatusBarProgressIndicator) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 類別 | [**SecondaryTile**](/uwp/api/Windows.UI.StartScreen.SecondaryTile) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 類別 | [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 類別 | [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 類別 | [**StatusBar**](/uwp/api/Windows.UI.ViewManagement.StatusBar) 類別 |
| 儲存空間和 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** 類別 | [**KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) 類別 |
| **System.IO** 命名空間 | [**Windows.Storage**](/uwp/api/Windows.Storage)、[**Windows.Storage.Streams**](/uwp/api/Windows.Storage.Streams) 命名空間 |
| **System.IO.Directory** 類別 | [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) 類別 |
| **System.IO.File** 類別 | [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 和 [**PathIO**](/uwp/api/Windows.Storage.PathIO) 類別
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 類別 |[**ApplicationData.LocalFolder**](/uwp/api/windows.storage.applicationdata.localfolder) 屬性 |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 類別 | [**ApplicationData.LocalSettings**](/uwp/api/windows.storage.applicationdata.localsettings) 屬性 |
| **System.IO.Stream** 類別 | 仍受支援，但使用 ReadAsync() 與 WriteAsync()，而不是 BeginRead()/EndRead() 與 BeginWrite()/EndWrite()。 |
| 電子錢包 | |
| **Microsoft.Phone.Wallet** 命名空間 | [**Windows.ApplicationModel.Wallet**](/uwp/api/Windows.ApplicationModel.Wallet) 命名空間 |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 方法 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 方法 |


下一個主題是[移植專案](wpsl-to-uwp-porting-to-a-uwp-project.md)。