---
description: 使用 Windows.Web.Syndication 命名空間中的功能，以 RSS 和 Atom 標準為依據產生的同步發佈摘要，來抓取或建立最新最熱門的網頁內容。
title: RSS/Atom 摘要
ms.assetid: B196E19B-4610-4EFA-8FDF-AF9B10D78843
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5b5312614c7060118fdb4678aa80ae51d6734486
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458638"
---
# <a name="rssatom-feeds"></a>RSS/Atom 摘要


**重要 API**

-   [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819)
-   [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)
-   [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632)

使用 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空間中的功能，以 RSS 和 Atom 標準為依據產生的同步發佈摘要，來抓取或建立最新最熱門的網頁內容。

## <a name="what-is-a-feed"></a>何謂摘要？

網頁摘要是一種文件，包含由文字、連結和影像所組成的任何數目的個別項目。 對摘要所做的更新是以新項目的形式呈現，用來在網路上推廣最新的內容。 內容取用者可以使用摘要讀取程式應用程式以彙總和監視任何數目的個別內容作者的摘要，以利快速且方便地存取最新的內容。

## <a name="which-feed-format-standards-are-supported"></a>支援哪些摘要格式標準？

通用 Windows 平台 (UWP) 支援從 0.91 到 RSS 2.0 的 RSS 格式標準以及從 0.3 到 1.0 的 Atom 標準的摘要抓取。 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空間中的類別可定義能夠代表 RSS 以及 Atom 元素的摘要和摘要項目。

