---
author: jwmsft
title: 應用程式分析
description: 分析應用程式的效能問題。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 346e6790c6578bf861ba1dda937eae6d4d50f00f
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5555037"
---
# <a name="app-analysis-overview"></a>應用程式分析概觀

「應用程式分析」是一個工具，可為開發人員提供效能問題的提醒通知。 「應用程式分析」會依據一組效能指導方針和最佳做法執行您的應用程式程式碼。

「應用程式分析」會從 App 所遇到的常見效能問題的規則集來識別問題。 「應用程式分析」會適時指向 Visual Studio 的時間軸工具、來源資訊及文件，來為您提供調查的方法。

「應用程式分析」中的規則會參考作為您 App 檢查依據的指導方針和最佳做法。

## <a name="decoded-image-size-larger-than-render-size"></a>解碼的影像大小大於呈現大小

擷取影像時是以非常高的解析度擷取，這可能會導致 App 在將影像資料解碼時使用較多 CPU，以及在從磁碟載入影像之後使用較多記憶體。 但是如果只為了顯示比原生大小還小的影像，而將高解析度影像解碼並儲存在記憶體中，實在不太合理。 因此，請改為使用 DecodePixelWidth 和 DecodePixelHeight 屬性，以將在螢幕上繪製的實際大小建立一個影像版本。

### <a name="impact"></a>影響

以影像的非原生大小顯示影像不僅會對 CPU 時間造成負面影響 (因為解碼成適當的大小，還有下載時間)，也會對記憶體造成負面影響。

### <a name="causes-and-solutions"></a>原因和解決方案

#### <a name="image-is-not-being-set-asynchronously"></a>影像是以非同步方式設定

App 使用 SetSource()，而不是 SetSourceAsync()。 當設定串流以透過非同步方式將影像解碼時，您應該一律避免使用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)，改為使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)。 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>影像在 ImageSource 不在動態樹狀結構中時被呼叫

使用 SetSourceAsync 或 UriSource 來設定內容之後，BitmapImage 連接到動態的 XAML 樹狀結構。 您在設定來源之前，應該一律將 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) 附加到動態樹狀結構。 每當在標記中指定影像元素或筆刷時，就自動會是這種情況。 以下提供範例。 

**動態樹狀結構範例**

範例 1 (好)—在標記中指定的統一資源識別元 (URI)。

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

範例 2 標記—在程式碼後置中指定的 URI。

```xml
<Image x:Name="myImage"/>
```

範例 2 程式碼後置 (好)—將 BitmapImage 連接到樹狀結構之後才設定其 UriSource。

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

範例 2 程式碼後置 （壞） — 將 BitmapImage UriSource 設定連接到樹狀結構之前先。

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>影像筆刷為非矩形 

當影像用於非矩形筆刷時，影像會使用軟體點陣化路徑，這將完全不會縮放影像。 此外，它也必須在軟體和硬體記憶體中都儲存一份影像。 例如，如果將一個影像用來作為橢圓形的筆刷，就會將可能很大的完整影像在內部儲存兩次。 使用非矩形筆刷時，您的 App 應該會將其影像預先縮放到接近要呈現的大小。

或者，您也可以使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 屬性來設定一個明確的解碼大小，以將在螢幕上繪製的實際大小建立一個影像版本。

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 的單位預設是實體像素。 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 屬性可以用來變更這個行為：將 **DecodePixelType** 設定為 **Logical** 會導致解碼大小自動採用系統目前的縮放比例，類似於其他的 XAML 內容。 因此，舉例來說，如果您希望 **DecodePixelWidth** 和 **DecodePixelHeight** 符合影像顯示所在之 Image 控制項的 Height 和 Width 屬性，將 **DecodePixelType** 設定為 **Logical** 通常是適當的。 有了使用實體像素的預設行為時，您必須自行考量系統目前的縮放比例；而且您必須接聽縮放比例變更通知，以因應使用者變更其顯示喜好設定的情況。

在某些無法預先決定適當解碼大小的情況下，您應該遵從 XAML 的自動以正確大小解碼功能，它會在未指定明確的 DecodePixelWidth/DecodePixelHeight 時，盡可能嘗試以適當大小將影像解碼。

如果您預先知道影像內容的大小，就應該設定明確的解碼大小。 如果提供的解碼大小是相對其他 XAML 元素的大小，您也應該一併將 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 設定為 **Logical**。 例如，如果您使用 Image.Width 和 Image.Height 來明確設定內容大小，您可以將 DecodePixelType 設定為 DecodePixelType.Logical 以使用與 Image 控制項相同的邏輯像素維度，然後明確使用 BitmapImage.DecodePixelWidth 和/或 BitmapImage.DecodePixelHeight 來控制影像的大小，以達到潛在節省大量記憶體的目的。

