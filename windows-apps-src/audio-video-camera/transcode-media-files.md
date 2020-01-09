---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 您可以使用 Windows.Media.Transcoding API，將視訊檔案從一種格式轉碼成另一種格式。
title: 轉碼媒體檔案
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 38aef2779908e173712bda0f35ca9e0651fb786b
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683871"
---
# <a name="transcode-media-files"></a>轉碼媒體檔案



您可以使用 [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding) API，將視訊檔案從一種格式轉碼成另一種格式。

*轉碼*是數位媒體檔案 (例如視訊或音訊檔案) 的轉換，也就是從一種格式變成另一種格式。 通常先進行檔案的解碼，然後重新編碼，便產生新的格式。 例如，您可以將 Windows Media 檔案轉換成 MP4，以便在支援 MP4 格式的可攜式裝置上播放。 或者，您可以將高畫質影片檔案轉換成較低的解析度。 在這種情況下，重新編碼的檔案可以使用和原始檔案相同的轉碼器，但編碼設定檔不同。

## <a name="set-up-your-project-for-transcoding"></a>設定您的專案進行轉碼

除了預設專案範本所參考的命名空間之外，您需要參考這些命名空間，以使用本文中的程式碼轉碼媒體檔案。

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>選取來源和目的地檔案

您的 app 判斷轉碼的來源和目的地檔案的方式取決於您的實作。 這個範例使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) 讓使用者挑選來源和目的地檔案。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>建立媒體編碼設定檔

編碼設定檔包含決定目的地檔案編碼方式的各項設定。 當您進行檔案的轉碼時，您可以在這裡找到很多相當實用的選項。

[  **MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 類別提供用來建立預先定義的編碼設定檔的靜態方法：

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>用於建立僅限音訊編碼設定檔的方法

方法  |Profile  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple 無失真音訊轉碼器 (ALAC) 音訊         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |免費無失真音訊轉碼器 (FLAC) 音訊。         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC 音訊 (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 音訊         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV 音訊         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media 音訊 (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>用於建立音訊/視訊編碼設定檔的方法

方法  |Profile  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |高效率視訊編碼 (HEVC) 視訊，也稱為 H.265 視訊 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 視訊 (H.264 視訊加 AAC 音訊) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media 視訊 (WMV) |


下列程式碼會建立 MP4 視訊的設定檔。

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

靜態 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 方法會建立 MP4 編碼設定檔。 這個方法的參數會提供影片的目標解析度。 在這種情況下，[**VideoEncodingQuality.hd720p**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) 表示 1280 x 720 個像素，每秒 30 個畫面。 (「720p」表示每一個畫面有 720 條漸進式掃描線。) 預先定義設定檔的其他方法都是沿用這個模式。

另一種方法是，您可以使用 [**MediaEncodingProfile.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync) 方法來建立一個符合現有媒體檔案的設定檔。 或者，如果您知道自己需要的正確編碼設定，可以建立新的 [**MediaEncodingProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 物件，並填入設定檔詳細資料。

## <a name="transcode-the-file"></a>檔案轉碼

若要進行檔案的轉碼，請建立新的 [**MediaTranscoder**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.MediaTranscoder) 物件並呼叫 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) 方法。 傳入來源檔案、目的地檔案，然後進行設定檔的編碼。 再呼叫 [**PrepareTranscodeResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) 物件上的 [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 方法，該物件是從非同步轉碼作業傳回的物件。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>回應轉碼進度

您可以註冊事件在非同步進度 [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 的進度變更時回應。 這些事件屬於通用 Windows 平台 (UWP) app 的非同步程式設計架構，而不是只屬於轉碼 API。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>編碼中繼資料串流
從 Windows 10 版本1803開始，您可以在轉碼媒體檔案時包含計時中繼資料。 不同于上述的影片轉碼範例，其使用內建的媒體編碼設定檔建立方法，例如[**MediaEncodingProfile. CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)，您必須手動建立中繼資料編碼設定檔，以支援您所編碼的元資料類型。

建立中繼資料 incoding 設定檔的第一個步驟是建立一個 [**TimedMetadataEncodingProperties**] 物件，以描述要轉碼之中繼資料的編碼方式。 子類型屬性是指定元資料類型的 GUID。 每個元資料類型的編碼細節都是專屬的，而且不是由 Windows 提供。 在此範例中，會使用 GoPro 中繼資料（gprs）的 GUID。 接下來，呼叫[**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata)來設定資料的二進位 blob，以描述元資料格式特有的資料流程格式。 接下來， **TimedMetadataStreamDescriptor**（ https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) 是從編碼屬性建立，而追蹤標籤和名稱則是允許應用程式讀取 endcoded 串流以識別中繼資料資料流程，並選擇性地在 UI 中顯示資料流程名稱。 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

建立**TimedMetadataStreamDescriptor**之後，您可以建立**MediaEncodingProfile**來描述要在檔案中編碼的影片、音訊和中繼資料。 在最後一個範例中建立的**TimedMetadataStreamDescriptor**會傳遞至這個範例 helper 函式，並藉由呼叫[**SetTimedMetadataTracks**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks)來加入至**MediaEncodingProfile** 。

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




