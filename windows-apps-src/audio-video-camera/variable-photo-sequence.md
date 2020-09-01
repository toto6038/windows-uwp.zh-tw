---
ms.assetid: 7DBEE5E2-C3EC-4305-823D-9095C761A1CD
description: 本文章示範如何擷取可變相片序列，讓您以快速連續的方式拍攝多個影像畫面，並針對每個畫面使用不同焦點、閃光燈、ISO、曝光度及曝光補償設定進行設定。
title: 可變相片序列
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a453aba0c8992e0df348e54620add5d65fce4403
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175632"
---
# <a name="variable-photo-sequence"></a>可變相片序列



本文章示範如何擷取可變相片序列，讓您以快速連續的方式拍攝多個影像畫面，並針對每個畫面使用不同焦點、閃光燈、ISO、曝光度及曝光補償設定進行設定。 這個功能可用於建立高動態範圍 (HDR) 影像這類案例。

如果您想要擷取 HDR 影像，但不想要實作您自己的處理演算法，您可以使用 [**AdvancedPhotoCapture**](/uwp/api/Windows.Media.Capture.AdvancedPhotoCapture) API 來使用 Windows 內建的 HDR 功能。 如需詳細資訊，請參閱[高動態範圍 (HDR) 相片擷取](high-dynamic-range-hdr-photo-capture.md)。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 建議您先熟悉該文中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

## <a name="set-up-your-app-to-use-variable-photo-sequence-capture"></a>將您的 app 設定為使用可變相片序列擷取

除了基本媒體擷取所需的命名空間之外，實作可變相片序列擷取還需要下列命名空間。

[!code-cs[VPSUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSUsing)]

宣告一個成員變數來儲存 [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 物件，用來初始化相片序列擷取。 宣告 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 物件的陣列，以儲存序列中每個擷取的影像。 此外，宣告陣列來儲存每個框架的 [**CapturedFrameControlValues**](/uwp/api/Windows.Media.Capture.CapturedFrameControlValues) 物件。 這可以由您的影像處理演算法用來判斷要使用哪些設定來擷取每個框架。 最後，請宣告將用來追蹤目前正在擷取序列中之影像的索引。

[!code-cs[VPSMemberVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVPSMemberVariables)]

## <a name="prepare-the-variable-photo-sequence-capture"></a>準備可變相片序列擷取

初始化您的 [MediaCapture](./index.md) 之後，請確定目前裝置上支援可變相片序列，方法是從媒體擷取的 [**VideoDeviceController**](/uwp/api/Windows.Media.Devices.VideoDeviceController) 取得 [**VariablePhotoSequenceController**](/uwp/api/Windows.Media.Devices.Core.VariablePhotoSequenceController) 的執行個體，並檢查 [**Supported**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.supported) 屬性。

[!code-cs[IsVPSSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsVPSSupported)]

從可變相片序列控制器取得 [**FrameControlCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameControlCapabilities) 物件。 這個物件有可以針對相片序列每個畫面設定的屬性。 其中包含：

-   [**曝光**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposure)
-   [**ExposureCompensation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.exposurecompensation)
-   [**閃爍**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.flash)
-   [**焦點**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.focus)
-   [**IsoSpeed**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.isospeed)
-   [**PhotoConfirmation**](/uwp/api/windows.media.devices.core.framecontrolcapabilities.photoconfirmationsupported)

這個範例會為每個畫面設定不同曝光補償值。 若要確認目前裝置上的相片序列支援曝光補償，請檢查 [**FrameExposureCompensationCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameExposureCompensationCapabilities) 物件的 [**Supported**](/uwp/api/windows.media.devices.exposurecompensationcontrol.supported) 屬性，該物件可以透過 **ExposureCompensation** 屬性存取。

[!code-cs[IsExposureCompensationSupported](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetIsExposureCompensationSupported)]

針對您想要擷取的每個畫面建立新的 [**FrameController**](/uwp/api/Windows.Media.Devices.Core.FrameController) 物件。 這個範例會擷取三個畫面。 針對您想要的控制項設定值以符合每個畫面。 然後清除 **VariablePhotoSequenceController** 的 [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) 集合，並且將每個畫面控制器新增至集合。

[!code-cs[InitFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitFrameControllers)]

建立 [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) 物件，以設定您要用於已擷取影像的編碼。 呼叫靜態方法 [**MediaCapture.PrepareVariablePhotoSequenceCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparevariablephotosequencecaptureasync)，傳入編碼屬性。 這個方法會傳回 [**VariablePhotoSequenceCapture**](/uwp/api/Windows.Media.Capture.Core.VariablePhotoSequenceCapture) 物件。 最後，註冊 [**PhotoCaptured**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) 和 [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) 事件的事件處理常式。

[!code-cs[PrepareVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPrepareVPS)]

## <a name="start-the-variable-photo-sequence-capture"></a>開始可變相片序列擷取

若要開始可變相片序列的擷取，請呼叫 [**VariablePhotoSequenceCapture.StartAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.startasync)。 請務必初始化陣列，以儲存擷取的影像和畫面控制值。並且將目前的索引設為 0。 設定您 App 的錄製狀態變數並且更新 UI，以防止在此擷取進行中時開始另一個擷取。

[!code-cs[StartVPSCapture](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartVPSCapture)]

## <a name="receive-the-captured-frames"></a>接收擷取的畫面

針對每個擷取的畫面引發 [**PhotoCaptured**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.photocaptured) 事件。 儲存畫面控制值和畫面的已擷取影像，然後增加目前畫面索引。 這個範例示範如何取得每個畫面的 [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) 表示法。 如需使用 **SoftwareBitmap** 的詳細資訊，請參閱[影像處理](imaging.md)。

[!code-cs[OnPhotoCaptured](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnPhotoCaptured)]

## <a name="handle-the-completion-of-the-variable-photo-sequence-capture"></a>處理完成可變相片序列擷取

序列中的所有畫面都已擷取時，會引發 [**Stopped**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.stopped) 事件。 更新您的 app 的錄製狀態並更新 UI 以讓使用者初始化新的擷取。 此時，您可以將已擷取影像和畫面控制值傳遞到影像處理程式碼。

[!code-cs[OnStopped](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnStopped)]

## <a name="update-frame-controllers"></a>更新畫面控制器

如果您想要使用不同的每個畫面設定執行另一個可變相片序列擷取，您不需要完全重新初始化 **VariablePhotoSequenceCapture**。 您可以清除 [**DesiredFrameControllers**](/uwp/api/windows.media.devices.core.variablephotosequencecontroller.desiredframecontrollers) 集合，然後新增新畫面控制器或您可以修改現有的畫面控制器值。 下列範例會檢查 [**FrameFlashCapabilities**](/uwp/api/Windows.Media.Devices.Core.FrameFlashCapabilities) 物件來確認目前的裝置針對可變相片序列畫面支援閃光燈或閃光燈電源。 若是如此，會更新每個畫面以啟用 100% 電源的閃光燈。 先前為每個畫面設定的曝光補償值仍在使用中。

[!code-cs[UpdateFrameControllers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateFrameControllers)]

## <a name="clean-up-the-variable-photo-sequence-capture"></a>清除可變相片序列擷取

當您完成擷取可變相片序列或暫停您的 app 時，藉由呼叫 [**FinishAsync**](/uwp/api/windows.media.capture.core.variablephotosequencecapture.finishasync) 來清除可變相片序列物件。 取消註冊物件的事件處理常式並將它設定為 Null。

[!code-cs[CleanUpVPS](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVPS)]

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 