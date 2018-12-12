---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 您可以使用 Windows.Media.Transcoding API，將視訊檔案從一種格式轉碼成另一種格式。
title: 轉碼媒體檔案
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a6eb19ca5954b3ce71ecbaefe3339bee78f8717
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933516"
---
# <a name="transcode-media-files"></a>轉碼媒體檔案



您可以使用 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) API，將視訊檔案從一種格式轉碼成另一種格式。

*轉碼*是數位媒體檔案 (例如視訊或音訊檔案) 的轉換，也就是從一種格式變成另一種格式。 通常先進行檔案的解碼，然後重新編碼，便產生新的格式。 例如，您可以將 Windows Media 檔案轉換成 MP4，以便在支援 MP4 格式的可攜式裝置上播放。 或者，您可以將高畫質影片檔案轉換成較低的解析度。 在這種情況下，重新編碼的檔案可以使用和原始檔案相同的轉碼器，但編碼設定檔不同。

## <a name="set-up-your-project-for-transcoding"></a>設定您的專案進行轉碼

除了預設專案範本所參考的命名空間之外，您需要參考這些命名空間，以使用本文中的程式碼轉碼媒體檔案。

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>選取來源和目的地檔案

您的 app 判斷轉碼的來源和目的地檔案的方式取決於您的實作。 這個範例使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者挑選來源和目的地檔案。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>建立媒體編碼設定檔

編碼設定檔包含決定目的地檔案編碼方式的各項設定。 當您進行檔案的轉碼時，您可以在這裡找到很多相當實用的選項。

[**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 類別提供用來建立預先定義的編碼設定檔的靜態方法：

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>用於建立僅限音訊編碼設定檔的方法

方法  |設定檔  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple 無失真音訊轉碼器 (ALAC) 音訊         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |免費無失真音訊轉碼器 (FLAC) 音訊。         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC 音訊 (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 音訊         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV 音訊         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media 音訊 (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>用於建立音訊/視訊編碼設定檔的方法

方法  |設定檔  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |高效率視訊編碼 (HEVC) 視訊，也稱為 H.265 視訊 |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 視訊 (H.264 視訊加 AAC 音訊) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media 視訊 (WMV) |


下列程式碼會建立 MP4 視訊的設定檔。

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

靜態 [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 方法會建立 MP4 編碼設定檔。 這個方法的參數會提供影片的目標解析度。 在這種情況下，[**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) 表示 1280 x 720 個像素，每秒 30 個畫面。 (「720p」表示每一個畫面有 720 條漸進式掃描線。) 預先定義設定檔的其他方法都是沿用這個模式。

另一種方法是，您可以使用 [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) 方法來建立一個符合現有媒體檔案的設定檔。 或者，如果您知道自己需要的正確編碼設定，可以建立新的 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 物件，並填入設定檔詳細資料。

## <a name="transcode-the-file"></a>檔案轉碼

若要進行檔案的轉碼，請建立新的 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 物件並呼叫 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) 方法。 傳入來源檔案、目的地檔案，然後進行設定檔的編碼。 再呼叫 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 物件上的 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 方法，該物件是從非同步轉碼作業傳回的物件。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>回應轉碼進度

您可以註冊事件在非同步進度 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 的進度變更時回應。 這些事件屬於通用 Windows 平台 (UWP) app 的非同步程式設計架構，而不是只屬於轉碼 API。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>編碼中繼資料串流
從 Windows 10，版本 1803 起，您可以包含定時中繼資料時轉碼媒體檔案。 不同於視訊轉碼以上範例中，使用內建的媒體編碼設定檔建立方法，例如[**MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)，您必須手動建立中繼資料編碼設定檔以支援您編碼的中繼資料的類型.

這個中建立的中繼資料 incoding 設定檔的第一個步驟是建立 [**TimedMetadataEncodingProperties**] 物件描述的中繼資料要轉碼的編碼方式。 子類型屬性會指定的中繼資料類型的 GUID。 針對每個中繼資料類型編碼的詳細資料是專屬並不由 Windows 提供。 在這個範例中，會使用 GoPro 中繼資料 (gprs) 的 GUID。 接下來， [**SetFormatUserData**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata)呼叫來設定的描述資料流格式的中繼資料格式特定資料的二進位 blob。 下一步， **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor)會從編碼屬性，建立和播放軌標籤和名稱是以允許應用程式讀取 endcoded 資料流，找出中繼資料串流，並選擇性地在 UI 中顯示的資料流的名稱。 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

建立**TimedMetadataStreamDescriptor**之後, 您可以建立**MediaEncodingProfile**描述視訊、 音訊及中繼資料檔案中進行編碼。 **TimedMetadataStreamDescriptor**在最後一個範例中建立會傳遞到此範例協助程式函式，並藉由呼叫[**SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks)新增到**MediaEncodingProfile** 。

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




