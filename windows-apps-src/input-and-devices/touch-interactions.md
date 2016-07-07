---
author: Karl-Bridge-Microsoft
Description: "使用已針對觸控最佳化，但在功能上與所有輸入裝置一致的直覺式特殊使用者互動體驗，建立通用 Windows 平台 (UWP) 應用程式。"
title: "觸控互動"
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: a78dd1030e653d1cf0a1d7f191b4768e5a99860a

---

# 觸控互動


在設計應用程式時，請先預期觸控會是使用者的主要輸入方法。 若您使用平台控制項，在觸控板、滑鼠和畫筆/手寫筆的支援方面則不需要額外的程式設計，因為 UWP app 對此提供免費支援。

不過，請記住，針對觸控最佳化的 UI 未必優於傳統 UI。 兩者對技術和應用程式而言各有優缺點。 在設計觸控優先的 UI 之前，了解觸控 (包含觸控板)、畫筆/手寫筆、滑鼠以及鍵盤輸入之間的核心差異是很重要的。

**重要 API**

-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)



許多裝置都配備多點觸控螢幕，支援使用一或多根手指 (或觸控點) 進行輸入。 觸控點及其移動方式都會解譯為觸控手勢與操作，以支援各種不同的使用者互動。

通用 Windows 平台 (UWP) 包含一些處理觸控輸入的不同機制，讓您能夠建立使用者可自信探索的沈浸式體驗。 此處將說明在 UWP app 中使用觸控輸入的基本知識。

觸控互動需要具備三個要件：

-   觸控式顯示器。
-   一或多根手指在該顯示器上直接接觸 (或鄰近性，如果顯示器具有近接感測器且支援暫留偵測)。
-   觸控接觸點的移動 (或者沒有，根據時間閾值)。

觸控感應器提供的輸入資料可以：

-   解譯為直接操作一或多個 UI 元素的實際手勢 (例如，移動瀏覽、旋轉、調整大小或移動)。 相反地，透過元素的屬性視窗、對話方塊或其他 UI 能供性進行互動，會被視為間接操作。
-   辨識為替代的輸入法，例如滑鼠或手寫筆。
-   用來補充或修改其他輸入法的層面，例如弄髒手寫筆繪製的筆跡筆觸。

觸控輸入通常會與在螢幕上直接操作元素有關。 元素會立即回應其點擊測試區域內的任何觸控點，並適當地回應觸控點的任何後續動作 (包括移除)。

請務必謹慎設計自訂的觸控手勢和互動。 它們應該是直覺式、具回應性且可供探索，而且應該能讓使用者自信地探索您的 app。

確保 app 功能會在每個支援的輸入裝置類型上以一致的方式公開。 如有需要，請使用特定形式的間接輸入模式，例如，適用於鍵盤互動的文字輸入，或者適用於滑鼠或手寫筆的 UI 能供性。

請記住，傳統的輸入裝置 (例如滑鼠和鍵盤) 對許多使用者來說都很熟悉且具吸引力。 它們可以提供觸控功能可能無法提供的速度、準確度以及觸感反應。

針對所有輸入裝置提供獨特而鮮明的互動體驗，支援的功能和喜好設定越多越好、適用的對象越廣越好，這樣將能吸引更多的客戶使用您的 app。

## 比較觸控互動需求

下表顯示在設計觸控最佳化 UWP app 時應考慮存在於輸入裝置間的一些差異性。

