---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: 本主題說明如何將效果套用到相機預覽和錄製中的視訊串流，並示範如何使用影像防震效果。
title: 視訊擷取的效果
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d427a532e9821b81b6f23d08babecd692c8c95e1
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698350"
---
# <a name="effects-for-video-capture"></a>視訊擷取的效果


本主題說明如何將效果套用到相機預覽和錄製中的視訊串流，並示範如何使用影像防震效果。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 我們建議您先熟悉該文章中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文章中的程式碼假設您的 App 已有正確初始化的 MediaCapture 執行個體。

## <a name="adding-and-removing-effects-from-the-camera-video-stream"></a>針對相機視訊串流新增和移除效果
若要從裝置的相機擷取或預覽視訊，您必須使用在[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中所描述的 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) 物件。 在您初始化 **MediaCapture** 物件之後，若要將一或多個視訊效果新增到預覽或擷取串流，您可以呼叫 [**AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035)，傳遞代表要新增之效果的 [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Effects.IVideoEffectDefinition) 物件，以及指出效果應該新增到相機的預覽串流或錄製串流的 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) 列舉成員。

> [!NOTE]
> 在某些裝置上，預覽串流和擷取串流是相同的，這表示當您呼叫 **AddVideoEffectAsync** 時，若您指定 **MediaStreamType.VideoPreview** 或 **MediaStreamType.VideoRecord**，該效果將會同時套用到預覽和錄製串流。 您可以透過檢查針對 **MediaCapture** 物件之 [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.MediaCaptureSettings) 的 [**VideoDeviceCharacteristic**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings.VideoDeviceCharacteristic) 屬性，來判斷目前裝置上的預覽和錄製串流是否是相同的。 如果此屬性的值為 **VideoDeviceCharacteristic.AllStreamsIdentical** 或 **VideoDeviceCharacteristic.PreviewRecordStreamsIdentical**，便代表這兩個串流是相同的，而您針對任何一個串流所套用的效果，也將會套用到另外一個串流。

下列範例會將效果套用到相機預覽和錄製串流。 此範例將示範如何檢查以確認錄製和預覽串流是相同的。

[!code-cs[BasicAddEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetBasicAddEffect)]

