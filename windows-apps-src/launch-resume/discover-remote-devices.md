---
author: PatrickFarley
title: 探索遠端裝置
description: 了解如何使用專案 Rome 從應用程式探索遠端裝置。
ms.assetid: 5b4231c0-5060-49e2-a577-b747e20cf633
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，連接裝置、 遠端系統、 rome 的 project rome
ms.localizationpriority: medium
ms.openlocfilehash: 02d04074ece0033da8c3454a95bc35af201903f3
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "5473876"
---
# <a name="discover-remote-devices"></a>探索遠端裝置
您的 App 可以使用和探索裝置相同的 Microsoft 帳戶登入，使用無線網路、藍牙及雲端連線來探索 Windows 裝置。 遠端裝置不需要安裝任何特殊的軟體即可以搜尋。

> [!NOTE]
> 本指南假設您已經依照[啟動遠端 app](launch-a-remote-app.md) 中的步驟，將存取權授與遠端系統功能。

## <a name="filter-the-set-of-discoverable-devices"></a>篩選一組可搜尋的裝置
您可以使用 [**RemoteSystemWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher) 搭配篩選器，縮小一組可搜尋裝置的範圍。 篩選器可以偵測探索類型 (近端、本機網路和雲端連線)、裝置類型 (桌上型電腦、行動裝置、Xbox、中樞和全像攝影) 以及可用性狀態 (使用遠端系統功能的裝置可用性狀態)。

因為物件會當成參數傳遞到其建構函式，所以必須在初始化 **RemoteSystemWatcher** 物件期間或之前建構篩選物件。 下列程式碼可建立每種可用類型的篩選器，並新增至清單中。

> [!NOTE]
> 這些範例的程式碼要求您的檔案中必須有 `using Windows.System.RemoteSystems` 陳述式。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetMakeFilterList)]

> [!NOTE]
> 「近端」篩選值無法保證實體鄰近程度。 對於需要可靠實體鄰近性的案例，請在篩選器中使用 [**RemoteSystemDiscoveryType.SpatiallyProximal**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemdiscoverytype) 值。 此篩選器目前只允許藍牙找到的裝置。 由於保證實體鄰近性的新探索機制及通訊協定已在支援之列，因此也包含在其中。  
[**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 類別中還有表示找到的裝置實際上是否在實體鄰近範圍內的屬性：[**RemoteSystem.IsAvailableBySpatialProximity**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem.IsAvailableByProximity)。

> [!NOTE]
> 如果您想要探索 (由您探索的類型篩選條件選擇) 區域網路上的裝置，您的網路需要使用「私人」或「網域」設定檔。 您的裝置不會透過「公用」網路探索其他裝置。

建立 [**IRemoteSystemFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.IRemoteSystemFilter) 物件的清單之後，即可傳遞至 **RemoteSystemWatcher** 的建構函式。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetCreateWatcher)]

呼叫這個監控程式的 [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.Start) 方法時，只有偵測到符合下列所有條件的裝置時，才會引發 [**RemoteSystemAdded**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemWatcher.RemoteSystemAdded) 事件︰
* 可透過近端連線搜尋
* 其是桌面或電話
* 其歸類為可用

從該處開始，處理事件、擷取 [**RemoteSystem**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) 物件，以及連線到遠端裝置的程序，和[啟動遠端 app](launch-a-remote-app.md) 完全相同。 簡單說，**RemoteSystem** 物件會儲存為 [**RemoteSystemAddedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemAddedEventArgs) 物件 (透過每個 **RemoteSystemAdded** 事件傳入) 的屬性。

## <a name="discover-devices-by-address-input"></a>依據位址輸入探索裝置
有些裝置可能無法透過掃描來與使用者關聯或搜尋到，但如果探索的 app 直接使用位址，仍然可以到達。  [**HostName**](https://msdn.microsoft.com/library/windows/apps/windows.networking.hostname.aspx) 類別可用來表示遠端裝置的位址。 這通常會以 IP 位址的形式儲存，但也允許數個其他格式 (如需詳細資訊，請參閱 [**HostName 建構函式**](https://msdn.microsoft.com/library/windows/apps/br207118.aspx))。

如果提供有效的 **HostName** 物件，就可以擷取**RemoteSystem** 物件。 如果位址資料無效，則會傳回 `null` 物件參照。

[!code-cs[Main](./code/DiscoverDevices/MainPage.xaml.cs#SnippetFindByHostName)]

## <a name="querying-a-capability-on-a-remote-system"></a>查詢遠端系統上的功能

雖然有別於探索篩選，查詢裝置功能可能仍舊是探索程序的重要部分。 您可以使用 [**RemoteSystem.GetCapabilitySupportedAsync**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystem.GetCapabilitySupportedAsync) 方法查詢找到的遠端裝置來支援特定功能，例如遠端工作階段連線能力或空間實體 (全息影像) 共用。 如需可查詢功能的清單，請參閱 [**KnownRemoteSystemCapabilities**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.knownremotesystemcapabilities) 類別。

```csharp
// Check to see if the given remote system can accept LaunchUri requests
bool isRemoteSystemLaunchUriCapable = remoteSystem.GetCapabilitySupportedAsync(KnownRemoteSystemCapabilities.LaunchUri);
```

## <a name="cross-user-discovery"></a>跨使用者探索

開發人員可以指定探索用鄰近戶端裝置的_所有_裝置，而不只是註冊到相同使用者的裝置。 這會透過特殊的 **IRemoteSystemFilter**、[**RemoteSystemAuthorizationKindFilter**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkindfilter) 進行實作。 就像其他篩選類型一樣進行實作︰

```csharp
// Construct a user type filter that includes anonymous devices
RemoteSystemAuthorizationKindFilter authorizationKindFilter = new RemoteSystemAuthorizationKindFilter(RemoteSystemAuthorizationKind.Anonymous);
// then add this filter to the RemoteSystemWatcher
```

* **Anonymous** 的 [**RemoteSystemAuthorizationKind**](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemauthorizationkind) 值允許探索所有的近端裝置，即使是不受信任使用者的那些裝置也可以。
* **SameUser** 值會篩選探索，僅保留與用戶端裝置一樣註冊到相同使用者的裝置。 這是是預設行為。

### <a name="checking-the-cross-user-sharing-settings"></a>檢查跨使用者共用設定

除了上述指定在探索 App 中指定的篩選器之外，用戶端裝置本身也必須設定為允許從登入的裝置來與其他使用者共用體驗。 這是可以使用 **RemoteSystem** 類別的靜態方法查詢的系統設定：

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Anonymous".
}
```

若要變更此設定，使用者必須開啟 **\[設定\]** App。 在 **\[系統\]** > **\[共用體驗\]** > **\[在裝置間共用\]** 功能表中有下拉式方塊，使用者可以在那裡指定其系統可以與哪些裝置共用。

![共用體驗設定頁面](images/shared-experiences-settings.png)

## <a name="related-topics"></a>相關主題
* [已連線的應用程式與裝置 (專案 Rome)](connected-apps-and-devices.md)
* [啟動遠端 App](launch-a-remote-app.md)
* [遠端系統 API 參考](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)
* [遠端系統範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
