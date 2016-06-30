---
author: mcleblanc
ms.assetid: A37ADD4A-2187-4767-9C7D-EDE8A90AA215
title: "規劃效能"
description: "使用者會期望其 app 保持回應性，並可自在地使用，而不會耗盡電池。"
translationtype: Human Translation
ms.sourcegitcommit: afb508fcbc2d4ab75188a2d4f705ea0bee385ed6
ms.openlocfilehash: 39d57811a07b4c404da4b7e369e3bf5441fa99c0

---
# 規劃效能

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


使用者會期望其 app 保持回應性，並可自在地使用，而不會耗盡電池。 在技術上來說，效能是非功能的需求，但是將效能視為功能可協助您滿足使用者的期望。 指定目標和測量是主要因素。 決定您的效能關鍵案例是什麼；定義良好效能代表什麼意義。 然後在整個專案週期中及早並經常進行測量，以確保您能夠達成目標。

## 指定目標

使用者經驗是定義良好效能的基本方法。 app 啟動時間可以影響使用者對其效能的認知。 使用者可能會將 app 啟動時間少於 1 秒視為有最佳效能，少於 5 秒視為效能良好，而大於 5 秒則視為效能不佳。

其他衡量標準，例如記憶體，則對使用者經驗的影響較不明顯。 暫停或非使用中的 app 終止的機率，會隨著使用中 app 使用的記憶體量而提高。 高記憶體使用量會讓使用者在系統的所有 app 中獲得較差的使用經驗是一般規則，所以設定記憶體消耗量的目標是合理的。 請考量您的使用者認知的 app 概略大小：小型、中型或大型。 效能的期望與這個認知相互關聯。 例如，沒有使用大量媒體的小型 app 應該使用少於 100MB 的記憶體。

最好是設定初始目標，然後再做修改，而不是完全不設目標。 您的 app 效能目標應該是特定的而且可以評估，且它們應該分成三種類別：需要多久時間讓使用者或 app 完成工作 (時間)；app 重新繪製本身以回應使用者互動的等級和連續性 (流暢度)；以及 app 節省系統資源的程度，包括電池電力 (效率)。

## 時間

請考量使用者在您的 app 中完成其工作的可接受經過時間範圍 (*互動等級*)。 針對每個互動等級指派標籤、認知的使用者觀點，以及理想和最大持續時間。 以下是一些建議。

| 互動等級標籤 | 使用者認知                 | 理想            | 最大值          | 範例                                                                     |
|-------------------------|---------------------------------|------------------|------------------|------------------------------------------------------------------------------|
| 快速                    | 最察覺不到的延遲      | 100 毫秒 | 200 毫秒 | 顯示應用程式列；按下某個按鈕 (第一個回應)                        |
| 一般                 | 反應快但並不迅速             | 300 毫秒 | 500 毫秒 | 調整大小；語意式縮放                                                        |
| 有回應              | 反應不快，但感覺有回應 | 500 毫秒 | 1 秒         | 瀏覽到不同頁面；從暫停狀態繼續應用程式          |
| 啟動                  | 具有競爭力的經驗          | 1 秒         | 3 秒        | 第一次啟動應用程式或在應用程式終止後啟動應用程式 |
| 連續              | 不再感覺有回應      | 500 毫秒 | 5 秒        | 從網際網路下載檔案                                            |
| 網頁驗證                 | 冗長；使用者可能切換離開    | 500 毫秒 | 10 秒       | 從市集安裝多個 app                                         |

 

您現在可以將互動等級指派到您的 app 效能案例。 您可以將 app 的時間點參考、一部分的使用者經驗，以及一個互動等級指派給每個案例。 以下是針對飲食 app 範例的一些建議。


