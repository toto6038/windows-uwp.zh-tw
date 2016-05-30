---
author: drewbatgit
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 您可以使用 Windows.Media.Transcoding API，將視訊檔案從一種格式轉碼成另一種格式。
title: 轉碼媒體檔案
---

# 轉碼媒體檔案

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


您可以使用 [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105) API，將視訊檔案從一種格式轉碼成另一種格式。

*轉碼*是數位媒體檔案 (例如視訊或音訊檔案) 的轉換，也就是從一種格式變成另一種格式。 通常先進行檔案的解碼，然後重新編碼，便產生新的格式。 例如，您可以將 Windows Media 檔案轉換成 MP4，以便在支援 MP4 格式的可攜式裝置上播放。 或者，您可以將高畫質影片檔案轉換成較低的解析度。 在這種情況下，重新編碼的檔案可以使用和原始檔案相同的轉碼器，但編碼設定檔不同。

## 設定您的專案進行轉碼

除了預設專案範本所參考的命名空間之外，您需要參考這些命名空間，以使用本文中的程式碼轉碼媒體檔案。

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## 選取來源和目的地檔案

您的 app 判斷轉碼的來源和目的地檔案的方式取決於您的實作。 這個範例使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者挑選來源和目的地檔案。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## 建立媒體編碼設定檔

編碼設定檔包含決定目的地檔案編碼方式的各項設定。 當您進行檔案的轉碼時，您可以在這裡找到很多相當實用的選項。

[
            **MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 類別提供用來建立預先定義的編碼設定檔的靜態方法：

-   WAV
-   AAC 音訊 (M4A)
-   MP3 音訊
-   Windows Media 音訊 (WMA)
-   AVI
-   MP4 視訊 (H.264 視訊加 AAC 音訊)
-   Windows Media 視訊 (WMV)

清單中的前四個設定檔只包含音訊。 其他三個包含視訊和音訊。

下列程式碼會建立 MP4 視訊的設定檔。

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

靜態 [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078) 方法會建立 MP4 編碼設定檔。 這個方法的參數會提供影片的目標解析度。 在這種情況下，[**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290) 表示 1280 x 720 個像素，每秒 30 個畫面。 (「720p」表示每一個畫面有 720 條漸進式掃描線。) 預先定義設定檔的其他方法都是沿用這個模式。

另一種方法是，您可以使用 [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047) 方法來建立一個符合現有媒體檔案的設定檔。 或者，如果您知道自己需要的正確編碼設定，可以建立新的 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 物件，並填入設定檔詳細資料。

## 檔案轉碼

若要進行檔案的轉碼，請建立新的 [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080) 物件並呼叫 [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936) 方法。 傳入來源檔案、目的地檔案，然後進行設定檔的編碼。 再呼叫 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 物件上的 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 方法，該物件是從非同步轉碼作業傳回的物件。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## 回應轉碼進度

您可以註冊事件在非同步進度 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 的進度變更時回應。 這些事件屬於通用 Windows 平台 (UWP) app 的非同步程式設計架構，而不是只屬於轉碼 API。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]

 

 






<!--HONumber=May16_HO2-->


