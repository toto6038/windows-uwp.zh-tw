---
author: drewbatgit
ms.assetid: 25553c4d-fa4f-4130-af9b-97f993fefd43
description: 本節提供建立播放音訊和視訊之通用 Windows 應用程式的詳細資訊。
title: 媒體播放
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 888131d37a86793d67ce71376b158bd7de734331
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/19/2017
ms.locfileid: "837021"
---
# <a name="media-playback"></a>媒體播放

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節提供建立播放音訊和視訊之通用 Windows 應用程式的詳細資訊。 

## <a name="media-playback-developer-features"></a>媒體播放開發人員功能

下表列出操作說明文章，可提供將媒體播放功能新增至您 App 的詳細指導方針。
 
| 主題                                                                                             | 說明                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用 MediaPlayer 播放音訊和視訊](play-audio-and-video-with-mediaplayer.md) | 本文說明如何充分利用新功能，以及適用於 UWP 應用程式的媒體播放系統的改進功能。 從 Windows 10 版本 1607 開始，播放媒體的建議最佳做法是使用 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 類別而不是 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaElement) 進行媒體播放。 已經引入精簡的 XAML 控制項 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement)，讓您可以在 XAML 頁面中轉譯媒體內容。 **MediaPlayer** 提供幾項優點，包括與系統媒體傳輸控制項的自動整合，以及更簡單、適用於背景音訊的單一程序模型。 本文也說明如何將視訊轉譯至 Windows.UI.Composition 表面，以及如何使用 [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController) 同步處理多個媒體播放程式。                                                                                                          |
| [媒體項目、播放清單與曲目](media-playback-with-mediasource.md)                         | 本文示範如何使用 [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) 類別提供一個常見的方法來參考和播放來自不同來源 (例如本機或遠端檔案) 的媒體，並且公開一個常見的模型來存取媒體資料 (不論使用什麼基礎媒體格式)。 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) 類別延伸了 **MediaSource** 的功能，可讓您針對包含在媒體項目中的多個音訊、視訊及中繼資料播放軌進行管理和選取。 [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) 可讓您從一或多個媒體播放項目建立播放清單。                                                                                                               |
| [與系統媒體傳輸控制項整合](integrate-with-systemmediatransportcontrols.md)                               | 本文說明如何將您的 App 與系統媒體傳輸控制項 (SMTC) 整合。 從 Windows 10 版本 1607 開始，每個您建立來播放媒體的 **MediaPlayer** 執行個體，會由 SMTC 自動顯示。 本文說明如何提供 SMTC 與有關您正在播放之內容的中繼資料，以及如何增強或完全覆寫 SMTC 控制項的預設行為。                                   |
| [系統支援的定時中繼資料提示](system-supported-metadata-cues.md)                               | 本文描述如何充分利用可嵌入到媒體檔案或資料流的數種定時中繼資料格式。                                   |
| [建立、排程與管理媒體中斷](create-schedule-and-manage-media-breaks.md)                                                                             | 本文示範如何建立、排程及管理媒體播放 app 的媒體中斷。 從 Windows 10 版本 1607 開始，您可以使用 [**MediaBreakManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaBreakManager) 類別，快速且輕鬆地將媒體中斷新增至任何您用來播放 [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) 的 [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem)。 媒體中斷通常是用來將音訊或視訊廣告插入媒體內容。 當您排程一或多個媒體中斷之後，系統就會自動在播放期間的指定時間播放您的媒體內容。 **MediaBreakManager** 提供事件，讓您的 app 可以在媒體中斷開始、結束，或當使用者略過它們時加以回應。 您也可以針對媒體中斷存取 [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession)，以監視下載與緩衝處理進度更新等事件。                                                                                                                     |
| [在背景播放媒體](background-audio.md)                                                                             | 本文說明如何設定您的 app，當 app 從前景移至背景時，該媒體仍能繼續播放。 這表示即使使用者已最小化您的 App、回到主畫面，或以其他方式從您的 App 離開之後，您的 App 仍可繼續播放音訊。 Windows 10 版本 1607 引進了一個適用於背景媒體播放的新的單一程序模型，這個模型能比舊版雙程序模型更快速簡單地實作。 本文包含有關處理新的應用程式週期事件 [**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 以及 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 的詳細資訊，以在背景執行時管理您的應用程式記憶體使用量。                                                                                                                    |
| [彈性資料流](adaptive-streaming.md)                                                       | 本文章說明如何將彈性資料流多媒體內容播放新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時資料流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。                                          |
| [媒體傳播](media-casting.md)                                                                 | 本文示範如何從通用 Windows app 將媒體傳播到遠端裝置。                                                                                                                                                                                                       |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 本主題說明如何將 PlayReady 保護的媒體內容新增到您的通用 Windows 平台 (UWP) app。                                                                                                                                                                                |
| [PlayReady 加密媒體延伸](playready-encrypted-media-extension.md)                     | 本節說明如何修改 PlayReady Web app，以支援從舊版 Windows 8.1 到 Windows 10 版本所做的變更。                                                                                                                                       |

## <a name="media-playback-sdk-samples"></a>媒體播放 SDK 範例

下列的 SDK 範例示範 Windows 10 上 UWP 應用程式可用的媒體播放功能。 使用這些範例查看內容中使用的媒體播放 API 或者做為您應用程式的起點。

* [彈性資料流範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming)
* [背景音訊範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundMediaPlayback)
* [系統媒體傳輸範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/SystemMediaTransportControls)                                                                                               
 




