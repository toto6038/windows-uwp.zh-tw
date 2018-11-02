---
author: drewbatgit
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: 本文說明如何在通用 Windows 平台 (UWP) 應用程式中的 XAML 頁面快速顯示相機預覽串流。
title: 顯示相機預覽
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b258569b707f50051eaa266e2cae5b9971894cf3
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/02/2018
ms.locfileid: "5947113"
---
# <a name="display-the-camera-preview"></a>顯示相機預覽


本文說明如何在通用 Windows 平台 (UWP) 應用程式中的 XAML 頁面快速顯示相機預覽串流。 建立使用相機擷取相片和視訊的 app 會要求您執行如處理裝置和相機方向，或針對擷取之檔案設定編碼選項的工作。 針對某些 app 案例，您可能只想要從相機顯示預覽串流，而不擔心這些其他考量。 本文說明如何使用最少的程式碼執行這些動作。 請注意，您應該一律在完成後，遵循下列步驟正確地關閉預覽串流。

如需撰寫可擷取相片或影片之相機 app 的詳細資訊，請參閱[使用 MediaCapture 的基本相片、影片與音訊擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)。

## <a name="add-capability-declarations-to-the-app-manifest"></a>將功能宣告加入至應用程式資訊清單

為了讓您的 app 可存取裝置的相機，您必須宣告您的 app 使用 *webcam* 和 *microphone* 裝置功能。 

**將功能新增到應用程式資訊清單**

1.  在 Microsoft Visual Studio 中，按兩下 **\[方案總管\]** 中的 **package.appxmanifest** 項目，開啟應用程式資訊清單的設計工具。
2.  選取 **\[功能\]** 索引標籤。
3.  核取 **\[網路攝影機\]** 方塊和 **\[麥克風\]** 方塊。

## <a name="add-a-captureelement-to-your-page"></a>將 CaptureElement 新增到您的頁面

使用 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 在您的 XAML 頁面中顯示預覽串流。

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>使用 MediaCapture 來開始預覽串流


            [
              **MediaCapture**
            ](https://msdn.microsoft.com/library/windows/apps/br241124) 物件是您 app 對裝置相機的介面。 此類別是 Windows.Media.Capture 命名空間的成員。 本文中的範例除了使用包含在預設專案範本中的項目，也會使用 [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 和 [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx) 命名空間的 API。

新增 using 指示詞，在您的頁面的 .cs 檔案中包含下列命名空間。

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

宣告 **MediaCapture** 物件的類別成員變數，以及追蹤相機目前是否正在預覽的布林值。 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

宣告將用來確定顯示器不會在執行預覽時關閉的 [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest) 類型的變數。

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

建立 Helper 方法來啟動相機預覽，在此範例中稱為 **StartPreviewAsync**。 根據您的應用程式案例，您可能會想從載入頁面時呼叫的 **OnNavigatedTo** 事件處理常式呼叫此方法，或是等待並在回應 UI 事件時啟動預覽。

建立 **MediaCapture** 類別的新執行個體，然後呼叫 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 來初始化擷取裝置。 這個方法可能會失敗 (例如在沒有相機的裝置上)，因此您應該從 **try** 區塊中呼叫它。 若使用者在裝置的隱私權設定中停用相機存取，則當您嘗試初始化相機時將會擲回 **UnauthorizedAccessException**。 如果您沒有將適當的功能加入至應用程式資訊清單，您也會在開發期間看到此例外狀況。


            **重要事項** 在某些裝置系列，在授與您的應用程式存取裝置相機的權限之前，會先向使用者顯示使用者同意提示。 基於此因素，您必須只能從主要的 UI 執行緒呼叫 [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598)。 嘗試從另一個執行緒初始化相機，可能導致初始化失敗。

透過設定 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 屬性將 **MediaCapture** 連接到 **CaptureElement**。 呼叫 [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) 開始預覽。 如果其他的應用程式具有擷取裝置的專屬控制項，則這個方法將擲回**FileLoadException**。 如需在專屬控制項中接聽變更的相關資訊，請參閱下一節。

呼叫 [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestActive) 以確定裝置不會在預覽時進入睡眠。 最後，將 [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences) 屬性設定為 [**Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations) 以防止 UI 與 **CaptureElement** 在使用者變更裝置方向時旋轉。 如需處理裝置方向變更的詳細資訊，請參閱[**使用 MediaCapture 處理裝置方向**](handle-device-orientation-with-mediacapture.md)。  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>處理專屬控制項中的變更
如上節所述，如果其他的應用程式具有擷取裝置的專屬控制項，**StartPreviewAsync**將會擲回**FileLoadException**。 從 Windows 10 版本 1703 開始，您可以註冊[MediaCapture.CaptureDeviceExclusiveControlStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged)事件的處理常式，只要裝置的專屬控制項狀態變更，就會引發這個事件。 在這個事件的處理常式中，檢查[MediaCaptureDeviceExclusiveControlStatusChangedEventArgs.Status](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status)屬性，以查看目前狀態。 如果新的狀態是**SharedReadOnlyAvailable**，您就會知道目前無法啟動預覽，而且您可能會想要更新 UI 以提醒使用者。 如果新的狀態是**ExclusiveControlAvailable**，則可以嘗試重新啟動相機預覽。

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>關閉預覽串流

當您完成使用預覽串流時，您應該一律關閉串流，並妥善處置相關資源，以確保裝置上的相機可以供其他 app 使用。 關閉預覽串流的所需的步驟是：

-   如果相機目前正在預覽，請呼叫 [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) 停止預覽資料流。 如果您在未執行預覽時呼叫 **StopPreviewAsync**，將會擲回例外狀況。
-   將 **CaptureElement** 的 [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280) 屬性設為 Null。 使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) 以確保此呼叫會在 UI 執行緒上執行。
-   呼叫 **MediaCapture** 物件的 [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) 方法以釋放物件。 再次使用 [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) 以確保此呼叫會在 UI 執行緒上執行。
-   將 **MediaCapture** 成員變數設為 Null。
-   呼叫 [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestRelease) 以允許螢幕在非使用中時關閉。

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

當使用者離開您的頁面時，您應該藉由覆寫 [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507) 方法關閉預覽串流。

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

當 app 暫停時，您也應該正確地關閉預覽串流。 若要這樣做，請在您頁面的建構函式中登錄 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件的處理常式。

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

在 **Suspending** 事件處理常式中，藉由比較頁面類型與 [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390) 屬性，可先檢查以確保頁面已顯示 app 的 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)。 如果目前沒有顯示頁面，則應該已經引發 **OnNavigatedFrom** 事件，並關閉預覽串流。 如果目前正在顯示頁面，則從傳入處理常式的事件引數中取得 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 物件，確保系統不會在預覽串流關閉之前暫停您的 app。 關閉串流之後，呼叫延遲的 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法，讓系統繼續暫停您的 app。

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>相關主題

* [相機](camera.md)
* [使用 MediaCapture 進行基本相片、視訊和音訊的擷取](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [取得預覽畫面](get-a-preview-frame.md)
