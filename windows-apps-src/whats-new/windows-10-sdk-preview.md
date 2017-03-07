---
author: QuinnRadich
title: "Windows 10 版本 1607 預覽中的新功能"
description: "Windows 10 版本 1607 預覽與新的開發人員工具提供由新的通用 Windows 平台所提供的工具、功能及體驗。"
keywords: "新功能, 新功能, 更新, 多項更新, 功能, 新, Windows 10, 1607 預覽"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 835d5393-427f-4155-a737-d509ea1de99f
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 51076aed4ae55956164efaf3a3c01461e4b7338b
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-in-windows"></a>Windows 的新功能

Windows 10 版本 1607 預覽與針對 Windows 開發人員工具的更新持續提供由通用 Windows 平台所提供的工具、功能及體驗。 在 Windows 10 上[安裝工具和 SDK](http://go.microsoft.com/fwlink/?LinkId=821431) 之後，就表示您已經準備好[建立新的通用 Windows app](https://msdn.microsoft.com/library/windows/apps/bg124288)，或是探索[如何在 Windows 上使用現有的 App 程式碼](https://msdn.microsoft.com/library/windows/apps/mt238321)。

## <a name="windows-10-version-1607-preview"></a>Windows 10 版本 1607 預覽

功能 | 說明
 :---- | :----
網路功能 | 您現在可以訂閱 [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) 事件，便能提供伺服器 SSL/TLS 憑證的自訂驗證。 您也可以在 HTTP 要求中指定 [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) 列舉值，便能完全停用從快取中讀取 HTTP 回應。 現在藉由呼叫 [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank) 方法，便可以清除驗證認證，以促成「登出」狀況。
擴充功能 | Microsoft Edge 的新功能之一，便是能使用擴充功能。 使用者可藉由擴充功能來擴充 Microsoft Edge 的功能，以便針對特定對象，提供重要的特殊功能。 若要了解詳細資訊，請參閱[擴充功能文件](https://developer.microsoft.com/microsoft-edge/platform/documentation/extensions/#_blank)。
藍牙 API | App 現在可經由 [Windows.Devices.Bluetooth and Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank) 來存取遠端藍牙周邊裝置上的 RFCOMM 服務，而不必先與周邊裝置配對。 新方法可讓 app 搜尋與存取未配對裝置上的 RFCOMM 服務。
聊天 API | 您可以藉由新的 [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank) 類別，使得簡訊與雲端同步。
[適用於 Android 與 iOS 開發人員的 Windows 應用程式概念對應](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | 如果您是具備 Android 或 iOS 技巧和 (或) 程式碼的開發人員，而且您想要移到 Windows 10 和通用 Windows 平台 (UWP)，則此資源擁有您在三個平台之間對應平台功能 (和您的知識) 所需的資訊。
[企業資料保護 (EDP)](https://msdn.microsoft.com/windows/uwp/enterprise/wip-hub) | EDP 是桌上型電腦、膝上型電腦、平板電腦與手機上的一組行動裝置管理 (MDM) 功能。 EDP 讓企業在其管理的裝置上，對於資料 (企業檔案與資料 blob) 處理方式有更大的控制權。
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | 新的 AppExtensions 命名空間可讓您的 Windows 市集應用程式裝載其他 Windows 市集應用程式提供的內容。 您可以尋找、列舉與存取來自那些 app 的唯讀內容。
Windows IoT | Windows 10 IoT 核心版可讓您以 Windows 的熟悉感來建立 IoT 應用程式，而且目前可用於 Raspberry Pi 3，這是最新的 Raspberry Pi 機板。
媒體 API | Windows.Media.Playback 命名空間中新的 MediaBreak API 可讓您在使用 MediaSource 與 MediaPlaybackItem 播放媒體時，輕鬆排定與管理媒體中斷。 Windows.Media.Audio 命名空間中新的 AudioGraph API 新增了空間音訊處理，讓您為音訊圖節點指派 3D 定位的發射與接收。
地圖 API | [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank) 已經過改善，讓開發人員在非常傾斜的視角，可以取得接近攝影機的可見區域，而排除遠處與接近地平線的區域。 [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank) 類別已擴充，可以讓開發人員指定正確度，以便在反向地理編碼時最佳化網路流量。 開發人員現在可以使用 [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) 方法與指定經緯度，以善用下載離線地圖。 如需詳細資訊，請參閱[啟動 Windows 地圖 app](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank)。

