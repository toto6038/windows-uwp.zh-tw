---
description: 本文說明如何在通用 Windows 平台 (UWP) 應用程式中支援分享協定。
title: 分享資料
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 63e07e56acc8767e4acbad3688c13d75527e9c6f
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6985847"
---
# <a name="share-data"></a>分享資料


本文說明如何在通用 Windows 平台 (UWP) 應用程式中支援分享協定。 分享協定是一種在 app 之間快速分享資料 (例如文字、連結，照片和影片) 的簡單方法。 舉例來說，使用者可能會想要使用社交網路 app 與朋友分享網頁，或是將連結儲存在筆記本 app 以供日後參考。

## <a name="set-up-an-event-handler"></a>設定事件處理常式

新增要在每次使用者叫用分享時呼叫的 [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) 事件處理常式。 這會在使用者點選 app 中的控制項 (例如按鈕或是應用程式列命令) 時發生，或是在特定的情況下 (例如使用者完成了關卡並取得高分) 自動發生。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

當 [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) 事件發生時，您的 App 會收到 [**DataRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest) 物件。 這個物件包含一個 [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage)，可以用來提供使用者想分享的內容。 您必須提供標題和資料才能分享。 描述是選擇性的，但建議使用。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## <a name="choose-data"></a>選擇資料

您可以分享各種類型的資料，包括：

-   純文字
-   統一資源識別元 (URI)
-   HTML
-   格式化文字
-   點陣圖
-   檔案
-   自訂開發人員定義資料

[**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 物件可以包含其中的一或多種格式，任何組合皆可。 下列範例示範分享文字。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## <a name="set-properties"></a>設定屬性

當您封裝資料進行分享時，可以提供各種屬性，為目前分享的內容提供更多資訊。 這些屬性可協助目標 app 提升使用者體驗。 例如，description 屬性可在使用者利用多個 app 分享內容時提供協助。 分享影像或網頁連結時，如果加上縮圖，就可以提供使用者視覺上的參考。 如需詳細資訊，請參閱 [**DataPackagePropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)。

除了 title 之外所有的屬性都是選擇性的。 title 屬性是必須設定的強制性屬性。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## <a name="launch-the-share-ui"></a>啟動分享 UI

分享的 UI 是由系統所提供。 若要啟動它，請呼叫 [**ShowShareUI**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.ShowShareUI) 方法。

[!code-cs[Main](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## <a name="handle-errors"></a>處理錯誤

在大部分情況下，內容的分享過程其實不複雜。 但是，還是有可能發生意外的狀況。 例如，App 要求使用者選取要分享的內容，但使用者沒有選取任何內容。 為了處理這些情況，請使用 [**FailWithDisplayText**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest.FailWithDisplayText(System.String)) 方法在發生某些錯誤時向使用者顯示訊息。

## <a name="delay-share-with-delegates"></a>使用委派延遲分享

有時候，立即準備使用者想要分享的資料並沒有什麼意義。 例如，如果 app 支援以幾種不同的可能格式傳送大型影像檔案，在使用者做出選擇之前建立所有的影像是沒有效率的。

若要解決這個問題，[**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) 可以包含委派，這是接收 app 要求資料時所呼叫的函式。 當使用者要分享的資料會耗用大量資源時，建議您使用委派。

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>另請參閱 

* [App 間通訊](index.md)
* [接收資料](receive-data.md)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackagePropertySet](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx)
* [DataRequested](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
 

