---
author: drewbatgit
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: 本文說明如何使用 MediaFrameReader 搭配 MediaCapture 以取得包含來自擷取來源之音訊資料的 AudioFrames。
title: 使用 MediaFrameReader 處理音訊框架
ms.author: drewbat
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d69e8d8cca3932045d4b43d727210f84e816f30b
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/19/2018
ms.locfileid: "7295248"
---
# <a name="process-audio-frames-with-mediaframereader"></a>使用 MediaFrameReader 處理音訊框架

本文說明如何使用 [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) 搭配 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture)，從媒體畫面來源取得音訊資料。 若要了解如何使用 **MediaFrameReader** 從彩色、紅外線或景深相機 (舉例來說) 取得影像資料，請參閱[使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)。 該篇文章提供畫面讀取程式使用模式的一般概觀，並討論 **MediaFrameReader** 類別的一些額外功能，例如使用 **MediaFrameSourceGroup** 同時從多個來源擷取畫面。 

> [!NOTE] 
> 本文中所討論的功能只從 Windows 10 版本 1803 開始提供。

> [!NOTE] 
> 還有一個通用 Windows app 範例，示範使用 **MediaFrameReader** 顯示來自不同畫面來源 (包括色彩、深度與紅外線相機) 的畫面。 如需詳細資訊，請參閱[相機畫面範例](http://go.microsoft.com/fwlink/?LinkId=823230)。

## <a name="setting-up-your-project"></a>設定您的專案
擷取音訊框架的程序與取得其他媒體框架類型的程序大致相同。 就像任何使用 **MediaCapture** 的 App 一樣，您必須在嘗試存取任何相機裝置之前，宣告您的 App 是使用*網路攝影機*功能。 如果您的應用程式會從音訊裝置擷取，您也應該宣告*麥克風*裝置功能。 

**將功能新增到應用程式資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 **\[方案總管\]** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2.  選取 **\[功能\]** 索引標籤。
3.  核取 **\[網路攝影機\]** 方塊和 **\[麥克風\]** 方塊。
4.  如果要存取圖片媒體櫃和視訊媒體櫃，請選取 **\[圖片媒體櫃\]** 方塊和 **\[視訊媒體櫃\]** 方塊。



## <a name="select-frame-sources-and-frame-source-groups"></a>選取畫面來源和畫面來源群組

擷取音訊框架的第一個步驟是初始化代表音訊資料來源的 [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource)，例如麥克風或其他音訊擷取裝置。 若要這樣做，您必須建立 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 物件的新執行個體。 針對此範例，**MediaCapture** 的唯一初始化設定是將 [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) 設定為指出我們要從擷取裝置串流處理音訊。 

在呼叫 [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync) 後，您可以取得可存取媒體框架來源清單並包含 [**FrameSources**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources) 屬性。 此範例使用 Linq 查詢來選取所有框架來源，其中 [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo) 描述框架來源具有 **Audio** 的 [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype)，指出媒體來源產生音訊資料。

如果查詢傳回一或多個框架來源，您可以檢查 [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) 屬性以查看來源是否支援您想要的音訊格式，在此範例中，是浮動音訊資料。 查看 [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) 以確定來源支援您想要的音訊編碼。

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>建立並開始 MediaFrameReader

透過呼叫 [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_)、傳遞您在上一個步驟中選取的 **MediaFrameSource** 物件，取得 **MediaFrameReader** 的新執行個體。 根據預設，音訊框架會在緩衝模式中取得，如此比較不可能捨棄框架，但是如果您處理音訊框架的速度不夠快因而填滿系統分配的記憶體緩衝區，這種情形仍然會發生。

註冊 [**MediaFrameReader.FrameArrived**](*https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) 事件的處理常式，只要有可用的新音訊資料框架時就會引發該事件。 呼叫 [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) 來開始擷取音訊框架。 如果畫面讀取程式無法開始，呼叫傳回的狀態值會具有 [**Success**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus) 以外的值。

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

在 **FrameArrived** 事件處理常式中，在 **MediaFrameReader** 物件上呼叫以傳送者傳遞給處理常式的 [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe)，以嘗試擷取最新媒體畫面的參考。 請注意，此物件可能是 null，因此您在使用物件前應該一律先檢查。 從 **TryAcquireLatestFrame** 傳回包裝在 **MediaFrameReference** 中的媒體畫面類型，取決於您設定畫面讀取程式去取得何種類型的框架來源或來源。 由於此範例中的畫面讀取程式設定為取得音訊框架，因此它會使用 [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) 屬性取得基礎框架。 

以下範例中的此 **ProcessAudioFrame** 協助程式方法示範如何取得 [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe)，其中提供框架時間戳記以及是否從 **AudioMediaFrame** 物件中斷等資訊。 若要讀取或處理音訊範例資料，您需要從 **AudioMediaFrame** 物件取得 [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer) 物件、建立 [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)，然後呼叫 COM 方法 **IMemoryBufferByteAccess::GetBuffer** 來擷取資料。 請參閱程式碼清單下面的註釋，以了解存取原生緩衝區的詳細資訊。

資料的格式取決於框架來源。 在此範例中，選取媒體框架來源時，我們明確地確定所選取的框架來源使用浮動資料的單一頻道。 範例程式碼的其餘部分示範如何判斷框架中音訊資料的持續時間和取樣計數。  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> 為了對音訊資料進行操作，您必須存取原生記憶體緩衝。 若要這麼做，您必須包含下列程式碼清單，來使用 **IMemoryBufferByteAccess** COM 介面。 對原生緩衝區的作業必須在 **unsafe** 關鍵字的方法中執行。 您也需要在 **\[專案\] -> \[屬性\]** 對話方塊的 **\[組建\]** 索引標籤中勾選核取方塊，以允許不安全的程式碼。

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>使用 MediaFrameReader 搭配音訊資料的其他資訊

您可以存取 [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller) 屬性來擷取與音訊框架來源相關聯的 [**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController)。 此物件可用來取得或設定擷取裝置的串流屬性或控制擷取層級。 下列範例將音訊裝置設為靜音，以便畫面讀取程式可以持續取得框架，但所有樣本都具有 0 的值。

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

您可以使用 [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) 物件來將媒體畫面來源擷取的音訊資料傳遞至 [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph)。 將框架傳遞至 [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode) 的 [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) 方法。 如需使用音訊圖來擷取、處理和混合音訊訊號的詳細資訊，請參閱[音訊圖](audio-graphs.md)。

## <a name="related-topics"></a>相關主題

* [使用 MediaFrameReader 處理媒體畫面](process-media-frames-with-mediaframereader.md)
* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相機畫面範例](http://go.microsoft.com/fwlink/?LinkId=823230)
* [音訊圖](audio-graphs.md)
 






