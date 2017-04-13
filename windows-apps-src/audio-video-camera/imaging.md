---
author: laurenhughes
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: "本文說明如何使用 BitmapDecoder 和 BitmapEncoder 來載入及儲存影像檔，以及如何使用 SoftwareBitmap 物件來代表點陣圖影像。"
title: "建立、編輯和儲存點陣圖影像"
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: f0cf9d2928c8d6a0494092643daa19d9b437d3eb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-edit-and-save-bitmap-images"></a>建立、編輯和儲存點陣圖影像

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文說明如何使用 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 和 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 來載入及儲存影像檔，以及如何使用 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 物件來代表點陣圖影像。

**SoftwareBitmap** 類別是可從多個來源建立的多用途 API，包括影像檔、[**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/br243259) 物件、Direct3D 外觀和程式碼。 **SoftwareBitmap** 可讓您輕鬆地在不同的像素格式與 Alpha 模式之間轉換，並允許低階存取像素資料。 此外，**SoftwareBitmap** 是許多 Windows 功能經常使用的介面，包括：

-   [**CapturedFrame**](https://msdn.microsoft.com/library/windows/apps/dn278725) 可讓您以 **SoftwareBitmap** 取得相機所拍攝的畫面。

-   [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/dn930917) 可讓您取得 **VideoFrame** 的 **SoftwareBitmap** 表示。

-   [**FaceDetector**](https://msdn.microsoft.com/library/windows/apps/dn974129) 可讓您偵測 **SoftwareBitmap** 中的人臉。

本文中的範例程式碼使用下列命名空間中的 API。

[!code-cs[命名空間](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>使用 BitmapDecoder 從影像檔建立 SoftwareBitmap

若要從檔案建立 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358)，請取得包含影像資料的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 執行個體。 這個範例使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 讓使用者選取影像檔。

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

呼叫 **StorageFile** 物件的 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) 方法，取得包含影像資料的隨機存取資料流。 呼叫靜態方法 [**BitmapDecoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226182)，取得指定資料流的 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 類別執行個體。 呼叫 [**GetSoftwareBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn887332)，取得包含影像的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 物件。

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>使用 BitmapEncoder 將 SoftwareBitmap 儲存為檔案

若要將 **SoftwareBitmap** 儲存為檔案，請取得用來儲存影像的 **StorageFile** 執行個體。 這個範例使用 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 讓使用者選取輸出檔。

[!code-cs[PickOuputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOuputFile)]

呼叫 **StorageFile** 物件的 [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/br227116) 方法，取得將寫入影像的隨機存取資料流。 呼叫靜態方法 [**BitmapEncoder.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/br226211)，取得指定資料流的 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206) 類別執行個體。 **CreateAsync** 的第一個參數是 GUID，代表應該用來編碼影像的轉碼器。 **BitmapEncoder** 類別公開的一個屬性包含編碼器支援的每個轉碼器的識別碼，例如 [**JpegEncoderId**](https://msdn.microsoft.com/library/windows/apps/br226226)。

使用 [**SetSoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887337) 方法來設定將編碼的影像。 您可以設定 [**BitmapTransform**](https://msdn.microsoft.com/library/windows/apps/br226254) 屬性的值，對正在編碼的影像套用基本轉換。 [**IsThumbnailGenerated**](https://msdn.microsoft.com/library/windows/apps/br226225) 屬性決定編碼器是否產生縮圖。 請注意，並非所有檔案格式都支援縮圖，當您使用這項功能時，如果不支援縮圖，則會擲回不支援的作業錯誤。

呼叫 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216)，使編碼器將影像資料寫入指定的檔案。

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

您可以在建立 **BitmapEncoder** 時指定額外的編碼選項，方法是建立新的 [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) 物件，在其中填入一或多個代表編碼器設定的 [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) 物件。 如需支援的編碼器選項清單，請參閱 [BitmapEncoder 選項參考](bitmapencoder-options-reference.md)。

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>透過 XAML 影像控制項使用 SoftwareBitmap

若要在 XAML 頁面內使用 [**Image**](https://msdn.microsoft.com/library/windows/apps/br242752) 控制項顯示影像，請先在 XAML 頁面中定義 **Image** 控制項。

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

目前，**Image** 控制項只支援使用 BGRA8 編碼、預乘或無 Alpha 色板的影像。 嘗試顯示影像之前，請先測試以確定它具有正確的格式，如果沒有，請使用 **SoftwareBitmap** 靜態[**轉換**](https://msdn.microsoft.com/library/windows/apps/dn887362)方法，將影像轉換成支援的格式。

建立新的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 物件。 呼叫 [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856)，同時傳入 **SoftwareBitmap**，以設定來源物件的內容。 然後，您就可以將 **Image** 控制項的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br242760) 屬性，設定為新建立的 **SoftwareBitmapSource**。

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

您也可以使用 **SoftwareBitmapSource** 將 **SoftwareBitmap** 設定為 [**ImageBrush**](https://msdn.microsoft.com/library/windows/apps/br210101) 的 [**ImageSource**](https://msdn.microsoft.com/library/windows/apps/br210105)。

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>從 WriteableBitmap 建立 SoftwareBitmap

您可以呼叫 [**SoftwareBitmap.CreateCopyFromBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887370)，並提供 **WriteableBitmap** 的 **PixelBuffer** 屬性來設定像素資料，從現有的 **WriteableBitmap** 建立 **SoftwareBitmap**。 第二個引數可讓您要求新建立的 **WriteableBitmap** 的像素格式。 您可以使用 **WriteableBitmap** 的 [**PixelWidth**](https://msdn.microsoft.com/library/windows/apps/br243253) 和 [**PixelHeight**](https://msdn.microsoft.com/library/windows/apps/br243251) 屬性，指定新影像的尺寸。

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>以程式設計方式建立或編輯 SoftwareBitmap

本主題到目前為止已討論影像檔的處理方式。 您也可以在程式碼中建立新的 **SoftwareBitmap**，並使用相同的技術來存取和修改 **SoftwareBitmap** 的像素資料。

**SoftwareBitmap** 使用 COM Interop 公開包含像素資料的原始緩衝區。

若要使用 COM Interop，您必須在專案中加入 **System.Runtime.InteropServices** 命名空間的參考。

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

在您的命名空間內新增下列程式碼，以初始化 [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) COM 介面。

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

以您想要的像素格式和大小，建立新的 **SoftwareBitmap**。 或者，使用您想要編輯像素資料的現有 **SoftwareBitmap**。 呼叫 [**SoftwareBitmap.LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887380)，取得代表像素資料緩衝區的 [**BitmapBuffer**](https://msdn.microsoft.com/library/windows/apps/dn887325) 類別執行個體。 將 **BitmapBuffer** 轉型為 **IMemoryBufferByteAccess** COM 介面，然後呼叫 [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506)，將資料填入位元組陣列中。 使用 [**BitmapBuffer.GetPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887330) 方法來取得 [**BitmapPlaneDescription**](https://msdn.microsoft.com/library/windows/apps/dn887342) 物件，幫助您計算每個像素在緩衝區中的位移。

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

因為此方法會存取底層 Windows 執行階段類型的原始緩衝區，所以必須使用 **unsafe** 關鍵字來宣告它。 您也必須在 Microsoft Visual Studio 中設定您的專案，以允許不安全的程式碼編譯，其做法是開啟專案的 **\[屬性\]** 頁面、按一下 **\[建置\]** 屬性頁，然後選取 **\[容許 Unsafe 程式碼\]** 核取方塊。

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>從 Direct3D 外觀建立 SoftwareBitmap

若要從 Direct3D 表面建立 **SoftwareBitmap** 物件，您必須在專案中加入 [**Windows.Graphics.DirectX.Direct3D11**](https://msdn.microsoft.com/library/windows/apps/dn895104) 命名空間。

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

呼叫 [**CreateCopyFromSurfaceAsync**](https://msdn.microsoft.com/library/windows/apps/dn887373) 以從表面建立新的 **SoftwareBitmap**。 顧名思義，新的 **SoftwareBitmap** 有個別的影像資料複本。 修改 **SoftwareBitmap** 完全不會影響 Direct3D 表面。

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>將 SoftwareBitmap 轉換成不同的像素格式

**SoftwareBitmap** 類別提供靜態方法 [**Convert**](https://msdn.microsoft.com/library/windows/apps/dn887362)，可讓您從現有的 **SoftwareBitmap**，使用您指定的像素格式和 Alpha 模式，輕鬆地建立新的 **SoftwareBitmap**。 請注意，新建立的點陣圖有個別的影像資料複本。 修改新的點陣圖不會影響原始點陣圖。

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>對影像檔進行轉碼

您可以將影像檔直接從 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) 轉碼成 [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206)。 從要轉碼的檔案建立 [**IRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241731)。 從輸入資料流建立新的 **BitmapDecoder**。 建立新的 [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) 供編碼器寫入，然後呼叫 [**BitmapEncoder.CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/br226214)，並傳入記憶體內部資料流和解碼器物件。 設定您想要的編碼屬性。 輸入影像檔中未明確設定於編碼器的任何屬性，將會原封不動寫入輸出檔。 呼叫 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br226216)，使編碼器將記憶體內部資料流編碼。 最後，移至檔案資料流和記憶體內部資料流的開頭，並呼叫 [**CopyAsync**](https://msdn.microsoft.com/library/windows/apps/hh701827) 將記憶體內部資料流寫出至檔案資料流。

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>相關主題

* [BitmapEncoder 選項參考](bitmapencoder-options-reference.md)
* [影像中繼資料](image-metadata.md)
 

 




