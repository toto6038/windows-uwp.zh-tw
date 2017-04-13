---
author: drewbatgit
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: "查詢裝置已安裝的音訊及視訊編碼器與解碼器。"
title: "查詢已安裝的轉碼器"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 轉碼器, 編碼器, 解碼器, 查詢"
ms.openlocfilehash: 1bde2c24db6faa843d9118f330867e7f66b22e7e
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="query-for-installed-codecs"></a>查詢已安裝的轉碼器
本文告訴您如何查詢裝置已安裝的音訊及視訊編碼器與解碼器。 文章[支援的轉碼器](supported-codecs.md)列出每個裝置系列作業系統中所包含的轉碼器。 使用者或 App 可以在個別裝置上安裝其他轉碼器。 [CodecQuery](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery) 類別可以用來列出 App 執行所在裝置上目前已安裝的編碼器與解碼器集合。

**CodecQuery** 類別包含在 [windows.media.core](https://docs.microsoft.com/en-us/uwp/api/windows.media.core) 命名空間中，並提供一個不需要引數的建構函式。

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

**CodecQuery** 類別的 [FindAllAsync](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecquery#Windows_Media_Core_CodecQuery_FindAllAsync_Windows_Media_Core_CodecKind_Windows_Media_Core_CodecCategory_System_String_) 方法會傳回所有符合指定之參數的已安裝轉碼器。 這些參數包含 [CodecKind](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeckind) 列舉中的值 (這個值指定要傳回的是音訊還是視訊轉碼器) 和 [CodecCategory](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codeccategory) 值 (指定要傳回的是音訊還是視訊轉碼器)。

最後一個參數是表示媒體子類型 (例如 H264 視訊或 FLAC 音訊) 的字串，這個媒體子類型必須是 **FindAllAsync** 傳回的任何轉碼器所支援的類型。 您可以將 null 或空白字串指定給這個參數，傳送所有媒體子類型的轉碼器。 下列範例會列出目前的裝置上已安裝的所有視訊編碼器。

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

如果您指定媒體子類型，就必須使用[音訊子類型 GUID](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) 或[視訊子類型 GUID](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx) 中所列出的其中一個子類型 GUID 的字串表示。 [CodecSubtypes](https://docs.microsoft.com/en-us/uwp/api/windows.media.core.codecsubtypes) 類別提供大部分的受支援媒體子類型的屬性，這些屬性會傳回子類型 GUID 的字串表示。 您也可以將 FOURCC 代碼指定給媒體子類型參數。 如需詳細資訊，請參閱 [FOURCC 代碼](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx)。 

下列範例使用 FOURCC 代碼來查詢支援 H.264 視訊的視訊解碼器。 此作業的其中一個範例案例是在嘗試播放視訊檔案之前檢查看看是否支援視訊格式。

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

下列範本會查詢支援 FLAC 媒體子類型的音訊編碼器。 這個範例接著為媒體子類型建立 AudioEncodinAudioEncodingProperties 物件和 MediaEncodingProfile。 這可以用來錄製指定之格式的音訊，或從其他格式轉碼音訊。 如需詳細資訊，請參閱[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)和[轉碼媒體檔案](transcode-media-files.md)。

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>相關主題

* [支援的轉碼器](supported-codecs.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [轉碼媒體檔案](transcode-media-files.md)
 

 




