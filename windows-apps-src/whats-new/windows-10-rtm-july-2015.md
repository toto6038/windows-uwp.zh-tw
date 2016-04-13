---
Description: Windows 10 和新的開發人員工具提供新的通用 Windows 平台 (UWP) 所支援的工具、功能及使用經驗。
title: Windows 10 提供給開發人員的新功能 - 2015 年 7 月
---

# Windows 10 RTM 提供給開發人員的新功能：2015 年 7 月


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 10 和新的開發人員工具提供新的通用 Windows 平台 (UWP) 所支援的工具、功能及使用經驗。 在 Windows 10 上[安裝工具和 SDK](https://dev.windows.com/downloads) 之後，您已經準備好可以[建立新的通用 Windows 應用程式](https://msdn.microsoft.com/library/windows/apps/bg124288)，或探索如何使用您在[ Windows 上的現有應用程式程式碼](https://msdn.microsoft.com/library/windows/apps/mt238321)。

以下為您逐項列出 Windows 10 RTM 中的新功能。

## 彈性配置


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">獨特內容的多重檢視</td>
<td align="left">XAML 針對共用相同程式碼檔案的獨特檢視 (.xaml 檔案)，提供了定義這些檢視的新支援。 這可讓您輕鬆建立及維護針對特定裝置系列或案例而設計的不同檢視。 如果您的 app 有相異的 UI 內容、配置或瀏覽模型在不同的情況下會有很大的差異，請建立多重檢視。 例如，您可以搭配使用 [<strong>Pivot</strong>] (https://msdn.microsoft.com/library/windows/apps/dn608241) 與適合在您的行動應用程式上單手操作的瀏覽，但搭配使用 [<strong>SplitView</strong>] (https://msdn.microsoft.com/library/windows/apps/dn864360) 與針對傳統型 app 的滑鼠而最佳化的瀏覽功能表。</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">使用新的 [<strong>VisualState.StateTriggers</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890396) 功能，可以根據視窗高度/寬度或自訂觸發程序有條件地設定屬性。 過去，您必須在程式碼中處理 視窗 [<strong>SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br209055) 事件，並呼叫 [<strong>VisualStateManager.GoToState</strong>](https://msdn.microsoft.com/library/windows/apps/br209025)。</td>
</tr>
<tr class="odd">
<td align="left">Setter</td>
<td align="left"><p>使用新的 [<strong>VisualState.Setters</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890395) 語法，可以使用簡化的標記，在 [<strong>VisualStateManager</strong>] (https://msdn.microsoft.com/library/windows/apps/br209021) 中定義屬性變更。 過去，您必須使用腳本並建立動畫以套用屬性變更，例如將 [<strong>StackPanel</strong>] (https://msdn.microsoft.com/library/windows/apps/br209635) 的方向從水平變更為垂直。 在通用 Windows 應用程式中，您可以使用這個較簡單的 Setter 語法︰</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## XAML 功能


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">編譯的資料繫結 (x:Bind)</td>
<td align="left"><p>在通用 Windows 應用程式中，您可以使用 x:Bind 屬性所啟用的新編譯器型繫結機制。 編譯器型繫結是強型別，並且在編譯期間進行處理，不僅速度更快，並且可在繫結類型不符時提供編譯時間錯誤。 由於繫結會轉譯為編譯的應用程式程式碼，因此，現在您可以在 Visual Studio 中逐步執行程式碼以診斷特定的繫結問題，進而對繫結進行偵錯。 您也可以使用 x:Bind 繫結至方法，如下所示︰</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>對於典型的繫結案例，您可以使用 x:Bind 取代 Binding，而獲得較佳的效能和可維護性。</p></td>
</tr>
<tr class="even">
<td align="left">清單的宣告式增量轉譯 (x:Phase)</td>
<td align="left"><p>在通用 Windows 應用程式中，新的 X:phase 屬性可讓您使用 XAML 執行增量或分階段的清單轉譯，而不使用程式碼。 平移具有複雜項目的較長清單時，app 轉譯項目的速度可能無法跟上平移的速度，而造成不佳的使用者體驗。 分階段的轉譯可讓您指定清單項目中個別元素的優先順序，使快速平移案例中只會轉譯清單項目中最重要的部分。 這會為您的使用者產生更順暢的平移體驗。</p>
<p>在 Windows 8.1 中，您可以處理 [<strong>ContainerContentChanging</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298914) 事件，並撰寫可分階段轉譯清單項目的程式碼。 在 UWP app 中，您可以使用 x:Phase 屬性透過宣告方式完成分階段的轉譯。 X:phase 與編譯的繫結 X:bind 搭配使用時，可讓您輕鬆地為資料範本中的每個繫結元素指定轉譯優先順序。 在平移時，轉譯項目的工作會根據的階段分配時間，因此可支援增量項目轉譯。</p></td>
</tr>
<tr class="odd">
<td align="left">UI 元素的延遲載入 (x:DeferLoadingStrategy)</td>
<td align="left">在通用 Windows 應用程式中，新的 x:DeferLoadingStrategy 指示詞可讓您將部分的使用者介面指定為延遲載入，而改善啟動效能並減少 app 的記憶體使用量。 例如，如果您的應用程式 UI 有某個資料驗證元素只有在輸入不正確的資料時才會顯示，則您可以延遲到需要時才載入該元素。 隨後，元素物件並不會在頁面載入時建立，而只會在有資料錯誤而需要將其新增至頁面的視覺化樹狀結構時才會建立。</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">新的 [<strong>SplitView</strong>] (https://msdn.microsoft.com/library/windows/apps/dn864360) 控制項可讓您輕鬆顯示和隱藏暫時性內容。 它常用於最上層瀏覽案例，例如漢堡功能表；在此功能表中，會隱藏瀏覽內容，等到需要時再移入做為使用者動作結果。</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546) 是一個新的配置面板，可讓您定位及排列彼此有關係或與上層面板有關係的子物件。 例如，您可以指定某個文字應一律位於面板左側，而按鈕應一律對齊於文字下方。 在建立沒有明確線性模式的使用者介面時，如果需要使用 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) 或 [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704)，請使用 <strong>RelativePanel</strong>。</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">[<strong>CalendarView</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890052) 控制項可讓您使用可自訂、以月份為基礎的檢視，輕鬆檢視及選取日期和日期範圍。 <strong>CalendarView</strong> 支援最小、最大和封鎖日期，以限制可供選取的日期。 您也可以設定自訂密度列，用以顯示某天之排程的一般「密度」。</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083) 是一種下拉式控制項，最適合用來從 [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) 中挑選單一日期，然後取得各種重要的相關資訊，例如星期幾或行事曆行程密度。 它很類似 [<strong>DatePicker</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298584) 控制項，但是 <strong>DatePicker</strong> 最適合用來挑選已知日期，例如出生日期。</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">新的 [<strong>MediaTransportControls</strong>] (https://msdn.microsoft.com/library/windows/apps/dn831962) 類別可讓您更容易自訂 [<strong>MediaElement</strong>] (https://msdn.microsoft.com/library/windows/apps/br242926) 的傳輸控制項。 在 Windows 8.1 中，您可以啟用 <strong>MediaElement</strong> 的內建傳輸控制項，或自行建立會呼叫 <strong>MediaElement</strong> 方法的傳輸控制項。 現在，您可以使用 <strong>MediaTransportControls</strong> 的內建功能，且仍可輕鬆自訂符合 app 的外觀。</td>
</tr>
<tr class="odd">
<td align="left">屬性變更通知</td>
<td align="left"><p>在通用 Windows app 中，您可以在 [<strong>DependencyObject</strong>] (https://msdn.microsoft.com/library/windows/apps/br242356) 上接聽屬性變更，即使是沒有對應變更事件的屬性亦然。</p>
<p>通知的運作方式與事件相似，但實際上會公開做為回呼。 和事件處理常式一樣，回呼會使用寄件者引數，但不會使取事件引數。 它只會傳入屬性識別碼，以指出屬性。 透過這項資訊，您的 app 將可為多個屬性通知定義單一處理常式。 如需詳細資訊，請參閱 [<strong>RegisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831902) 和 [<strong>UnregisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831910)。</p></td>
</tr>
<tr class="even">
<td align="left">地圖</td>
<td align="left"><p>[<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) 類別已更新成提供 3D 空照圖和街景檢視。 這些新功能和先前的對應功能現在已可在通用 Windows 應用程式中使用。 請使用下列 API，將對應新增到您的 app：</p>
<ul>
<li>[<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751) 命名空間 - 顯示地圖。</li>
<li>[<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979) 命名空間 - 尋找位置和路線。</li>
</ul>
<p>若要立即開始在通用 Windows 應用程式中使用這些 API，請向 [Bing Maps Developer Center](https://www.bingmapsportal.com/) 索取金鑰。 如需詳細資訊，請參閱[How to authenticate a Maps app](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528)。 電腦及手機使用者可以從 [設定] app 下載離線地圖，這也是 Windows 10 的新功能之一。 在無法使用網際網路時，可用的離線地圖可讓 [<strong>MapControl</strong>] (https://msdn.microsoft.com/library/windows/apps/dn637004) 用來顯示地圖。</p></td>
</tr>
<tr class="odd">
<td align="left">輸入按鈕對應</td>
<td align="left">[<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>] (https://msdn.microsoft.com/library/windows/apps/hh943072) 類別有一個新 [<strong>OriginalKey</strong>] (https://msdn.microsoft.com/library/windows/apps/dn960168) 屬性，與 [<strong>Windows.System.VirtualKey</strong>] (https://msdn.microsoft.com/library/windows/apps/br241812) 的對應更新搭配使用時，可讓您取得與鍵盤輸入事件相關聯的原始、未對應的輸入按鈕。</td>
</tr>
<tr class="even">
<td align="left">筆跡</td>
<td align="left"><p>現在，要運用 C++、C# 或 Visual Basic 在 Windows 執行階段應用程式中使用穩定的筆跡功能，已較以往更為容易，這要歸功於 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項和基礎的 [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011) 類別。</p>
<p>[<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項可定義繪製及轉譯筆墨筆劃的重疊區域。 此控制項的功能 (輸入、處理和轉譯) 來自於 [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011)、[<strong>InkStroke</strong>](https://msdn.microsoft.com/library/windows/apps/br208485)、[<strong>InkRecognizer</strong>](https://msdn.microsoft.com/library/windows/apps/br208478) 和 [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979) 類別。</p>
<p></p>
<div class="alert">
<strong>重要事項</strong>：使用 JavaScript 的 Windows 應用程式中不支援這些類別。
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## 更新的 XAML 功能


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">CommandBar 和 AppBar 更新</td>
<td align="left"><p>[<strong>CommandBar</strong>] (https://msdn.microsoft.com/library/windows/apps/dn279427) 與 [<strong>AppBar</strong>] (https://msdn.microsoft.com/library/windows/apps/hh701927) 控制項已經過更新，使得不同裝置系列的 UWP app 有一致的 API、行為和使用者體驗。</p>
<p>通用 Windows 應用程式的 [<strong>CommandBar</strong>] (https://msdn.microsoft.com/library/windows/apps/dn279427) 控制項已經過改良，可提供 [<strong>AppBar</strong>] (https://msdn.microsoft.com/library/windows/apps/hh701927) 功能的超集，並提升在您的 app 中加以使用的彈性。 您應將 <strong>CommandBar</strong> 用於 Windows 10 上的所有新的通用 Windows 應用程式。 在 Windows 8.1 的 <strong>CommandBar</strong> 中，您可以僅使用實作 [<strong>ICommandBarElement</strong>] (https://msdn.microsoft.com/library/windows/apps/dn251969) 的控制項，例如 [<strong>AppBarButton</strong>] (https://msdn.microsoft.com/library/windows/apps/dn279244)。 在通用 Windows 應用程式中，除了 <strong>AppBarButton</strong> 以外，現在您可以將自訂內容放在 <strong>CommandBar</strong> 中。</p>
<p>[<strong>AppBar</strong>] (https://msdn.microsoft.com/library/windows/apps/hh701927) 控制項已經更新，可讓您更輕鬆地將使用 <strong>AppBar</strong> 的 Windows 8.1 應用程式移至通用 Windows 平台。 <strong>AppBar</strong> 依設計可與全螢幕應用程式搭配使用，並使用邊緣手勢來叫用。 控制帳戶已針對 Windows 10 中的視窗化應用程式和缺少邊緣手勢等問題進行更新。</p>
<p>隱藏的 [<strong>AppBar.ClosedDisplayMode</strong>] (https://msdn.microsoft.com/library/windows/apps/dn633872) 先前僅限於 Windows Phone，現在已可支援所有裝置系列，讓您選擇不同層級的命令提示。 [<strong>AppBar</strong>] (https://msdn.microsoft.com/library/windows/apps/hh701927) 依預設會顯示最少的提示，讓您在將 Windows 8.1 應用程式升級至通用 Windows 應用程式時能有一致的使用經驗，而無須再依賴平台中的邊緣手勢支援。</p>
<p><strong>新的</strong> [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) <strong>API：</strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483)、[<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484)、[<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486)、[<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485)、[<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487)。</p>
<p><strong>新的</strong> [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) <strong>API：</strong>[<strong>CommandBarOverflowPresenterStyle</strong>](https://msdn.microsoft.com/library/windows/apps/dn975227) 和 [<strong>CommandBarOverflowPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn975225)。</p></td>
</tr>
<tr class="even">
<td align="left">GridView 更新</td>
<td align="left">在 Windows 10 之前，Windows 上的預設 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 配置方向為水平，Windows Phone 則是垂直。 在 UWP app 中，<strong>GridView</strong> 對於所有裝置系列依預設皆採用垂直配置，以確保您有一致的預設體驗。</td>
</tr>
<tr class="odd">
<td align="left">AreStickyGroupHeadersEnabled 屬性</td>
<td align="left">現在，當您在 [<strong>ListView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242878) 或 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 中顯示分組資料時，群組標頭在清單捲動時會保持為可見狀態。 這在大型資料集中是很重要的，因為其標頭會提供使用者所檢視的資料內容。 不過，如果每個群組中只有少數幾個元素，您可能會想要將項目的標頭捲動到螢幕外。 您可以設定 [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) 和 [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849) 上的 [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028) 屬性，來控制此行為。</td>
</tr>
<tr class="even">
<td align="left">GroupHeaderContainerFromItemContainer 方法</td>
<td align="left">當您在 [<strong>ItemsControl</strong>] (https://msdn.microsoft.com/library/windows/apps/br242803) 中顯示分組資料時，您可以呼叫 [<strong>GroupHeaderContainerFromItemContainer</strong>] (https://msdn.microsoft.com/library/windows/apps/dn903984) 方法，以取得群組之父項標頭的參考。 例如，如果使用者刪除了群組中的最後一個項目，您可以取得群組標頭的參考，並一併移除項目和群組標頭。</td>
</tr>
<tr class="odd">
<td align="left">ChoosingGroupHeaderContainer 事件</td>
<td align="left">[<strong>ListViewBase</strong>] (https://msdn.microsoft.com/library/windows/apps/br242879) 上的新 [<strong>ChoosingGroupHeaderContainer</strong>] (https://msdn.microsoft.com/library/windows/apps/dn899082) 事件，可讓您為 [<strong>ListView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242878) 或 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 中的群組標頭設定狀態。 例如，您可能會處理此事件以設定群組標頭的 [<strong>AutomationProperties.NameProperty</strong>] (https://msdn.microsoft.com/library/windows/apps/br209099)，來表示輔助技術中的群組。</td>
</tr>
<tr class="even">
<td align="left">ChoosingItemContainer 事件</td>
<td align="left">[<strong>ListViewBase</strong>] (https://msdn.microsoft.com/library/windows/apps/br242879) 上的新 [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) 事件，可讓您進一步控制 [<strong>ListView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242878) 或 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 中的 UI 虛擬化。 將此事件與 [<strong>ContainerContentChanging</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298914) 事件搭配使用，可維持您在需要時從中開啟的回收容器佇列。 例如，如果資料來源已因為篩選而重設，您可以快速比對已建立的視覺效果集 (ItemContainers) 和其資料，以達到最佳效能。</td>
</tr>
<tr class="odd">
<td align="left">清單捲動虛擬化</td>
<td align="left"><p>XAML [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) 控制項有新的 [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) 事件，可改善資料收集發生變更時的控制效能。</p>
<p>系統現在不會執行會重新顯示入口動畫的完整清單重設，而會保有目前在檢視中的項目以及焦點和選取狀態；檢視區中的新項目和移除的項目會以動畫方式流暢地移入和移出。 資料收集在經過容器未終結的變更之後，應用程式將可快速比對任何「舊」項目及其先前的容器，並略過進一步處理容器生命週期覆寫方法的作業。 只有「新」項目會進行處理，並與回收或新的容器產生關聯。</p></td>
</tr>
<tr class="even">
<td align="left">SelectRange 方法和 SelectedRanges 屬性</td>
<td align="left">在通用 Windows 應用程式中，[<strong>ListView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242878) 與 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 控制項現在可讓您以項目索引的角度選取項目，而不是項目物件參考。 以此方式說明選取項目會更有效率，因為不需要為每個選取的項目建立項目物件。 如需詳細資訊，請參閱 [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244)、[<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245) 和 [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241)。</td>
</tr>
<tr class="odd">
<td align="left">新的 ListViewItemPresenter API</td>
<td align="left">[<strong>ListView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242878) 與 [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705) 使用項目展示器來提供選取項目和焦點的預設視覺效果。 在 UWP app 中，[<strong>ListViewItemPresenter</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298500) 與 [<strong>GridViewItemPresenter</strong>] (https://msdn.microsoft.com/library/windows/apps/dn279298) 有新的屬性，可讓您進一步自訂清單項目的視覺效果。 新的屬性是 [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905)、[<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923)、[<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370)、[<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372)、[<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931) 和 [<strong>SelectedPressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913937)。</td>
</tr>
<tr class="even">
<td align="left">SemanticZoom 更新</td>
<td align="left"><p>現在，[<strong>SemanticZoom</strong>] (https://msdn.microsoft.com/library/windows/apps/hh702601) 控制項可讓所有裝置系列的 UWP app 有單一而一致的行為。</p>
<p>在放大檢視中與縮小檢視之間切換的預設動作，是點選放大檢視中的群組標頭。 此行為與 Windows Phone 8.1 上的行為相同，但與使用捏合手勢進行縮放的 Windows 8.1 不同。 若要變更使用捏合進行縮放的檢視，請在 [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) 的內部 [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) 上設定 [<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=&quot;Enabled&quot;。</p>
<p>在通用 Windows 應用程式中，縮小檢視會取代放大檢視，且其大小會與它所取代的檢視相同。 此行為與 Windows 8.1 上的行為相同，但與 Windows Phone 8.1 不同；在 Windows Phone 8.1 中，縮小檢視會佔用整個螢幕大小，並呈現在所有其他內容之上。</p></td>
</tr>
<tr class="odd">
<td align="left">DatePicker 和 TimePicker 更新</td>
<td align="left">現在，[<strong>DatePicker</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298584) 與 [<strong>TimePicker</strong>] (https://msdn.microsoft.com/library/windows/apps/dn299280) 控制項會以一致的方式實作於所有裝置系列的通用 Windows 應用程式上。 它們在 Windows 10 中也有新的外觀。 現在的快顯部分在所有裝置上都會使用 [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013) 和 [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313) 控制項。 此行為與 Windows Phone 8.1 上的行為相同，但與使用 [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348) 控制項的 Windows 8.1 不同。 使用飛出視窗控制項可讓您更輕鬆地建立自訂的日期和時間選擇器。</td>
</tr>
<tr class="even">
<td align="left">新的 ScrollViewer API</td>
<td align="left">[<strong>ScrollViewer</strong>] (https://msdn.microsoft.com/library/windows/apps/br209527) 有新的 [<strong>DirectManipulationStarted</strong>] (https://msdn.microsoft.com/library/windows/apps/dn858544) 與 [<strong>DirectManipulationCompleted</strong>] (https://msdn.microsoft.com/library/windows/apps/dn858543) 事件，以在觸控平移開始和停止時通知您的 app。 您可以處理這些事件，以協調您的 UI 與這些使用者動作。</td>
</tr>
<tr class="odd">
<td align="left">MenuFlyout 更新</td>
<td align="left">在通用 Windows app 中，有新的 API 可讓您更輕鬆地建置更好的操作功能表。 新的 [<strong>MenuFlyout.ShowAt</strong>] (https://msdn.microsoft.com/library/windows/apps/dn913174) 方法可讓您指定飛出視窗相對於其他元素的顯示位置。 (而且，您的 [<strong>MenuFlyout</strong>] (https://msdn.microsoft.com/library/windows/apps/dn299030) 甚至可與您 app 視窗的界限重疊。) 使用新的 [<strong>MenuFlyoutSubItem</strong>] (https://msdn.microsoft.com/library/windows/apps/dn913170) 類別，可建立階層式功能表。</td>
</tr>
<tr class="even">
<td align="left">ContentPresenter、Grid 和 StackPanel 的新框線屬性</td>
<td align="left">一般容器控制項具有新的框線屬性，可讓您沿著控制項繪製框線，而不需要在 XAML 中新增額外的框線元素。 [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378)、[<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704) 和 [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) 具有下列新屬性：[<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179)、[<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181)、[<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183) 和 [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187)。</td>
</tr>
<tr class="odd">
<td align="left">ContentPresenter 上的新文字 API</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378) 有新的 API 可讓您進一步控制文字顯示：[<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982)、[<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933)、[<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935) 和 [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937)。</td>
</tr>
<tr class="even">
<td align="left">系統焦點視覺效果</td>
<td align="left">XAML 控制項的焦點視覺效果現在由系統所建立，而不是宣告為控制項範本中的 XAML 元素。 焦點視覺效果通常不需要在行動裝置上，讓系統視需要加以建立及管理，可改善應用程式效能。 如果您需要進一步控制焦點視覺效果，您可以覆寫系統行為，並提供一個定義焦點視覺效果的自訂控制項範本。 如需詳細資訊，請參閱 [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) 和 [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074)。</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>在通用 Windows app 中，[<strong>PasswordRevealMode</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890867) 屬性會取代 [<strong>IsPasswordRevealButtonEnabled</strong>] (https://msdn.microsoft.com/library/windows/apps/hh702579) 屬性，以在各個裝置系列間提供一致的行為。</p>
<p></p>
<div class="alert">
<strong>注意：</strong>在 Windows 10 之前，依預設不會顯示密碼顯示按鈕；在通用 Windows app 中則預設為顯示。 如果您的 app 基於安全性要求一律隱藏密碼，請務必將 [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) 設為 [隱藏]。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">可在 Windows Phone 8.1 上使用的 [<strong>IsTextScaleFactorEnabled</strong>] (https://msdn.microsoft.com/library/windows/apps/dn624997) 屬性，現在已可在所有裝置系列的通用 Windows app 上使用。</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">Windows Phone 8.1 中的 [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874) 控制項現在已可在所有裝置系列的通用 Windows app 上使用，且您應使用它來取代 [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771)。 <strong>AutoSuggestBox</strong> 會在使用者輸入時提供建議，並且可與各種不同的輸入類型搭配運作，例如觸控、鍵盤及輸入法等。 它也有一些新成員，使其成為更理想的搜尋方塊：[<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494) 屬性和 [<strong>QuerySubmitted</strong>](https://msdn.microsoft.com/library/windows/apps/dn996497) 事件。</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">Windows Phone 8.1 中的 [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972) 控制項現在已可在所有裝置系列的通用 Windows app 上使用。 <strong>ContentDialog</strong> 可讓您顯示可在各類裝置上順暢運作的可自訂強制回應對話方塊。</td>
</tr>
<tr class="odd">
<td align="left">樞紐分析</td>
<td align="left">Windows Phone 8.1 中的 [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) 控制項現在已可在所有裝置系列的通用 Windows app 上使用。 現在，您在行動裝置和桌面裝置的應用程式中，可以使用相同的<strong>樞紐分析</strong>控制項。 <strong>樞紐分析</strong>可根據螢幕大小和輸入類型提供可調整的行為。 您可以設定<strong>樞紐分析</strong>的樣式以提供類似索引標籤的行為，讓每個樞紐分析項目包含不同的資訊檢視。</td>
</tr>
</tbody>
</table>

 

## 文字


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Windows 核心文字 API</td>
<td align="left"><p>新的 [<strong>Windows.UI.Text.Core</strong>] (https://msdn.microsoft.com/library/windows/apps/dn958238) 命名空間具有一個主從式系統，可集中處理對單一伺服器的鍵盤輸入。</p>
<p>您可以使用它來操作自訂文字輸入控制項的編輯緩衝區。 文字輸入伺服器可確保文字輸入控制項的內容及其本身編輯緩衝區的內容，一律可透過應用程式與伺服器之間的非同步通訊通道保持同步。</p></td>
</tr>
<tr class="even">
<td align="left">向量圖示</td>
<td align="left">[<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921) 元素有新的 [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468) 和 [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466) 屬性可支援彩色字型；現在，您可以使用字型檔案來轉譯以字型為基礎的圖示。 當您使用 <strong>ColorFontPalleteIndex</strong> 進行調色盤切換時，單一圖示將可使用不同的色彩集來轉譯；例如，用來顯示圖示的啟用和停用版本。</td>
</tr>
<tr class="odd">
<td align="left">輸入法編輯器視窗事件</td>
<td align="left">使用者有時會透過在文字輸入方塊正下方的視窗中顯示的輸入法編輯器來輸入文字 (通常用於東亞語言)。 您可以使用 [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144) 事件以及 [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 上的 [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145) 屬性，使您的應用程式 UI 更順暢地與 IME 視窗搭配運作。</td>
</tr>
<tr class="even">
<td align="left">文字組合事件</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) 和 [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 有新的事件可在使用輸入法編輯器編譯文字時通知您的應用程式：[<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458)、[<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457) 和 [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456)。 您可以處理這些事件，以協調您的應用程式程式碼與 IME 文字編譯程序。 例如，您還可以為東亞語言實作內嵌的自動完成功能。</td>
</tr>
<tr class="odd">
<td align="left">改良的雙向文字處理</td>
<td align="left"><p>XAML 文字控制項有新的 API 可改善雙向文字處理效能，使各種不同的輸入語言都能有更理想的文字對齊和段落方向性。</p>
<p>[<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133) 屬性的預設值已變更為 <strong>DetectFromContent</strong>，因此依預設會啟用偵測閱讀順序的支援。 <strong>TextReadingOrder</strong> 屬性也已新增至 [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519)、[<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) 和 [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683)。</p>
<p>您可以將文字控制項上的 [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703) 屬性設為新的 <strong>DetectFromContent</strong> 值，以選擇啟用從內容中自動偵測對齊方式。</p></td>
</tr>
<tr class="even">
<td align="left">文字轉譯</td>
<td align="left">在 Windows 10 中，XAML app 中的文字目前在大多數的情況下能以幾近 Windows 8.1 的兩倍速度進行轉譯。 在大部分的情況下，您的 app 不需要任何變更即可受益於這項改良功能。 除了更快的轉譯速度外，這些改進功能也可將 XAML app 的一般記憶體使用量降低 5%。</td>
</tr>
</tbody>
</table>

 

## 應用程式模型


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>使用可在外部應用程式中啟動及執行單一動作的語音命令，來延伸 <strong>Cortana</strong> 的基本功能。</p>
<p>整合應用程式的基本功能，並提供一個中心進入點，讓使用者不需要開啟應用程式就能完成大部分的工作，使 <strong>Cortana</strong> 成為您的應用程式與使用者之間的橋樑。 在多數情況下，這可以為使用者節省很多時間和精力。</p>
<p>了解如何 [integrate your app into the Cortana canvas](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230)。 如果您需要相關概念，可以參考 [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics) 中有關於 <strong>Cortana</strong> 的特定設計建議與 UX 指導方針。</p></td>
</tr>
<tr class="even">
<td align="left">檔案總管</td>
<td align="left">新的 [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616) 的方法可讓您啟動檔案總管，並顯示您指定的資料夾內容。</td>
</tr>
<tr class="odd">
<td align="left">共用存放裝置</td>
<td align="left">新的 [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985) 的類別及其方法可讓您在使用「URI 啟動」啟動其他應用程式時傳遞共用權杖，讓其他應用程式共用您的檔案。 目標應用程式會兌換權杖，以取得來源應用程式的共用檔案。</td>
</tr>
<tr class="even">
<td align="left">設定</td>
<td align="left"><p>使用 ms-settings 通訊協定與 [<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476) 方法，以顯示內建的設定頁面。 例如，下列程式碼會顯示 Wi-Fi 設定的頁面。</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>如需您可以顯示的設定頁面清單，請參閱[How to display built-in settings pages by using the ms-settings protocol](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx)。</p></td>
</tr>
<tr class="odd">
<td align="left">App 間通訊</td>
<td align="left"><p>Windows 10 中新的 [app-to-app communication](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) API 可讓 Windows 應用程式 (以及 Windows Web 應用程式) 能夠相互啟動並交換資料與檔案。</p>
<p>使用這些新的 API，就能流暢地立即處理需要使用者使用多個應用程式的複雜工作。 例如，您的應用程式可以啟動社交網路 app 來選擇一位連絡人，或啟動結帳應用程式來完成付款程序。</p></td>
</tr>
<tr class="even">
<td align="left">app 服務</td>
<td align="left">app 服務是 app 對 Windows 10 中的其他app 提供服務的一項途徑。 app 服務會採用背景工作的形式執行。 前景應用程式可呼叫另一個 app 中的 app 服務，以在背景中執行工作。 如需應用程式服務 API 的參考資訊，請參閱 [<strong>Windows.ApplicationModel.AppService</strong>](https://msdn.microsoft.com/library/windows/apps/dn921731)。</td>
</tr>
<tr class="odd">
<td align="left">應用程式套件資訊清單</td>
<td align="left"><p>對 Windows 10 的 [package manifest schema](https://msdn.microsoft.com/library/windows/apps/br211474) 參考所做的更新，包括已新增、移除和變更的元素。</p>
<p>如需結構描述中的所有元素、屬性和類型的參考資訊，請參閱[Element Hierarchy](https://msdn.microsoft.com/library/windows/apps/dn934819)。</p></td>
</tr>
</tbody>
</table>

 

## 裝置


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>Microsoft Surface Hub 是功能強大的小組共同作業裝置和大型螢幕平台，適用於從 Surface Hub 或是您已連接的裝置原生執行的通用 Windows app。</p>
<p>您可以根據本身的商業需求建置量身打造的 app，充分利用大型螢幕、觸控和筆跡輸入，和大量上架硬體，例如相機和感應器。</p>
<p>請參考 [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics) 中有關於 Surface Hub 的特定設計建議與 UX 指導方針。 這些文件說明通用 Windows app 的回應式設計技術。</p>
<p>如需支援公共共用應用程式的詳細資訊，請參閱 [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019)。</p>
<p>如需筆跡輸入的相關資訊，以及在新的 [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) 控制項上支援多點筆跡的詳細資訊，請參閱 [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) 和 [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452)。</p>
<p>如需處理感應器輸入的資訊，請參閱[Integrating devices, printers, and sensors](https://msdn.microsoft.com/library/windows/apps/br229563)。</p></td>
</tr>
<tr class="even">
<td align="left">位置</td>
<td align="left"><p>Windows 10 導入了新方法，用以提示使用者要求存取其位置的權限，即 [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152)。</p>
<p>使用者可以使用 [設定]<strong></strong> 應用程式中的 [位置隱私權設定]<strong></strong> 來設定其位置資料的隱私權。 您的應用程式只能在下列情況下存取使用者的位置：</p>
<ul>
<li><strong>[此裝置的位置]</strong>已開啟 (不適用於 Windows 10 for Phone)。</li>
<li>位置服務設定 [位置]<strong></strong> 已 [開啟]<strong></strong>。</li>
<li>在 [選擇可以使用您的位置的應用程式]<strong></strong> 底下，將您的應用程式設為 [開啟]<strong></strong>。</li>
</ul>
<p>在存取使用者的位置之前，務必要呼叫 [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152)。 此時，您的 app 必須在前景，且 <strong>RequestAccessAsync</strong> 必須是從 UI 執行緒呼叫。 在使用者授與您的應用程式存取其位置的權限之前，您的應用程式將無法存取位置資料。</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>[<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) Windows 執行階段命名空間導入了 Microsoft 的 AllJoyn 開放原始碼軟體架構和服務的實作。 這些 API 可讓您的通用 Windows 裝置應用程式能夠參與 AllJoyn 驅動物聯網 (IoT) 案例中的其他裝置。 如需 AllJoyn C API 的詳細資訊，請在 [The AllSeen Alliance](https://allseenalliance.org/) 下載文件。</p>
<p>使用包含在此版本中的 [AllJoynCodeGen tool](https://msdn.microsoft.com/library/windows/apps/dn913809)，可產生能夠用來在您的裝置應用程式中啟用 AllJoyn 案例的 Windows 元件。</p>
<div class="alert">
<strong>注意</strong>：Windows 10 IoT 核心版現在已可用於小型裝置的新類別，讓您使用 Windows 和 Visual Studio 建立「物聯網」(IoT) 裝置。 在 [WindowsOnDevices.com](http://www.windowsondevices.com/) 深入了解 Windows。
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">在行動裝置 (XAML) 上列印 API</td>
<td align="left">還有一組單一且整合的 API，可讓您跨裝置系列從您的 XAML 型 UWP 應用程式進行列印，包括行動裝置。 現在，您可以使用熟悉的列印相關 API，從 [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489) 和 [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325) 命名空間新增列印到您的行動應用程式。</td>
</tr>
<tr class="odd">
<td align="left">電池</td>
<td align="left"><p>電池 Api 在 [<strong>Windows.Devices.Power</strong>] (https://msdn.microsoft.com/library/windows/apps/dn895017) 命名空間可讓您的應用程式深入了解任何與執行應用程式的裝置連接的電池。</p>
<p>建立一個 [<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004) 物件以代表個別的電池控制項，或彙總所有的電池控制項 (分別由 [<strong>FromIdAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn895013) 或 [<strong>AggregateBattery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895011) 所建立時)。</p>
<p>使用 [<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015) 方法傳回 [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005) 物件，以指出對應電池的充電、容量和狀態。</p></td>
</tr>
<tr class="even">
<td align="left">MIDI 裝置</td>
<td align="left"><p>新的 [<strong>Windows.Devices.Midi</strong>](https://msdn.microsoft.com/library/windows/apps/dn895002) 命名空間可讓您建立︰</p>
<ul>
<li>可以與外部 MIDI 裝置通訊的應用程式。</li>
<li>直接與 Microsoft GS MIDI 軟體合成器通訊的應用程式與外部裝置。</li>
<li>多個用戶端同時存取單一 MIDI 連接埠的案例。</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">自訂感應器支援</td>
<td align="left">[<strong>Windows.Devices.Sensors.Custom</strong>](https://msdn.microsoft.com/library/windows/apps/dn895032) 命名空間可讓硬體開發人員定義新的自訂感應器類型，例如 CO2 感應器。</td>
</tr>
<tr class="even">
<td align="left">主機卡模擬 (HCE)</td>
<td align="left"><p>主機卡模擬可讓您實作在作業系統中裝載的 NFC 卡模擬服務，同時仍然能夠透過 NFC 無線電波與外部讀取器終端機通訊。</p>
<p>實作背景工作，以透過 NFC 模擬智慧卡。 若要觸發背景工作，請使用 [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853) 類別。</p>
<p>[<strong>SmartCardTriggerType</strong>](https://msdn.microsoft.com/library/windows/apps/dn608017) 列舉中的 <strong>EmulatorHostApplicationActivated</strong> 值可讓您的應用程式得知已發生 HCE 事件。</p></td>
</tr>
</tbody>
</table>

 

## 圖形


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | Windows 10 中的 DirectX 12 導入了下一版的 Microsoft Direct3D，也就是 DirectX 核心的 3D 圖形 API。 [Direct3D 12 圖形](https://msdn.microsoft.com/library/windows/desktop/dn903821)可支援低層級、類似主控台 API 的效率與效能。 Direct3D 12 速度較快，且比以往更有效率。 它可支援更豐富的場景、更多的物件、更複雜的效果，並能夠充分使用現代化的圖形硬體。                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | 在通用 Windows app 中，您可以使用新的 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) 類型做為 XAML 影像來源。 這可讓您將為編碼的影像傳至 XAML 架構以立即顯示於畫面上，而略過 XAML 架構所執行的影像解碼。 您可以達到更快速的影像轉譯，例如，直接從相機轉譯低延遲相片、使用自訂影像解碼器、擷取來自 DirectX 表面的框架，或甚至從頭開始建立記憶體中的影像，並使用低延遲和低記憶體負荷直接在 XAML 中全部加以轉譯。                                                                                                     |
| 透視相機   | 在通用 Windows 應用程式中，XAML 有新的 Transform3D API 可讓您將透視轉換套用到 XAML 樹狀結構 (或場景)，這會根據該單一全場景轉換 (或相機) 轉換所有的 XAML 子元素。 您可以使用 MatrixTransform 和複雜的數學運算執行此作業，但 Transform3D 可大幅簡化此效果，同時支援要動畫呈現的效果。 如需詳細資訊，請參閱 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919) 屬性、[**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748)、[**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) 和 [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740)。 |

 

## 媒體


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">HTTP 即時資料流</td>
<td align="left">您可以使用新的 [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912) 類別，將彈性視訊資料流功能新增到您的應用程式。 將物件指向資料流資訊清單檔案，物件即會初始化。 支援的資訊清單格式包括 HTTP 即時資料流 (HLS) 與透過 HTTP 的動態彈性資料流 (DASH)。 一旦物件繫結至 XAML 媒體元素，彈性播放隨即開始。 資料流的屬性 (例如可用、最小和最大位元速率) 可供查詢，並在適當情況下設定。</td>
</tr>
<tr class="even">
<td align="left">媒體基礎轉換 (MFT) 的媒體基礎轉碼視訊處理器 (XVP) 支援</td>
<td align="left"><p>使用媒體基礎轉換 (MFT) 的 Windows 應用程式現在可以使用<strong>媒體基礎轉碼視訊處理器</strong> (XVP) 來轉換、縮放以及轉換原始視訊資料︰</p>
<p>新的 [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919) 屬性即使在 Microsoft DirectX 視訊加速 (DXVA) 模式中，也可讓您輸出至呼叫者配置的紋理。</p>
<p>新的 [<strong>IMFVideoProcessorControl2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn800741) 介面可讓您的應用程式支援硬體效果、查詢支援的硬體效果，以及覆寫由視訊處理器執行的旋轉操作。</p></td>
</tr>
<tr class="odd">
<td align="left">轉碼</td>
<td align="left">新的 [<strong>MediaProcessingTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn806005) API 可讓您的應用程式在背景工作中執行媒體轉碼，讓您的轉碼操作即便在前景應用程式已終止時仍可繼續執行。</td>
</tr>
<tr class="even">
<td align="left">MediaElement 媒體失敗事件</td>
<td align="left">在通用 Windows app 中，即使在解碼其中一個串流時發生錯誤，[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 仍將播放包含多個串流的內容，只要媒體內容至少包含一個有效的串流即可。 例如，如果包含音訊和視訊串流之內容中的視訊串流失敗，<strong>MediaElement</strong> 仍然會播放音訊串流。 [<strong>PartialMediaFailureDetected</strong>](https://msdn.microsoft.com/library/windows/apps/dn889635) 事件會通知您串流內有其中一個串流無法解碼。 它也可以讓您了解何種類型的串流失敗，讓您可在 UI 中反映該項資訊。 如果媒體串流內的所有串流皆失敗，將會引發 [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393) 事件。</td>
</tr>
<tr class="odd">
<td align="left">使用 MediaElement 的彈性視訊資料流的支援</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 有新的 [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085) 方法可支援彈性視訊資料流。 您可以使用此方法將媒體來源設為 [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912)。</td>
</tr>
<tr class="even">
<td align="left">使用 MediaElement 和影像進行轉型</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) 和 [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752) 控制項有新的 [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012) 方法。 您可以使用此方法，以程式設計方式將內容從任何媒體或影像元素傳送至更多樣化的遠端裝置，例如 Miracast、藍牙及 DLNA 等。當您在 <strong>MediaElement</strong> 上將 [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977) 設為 true 時，就會自動啟用此功能。</td>
</tr>
<tr class="odd">
<td align="left">傳統型應用程式的媒體傳輸控制項</td>
<td align="left">[<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299) 介面與相關 API 可讓傳統型應用程式與內建的系統媒體傳輸控制項互動。 這包括回應使用者與傳輸控制項按鈕的互動，以及更新傳輸控制項顯示畫面，以顯示與目前播放的媒體內容相關的中繼資料。</td>
</tr>
<tr class="even">
<td align="left">隨機存取 JPEG 編碼和解碼</td>
<td align="left">新的 WIC 方法 [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) 與 [<strong>IWICJpegFrameDecode</strong>] (https://msdn.microsoft.com/library/windows/desktop/dn903834) 可啟用 JPEG 影像的編碼和解碼。 現在，您也可以啟用影像資料的索引編製，以使用較大量的記憶體換取對大型影像有效率的隨機存取。</td>
</tr>
<tr class="odd">
<td align="left">媒體組合的重疊</td>
<td align="left">新的 [<strong>MediaOverlay</strong>](https://msdn.microsoft.com/library/windows/apps/dn764793) 與 [<strong>MediaOverlayLayer</strong>](https://msdn.microsoft.com/library/windows/apps/dn764795) API 輕易地將多個層級的靜態或動態媒體內容新增至媒體組合。 您可以對每個層級調整不透明度、位置和時間，甚至可以實作您自己為輸入層自訂的撰寫器。</td>
</tr>
<tr class="even">
<td align="left">新的效果架構</td>
<td align="left">[<strong>Windows.Media.Effects</strong>](https://msdn.microsoft.com/library/windows/apps/dn278802) 命名空間提供了簡單且直覺的架構，用以將效果新增至音訊和視訊串流。 此架構包含可讓您實作以建立自訂音訊與視訊效果並將其插入到媒體管線中的基本介面。</td>
</tr>
</tbody>
</table>

 

## 網路功能


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">通訊端</td>
<td align="left"><p>通訊端更新包括：</p>
<ul>
<li><strong>通訊端代理程式。</strong> 通訊端代理程式可代表在應用程式生命週期中處於任何狀態的應用程式建立及關閉通訊端連線。 這會讓應用程式和它們所提供的服務更易於探索。 例如，透過通訊端代理程式，Win32 服務即使在未執行時，仍然可以接受連入的通訊端連線。</li>
<li><strong>輸送量的改進。</strong> 通訊端輸送量已針對使用 [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960) 命名空間的應用程式最佳化。</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">背景傳輸的後續處理工作</td>
<td align="left">[<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242) 命名空間中的新 API 可讓您登錄後續處理工作的群組。 您的應用程式可以在背景傳輸成功或失敗時隨即採取動作，而不會等待使用者下一次繼續執行應用程式，即使不在前景中亦然。</td>
</tr>
<tr class="odd">
<td align="left">藍牙對廣告的支援</td>
<td align="left">透過 [<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325) 命名空間，您的應用程式可以傳送、接收和篩選藍牙 LE 廣告。</td>
</tr>
<tr class="even">
<td align="left">Wi-Fi Direct API 更新</td>
<td align="left"><p>裝置代理程式會進行更新，使裝置不需要離開應用程式即可配對。 對 [<strong>Windows.Devices.WiFiDirect</strong>] (https://msdn.microsoft.com/library/windows/apps/dn297687) 命名空間進行新增後，裝置也因此讓本身能夠讓其他裝置探索到，並讓它接聽連入的連線通知。</p>
<div class="alert">
<strong>請注意</strong>：在此版本中，Wi-Fi Direct 增強功能並未內建於 UX 中，並且僅支援按鈕配對。 此外，此版本僅支援一個使用中連線。
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">JSON 支援的改進</td>
<td align="left">[<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639) 命名空間現在已可在偵錯工作階段中轉換 JSON 物件時進一步支援現有的標準定義和開發人員經驗。</td>
</tr>
</tbody>
</table>

 

## 安全性


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| ECC 加密              | [
            **Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) 命名空間中的新 API 可支援橢圓曲線密碼編譯 (ECC)，這是以有限欄位的橢圓曲線為基礎的公開金鑰密碼編譯實作。 ECC 的演算方式比 RSA 更複雜，所提供的金鑰較小、可降低記憶體使用量，並改善效能。 它可為 Microsoft 服務和客戶提供 RSA 金鑰與 NIST 核准之曲線參數的替代方法。 |
| Microsoft Passport          | Microsoft Passport 是一種使用非對稱式密碼編譯和手勢來取代密碼的替代驗證方法。 [
            **認證**](https://msdn.microsoft.com/library/windows/apps/br227089)命名空間中的類別 (例如 [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043)) 可讓開發人員使用 Microsoft Passport 輕鬆地建立應用程式，而迴避掉複雜的密碼編譯或生物特徵辨識技術。  |
| Microsoft Passport for Work | Microsoft Passport for Work 是一種登入 Windows 的替代方法，它會使用您未使用密碼、智慧卡和虛擬智慧卡的 Active Directory 帳戶。 您可以選擇要停用還是啟用此原則設定。 |
| 權杖代理人                | 權杖代理人是一種新的驗證架構，可讓應用程式輕易地連接到線上身分識別提供者 （如 Facebook)。 諸如帳戶使用者名稱和密碼管理和精簡型 UI 等功能，可大幅改善使用者的驗證經驗。 |

 

## 系統服務


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">電源</td>
<td align="left"><p>現在，在啟用或停用省電模式時，您的 Windows 傳統型應用程式將可收到通知。 藉由回應變動的電力情況，您的應用程式有機會延長電池使用時間。</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380)：使用這個新的 GUID 與 [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082) 函式，可在啟用或停用省電模式時收到通知。</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232)：此結構已更新而可支援省電模式。 第四個成員 <strong>SystemStatusFlag</strong> (先前名為 Reserved1) 現在可指出省電模式是否已啟用。 使用 [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693) 函式可擷取此結構的指標。</p></td>
</tr>
<tr class="even">
<td align="left">版本</td>
<td align="left"><p>您可以使用 [Version Helper functions](https://msdn.microsoft.com/library/windows/desktop/dn424972) 來確認作業系統的版本。 在 Windows 10 中，這些 Helper 函式包含新的函式 [<strong>IsWindows10OrGreater</strong>](https://msdn.microsoft.com/library/windows/desktop/dn905474)。 當您想要確認系統版本時，您應使用 Helper 函式，而不是已過時的 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 和 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 函式。 如需如何取得系統版本的詳細資訊，請參閱[Getting the System Version](https://msdn.microsoft.com/library/windows/desktop/ms724429)。</p>
<p>如果您使用已過時的 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 或 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 函式來取得 [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833) 或 [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834) 結構中的版本資訊，請留意這些結構所包含的版本號碼已從 Windows 8.1 和 Windows Server 2012 R2 的 6.3 增加到 Windows 10 的 10.0。 如需關於作業系統版本號碼的詳細資訊，請參閱[Operating System Version](https://msdn.microsoft.com/library/windows/desktop/ms724832)。</p>
<p>您也必須明確地在應用程式中將 Windows 8.1 或 Windows 10 定為目標，才能透過 [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) 或 [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) 函式取得這些版本的正確版本資訊。 如需如何在您的應用程式中將這些 Windows 版本定為目標的相關資訊，請參閱[Targeting your application for Windows](https://msdn.microsoft.com/library/windows/desktop/dn481241)。</p></td>
</tr>
<tr class="odd">
<td align="left">使用者資訊</td>
<td align="left">[<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) 命名空間中的新 API 可讓您輕鬆存取使用者的相關資訊，例如其使用者名稱和帳戶圖片。 它也可讓您回應登入和登出之類的使用者事件。</td>
</tr>
<tr class="even">
<td align="left">記憶體管理和分析</td>
<td align="left">[<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) 中的記憶體分析 API 支援已延伸至所有平台，且其整體功能已使用新的類別和函式進行增強。</td>
</tr>
</tbody>
</table>

 

## 存放裝置


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">適用於 Windows Phone 的檔案搜尋 API</td>
<td align="left"><p>身為應用程式發佈者，您可以藉由在應用程式資訊清單中新增擴充功能，以登錄您的應用程式，使其與您發佈的其他應用程式共用存放裝置資料夾。 然後，呼叫 [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>](https://msdn.microsoft.com/library/windows/apps/dn889607) 方法以取得共用的儲存位置。</p>
<p>Windows 執行階段應用程式的強式安全性模型通常會防止應用程式相互共用資料。 但是，以個別使用者為基礎，讓來自於相同發佈者的應用程式共用檔案和設定，可能會幫助。</p></td>
</tr>
</tbody>
</table>

 

## 工具


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Visual Studio 中的即時視覺化樹狀結構</td>
<td align="left">Visual Studio 具有新的即時視覺化樹狀結構功能。 您可以在偵錯時使用此功能，以快速了解應用程式的視覺化樹狀結構的狀態，並探索元素屬性的設定方式。 它也可讓您在應用程式執行時變更屬性值，而可逕行調整和實驗而無需重新啟動。</td>
</tr>
<tr class="even">
<td align="left">追蹤記錄</td>
<td align="left"><p>[
            TraceLogging](https://msdn.microsoft.com/library/windows/desktop/dn904636) 是使用者模式應用程式與核心模式驅動程式新的事件追蹤 API；其建置基礎為 [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803) (ETW)。 此 API 有簡化的方式可供您檢測程式碼以及納入具有事件的結構化資料，而不需要個別的檢測資訊清單 XML 檔案。</p>
<p>WinRT、.NET 和 C/C++ TraceLogging API 可用來服務不同的開發人員對象。</p></td>
</tr>
</tbody>
</table>

 

## 使用者經驗


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">語音辨識</td>
<td align="left">通用 Windows 平台現在可支援較長聽寫案例的連續語音辨識。 請在語音互動文件中了解如何啟用連續聽寫。</td>
</tr>
<tr class="even">
<td align="left">不同應用程式平台之間的拖放功能</td>
<td align="left"><p>新的 [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216) 命名空間為通用 Windows app 引進了拖放功能。 過去，桌面程式常見的拖放案例 (例如，將文件從資料夾中拖曳到 Outlook 電子郵件中，加以附加) 無法用於通用 Windows 應用程式。 使用這些新的 API，您的應用程式將可讓使用者輕鬆地在不同的通用 Windows app 與桌面之間移動資料。</p>
<p>為了支援應用程式之間的拖放功能，下列新的 API 已新增至 XAML：</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911)：[<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558)、[<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560)、[<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562)、[<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917)、[<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714)、[<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372)：[<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912)、[<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913)、[<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710)、[<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914)、[<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953)、[<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549)、[<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">自訂視窗標題列</td>
<td align="left">針對適用於桌面裝置系列的 UWP 應用程式，您現在可以使用 [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115) 類別、[<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131) 屬性和 [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560) 方法，將預設的 Windows 標題列內容取代為您自訂的 XAML 內容。 您的 XAML 會被視為「系統 chrome」，因此 Windows 將會處理輸入事件，而不是您的應用程式。 這表示使用者即使按一下您的自訂標題列內容，仍可拖曳視窗調整其大小。</td>
</tr>
</tbody>
</table>

 

## Web


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>Internet Explorer 導入了邊緣模式：是一個新的「即時」文件模式，其設計目的是要充分利用與其他新型瀏覽器和現代 Web 內容的互通性。 此實驗性模式正逐漸推展至隨機選擇的一組 Windows 10 使用者。 您可以透過新的 IE <strong>about:flags</strong> 機制，手動啟用或停用邊緣模式。 如需詳細資訊，請參閱：</p>
<ul>
<li>[
            Living on the Edge – our next step in helping the web just work](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx).</li>
<li>[
            The Internet Explorer for Windows 10 Developer Guide](https://dev.windows.com/microsoft-edge/).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">WebView 邊緣模式瀏覽</td>
<td align="left">[<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 控制項以相同的轉譯引擎做為新的邊緣瀏覽器。 這可提供最精確且符合標準的 HTML 轉譯模式。</td>
</tr>
<tr class="odd">
<td align="left">關閉執行緒 WebView</td>
<td align="left">您可以指定 [<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034)，以啟用在個別的背景執行緒上處理和顯示 Web 內容的功能。 這可以改善某些特定情況下的效能。</td>
</tr>
<tr class="even">
<td align="left">WebView.UnsupportedUriSchemeIdentified 事件</td>
<td align="left"><p>新的 [<strong>WebView.UnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn974400) 事件可讓您決定應用程式應如何處理不支援的 URI 配置。 您可以處理此事件，讓您的應用程式針對不支援的 URI 配置提供自訂處理方式。</p>
<p>針對 HTML WebView 控制項，請檢視 [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx) 事件。</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.NewWindowRequested 事件</td>
<td align="left"><p>新的 [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397) 事件可讓您在 WebView 中的指令碼要求新的瀏覽器視窗時予以回應。</p>
<p>針對 HTML WebView 控制項，請檢視 [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030) 事件。</p></td>
</tr>
<tr class="even">
<td align="left">WebView.PermissionRequested 事件</td>
<td align="left"><p>新的 [<strong>WebView.PermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974398) 事件可讓 WebView 內容利用豐富的新型 HTML5 API 功能；此 API 會要求使用者提供特殊權限，例如地理位置。</p>
<p>針對 HTML WebView 控制項，請檢視 [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx) 事件。</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.UnviewableContentIdentified 事件</td>
<td align="left"><p>新的 [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351) 事件可讓您在 [<strong>WebView</strong>] (https://msdn.microsoft.com/library/windows/apps/br227702) 上的非 Web 內容 (例如 PDF 檔案或 Office 文件) 被瀏覽時予以回應。</p>
<p>針對 HTML WebView 控制項，請檢視 [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716) 事件。</p></td>
</tr>
<tr class="even">
<td align="left">WebView.AddWebAllowedObject 方法</td>
<td align="left"><p>您可以呼叫新的 [<strong>WebView.AddWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn903993) 方法，將 WinRT 物件插入到 XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 中，然後裝載在該 <strong>WebView</strong> 中的受信任 JavaScript 呼叫其函式。 例如，Web 內容可藉由要求其父項應用程式呼叫 [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642) WinRT API，來顯示系統通知。</p>
<p>針對 HTML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 控制項，請檢視 [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632) 方法。</p></td>
</tr>
<tr class="odd">
<td align="left">WebView.ClearTemporaryWebDataAync 方法</td>
<td align="left">當使用者與 XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) 內的 Web 內容互動時，<strong>WebView</strong> 控制項會根據該使用者的工作階段快取資料。 您可以呼叫的新 [<strong>ClearTemporaryWebDataAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn974394) 方法來清除此快取。 例如，您可以在一位使用者登出應用程式，而使另一位使用者無法存取先前工作階段的任何資料時，將快取清除。</td>
</tr>
</tbody>
</table>



<!--HONumber=Mar16_HO5-->


