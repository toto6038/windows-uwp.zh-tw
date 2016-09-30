---
author: mcleanbyron
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: "了解如何在您的 app 中抓取 AdControl 錯誤。"
title: "JavaScript 錯誤處理的逐步解說"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: d26a8efeb253c6c793d8edd21d7452bbf15da261


---

# JavaScript 錯誤處理的逐步解說


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

了解如何在您的 app 中抓取 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 錯誤。

這些範例假設您有包含 **AdControl** 的 JavaScript/HTML app。 如需示範如何將 **AdControl** 新增到 app 的逐步指示，請參閱 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。 如需示範如何將橫幅廣告新增到 JavaScript/HTML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

1.  在 default.html 檔案中，為 **onErrorOccurred** 事件新增一個值，該事件是您為 **AdControl** 定義 **div** 中之 **data-win-options** 的所在位置。 請在 default.html 檔案中尋找以下程式碼。

    ``` syntax
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

    在 **adUnitId** 後方，新增 **onErrorOccurred** 事件的值。

    ``` syntax
    onErrorOccurred: errorLogger
    ```

    以下是 **div** 的完整程式碼。

    ``` syntax
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270', onErrorOccurred: errorLogger}">
    </div>
    ```

2.  建立將顯示文字的 **div**，您就可以看見所產生的訊息。 要這樣做，請在 **div** 後方為 **myAd** 新增以下程式碼。

    ``` syntax
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  建立將觸發錯誤事件的 **AdControl**。 一個 app 中，所有 **AdControl** 物件只能有一個應用程式識別碼。 所以建立另一個具有不同應用程式識別碼的 AdControl 物件，將會在執行階段觸發錯誤。 若要這樣做，請在之前您已經建立的 **div** 區段後方，將以下程式碼新增至 default.html 頁面的內文。

    ``` syntax
    <!-- since only one applicationId can be used, the following ad control will fire an error event -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000',
        adUnitId: '10865270', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  在專案的 default.js 檔案中，您將會在預設的初始化函式之後新增 **errorLogger** 的事件處理常式。 捲動到檔案結尾處，然後在最後一個分號後方放入以下程式碼。

    ``` syntax
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  建置和執行檔案。

您將會看到來自您之前建立之範例 app 的原始廣告，以及該廣告底下描述錯誤的文字。 您不會看到識別碼為 **liveAd** 的廣告。

## 相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