請注意，決定解碼內容的大小時，應考量 Image.Stretch。

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>在 BitmapIcons 內部使用的影像回復為解碼成原始大小 

請使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 屬性來設定一個明確的解碼大小，以將在螢幕上繪製的實際大小建立一個影像版本。

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>在螢幕上顯示得極大的影像回復為解碼成原始大小 

在螢幕上顯示得極大的影像回復為解碼成原始大小。 請使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 屬性來設定一個明確的解碼大小，以將在螢幕上繪製的實際大小建立一個影像版本。

#### <a name="image-is-hidden"></a>影像被隱藏

透過在主機影像元素、筆刷或任何父元素上，將 Opacity 設定為 0，或將 Visibility 設定為 Collapsed，即可隱藏影像。 之所以在螢幕上看不見影像，是因為裁剪或透明度可能回復為解碼成原始大小。 

#### <a name="image-is-using-ninegrid-property"></a>影像使用 NineGrid 屬性

當影像用於 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) 時，影像會使用軟體點陣化路徑，這將完全不會縮放影像。 此外，它也必須在軟體和硬體記憶體中都儲存一份影像。 使用 **NineGrid** 時，您的 App 應該會將其影像預先縮放到接近要呈現的大小。

使用 NineGrid 屬性的影像會回復為解譯成原始大小。 請考慮在原始影像新增 NineGrid 效果。

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>DecodePixelWidth 或 DecodePixelHeight 被設定為大於將在螢幕上顯示的影像大小 

如果 DecodePixelWidth/Height 被明確設定為大於將在螢幕上顯示的影像大小，則 App 會不必要地使用額外的記憶體，每一像素最多可達 4 個位元組，大型影像很快就會變得非常耗費資源。 影像也會藉由使用雙線性縮放來縮小，這在縮放比例變大時可能會導致影像模糊。

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>影像在產生拖放影像的過程中被解碼

請使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 屬性來設定一個明確的解碼大小，以將在螢幕上繪製的實際大小建立一個影像版本。

## <a name="collapsed-elements-at-load-time"></a>載入時摺疊的元素

應用程式中一種常見的模式就是一開始隱藏 UI 中的元素，在稍後才顯示。 在大多數情況下，應該使用 x:Load 或 x:DeferLoadStrategy 來延遲這些元素，以避免支付在載入時建立元素的費用。

這包括使用可見度轉換器的布林值來隱藏項目直到稍後才顯示的情況。

### <a name="impact"></a>影響

摺疊的元素會與其他元素一起載入，並且也會增加載入時間。

### <a name="cause"></a>原因

因為項目在載入時摺疊，所以觸發了這個規則。 將元素摺疊或將其不透明度設定為 0 並不會防止建立該元素。 這個規則可能是由使用可見度轉換器的布林值並採用預設值 false 的 App 所造成。

### <a name="solution"></a>解決方案

使用 [x:Load attribute](../xaml-platform/x-load-attribute.md) 或 [x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785)，您便可延遲載入某個 UI，而在需要時才載入它。 這是一個相當好的方式，可延遲處理在第一個畫面不顯示的 UI。 您可以選擇在需要時載入元素，或是隨著一組延遲邏輯一起載入。 若要觸發載入，請針對您希望載入的元素呼叫 findName。 x:Load 擴充可解除載入元素的 x:DeferLoadStrategy 功能，以及透過 x:Bind 控制載入狀態。

在某些情況下，使用 findName 來顯示某個 UI 可能無法作為解答。 如果您預期在按一下按鈕且延遲很低的情況下實現某個重要的 UI，便會是這種情況。 在此情況下，您可能會想要使用更多的記憶體來降低 UI 延遲，若是如此，您應該使用 x:DeferLoadStrategy，並針對您希望實現的元素將 Visibility 設定為 Collapsed。 在載入頁面且 UI 執行緒可用之後，您可以在必要時呼叫 findName 來載入元素。 使用者將無法看見元素，直到您將元素的 Visibility 設定為 Visible 為止。

## <a name="listview-is-not-virtualized"></a>ListView 未虛擬化

UI 虛擬化是您對改善集合效能所能做的最重要改進。 這意謂著系統會依需求建立代表項目的 UI 元素。 對於繫結至 1000 個項目集合的項目控制項，同時針對所有項目建立 UI 是一種資源浪費，因為項目不會同時全部顯示。 ListView 和 GridView (及其他標準 ItemsControl 衍生的控制項) 會為您執行 UI 虛擬化。 當項目即將被捲動到檢視中 (相差幾頁) 時，架構會產生項目的 UI 並且快取它們。 當不太可能再次顯示那些項目時，架構就會回收記憶體。

