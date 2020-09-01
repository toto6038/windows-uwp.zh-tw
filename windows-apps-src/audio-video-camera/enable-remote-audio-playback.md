---
description: 本文說明如何使用 AudioPlaybackConnection 讓連線到 Bluetooth 的遠端裝置在本機電腦上播放音訊。
title: 啟用來自遠端藍牙連線裝置的音訊播放
ms.date: 05/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8a23758612b3c595f808fe2ffe4f38e558cf0740
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163962"
---
# <a name="enable-audio-playback-from-remote-bluetooth-connected-devices"></a>啟用來自遠端藍牙連線裝置的音訊播放

本文說明如何使用 [AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection) 讓連線到 Bluetooth 的遠端裝置在本機電腦上播放音訊。

從 Windows 10 開始，2004版遠端音訊來源可以將音訊串流處理至 Windows 裝置，以啟用像是將電腦設定為與 Bluetooth 喇叭一樣的行為，並允許使用者聽到電話的音訊。 此執行程式會使用作業系統中的藍牙元件來處理傳入的音訊資料，並在系統的音訊端點上播放，例如內建的電腦喇叭或有線耳機。 基礎藍牙 A2DP 接收的啟用是由應用程式管理，而這些應用程式負責使用者案例，而不是系統。

[AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection)類別可用來啟用和停用遠端裝置的連線，以及建立連線，讓遠端音訊播放開始。

## <a name="add-a-user-interface"></a>新增使用者介面

針對本文中的範例，我們將使用下列簡單的 XAML UI 來定義 **ListView** 控制項，以顯示可用的遠端裝置、顯示連接狀態的 **TextBlock** ，以及用來啟用、停用和開啟連接的三個按鈕。

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml" id="snippet_AudioPlaybackConnectionXAML":::

## <a name="use-devicewatcher-to-monitor-for-remote-devices"></a>使用 DeviceWatcher 監視遠端裝置

[DeviceWatcher](/uwp/api/windows.devices.enumeration.devicewatcher)類別可讓您偵測已連線的裝置。 [AudioPlaybackConnection. GetDeviceSelector](/uwp/api/windows.media.audio.audioplaybackconnection.getdeviceselector)方法會傳回字串，告知裝置監看員要監看哪些類型的裝置。 將此字串傳遞至 **DeviceWatcher** 函式。 

當裝置監看員啟動時所連線的每個裝置，以及當裝置監看員正在執行時連線的任何裝置，都會引發[DeviceWatcher。](/uwp/api/windows.devices.enumeration.devicewatcher.added) 如果先前連接的裝置中斷連線，就會引發[DeviceWatcher。](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 

呼叫 [DeviceWatcher。開始](/uwp/api/windows.devices.enumeration.devicewatcher.start) 觀察支援音訊播放連接的連線裝置。 在此範例中，我們會在載入 UI 中的主要 **方格** 控制項時，啟動 [裝置管理員]。 如需使用 **DeviceWatcher**的詳細資訊，請參閱 [列舉裝置](../devices-sensors/enumerate-devices.md)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_MainGridLoaded":::


在裝置監看員 **新增** 的事件中，每個探索到的裝置都會以 [DeviceInformation](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 物件表示。 將每個探索到的裝置新增至可觀察的集合，該集合系結至 UI 中的 **ListView** 控制項。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareDevices":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Added":::


## <a name="enable-and-release-audio-playback-connections"></a>啟用和發行音訊播放連接

使用裝置開啟連接之前，必須先啟用連線。 這會通知系統有一個新的應用程式想要在電腦上播放來自遠端裝置的音訊，但是在連線開啟之前，音訊不會開始播放，這會在稍後的步驟中顯示。

在 [ **啟用音訊播放** 連線] 按鈕的 click 處理常式中，取得與 **ListView** 控制項中目前所選裝置相關聯的裝置識別碼。 這個範例會維護已啟用之 **AudioPlaybackConnection** 物件的字典。 這個方法會先檢查所選裝置的字典中是否已經有專案。 接下來，此方法會藉由呼叫[TryCreateFromId](/uwp/api/windows.media.audio.audioplaybackconnection.trycreatefromid)並傳入選取的裝置識別碼，嘗試建立所選裝置的**AudioPlaybackConnection** 。 

如果成功建立連接，請將新的 **AudioPlaybackConnection** 物件加入至應用程式的字典、註冊物件 [StateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) 事件的處理常式，然後呼叫[StartAsync](/uwp/api/windows.media.audio.audioplaybackconnection.startasync) 來通知系統已啟用新的連接。 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeclareConnections":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_EnableAudioPlaybackConnection":::


## <a name="open-the-audio-playback-connection"></a>開啟音訊播放連接

在上一個步驟中，已建立音訊播放連接，但在透過呼叫 [Open](/uwp/api/windows.media.audio.audioplaybackconnection.open) 或 [OpenAsync](/uwp/api/windows.media.audio.audioplaybackconnection.openasync)開啟連接之前，音效不會開始播放。 在 [ **開啟音訊播放** 連線] 按鈕中，按一下 [處理常式]，取得目前選取的裝置，並使用識別碼從應用程式的連接字典中取出 **AudioPlaybackConnection** 。 等候**OpenAsync**的呼叫，並檢查傳回的[AudioPlaybackConnectionOpenResultStatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult)物件的**狀態值**，以查看連接是否成功開啟，如果是，則更新連接狀態文字方塊。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_OpenAudioPlaybackConnectionButton":::

## <a name="monitor-audio-playback-connection-state"></a>監視音訊播放連接狀態

每當連接的狀態變更時，就會引發 [AudioPlaybackConnection. ConnectionStateChanged](/uwp/api/windows.media.audio.audioplaybackconnection.statechanged) 事件。 在此範例中，此事件的處理常式會更新 [狀態] 文字方塊。 請記得更新 [RunAsync](/uwp/api/windows.ui.core.coredispatcher.runasync) 呼叫內的 ui，以確保在 ui 執行緒上進行更新。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ConnectionStateChanged":::

## <a name="release-connections-and-handle-removed-devices"></a>發行連接並處理已移除的裝置

這個範例會提供 **放開音訊播放** 連線按鈕，讓使用者可以放開音訊播放連線。 在此事件的處理常式中，我們會取得目前選取的裝置，並使用裝置的識別碼在字典中查閱 **AudioPlaybackConnection** 。 呼叫 **Dispose** 以釋放參考，並釋放任何相關聯的資源，並從字典中移除連接。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_ReleaseAudioPlaybackConnectionButton":::

您應該處理啟用或開啟連線時移除裝置的案例。 若要這樣做，請針對裝置監看員的 DeviceWatcher 執行處理常式 [。已移除](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 事件。 首先，已移除裝置的識別碼會用來從系結至應用程式 **ListView** 控制項的可觀察集合中移除裝置。 接下來，如果與此裝置相關聯的連線是在應用程式的字典中，則會呼叫 **Dispose** 來釋出相關聯的資源，然後從字典中移除連接。 這一切都是在 **RunAsync** 呼叫中完成，以確保 ui 執行緒上的 ui 更新執行。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioPlaybackConnectionExample/cs/MainPage.xaml.cs" id="snippet_DeviceWatcher_Removed":::

## <a name="related-topics"></a>相關主題

[媒體播放](media-playback.md)


 