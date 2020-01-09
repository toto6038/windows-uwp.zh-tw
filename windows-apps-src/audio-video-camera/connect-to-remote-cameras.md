---
ms.assetid: ''
description: 本文將說明如何連線到遠端相機，並取得 MediaFrameSourceGroup 以從每個相機抓取畫面。
title: 連線到遠端照相機
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c7b876cff994f775b770d22c103d27271047b269
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683631"
---
# <a name="connect-to-remote-cameras"></a>連線到遠端照相機

本文說明如何連接到一或多個遠端相機，並取得可讓您從每張攝影機讀取畫面的[**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)物件。 如需從媒體來源讀取畫面格的詳細資訊，請參閱[使用 MediaFrameReader 處理媒體框架](process-media-frames-with-mediaframereader.md)。 如需配對裝置的詳細資訊，請參閱[配對裝置](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

> [!NOTE] 
> 本文所討論的功能只有從 Windows 10 版本1903開始提供。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>建立 DeviceWatcher 類別以監看是否有可用的遠端相機

[**DeviceWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher)類別會監視您的應用程式可用的裝置，並在新增或移除裝置時通知您的應用程式。 藉由呼叫[**DeviceInformation**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)來取得**DeviceWatcher**的實例，傳入可識別您想要監視之裝置類型的先進查詢語法（AQS）字串。 指定網路攝影機裝置的 AQS 字串如下所示：

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> 協助程式方法[**MediaFrameSourceGroup。 GetDeviceSelector**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector)會傳回 AQS 字串，其會監視本機連線和遠端網路攝影機。 若只要監視網路攝影機，您應該使用如上所示的 AQS 字串。


當您藉由呼叫[**start**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start)方法來啟動傳回的**DeviceWatcher**時，它會針對目前可用的每個網路攝影機引發[**新增**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)的事件。 在您藉由呼叫[**stop**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)停止監看員之前，會在新的網路攝影機裝置可用時**引發新增的事件，** 而且當相機裝置無法使用時，將會引發[**已移除**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.removed)的事件。

傳遞至**已加入**和**已移除**事件處理常式的事件引數分別是[**DeviceInformation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)或[**DeviceInformationUpdate**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformationupdate)物件。 這些物件的每一個都有一個**Id**屬性，這是引發事件的網路攝影機識別碼。 將此識別碼傳遞至[**MediaFrameSourceGroup. FromIdAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)方法，以取得可供您用來從相機抓取框架的[**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)物件。

## <a name="remote-camera-pairing-helper-class"></a>遠端相機配對 helper 類別

下列範例顯示的 helper 類別，會使用**DeviceWatcher**來建立及更新**MediaFrameSourceGroup**物件的**ObservableCollection** ，以支援將資料系結至相機清單。 一般應用程式會將**MediaFrameSourceGroup**包裝在自訂模型類別中。 請注意，helper 類別會維護應用程式[**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)的參考，並更新對[**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)的呼叫內的相機集合，以確保系結至集合的 ui 會在 ui 執行緒上更新。

此外，這個範例也會處理**已新增**和**移除**的事件以外的[**DeviceWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)事件。 在**更新**的處理常式中，關聯的遠端相機裝置會從移除，然後再加回集合。

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [具有 MediaCapture 的基本相片、影片和音訊捕獲](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相機框架範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [使用 MediaFrameReader 處理媒體框架](process-media-frames-with-mediaframereader.md)
 

 




