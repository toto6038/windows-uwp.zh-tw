---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: "此文章討論如何使用相機設定檔來探索和管理不同視訊擷取裝置的功能。 這其中包括如下的工作：選取支援特定解析度或畫面播放速率的設定檔、選取支援可同時存取多台相機的設定檔，以及選取支援 HDR 的設定檔。"
title: "使用相機設定檔探索並選取相機功能"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: f45fea396c775a7d9e783be1d0a821ff68716279
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>使用相機設定檔探索並選取相機功能

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


此文章討論如何使用相機設定檔來探索和管理不同視訊擷取裝置的功能。 這其中包括如下的工作：選取支援特定解析度或畫面播放速率的設定檔、選取支援可同時存取多台相機的設定檔，以及選取支援 HDR 的設定檔。

> [!NOTE] 
> 本文是以[使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)中討論的概念和程式碼為基礎，其中說明實作基本相片和視訊擷取的步驟。 建議您先熟悉該文中的基本媒體擷取模式，然後再移到更多進階的擷取案例。 本文章中的程式碼假設您的 app 已有正確初始化的 MediaCapture 執行個體。

 

## <a name="about-camera-profiles"></a>關於相機設定檔

不同裝置上的相機支援不同的功能，包括支援的擷取解析度、視訊擷取的畫面播放速率，以及是否支援 HDR 或可變畫面播放速率擷取。 通用 Windows 平台 (UWP) 媒體擷取架構將這組功能存放在 [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) 中。 以 [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694) 物件表示的相機設定檔有三個媒體描述集合；一個用於相片擷取、一個用於視訊擷取，另一個用於視訊預覽。

在初始化 [MediaCapture](capture-photos-and-video-with-mediacapture.md) 物件之前，您可以在目前裝置上查詢擷取裝置，以查看支援哪些設定檔。 當您選取支援的設定檔時，您知道此擷取裝置支援設定檔的媒體描述中的所有功能。 這樣就不需要試用與錯誤方法來判斷特定裝置上支援哪些功能組合。

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

本文中的程式碼範例透過探索支援各種功能的相機設定檔來取代此最小初始化，然後會使用這些相機設定檔來初始化媒體擷取裝置。

## <a name="find-a-video-device-that-supports-camera-profiles"></a>尋找可支援相機設定檔的視訊裝置

搜尋支援的相機設定檔之前，您應該尋找可支援使用相機設定檔的擷取裝置。 以下範例中定義的 **GetVideoProfileSupportedDeviceIdAsync** 協助程式方法使用 [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 方法，擷取所有可用的視訊擷取裝置清單。 它會循環顯示清單中的所有裝置，並對每個裝置呼叫靜態方法 ([**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714))，以查看是否支援視訊設定檔。 此外，每個裝置的 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 屬性可讓您指定您希望相機在裝置的正面或背面。

如果在指定的面板上找到支援相機設定檔的裝置，則會傳回包含裝置識別碼字串的 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 值。

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

如果 **GetVideoProfileSupportedDeviceIdAsync** 協助程式方法傳回的裝置識別碼為 null 或空字串，表示指定的面板上沒有任何支援相機設定檔的裝置。 在此情況下，您應該在不使用設定檔的情況下初始化媒體擷取裝置。

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>根據支援的解析度和畫面播放速率選取設定檔

若要選取具有特定功能 (例如有達到特定解析度和畫面播放速率的能力) 的設定檔，您應該先呼叫上面定義的協助程式方法，以取得支援使用相機設定檔的擷取裝置識別碼。

建立新的 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) 物件，並傳入選取的裝置識別碼。 接著，呼叫靜態方法 [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708)，以取得裝置支援的所有相機設定檔清單。

這個範例使用 Linq 查詢方法 (包含在 using**System.Linq** 命名空間中) 來選取包含 [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) 物件的設定檔，其中 [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) 和 [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) 屬性符合要求的值。 如果找到相符的值，則 **MediaCaptureInitializationSettings** 的 [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) 和 [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) 會設定為 Linq 查詢所傳回之匿名類型中的值。 如果找不到相符的值，則會使用預設設定檔。

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

以您所需的相機設定檔填入 **MediaCaptureInitializationSettings** 後，您只需在媒體擷取物件上呼叫 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 來將它設為所需的設定檔即可。

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="select-a-profile-that-supports-concurrence"></a>選取支援並行處理的設定檔

您可以使用相機設定檔來判斷裝置是否支援從多台相機可同時擷取視訊。 在這個案例中，您需要建立兩組擷取物件：一個適用於前置鏡頭，一個適用於後置鏡頭。 針對每台相機，建立 **MediaCapture**、**MediaCaptureInitializationSettings**，以及用來保存擷取裝置識別碼的字串。 此外，新增布林值變數，以追蹤是否支援並行處理。

[!code-cs[ConcurrencySetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConcurrencySetup)]

靜態方法 [**MediaCapture.FindConcurrentProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926709) 會傳回由指定的擷取裝置支援並且支援並行處理的相機設定檔清單。 使用 Linq 查詢來尋找支援並行處理並由前置和後置鏡頭支援的設定檔。 如果找到符合這些需求的設定檔，請在每個 **MediaCaptureInitializationSettings** 物件上設定此設定檔，並將布林值並行處理追蹤變數設定為 true。

[!code-cs[FindConcurrencyDevices](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindConcurrencyDevices)]

針對您 app 案例的主要相機呼叫 **MediaCapture.InitializeAsync**。 如果支援並行處理，則初始化第二台相機。

[!code-cs[InitConcurrentMediaCaptures](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitConcurrentMediaCaptures)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video"></a>使用已知的設定檔來尋找支援 HDR 視訊的設定檔

像其他案例一樣，開始選取支援 HDR 的設定檔。 建立 **MediaCaptureInitializationSettings**以及用來保存擷取裝置識別碼的字串。 新增布林值變數，以追蹤是否支援 HDR 視訊。

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

使用上面定義的 **GetVideoProfileSupportedDeviceIdAsync** 協助程式方法，取得可支援相機設定檔之擷取裝置的裝置識別碼。

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

靜態方法 [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) 會傳回由指定的裝置支援並依指定的 [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) 值分類的相機設定檔。 在這個案例中，指定 **VideoRecording** 值可將傳回的相機設定檔限制為支援視訊錄製的設定檔。

循環顯示傳回的相機設定檔清單。 對於每個相機設定檔，在設定檔檢查中循環顯示每個 [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)，以查看 [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) 屬性是否為 true。 找到合適的媒體描述後，請中斷迴圈並將設定檔和描述物件指派給 **MediaCaptureInitializationSettings** 物件。

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>判斷裝置是否支援同時相片和視訊擷取

許多裝置都支援同時擷取相片和視訊。 若要判斷擷取裝置是否支援此功能，請呼叫 [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) 以取得裝置所支援的所有相機設定檔。 使用連結查詢來尋找至少有一個 [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) 項目和一個 [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) 項目的設定檔，這表示此設定檔可支援同時擷取。

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

您可以修改此查詢，以尋找支援特定解析度或其他功能 (同時視訊錄製除外) 的設定檔。 您也可以使用 [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) 並指定 [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) 值來擷取支援同時擷取的設定檔，但查詢所有設定檔會提供更完整的結果。

## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




