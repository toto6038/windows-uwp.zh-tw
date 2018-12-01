---
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: 了解如何在您的 app 中抓取 AdControl 錯誤。
title: JavaScript 錯誤處理的逐步解說
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, 錯誤處理, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 68f49cd97e8b4e2ef5e20502909a7dc8cb4ab676
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338525"
---
# <a name="error-handling-in-javascript-walkthrough"></a>JavaScript 錯誤處理的逐步解說

此逐步解說示範如何在您的 JavaScript 應用程式中抓取廣告相關錯誤。 此逐步解說示範使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 來顯示橫幅廣告，但它的一般概念也適用於插播式廣告和原生廣告。

這些範例假設您有包含 **AdControl** 的 JavaScript 應用程式。 如需示範如何將 **AdControl** 新增到應用程式的逐步指示，請參閱 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。 如需示範如何將橫幅廣告新增到 JavaScript/HTML app 的完整範例專案，請參閱 [GitHub 上的廣告範例](http://aka.ms/githubads)。

1.  在 default.html 檔案中，為 **onErrorOccurred** 事件新增一個值，該事件是您為 **AdControl** 定義 **div** 中之 **data-win-options** 的所在位置。 請在 default.html 檔案中尋找以下程式碼。
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    在 **adUnitId** 屬性後方，新增 **onErrorOccurred** 事件的值。
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  建立將顯示文字的 **div**，您就可以看見所產生的訊息。 要這樣做，請在 **div** 後方為 **myAd** 新增以下程式碼。
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  建立將觸發錯誤事件的 **AdControl**。 一個 app 中，所有 **AdControl** 物件只能有一個應用程式識別碼。 所以建立另一個具有不同應用程式識別碼的 AdControl 物件，將會在執行階段觸發錯誤。 若要這樣做，請在之前您已經建立的 **div** 區段後方，將以下程式碼新增至 default.html 頁面的內文。
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  在專案的 default.js 檔案中，您將會在預設的初始化函式之後新增 **errorLogger** 的事件處理常式。 捲動到檔案結尾處，然後在最後一個分號後方放入以下程式碼。
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  建置和執行檔案。 您將會看到來自您之前建立之範例 app 的原始廣告，以及該廣告底下描述錯誤的文字。 您不會看到識別碼為 **liveAd** 的廣告。

## <a name="related-topics"></a>相關主題

* [GitHub 上的廣告範例](http://aka.ms/githubads)