UI 虛擬化只是可改善集合效能的數個重要因素其中之一。 降低集合項目的複雜度及使用資料虛擬化是其他兩個可改善集合效能的重要層面。 如需有關改善 ListViews 和 GridViews 內集合效能的詳細資訊，請參閱 [ListView 與 GridView UI 最佳化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)一文，以及 [ListView 和 GridView 資料虛擬化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization)一文。

### <a name="impact"></a>影響

非虛擬化 ItemsControl 會載入超過其所需的子項目數量，因而導致增加載入時間和資源使用量。

### <a name="cause"></a>原因

檢視區概念對 UI 虛擬化很重要，因為架構必須建立可能要顯示的元素。 一般而言，ItemsControl 的檢視區是邏輯控制項的延伸。 例如，ListView 的檢視區是 ListView 元素的寬度和高度。 有些面板允許子元素有不限數量的空間，範例是 ScrollViewer 和 Grid，使用自動調整大小的列或欄。 將虛擬化的 ItemsControl 放在這類面板中時，它會佔用足夠的空間來顯示其所有項目，這會導致虛擬化變成無效。 

### <a name="solution"></a>解決方案

在您使用的 ItemsControl 上設定寬度和高度以還原虛擬化。

## <a name="ui-thread-blocked-or-idle-during-load"></a>UI 執行緒在載入時被封鎖或閒置

UI 執行緒封鎖係指對執行關閉執行緒來封鎖 UI 執行緒之函式的同步呼叫。  

