---
author: Xansky
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: 閱讀有關在 JavaScript/HTML 應用程式中使用 Microsoft Advertising 程式庫開發之常見問題的解決方案。
title: HTML 和 JavaScript 疑難排解指南
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: Windows 10, UWP, 廣告, 廣告, AdControl, 疑難排解, HTML, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 38f0b36769d13d119965e7d15c5812b9ba1d6ecd
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "6645779"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>HTML 和 JavaScript 疑難排解指南

本主題包含在 JavaScript/HTML 應用程式中使用 Microsoft Advertising 程式庫開發之常見問題的解決方案。

* [HTML](#html)
  * [沒有顯示 AdControl](#html-notappearing)
  * [黑色方塊閃爍然後消失](#html-blackboxblinksdisappears)
  * [廣告沒有重新整理](#html-adsnotrefreshing)

* [JavaScript](#js)
  * [沒有顯示 AdControl](#js-adcontrolnotappearing)
  * [黑色方塊閃爍然後消失](#js-blackboxblinksdisappears)
  * [廣告沒有重新整理](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>沒有顯示 AdControl

1.  確定已在 Package.appxmanifest 中選取 **\[網際網路 (用戶端)\]** 功能。

2.  確定 JavaScript 參考存在。 若 &lt;head&gt; 區段中沒有 ad.js 參考，**AdControl** 將無法顯示，且建置期間會發生錯誤。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  檢查應用程式識別碼和廣告單位識別碼。 這些識別碼必須符合應用程式識別碼和廣告單位識別碼，您在合作夥伴中心中取得。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  檢查 **height** 和 **width** 屬性。 這兩個屬性必須設定為其中一個[橫幅廣告支援的廣告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  檢查元素的位置。 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 必須在可檢視區域內。

6.  檢查 **visibility** 屬性。 這個屬性不得設為 collapsed 或 hidden。 可以在行內 (如下所示) 或外部樣式表中設定這個屬性。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  檢查 **position** 屬性。 position 屬性必須根據該元素的其他屬性 (例如，父元素的 margin 以及 z-index) 設定為適當的值。 可以在行內 (如下所示) 或外部樣式表中設定這個屬性。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  檢查 **z-index** 屬性。 **z-index** 屬性必須設為足夠高，使 **AdControl** 一律會顯示在其他元素之上。 可以在行內 (如下所示) 或外部樣式表中設定這個屬性。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  檢查外部樣式表。 如果屬性是透過外部樣式表來設定在 **AdControl** 元素上，請確認上述的所有屬性都設定正確。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. 檢查 **AdControl** 的父項。 如果 **AdControl** 位於父元素中，則父元素的狀態必須是使用中且可見。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. 確定 **AdControl** 沒有在檢視區隱藏。 **AdControl** 必須可見，廣告才能正確顯示。

12. 不應在模擬器中測試 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 和 [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 的實際值。 若要確保 **AdControl** 如預期般運作，請對 **ApplicationId** 和 **AdUnitId** 都使用[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑色方塊閃爍然後消失

1.  再次檢查前述[沒有顯示 AdControl](#html-notappearing) 一節中的所有步驟。

2.  處理 **onErrorOccurred** 事件，並以傳遞到事件處理常式的訊息來判斷是否發生問題及擲回的問題類型為何。 在 [JavaScript 錯誤處理的逐步解說](error-handling-in-javascript-walkthrough.md)中可以找到更多詳細資訊。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    造成黑色方塊的常見錯誤為「沒有可用的廣告」。 這個錯誤代表要求沒有傳回可用的廣告。

3.  **AdControl** 運作正常。 根據預設，**AdControl** 會在無法顯示廣告時摺疊。 相同父元素中的其他子元素可能會移動，以填滿已摺疊之 **AdControl** 的空位，直到下一次發出要求時才會展開。

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>廣告沒有重新整理

1.  檢查 **isAutoRefreshEnabled** 屬性。 根據預設，這個選用的屬性會設為 true。 當設為 false 時，必須使用 **refresh** 方法來擷取另一個廣告。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  檢查對 **refresh** 方法的呼叫。 當使用自動重新整理時，無法使用 **refresh** 來擷取另一個廣告。 使用手動重新整理時，**refresh** 應於最少 30 秒至 60 秒 (依裝置目前的數據連線而定) 之後才呼叫。

    此範例示範如何使用 **refresh** 方法。 以下範例 HTML 程式碼範例示範當 **isAutoRefreshEnabled** 設為 false 時，如何具現化 **AdControl**。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    此範例示範如何使用 **refresh** 函式。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** 運作正常。 有時候，同樣的廣告可能連續出現超過一次，使之看起像是沒有重新整理。

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>沒有顯示 AdControl

1.  確定已在 Package.appxmanifest 中選取 **\[網際網路 (用戶端)\]** 功能。

2.  確定 **AdControl** 已具現化。 如果 **AdControl** 尚未具現化。 它將無法供使用。

    以下程式碼片段顯示具現化 **AdControl** 的範例。 此 HTML 程式碼顯示設定 **AdControl** 之 UI 的範例。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    以下 JavaScript 程式碼顯示具現化 **AdControl** 的範例。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  檢查父元素。 父項 **&lt;div&gt;** 元素必須正確指派、使用中，並且可見。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  檢查應用程式識別碼和廣告單位識別碼。 這些識別碼必須符合應用程式識別碼和廣告單位識別碼，您在合作夥伴中心中取得。 如需詳細資訊，請參閱[在您的應用程式中設定廣告單元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  檢查 **AdControl** 的父元素。 該父元素必須為使用中且可見。

6.  不應在模擬器中測試 **ApplicationId** 和 **AdUnitId** 的實際值。 若要確保 **AdControl** 如預期般運作，請對 **ApplicationId** 和 **AdUnitId** 都使用[測試值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑色方塊閃爍然後消失

1.  再次檢查[沒有顯示 AdControl](#js-adcontrolnotappearing) 一節中的所有步驟。

2.  處理 **onErrorOccurred** 事件，並以傳遞到事件處理常式的訊息來判斷是否發生問題及擲回的問題類型為何。 在 [JavaScript 錯誤處理的逐步解說](error-handling-in-javascript-walkthrough.md)中可以找到更多詳細資訊。

    此範例示範如何實作報告錯誤訊息的錯誤處理常式。 此 HTML 程式碼片段提供如何設定顯示錯誤訊息之 UI 的範例。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    此範例示範如何具現化 **AdControl**。 此函式會插入 app.onactivated 檔案中。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    此範例示範如何報告錯誤。 此函式會插入到 default.js 檔案中自我執行函式的下方。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    造成黑色方塊的常見錯誤為「沒有可用的廣告」。 這個錯誤代表要求沒有傳回可用的廣告。

3.  **AdControl** 運作正常。 有時候，同樣的廣告可能連續出現超過一次，使之看起像是沒有重新整理。

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>廣告沒有重新整理

1.  檢查 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 屬性 (其屬於您的 **AdControl**) 是否設為 false。 根據預設，這個選用的屬性會設為 **true**。 當設為 **false** 時，必須使用 **Refresh** 方法來擷取另一個廣告。

2.  檢查對 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 方法的呼叫。 使用自動重新整理 (**IsAutoRefreshEnabled** 為 **true**) 時，**Refresh** 無法用來擷取其他廣告。 使用手動重新整理 (**IsAutoRefreshEnabled** 為 **false**) 時，僅應在至少 30 到 60 秒後 (取決於裝置的目前資料連線) 才呼叫 **Refresh**。

    此範例示範如何建立 **AdControl** 的 **div**。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    此範例顯示如何使用 **Refresh** 函式。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** 運作正常。 有時候，同樣的廣告可能連續出現超過一次，使之看起像是沒有重新整理。

 

 