<!-- DHALE: used HTML table here b/c WDCML src used rowspans -->
<table>
<tr><th>案例</th><th>時間點</th><th>使用者經驗</th><th>互動等級</th></tr>
<tr><td rowspan="3">瀏覽到食譜網頁 </td><td>第一個回應</td><td>頁面轉換動畫啟動</td><td>快速 (100-200 毫秒)</td></tr>
<tr><td>有回應</td><td>食材已載入；但沒有影像</td><td>有回應 (500 毫秒 - 1 秒)</td></tr>
<tr><td>視覺上完成</td><td>已載入全部內容；且顯示影像</td><td>連續 (500 毫秒 - 5 秒)</td></tr>
<tr><td rowspan="2">搜尋食譜</td><td>第一個回應</td><td>已按下 搜尋 按鈕</td><td>快速 (100 - 200 毫秒)</td></tr>
<tr><td>視覺上完成</td><td>顯示當地食譜標題清單</td><td>一般 (300 - 500 毫秒)</td></tr>
</table>

如果您正在顯示動態內容，則也要考慮內容更新目標。 您的目標是要每隔幾秒重新整理一次內容嗎？ 或是每幾分鐘、幾小時，甚至一天重新整理一次內容是可接受的使用者經驗？

使用您指定的目標，您現在更容易測試、分析及最佳化您的 app。

## 流暢度

您的 app 的特定可測量流暢度目標可能包含：

-   無畫面重新繪製停止和啟動 (問題)。
-   每秒 60 格 (FPS) 的動畫轉譯。
-   當使用者移動瀏覽/捲動時，app 每秒顯示 3 到 6 頁內容。

## 效率

app 的特定可測量效率目標可能包含：

-   對於您 app 的程序，CPU 百分比隨時位於或低於 *N*，且以 MB 為單位的記憶體使用量隨時位於或低於 *M*。
-   當 app 為非使用中時，app 的程序的 *N* 和 *M* 皆為零。
-   當 app 為使用中時，電池電力可以使用 *X* 小時；app 為非使用中時，裝置會保留其電力 *Y* 小時。

## 針對效能設計 app