<table>
<tbody><tr><th>因素</th><th>觸控互動</th><th>滑鼠、鍵盤、畫筆/手寫筆互動</th><th>觸控板</th></tr>
<tr><td rowspan="3">精確度</td><td>指尖的接觸區域大於單一 x-y 座標，增加了啟動非預期命令的機會。</td><td>滑鼠和畫筆/手寫筆可以提供精確的 x-y 座標。</td><td>與滑鼠相同。</td></tr>
<tr><td>接觸區域的形狀會隨著移動而變化。  </td><td>滑鼠移動和畫筆/手寫筆筆觸可以提供精確的 x-y 座標。 鍵盤焦點很明確。</td><td>與滑鼠相同。</td></tr>
<tr><td>沒有滑鼠游標協助目標預測。</td><td>滑鼠游標、畫筆/手寫筆游標以及鍵盤焦點，都可以協助目標預測。</td><td>與滑鼠相同。</td></tr>
<tr><td rowspan="3">人體構造</td><td>指尖的移動並不精準，因為用單指或多指進行直線動作很困難。 這是因為手關節的曲度以及手指進行動作時牽涉到好幾個關節。</td><td>使用滑鼠或畫筆/手寫筆進行直線動作較為簡單，因為控制這些裝置的手實際經過的距離比畫面上的游標短。</td><td>與滑鼠相同。</td></tr>
<tr><td>顯示裝置觸控表面上的某些區域，會因為手指姿勢和使用者手持裝置的緣故而不容易觸控。</td><td>滑鼠和畫筆/手寫筆可以觸控畫面的任一部分，而鍵盤則可透過定位順序存取任一控制項。 </td><td>手指姿勢和握法可能會是個問題。</td></tr>
<tr><td>物件可能會被一或多個指尖或是使用者的手擋到。 這稱為遮蔽。</td><td>間接輸入裝置不會造成遮蔽。</td><td>與滑鼠相同。</td></tr>
<tr><td>物件狀態</td><td>觸控使用的是一種雙狀態模型：顯示裝置的觸控表面不是已觸碰 (開) 就是未觸碰 (關)。 沒有可以觸發其他視覺化回饋的暫留狀態。</td><td>
<p>滑鼠、畫筆/手寫筆以及鍵盤都會顯示三重狀態模型：放開 (關)、按下 (開) 以及暫留 (焦點)。</p>
<p>暫留可以讓使用者透過與 UI 元素關聯的工具提示進行探索和學習。 暫留和焦點效果可以轉達哪些物件是互動式，並且也可以協助目標預測。 
</p>
</td><td>與滑鼠相同。</td></tr>
<tr><td rowspan="2">多元化互動</td><td>支援多點觸控：一個觸控表面上多個輸入點 (指尖)。</td><td>支援單一輸入點。</td><td>與觸控相同。</td></tr>
<tr><td>支援透過手勢 (例如點選、拖曳、滑動、捏合以及旋轉) 直接操作物件。</td><td>不支援直接操作，因為滑鼠、畫筆/手寫筆以及鍵盤是間接輸入裝置。</td><td>與滑鼠相同。</td></tr>
</tbody></table>



**注意**  
間接輸入的優勢在於已經過 25 年以上的技術改良。 有一些功能 (例如暫留觸發工具提示) 是專門設計來解決觸控板、滑鼠、畫筆/手寫筆以及鍵盤輸入的 UI 探索問題。 這類的 UI 功能已經針對觸控輸入提供的豐富經驗而重新設計，不會犧牲這些裝置上的使用者經驗。

 

## 使用觸控回饋

與應用程式進行互動時的適當視覺化回饋，可以協助使用者辨識、學習及適應應用程式與 Windows 8 如何解譯他們的互動。 視覺化回饋可以指示互動成功、轉送系統狀態、改善控制感應、減少錯誤、協助使用者了解系統和輸入裝置，並能激發使用者互動意願。

當使用者依賴觸控輸入進行要求正確與精確位置的活動時，視覺化回饋就顯得相當重要。 隨時隨地偵測到觸控輸入時便顯示回饋，可以協助使用者了解應用程式及其控制項所定義的任何自訂目標規則。


## 目標預測

目標預測透過以下各項進行最佳化：

-   觸控目標大小

    清楚的大小指導方針，可以確定應用程式能夠提供正常的 UI，裡面包含的物件和控制項都可以輕易、放心當作目標。

-   接觸幾何

    手指的整個接觸面會判斷最可能的目標物件為何。

