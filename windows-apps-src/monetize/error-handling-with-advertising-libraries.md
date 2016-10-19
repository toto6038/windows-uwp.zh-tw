---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: "了解如何處理 Microsoft Advertising 程式庫中 AdControl 類別所產生的錯誤。"
title: "Microsoft Advertising 程式庫的錯誤處理"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: dedac33d86f50b63de300f78a9f9961efc1c016b

---

# Microsoft Advertising 程式庫的錯誤處理




本主題提供如何處理 Microsoft Advertising 程式庫中 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 類別所產生之錯誤的相關基本資訊。

<span id="bkmk-javascript"/>
## JavaScript/HTML app

處理 JavaScript 中 **AdControl** 的錯誤：

1.  指派 **onErrorOccurred** 事件給事件處理常式。

2.  為事件處理常式撰寫程式碼。

**onErrorOccurred** 事件處理常式是在 **AdControl** 之 **div** 的 **data-win-options** 屬性中設定。 在以下範例中， **onErrorOccurred** 事件是設定成由名為 **errorLogger** 的函式處理。

``` syntax
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: 'ADPT33', onErrorOccurred: errorLogger}">
</div>
```

錯誤處理函式是宣告式的，並且必須使用 [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函式括住。

錯誤處理常式會在 **AdControl** 錯誤發生時抓取 JavaScript 錯誤物件。 錯誤物件會提供兩個引數給錯誤處理常式。 如需詳細資訊，請參閱[非同步 Windows 執行階段方法中的特殊錯誤屬性](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx)。

以下是處理 **onErrorOccurred** 事件且名為 **errorLogger** 之錯誤處理函式的範例。

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage + " error code: " + evt.errorCode + \n");
});
```

請參閱 [JavaScript 錯誤處理的逐步解說](error-handling-in-javascript-walkthrough.md)以取得示範 JavaScript 中 **AdControl** 錯誤處理的逐步解說。

<span id="bkmk-dotnet"/>
## XAML app

處理 XAML app 中的 **AdControl** 錯誤：

* 指派 **ErrorOccurred** 事件給事件處理常式委派的名稱。

* 為事件處理常式委派撰寫程式碼，以使其使用兩個參數：一個適用於寄件者的 **Object** 和一個 **AdErrorEventArgs** 物件。

以下是將名為 **OnAdError** 的委派指派給 **ErrorOccurred** 事件的範例。

``` syntax
this.ErrorOccurred = OnAdError;
```

以下是在 Visual Studio 中撰寫錯誤資訊給輸出視窗之 **OnAdError** 委派的範例定義。

``` syntax
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
}
```

請參閱 [XAML/C# 錯誤處理的逐步解說](error-handling-in-xamlc-walkthrough.md)以取得 XAML 和 C# 中示範 **AdControl** 錯誤處理的逐步解說。

 

 



<!--HONumber=Aug16_HO3-->


