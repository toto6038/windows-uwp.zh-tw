---
author: Jwmsft
Description: A web view control embeds a view into your app that renders web content using the Microsoft Edge rendering engine. Hyperlinks can also appear and function in a web view control.
title: 網頁檢視
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: dfe7495e08bfcecac839b0ae15d2d65b00311298
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "7100812"
---
# <a name="web-view"></a>網頁檢視
 

網頁檢視控制項會將檢視嵌入您的應用程式中，而應用程式使用 Microsoft Edge 轉譯引擎來轉譯網頁內容 。 超連結也可以在網頁檢視控制項中顯示和運作。

> **重要 API**：[WebView 類別](https://msdn.microsoft.com/library/windows/apps/br227702)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用網頁檢視控制項，可在您的應用程式套件中顯示來自遠端網頁伺服器的豐富格式 HTML 內容、動態產生的程式碼或內容檔案。 豐富內容也能夠包含指令碼程式碼，並在指令碼與應用程式的程式碼之間通訊。

## <a name="create-a-web-view"></a>建立網頁檢視

**修改網頁檢視的外觀**

[WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 不是 [Control](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx) 子類別，因此它沒有控制項範本。 不過，您可以設定各種屬性來控制網頁檢視的一些視覺外觀。
- 若要限制顯示區域，請設定 [Width](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) 和 [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) 屬性。 
- 若要轉譯、延展、扭曲和旋轉網頁檢視，請使用 [RenderTransform](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.rendertransform.aspx) 屬性。
- 若要控制網頁檢視的不透明度，請設定 [Opacity](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.opacity.aspx) 屬性。
- 若要指定在 HTML 內容未指定色彩時當成網頁背景使用的色彩，請設定 [DefaultBackgroundColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultbackgroundcolor.aspx) 屬性。 

**取得網頁標題**

您可以使用 [DocumentTitle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.documenttitle.aspx) 屬性來取得目前顯示在網頁檢視中的 HTML 文件標題。 

**輸入事件和 Tab 順序**

雖然 WebView 不是 Control 子類別，但是會接收鍵盤輸入焦點，並參與 Tab 順序。 它提供 [Focus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.focus.aspx) 方法，以及 [GotFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.gotfocus.aspx) 和 [LostFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.lostfocus.aspx) 事件，但是它沒有 Tab 相關的屬性。 它在 Tab 順序中的位置，與在 XAML 文件順序中的位置一樣。 Tab 順序包括網頁檢視內容中可接收輸入焦點的所有元素。 

如 [WebView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.aspx) 類別頁面的 Events 表格中所指出，網頁檢視不支援繼承自[UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx) 的大部分使用者輸入事件 (例如 [KeyDown](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keydown.aspx)、[KeyUp](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.keyup.aspx) 和 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx))。 相反地，您可以搭配使用 [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) 與 JavaScript **eval** 函式來使用 HTML 事件處理常式，以及使用 HTML 事件處理常式中的 **window.external.notify** 來通知使用 [WebView.ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) 的應用程式。

### <a name="navigating-to-content"></a>瀏覽至內容

網頁檢視提供數種 API 來進行基本瀏覽：[GoBack](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goback.aspx)、[GoForward](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.goforward.aspx)、[Stop](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.stop.aspx)、[Refresh](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.refresh.aspx)、[CanGoBack](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoback.aspx) 和 [CanGoForward](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cangoforward.aspx)。 您可以使用這些元素為您的應用程式新增典型的網頁瀏覽功能。 

若要設定網頁檢視的初始內容，請在 XAML 中設定 [Source](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.source.aspx) 屬性。 XAML 剖析器會自動將字串轉換為 [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.uri.aspx)。 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

Source 屬性可以設定在程式碼中，但是，如果不這麼做，您通常會使用其中一種 **Navigate** 方法來載入程式碼中的內容。 

若要載入網頁內容，請搭配使用 [Navigate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigate.aspx) 方法與使用 http 或 https 配置的 **Uri**。 

```csharp
webView1.Navigate("http://www.contoso.com");
```

若要瀏覽至具有 POST 要求和 HTTP 標頭的 URI，請使用 [NavigateWithHttpRequestMessage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage.aspx) 方法。 此方法僅支援 [HttpRequestMessage.Method](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httprequestmessage.method.aspx) 屬性值的 [HttpMethod.Post](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.post.aspx) 和 [HttpMethod.Get](https://msdn.microsoft.com/library/windows/apps/xaml/windows.web.http.httpmethod.get.aspx)。 

若要從您應用程式的 [LocalFolder]() 或 [TemporaryFolder]() 資料存放區載入未壓縮和未加密內容，請搭配使用 **Navigate** 方法與使用 [ms-appdata 配置]()的 **Uri**。 這個配置的網頁檢視支援需要您將您的內容放入本機或暫存資料夾的子資料夾中。 這樣可瀏覽至 URI (例如 ms-appdata:///local/*folder*/*file*.html 和ms-appdata:///temp/*folder*/*file*.html)。 (若要載入壓縮或加密檔案，請參閱 [NavigateToLocalStreamUri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx))。 

所有這些第一層子資料夾都是與其他第一層子資料夾中的內容隔離。 例如，您可以瀏覽至 ms-appdata:///temp/folder1/file.html，但此檔案中不能有 ms-appdata:///temp/folder2/file.html 的連結。 不過，您仍然可以使用 **ms-appx-web 配置**來連結至應用程式套件中的 HTML 內容，以及使用 **http** 和 **https** URI 配置來連結至 Web 內容。

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

若要從 app 套件中載入內容，請搭配使用 **Navigate** 方法與使用 [ms-appx-web 配置](https://msdn.microsoft.com/library/windows/apps/xaml/jj655406.aspx#ms_appx_web)的 **Uri**。 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

您可以使用 [NavigateToLocalStreamUri](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetolocalstreamuri.aspx) 方法，透過自訂解析器載入本機內容。 這適用於更進階的案例，例如，下載和快取 Web 內容以進行離線使用，或是解壓縮壓縮檔中的內容。

### <a name="responding-to-navigation-events"></a>回應瀏覽事件

網頁檢視控制項提供數個事件，以用來回應瀏覽和內容載入狀態。 根網頁檢視內容的事件發生順序如下：[NavigationStarting](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationstarting.aspx)、[ContentLoading](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.contentloading.aspx)、[DOMContentLoaded](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.domcontentloaded.aspx)、[NavigationCompleted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigationcompleted.aspx)。


**NavigationStarting** - 發生在網頁檢視瀏覽至新的內容之前。 將 WebViewNavigationStartingEventArgs.Cancel 屬性設定為 true，即可取消此事件之處理常式中的瀏覽。 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** - 發生在網頁檢視開始載入新的內容時。 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** - 發生在網頁檢視完成剖析目前 HTML 內容時。 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** - 發生在網頁檢視完成載入目前內容或瀏覽失敗時。 若要判斷瀏覽是否失敗，請檢查 [WebViewNavigationCompletedEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.aspx) 類別的 [IsSuccess](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess.aspx) 和 [WebErrorStatus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus.aspx) 屬性。 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

針對網頁檢視內容中的每個 **iframe**，類似事件的發生順序都相同︰ 
- [FrameNavigationStarting](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationstarting.aspx) - 發生在網頁檢視中的框架瀏覽至新的內容之前。 
- [FrameContentLoading](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framecontentloading.aspx) - 發生在網頁檢視中的框架開始載入新的內容時。 
- [FrameDOMContentLoaded](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framedomcontentloaded.aspx) - 發生在網頁檢視中的框架完成剖析其目前 HTML 內容時。 
- [FrameNavigationCompleted](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.framenavigationcompleted.aspx) - 發生在網頁檢視中的框架完成載入其內容時。 

### <a name="responding-to-potential-problems"></a>回應潛在問題

您可以回應內容的潛在問題，例如長時間執行的指令碼、網頁檢視無法載入的內容，以及不安全內容的警告。 

指令碼正在執行時，您的應用程式看起來會像沒有回應。 如果網頁檢視執行 JavaScript 並且可以中斷指令碼，則 [LongRunningScriptDetected](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.longrunningscriptdetected.aspx) 事件會定期發生。 若要判斷指令碼已執行多久的時間，請檢查 [WebViewLongRunningScriptDetectedEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.aspx) 的 [ExecutionTime](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime.aspx) 屬性。 若要停止指令碼，請將事件引數 [StopPageScriptExecution](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution.aspx) 屬性設定為 **true**。 除非在後續網頁檢視瀏覽期間重新載入停止的指令碼，否則不會再次執行停止的指令碼。 

網頁檢視控制項無法裝載任意的檔案類型。 嘗試載入網頁檢視無法裝載的內容時，會發生 [UnviewableContentIdentified](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unviewablecontentidentified.aspx) 事件。 您可以處理此事件並通知使用者，或使用 [Launcher](https://msdn.microsoft.com/library/windows/apps/xaml/windows.system.launcher.aspx) 類別將檔案重新導向至外部瀏覽器或另一個應用程式。

同樣地，在網頁內容中叫用不支援的 URI 配置 (例如 fbconnect:// 或 mailto://) 時，會發生 [UnsupportedUriSchemeIdentified](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsupportedurischemeidentified.aspx) 事件。 您可以處理這個事件來提供自訂行為，而不是允許預設系統啟動程式啟動 URI。

網頁檢視針對 SmartScreen 篩選工具回報為不安全的內容顯示警告頁面時，會發生 [UnsafeContentWarningDisplayingevent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying.aspx)。 如果使用者選擇繼續瀏覽，則後續的頁面瀏覽不會顯示警告，也不會引發事件。

### <a name="handling-special-cases-for-web-view-content"></a>處理網頁檢視內容的特殊情況

您可以使用 [ContainsFullScreenElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelement.aspx) 屬性和 [ContainsFullScreenElementChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.containsfullscreenelementchanged.aspx) 事件，來偵測、回應與啟用網頁內容中的全螢幕體驗 (例如全螢幕影片播放)。 例如，您可能使用 ContainsFullScreenElementChanged 事件來調整網頁檢視，使其佔用整個應用程式檢視，或者，如下列範例所示，在想要具有全螢幕網頁體驗時，讓視窗型應用程式進入全螢幕模式。

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

您可以使用 [NewWindowRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.newwindowrequested.aspx) 事件來處理下列情況：託管的網頁內容要求顯示新視窗 (例如快顯視窗) 時。 您可以使用另一個 WebView 控制項來顯示所要求視窗的內容。

使用 [PermissionRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx) 事件，以啟用需要特殊功能的 Web 功能。 這些目前包含地理位置、IndexedDB 存放區，以及使用者音訊和視訊 (例如，從麥克風或網路攝影機)。 如果您的應用程式存取使用者位置或使用者媒體，則仍需要在應用程式資訊清單中宣告此功能。 例如，在 Package.appxmanifest 中，使用地理位置的應用程式最少需要下列功能宣告：

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

除了處理 [PermissionRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.permissionrequested.aspx) 事件的應用程式之外，使用者還必須核准要求位置或媒體功能之應用程式的標準系統對話方塊，以啟用這些功能。

以下範例示範應用程式如何從 Bing 啟用地圖中的地理位置︰

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

如果您的應用程式需要使用者輸入或其他非同步作業來回應權限要求，請使用 [WebViewPermissionRequest](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx) 的 [Defer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.aspx) 方法來建立可稍後處理的 [WebViewDeferredPermissionRequest](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewdeferredpermissionrequest.aspx)。 請參閱 [WebViewPermissionRequest.Defer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewpermissionrequest.defer.aspx)。 

如果使用者必須安全地登出網頁檢視中所裝載的網站，或是其他安全性十分重要的情況，請透過網頁檢視工作階段呼叫靜態方法 [ClearTemporaryWebDataAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.cleartemporarywebdataasync.aspx) 來清除所有本機快取的內容。 這可以防止惡意使用者存取敏感性資料。 

### <a name="interacting-with-web-view-content"></a>與網頁檢視內容互動

使用 [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) 方法叫用指令碼或將其插入網頁檢視內容，以及使用 [ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) 事件從網頁檢視內容取回資訊，即可與網頁檢視內容互動。

若要在網頁檢視內容內叫用 JavaScript，請使用 [InvokeScriptAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.invokescriptasync.aspx) 方法。 叫用的指令碼可以只傳回字串值。 

例如，如果 `webView1` 網頁檢視的內容包含使用 3 個參數的 `setDate` 函式，則可以像這樣進行叫用。 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


您可以搭配使用 **InvokeScriptAsync** 與 JavaScript **eval** 函式，以將內容插入網頁中。

在這裡，XAML 文字方塊的文字 (`nameTextBox.Text`) 會寫入 `webView1` 所裝載 HTML 頁面中的 div。 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

網頁檢視內容中的指令碼可以搭配使用 **window.external.notify** 與 string 參數，以將資訊送回應用程式。 若要接收這些訊息，請處理 [ScriptNotify](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.scriptnotify.aspx) 事件。 

若要在呼叫 window.external.notify 時讓外部網頁引發 **ScriptNotify** 事件，您必須在應用程式資訊清單的 **ApplicationContentUriRules** 區段中包含頁面的 URI (您可以在 Microsoft Visual Studio 之 Package.appxmanifest 設計工具的 [內容 URI] 索引標籤上這麼做)。這個清單中的 URI 必須使用 HTTPS，而且可以包含子網域萬用字元 (例如，`https://*.microsoft.com`)，但是不可以包含網域萬用字元 (例如，`https://*.com` 和 `https://*.*`)。 資訊清單需求不適用於源自 app 套件的內容、使用 ms-local-stream:// URI 的內容，或使用 [NavigateToString](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.navigatetostring.aspx) 載入的內容。 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>在網頁檢視中存取 Windows 執行階段

您可以使用 [AddWebAllowedObject](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx) 方法，將原生類別的執行個體從 Windows 執行階段元件插入網頁檢視的 JavaScript 內容。 這樣可在該網頁檢視的 JavaScript 內容中完整存取該物件的原生方法、屬性和事件。 類別必須使用 [AllowForWeb](https://msdn.microsoft.com/library/windows/apps/xaml/windows.foundation.metadata.allowforwebattribute.aspx) 屬性進行裝飾。 

例如，這個程式碼會插入從 Windows 執行階段元件匯入至網頁檢視的 `MyClass` 執行個體。

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

如需詳細資訊，請參閱 [WebView.AddWebAllowedObject](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.addweballowedobject.aspx)。 

此外，還可以允許網頁檢視中的受信任 JavaScript 內容直接存取 Windows 執行階段 API。 這會提供網頁檢視中所裝載 Web 應用程式的功能強大原生功能。 若要啟用這項功能，在 Package.appxmanifest 中應用程式的 ApplicationContentUriRules 中，必須將受信任內容的 URI 設為允許清單，而且 WindowsRuntimeAccess 特別設定為 "all"。 

這個範例示範應用程式資訊清單的一個區段。 在這裡，本機 URI 具有 Windows 執行階段的存取權。 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>網頁內容裝載的選項

您可以使用 [WebView.Settings](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.settings.aspx) 屬性 (類型為 [WebViewSettings](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewsettings.aspx)) 來控制是否啟用 JavaScript 和 IndexedDB。 例如，如果您使用網頁檢視來顯示完全靜態內容，則可能要停用 JavaScript 以獲得最佳效能。

### <a name="capturing-web-view-content"></a>擷取網頁檢視內容

若要與其他 app 共用網頁檢視內容，請使用 [CaptureSelectedContentToDataPackageAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync.aspx) 方法，這會將選取的內容傳回為 [DataPackage](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datapackage.aspx)。 這是非同步方法，因此您必須使用延遲，防止 [DataRequested](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx) 事件處理常式在非同步呼叫完成之前返回。 

若要取得目前網頁檢視內容的預覽影像，請使用 [CapturePreviewToStreamAsync](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.capturepreviewtostreamasync.aspx) 方法。 這個方法會建立目前內容的映像，並將它寫入指定的資料流中。 

### <a name="threading-behavior"></a>執行緒行為

網頁檢視內容預設會裝載於傳統型裝置系列之裝置的 UI 執行緒上，並從所有其他裝置上的 UI 執行緒予以卸載。 您可以使用 [WebView.DefaultExecutionMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webview.defaultexecutionmode.aspx) 靜態屬性，來查詢目前用戶端的預設執行緒行為。 必要時，您可以使用 [WebView(WebViewExecutionMode)](https://msdn.microsoft.com/library/windows/apps/xaml/dn932036.aspx) 建構函式來覆寫這個行為。 

> **注意**&nbsp;&nbsp;將 UI 執行緒上的內容裝載到行動裝置時可能會發生效能問題，因此，請務必在變更 DefaultExecutionMode 時在所有目標裝置上進行測試。

從 UI 執行緒卸載內容的網頁檢視與父控制項不相容，而父控制項需要手勢才能從網頁檢視控制項散佈到父項 (例如 [FlipView](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.flipview.aspx)、[ScrollViewer](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) 和其他相關控制項)。 這些控制項將無法接收關閉執行緒網頁檢視中所起始的手勢。 此外，不直接支援列印關閉執行緒網頁內容；您應該改為使用 [WebViewBrush](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.webviewbrush.aspx) 填滿來列印元素。

## <a name="recommendations"></a>建議


-   確定所載入的網站在裝置上格式正確，以及使用的色彩、印刷格式及瀏覽方式，都和應用程式的其他部分一致。
-   應該適當地調整輸入欄位的大小。 使用者可能不知道他們可以放大來輸入文字。
-   如果網頁檢視在外觀上與應用程式的其他部分不像，請考慮採用替代控制項或方式來完成相關工作。 如果網頁檢視符合應用程式的其他部分，使用者會將其與其他部分視為一個整體，而獲得無縫式經驗。



## <a name="related-topics"></a>相關主題

* [WebView 類別](https://msdn.microsoft.com/library/windows/apps/br227702)
 

 