-   擦選

    手指在群組中的項目之間拖曳，可以輕鬆重新轉換目標 (例如，選項按鈕)。 拿起手指不觸摸之後，就會啟動目前的項目。

-   搖晃

    對於密集封裝的項目 (例如超連結)，只要手指按下、不滑動並在項目上來回搖晃，即可輕易重新選定目標。 由於遮蔽的緣故，因此會透過工具提示或狀態列來識別目前的項目，並且會在放開手指時啟動。

## 準確度

使用下列功能針對隨性互動進行設計：

-   貼齊點：在使用者與內容進行互動時，可以較容易停在想要的位置。
-   指向性「柵欄」：可在即使手以稍微弧形的方式移動時，協助進行垂直或水平移動瀏覽。 如需詳細資訊，請參閱[移動瀏覽的指導方針](guidelines-for-panning.md)。

## 遮蔽

下列方式可以避免手指與手部遮蔽問題：

-   UI 的大小和放置位置

    將 UI 元素設計得大一些，使元素不致於被指尖接觸區域完全覆蓋。

    盡可能將功能表和快顯通知置於接觸區域的上方。

-   工具提示

    當使用者的手指一直接觸物件時顯示工具提示。 這個功能很有用，能夠描述物件功能。 使用者可以拖曳指尖離開物件，避免叫用工具提示。

    對於小型物件，請讓工具提示位移，使提示不致於被指尖接觸區域覆蓋。 這對於目標預測很實用。

-   精確度控點

    在要求精確度 (例如文字選取) 的情況下，請提供位移的選取控點以提高精確度。 如需詳細資訊，請參閱[選取文字和影像的指導方針 (Windows 執行階段 app)](guidelines-for-textselection.md)。

## 計時

避免計時模式的變更有利於直接操作。 直接操作可以模擬直接、即時的實體物件控制。 物件會隨著手指移動而回應。

另一方面，計時互動是發生在觸控互動之後。 計時互動一般是根據一些隱藏的閾值 (例如時間、距離或速度) 來決定要執行的命令。 計時互動必須要等到系統執行動作之後，才會有視覺化回饋。

直接操作提供一些超越計時互動的優勢：

-   互動時的立即視覺化回饋，可以讓使用者感覺更投入、更自信以及操控自如。
-   直接操作使得探索系統時較為安全，因為這些操作是可以還原的，使用者可以藉由邏輯且直覺式的方式回到先前的動作。
-   直接影響物件的互動和虛擬實境互動比較直覺式、易搜尋、易記憶。 它們不倚賴模糊或抽象的互動。
-   計時互動可能會難以執行，因為使用者必須先達到任意且隱藏的閾值，才構成執行條件。

此外，下列是幾個強烈建議的原則：

-   不應該以使用的手指數來辨別操作行為。
-   互動應該支援複合式操作。 例如，在拖曳手指進行移動瀏覽時，透過捏合進行縮放。
-   不應該以時間來辨別互動。 不論執行時間長短，相同的互動應該產生相同的結果。 以時間為基礎的啟動會對使用者造成強制性的延遲，同時對直接操作的沈浸式性質和系統回應感知功能造成減損。

    **注意** 使用特定計時互動來協助學習和探索 (例如長按) 為例外的狀況。

     

-   適當的說明和視覺提示，對於進階互動的應用會產生很大的作用。


## App 檢視


透過 app 檢視的移動瀏覽/捲動和縮放設定，調整使用者互動方式。 應用程式檢視指示使用者如何存取並操作應用程式與其內容。 檢視也提供如慣性、內容界限彈回及貼齊點等行為。