此外，Atom 1.0 和 RSS 2.0 格式皆允許它們的摘要文件包含官方規格未定義的元素或屬性。 經過一段時間後，這些自訂元素與屬性已經變成一種方法，用以定義由其他 Web 服務資料格式 (如 GData 與 OData) 所取用的網域特定資訊。 為了支援此新增功能，[**SyndicationNode**](https://msdn.microsoft.com/library/windows/apps/br243585) 類別代表一般 XML 元素。 使用 **SyndicationNode** 搭配 [**Windows.Data.Xml.Dom**](https://msdn.microsoft.com/library/windows/apps/br240819) 命名空間中的類別，可允許應用程式存取屬性、延伸以及任何可能包含的內容。

請注意，對於同步發佈的內容，Atom 發佈通訊協定 ([**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609)) 的 UWP 實作僅根據 Atom 與 Atom 發佈標準支援摘要內容作業。

## <a name="using-syndicated-content-with-network-isolation"></a>使用具有網路隔離的同步發佈內容

在 UWP 中的網路隔離功能可讓開發人員控制和限制 UWP 應用程式的網路存取。 並非所有的應用程式都需要存取網路。 不過對於那些需要存取網路的應用程式，UWP 提供對網路不同層級的存取權，這些存取權可透過選取適當的功能來啟用。

網路隔離可讓開發人員為每個應用程式定義所需網路存取權的範圍。 沒有定義適當範圍的應用程式在於防止存取指定類型的網路，以及特定類型的網路要求 (對外用戶端起始的要求，或是對內未經同意的要求以及對外用戶端起始的要求)。 設定和強制網路隔離的功能可確保如果應用程式確實受到威脅，它只能存取已明確授與應用程式存取權的網路。 這將可大幅減少對其他應用程式和 Windows 的影響範圍。

網路隔離會影響 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 和 [**Windows.Web.AtomPub**](https://msdn.microsoft.com/library/windows/apps/br210609) 命名空間中任何想存取網路的類別元素。 Windows 會主動強制網路隔離。 如果未啟用適當的網路功能，則在 **Windows.Web.Syndication** 或 **Windows.Web.AtomPub** 命名空間中呼叫類別元素會因為網路隔離而導致網路存取失敗。

建立應用程式時，會在應用程式資訊清單中設定應用程式的網路功能。 網路功能通常會使用 Microsoft Visual Studio2015，開發應用程式時新增。 也可以使用文字編輯器在應用程式資訊清單檔案中手動設定網路功能。

如需網路隔離和網路功能的詳細資訊，請參閱[網路功能基本知識](networking-basics.md)主題中的＜功能＞一節。

## <a name="how-to-access-a-web-feed"></a>如何存取網頁摘要

本節說明如何在以 C# 或 Javascript 撰寫的 UWP 應用程式中使用 [**Windows.Web.Syndication**](https://msdn.microsoft.com/library/windows/apps/br243632) 命名空間的類別來抓取和顯示網頁摘要。

**先決條件**

若要確保 UWP 應用程式的網路可正常運作，您必須在專案 **Package.appxmanifest** 檔案中設定所需的所有網路功能。 如果應用程式需要以用戶端的形式連線到網際網路上的遠端服務，則需要 **internetClient** 功能。 如需詳細資訊，請參閱[網路功能基本知識](networking-basics.md)主題中的＜功能＞一節。

**從網頁摘要抓取同步發佈內容**

現在我們要檢閱一些示範如何抓取摘要的程式碼，然後顯示摘要所包含的每一個個別項目。 設定和傳送要求之前，我們會先定義一些要在作業期間使用的變數，然後初始化 [**SyndicationClient**](https://msdn.microsoft.com/library/windows/apps/br243456) 的執行個體，這可定義要用來抓取和顯示摘要的方法和屬性。

如果傳遞給建構函式的 *uriString* 不是有效 URI，[**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) 建構函式會擲回例外狀況。 所以我們要使用 try/catch 區塊來驗證 *uriString*。

> [!div class="tabbedCodeSnippets"]
```csharp
Windows.Web.Syndication.SyndicationClient client = new Windows.Web.Syndication.SyndicationClient();
Windows.Web.Syndication.SyndicationFeed feed;
// The URI is validated by catching exceptions thrown by the Uri constructor.
Uri uri = null;
// Use your own uriString for the feed you are connecting to.
string uriString = "";
try
{
    uri = new Uri(uriString);
}
catch (Exception ex)
{
    // Handle the invalid URI here.
}
```
```javascript
var currentFeed = null;
var currentItemIndex = 0;
var client = new Windows.Web.Syndication.SyndicationClient();
// The URI is validated by catching exceptions thrown by the Uri constructor.
var uri = null;
try {
    uri = new Windows.Foundation.Uri(uriString);
} catch (error) {
    WinJS.log && WinJS.log("Error: Invalid URI");
    return;
}
```

接下來，我們會透過設定所需的任何伺服器認證 ([**ServerCredential**](https://msdn.microsoft.com/library/windows/apps/br243461) 屬性)、Proxy 認證 ([**ProxyCredential**](https://msdn.microsoft.com/library/windows/apps/br243459) 屬性) 及 HTTP 標頭 ([**SetRequestHeader**](https://msdn.microsoft.com/library/windows/apps/br243462) 方法) 來設定要求。 設定好基本的要求參數之後，會使用應用程式提供的摘要 URI 字串建立有效的 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br226017) 物件。 然後，將 **Uri** 物件傳遞到 [**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) 函式以要求摘要。

假設傳回所需的摘要內容，範例程式碼會逐一查看每個摘要項目，呼叫 **displayCurrentItem** (我們將在稍後定義)，以便透過 UI 以清單方式顯示項目及其內容。

您必須撰寫程式碼，以處理在呼叫大多數非同步網路方法時的例外狀況。 您的例外狀況處理常式可以抓取例外狀況發生原因的更詳細資訊，更清楚地了解失敗的情況並作出適當的決定。

如果無法與 HTTP 伺服器建立連線或 [**Uri**](https://msdn.microsoft.com/library/windows/apps/br243460) 物件未指向有效的 AtomPub 或 RSS 摘要，[**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br226017) 方法會擲回例外狀況。 如果發生錯誤，Javascript 範例程式碼會使用 **onError** 函式來擷取例外狀況，然後列印出例外狀況中的詳細訊息。

> [!div class="tabbedCodeSnippets"]
```csharp
try
{
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.SetRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    feed = await client.RetrieveFeedAsync(uri);
    // Retrieve the title of the feed and store it in a string.
    string title = feed.Title.Text;
    // Iterate through each feed item.
    foreach (Windows.Web.Syndication.SyndicationItem item in feed.Items)
    {
        displayCurrentItem(item);
    }
}
catch (Exception ex)
{
    // Handle the exception here.
}
```
```javascript
function onError(err) {
    WinJS.log && WinJS.log(err, "sample", "error");
    // Match error number with a ErrorStatus value.
    // Use Windows.Web.WebErrorStatus.getStatus() to retrieve HTTP error status codes.
    var errorStatus = Windows.Web.Syndication.SyndicationError.getStatus(err.number);
    if (errorStatus === Windows.Web.Syndication.SyndicationErrorStatus.invalidXml) {
        displayLog("An invalid XML exception was thrown. Please make sure to use a URI that points to a RSS or Atom feed.");
    }
}
// Retrieve and display feed at given feed address.
function retreiveFeed(uri) {
    // Although most HTTP servers do not require User-Agent header, 
    // others will reject the request or return a different response if this header is missing.
    // Use the setRequestHeader() method to add custom headers.
    client.setRequestHeader("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
    client.retrieveFeedAsync(uri).done(function (feed) {
        currentFeed = feed;
        WinJS.log && WinJS.log("Feed download complete.", "sample", "status");
        var title = "(no title)";
        if (currentFeed.title) {
            title = currentFeed.title.text;
        }
        document.getElementById("CurrentFeedTitle").innerText = title;
        currentItemIndex = 0;
        if (currentFeed.items.size > 0) {
            displayCurrentItem();
        }
        // List the items.
        displayLog("Items: " + currentFeed.items.size);
     }, onError);
}
```

在上一個步驟中，[**RetrieveFeedAsync**](https://msdn.microsoft.com/library/windows/apps/br243460) 已傳回要求的摘要內容，而範例程式碼也順利地逐一查看可用的摘要項目。 這些項目都會使用 [**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) 物件來表示，而這個物件包含相關同步發佈標準 (RSS 或 Atom) 提供的所有項目屬性和內容。 在下列範例中，我們觀察到 **displayCurrentItem** 函式透過每個項目運作，且透過各種指定 UI 元素來顯示其內容。

> [!div class="tabbedCodeSnippets"]
```csharp
private void displayCurrentItem(Windows.Web.Syndication.SyndicationItem item)
{
    string itemTitle = item.Title == null ? "No title" : item.Title.Text;
    string itemLink = item.Links == null ? "No link" : item.Links.FirstOrDefault().ToString();
    string itemContent = item.Content == null ? "No content" : item.Content.Text;
    //displayCurrentItem is continued below.
```
```javascript
function displayCurrentItem() {
    var item = currentFeed.items[currentItemIndex];
    // Display item number.
    document.getElementById("Index").innerText = (currentItemIndex + 1) + " of " + currentFeed.items.size;
    // Display title.
    var title = "(no title)";
    if (item.title) {
        title = item.title.text;
    }
    document.getElementById("ItemTitle").innerText = title;
    // Display the main link.
    var link = "";
    if (item.links.size > 0) {
        link = item.links[0].uri.absoluteUri;
    }
    var link = document.getElementById("Link");
    link.innerText = link;
    link.href = link;
    // Display the body as HTML.
    var content = "(no content)";
    if (item.content) {
        content = item.content.text;
    }
    else if (item.summary) {
        content = item.summary.text;
    }
    document.getElementById("WebView").innerHTML = window.toStaticHTML(content);
                //displayCurrentItem is continued below.
```

如同稍早的建議，[**SyndicationItem**](https://msdn.microsoft.com/library/windows/apps/br243533) 物件所代表的內容類型會隨著發佈摘要所採用的摘要標準 (RSS 或 Atom) 而異。 例如，Atom 摘要能夠提供 [**Contributors**](https://msdn.microsoft.com/library/windows/apps/br243540) 的清單，但是 RSS 摘要則否。 不過，您可以使用 [**SyndicationItem.ElementExtensions**](https://msdn.microsoft.com/library/windows/apps/br243543) 屬性，存取和顯示兩個標準都不支援的摘要項目所包含的延伸元素 (例如，Dublin Core 延伸元素)，如下列範例程式碼所示範：

> [!div class="tabbedCodeSnippets"]
```csharp
    //displayCurrentItem continued.
    string extensions = "";
    foreach (Windows.Web.Syndication.SyndicationNode node in item.ElementExtensions)
    {
        string nodeName = node.NodeName;
        string nodeNamespace = node.NodeNamespace;
        string nodeValue = node.NodeValue;
        extensions += nodeName + "\n" + nodeNamespace + "\n" + nodeValue + "\n";
    }
    this.listView.Items.Add(itemTitle + "\n" + itemLink + "\n" + itemContent + "\n" + extensions);
}
```
```javascript
    // displayCurrentItem function continued.
    var bindableNodes = [];
    for (var i = 0; i < item.elementExtensions.size; i++) {
        var bindableNode = {
            nodeName: item.elementExtensions[i].nodeName,
             nodeNamespace: item.elementExtensions[i].nodeNamespace,
             nodeValue: item.elementExtensions[i].nodeValue,
        };
        bindableNodes.push(bindableNode);
    }
    var dataList = new WinJS.Binding.List(bindableNodes);
    var listView = document.getElementById("extensionsListView").winControl;
    WinJS.UI.setOptions(listView, {
        itemDataSource: dataList.dataSource
    });
}
```
