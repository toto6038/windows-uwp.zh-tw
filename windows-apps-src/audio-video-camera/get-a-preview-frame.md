---
ms.assetid: 05E418B4-5A62-42BD-BF66-A0762216D033
description: 本主題示範如何從媒體擷取預覽資料流取得單一預覽畫面。
title: 取得預覽框架
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 7faa018dad336b6e22dd236e57585cade38f8a94
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476668"
---
# <a name="get-a-preview-frame"></a>取得預覽框架


本主題示範如何從媒體擷取預覽串流取得單一預覽畫面。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 我們建議您先熟悉該文章中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體，而且您有一個具有使用中視訊預覽資料流的 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278)。

除了基本媒體擷取所需的命名空間，擷取預覽畫面還需要下列命名空間。

[!code-cs[PreviewFrameUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewFrameUsing)]

當您要求預覽框架時，您可以使用想要的格式建立 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 物件，以指定您要用來接收框架的格式。 這個範例會呼叫 [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) 並指定 [**MediaStreamType.VideoPreview**](https://msdn.microsoft.com/library/windows/apps/br226640) 來要求預覽資料流的屬性，以建立與預覽資料流解析度相同的視訊框架。 預覽串流的寬度和高度將用來建立新的視訊框架。

[!code-cs[CreateFormatFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateFormatFrame)]

如果您的 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 物件已初始化，而且您有使用中的預覽串流，請呼叫 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926711) 取得預覽串流。 傳入上一個步驟中建立的視訊框架，以指定傳回框架的格式。

[!code-cs[GetPreviewFrameAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewFrameAsync)]

存取 [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 物件的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) 屬性，以取得預覽畫面的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 表示。 如需有關儲存、載入和修改軟體點陣圖的資訊，請參閱[影像處理](imaging.md)。

[!code-cs[GetPreviewBitmap](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewBitmap)]

如果您想要透過 Direct3D API 使用影像，您也可以取得預覽框架的 [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) 表示。

[!code-cs[GetPreviewSurface](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPreviewSurface)]

> [!IMPORTANT]
> 無論是傳回之 **VideoFrame** 的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn930926) 屬性或 [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn930920) 屬性，根據您如何呼叫 **GetPreviewFrameAsync** 以及根據正在執行您 app 的裝置，這些屬性可能會是 Null。

> - 如果您呼叫能接受 **VideoFrame** 引數之 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926713) 的多載，則傳回的 **VideoFrame** 會有非 Null 的 **SoftwareBitmap**，且 **Direct3DSurface** 屬性將會是 Null。
> - 如果您呼叫之 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) 的多載，在使用 Direct3D 表面於內部呈現畫面的裝置上沒有任何引數，則 **Direct3DSurface** 屬性將會是非 Null，且 **SoftwareBitmap** 屬性將會是 Null。
> - 如果您呼叫之 [**GetPreviewFrameAsync**](https://msdn.microsoft.com/library/windows/apps/dn926712) 的多載，在不使用 Direct3D 表面來在內部代表框架的裝置上沒有任何引數，則 **SoftwareBitmap** 屬性將會是非 Null，且 **Direct3DSurface** 屬性將會是 Null。

您的 app 在嘗試於 **SoftwareBitmap** 或 **Direct3DSurface** 屬性傳回的物件上操作之前，應該先檢查是否有 Null 值。

預覽框架使用完畢後，請務必呼叫其 [**Close**](https://msdn.microsoft.com/library/windows/apps/dn930918) 方法 (對應於 C# 中的 Dispose)，以釋放框架使用的資源。 或者，使用 **using** 模式來自動處置物件。

[!code-cs[CleanUpPreviewFrame](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpPreviewFrame)]

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




