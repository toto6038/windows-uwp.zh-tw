---
author: stevewhims
description: 本主題提供 WindowsPhone Silverlight Api 與其通用 Windows 平台 (UWP) 對等的完整對應。
title: WindowsPhone Silverlight 至 UWP 命名空間與類別對應
ms.assetid: 33f06706-4790-48f3-a2e4-ebef9ddb61a4
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 54118b41fc1f3036dddba9a0cfb8ecd860c1e233
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5986209"
---
# <a name="windowsphone-silverlight-to-uwp-api-mappings"></a>WindowsPhone Silverlight UWP API 對應


本主題提供 WindowsPhone Silverlight Api 與其通用 Windows 平台 (UWP) 對等的完整對應。 不過，通常沒有一對一的功能對應：這兩個平台在命名空間與類別中都可能有比對方多或少的功能。

當您使用 UWP 專案中，當您重新使用來自 WindowsPhone Silverlight 專案的原始程式碼時，對應表格有助於您。 兩個平台之間的命名空間和類別 (包括 UI 控制項) 的名稱有所不同。 在許多情況下，相當簡單，只要變更命名空間名稱，您的程式碼就會進行編譯。 有時候，除了命名空間名稱之外，類別或 API 名稱也會變更。 其他時候，則需要更費工夫來進行對應，而只有在少數情況下會需要變更方法。

**如何使用此表格：** 首先，搜尋您目前使用之類別的名稱。 當只變更命名空間名稱無法達成對應時，就會列出類別。 如果未列出您的類別，則表示只需要變更命名空間即可達成對應。 因此，請尋找您類別的命名空間名稱，然後您就可以找到對等的 UWP 命名空間名稱。 您的類別會在該命名空間中。 如果未列出您的命名空間，則表示它的名稱未變更。

**注意：** windows 10 支援更多的.NET Framework 比 Windows Phone 市集 app 的功能。 例如，windows 10 具有數個 System.ServiceModel.\* 命名空間，以及 System.Net、 System.Net.NetworkInformation 與 System.Net.Sockets。
此外，在 windows 10 app 中，您將可以從.NET 原生，其中的事先編譯技術，其將 MSIL 轉換成原生可執行的機器程式碼。 .NET 原生 app 比其對應的 MSIL 啟動更快、使用更少的記憶體，而且消耗較少的電池電力。

