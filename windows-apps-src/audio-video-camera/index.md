---
ms.assetid: 0fc12d26-f1cf-4da7-b5a7-735a5074b74a
description: 本節提供有關建立通用 Windows app 以擷取、播放或編輯相片、視訊或音訊的資訊。
title: 音訊、視訊和相機
---

# 音訊、視訊和相機

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本節提供有關建立通用 Windows app 以擷取、播放或編輯相片、視訊或音訊的資訊。
 
| 主題                                                                                             | 說明                                                                                                                                                                                                                                                                                    |
|---------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [使用 CameraCaptureUI 擷取相片和視訊](capture-photos-and-video-with-cameracaptureui.md) | 本文描述如何使用 [CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md) 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。                                                                                                            |
| [使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)       | 本文描述使用 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br241124) API 擷取相片和視訊的步驟，包括初始化和關閉 MediaCapture 以及處理裝置方向的變更。                                  |
| [偵測影像或視訊中的臉部](detect-and-track-faces-in-an-image.md)                         | 本主題示範例如何使用已進行最佳化的 [FaceTracker](https://msdn.microsoft.com/library/windows/apps/dn974150)，在一連串視訊框架中用來追蹤隨著時間改變的臉部。                                                                                                               |
| [媒體組合和編輯](media-compositions-and-editing.md)                               | [Microsoft 媒體基礎](https://msdn.microsoft.com/library/windows/desktop/ms694197) API 中的 API。                                                                                                                                                                                 |
| [影像處理](imaging.md)                                                                             | 本文說明如何使用 [SoftwareBitmap](https://msdn.microsoft.com/library/windows/apps/dn887358) 物件來載入及儲存影像檔，以代表點陣圖影像。                                                                                                                     |
| [轉碼媒體檔案](transcode-media-files.md)                                                 | 您可以使用 [Windows.Media.Transcoding](https://msdn.microsoft.com/library/windows/apps/br207105) API，將視訊檔案從一種格式轉碼成另一種格式。                                                                                                                                |
| [在背景處理媒體檔案](process-media-files-in-the-background.md)                 | 本文示範如何使用 [MediaProcessingTrigger](https://msdn.microsoft.com/library/windows/apps/dn806005) 和背景工作，在背景處理媒體檔案。                                                                                                       |
| [使用 MediaSource 進行媒體播放](media-playback-with-mediasource.md)                             | [MediaSource](https://msdn.microsoft.com/library/windows/apps/dn930905) 類別提供一個常見的方法來參考和播放來自不同來源 (例如本機或遠端檔案) 的媒體，並且公開一個常見的模型來存取媒體資料 (不論使用什麼基礎媒體格式)。  |
| [彈性資料流](adaptive-streaming.md)                                                       | 本文章說明如何將彈性資料流多媒體內容播放新增到通用 Windows 平台 (UWP) app。 本功能目前支援 HTTP 即時資料流 (HLS) 與 HTTP 動態資料流 (DASH) 內容播放。                                          |
| [背景音訊](background-audio.md)                                                           | 本文描述如何建立可在背景播放音訊的 UWP app。                                                                                                                                                                                                               |
| [系統媒體傳輸控制項](system-media-transport-controls.md)                             | [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677) 類別可讓您的 app 使用內建於 Windows 的系統媒體傳輸控制項，以及更新控制項顯示您的 app 目前正在播放的媒體相關的中繼資料。 |
| [媒體投射](media-casting.md)                                                                 | 本文示範如何從通用 Windows app 將媒體傳播到遠端裝置。                                                                                                                                                                                                       |
| [音訊圖](audio-graphs.md)                                                                   | 本文示範如何使用 [Windows.Media.Audio](https://msdn.microsoft.com/library/windows/apps/dn914341) 命名空間中的 API 來建立音訊路由傳送、混音及處理案例的音訊圖。                                                                            |
| [MIDI](midi.md)                                                                                   | 本文示範如何列舉 MIDI (樂器數位介面) 裝置，並且從通用 Windows app 傳送及接收 MIDI 訊息。                                                                                                                                   |
| [相機獨立閃光燈](camera-independent-flashlight.md)                                 | 本文說明如何存取和使用裝置的燈光 (如果有的話)。 燈光功能分別從裝置的相機和閃燈功能進行管理。                                                                                                                 |
| [支援的轉碼器](supported-codecs.md)                                                           | 本文列出 UWP app 支援的音訊與視訊轉碼器和格式。                                                                                                                                                                                                                  |
| [PlayReady DRM](playready-client-sdk.md)                                                          | 本主題描述如何將 PlayReady 保護的媒體內容新增到您的通用 Windows 平台 (UWP) app。                                                                                                                                                                                |
| [PlayReady 加密媒體延伸](playready-encrypted-media-extension.md)                     | 本節描述如何修改 PlayReady Web app，以支援從舊版 Windows 8.1 到 Windows 10 版本所做的變更。                                                                                                                                       |

 

 

 






<!--HONumber=Mar16_HO1-->


