---
author: eliotcowley
Description: 設計您的 app，讓它在電視上看起來很美觀而且運作良好。
title: 針對 Xbox 與電視設計
ms.assetid: 780209cb-3e8a-4cf7-8f80-8b8f449580bf
label: Designing for Xbox and TV
template: detail.hbs
isNew: true
---

> \[本文說明尚未提供的功能。 此功能在正式發行前可能會進行大幅修改。 Microsoft 對此處提供的資訊，不提供任何明確或隱含的瑕疵擔保。\]

# 針對 Xbox 與電視設計

設計您的通用 Windows 平台 (UWP) app，讓它在 Xbox One 及電視螢幕上看起來很美觀，而且能正常運作。

## 概觀

通用 Windows 平台可讓您創造跨多個 Windows 10 裝置的絕佳體驗。 
UWP 架構提供的大部分功能可讓 app 在這些裝置上使用相同的使用者介面 (UI)，無需進行額外的工作。 
不過，如果要量身打造並提供最佳化的 app 以便在 Xbox One 和電視螢幕上運作良好，則需要特殊考量。

坐在房間一端的沙發上，使用遊戲台或遙控器與電視互動的體驗，稱為「10 英呎體驗」****。 
這個名稱的由來是因為使用者通常坐在離螢幕大約 10 英呎遠的位置。 
這是一個獨特的挑戰，因為我們不會稱與電腦互動是 *2 英呎*體驗。 
如果您為 Xbox One 或其他輸出至電視螢幕的裝置開發 app，並使用控制器做為輸入，您就必須記住這一點。

您並非需要本篇文章中所有的步驟，才能讓 app 創造出 10 英呎體驗，但是了解這些步驟，為您的 app 做出適當的決定，可針對您的 app 特定需求，量身打造出更良好的 10 英呎體驗。 
當您打算在 10 英呎環境中運作您的 app 時，請考慮下列設計原則。

### 簡單

針對 10 英呎環境的設計會產生一些獨特的挑戰。 解析度和檢視距離會讓人們很難處理太多資訊。 
請嘗試讓設計保持清晰，盡量精簡為最簡單的元件。 在電視上顯示的資訊量應該與在行動電話上 (而不是電腦上) 看到的內容差不多。

![Xbox One 主畫面](images/designing-for-tv/xbox-home-screen.png)

### 易懂

10 英呎環境中的 UWP app 應該直覺且易於使用。 讓焦點清楚而且易懂。 
排列好內容，讓空間的移動一致而且可預測。 為使用者的動作提供最短的路徑。

![Xbox One 電影應用程式](images/designing-for-tv/xbox-movies-app.png)

_**螢幕擷取畫面中所顯示的所有電影都能在 Microsoft 電影與電視上取得。**_  

### 迷人

大螢幕能提供最身歷其境、類似電影的體驗。 無縫的場景、順暢的動作，以及生動活潑的色彩和排版，都能讓您的 app 提升到不同的層次。 盡量明顯而美觀。

![Xbox One 的虛擬人偶應用程式](images/designing-for-tv/xbox-avatar-app.png)

### 10 英呎體驗最佳化

您現在知道了出色的 UWP app 具備 10 英呎體驗的設計原則，請仔細閱讀下列概觀，了解可最佳化您的 app 並提供絕佳使用者體驗的特定方式。

