---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: 查詢裝置已安裝的音訊及視訊編碼器與解碼器。
title: 查詢已安裝的轉碼器
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 轉碼器, 編碼器, 解碼器, 查詢
ms.localizationpriority: medium
ms.openlocfilehash: 4241aad5a01617d6a002c6f5d6da0a4bb1455616
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8687085"
---
# <a name="query-for-codecs-installed-on-a-device"></a>查詢裝置已安裝的轉碼器
**[CodecQuery](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery)** 類別可讓您查詢目前的裝置上已安裝的轉碼器。 Windows 10 針對不同裝置系列所隨附的轉碼器清單列在[支援的轉碼器](supported-codecs.md)文章中，但因為使用者和應用程式可以在裝置上安裝其他的轉碼器，所以您可能會想要查詢執行階段的轉碼器支援，以判斷目前裝置提供哪些轉碼器。

CodecQuery API 屬於 **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** 命名空間，因此您需要在您的應用程式中包括這個命名空間。

CodecQuery API 屬於 **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)** 命名空間，因此您需要在您的應用程式中包括這個命名空間。

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

呼叫建構函式，以初始化**CodecQuery**類別的新執行個體。

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

**[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery.findallasync)** 方法會傳回符合所提供參數的所有已安裝轉碼器。 這些參數包括指定查詢音訊和 (或) 視訊轉碼器的 **[CodecKind](https://docs.microsoft.com/uwp/api/windows.media.core.codeckind)** 值、指定查詢編碼器還是解碼器的 **[CodecCategory](https://docs.microsoft.com/uwp/api/windows.media.core.codeccategory)** 值，以及代表您正在查詢之媒體編碼子類型的字串，如 H.264 視訊或 MP3 音訊。

針對子類型值指定空字串或空值，以傳回所有子類型的轉碼器。 下列範例會列出裝置上已安裝的所有視訊編碼器。

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

您傳入**FindAllAsync**的子類型字串可以是子類型 GUID 的字串表示，而 GUID 是由系統或子類型的 FOURCC 程式碼所定義。 這組支援的媒體子類型 GUID 列在[音訊子類型 GUID](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) (英文) 和[視訊子類型 GUID](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx) (英文) 文章中，但 **[CodecSubtypes](https://docs.microsoft.com/uwp/api/windows.media.core.codecsubtypes)** 類別提供屬性，以傳回每個所支援子類型的 GUID 值。 如需 FOURCC 代碼的詳細資訊，請參閱 [FOURCC 代碼](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx)(英文)。 

下列範例指定 FOURCC 代碼 "H264"，判斷裝置上是否已安裝 H.264 視訊解碼器。 您可以先執行這項查詢，再嘗試播放 H.264 視訊內容。 您也可以在播放時處理不受支援的轉碼器。 如需詳細資訊，請參閱[處理開啟媒體項目時不支援的轉碼器和不明的錯誤](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items)。

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

下列範例查詢判斷目前的裝置上是否已安裝 FLAC 音訊編碼器，如果已安裝，則會建立子類型的**[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)**，這個子類型可以用於將音訊擷取到檔案，或將音訊從另一種格式轉碼為 FLAC 音訊檔案。

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [轉碼媒體檔案](transcode-media-files.md)
* [支援的轉碼器](supported-codecs.md)
 

 




