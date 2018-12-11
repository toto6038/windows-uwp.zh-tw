---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: 了解如何處理 Microsoft Advertising 程式庫中 AdControl 類別所產生的錯誤。
title: 處理廣告錯誤
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 錯誤處理, JavaScript, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: a6c14ecf8e8909ab6cd95a54ca8144fbf8a8d912
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8888208"
---
# <a name="handle-ad-errors"></a>處理廣告錯誤

每個 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)、[InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 和 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 類別都有 **ErrorOccurred** 事件，會在發生廣告相關錯誤時引發。 您的應用程式程式碼可以處理這個事件，並檢查事件引數物件的 [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) 和 [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) 屬性以協助判斷錯誤原因。

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML app

處理 XAML app 中的廣告相關錯誤：

1. 指派 **AdControl**、**InterstitialAd** 或 **NativeAdsManagerV2** 物件的 **ErrorOccurred** 事件給事件處理常式委派的名稱。

2. 為事件處理常式委派撰寫程式碼，以使其使用兩個參數：一個適用於寄件者的 **Object** 和一個 [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs) 物件。

Here is an example that assigns a delegate named **OnAdError** to the **ErrorOccurred** event of an **AdControl** object named *myBannerAdControl*.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

以下是在 Visual Studio 中撰寫錯誤資訊給輸出視窗之 **OnAdError** 委派的範例定義。

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

請參閱 [XAML/C# 錯誤處理的逐步解說](error-handling-in-xamlc-walkthrough.md)以取得 XAML 和 C# 中示範 **AdControl** 錯誤處理的逐步解說。

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>JavaScript/HTML app

To handle **ErrorOccur** errors in a JavaScript app:

1.  Assign the **onErrorOccurred** event to an event handler.

2.  為事件處理常式撰寫程式碼。

Here is an example that assigns an event handler named **errorLogger** to the **ErrorOccurred** event of an **AdControl** object.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

錯誤處理函式是宣告式的，並且必須使用 [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函式括住。

錯誤處理常式會在發生錯誤時抓取 JavaScript 錯誤物件。 錯誤物件會提供兩個引數給錯誤處理常式。 如需詳細資訊，請參閱[非同步 Windows 執行階段方法中的特殊錯誤屬性](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx)。

Here is an example of an error handling function named **errorLogger** that handles the **onErrorOccurred** event.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

請參閱 [JavaScript 錯誤處理的逐步解說](error-handling-in-javascript-walkthrough.md)以取得示範 JavaScript 中 **AdControl** 錯誤處理的逐步解說。
