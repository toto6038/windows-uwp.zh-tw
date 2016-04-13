---
ms.assetid: CC0D6E9B-128D-488B-912F-318F5EE2B8D3
description: 此文章說明如何使用 CameraCaptureUI 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。
title: 使用 CameraCaptureUI 擷取相片和視訊
---

# 使用 CameraCaptureUI 擷取相片和視訊

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


此文章說明如何使用 CameraCaptureUI 類別，透過 Windows 內建的相機 UI 來擷取相片或視訊。 這個功能很容易使用，可讓 app 只需幾行程式碼即可取得使用者所擷取的相片或視訊。

如果您的案例需要更健全的擷取作業低階控制，您應該使用 [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) 物件並實作您自己的擷取體驗。 如需詳細資訊，請參閱[使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)。

## 使用 CameraCaptureUI 擷取相片

若要使用相機擷取 UI，您應該在專案中包含 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) 命名空間。 若要使用傳回的影像檔案進行檔案作業，請包含 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346)。

[!code-cs[UsingCaptureUI](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingCaptureUI)]

若要擷取相片，請建立新的 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 物件。 使用物件的 [**PhotoSettings**](https://msdn.microsoft.com/library/windows/apps/br241058) 屬性，可以指定所傳回相片的屬性，例如相片的影像格式。 根據預設，相機擷取 UI 可讓使用者在傳回相片前進行裁剪，但可使用 [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 屬性予以停用。 此範例會設定 [**CroppedSizeInPixels**](https://msdn.microsoft.com/library/windows/apps/br241044) 以要求傳回的影像為 200 x 200 像素。

**注意：**行動裝置系列中的裝置不支援在 CameraCaptureUI 中進行影像裁剪。 當您的 app 在這些裝置上執行時會忽略 [**AllowCropping**](https://msdn.microsoft.com/library/windows/apps/br241042) 屬性的值。

呼叫 [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) 並指定 [**CameraCaptureUIMode.Photo**](https://msdn.microsoft.com/library/windows/apps/br241040)，來指定應該要擷取相片。 這個方法會傳回 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 包含影像的執行個體 (如果擷取成功的話)。 如果使用者取消擷取，則傳回的物件為 Null。

[!code-cs[CapturePhoto](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCapturePhoto)]

一旦擁有包含所擷取相片的 **StorageFile**，您即可建立 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 物件，該物件可搭配數個不同的通用 Windows app 功能使用。

首先，您應該在專案中包含 [**Windows.Graphics.Imaging**](https://msdn.microsoft.com/library/windows/apps/br226400) 命名空間。

[!code-cs[UsingSoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmap)]

呼叫 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) 以從影像檔取得資料流。 呼叫 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182) 以取得資料流的點陣圖解碼器。 然後呼叫 [**GetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887332) 以取得影像的 **SoftwareBitmap** 表示法。

[!code-cs[SoftwareBitmap](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmap)]

若要在 UI 中顯示影像，請在 XAML 頁面中宣告 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 控制項。

[!code-xml[ImageControl](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetImageControl)]

若要在 XAML 頁面中使用軟體點陣圖，請在專案中包含 using [**Windows.UI.Xaml.Media.Imaging**](https://msdn.microsoft.com/library/windows/apps/br243258) 命名空間。

[!code-cs[UsingSoftwareBitmapSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingSoftwareBitmapSource)]

**Image** 控制項要求影像來源為 BGRA8 格式 (具有預乘的 Alpha 或沒有 Alpha)，所以呼叫靜態方法 [**SoftwareBitmap.Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362) 以使用所需的格式建立新的軟體點陣圖。 接下來，建立新的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 物件並呼叫 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) 以將軟體點陣圖指派給來源。 最後，設定**Image** 控制項的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 屬性，以在 UI 中顯示所擷取的相片。

[!code-cs[SetImageSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetImageSource)]

## 使用 CameraCaptureUI 擷取視訊

若要擷取視訊，請建立新的 [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) 物件。 使用物件的 [**VideoSettings**](https://msdn.microsoft.com/library/windows/apps/br241059) 屬性，可以指定所傳回視訊的屬性，例如視訊的影像格式。

呼叫 [**CaptureFileAsync**](https://msdn.microsoft.com/library/windows/apps/br241057) 並指定 [**Video**](https://msdn.microsoft.com/library/windows/apps/br241059)，以指定應該擷取視訊。 這個方法會傳回 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 包含視訊的執行個體 (如果擷取成功的話)。 如果使用者取消擷取，則傳回的物件為 Null。

[!code-cs[CaptureVideo](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetCaptureVideo)]

您對擷取的視訊檔案所做的動作取決於您的 app 案例。 本文的其餘部分說明如何從一或多個擷取的視訊快速建立媒體組合並顯示在 UI 中。

首先，新增 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) 控制項，以便讓視訊組合顯示在 XAML 頁面中。

[!code-cs[MediaElement](./code/CameraCaptureUIWin10/cs/MainPage.xaml#SnippetMediaElement)]

將 [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565) 和 [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) 命名空間新增到您的專案。


[!code-cs[UsingMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetUsingMediaComposition)]

宣告 [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) 物件的成員變數以及您想要在頁面存留期間留在範圍內的 [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716)。

[!code-cs[DeclareMediaComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

在擷取任何視訊之前，您應該建立 **MediaComposition** 類別的新執行個體。

[!code-cs[InitComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetInitComposition)]

利用從相機擷取 UI 傳回的視訊檔案，藉由呼叫 [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607) 來建立新的 [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596)。 將媒體剪輯新增至合成的 [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) 集合。

呼叫 [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674)，從合成建立 **MediaStreamSource** 物件。

[!code-cs[AddToComposition](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetAddToComposition)]

最後，將資料流來源設為使用媒體元素的 [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029) 方法，以在 UI 中顯示合成。

[!code-cs[SetMediaElementSource](./code/CameraCaptureUIWin10/cs/MainPage.xaml.cs#SnippetSetMediaElementSource)]

您可以繼續擷取視訊短片，並將它們新增到組合。 如需媒體組合的詳細資訊，請參閱[媒體組合和編輯](media-compositions-and-editing.md)。

**注意**  
本文章適用於撰寫通用 Windows 平台 (UWP) app 的 Windows 10 開發人員。 如果您是為 Windows 8.x 或 Windows Phone 8.x 進行開發，請參閱[封存文件](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相關主題

* [使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)
* [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030)
 

 






<!--HONumber=Mar16_HO1-->