當檢視區無法容納整個檢視內容時， [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 控制項的移動瀏覽和捲動設定可以規定使用者在單一檢視內的瀏覽方式。 例如，單一檢視可以是雜誌或書籍的其中一頁、電腦的資料夾結構、文件庫或者相簿。

縮放設定可套用至視覺化縮放 (透過 [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527) 控制項來支援) 和 [**Semantic Zoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) 控制項。 語意式縮放是一種觸控最佳化技術，適合在單一檢視中顯示及瀏覽大量的相關資料或內容。 它的運作方式是使用兩種不同的分類模式 (或縮放比例)。 這就類似在單一檢視內的移動瀏覽和捲動。 移動瀏覽和捲動可以和語意式縮放搭配使用。

您可以使用 app 檢視與事件，來修改移動瀏覽/捲動和縮放行為。 與處理指標和手勢事件相比，它可以提供更順暢的互動體驗。

如需有關 app 檢視的詳細資訊，請參閱[控制項、配置及文字](https://msdn.microsoft.com/library/windows/apps/mt228348)。

## 自訂觸控互動


如果您實作自己的互動支援，請牢記使用者所期待的是與 App UI 元素直接互動的直覺式體驗。 建議您在平台控制項程式庫上將您的自訂互動模型化，以保持一致且可探索的 UI 體驗。 這些程式庫中的控制項提供完整的使用者互動體驗，包含標準互動、動畫物理效果、視覺化回饋及協助工具。 只有在需求明確且定義清楚，而且沒有基本的互動可以支援您的情況時，才能建立自訂互動。

若要提供自訂的觸控支援，您可以處理各種不同的 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 事件。 這些事件會分成三個等級的抽象概念。

-   靜態手勢事件會在互動完成之後觸發。 手勢事件包含 [**Tapped**](https://msdn.microsoft.com/library/windows/apps/br208985)、[**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/br208922)、[**RightTapped**](https://msdn.microsoft.com/library/windows/apps/br208984) 及 [**Holding**](https://msdn.microsoft.com/library/windows/apps/br208928)。

    您可以透過將 [**IsTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208939)、[**IsDoubleTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208931)、[**IsRightTapEnabled**](https://msdn.microsoft.com/library/windows/apps/br208937) 及 [**IsHoldingEnabled**](https://msdn.microsoft.com/library/windows/apps/br208935) 設定為 **false**，來停用特定元素的手勢事件。

-   指標事件 (例如 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 和 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)) 會針對每個觸控點提供低階詳細資料，包括指標移動以及區分按下和放開事件的能力。

    指標是含有統一事件機制的泛型輸入類型。 它會公開作用中輸入來源 (觸控、觸控板、滑鼠或手寫筆) 的基本資訊 (例如螢幕位置)。

-   操作手勢事件 (例如 [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950)) 會指出進行中的互動。 當使用者觸控元素並繼續進行，直到該使用者舉起手指或操作取消為止，就會開始觸發它們。

    操作事件包括多點觸控互動 (例如縮放、移動瀏覽或旋轉)，以及使用慣性和速度資料的互動 (例如拖曳)。 操作事件所提供的資訊不會識別已執行的互動形式，而是會包括像是位置、平移量及速度等資料。 您可以使用這個觸控資料來判斷應該執行的互動類型。

以下提供 UWP 支援的基本觸控手勢組合。

| 名稱           | 類型                 | 說明                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| 點選            | 靜態手勢       | 一根手指觸碰螢幕後提起手指。                                            |
| 長按 | 靜態手勢       | 一根手指觸碰螢幕後停在原地。                                      |
| 滑動          | 操作手勢 | 一或多根手指觸碰螢幕後，再往同一個方向移動。                   |
| 撥動          | 操作手勢 | 一或多根手指觸碰螢幕後，再往同一個方向短距離移動。  |
| 轉動           | 操作手勢 | 二或多根手指輕觸螢幕後，往順時鐘或逆時鐘方向弧形移動。 |
| 捏合          | 操作手勢 | 二或多根手指觸碰螢幕後，再朝靠攏的方向移動。                         |
| 伸展        | 操作手勢 | 二或多根手指觸碰螢幕後，再朝分開的方向移動。                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## 手勢事件


如需個別控制項的詳細資訊，請參閱[控制項清單](https://msdn.microsoft.com/library/windows/apps/mt185406)。

## 指標事件


指標事件是透過各種不同的作用中輸入來源所引發，包括觸控、觸控板、手寫筆和滑鼠 (它們會取代傳統的滑鼠事件)。

指標事件的基礎是單一輸入點 (手指、畫筆筆尖、滑鼠游標)，而且不支援以速度為基礎的互動。

以下是指標事件及其相關事件引數的清單。

| 事件或類別                                                       | 說明                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)             | 單指觸碰螢幕時就會發生。               |
| [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972)           | 在相同的觸控點上提起時就會發生。                |
| [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970)                 | 在螢幕上拖曳指標時就會發生。         |
| [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968)             | 當指標進入元素的界限區域時就會發生。 |
| [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969)               | 當指標離開元素的界限區域時就會發生。  |
| [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964)           | 當觸控點異常遺失時就會發生。               |
| [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)     | 若有另一個元素採用指標擷取時就會發生。    |
| [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973)   | 當滑鼠滾輪的差異值變更時發生。         |
| [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) | 提供所有指標事件的資料。                         |

 

下列範例示範如何使用 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)、[**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 及 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件來處理 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) 物件上的點選互動。

首先，在 Extensible Application Markup Language (XAML) 中建立名為 `touchRectangle` 的 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Height="100" Width="200" Fill="Blue" />
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```

接下來，指定 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971)、[**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 及 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件的接聽程式。

```ManagedCPlusPlus
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &amp;MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &amp;MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &amp;MainPage::touchRectangle_PointerExited);
}
```

```CSharp
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```VisualBasic
Public Sub New()

    &#39; This call is required by the designer.
    InitializeComponent()

    &#39; Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

最後，當 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) 和 [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) 事件處理常式再次將 **Height** 和 **Width** 設定為它們的開始值時，[**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) 事件處理常式會提高 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) 的 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751)。

```ManagedCPlusPlus
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```CSharp
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```VisualBasic
&#39; Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    &#39; Pointer moved outside Rectangle hit test area.
    &#39; Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