| 功能        | 說明           |
| -------------------------------------------------------------- |--------------------------------|
| [遊戲台與遙控器](#gamepad-and-remote-control)      | 確定您的 app 可使用遊戲台和遙控器順暢運作，是最佳化 10 英呎體驗最重要的步驟。 您可以對遊戲台和遙控器進行一些特定的增強功能，在使用者動作有某種程度受限的裝置上，最佳化使用者的互動體驗。 |
| [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction) | UWP 提供「XY 焦點瀏覽」****，可讓使用者四處瀏覽 app 的 UI。 不過，這限制使用者只能向上、向下、向左和向右瀏覽。 本節概述處理此功能和其他考量的建議。 |
| [滑鼠模式](#mouse-mode)|在某些使用者介面 (例如地圖和繪圖介面) 中，無法使用或不方便使用 XY 焦點瀏覽。 對於這些介面，UWP 提供「滑鼠模式」****讓遊戲台/遙控器可自由瀏覽，就像傳統型電腦的滑鼠一樣。|
| [視覺焦點](#focus-visual)  | 視覺焦點是目前有焦點的 UI 元素周圍的框線。 這可協助引導使用者輕鬆瀏覽您的 UI 而不會迷失。 如果焦點不是很清楚，使用者可能會在您的 UI 中迷路，而無法獲得良好的體驗。  |
| [焦點佔用](#focus-engagement) | 若要在 UI 元素上設定焦點佔用，使用者必須按下 [A/選取]**** 按鈕以與其進行互動。 這有助於針對使用者瀏覽您 app 的 UI，建立更棒的使用體驗。
| [調整 UI 元素大小](#ui-element-sizing)  | 通用 Windows 平台使用[縮放與有效像素](..\layout\design-and-ui-intro.md#effective-pixels-and-scaling)，根據檢視距離來調整 UI。 了解如何調整大小並套用到整個 UI，可協助最佳化 10 英呎環境的 app。  |
|  [電視安全區域](#tv-safe-area) | UWP 預設會自動避免在電視不安全的區域 (接近螢幕邊緣的區域) 中顯示任何 UI。 不過，這會產生一種「被框住」的效果，UI 看起來就像信箱一樣。 為了讓您的 app 能真正融入電視螢幕，您要加以修改，讓 app 在支援的電視能延伸到螢幕的邊緣。 |
| [色彩](#colors)  |  UWP 支援色彩佈景主題，優先採用系統佈景主題的 app 在 Xbox One 上將會預設為「深色」****。 如果您的 app 有特定的色彩佈景主題，您應該考慮到有些色彩不適合電視，應盡量避免使用。 |
| [音效](../style/sound.md)    | 音效聲音在 10 英呎體驗中扮演關鍵角色，其為使用者提供身歷其境的體驗與回應。 在 Xbox One 上執行 app 時，UWP 可提供針對通用控制項自動開啟音效的功能。 深入了解關於 UWP 內建音效支援及如何善用的詳細資訊。    |
| [UI 控制項的指導方針](#guidelines-for-ui-controls)  |  提供數種可針對多部裝置良好運作的 UI 控制項，但當在電視上使用時具有特定考量。 深入了解有關專為 10 英呎體驗設計時使用這些控制項的一些最佳做法。 |

<!--[elcowle] We may uncomment this now that the Sound article is live-->
<!--| [Sound](../style/sound.md)  |  Sounds play a key role in the 10-foot experience, helping to immerse and give feedback to the user. The UWP provides functionality that automatically turns on sounds for common controls when the app is running on Xbox One. Find out more about the sound support built into the UWP and learn how to take advantage of it. |-->

## 遊戲台與遙控器

就像電腦的鍵盤和滑鼠，以及手機和平板電腦的觸控，遊戲台與遙控器是 10 英呎體驗的主要輸入裝置。 
本節將介紹什麼是硬體按鈕，以及它們所執行的動作。 
在 [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction)和[滑鼠模式](#mouse-mode)中，您將了解使用這些輸入裝置時如何最佳化您的 app。

遊戲台和遙控器行為的原始品質取決於您的 app 支援鍵盤的程度。 要確保您的 app 可搭配遊戲台/遙控器正確運作的一個好方式是確定它可搭配電腦的鍵盤正確運作，然後使用遊戲台/遙控器測試來尋找您 UI 的弱點。

### 硬體按鈕

本文件將會以下圖中所提供的名稱參照按鈕。

![遊戲台與遙控器按鈕圖](images/designing-for-tv/hardware-buttons-gamepad-remote.png)

您可以從圖中看到，遊戲台支援一些遙控器不支援的按鈕，反之亦然。 雖然您可以使用只在一種輸入裝置上支援的按鈕，讓瀏覽 UI 的速度更快，但是請注意，使用這些按鈕進行重要的互動，可能會產生使用者無法與特定部分 UI 互動的情況。

下表列出 UWP app 支援的所有硬體按鈕，以及哪一種輸入裝置支援這些按鈕。

| 按鈕                    | 遊戲台   | 遙控器    |
|---------------------------|-----------|-------------------|
| A/選取按鈕           | 是       | 是               |
| B/返回按鈕             | 是       | 是               |
| 方向鍵 (D 鍵)   | 是       | 是               |
| 功能表按鈕               | 是       | 是               |
| 檢視按鈕               | 是       | 是               |
| X 和 Y 按鈕           | 是       | 否                |
| 左搖桿                | 是       | 否                |
| 右搖桿               | 是       | 否                |
| LT 鍵和 RT 鍵   | 是       | 否                |
| LB 鍵和 RB 鍵    | 是       | 否                |
| OneGuide 按鈕           | 否        | 是               |
| 音量按鈕             | 否        | 是               |
| 頻道按鈕            | 否        | 是               |
| 媒體控制項按鈕     | 否        | 是               |
| 靜音按鈕               | 否        | 是               |

### 內建按鈕支援

UWP 會自動將現有的鍵盤輸入行為對應到遊戲台與遙控器輸入。 下表列出這些內建的對應。

| 鍵盤              | 遊戲台/遙控器                        |
|-----------------------|---------------------------------------|
| 方向鍵            | 方向鍵 (也是遊戲台的左搖桿)    |
| 空格鍵              | A/選取按鈕                       |
| Enter 鍵                 | A/選取按鈕                       |
| ESC 鍵                | B/返回按鈕*                        |

\*當 app 處理的 B 按鈕既不是 [KeyDown](https://msdn.microsoft.com/library/windows/apps/br208941) 事件也不是 [KeyUp](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.keyup.aspx) 事件時，則會觸發 [SystemNavigationManager.BackRequested](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.core.systemnavigationmanager.backrequested.aspx) 事件，這在 app 內會造成返回瀏覽。

### 快速鍵支援

快速鍵可用來加速瀏覽 UI。 不過可能只有某些輸入裝置才有這些按鈕，所以請記住，並非所有使用者都能使用這些功能。 事實上，遊戲台是 Xbox One 上目前唯一支援 UWP app 快速鍵功能的輸入裝置。

下表列出 UWP 內建，以及您可自行實作的快速鍵支援。 請在您自訂的 UI 利用這些行為，以提供一致、友善的使用者體驗。

| 互動   | 鍵盤   | 遊戲台      | 內建於︰  | 建議用於： |
|---------------|------------|--------------|----------------|------------------|
| 移動瀏覽       | 無       | 右搖桿  | 無           |      [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) |
| 向上一頁/向下一頁  | 向上一頁/向下一頁 | LT 鍵/RT 鍵 | 無 | `ScrollViewer` 和清單/格線
| 向左一頁/向右一頁 | 無 | LB 鍵/RB 鍵 | [Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) | `ScrollViewer`
| 放大/縮小        | CTRL +/- | LT 鍵/RT 鍵 | `ScrollViewer` | 支援放大和縮小的檢視

## XY 焦點瀏覽和互動

如果您的 app 支援使用鍵盤正確瀏覽焦點，這也會順利轉譯到遊戲台與遙控器。 
使用方向鍵瀏覽對應至 [方向鍵 (D 鍵)]**** (以及遊戲台上的 [左搖桿]****)，與 UI 元素的互動對應至 [Enter/選取]**** 鍵 (參閱[遊戲台與遙控器](#gamepad-and-remote-control))。 如需鍵盤設計指導方針，請參閱[鍵盤互動](keyboard-interactions.md)。

如果正確實作鍵盤支援，您的 app 應該可以正確運作，不過可能需要一些額外的工作才能支援每種狀況。 請思考您的 app 特定需求，以盡可能提供最佳的使用者體驗。

### 無法存取的 UI

因為 XY 焦點瀏覽限制使用者只能向上、向下、向左和向右移動，所以您有可能無法存取某些 UI。 
下圖說明 XY 焦點瀏覽不支援的 UI 配置類型範例。 
請注意，使用使用遊戲台/遙控器無法存取中間的元素，因為會優先瀏覽垂直和水平方向，中間的元素將永遠不會優先取得焦點。

![四個角有元素，且中間有無法存取的元素](images/designing-for-tv/2d-navigation-best-practices-ui-layout-to-avoid.png)

如果因為某些原因無法重新排列 UI，請使用下一節討論的其中一個技術來覆寫預設焦點行為。

### 覆寫預設的瀏覽 <a name="overriding-the-default-navigation"></a>

雖然 UWP 嘗試讓使用者認為方向鍵/左搖桿的瀏覽方式很直覺，但是無法保證針對您的 app 行為最佳化。 
確保瀏覽針對您的 app 最佳化的最佳方式是使用遊戲台加以測試，確認使用者能針對 app 的案例以直覺的方式存取每個 UI 元素。 如果您的 app 案例呼叫 XY 焦點瀏覽無法達到的行為，請考慮下列各節中的下列建議並/或覆寫行為，將焦點放在合理的項目上。

下列程式碼片段示範如何覆寫 XY 焦點瀏覽行為︰

```xml
<StackPanel>
    <Button x:Name="MyBtnLeft" 
            Content="Search" />
    <Button x:Name="MyBtnRight" 
            Content="Delete"/>
    <Button x:Name="MyBtnTop" 
            Content="Update" />
    <Button x:Name="MyBtnDown" 
            Content="Undo" />
    <Button Content="Home"  
            XYFocusLeft="{x:Bind MyBtnLeft}" 
            XYFocusRight="{x:Bind MyBtnRight}"
            XYFocusDown="{x:Bind MyBtnDown}"
            XYFocusUp="{x:Bind MyBtnTop}" />
</StackPanel>
```

在這個案例中，當焦點在 `Home` 按鈕上而使用者瀏覽到左邊時，焦點會移到 `MyBtnLeft` 按鈕；如果使用者瀏覽到右邊，焦點會移到 `MyBtnRight` 按鈕等等。

若要防止焦點從某個特定方向移出控制項，請使用 `XYFocus*` 屬性，將其指向相同的控制項︰

```xml
<Button Name="HomeButton"  
        Content="Home"  
        XYFocusLeft ="{x:Bind HomeButton}" />
```

### 最少點選次數的路徑 <a name="path-of-least-clicks"></a>

請嘗試讓使用者以最少的點選次數執行最常見的工作。 在下列範例中，[TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 放在 [播放]**** 按鈕 (一開始會取得焦點) 與常用的元素之間，讓不必要的元素放在優先的工作之間。

![最佳的瀏覽做法提供最少點選次數的路徑](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks.png)

在下列範例中，[TextBlock](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 改放在 [播放]**** 按鈕上方。 
只要重新排列 UI，不要將不必要的元素放在優先的工作之間，即可大幅改善您 app 的可用性。

![TextBlock 移動到 [播放] 按鈕上方，不再位於優先工作之間](images/designing-for-tv/2d-navigation-best-practices-provide-path-with-least-clicks-2.png)

### CommandBar 和 ContextFlyout

使用 [CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 時，請記住[問題︰UI 元素位於長捲動清單/格線之後](#problem-ui-elements-located-after-long-scrolling-list-grid)中所述的捲動清單問題。 下列影像顯示一個 `CommandBar` 位於清單/格線下方的 UI 配置。 使用者必須一直向下捲動完整個清單/格線，才能到達 `CommandBar`。

![CommandBar 位於清單/格線的底部](images/designing-for-tv/2d-navigation-best-practices-commandbar-and-contextflyout.png)

如果您將 `CommandBar` 放在清單/格線的「上方」**會怎麼樣？ 雖然使用者向下捲動清單/格線後必須捲動回去才能到達 `CommandBar`，但是比起前一種設定，瀏覽的範圍比較少。 請注意，這是假設您的 app 最初的焦點是放置在 `CommandBar` 旁邊或上方；如果最初的焦點是在清單/格線下方，此方法也同樣不佳。 如果這些 `CommandBar` 項目是不需要經常存取的全域動作項目 (例如 [同步]**** 按鈕)，位於清單/格線上方也許可以接受。

如果您的 app 有 `CommandBar`，當中有使用者必須容易存取的項目，您可以考慮將這些項目放在 `ContextFlyout` 內，並從 `CommandBar` 移除。 

雖然您無法垂直堆疊 `CommandBar` 的項目，但是如果您的 UI 配置可以運作，可以考慮垂直捲動方向放置這些項目 (例如，放在垂直捲動清單的左邊或右邊，或是放在水平捲動清單的頂端或底部)。

### UI 配置挑戰

由於 XY 焦點瀏覽的特性，有些 UI 配置更具挑戰性，而且應該依個別案例評估。 雖然沒有一種「正確」的方式，您選擇的解決方案也取決於您 app 的特定需求，不過您還是可以使用一些技術，創造絕佳的電視體驗。

為了更深入了解，我們來看看一個假想的 app，說明這些問題和解決這些問題的技術。

> **注意** &nbsp;&nbsp;這個假造的 app 是為了說明 UI 問題和可能的解決方案，而不是針對您特定的 app 示範最佳的使用者體驗。

以下是一個假想的房地產 app，顯示可供銷售的房屋清單、地圖、房地產說明，以及其他資訊。 這個 app 有三個挑戰，您可以使用下列技術克服︰

- [重新排列 UI](#ui-rearrange)
- [焦點佔用](#engagement)
- [滑鼠模式](#mouse-mode)

![假造的房地產 app](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app.png)

#### 問題：UI 元素位於長的捲動清單/格線之後 <a name="problem-ui-elements-located-after-long-scrolling-list-grid"></a>

下列影像中所顯示的房地產的 [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 是很長的捲動清單。 如果 `ListView` 上「不」**需要[佔用](#focus-engagement)，當使用者瀏覽到清單，焦點會放在清單中的第一個項目。 如果使用者要到達 [上一個]**** 或 [下一個]**** 按鈕時，他們必須瀏覽通過清單中的所有項目。 要求使用者瀏覽整個清單會非常痛苦&mdash;如果清單夠短，這還算可以接受&mdash;，所以您可能要考慮其他方案。

![房地產 app︰50 個項目的清單需要點選 51 次才能到達下面的按鈕](images/designing-for-tv/2d-focus-navigation-and-interaction-real-estate-app-list.png)

#### 解決方案

##### 重新排列 UI <a name="ui-rearrange"></a>

除非您最初的焦點是放在頁面底部，否則將 UI 元素放在長捲動清單上方比放在下方更容易存取。 
如果這個新的配置適用於其他裝置，請針對所有裝置系列變更配置，而不要只是針對 Xbox One 進行特殊的 UI 配置，這是比較經濟的方法。 
此外，垂直捲動方向放置 UI 元素 (也就是水平放在垂直捲動清單兩側，或垂直放在水平捲動清單上下) 可更方便存取。

![房地產 app︰放置按鈕在長捲動清單上方](images/designing-for-tv/2d-focus-navigation-and-interaction-ui-rearrange.png)

##### 焦點佔用 <a name="engagement"></a>

「需要」**佔用時，整個 `ListView` 會變成單一的焦點目標。 使用者可以略過清單的內容，以取得下一個可設定焦點的元素。 請在[焦點佔用](#focus-engagement)中閱讀更多關於哪些控制項支援佔用，以及如何使用的內容。

![房地產 app︰設定需要佔用，只需按 1 次就可到達 [上一個/下一個] 按鈕](images/designing-for-tv/2d-focus-navigation-and-interaction-engagement.png)

#### 問題︰ScrollViewer 沒有任何可設定焦點的元素

由於 XY 焦點瀏覽仰賴一次僅瀏覽單一可設定 UI 元素的設計，因此沒有任何可設定焦點的元素的 [ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) (例如本範例所示的只包含文字) 可能會造成使用者無法檢視 `ScrollViewer` 中的所有內容。 
如需這個案例和其他相關案例的解決方案，請參閱[焦點佔用](#focus-engagement)。

![房地產 app︰ScrollViewer 只包含文字](images/designing-for-tv/2d-focus-navigation-and-interaction-scrollviewer.png)

#### 問題︰自由捲動 UI

當您的 app 需要自由捲動 UI 時 (例如繪圖介面，或本範例中的地圖)，XY 焦點瀏覽就無法運作。 
在這種情況下，您可以開啟[滑鼠模式](#mouse-mode)以允許使用者自由地在 UI 元素內瀏覽。

![使用滑鼠模式的地圖 UI 元素](images/designing-for-tv/map-mouse-mode.png)

## 滑鼠模式

如 [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction)中所述，在 Xbox One 上，焦點是使用 XY 瀏覽系統移動，讓使用者能夠透過向上、向下、向左和向右在控制項之間移動焦點。 
不過，某些控制項 (例如 [WebView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.webview.aspx) 和 [MapControl](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx)) 需要類似滑鼠的互動，使用者可以自由地在控制項的界限內移動指標。 
還有一些 app，使用者要能夠在整個頁面移動指標，讓使用遊戲台/遙控器的使用者能夠擁有類似使用電腦滑鼠的體驗。

對於這些案例，您應該對整個頁面或對某個頁面內的某個控制項要求指標 (滑鼠模式)。 
例如，您的 app 可以有一個有 `WebView` 控制項的頁面，只有在這個控制項當中才使用滑鼠模式，而在其他地方則仍使用 XY 焦點瀏覽。 
若要要求指標，您可以指定「控制項或頁面佔用時」****或「頁面有焦點時」****是否需要指標。

> **注意** &nbsp;&nbsp;當控制項取得焦點，不支援要求指標。

下圖顯示遊戲台/遙控器在滑鼠模式中的按鈕對應。

![遊戲台/遙控器在滑鼠模式中的按鈕對應](images/designing-for-tv/mouse-mode.png)

> **注意** &nbsp;&nbsp;只有在 Xbox One 上使用遊戲台/遙控器時才支援滑鼠模式。 在其他裝置系列和輸入類型上，會以無訊息方式略過。

在控制項或頁面上使用 `RequiresPointer` 屬性可在其上啟用滑鼠模式。 `RequiresPointer` 有三個可能值︰`Never` (預設值)、`WhenEngaged` 以及 `WhenFocused`。

> **注意** &nbsp;&nbsp;`RequiresPointer` 是新的 API，尚未編寫文件。 

<!--TODO: Link to doc-->

### 啟用控制項上的滑鼠模式

當使用者以 `RequiresPointer="WhenEngaged"` 設定佔用控制項時，會在控制項上啟用滑鼠模式，直到使用者離開為止。 下列程式碼片段示範一個簡單的 `MapControl`，在被佔用時啟用滑鼠模式︰

```xml
<Page>
    <Grid>
        <MapControl IsEngagementRequired="true" 
                    RequiresPointer="WhenEngaged"/>
    </Grid>
</Page> 
```

> **注意** &nbsp;&nbsp;如果控制項在被佔用時啟用滑鼠模式，它也必須使用 `IsEngagementRequired="true"` 的佔用設定，否則永遠不會啟用滑鼠模式。

當控制項處於滑鼠模式時，其巢狀控制項也同樣會處於滑鼠模式。 此時會忽略其子項要求的模式&mdash;因為不可能父項處於滑鼠模式，而子項不是。

此外，只有在控制項取得焦點時才會檢查控制項的要求模式，因此當控制項有焦點時，模式不會動態變更。

### 啟用頁面上的滑鼠模式

若頁面有 `RequiresPointer="WhenFocused"` 屬性，當整個頁面取得焦點時，將啟用滑鼠模式。 下列程式碼片段示範提供頁面這個屬性︰

```xml
<Page RequiresPointer="WhenFocused">
    ...
</Page> 
```

> **注意** &nbsp;&nbsp;`WhenFocused`值只支援用於 [Page](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.aspx) 物件。 如果您嘗試在控制項上設定這個值，就會擲回例外狀況。

## 視覺焦點

視覺焦點是目前有焦點的 UI 元素周圍的框線。 這可協助引導使用者輕鬆瀏覽您的 UI 而不會迷失。

透過視覺更新，以及對視覺焦點新增的許多自訂選項，開發人員可以信任單一視覺焦點能在電腦和 Xbox One 上，以及在支援鍵盤和/或遊戲台/遙控器的任何其他 Windows 10 裝置上運作良好。

雖然不同平台上可以使用相同的視覺焦點，但是使用者對於 10 英呎體驗遇到的狀況稍有不同。 您應該假設使用者不會完全注意到整個電視螢幕，因此目前聚焦的元素對使用者而言隨時都清晰可見非常重要，避免視覺搜尋遇到挫折。

也請務必記住，視覺焦點預設會在使用遊戲台或遙控器 (而「不是」**鍵盤) 時顯示。 因此，即使您未實作，當您在 Xbox One 上執行您的 app 時也會顯示。

### 最初的視覺焦點位置

啟動 app 或瀏覽到頁面時，請將焦點放在使用者應採取動作的第一個 UI 元素上。 例如，相片 app 可以將焦點放在圖庫中的第一個項目，瀏覽到歌曲詳細檢視的音樂 app 可以將焦點放在播放按鈕，以方便播放音樂。

請嘗試將最初的焦點放在您 app 的左上方區域 (如果是由右至左的流程，則放在右上方)。 大部分的使用者一開始通常會將焦點放在這個角落，因為這是 app 內容流程通常開始的位置。

### 讓焦點清晰可見

螢幕上永遠要顯示一個視覺焦點讓使用者可以挑選而停駐，而不需要搜尋焦點。 同樣地，螢幕上隨時要有一個可設定焦點的項目&mdash;例如，不要使用只有文字而沒有可設定焦點元素的快顯視窗。

### 淺色關閉重疊

若要讓使用者注意使用者目前使用遊戲控制器或遙控器操作的 UI 元素，在 Xbox One 上執行 app 時，UWP 會自動新增一個「煙霧」層，蓋住快顯 UI 以外的區域。 這不需要任何額外的工作，但是設計您的 UI 時要記住這一點。

## 焦點佔用

焦點佔用是為了更容易使用遊戲台或遙控器與 app 互動。 

> **注意** &nbsp;&nbsp;設定焦點佔用不會影響鍵盤或其他輸入裝置。

當 [FrameworkElement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) 物件上的屬性 `IsFocusEngagementEnabled` 設定為 `True` 時，它會將控制項標示為需要焦點佔用。 這表示，使用者必須按下 [A/選取]**** 按鈕來「佔住」控制項並與它互動。 完成動作時，使用者可以按 [B/返回]**** 按鈕離開控制項並瀏覽到其他位置。

> **注意** &nbsp;&nbsp;`IsFocusEngagementEnabled` 是新的 API，尚未編寫文件。

### 焦點受困

焦點受困是當使用者嘗試瀏覽 app 的 UI，但「受困」在控制項內時所發生的狀況，很難或甚至無法移動到控制項之外。

下列範例顯示產生焦點受困的 UI。

![按鈕在水平滑桿左邊和右邊](images/designing-for-tv/focus-engagement-focus-trapping.png)

如果使用者想要從左邊的按鈕瀏覽到右邊的按鈕，合理的狀況是假設使用者只要按下方向鍵/左搖桿向右兩次。 
不過，如果 [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx) 不需要佔用，就會發生下列行為︰當使用者第一次按右鍵，焦點會移到 `Slider`，然後當使用者再按右鍵一次，`Slider` 的控點會向右移動。 使用者會一直將控點往右移動，而無法到達按鈕。

有幾種方法可以解決這個問題。 其中一個方法是設計不同的配置，類似 [XY 焦點瀏覽和互動](#xy-focus-navigation-and-interaction)中的房地產 app 範例，我們將「上一個」****按鈕和「下一個」****按鈕重新配置在 `ListView` 上方。 垂直而非水平堆疊控制項可以解決問題，如下列影像所示。

![按鈕在水平滑桿上方和下方](images/designing-for-tv/focus-engagement-focus-trapping-2.png)

現在，使用者可以使用方向鍵/左搖桿向上及向下瀏覽到每個控制項，當 `Slider` 有焦點時，使用者可以按左鍵和按右鍵如預期般移動 `Slider` 控點。

解決這個問題的另一種方法需要在 `Slider` 上設定佔用。 如果您設定 `IsFocusEngagementEnabled="True"`，這會導致下列行為。

![滑桿上需要焦點佔用，讓使用者可以瀏覽到右邊的按鈕](images/designing-for-tv/focus-engagement-slider.png)

當 `Slider` 需要焦點佔用時，使用者只要按方向鍵/左搖桿向右兩次，就可以到達右邊的按鈕。 這個解決方案很棒，因為它不需要調整任何 UI，就能產生預期的行為。

### 項目控制項

除了 [Slider](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.slider.aspx) 控制項，還有其他您需要佔用的控制項，例如︰

- [ListBox](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listbox.aspx)
- [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx)
- [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)
- [FlipView](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.flipview)

這些控制項與 `Slider` 控制項不同，不會在自己本身內限制焦點，不過當它們包含大量資料時，可能會造成可用性問題。 以下是 `ListView` 包含大量資料的範例。

![有大量資料，以及頂端按鈕和底部按鈕的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls.png)

類似 `Slider` 範例，我們來嘗試使用遊戲台/遙控器從上方按鈕瀏覽到下方按鈕。 
焦點從頂端按鈕開始，按下方向鍵/搖桿會將焦點放在 `ListView` 中的第一個項目上 ("Item 1")。 
當使用者再向下按一次，清單中下一個項目會取得焦點，而不是底部的按鈕。 
若要到達按鈕，使用者必須先瀏覽 `ListView` 中的每個項目。 
如果 `ListView` 包含大量資料，這可能相當不便，而且使用者體驗不佳。

若要解決這個問題，請將 `ListView` 上的 `IsFocusEngagementEnabled="True"` 屬性設定為需要在其上佔用。 
這樣可讓使用者只要向下按，就可快速跳過 `ListView`。 不過，除非使用者在清單有焦點時按下 [A/選取]**** 按鈕佔用清單，然後按下 [B/返回]**** 按鈕離開清單，否則使用者無法捲動清單，或從清單中選擇項目。

![需要佔用的 ListView](images/designing-for-tv/focus-engagement-list-and-grid-controls-2.png)

#### ScrollViewer

[ScrollViewer](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) 與這些控制項稍有不同，其具有待考量的個別原因。 如果您有具可設定焦點內容的 `ScrollViewer`，瀏覽至 `ScrollViewer` 預設可讓您在其可設定焦點的元素之間移動。 就像在 `ListView` 中一樣，您必須捲動每個項目才能瀏覽到 `ScrollViewer` 之外。 

如果 `ScrollViewer` *沒有*可設定焦點的內容&mdash;例如如果只包含文字&mdash;，您可以設定 `IsFocusEngagementEnabled="True"` 讓使用者可以使用 [A/選取]**** 按鈕來佔用 `ScrollViewer`。 佔用之後，使用者可以使用 [方向鍵/左搖桿]**** 捲動所有文字，然後在完成時按 [B/返回]**** 按鈕離開。

另一種方法是在 `ScrollViewer` 上設定 `IsTabStop="True"`，如此一來使用者便無須佔用控制項&mdash;，而可在 `ScrollViewer` 中無可設定焦點的元素時，使用 [方向鍵/左搖桿]**** 在上頭放置焦點然後捲動瀏覽。

### 焦點佔用預設值

有些控制項會造成焦點受困，所以其預設設定需要焦點佔用，而有些控制項預設關閉焦點佔用，但可以將它開啟獲得好處。 下表列出這些控制項，及其預設的焦點佔用行為。

| 控制項               | 焦點佔用預設值  |
|-----------------------|---------------------------|
| CalendarDatePicker    | 開啟                        |
| FlipView              | 關閉                       |
| GridView              | 關閉                       |
| ListBox               | 關閉                       |
| ListView              | 關閉                       |
| ScrollViewer          | 關閉                       |
| SemanticZoom          | 關閉                       |
| Slider                | 開啟                        |

當 `IsFocusEngagementEnabled="True"` 時，其他所有 UWP 控制項將不會導致任何行為或視覺變更。

## 調整 UI 元素大小

因為在 10 英呎環境中的 app 使用者會使用遙控器或遊戲台，而且坐在離螢幕數英呎遠的位置，所以您的設計必須納入一些特別的 UI 考量。 
請確定 UI 有適當的內容密度，也不要太雜亂，讓使用者可以輕鬆瀏覽和選取元素。 請記住︰重點是簡單。

### 縮放比例與調適型配置

「縮放比例」****有助於確保 UI 元素以適合 app 執行裝置的大小顯示。 
在桌面上，您可以在 [設定] &gt; [系統] &gt; [顯示]**** 中找到這個設定，以滑動值表示。 
手機上也有這個相同的設定 (如果裝置支援)。

![變更文字、應用程式與其他項目的大小](images/designing-for-tv/ui-scaling.png) 

Xbox One 上沒有這類系統設定。不過，如果要適當調整 UWP UI 元素的大小以適用於電視，則預設會調整為 [200%]****。 
只要 UI 元素能針對其他裝置適當調整大小，就能針對電視適當調整大小。 
Xbox One 以 1080p (1920 x 1080 像素) 呈現您的 app。 因此在從電腦等其他裝置帶入 app 時，請確定採用[調適型技術](https://msdn.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design)，以 100% 縮放讓 UI 呈現 960 x 540 像素最佳外觀。

針對 Xbox 設計與針對電腦設計稍有不同，因為您只需要考慮 1920 x 1080 一種解析度。 
如果使用者的電視解析度比較高，就沒什麼關係&mdash;UWP app 一律會縮放至 1080p。

但無論電視解析度為何，當您的 app 在 Xbox One 上執行時，也會提取 200% 的正確資產大小。

### 內容密度

設計 app 時請記住，使用者會從遠距離檢視 UI 而且使用遙控器或遊戲控制器與 UI 互動，比起使用滑鼠或觸控輸入要花費更多的時間瀏覽。

#### UI 控制項的大小

互動式 UI 元素的高度至少要有 32 epx (有效像素)。 這是常見 UWP 控制項的預設值，以縮放比例 200% 使用時，可確保從遠距離也能看見 UI 元素，並協助減少內容密度。 

![縮放比例 100% 和 200% 的 UWP 按鈕](images/designing-for-tv/button-100-200.png)

#### 點選次數

使用者從電視螢幕一邊瀏覽到另一邊時，以不超過「點選六次」****的原則來簡化您的 UI。 同樣地，這裡適用「簡單」****的原則。 如需更多詳細資料，請參閱[最少點選次數的路徑](#path-of-least-clicks)。

![跨 6 個圖示](images/designing-for-tv/six-clicks.png)

### 文字大小

若要從遠距離看見您的 UI，請使用下列重要規則︰

* 主要的文字和閱讀內容︰最小 15 epx
* 非關鍵的文字和補充內容︰最小 12 epx

在 UI 中使用較大文字時，請挑選不會限制螢幕實際可用空間太多、佔用其他內容可能填入空間的大小。

### 不使用縮放比例

我們建議您的 app 充分利用縮放比例支援，針對每個裝置類型進行縮放，可協助 app 在所有裝置上正確執行。 
不過，您也可以不使用這個行為，以縮放比例 100% 來設計所有 UI。 請注意，您無法將縮放比例變更為 100% 以外的值。

您可以使用下列程式碼片段，選擇不使用縮放比例︰

```csharp
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```

`result` 將會通知您是否成功選擇不要採用。

請務必將本主題中所述的「有效」**像素值加倍來得到「實際」**像素值，計算 UI 元素的適當大小。

## 電視安全區域

由於歷史上以及技術上的理由，並非所有的電視都以相同的方式顯示螢幕邊緣的內容。 UWP 預設會避免在電視不安全區域中顯示任何 UI 內容，改為只繪製頁面背景。

下列影像中，電視不安全區域以藍色區域表示。

![電視不安全區域](images/designing-for-tv/tv-unsafe-area.png)

您可以將背景設定為靜態或佈景主題色彩，或設定為影像，如以下程式碼片段示範。

### 佈景主題色彩

```xml
<Page x:Class="Sample.MainPage"
      Background="{ThemeResource ApplicationPageBackgroundThemeBrush}"/>
```

### 影像

```xml
<Page x:Class="Sample.MainPage"
      Background="\Assets\Background.png"/>
```

這是您的 app 不進行額外工作的外觀。

![電視安全區域](images/designing-for-tv/tv-safe-area.png)

這不是最佳的結果，因為它讓 app 產生「被框住」的效果，某些 UI (例如瀏覽窗格和格線) 似乎被裁切掉。 不過，您可以進行最佳化，延伸部份 UI 到螢幕邊緣，讓 app 有更多電影效果。

### 將 UI 繪製到邊緣

我們建議您使用特定 UI 元素，延伸到螢幕邊緣，讓使用者感覺更投入。 這些包括 [ScrollViewers](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)、[瀏覽窗格](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/nav-pane)以及 [CommandBars](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx)。

但同樣重要的是，互動式元素和文字則一律要避開螢幕邊緣，以確保在一些電視上不會被裁切掉。 我們建議您在螢幕邊緣的 5% 以內，只繪製不重要的視覺效果。 如[調整 UI 元素大小](#ui-element-sizing)所述，遵循 Xbox One 主機預設縮放比例 200% 的 UWP app 會使用 960 x 540 epx 的區域，所以在您的 app UI 中，您應該避免在下列區域中放置重要的 UI：

- 從頂端和底端算起 27 epx
- 從左邊和右邊算起 48 epx

有兩種方式可以讓 UI 延伸到螢幕邊緣：「核心視窗界限」**和「負邊界」**。

### 核心視窗界限

對於只針對 10 英呎體驗的 UWP app，使用核心視窗界限是比較簡單的選項。

在 `App.xaml.cs` 的 `OnLaunched` 方法中，新增下列程式碼：

```csharp
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode
    (Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```

有了這行程式碼，app 視窗會延伸到螢幕邊緣，所以您必須將所有互動式和重要 UI 移入稍早所述的電視安全區域中。 暫時性 UI (例如操作功能表和開啟的[下拉式方塊](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.combobox.aspx)) 會自動留在電視安全區域內。

![核心視窗界限](images/designing-for-tv/core-window-bounds.png)

### 負邊界

對於針對各種裝置 (例如行動裝置、桌上型電腦及 Xbox One) 的 UWP app，要量身打造調適型配置，負邊界是比較直覺的方法。 
我們建議您建立[自訂的觸發程序](#custom-visual-state-trigger-for-xbox-one)，針對電視配置修改邊界。

#### 窗格背景 

瀏覽窗格通常會繪製在螢幕邊緣附近，所以背景應該要延伸到電視不安全區域，以免產生不適當的間距。 
您可在 [SplitView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.splitview.aspx) 控制項上使用負邊界執行此動作 (此控制項常做為瀏覽窗格建置組塊)，對 `SplitView` 的內容使用正邊界，讓內容保持在電視安全區域內。

![延伸到螢幕邊緣的瀏覽窗格](images/designing-for-tv/tv-safe-areas-2.png)

現在，瀏覽窗格的背景已經延伸到螢幕邊緣，同時瀏覽項目也保留在電視安全區域內。 
`SplitView` 的內容 (此案例為項目的格線) 延伸到螢幕底部，所以看起來是持續往下而沒有被截斷，同時格線頂端也仍然在電視安全區域內。 本節稍後您將了解如何在電視安全區域中保留擁有焦點的項目。

下列程式碼片段可達到這個效果︰

```xml
<SplitView x:Name="RootSplitView"
           Margin="-48, -27">
            <SplitView.Pane>
                 <ListView x:Name="NavMenuList"
                           Margin="0,75,0,27"
                           ContainerContentChanging=
                                "NavMenuItemContainerContentChanging"
                           ItemContainerStyle="{StaticResource 
                                NavMenuItemContainerStyle}"
                           ItemTemplate="{StaticResource NavMenuItemTemplate}"
                           ItemInvoked="NavMenuList_ItemInvoked"/>
            </SplitView.Pane>
            <Frame x:Name="frame"
                   Margin="0,27,48,27"
                   Navigating="OnNavigatingToPage"
                   Navigated="OnNavigatedToPage"/>
    </SplitView>
```

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 是另一個常放在 app 一邊或多邊附近的窗格範例，也因此其背景應該延伸到電視螢幕邊緣。 其通常亦包含 [更多]**** 按鈕 (在右側以「...」顯示)，且此按鈕應會留在電視安全區域中。 以下是達到所需互動和視覺效果的幾個不同的策略。

**選項 1**︰將 `CommandBar` 背景色彩變更為透明或與頁面背景相同的色彩︰

```xml
<CommandBar x:Name="topbar" 
            Background="{ThemeResource SystemControlBackgroundAltHighBrush}">
            ...
</CommandBar>
```

如此一來會讓 `CommandBar` 看起來就像在頁面相同背景的頂端，背景就能順暢地延伸到螢幕邊緣。

**選項 2**︰新增一個背景矩形，其填滿是與 `CommandBar` 背景相同的色彩，並且使用負邊界延伸到螢幕邊緣︰

```xml
<Rectangle VerticalAlignment="Top" 
            HorizontalAlignment="Stretch" 
            Margin="0,-27,-48,0"      
            Fill="{ThemeResource SystemControlBackgroundChromeMediumBrush}"/>
<CommandBar x:Name="topbar" 
            VerticalAlignment="Top" 
            HorizontalContentAlignment="Stretch">
            ...
</CommandBar>
```

> **注意** &nbsp;&nbsp;如果使用這個方法，請注意 [更多]**** 按鈕會變更開啟之 `CommandBar` 的高度 (如有需要)，以便在 `AppBarButton` 的圖示之下顯示其標籤。 我們建議您將標籤移動到其圖示的「右側」**，避免發生調整大小的情形。

#### 背景影像和媒體元素

大部分的影像和其他媒體元素在邊緣不會包含重要資訊，所以將這些 UI 元素繪製到螢幕邊緣提供身歷其境的體驗是安全的作法。 下列程式碼片段示範將影像繪製到螢幕邊緣的範例︰

```xml
<Image Source="\Assets\HeaderBackground.png" 
       Stretch="Uniform" 
       Height="227" 
       VerticalAlignment="Top" 
       Margin="-48,-27,-48,0"/>
```

您也可以針對媒體 (例如影片) 執行相同的動作。

#### 捲動清單和格線的尾端

清單和格線經常會包含一個螢幕無法同時容納的多個項目。 發生這種情況時，我們建議您將清單或格線延伸到螢幕邊緣。 水平捲動清單和格線時應該向右邊延伸，垂直捲動時應該向下延伸。

![電視安全區域格線截斷](images/designing-for-tv/tv-safe-area-grid-cutoff.png)

雖然清單或格線會像這樣延伸，但是請務必讓視覺焦點及其關聯的項目保留在電視安全區域內。

![捲動格線焦點應保留在電視安全區域內](images/designing-for-tv/scrolling-grid-focus.png)

UWP 具有可將視覺焦點保留在 [VisibleBounds](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.applicationview.visiblebounds.aspx) 內的功能，但是您必須新增邊框間距，以確保清單/格線項目可以捲動到安全區域的視野範圍內。 具體來說，您要對 [ListView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 或 [GridView](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 的 [ItemsPresenter](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) 新增正邊界，如下列程式碼片段所示︰

```xml
<Style x:Key="TitleSafeListViewStyle" 
       TargetType="ListView">
    <Setter Property="Margin" 
            Value="0,0,0,-27"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="ListView">
                    <Border BorderBrush="{TemplateBinding BorderBrush}" 
                            Background="{TemplateBinding Background}" 
                            BorderThickness="{TemplateBinding BorderThickness}">
                        <ScrollViewer x:Name="ScrollViewer"
                                      TabNavigation="{TemplateBinding TabNavigation}"
                                      HorizontalScrollMode="{TemplateBinding ScrollViewer.HorizontalScrollMode}"
                                      HorizontalScrollBarVisibility="{TemplateBinding ScrollViewer.HorizontalScrollBarVisibility}"
                                      IsHorizontalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsHorizontalScrollChainingEnabled}"
                                      VerticalScrollMode="{TemplateBinding ScrollViewer.VerticalScrollMode}"
                                      VerticalScrollBarVisibility="{TemplateBinding ScrollViewer.VerticalScrollBarVisibility}"
                                      IsVerticalScrollChainingEnabled="{TemplateBinding ScrollViewer.IsVerticalScrollChainingEnabled}"
                                      IsHorizontalRailEnabled="{TemplateBinding ScrollViewer.IsHorizontalRailEnabled}"
                                      IsVerticalRailEnabled="{TemplateBinding ScrollViewer.IsVerticalRailEnabled}"
                                      ZoomMode="{TemplateBinding ScrollViewer.ZoomMode}"
                                      IsDeferredScrollingEnabled="{TemplateBinding ScrollViewer.IsDeferredScrollingEnabled}"
                                      BringIntoViewOnFocusChange="{TemplateBinding ScrollViewer.BringIntoViewOnFocusChange}"
                                      AutomationProperties.AccessibilityView="Raw">
                            <ItemsPresenter Header="{TemplateBinding Header}"
                                            HeaderTemplate="{TemplateBinding HeaderTemplate}"
                                            HeaderTransitions="{TemplateBinding HeaderTransitions}"
                                            Footer="{TemplateBinding Footer}"
                                            FooterTemplate="{TemplateBinding FooterTemplate}"
                                            FooterTransitions="{TemplateBinding FooterTransitions}"
                                            Padding="{TemplateBinding Padding}" 
                                            Margin="0,27,0,27"/>
                    </ScrollViewer>
                </Border>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

您可以將前面的程式碼片段放在頁面或 app 資源中，然後再透過下列方式存取︰

```xml
<Page>
    <Grid>
        <ListView Style="{StaticResource TitleSafeListViewStyle}"
                  ... />
```

> **注意** &nbsp;&nbsp;這個程式碼片段是特別針對 `ListView`；如果是 `GridView` 樣式，請為 [ControlTemplate](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.aspx) 和 [Style](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.style.aspx) 將 [TargetType](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.controltemplate.targettype.aspx) 屬性設定為 `GridView`。


### 適用於 Xbox One 的自訂視覺狀態觸發程序 <a name="custom-visual-state-trigger-for-xbox-one"></a>

若要針對 10 英呎體驗量身打造您的 UWP app，建議您在 app 偵測到在 Xbox 主機上啟動時變更配置。 這可以透過使用自訂視覺狀態觸發程序進行，如下列程式碼片段所示︰

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <triggers:DeviceFamilyTrigger DeviceFamily="Windows.Xbox"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="RootSplitView.Margin" 
                        Value="-48,-27"/>
                <Setter Target="RootSplitView.OpenPaneLength" 
                        Value="368"/>
                <Setter Target="RootSplitView.CompactPaneLength" 
                        Value="96"/>
                <Setter Target="NavMenuList.Margin" 
                        Value="0,75,0,27"/>
                <Setter Target="Frame.Margin" 
                        Value="0,27,48,27"/>
                <Setter Target="NavMenuList.ItemContainerStyle" 
                        Value="{StaticResource NavMenuItemContainerXboxStyle}"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

若要建立觸發程序，請將下列類別新增到您的 app。 這是稍早的 XAML 程式碼所參考的類別︰

```csharp
class DeviceFamilyTrigger : StateTriggerBase
{
    private string _currentDeviceFamily, _queriedDeviceFamily;

    public string DeviceFamily
    {
        get
        {
            return _queriedDeviceFamily;
        }
        
        set
        {
            _queriedDeviceFamily = value;
            _currentDeviceFamily = AnalyticsInfo.VersionInfo.DeviceFamily;
            SetActive(_queriedDeviceFamily == _currentDeviceFamily);
        }
    }
}
```

新增自訂觸發程序之後，每當您的 app 偵測到它在 Xbox One 主機上執行時，就會自動修改您在 XAML 程式碼中指定的配置。

## 色彩

通用 Windows 平台預設不會執行任何動作來修改 app 的色彩。 也就是說，您的 app 可以採取一些色彩設定的增強功能，來改善電視上的視覺體驗。

### 應用程式佈景主題

您可以根據適合 app 的方式選擇「應用程式佈景主題」**** (深色或淺色)，或者選擇不使用佈景主題。 請在[色彩佈景主題](../style/color.md#color-themes)中閱讀更多有關佈景主題的一般建議。

UWP 也能讓 app 根據執行的裝置所提供的系統設定，動態設定佈景主題。 
雖然 UWP 一律會優先採用使用者指定的佈景主題設定，但是每個裝置也會提供適當的預設佈景主題。 
由於 Xbox One 的「媒體」**體驗要求比「生產力」**體驗要求還高，因此預設為深色的系統佈景主題。 
如果您的 app 佈景主題是根據系統設定，在 Xbox One 上就會預設為深色。

### 輔色

UWP 提供一個很方便的方式可以公開使用者從其系統設定選取的「輔色」****。

使用者可以在 Xbox One 上選取使用者的色彩，就如同在電腦上選取輔色一樣。 
只要您的 app 透過筆刷或色彩資源呼叫這些輔色，就會採用使用者在系統設定中選取的色彩。 請注意，Xbox One 上的輔色依使用者 (不是依系統) 而定。

另外，Xbox One 上的使用者色彩組與電腦、手機和其他裝置上的不同。 
部分原因是因為這些色彩在 Xbox One 上是為了擁有最佳 10 英呎體驗，依照本文所述的方法和策略而手動挑選的。

只要您的 app 使用筆刷資源 (例如 **SystemControlForegroundAccentBrush**) 或色彩資源 (**SystemAccentColor**)，或直接透過 [UIColorType.Accent*](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.viewmanagement.uicolortype.aspx) API 改為呼叫輔色，這些色彩就會取代為適合電視的輔色。 高對比的筆刷色彩也會從系統 (就像在電腦和手機上一樣) 提取，但是是使用適合電視的色彩。

若要深入了解一般的輔色，請參閱[輔色](../style/color.md#accent-color)。

### 電視之間的色彩差異

針對電視設計時，請注意色彩顯示的方式依照所呈現的電視而相當不同。 不要認為色彩會完全像在監視器上顯示的一樣。 如果您的 app 依賴細微的色彩差異來區分 UI，色彩可能會混合在一起，而讓使用者感到困惑。 無論使用的電視為何，請試著使用足以讓使用者能夠清楚分辨的色彩。

### 電視安全色彩

色彩的 RGB 值代表紅色、綠色及藍色的濃度。 電視無法處理極端的濃度值；因此，您在針對 10 英呎體驗設計時，應該避免使用這些色彩。 它們可以在某些電視上會產生奇異的帶狀效果，或者看起來像褪色一樣。 此外，高濃度色彩可能會導致開花的效果 (附近的像素開始繪製相同的色彩)。 

雖然不同思想學派對於何謂電視安全色彩有不同的意見，但是 16-235 (或以十六進位表示的 10-EB) 的 RGB 值內的色彩通常可安全地用於電視。

![電視安全色彩範圍](images/designing-for-tv/tv-safe-colors.png)

### 修正電視不安全色彩

藉由調整其 RGB 值，分別將電視不安全色彩修正到電視安全範圍內，通常稱為「色彩鉗制」****。 這個方法可能適合調色盤色彩不豐富的 app。 不過，使用此方法修正色彩可能會造成色彩彼此之間互相衝突，而無法提供最佳的 10 英呎體驗。

若要最佳化電視的調色盤，建議您先透過諸如色彩鉗制等方法確認色彩是電視安全色彩，然後再改用**縮放**方法。

這會將所有色彩的 RGB 值都調整某個特定比例而落在電視安全範圍內。 調整 app 的所有色彩有助於防止色彩發生衝突，以獲得更好的 10 英呎體驗。

![鉗制與縮放](images/designing-for-tv/clamping-vs-scaling.png)

### 資產

對色彩進行變更時，請務必要一併更新資產。 如果您的 app 使用 XAML 中看起來與資產色彩相同的色彩，但您只是更新 XAML 程式碼，您的資產色彩可能會不好看。

### UWP 色彩範例

[UWP 色彩佈景主題](../style/color.md#color-themes)針對深色佈景主題將 app 的背景設計為「黑色」****，針對淺色佈景主題設計為「白色」****。 因為黑色或白色都不是電視安全色彩，所以這些色彩都必須使用「鉗制」**修正。 這些色彩修正之後，所有其他色彩都必須透過「縮放」**調整，以保留必要的對比。

<!--[v-lcap to eliot]why is the above paragraph in the past tense?-->
<!--[elcowle] Because this is something that Microsoft had to do to the UWP color themes to accommodate TV-safe colors for Xbox. These themes are then provided in the below code sample.-->

下列範例程式碼提供一個已針對電視使用最佳化的色彩佈景主題︰

```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FF101010"/>
                <Color x:Key="SystemAltHighColor">#FF101010</Color>
                <Color x:Key="SystemAltLowColor">#33101010</Color>
                <Color x:Key="SystemAltMediumColor">#99101010</Color>
                <Color x:Key="SystemAltMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemAltMediumLowColor">#66101010</Color>
                <Color x:Key="SystemBaseHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemBaseLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemBaseMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemChromeAltLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FF333333</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF858585</Color>
                <Color x:Key="SystemChromeHighColor">#FF767676</Color>
                <Color x:Key="SystemChromeLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeMediumColor">#FF262626</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FF2B2B2B</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19EBEBEB</Color>
                <Color x:Key="SystemListMediumColor">#33EBEBEB</Color>
            </ResourceDictionary>
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="ApplicationPageBackgroundThemeBrush" 
                                 Color="#FFEBEBEB" /> 
                <Color x:Key="SystemAltHighColor">#FFEBEBEB</Color>
                <Color x:Key="SystemAltLowColor">#33EBEBEB</Color>
                <Color x:Key="SystemAltMediumColor">#99EBEBEB</Color>
                <Color x:Key="SystemAltMediumHighColor">#CCEBEBEB</Color>
                <Color x:Key="SystemAltMediumLowColor">#66EBEBEB</Color>
                <Color x:Key="SystemBaseHighColor">#FF101010</Color>
                <Color x:Key="SystemBaseLowColor">#33101010</Color>
                <Color x:Key="SystemBaseMediumColor">#99101010</Color>
                <Color x:Key="SystemBaseMediumHighColor">#CC101010</Color>
                <Color x:Key="SystemBaseMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeAltLowColor">#FF1F1F1F</Color>
                <Color x:Key="SystemChromeBlackHighColor">#FF101010</Color>
                <Color x:Key="SystemChromeBlackLowColor">#33101010</Color>
                <Color x:Key="SystemChromeBlackMediumLowColor">#66101010</Color>
                <Color x:Key="SystemChromeBlackMediumColor">#CC101010</Color>
                <Color x:Key="SystemChromeDisabledHighColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeDisabledLowColor">#FF7A7A7A</Color>
                <Color x:Key="SystemChromeHighColor">#FFB2B2B2</Color>
                <Color x:Key="SystemChromeLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeMediumColor">#FFCCCCCC</Color>
                <Color x:Key="SystemChromeMediumLowColor">#FFDDDDDD</Color>
                <Color x:Key="SystemChromeWhiteColor">#FFEBEBEB</Color>
                <Color x:Key="SystemListLowColor">#19101010</Color>
                <Color x:Key="SystemListMediumColor">#33101010</Color>
            </ResourceDictionary> 
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

> **注意** &nbsp;&nbsp;淺色佈景主題 **SystemChromeMediumLowColor** 和 **SystemChromeMediumLowColor** 是刻意使用相同的色彩，而非鉗制的結果所造成。 

> **注意** &nbsp;&nbsp;十六進位色彩以 **ARGB** 指定 (Alpha 紅色綠色藍色)。

我們不建議在能夠顯示完整範圍而不需使用鉗制的顯示器上使用電視安全色彩，因為色彩看起來會像褪色一樣。 當您的 app 在 Xbox 上而「不是」**其他平台上執行時，請載入資源字典 (上一個範例)。 在 `App.xaml.cs` 的 `OnLaunched` 方法中，新增下列檢查：

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Xbox")
{ 
    this.Resources.MergedDictionaries.Add(new ResourceDictionary 
    { 
        Source = new Uri("ms-appx:///TenFootStylesheet.xaml") 
    }); 
}
```

這可確保無論 app 在哪個裝置上執行，都能顯示正確的色彩，提供使用者更好、更賞心悅目的體驗。

## UI 控制項的指導方針

提供數種可針對多部裝置良好運作的 UI 控制項，但當在電視上使用時具有特定考量。 深入了解有關專為 10 英呎體驗設計時使用這些控制項的一些最佳做法。

### Pivot 控制項

[Pivot](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.pivot.aspx) 控制項有一些您可以設定的屬性，像是標題不會像在手機與平板電腦上在螢幕周圍換行。 對於大型螢幕顯示 (例如電視) 這是更好的體驗，因為標題換行可能會讓使用者分心。 如需詳細資訊，請參閱[索引標籤和樞紐](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tabs-pivot)。

### 瀏覽窗格

UWP 可在所有裝置上顯示一致的外觀及操作方式。 如需關於瀏覽窗格在不同螢幕中如何有不同行為的詳細資訊，以及針對遊戲台/遙控器瀏覽的最佳做法，請參閱[瀏覽窗格](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/nav-pane)。

### CommandBar 標籤

[CommandBar](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.commandbar.aspx) 控制項有一個屬性會導致圖示旁邊的標籤一律顯示。 這非常適用於 10 英呎體驗，因為它讓使用者能以最少的點選次數查看按鈕執行的動作。 這對於其他裝置類型而言也是很好的模型。

### 工具提示

[Tooltip](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.tooltip.aspx) 控制項是為了當使用者將滑鼠停留在元素上，或者點選並按住元素上的圖形時，在 UI 中提供更多資訊。 對於遊戲台和遙控器，當元素取得焦點一小段時間之後，`Tooltip` 就會出現在螢幕上停留一小段時間，然後消失。 如果使用了太多 `Tooltip`，這種行為可能會讓使用者分心。 針對電視設計時，請嘗試避免使用工具提示。

### 按鈕樣式

雖然標準的 UWP 按鈕能在電視上順利運作，但是一些按鈕視覺樣式更能讓 UI 吸引注意，您可以針對所有平台考慮這些樣式，特別是在 10 英呎體驗中，更需要清楚傳達焦點所在。 若要深入了解這些樣式，請參閱[按鈕](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/buttons)。

### 巢狀的 UI 元素

當 UI 元素巢狀於其他 UI 元素內部時，使用者預設無法存取巢狀的 UI 元素。

其中一個主要案例是當使用者將滑鼠停留在巢狀的 UI 元素上時才顯示 UI，否則不會顯示。

![滑鼠停留在上面時顯示的 UI 元素](images/designing-for-tv/2d-navigation-best-practices-ui-elements-display-on-mouse-hover.png)

針對遊戲台/遙控器輸入處理此種案例的建議方式是將這些 UI 元素放在 `ContextFlyout` 中。

## 摘要

針對 10 英呎經驗的設計已納入特殊考量，使其有別於其他所有平台的設計。 您當然可以直接將 UWP app 移植至 Xbox One 且它亦可運作，但它不一定是針對 10 英呎體驗最佳化，且會讓使用者感到失望。 遵循本文所述的指導方針，以確定 app 在電視上仍可呈現優異效果。

## 相關文章

- [通用 Windows 平台 (UWP) 應用程式的裝置基本資訊](device-primer.md)
- [遊戲台與遙控器的互動](gamepad-and-remote-interactions.md)
- [UWP app 中的音效](../style/sound.md)


<!--HONumber=May16_HO2-->


