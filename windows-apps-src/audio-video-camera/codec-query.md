---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: 查詢裝置已安裝的音訊及視訊編碼器與解碼器。
title: 查詢已安裝的轉碼器
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 轉碼器, 編碼器, 解碼器, 查詢
ms.localizationpriority: medium
ms.openlocfilehash: 75aac91f41a854ee21a3ccfaf5b9a9c0f19bfef8
ms.sourcegitcommit: d7783efb1c60b81e94898294fc5794c1d3320004
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/30/2021
ms.locfileid: "105982631"
---
# <a name="query-for-codecs-installed-on-a-device"></a>查詢裝置已安裝的轉碼器
**[CodecQuery](/uwp/api/windows.media.core.codecquery)** 類別可讓您查詢目前裝置上所安裝的編解碼器。 Windows 10 針對不同裝置系列所隨附的轉碼器清單列在[支援的轉碼器](supported-codecs.md)文章中，但因為使用者和應用程式可以在裝置上安裝其他的轉碼器，所以您可能會想要查詢執行階段的轉碼器支援，以判斷目前裝置提供哪些轉碼器。

CodecQuery API 屬於 **[Windows.Media.Core](/uwp/api/windows.media.core)** 命名空間，因此您需要在您的應用程式中包括這個命名空間。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetCodecQueryUsing":::

呼叫建構函式，以初始化 **CodecQuery** 類別的新執行個體。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetNewCodecQuery":::

**[FindAllAsync](/uwp/api/windows.media.core.codecquery.findallasync)** 方法會傳回所有符合所提供參數的已安裝編解碼器。 這些參數包括 **[CodecKind](/uwp/api/windows.media.core.codeckind)** 值，指定您是要查詢音訊或影片編解碼器或兩者、指定您是否要查詢編碼器或解碼器的 **[CodecCategory](/uwp/api/windows.media.core.codeccategory)** 值，以及代表您要查詢之媒體編碼子類型的字串，例如 h.264 影片或 MP3 音訊。

指定子類型值的空字串，以傳回所有子類型的編解碼器。 下列範例會列出裝置上已安裝的所有視訊編碼器。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetFindAllEncoders":::

您傳入 **FindAllAsync** 的子類型字串可以是子類型 GUID 的字串表示，而 GUID 是由系統或子類型的 FOURCC 程式碼所定義。 這組支援的媒體子類型 GUID 列在 [音訊子類型 GUID](/windows/desktop/medfound/audio-subtype-guids) (英文) 和 [視訊子類型 GUID](/windows/desktop/medfound/video-subtype-guids) (英文) 文章中，但 **[CodecSubtypes](/uwp/api/windows.media.core.codecsubtypes)** 類別提供屬性，以傳回每個所支援子類型的 GUID 值。 如需 FOURCC 代碼的詳細資訊，請參閱 [FOURCC 代碼](/windows/desktop/DirectShow/fourcc-codes)(英文)。 

下列範例指定 FOURCC 代碼 "H264"，判斷裝置上是否已安裝 H.264 視訊解碼器。 您可以先執行這項查詢，再嘗試播放 H.264 視訊內容。 您也可以在播放時處理不受支援的轉碼器。 如需詳細資訊，請參閱[處理開啟媒體項目時不支援的轉碼器和不明的錯誤](./media-playback-with-mediasource.md#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsH264Supported":::

下列範例會查詢以判斷 FLAC 音訊編碼器是否已安裝在目前的裝置上，如果是，則會針對可用於將音訊捕獲到檔案或將音訊從另一個格式轉換成 FLAC 音訊檔案的子類型建立 **[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** 。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsFLACSupported":::

## <a name="related-topics"></a>相關主題

* [媒體播放](media-playback.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [轉碼媒體檔案](transcode-media-files.md)
* [支援的轉碼器](supported-codecs.md)
 

 