如需可改善您 App 啟動效能的最佳做法完整清單，請參閱 [App 啟動效能的最佳做法](https://msdn.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance)和[讓 UI 執行緒保持回應](https://msdn.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive)。

### <a name="impact"></a>影響

在載入時被封鎖或閒置的 UI 執行緒將會防止配置及其他 UI 作業，而增加啟動時間。

### <a name="cause"></a>原因

平台的 UI 程式碼和您應用程式的 UI 程式碼都是在相同的 UI 執行緒上執行。 該執行緒上一次只能執行一個指令，如果您的應用程式程式碼花費太多時間來處理事件，架構便無法執行配置或引發代表使用者互動的新事件。 App 的回應性與是否有 UI 執行緒來處理工作相關。

### <a name="solution"></a>解決方案

即使您的 App 有部分無法完整運作，它仍然可以與使用者互動。 例如，如果應用程式顯示資料時需要一些時間抓取，您可以在不考慮應用程式啟動程式碼的情況下，用非同步的方式抓取資料來執行程式碼。 等到資料可供使用時，再將該資料填入 App 的使用者介面。 為了協助保持 App 的回應性，平台為其許多 API 提供非同步的版本。 非同步 API 可確保使用中的執行緒不會被封鎖太長的時間。 當您從 UI 執行緒呼叫 API 時，如果有非同步版本可用，請使用非同步版本。

## <a name="binding-is-being-used-instead-of-xbind"></a>使用 {Binding}，而不是 {x:Bind}

當您的 App 使用 {Binding} 陳述式時，便會引發這個規則。 若要改善 App 效能，應考量讓 App 使用 {x:Bind}。

### <a name="impact"></a>影響

{Binding} 需要比 {x:Bind} 更多的時間和記憶體來執行。

### <a name="cause"></a>原因

App 使用 {Binding}，而不是 {x:Bind}。 {Binding} 會帶來不小的工作集和 CPU 額外負荷。 建立 {Binding} 會造成一系列的配置，而更新繫結目標則可能造成反射和 Boxing。

### <a name="solution"></a>解決方案

使用 {x:Bind} 標記延伸，這會在建置階段編譯繫結。 {x:Bind} 繫結 (通常指已編譯的繫結) 除了具有極佳的效能、可在編譯時驗證您的繫結運算式之外，還可透過讓您在產生以作為頁面部分類別的程式碼檔中設定中斷點，來支援偵錯功能。 

請注意 x:Bind 並非所有情況 (例如晚期繫結案例) 都適用。 如需 {x:Bind} 未涵蓋之情況的完整清單，請參閱 {x:Bind} 文件。

## <a name="xname-is-being-used-instead-of-xkey"></a>使用 x:Name，而不是 x:Key

ResourceDictionaries 通常是用來將您的資源儲存在稍微全域的層級，亦即 App 想要在多個地方參考的資源；例如樣式、筆刷、範本等。 一般而言，我們已將 ResourceDictionaries 最佳化，讓它除非接獲要求，否則不將資源具現化。 但是有少數幾個您需要稍加小心的地方。

### <a name="impact"></a>影響

任何含有 x:Name 的資源都會在 ResourceDictionary 一建立好就立即具現化。 這是因為 x:Name 會告知平台您的 App 需要此資源的欄位存取權，因此平台需要建立可供建立參考的內容。

### <a name="cause"></a>原因

您的 App 在資源上設定 x:Name。

### <a name="solution"></a>解決方案

當您不從程式碼後置參考資源時，請使用 x:Key，而不要使用 x:Name。

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>集合控制項使用非虛擬面板

如果您提供自訂項目面板範本 (請參閱 ItemsPanel)，請確定您使用的是虛擬面板，例如 ItemsWrapGrid 或 ItemsStackPanel。 如果您使用的是 VariableSizedWrapGrid、WrapGrid 或 StackPanel，則不會虛擬化。 此外，只有在使用 ItemsWrapGrid 或 ItemsStackPanel 時，才會引發下列 ListView 事件：ChoosingGroupHeaderContainer、ChoosingItemContainer 和 ContainerContentChanging。

UI 虛擬化是您對改善集合效能所能做的最重要改進。 這意謂著系統會依需求建立代表項目的 UI 元素。 對於繫結至 1000 個項目集合的項目控制項，同時針對所有項目建立 UI 是一種資源浪費，因為項目不會同時全部顯示。 ListView 和 GridView (及其他標準 ItemsControl 衍生的控制項) 會為您執行 UI 虛擬化。 當項目即將被捲動到檢視中 (相差幾頁) 時，架構會產生項目的 UI 並且快取它們。 當不太可能再次顯示那些項目時，架構就會回收記憶體。

UI 虛擬化只是可改善集合效能的數個重要因素其中之一。 降低集合項目的複雜度及使用資料虛擬化是其他兩個可改善集合效能的重要層面。 如需有關改善 ListViews 和 GridViews 內集合效能的詳細資訊，請參閱 [ListView 與 GridView UI 最佳化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview)一文，以及 [ListView 和 GridView 資料虛擬化](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization)一文。

### <a name="impact"></a>影響

非虛擬化 ItemsControl 會載入超過其所需的子項目數量，因而導致增加載入時間和資源使用量。

### <a name="cause"></a>原因

您使用不支援虛擬化的面板。

### <a name="solution"></a>解決方案

使用虛擬面板，例如 ItemsWrapGrid 或 ItemsStackPanel。

## <a name="accessibility-uia-elements-with-no-name"></a>協助工具：沒有名稱的 UIA 元素

在 XAML 中，您可以透過設定 AutomationProperties.Name 來提供名稱。 如果未設定 AutomationProperties.Name，則許多自動化對等會提供預設名稱給 UIA。 

### <a name="impact"></a>影響

如果使用者接觸到沒有名稱的元素，他們通常無法得知該元素與什麼相關。 

### <a name="cause"></a>原因

元素的 UIA 名稱為 null 或空白。 這個規則會檢查 UIA 看到的內容，而不是 AutomationProperties.Name 的值。

### <a name="solution"></a>解決方案

將控制項 XAML 中的 AutomationProperties.Name 屬性設定為適當的當地語系化字串。

有時，正確的應用程式修正不是提供名稱，而是將 UIA 元素從所有地方 (原始樹狀結構除外) 移除。 做法是在 XAML 中設定 AutomationProperties.AccessibilityView = “Raw”。

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>協助工具：具有相同 Controltype 的 UIA 元素不應該有相同的名稱

兩個具有相同 UIA 父系的 UIA 元素不得有相同的 Name 和 ControlType。 如果兩個控制項的 ControlType 不同，則可以有相同的 Name。 

這個規則不會檢查是否有具有不同父系的重複名稱。 不過，在大部分情況下，在整個視窗中不應該有重複的 Name 和 ControlType，即使其父系不同。 可接受一個視窗內有重複名稱的情況是含有完全相同項目的兩個清單。 在此情況下，會預期清單項目要有相同的 Name 和 ControlType。

### <a name="impact"></a>影響

如果使用者觸及的元素與另一個具有相同 UIA 父系的元素有相同的 Name 和 ControlType，則使用者可能會無法區別這兩個元素間的差異。

### <a name="cause"></a>原因

具有相同 UIA 父系的 UIA 元素有相同的 Name 和 ControlType。

### <a name="solution"></a>解決方案

在 XAML 中使用 AutomationProperties.Name 來設定名稱。 在經常發生這種情況的清單中，請使用繫結將 AutomationProperties.Name 的值繫結至資料來源。


