---
ms.assetid: ''
description: 這篇文章會示範如何連接到遠端的相機，並取得 MediaFrameSourceGroup 從每部相機擷取框架。
title: 連接到遠端的相機
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bc719b8dad2adef0542edf284d257846052eac21
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/24/2019
ms.locfileid: "63789590"
---
# <a name="connect-to-remote-cameras"></a>連接到遠端的相機

這篇文章會示範如何連接到遠端的一或多個數位相機，並取得[ **MediaFrameSourceGroup** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup)可讓您讀取從每個相機的畫面格的物件。 如需有關讀取的媒體來源畫面格的詳細資訊，請參閱[處理媒體框架與 MediaFrameReader](process-media-frames-with-mediaframereader.md)。 如需有關與裝置配對的詳細資訊，請參閱[配對的裝置](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices)。

> [!NOTE] 
> 只會提供開頭為 Windows 10 版本 1903，本文所討論的功能。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>建立監看可用的遠端相機 DeviceWatcher 類別

[ **DeviceWatcher** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher)類別監視的裝置可在應用程式，並新增或移除裝置時，通知您的應用程式。 取得的執行個體**DeviceWatcher**藉由呼叫[ **DeviceInformation.CreateWatcher**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)，並傳入進階查詢語法 (AQS) 字串，識別的型別您想要監視的裝置。 指定網路攝影機裝置的 AQS 字串如下：

```
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> 協助程式方法[ **MediaFrameSourceGroup.GetDeviceSelector** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector)傳回 AQS 字串來監視本機連接與遠端網路攝影機。 若要監視只有網路相機，您應該使用如上所示的 AQS 字串。


當您啟動傳回**DeviceWatcher**藉由呼叫[**開始**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.start)方法，它會引發[ **Added** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.added)目前可供每個網路攝影機的事件。 直到您藉由呼叫停止監看員[**停止**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.stop)，則**Added**當新的網路攝影機裝置變成可用時，就會引發事件和[ **已移除**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.devicewatcher.removed)當相機的裝置無法使用時，就會引發事件。

事件引數傳遞至**Added**並**已移除**事件處理常式已[ **DeviceInformation** ](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceInformation)或[ **DeviceInformationUpdate** ](https://docs.microsoft.com/en-us/uwp/api/windows.devices.enumeration.deviceinformationupdate)物件，分別。 每個物件具有**識別碼**是網路攝影機，引發事件的識別項的屬性。 傳遞到此識別碼[ **MediaFrameSourceGroup.FromIdAsync** ](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)方法來取得[ **MediaFrameSourceGroup** ](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync)物件，您可以使用從相機擷取框架。

## <a name="remote-camera-pairing-helper-class"></a>遙控攝影機配對的協助程式類別

下列範例示範使用協助程式類別**DeviceWatcher**來建立和更新**ObservableCollection**的**MediaFrameSourceGroup**物件以支援資料繫結至攝影機的清單。 一般而言，應用程式會包裝**MediaFrameSourceGroup**自訂模型類別中。 附註的協助程式類別會維護應用程式的參考[ **CoreDispatcher** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) ，並更新相機內呼叫的集合[ **RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync)以確保繫結至集合的 UI 更新 UI 執行緒上。

此外，這個範例會處理[ **DeviceWatcher.Updated** ](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicewatcher.updated)除了事件**Added**並**已移除**事件。 在 **更新**移除處理常式，相關聯的遙控攝影機裝置並再加回至集合。

[!code-cs[SnippetRemoteCameraPairingHelper](./code/Frames_Win10/Frames_Win10/RemoteCameraPairingHelper.cs#SnippetRemoteCameraPairingHelper)]


## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [MediaCapture 擷取基本的相片、 視訊和音訊](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相機框架範例](https://go.microsoft.com/fwlink/?LinkId=823230)
* [處理媒體與 MediaFrameReader 的畫面格](process-media-frames-with-mediaframereader.md)
 

 




