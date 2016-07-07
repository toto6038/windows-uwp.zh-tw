---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: "本文說明如何在通用 Windows 平台 (UWP) 應用程式中的 XAML 頁面快速顯示相機預覽串流。"
title: "簡單的相機預覽存取"
translationtype: Human Translation
ms.sourcegitcommit: 72abc006de1925c3c06ecd1b78665e72e2ffb816
ms.openlocfilehash: 05e752925c07b0e3720fbdd42d785381aa08b99c

---

# 簡單的相機預覽存取

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本文說明如何在通用 Windows 平台 (UWP) 應用程式中的 XAML 頁面快速顯示相機預覽串流。 建立使用相機擷取相片和視訊的 app 會要求您執行如處理裝置和相機方向，或針對擷取之檔案設定編碼選項的工作。 針對某些 app 案例，您可能只想要從相機顯示預覽串流，而不擔心這些其他考量。 本文說明如何使用最少的程式碼執行這些動作。 請注意，您應該一律在完成後，遵循下列步驟正確地關閉預覽串流。

如需撰寫擷取相片或視訊之相機 app 的詳細資訊，請參閱[使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)。

## 將功能宣告加入至 app 資訊清單

為了讓您的 app 可存取裝置的相機，您必須宣告您的 app 使用 *webcam* 和 *microphone* 裝置功能。 如果您要將拍攝的相片和影片儲存到使用者的圖片媒體櫃或視訊媒體櫃，您也必須宣告 *picturesLibrary* 和 *videosLibrary* 功能。

**將功能新增到 app 資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 \[方案總管\] 中的 package.appxmanifest 項目，開啟 app 資訊清單的設計工具。
2.  選取 \[功能\] 索引標籤。
3.  核取 \[網路攝影機\] 方塊和 \[麥克風\] 方塊。
4.  如果要存取圖片媒體櫃和視訊媒體櫃，請選取 \[圖片媒體櫃\] 方塊和 \[視訊媒體櫃\] 方塊。

## 將 CaptureElement 新增到您的頁面

使用 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 在您的 XAML 頁面中顯示預覽串流。

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]

## 使用 MediaCapture 來開始預覽串流


            [
              **MediaCapture**
            ](https://msdn.microsoft.com/library/windows/apps/br241124) 物件是您 app 對裝置相機的介面。 此類別是 Windows.Media.Capture 命名空間的成員。 本文中的範例除了使用包含在預設專案範本中的項目，也會使用 [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 和 [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx) 命名空間的 API。

新增 using 指示詞，在您的頁面的 .cs 檔案中包含下列命名空間。

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

宣告 **MediaCapture** 物件的類別變數。

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

建立 **MediaCapture** 類別的新執行個體，然後呼叫 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 來初始化擷取裝置。 這個方法可能會失敗 (例如在沒有相機的裝置上)，因此您應該從 **try** 區塊中呼叫它。 若使用者在裝置的隱私權設定中停用相機存取，則當您嘗試初始化相機時將會擲回 **UnauthorizedAccessException**。 如果您沒有將適當的功能加入至應用程式資訊清單，您也會在開發期間看到此例外狀況。


            **重要事項** 在某些裝置系列，在授與您的應用程式存取裝置相機的權限之前，會先向使用者顯示使用者同意提示。 基於此因素，您必須只能從主要的 UI 執行緒呼叫 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)。 嘗試從另一個執行緒初始化相機，可能導致初始化失敗。

透過設定 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 屬性將 **MediaCapture** 連接到 **CaptureElement**。 最後，呼叫 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 開始預覽。

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]


## 關閉預覽串流

當您完成使用預覽串流時，您應該一律關閉串流，並妥善處置相關資源，以確保裝置上的相機可以供其他 app 使用。 關閉預覽串流的所需的步驟是：

-   呼叫 [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) 停止預覽串流。
-   將 **CaptureElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 屬性設為 null。
-   呼叫 **MediaCapture** 物件的 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 方法以釋放物件。
-   將 **MediaCapture** 成員變數設為 null。

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

當使用者離開您的頁面時，您應該藉由覆寫 [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 方法關閉預覽串流。

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

當 app 暫停時，您也應該正確地關閉預覽串流。 若要這樣做，請在您頁面的建構函式中登錄 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件的處理常式。

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

在 **Suspending** 事件處理常式中，藉由比較頁面類型與 [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390) 屬性，可先檢查以確保頁面已顯示 app 的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 如果目前沒有顯示頁面，則應該已經引發 **OnNavigatedFrom** 事件，並關閉預覽串流。 如果目前正在顯示頁面，則從傳入處理常式的事件引數中取得 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 物件，確保系統不會在預覽串流關閉之前暫停您的 app。 關閉串流之後，呼叫延遲的 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法，讓系統繼續暫停您的 app。

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]

## 從預覽串流擷取靜止影像

以 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/dn887358) 格式從媒體擷取預覽串流中取得靜止影像的方式很簡單。 如需詳細資訊，請參閱[取得預覽框架](get-a-preview-frame.md)。

## 相關主題

* [使用 MediaCapture 擷取相片和視訊](capture-photos-and-video-with-mediacapture.md)
* [取得預覽框架](get-a-preview-frame.md)



<!--HONumber=Jun16_HO4-->


