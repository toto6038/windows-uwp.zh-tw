---
author: drewbatgit
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: 本文說明 UWP app 如何偵測及回應音訊資料流層級的系統起始變更。
title: 偵測及回應音訊狀態變更
ms.author: drewbat
ms.date: 04/03/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f7b4addf2a7bdc2d93cbcf64f13a640a4ef5b12a
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "6252759"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>偵測及回應音訊狀態變更
從 Windows 10 版本 1803 開始，您的應用程式可偵測系統何時將您應用程式使用之音訊的音量降低或設為靜音。 您可以收到關於擷取和轉譯串流、特定音訊裝置和音訊類別，或您的應用程式用於播放媒體之 [**MediaPlayer**](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) 物件的相關通知。 例如，系統可能會在鬧鈴響起時，降低 (或者「迴避」) 音訊播放音量。 如果您的應用程式未在應用程式資訊清單中宣告 *backgroundMediaPlayback* 功能，當您的應用程式進入背景時，系統會將其設為靜音。 

對於所有受支援音訊串流的音訊狀態變更處理模式都相同。 首先，建立 [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor) 類別的執行個體。 在下列範例中，應用程式使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 類別來擷取遊戲聊天的音訊。 呼叫 factory 方法來取得預設通訊裝置的遊戲聊天音訊擷取串流之相關聯音訊狀態監視器。  接著，註冊 [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) 事件的處理常式，此事件會在系統變更相關串流的音訊時引發。

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

在**SoundLevelChanged**事件處理常式中，檢查[**Audiostatemonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel)屬性**AudioStateMonitor**寄件者傳入處理常式來判斷新音訊資料流層級的功能。 在此範例中，應用程式在音量設為靜音時停止擷取音訊，並在恢復最大音量時繼續擷取。

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

如需使用 **MediaCapture** 擷取音訊的詳細資訊，請參閱[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)。

每個 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 類別的執行個體都有與其關聯的 **AudioStateMonitor**，您可用來偵測系統何時變更目前播放內容的音量大小。 您可以根據正在播放的內容類型，決定以不同方式處理音訊狀態變更。 例如，您可以決定在音量降低時暫停播放播客，但如果內容是音樂則繼續播放。 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

如需使用 **MediaPlayer** 的詳細資訊，請參閱[使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md)。 

## <a name="related-topics"></a>相關主題