| WindowsPhone Silverlight | Windows 執行階段 |
| ------------------------- | --------------- |
| 廣告 | |
| **Microsoft.Advertising.Mobile.UI.AdControl** 類別 | [AdControl](http://msdn.microsoft.com/library/advertising-windows-sdk-api-reference-adcontrol.aspx) 類別 |
| 鬧鐘、提醒及背景代理程式 | |
| **Microsoft.Phone.BackgroundAgent** 類別 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 類別 |
| **Microsoft.Phone.Scheduler** 命名空間 | [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847) 命名空間 |
| **Microsoft.Phone.Scheduler.Alarm** 類別 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 和 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 類別 |
| **Microsoft.Phone.Scheduler.PeriodicTask**、**ScheduledAction**、**ScheduledActionService**、**ScheduledTask**、**ScheduledTaskAgent** 類別 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 類別 |
| **Microsoft.Phone.Scheduler.Reminder** 類別 | [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 和 [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 類別 |
| **Microsoft.Phone.PictureDecoder** 類別 | [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 類別 |
| **Microsoft.Phone.BackgroundAudio** 命名空間 | [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562) 命名空間 |
| **Microsoft.Phone.BackgroundTransfer** 命名空間 | [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空間 |
| app 模型和環境 | |
| **System.AppDomain** 類別 | 沒有直接的對等項目。 請參閱 [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324)、[**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016) 類別 |
| **System.Environment** 類別 | 沒有直接的對等項目 |
| **System.ComponentModel.Annotations** 類別  | 沒有直接的對等項目 |
| **System.ComponentModel.BackgroundWorker** 類別 | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) 類別 |
| **System.ComponentModel.DesignerProperties** 類別 | [**DesignMode**](https://msdn.microsoft.com/library/windows/apps/br224664) 類別 |
| **System.Threading.Thread**、**System.Threading.ThreadPool** 類別 | [**ThreadPool**](https://msdn.microsoft.com/library/windows/apps/br229621) 類別 |
| (ST = **System.Threading**) <br/> **ST.Thread.MemoryBarrier** 方法 | (ST = **System.Threading**) <br/> **ST.Interlocked.MemoryBarrier** 方法 |
| (ST = **System.Threading**) <br/> **ST.Thread.ManagedThreadId** 屬性 | (S = **System**) <br/> **S.Environment.ManagedThreadId** 屬性 |
| **System.Threading.Timer** 類別 | [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/br230587) 類別 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.Dispatcher** 類別 | [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 類別 |
| (SWT = **System.Windows.Threading**) <br/> **SWT.DispatcherTimer** 類別 | [**DispatcherTimer**](https://msdn.microsoft.com/library/windows/apps/br244250) 類別 |
| Blend for Visual Studio | |
| (MEDC = **Microsoft.Expression.Drawing.Core**) <br/> **MEDC.GeometryHelper** 類別 | 沒有直接的對等項目 |
| **Microsoft.Expression.Interactivity** 命名空間 | [Microsoft.Xaml.Interactivity](http://go.microsoft.com/fwlink/p/?LinkId=328776) 命名空間 |
| **Microsoft.Expression.Interactivity.Core** 命名空間 | [Microsoft.Xaml.Interactions.Core](http://go.microsoft.com/fwlink/p/?LinkId=328773) 命名空間 |
| (MEIC = **Microsoft.Expression.Interactivity.Core**) <br/> **MEIC.ExtendedVisualStateManager** 類別 | 沒有直接的對等項目 |
| **Microsoft.Expression.Interactivity.Input** 命名空間 | 沒有直接的對等項目 |
| **Microsoft.Expression.Interactivity.Media** 命名空間 | [Microsoft.Xaml.Interactions.Media](http://go.microsoft.com/fwlink/p/?LinkId=328775) 命名空間 |
| **Microsoft.Expression.Shapes** 命名空間 | 沒有直接的對等項目 |
| (MI = **Microsoft.Internal**) <br/> **MI.IManagedFrameworkInternalHelper** 介面 | 沒有直接的對等項目 |
| 連絡人和行事曆資料 | |
| **Microsoft.Phone.UserData** 命名空間 | [**Windows.ApplicationModel.Contacts**](https://msdn.microsoft.com/library/windows/apps/br225002)、[**Windows.ApplicationModel.Appointments**](https://msdn.microsoft.com/library/windows/apps/dn263359) 命名空間 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Account**、**ContactAddress**、**ContactCompanyInformation**、**ContactEmailAddress**、**ContactPhoneNumber** 類別 | [**Contact**](https://msdn.microsoft.com/library/windows/apps/br224849) 類別 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Appointments** 類別 | [**AppointmentCalendar**](https://msdn.microsoft.com/library/windows/apps/dn596134) 類別 |
| (MPU = **Microsoft.Phone.UserData**) <br/> **MPU.Contacts** 類別 | [**ContactStore**](https://msdn.microsoft.com/library/windows/apps/dn624859) 類別 |
| 控制項和 UI 基礎結構 | |
| **ControlTiltEffect.TiltEffect** 類別 | 來自 Windows 執行階段動畫庫的動畫會內建至通用控制項的預設「樣式」中。 請參閱[動畫](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **Microsoft.Phone.Controls** 命名空間 | [**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空間 |
| (MPC = **Microsoft.Phone.Controls**) <br/> **MPC.ContextMenu** 類別 | [**PopupMenu**](https://msdn.microsoft.com/library/windows/apps/br208693) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.DatePickerPage** 類別 | [**DatePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn625013) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.GestureListener** 類別 | [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.LongListSelector** 類別 | [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.ObscuredEventArgs** 類別 | [**SystemProtection**](https://msdn.microsoft.com/library/windows/apps/jj585394)、[**WindowActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208377) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.Panorama** 類別 | [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationFrame**、<br/>(SWN = **System.Windows.Navigation**) <br/>**SWN.NavigationService** 類別 | [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.PhoneApplicationPage** 類別 | [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TiltEffect** 類別 | [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/hh969164) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.TimePickerPage** 類別 | [**TimePickerFlyout**](https://msdn.microsoft.com/library/windows/apps/dn608313) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowser** 類別 | [**WebView**](https://msdn.microsoft.com/library/windows/apps/br227702) 類別 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WebBrowserExtensions** 類別 | 沒有直接的對等項目 |
| (MPC = **Microsoft.Phone.Controls**) <br/>**MPC.WrapPanel** 類別 | 沒有針對一般配置目的的直接對等項目。 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) 和 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) 可以用在項目控制項的項目面板範本中。 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq** 命名空間 | 沒有直接的對等項目 |
| (MPD = **Microsoft.Phone.Data**) <br/>**MPD.Linq.Mapping** 命名空間 | 沒有直接的對等項目 |
| **Microsoft.Phone.Globalization** 命名空間 | 沒有直接的對等項目 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.DeviceExtendedProperties**、**DeviceStatus** 類別 | [**EasClientDeviceInformation**](https://msdn.microsoft.com/library/windows/apps/hh701390)、[**MemoryManager**](https://msdn.microsoft.com/library/windows/apps/dn633831) 類別。 如需更多詳細資料，請參閱[裝置狀態](wpsl-to-uwp-input-and-sensors.md)。 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.MediaCapabilities** 類別 | 沒有直接的對等項目 |
| (MPI = **Microsoft.Phone.Info**) <br/>**MPI.UserExtendedProperties** 類別 | [**AdvertisingManager**](https://msdn.microsoft.com/library/windows/apps/dn363391) 類別 |
| **System.Windows** 命名空間 | [**Windows.UI.Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 命名空間 |
| **System.Windows.Automation** 命名空間 | [**Windows.UI.Xaml.Automation**](https://msdn.microsoft.com/library/windows/apps/br209179) 命名空間 |
| **System.Windows.Controls**、**System.Windows.Input** 命名空間 | [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)、[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)、[**Windows.UI.Xaml.Controls**](https://msdn.microsoft.com/library/windows/apps/br227716) 命名空間 |
| **System.Windows.Controls.DrawingSurface**、**DrawingSurfaceBackgroundGrid** 類別 | [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834) 類別 |
| **System.Windows.Controls.RichTextBox** 類別 | [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/br227548) 類別 |
| **System.Windows.Controls.WrapPanel** 類別 | 沒有針對一般配置目的的直接對等項目。 [**ItemsWrapGrid**](https://msdn.microsoft.com/library/windows/apps/dn298849) 和 [**WrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227717) 可以用在項目控制項的項目面板範本中。 |
| **System.Windows.Controls.Primitives** 命名空間 | [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818) 命名空間 |
| **System.Windows.Controls.Shapes** 命名空間 | [**Windows.UI.Xaml.Controls.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 命名空間 |
| **System.Windows.Data** 命名空間 | [**Windows.UI.Xaml.Data**](https://msdn.microsoft.com/library/windows/apps/br209917) 命名空間 |
| **System.Windows.Documents** 命名空間 | [**Windows.UI.Xaml.Documents**](https://msdn.microsoft.com/library/windows/apps/br209984) 命名空間 |
| **System.Windows.Ink** 命名空間 | 沒有直接的對等項目 |
| **System.Windows.Markup** 命名空間 | [**Windows.UI.Xaml.Markup**](https://msdn.microsoft.com/library/windows/apps/br228046) 命名空間 | 
| **System.Windows.Navigation** 命名空間 | [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300) 命名空間 |
| **System.Windows.UIElement.Tap** 事件、**EventHandler&lt;GestureEventArgs&gt;** 委派 | [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985) 事件、[**TappedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227993) 委派 |
| 資料和服務 |  |
| **System.Data.Linq.DataContext** 類別 | 沒有直接的對等項目 |
| **System.Data.Linq.Mapping.ColumnAttribute** 類別 | 沒有直接的對等項目 |
| **System.Data.Linq.SqlClient.SqlHelpers** 類別 | 沒有直接的對等項目 |
| 裝置 | |
| **Microsoft.Devices**、**Microsoft.Devices.Sensors** 命名空間 | [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/br225459)、[**Windows.Devices.Enumeration.Pnp**](https://msdn.microsoft.com/library/windows/apps/br225517)、[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)、[**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) 命名空間 |
| **Microsoft.Devices.Camera**、**Microsoft.Devices.PhotoCamera** 類別 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 類別。 還有 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 類別 (僅限 Windows)。 |
| **Microsoft.Devices.CameraButtons** 類別 | [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 類別 |
| **Microsoft.Devices.CameraVideoBrushExtensions** 類別 | [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 類別 |
| **Microsoft.Devices.Environment** 類別 | 沒有直接的對等項目。 為了因應此情況，請使用條件式編譯並定義自訂符號。 或者，您也可以使用 [IsAttached](http://msdn.microsoft.com/library/e299w87h.aspx) 屬性來設計一個因應措施。 |
| **Microsoft.Devices.MediaHistory** 類別 | 沒有直接的對等項目 |
| **Microsoft.Devices.VibrateController** 類別 | [**VibrationDevice**](https://msdn.microsoft.com/library/windows/apps/jj207230) 類別 |
| **Microsoft.Devices.Radio.FMRadio** 類別 | 沒有直接的對等項目 |
| **Microsoft.Devices.Sensors.Accelerometer**、**Compass** 類別 | 在 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/br206408) 命名空間中 |
| **Microsoft.Devices.Sensors.Gyroscope** 類別 | [**Gyrometer**](https://msdn.microsoft.com/library/windows/apps/br225718) 類別 |
| **Microsoft.Devices.Sensors.Motion** 類別 | [**Inclinometer**](https://msdn.microsoft.com/library/windows/apps/br225766) 類別 |
| 全球化 | |
| **System.Globalization** 命名空間 | [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 命名空間 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentCulture** 屬性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentCulture** 屬性 |
| (ST = **System.Threading**) <br/> **ST.Thread.CurrentUICulture** 屬性 | (SG = **System.Globalization**) <br/> **S.CultureInfo.CurrentUICulture** 屬性 |
| 圖形和動畫 | |
| **Microsoft.Xna.Framework.\*** 命名空間、[XNA Framework 類別庫](http://go.microsoft.com/fwlink/p/?LinkId=263769)、[內容管線類別庫](http://go.microsoft.com/fwlink/p/?LinkId=263770) | 沒有直接的對等項目。 一般而言，請使用 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) 搭配 C++。 請參閱[開發遊戲](https://msdn.microsoft.com/library/windows/apps/hh452744)和 [DirectX 與 XAML 互通性](https://msdn.microsoft.com/library/windows/apps/hh825871)。 |
| **Microsoft.Xna.Framework.Audio.Microphone** 類別 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 類別 |
| **Microsoft.Xna.Framework.Audio.SoundEffect** 類別 | [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 類別 |
| **Microsoft.Xna.Framework.GamerServices** 命名空間 | (WPS = **Windows.Phone.System**) <br/> [**WPS.UserProfile.GameServices.Core**](https://msdn.microsoft.com/library/windows/apps/jj207609) 命名空間 |
| **Microsoft.Xna.Framework.GamerServices.Guide** 類別 | 沒有直接的對等項目 |
| **Microsoft.Xna.Framework.Input.GamePad** 類別 | [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 類別 |
| **Microsoft.Xna.Framework.Input.Touch.TouchPanel** 類別 | [**GestureRecognizer**](https://msdn.microsoft.com/library/windows/apps/br241937) 類別 |
| (MXFM = **Microsoft.Xna.Framework.Media**) <br/> **MXFM.MediaLibrary**, **MXFM.PhoneExtensions.MediaLibraryExtensions** 類別 | [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 類別 |
| **Microsoft.Xna.Framework.Media.MediaQueue** 類別 | [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) 類別 |
| **Microsoft.Xna.Framework.Media.Playlist** 類別 | [**BackgroundMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652527) 類別 |
| **System.Windows.Media** 命名空間 | [**Windows.UI.Xaml.Media**](/uwp/api/Windows.UI.Xaml.Media) 命名空間 |
| **System.Windows.Media.RadialGradientBrush** 類別 | 沒有直接的對等項目。 請參閱[媒體和圖形](wpsl-to-uwp-porting-xaml-and-ui.md)。 |
| **System.Windows.Media.Animation** 命名空間 | [**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/br243232) 命名空間 |
| **System.Windows.Media.Effects** 命名空間 | 沒有直接的對等項目 |
| **System.Windows.Media.Imaging** 命名空間 | [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 命名空間 |
| **System.Windows.Media.Media3D** 命名空間 | [**Windows.UI.Xaml.Media.Media3D**](https://msdn.microsoft.com/library/windows/apps/br243274) 命名空間 |
| **System.Windows.Shapes** 命名空間 | [**Windows.UI.Xaml.Shapes**](/uwp/api/Windows.UI.Xaml.Shapes) 命名空間 |
| 啟動程式與選擇器 | |
| **Microsoft.Phone.Tasks.AddressChooserTask**、**EmailAddressChooserTask**、**PhoneNumberChooserTask** 類別 | [**ContactPicker**](https://msdn.microsoft.com/library/windows/apps/br224913) 類別 |
| **Microsoft.Phone.Tasks.AddWalletItemTask**、**AddWalletItemResult** 類別 | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) 命名空間 |
| **Microsoft.Phone.Tasks.BingMapsDirectionsTask**、**BingMapsTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.CameraCaptureTask** 類別 | [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 類別。 還有 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 類別 (僅限 Windows)。 |
| **Microsoft.Phone.Tasks.MarketplaceDetailTask** | [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 類別 ([**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) 方法) |
| **Microsoft.Phone.Tasks.ConnectionSettingsTask**、**MarketplaceHubTask**、**MarketplaceReviewTask**、**MarketplaceSearchTask**、**MediaPlayerLauncher**、**SearchTask**、**SmsComposeTask**、**WebBrowserTask** 類別 | [**Launcher**](https://msdn.microsoft.com/library/windows/apps/br241801) 類別 |
| **Microsoft.Phone.Tasks.EmailComposeTask** 類別 | [**EmailMessage**](https://msdn.microsoft.com/library/windows/apps/dn631270) 類別 |
| **Microsoft.Phone.Tasks.GameInviteTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.MapDownloaderTask**、**MapsDirectionsTask**、**MapsTask**、**MapUpdaterTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.PhoneCallTask** 類別 | [**PhoneCallManager**](https://msdn.microsoft.com/library/windows/apps/dn624832) 類別 |
| **Microsoft.Phone.Tasks.PhotoChooserTask** 類別 | [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 類別 |
| **Microsoft.Phone.Tasks.SaveAppointmentTask** 類別 | [**AppointmentManager**](https://msdn.microsoft.com/library/windows/apps/dn297254) 類別 |
| **Microsoft.Phone.Tasks.SaveContactTask**、**SaveEmailAddressTask**、**SavePhoneNumberTask** 類別 | [**StoredContact**](https://msdn.microsoft.com/library/windows/apps/jj207727) 類別 (僅限 Windows Phone) | 
| **Microsoft.Phone.Tasks.SaveRingtoneTask** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Tasks.ShareLinkTask**、**ShareMediaTask**、**ShareStatusTask** 類別 | [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) 類別 |
| 位置 | |
| **System.Device.Location** 命名空間 | [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603) 命名空間 |
| **System.Device.GeoCoordinateWatcher** 類別 | [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) 類別 |
| 地圖 | |
| **Microsoft.Phone.Maps** 命名空間 | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間 |
| **Microsoft.Phone.Maps.Controls** 命名空間 | [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空間 |
| **Microsoft.Phone.Maps.Controls.Map** 類別 | [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) 類別 |
| **Microsoft.Phone.Maps.Services** 命名空間 | [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間 |
| **Microsoft.Phone.Maps.Services.GeocodeQuery**、**ReverseGeocodeQuery** 類別 | [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) 類別 |
| **System.Device.Location.GeoCoordinate** 類別 | [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) 類別 |
| **Microsoft.Phone.Maps.Services.Route** 類別 | [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) 類別 |
| **Microsoft.Phone.Maps.Services.RouteQuery** 類別 | [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) 類別 |
| 賺錢 | |
| **Microsoft.Phone.Marketplace** 命名空間 | [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197) 命名空間 |
| 媒體 | |
| **Microsoft.Phone.Media** 命名空間 | [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 類別 |
| 網路功能 | |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.DeviceNetworkInformation** 類別 | [**Hostname**](https://msdn.microsoft.com/library/windows/apps/br207113)、[**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterface** 類別 | [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceInfo** 類別 | [**ConnectionProfile**](https://msdn.microsoft.com/library/windows/apps/br207249) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.NetworkInterfaceList** 類別 | [**NetworkInformation**](https://msdn.microsoft.com/library/windows/apps/br207293) 類別 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.SocketExtensions** 類別 | 沒有直接的對等項目 |
| (MPNN = **Microsoft.Phone.Net.NetworkInformation**) <br/> **MPNN.WebRequestExtensions** 類別 | 沒有直接的對等項目 |
| **Microsoft.Phone.Networking.Voip** 命名空間 | 沒有直接的對等項目 |
| **System.Net.CookieCollection** 類別 | 仍受支援，但缺少某些屬性 (例如 IsReadOnly) |
| **System.Net.DownloadProgressChangedEventArgs** 類別，和與 **System.Net.WebClient** 相關的類似類別 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))。 衍生自 [System.Net.Http.StreamContent](https://msdn.microsoft.com/library/system.net.http.streamcontent.aspx) 以測量進度。 |
| **System.Net.DnsEndPoint**、**IPAddress** 類別 | 這些類別仍受支援，但缺少某些屬性。 或者，移植到 [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) 類別。 |
| **System.Net.HttpUtility** 類別 | [**HtmlFormatHelper**](https://msdn.microsoft.com/library/windows/apps/hh738437) 類別 |
| **System.Net.HttpWebRequest** 類別 | 部分支援，但具前瞻性的建議替代方案為 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))。 這些 API 使用 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) 來代表 HTTP 要求。 |
| **System.Net.HttpWebResponse** 類別 | 仍受支援，但使用 Dispose() 而不是 Close()。 但具前瞻性的建議替代方案為 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))。 這些 API 使用 [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) 來代表 HTTP 回應。 |
| (SNN = **System.Net.NetworkInformation**) <br/> **SNN.NetworkChange** 類別 | 仍受支援，但建構函式除外。 |
| **System.Net.OpenReadCompletedEventArgs** 類別，和與 **System.Net.WebClient** 相關的類似類別 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.Net.Sockets.Socket** 類別 | 仍受支援，但使用 Dispose() 而不是 Close()。 或者，移植到 [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) 類別。 |
| **System.Net.Sockets.SocketException** 類別 | 仍受支援，但使用 SocketErrorCode 屬性而不是 ErrorCode。 |
| **System.Net.Sockets.UdpAnySourceMulticastClient**、**UdpSingleSourceMulticastClient** 類別 | [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) 類別 |
| **System.Net.UploadProgressChangedEventArgs** 類別，和與 **System.Net.WebClient** 相關的類似類別 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.Net.WebClient** 類別 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.Net.WebRequest** 類別 | 部分支援 (不同組的屬性)，但具前瞻性的建議替代方案為 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))。 這些 API 使用 [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) 來代表 HTTP 要求。 |
| **System.Net.WebResponse** 類別 | 仍受支援，但使用 Dispose() 而不是 Close()。 但具前瞻性的建議替代方案為 [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx))。 這些 API 使用 [System.Net.Http.HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx) 來代表 HTTP 回應。 |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventArgs** 類別 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| (SN = **System.Net**) <br/> **SN.WriteStreamClosedEventHandler** 類別 | [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) 類別 (或 [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient.aspx)) |
| **System.UriFormatException** 類別 | **System.FormatException** 類別 |
| 通知 | |
| MPN = **Microsoft.Phone.Notification** 命名空間 | [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661)、[**Windows.Networking.PushNotifications**](https://msdn.microsoft.com/library/windows/apps/br241307) 命名空間 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotification** 類別 | [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616) 類別 |
| MPN = **Microsoft.Phone.Notification** <br/> **MPN.HttpNotificationChannel** 類別 | [**PushNotificationChannel**](https://msdn.microsoft.com/library/windows/apps/br241283) 類別 |
| 程式設計 | |
| **System** 命名空間 | [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021) 命名空間 |
| **System.Diagnostics.StackFrame**、**StackTrace** 類別 | 沒有直接的對等項目 |
| **System.Diagnostics** 命名空間 | [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/br206677) 命名空間 |
| **System.ICloneable** 介面 | 傳回適當類型的自訂方法。 |
| **System.Reflection.Emit.ILGenerator** 類別 | 沒有直接的對等項目 |
| 反應擴充功能 | |
| **Microsoft.Phone.Reactive** 命名空間 | 沒有直接的對等項目 |
| 反映 | |
| **System.Type** 類別 | **System.Reflection.TypeInfo** 類別。 請參閱 [.NET Framework 中適用於 UWP apps 的反映](https://msdn.microsoft.com/library/hh535795.aspx)。 |
| 資源 | |
| **System.Resources.ResourceManager** 類別 | (WA = **Windows.ApplicationModel**)<br/>[**WA.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 和 [**WA.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) 命名空間、[**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078) 類別。 請參閱[在 Windows 執行階段 app 中建立及擷取資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh694557.aspx)。 |
| 防護晶片 | |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementChannel**、**MPS.SecureElementSession** 類別 | [**SmartCardConnection**](https://msdn.microsoft.com/library/windows/apps/dn608002) 類別 |
| (MPS = **Microsoft.Phone.SecureElement**) <br/> **MPS.SecureElementReader** 類別 | [**SmartCardReader**](https://msdn.microsoft.com/library/windows/apps/dn263857) 類別 |
| 安全性 | |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.Aes**、**SSC.RSA** 類別 | [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.HMACSHA256**、**SSC.SHA256** 類別 | [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.ProtectedData** 類別 | [**DataProtectionProvider**](https://msdn.microsoft.com/library/windows/apps/br241559) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.RandomNumberGenerator** 類別 | [**CryptographicBuffer**](https://msdn.microsoft.com/library/windows/apps/br227092) 類別 |
| (SSC = **System.Security.Cryptography**) <br/> **SSC.X509Certificates.X509Certificate** 類別 | [**CertificateEnrollmentManager**](https://msdn.microsoft.com/library/windows/apps/br212075) 類別 |
| 殼層 | |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBar** 類別 | [**CommandBar**](https://msdn.microsoft.com/library/windows/apps/dn279427) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarIconButton** 類別 | [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 類別 (在 [**PrimaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279430) 屬性內使用時) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ApplicationBarMenuItem** 類別 | [**AppBarButton**](https://msdn.microsoft.com/library/windows/apps/dn279244) 類別 (在 [**SecondaryCommands**](https://msdn.microsoft.com/library/windows/apps/dn279434) 屬性內使用時) |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.CycleTileData**、**MPSh.FlipTileData**、**MPSh.IconicTileData**、**MPSh.ShellTileData**、**MPSh.StandardTileData** 類別 | [**TileTemplateType**](https://msdn.microsoft.com/library/windows/apps/br208621) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.PhoneApplicationService** 類別 | [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016)、[**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ProgressIndicator** 類別 | [**StatusBarProgressIndicator**](https://msdn.microsoft.com/library/windows/apps/dn633865) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTile** 類別 | [**SecondaryTile**](https://msdn.microsoft.com/library/windows/apps/br242183) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellTileSchedule** 類別 | [**TileUpdater**](https://msdn.microsoft.com/library/windows/apps/br208628) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.ShellToast** 類別 | [**ToastNotificationManager**](https://msdn.microsoft.com/library/windows/apps/br208642) 類別 |
| (MPSh = **Microsoft.Phone.Shell**) <br/> **MPSh.SystemTray** 類別 | [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) 類別 |
| 儲存空間和 I/O | |
| **Microsoft.Phone.Storage.ExternalStorage**、**ExternalStorageDevice**、**ExternalStorageFile**、**ExternalStorageFolder** 類別 | [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 類別 |
| **System.IO** 命名空間 | [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)、[**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/br241791) 命名空間 |
| **System.IO.Directory** 類別 | [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 類別 |
| **System.IO.File** 類別 | [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 和 [**PathIO**](https://msdn.microsoft.com/library/windows/apps/hh701663) 類別
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageFile** 類別 |[**ApplicationData.LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 屬性 |
| (SII = **System.IO.IsolatedStorage**) <br/> **SII.IsolatedStorageSettings** 類別 | [**ApplicationData.LocalSettings**](https://msdn.microsoft.com/library/windows/apps/windows.storage.applicationdata.localsettings.aspx) 屬性 |
| **System.IO.Stream** 類別 | 仍受支援，但使用 ReadAsync() 與 WriteAsync()，而不是 BeginRead()/EndRead() 與 BeginWrite()/EndWrite()。 |
| 電子錢包 | |
| **Microsoft.Phone.Wallet** 命名空間 | [**Windows.ApplicationModel.Wallet**](https://msdn.microsoft.com/library/windows/apps/dn631399) 命名空間 |
| Xml | |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTime** 方法 |
| (SX = **System.Xml**) | **SX.XmlConvert.ToDateTimeOffset** 方法 |


下一個主題是[移植專案](wpsl-to-uwp-porting-to-a-uwp-project.md)。