您現在可以使用您的效能目標，影響您 app 的設計。 使用範例飲食 app，在使用者瀏覽到食譜頁面之後，您可能會選擇[以遞增方式更新項目](optimize-gridview-and-listview.md#update-items-incrementally)，這樣會先轉譯食譜的名稱，遞延顯示食材，然後再顯示影像。 這樣會在移動瀏覽/捲動時維持回應性和流暢的 UI，在互動減緩步調讓 UI 執行緒跟上之後，可以不失真轉譯。 以下是一些也需要納入考慮的層面。

**UI**

-   最大化您的 app UI 之每個頁面的剖析和載入時間與記憶體效率 (特別是初始頁面)，方法是[最佳化您的 XAML 標記](optimize-xaml-loading.md)。 簡而言之，延遲載入 UI 和程式碼直到需要的時候。
-   對於 [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)，讓所有項目大小相同和盡量使用 [ListView 和 GridView 最佳化技術](optimize-gridview-and-listview.md)。
-   以標記形式宣告 UI，架構可以載入區塊中並重複使用，而不是必須在程式碼中建構。
-   摺疊 UI 元素直到使用者需要時才展開。 請參閱 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) 屬性。
-   偏好佈景主題轉場和腳本動畫。 如需詳細資訊，請參閱[動畫概念](https://msdn.microsoft.com/library/windows/apps/Mt187350)。 請記住，腳本動畫需要在畫面不斷更新，並讓 CPU 與圖形管線維持使用中狀態。 若要節省電池電力，如果使用者未與 app 互動，則不要讓動畫執行。
-   載入的影像應該以適合您要使用 [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) 方法呈現之檢視的大小載入。

**CPU、記憶體和電源**

-   排定優先順序較低的工作在優先順序較低的執行緒和/或核心上執行。 請參閱[非同步程式設計](https://msdn.microsoft.com/library/windows/apps/Mt187335)、[**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/BR209054) 屬性和 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/BR208211) 類別。
-   透過釋放不需要的高度耗費資源 (例如媒體)，最小化 app 的磁碟使用量。
-   最小化程式碼的工作集。
-   盡可能解除登錄事件處理常式和解除參照 UI 元素，以避免記憶體流失。
-   為了電池電力，在輪詢資料的頻率、查詢感應器，或排定工作在閒置的 CPU 上工作時，請抱持保守的態度。

**資料存取**

-   如果可能的話，預先擷取內容。 若是自動預先擷取，請參閱 [**ContentPrefetcher**](https://msdn.microsoft.com/library/windows/apps/Dn279042) 類別。 若是手動預先擷取，請參閱 [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/BR224847) 命名空間和 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/Hh700517) 類別。
-   如果可能的話，快取耗費高度資源才能存取的內容。 請參閱 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/BR241621) 和 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) 屬性。
-   對於快取遺失，儘快顯示預留位置 UI，指出 app 仍在載入內容。 以不讓使用者覺得突兀的方式，從預留位置內容轉換成即時內容。 例如，app 載入即時內容時，不會變更使用者手指或滑鼠指標下方的內容。

**啟動和繼續 app**

-   延遲 app 的啟動顯示畫面，而且除非必要，否則不要延長 app 的啟動顯示畫面。 如需詳細資料，請參閱[建立快速而流暢的 app 啟動經驗](http://go.microsoft.com/fwlink/p/?LinkId=317595)和[延長顯示啟動顯示畫面](https://msdn.microsoft.com/library/windows/apps/Mt187309)。
-   停用啟動顯示畫面關閉之後立即出現的動畫，原因是它們會給人延遲 app 啟動時間的印象。

**彈性 UI 和方向**

-   使用 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021) 類別。
-   只立即完成必要的工作，將需要大量資源的 app 工作延遲到稍後再進行 — 您的 app 有 200 到 800 毫秒可完成工作，超過這段時間之後，使用者就會看到 app UI 處於裁切的狀態。

有了與效能相關的設計，您就可以開始編寫 app 的程式碼。

## 檢測效能

編寫程式碼時，您可以加入 app 執行時在特定點記錄訊息和事件的程式碼。 之後，當您要測試 app 時，可以使用 Windows Performance Recorder 和 Windows Performance Analyzer (兩者皆隨附於 [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx) 中) 之類的分析工具，建立及檢視關於您 app 效能的報告。 您可以在此報告中尋找這些訊息和事件，以協助您更輕鬆地分析報告的結果。

通用 Windows 平台 (UWP) 提供由 [Windows 事件追蹤 (ETW)](https://msdn.microsoft.com/library/windows/desktop/Bb968803) 所支援的記錄 API，共同提供豐富的事件記錄和追蹤解決方案。 這些 API 屬於 [**Windows.Foundation.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/BR206677) 命名空間的一部分，其中包含 [**FileLoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264138)、[**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/Dn264195)、[**LoggingChannel**](https://msdn.microsoft.com/library/windows/apps/Dn264202) 以及 [**LoggingSession**](https://msdn.microsoft.com/library/windows/apps/Dn264217) 類別。

若要在 app 執行時，於報告中的特定點記錄訊息，請建立 **LoggingChannel** 物件，然後呼叫該物件的 [**LogMessage**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingchannel.logmessage.aspx) 方法，如下所示。

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingChannel myLoggingChannel = new LoggingChannel("MyLoggingChannel");

myLoggingChannel.LogMessage(LoggingLevel.Information, "Here' s my logged message.");

// ...
```

若要在 app 執行時，於報告中記錄一段時間的開始和結束事件，請建立 **LoggingActivity** 物件，然後呼叫該物件的 [**LoggingActivity**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.diagnostics.loggingactivity.loggingactivity.aspx) 建構函式，如下所示。

```csharp
// using Windows.Foundation.Diagnostics;
// ...

LoggingActivity myLoggingActivity;

// myLoggingChannel is defined and initialized in the previous code example.
using (myLoggingActivity = new LoggingActivity("MyLoggingActivity"), myLoggingChannel))
{   // After this logging activity starts, a start event is logged.
    
    // Add code here to do something of interest.
    
}   // After this logging activity ends, an end event is logged.

// ...
```

另請參閱[記錄範例](http://go.microsoft.com/fwlink/p/?LinkId=529576)。

檢測 app 之後，您可以測試及測量 app 的效能。

## 根據效能目標測試及測量

效能計劃的一部分是定義要在開發期間測量效能的所有點。 這有很多目的，在專案的打造設計原型、開發或部署期間測量所代表意義各有不同。 塑造設計原型初期階段期間測量效能是極為重要的，因此我們建議您在程式碼進行有意義的工作時馬上測量。 早期測量可讓您了解 app 耗用資源的部分，並藉此做出設計決策。 這會讓您的 app 成為可承受高負載的高效能 app。 越晚變更設計，通常越昂貴。 太晚在產品週期測量效能的結果是您可能無法完成專案或應用程式效能不佳。

使用這些技術和工具對照原始的效能目標測試 app 的成果。

-   針對各種硬體設定進行測試，包含全方位電腦和桌上型電腦、膝上型電腦、輕量級筆記型電腦、平板電腦及其他行動裝置。
-   針對各種螢幕大小進行測試。 雖然較寬的螢幕大小可以顯示更多內容，但是顯示這些額外內容可能對效能產生負面影響。
-   盡可能排除測試變數。
    -   關閉測試裝置上的背景 app。 若要這樣做，在 Windows 中從 \[開始\] 功能表選取 \[設定\]\[個人化\]\[鎖定畫面\]。 選取每個使用中的 app，然後選取 \[無\]。
    -   將 app 編譯為原生程式碼，方法是在發行組態中建置，然後再將它部署到測試裝置。
    -   若要確保自動維護不會影響測試裝置的效能，請手動觸發程序並等候它完成。 在 Windows 中，於 \[開始\] 功能表中搜尋 \[安全性與維護\]。 在 \[維護\] 區域的 \[自動維護\] 底下，選取 \[開始維護\] 並且等待狀態從 \[正在進行維護\] 變更。
    -   執行 app 多次有助於排除隨機測試變數，並確保測量結果一致。
-   降低的電源可用性測試。 使用者裝置的電源可能遠遠小於您開發電腦的電源。 Windows 是針對低電源裝置 (例如行動裝置) 所設計。 在平台上執行的 app 應該確定可以順利在這些裝置上執行。 我們發現低電源裝置的運作速度大約是桌上型電腦的四分之一，您必須據此設定您的目標。
-   使用工具的組合 (例如 Microsoft Visual Studio 和 Windows Performance Analyzer) 測量 app 效能。 Visual Studio 的設計是提供以 app 為主的分析，例如原始程式碼連結。 Windows Performance Analyzer 的設計則是提供以系統為主的分析，例如提供系統資訊、觸控操作事件的相關資訊，以及磁碟輸入/輸出 (I/O) 和圖形處理器 (GPU) 成本的相關資訊。 兩個工具都提供追蹤擷取和匯出，而且可以重新開啟共用追蹤和事後追蹤。
-   將 app 提交到市集進行認證之前，請務必將與效能相關的測試案例加入您的測試計劃中，如 [Windows 應用程式認證套件測試](windows-app-certification-kit-tests.md)的＜效能測試＞一節，以及 [Windows 市集應用程式測試案例](https://msdn.microsoft.com/library/windows/apps/Dn275879)的＜效能和穩定性＞一節中所述。

如需詳細資訊，請參閱這些資源和分析工具。

-   [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/apps/xaml/hh448170.aspx)
-   [Windows Performance Toolkit](https://msdn.microsoft.com/library/windows/apps/xaml/hh162945.aspx)
-   [使用 Visual Studio 診斷工具分析效能](https://msdn.microsoft.com/library/windows/apps/xaml/hh696636.aspx)
-   //build/ 演講 [XAML 效能](https://channel9.msdn.com/Events/Build/2015/3-698)
-   //build/ 演講 [Visual Studio 2015 中新的 XAML 工具](https://channel9.msdn.com/Events/Build/2015/2-697)

## 回應效能測試結果

分析您的效能測試結果之後，判斷是否需要任何變更，例如：

-   是否應該變更任何 app 設計決策，或最佳化您的程式碼？
-   是否應該在程式碼中新增、移除或變更任何檢測？
-   是否應該修改任何效能目標？

如果需要任何變更，進行變更然後返回檢測或測試並重複。

## 最佳化

僅最佳化您的 app 中的效能關鍵程式碼路徑：花費大部分時間的項目。 分析會告訴您是哪些項目。 您通常必須在建立遵循最佳設計做法的軟體和編寫最佳化程式碼這兩者間做出取捨。 在效能較不重要的地方，您最好以開發人員生產力和良好的軟體設計為優先考量。




<!--HONumber=Jun16_HO4-->