&#39; Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    &#39; Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

&#39; Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    &#39; Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## 操作事件


如果您的 app 中需要支援多指互動或是使用速度資料的互動，請使用操作事件。

您可以使用操作事件來偵測互動 (例如拖曳、縮放及按住不放)。

以下是操作事件及其相關事件引數的清單。

| 事件或類別                                                                                               | 說明                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**ManipulationStarting 事件**](https://msdn.microsoft.com/library/windows/apps/br208951)                                   | 第一次建立操作處理器時發生。                                                                                  |
| [**ManipulationStarted 事件**](https://msdn.microsoft.com/library/windows/apps/br208950)                                     | 當輸入裝置開始在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 進行操作時發生。                                            |
| [**ManipulationDelta 事件**](https://msdn.microsoft.com/library/windows/apps/br208946)                                         | 當輸入裝置在操作期間變更位置時發生。                                                                      |
| [**ManipulationInertiaStarting 事件**](https://msdn.microsoft.com/library/windows/apps/hh702425)                | 在操作和慣性開始的時候，只要輸入裝置不與 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 物件接觸便發生。 |
| [**ManipulationCompleted 事件**](https://msdn.microsoft.com/library/windows/apps/br208945)                                 | 當在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 進行的操作和慣性互動完成時發生。                                          |
| [**ManipulationStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702132)               | 提供 [**ManipulationStarting**](https://msdn.microsoft.com/library/windows/apps/br208951) 事件的資料。                                         |
| [**ManipulationStartedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702101)                 | 提供 [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 事件的資料。                                           |
| [**ManipulationDeltaRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702051)                     | 提供 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件的資料。                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702074) | 提供 [**ManipulationInertiaStarting**](https://msdn.microsoft.com/library/windows/apps/br208947) 事件的資料。                           |
| [**ManipulationVelocities**](https://msdn.microsoft.com/library/windows/apps/br242032)                                              | 描述發生操作的速度。                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh702035)             | 提供 [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) 事件的資料。                                       |

 

手勢是由一系列操作事件所組成。 每個手勢都是以 [**ManipulationStarted**](https://msdn.microsoft.com/library/windows/apps/br208950) 事件開始，例如當使用者觸碰螢幕。

接著觸發一或多個 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件。 例如，當您觸碰螢幕，然後將手指劃過螢幕時。 最後，當互動完成時，就會引發 [**ManipulationCompleted**](https://msdn.microsoft.com/library/windows/apps/br208945) 事件。

**注意：**如果您沒有觸控式螢幕監視器，可以在使用滑鼠和滑鼠滾輪介面的模擬器中測試您的操作事件程式碼。

 

下列範例示範如何使用 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件來處理 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) 上的滑動互動，並在螢幕上移動它。

首先，在 XAML 中建立了一個名為 `touchRectangle` 且 [**Height**](https://msdn.microsoft.com/library/windows/apps/br208718) 和 [**Width**](https://msdn.microsoft.com/library/windows/apps/br208751) 為 200 的 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)。

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Width="200" Height="200" Fill="Blue" 
           ManipulationMode="All"/>
</Grid>
```

接著，建立一個名為 `dragTranslation` 的全域 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027) 來轉譯 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371)。 在 **Rectangle** 上指定 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件接聽程式，並將 `dragTranslation` 新增到 **Rectangle** 的 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980)。

```ManagedCPlusPlus
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```CSharp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```VisualBasic
&#39; Global translation transform used for changing the position of 
&#39; the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```ManagedCPlusPlus
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &amp;MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```CSharp
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```VisualBasic
Public Sub New()

    &#39; This call is required by the designer.
    InitializeComponent()

    &#39; Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    &#39; New translation transform populated in 
    &#39; the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    &#39; Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

最後，在 [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/br208946) 事件處理常式中，使用 [**Delta**](https://msdn.microsoft.com/library/windows/apps/hh702058) 屬性上的 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/br243027) 來更新 [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371) 的位置。

```ManagedCPlusPlus
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```CSharp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```VisualBasic
&#39; Handler for the ManipulationDelta event.
&#39; ManipulationDelta data Is loaded into the
&#39; translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    &#39; Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## 路由事件


本文中所述的所有指標事件、手勢事件和操控事件都會做為「路由事件」**來實作。 這表示事件除了可由最初引發事件的物件處理外，還能由其他物件來處理。 即使原始元素未處理事件，物件樹狀目錄中的後續父項 (例如 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 的父容器或 app 的根 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503)) 也能選擇處理這些事件。 相反地，任何實際處理事件的物件可以將事件標示為已處理，如此一來就不會到達任何父元素。 如需有關路由事件概念以及這會如何影響您撰寫路由事件處理常式的詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/hh758286)。

## 可行與禁止事項


-   設計以觸控互動做為主要預期輸入方法的應用程式。
-   為所有類型的互動 (觸控、畫筆、手寫筆、滑鼠等等) 提供視覺回饋。
-   調整觸控目標大小、接觸幾何、擦選和搖晃來最佳化目標定位。
-   使用對齊點和方向性「柵欄」來最佳化正確性。
-   提供工具提示和控點，協助改進緊密組合的 UI 項目的觸控正確性。
-   如有可能，不要使用計時互動 (適當用法範例：觸碰並按住)。
-   如有可能，不要使用手指數目來辨別操作。


## 相關文章

* [處理指標輸入](handle-pointer-input.md)
* [識別輸入裝置](identify-input-devices.md) 
           **範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [使用者互動模式範例](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895) 
           **封存範例**
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：XAML 使用者輸入事件範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [XAML 捲動、移動瀏覽和縮放範例](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [輸入：使用 GestureRecognizer 處理手勢與操作](http://go.microsoft.com/fwlink/p/?LinkID=231605)
 

 







<!--HONumber=Jun16_HO5-->