請注意，**AddVideoEffectAsync** 會傳回實作代表已新增視訊效果之 [**IMediaExtension**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.IMediaExtension) 的物件。 某些效果能讓您透過將 [**PropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Foundation.Collections.PropertySet) 傳遞到 [**SetProperties**](https://msdn.microsoft.com/library/windows/apps/br240986) 方法中，來變更效果設定。

從 Windows10 版本 1607 開始，您也可以將由 **AddVideoEffectAsync** 傳回的物件傳遞到 [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957)，來從視訊管線移除效果。 **RemoveEffectAsync** 會自動判斷效果物件參數是否已新增到預覽或錄製串流，使您不需在進行呼叫時指定串流類型。

[!code-cs[RemoveOneEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetRemoveOneEffect)]

您也可以透過呼叫 [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) 並指定要移除效果的串流，來將所有的效果從預覽或擷取串流中移除。

[!code-cs[ClearAllEffects](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetClearAllEffects)]

## <a name="video-stabilization-effect"></a>影像防震效果

影像防震效果會處理視訊串流的框架，將手持擷取裝置造成搖晃的情況減到最少。 因為這項技術會導致像素右移、左移、上移和下移，也因為此效果無從得知超出視訊框架外的內容是什麼，所以會稍微裁剪原始視訊來得到防震視訊。 有一個公用程式函式可讓您調整視訊編碼設定，以最佳方式管理此效果所執行的裁剪工作。

在支援光學影像防震 (OIS) 的裝置上，OIS 會操作擷取裝置的機械來穩定視訊，因此不需要裁剪視訊畫面的邊緣。 如需詳細資訊，請參閱[視訊擷取的擷取裝置控制項](capture-device-controls-for-video-capture.md)。

### <a name="set-up-your-app-to-use-video-stabilization"></a>將您的 app 設定為使用影像防震

除了基本媒體擷取所需的命名空間，使用影像防震效果還需要下列命名空間。

[!code-cs[VideoStabilizationEffectUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEffectUsing)]

宣告成員變數來儲存 [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) 物件。 在效果實作中，您將會修改擷取到的視訊在編碼時所用的編碼屬性。 宣告兩個變數來儲存初始輸入和輸出編碼屬性的備份，以便稍後停用效果時可以還原屬性。 最後，因為將會從程式碼內的多個位置存取此物件，請宣告 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 類型的成員變數。

[!code-cs[DeclareVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

在這個案例中，您應該將媒體編碼設定檔物件指派給成員變數，以便稍後存取。

[!code-cs[EncodingProfileMember](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetEncodingProfileMember)]

### <a name="initialize-the-video-stabilization-effect"></a>初始化影像防震效果

在 **MediaCapture** 物件初始化之後，建立 [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) 物件的新執行個體。 呼叫 [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) 將此效果加入至視訊管線，並擷取 [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) 類別的執行個體。 指定 [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) 表示將此效果套用至視訊錄製資料流。

註冊 [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) 事件的事件處理常式，並呼叫協助程式方法 **SetUpVideoStabilizationRecommendationAsync**，本文稍後討論這兩項。 最後，將此效果的 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) 屬性設為 true 來啟用效果。

[!code-cs[CreateVideoStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### <a name="use-recommended-encoding-properties"></a>使用建議的編碼屬性

如本文稍早所述，影像防震效果所採用的技術，一定會稍微裁剪原始視訊來得到防震視訊。 在程式碼中定義下列 Helper 函式，經由調整視訊編碼屬性，以最佳方式處理效果的這項限制。 使用影像防震效果並不需要此步驟，但如果您不執行此步驟，產生的影片會稍微放大，因而稍微降低視覺逼真度。

在影像防震效果執行個體上呼叫 [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983)，傳入 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) 物件，讓效果知道您目前的輸入資料流編碼屬性，並傳入 **MediaEncodingProfile**，讓效果知道您目前的輸出編碼屬性。 這個方法會傳回 [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) 物件，其中包含最新建議的輸入和輸出資料流編碼屬性。

建議的輸入編碼屬性 (如果裝置支援) 是比您提供的初始設定更高的解析度，因此在套用效果裁剪之後，解析度損失最少。

呼叫 [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) 設定新的編碼屬性。 設定新的屬性之前，請使用成員變數來儲存初始編碼屬性，以便停用效果時可以恢復設定。

如果影像防震效果必須裁剪輸出視訊，則建議的輸出編碼屬性為裁剪後的視訊大小。 這表示輸出解析度會符合裁剪的視訊大小。 如果您不使用建議的輸出屬性，則視訊會放大以符合初始輸出大小，而這會導致視覺逼真度降低。

設定 **MediaEncodingProfile** 物件的 [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) 屬性。 設定新的屬性之前，請使用成員變數來儲存初始編碼屬性，以便停用效果時可以恢復設定。

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### <a name="handle-the-video-stabilization-effect-being-disabled"></a>處理停用的影像防震效果

如果像素輸送量太高，超過防震效果的處理能力，或如果系統偵測到效果執行速度變慢，則系統可能會自動停用影像防震效果。 如果發生這種情況，則會引發 EnabledChanged 事件。 *sender* 參數中的 **VideoStabilizationEffect** 執行個體指出效果的新狀態：啟用或停用。 [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) 具有 [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) 值，指出效果啟用或停用的原因。 請注意，如果您以程式設計方式啟用或停用效果，則也會引發這個事件，在此情況下原因是 **Programmatic**。

通常您會使用此事件來調整您 app 的 UI，指示目前的影像防震狀態。

[!code-cs[VideoStabilizationEnabledChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### <a name="clean-up-the-video-stabilization-effect"></a>清除影像防震效果

若要清除影像防震效果，請呼叫 [**RemoveEffectAsync**](https://msdn.microsoft.com/library/windows/apps/mt667957) 以從視訊管線移除效果。 如果包含初始編碼屬性的成員變數不是 Null，請使用它們來還原編碼屬性。 最後，移除 **EnabledChanged** 事件處理常式並將效果設定為 Null。

[!code-cs[CleanUpVisualStabilizationEffect](./code/SimpleCameraPreview_Win10/cs/MainPage.Effects.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




