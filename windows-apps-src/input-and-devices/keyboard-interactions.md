---
author: Karl-Bridge-Microsoft
Description: 同時使用鍵盤與類別事件處理常式，回應應用程式中從硬體或軟體鍵盤輸入的按鍵動作。
title: 鍵盤互動
ms.assetid: FF819BAC-67C0-4EC9-8921-F087BE188138
label: Keyboard interactions
template: detail.hbs
---

# 鍵盤互動


鍵盤輸入是 app 之整體使用者互動經驗的一個重要部分。 對於某些行動不便的人或認為使用鍵盤與 App 互動較有效率的使用者來說，鍵盤是不可或缺的。 例如，使用者必須能夠使用 Tab 和方向鍵來瀏覽您的 app、使用空格鍵和 Enter 鍵來啟用 UI 元素，以及使用鍵盤快速鍵來存取命令。
![鍵盤主角影像](images/input-patterns/input-keyboard-small.jpg)



**重要 API**

-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)
-   [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072)


設計良好的鍵盤 UI 是軟體協助工具的一個重要層面。 它讓視障使用者或受到某種程度運動神經傷害的使用者能夠瀏覽應用程式並與應用程式的功能互動。 這類使用者有可能無法使用滑鼠，因而必須仰賴像鍵盤增強功能工具、螢幕小鍵盤、螢幕放大機、螢幕助讀程式以及語音輸入公用程式這類協助技術。

使用者與 Universal app 互動的方式，可以透過硬體鍵盤和兩種軟體鍵盤：螢幕小鍵盤 (OSK) 和觸控式鍵盤。

<span></span>螢幕小鍵盤  
螢幕小鍵盤是視覺化的軟體鍵盤，可用來取代實體鍵盤，透過觸控、滑鼠、畫筆/手寫筆或其他指標裝置 (不需要觸控式螢幕) 來輸入資料。 提供螢幕小鍵盤的用意是針對沒有實體鍵盤的系統，或者使用者因行動不便而無法使用傳統實體輸入裝置的情況。 螢幕小鍵盤會模擬絕大部分的硬體鍵盤功能。

螢幕小鍵盤可以從 [設定] &gt; [輕鬆存取] 中的 [鍵盤] 頁面開啟。

**注意** 螢幕小鍵盤的優先順序高於觸控式鍵盤，如果顯示螢幕小鍵盤，就不會顯示觸控式鍵盤。

 

![螢幕小鍵盤](images/input-patterns/osk.png)

<sup>螢幕小鍵盤</sup>

<span id="Touch_keyboard"></span><span id="touch_keyboard"></span><span id="TOUCH_KEYBOARD"></span>觸控式鍵盤  
觸控式鍵盤是視覺化的軟體鍵盤，使用觸控輸入來輸入文字。 它只能輸入文字 (不模擬硬體鍵盤)，無法取代螢幕小鍵盤。

根據裝置而定，當文字欄位或其他可編輯的文字控制項取得焦點時，或當使用者透過**通知中心**以手動方式啟用時，觸控式鍵盤就會出現：

![[通知中心] 的觸控式鍵盤圖示](images/input-patterns/touch-keyboard-notificationcenter.png)

**注意** 使用者可能需要移至[設定] &gt; [系統] 中的 [**平板電腦模式**] 畫面，並開啟 [在將裝置做為平板電腦使用時，讓 Windows 可更容易使用觸控方式操控]，觸控式鍵盤才會自動出現。

 

如果您的 app 以程式設計方式將焦點設為文字輸入控制項，則不會叫用觸控式鍵盤。 這可消除不是由使用者直接進行的非預期行為。 不過，當焦點透過程式設計方式移至非文字輸入控制項時，鍵盤會自動隱藏。

當使用者在表單中的控制項之間瀏覽時，觸控式鍵盤通常會保持可見。 這種行為可能會因表單內的其他控制項類型而有所不同。

下面所列的非編輯控制項可以在文字輸入工作階段使用觸控式鍵盤接受焦點，而不用關閉鍵盤。 觸控式鍵盤會持續留在檢視中，不會無意義地變換 UI 讓使用者感到混淆，因為使用者可能會使用觸控式鍵盤，在控制項與文字項目之間做來回切換。

-   核取方塊
-   下拉式方塊
-   選項按鈕
-   捲軸
-   樹狀目錄
-   樹狀目錄項目
-   功能表
-   功能表列
-   功能表項目
-   工具列
-   清單
-   清單項目

以下是觸控式鍵盤各種模式的範例。 第一個影像是預設版面配置，第二個是拇指版面配置 (不是所有語言都有提供)。

以下是觸控式鍵盤各種模式的範例。 第一個影像是預設版面配置，第二個是拇指版面配置 (不是所有語言都有提供)。
<table>
<tr>
    <td>**預設版面配置模式的觸控式鍵盤：  **</td>
    <td>![the touch keyboard in default layout mode](images/touchkeyboard-standard.png)</td>
</tr>
<tr>
    <td>**展開版面配置模式的觸控式鍵盤：  **</td>
    <td>![the touch keyboard in expanded layout mode](images/touchkeyboard-expanded.png)</td>
</tr>
<tr>
    <td>**預設姆指版面配置模式的觸控式鍵盤：  **</td>
    <td>![the touch keyboard in thumb layout mode](images/touchkeyboard-thumb.png)</td>
</tr>
<tr>
    <td>**數字姆指版面配置模式的觸控式鍵盤：  **</td>
    <td>![the touch keyboard in numeric thumb layout mode](images/touchkeyboard-numeric-thumb.png)</td>
</tr>
</table>


成功的鍵盤互動讓使用者只利用鍵盤就可以完成基本的應用程式操作；也就是說，使用者可以使用所有的互動式元素以及啟動預設的功能。 有許多因素會影響成功的程度，包括鍵盤瀏覽、協助工具的便捷鍵，以及進階使用者的快速鍵。

**注意** 觸控式鍵盤不支援切換和大多數的系統命令 (請參閱[模式](#keyboard_command_patterns))

## <span id="Navigation"></span><span id="navigation"></span><span id="NAVIGATION"></span>瀏覽


若要搭配使用控制項 (包括瀏覽元素) 與鍵盤，控制項必須有焦點。 控制項接受鍵盤焦點的其中一種方式就是透過 Tab 瀏覽進行存取。 設計良好的鍵盤瀏覽模型會提供有邏輯且可預期的定位順序，讓使用者得以快速而有效率地瀏覽和使用您的應用程式。

所有的互動式控制項都應該有定位停駐點 (除非它們在群組中)，而非互動式控制項 (例如標籤) 則沒有。

可以將一組相關的控制項組成控制項群組並指派其定位停駐點。 控制項群組用於運作方式如同單一控制項的控制項集合，例如選項按鈕。 太多控制項時也會使用控制項群組，以便單獨使用 Tab 鍵進行有效率的瀏覽。 方向鍵、Home、End、Page Up 和 Page Down 會在群組中的控制項之間移動輸入焦點 (不可能使用這些按鍵從控制項群組進行瀏覽)。

當應用程式啟動時，您應將起始鍵盤焦點設在使用者最先直覺 (或最有可能) 互動的元素上。 通常，這是應用程式的主要內容檢視，這樣使用者就能立即開始使用方向鍵來捲動應用程式內容。

請勿在可能有負面或甚至悲慘結果的元素上設定起始鍵盤焦點。 這可避免遺失資料或系統存取權。

試著先同時依定位順序和顯示順序 (或視覺階層)排列並呈現最重要的命令、控制項及內容。 不過，實際的顯示位置可能取決於上層配置容器，以及會影響配置之子元素的某些屬性。 特別的是，使用格線隱喻或表格隱喻的配置可以有與定位順序完全不同的讀取順序。 這不一定是問題，但在測試應用程式的功能時應同時以可觸控 UI 和鍵盤存取 UI 的形式進行測試。

可能的話，定位順序應遵循讀取順序。 這可以減少混淆並視地區設定和語言而定。

請設定鍵盤按鈕與應用程式中適當 UI (上一頁和下一頁按鈕) 的關聯。

試著瀏覽回到應用程式的 [開始] 畫面，並且盡可能以簡單明瞭的方式瀏覽關鍵內容。

使用方向鍵做為鍵盤快速鍵，以便在複合元素中的各個子元素之間瀏覽。 如果樹狀檢視節點使用獨立的子元素來處理展開折疊以及節點啟動，請使用向左鍵或向右鍵，提供鍵盤展開折疊功能。 這與平台控制項一致。

因為觸控式鍵盤會遮住大部分的螢幕，所以通用 Windows 平台 (UWP) 可確保當使用者瀏覽表單的控制項時 (包括目前不在檢視中的控制項)，輸入欄位是在捲動檢視的焦點。 自訂控制項應模擬這個行為。

![顯示和未顯示觸控式鍵盤的表單](images/input-patterns/touch-keyboard-pan1.png)

在某些情況下，有些 UI 元素應該一直停留在畫面上。 設計 UI 以讓移動瀏覽區域中包含表單控制項，並讓重要的 UI 元素處於靜態。 例如：

![表單中包含應永久留在檢視中的區域](images/input-patterns/touch-keyboard-pan2.png)
## <span id="Activation"></span><span id="activation"></span><span id="ACTIVATION"></span>啟用


不論控制項目前是否有焦點，都可以用許多不同的方式加以啟用。

<span id="Spacebar__Enter__and_Esc"></span><span id="spacebar__enter__and_esc"></span><span id="SPACEBAR__ENTER__AND_ESC"></span>空格鍵、Enter 鍵和 Esc 鍵  
空格鍵應啟用具有輸入焦點的控制項。 Enter 鍵應啟用預設控制項或具有輸入焦點的控制項。 預設控制項是具有起始焦點的控制項或是專門回應 Enter 鍵的控制項 (通常會隨著輸入焦點變更)。 此外，Esc 鍵應關閉或結束暫時性 UI，例如功能表和對話方塊。

以下顯示的小算盤 app 使用空格鍵來啟用具有焦點的按鈕、將 Enter 鍵鎖定為 "=" 按鈕，以及將 Esc 鍵鎖定為 "C" 按鈕。

![小算盤 app](images/input-patterns/calculator.png)

<span id="Keyboard_modifiers"></span><span id="keyboard_modifiers"></span><span id="KEYBOARD_MODIFIERS"></span>鍵盤修飾詞  
鍵盤修飾詞分成下列類別： 

 
| 類別 | 描述 | 
|----------|-------------| 
| 快速鍵 | 不使用 UI 執行常見動作，例如 "Ctrl-S" 表示 [**儲存**]。 實作主 app 功能的鍵盤快速鍵。 並非每個命令都有或需要快速鍵。 |   
| 便捷鍵/熱鍵 | 指派給每個可見的最上層控制項，例如 "Alt-F" 表示 [**檔案**] 功能表。 便捷鍵不會叫用或啟用命令。 |
| 快速鍵 | 執行預設系統或 app 定義的命令，例如 "Alt-PrtScrn" 表示擷取螢幕畫面、"Alt-Tab" 切換 app，或 "F1" 取得說明。 與快速鍵相關聯的命令不需要是功能表項目。 |
| 應用程式鍵/功能表鍵 | 顯示操作功能表。 |
| Window 鍵/Command 鍵 | 啟用系統命令，例如 [**系統功能表**]、[**鎖定畫面**] 或 [**顯示桌面**] |

便捷鍵和快速鍵支援直接與控制項互動，而不是使用 Tab 鍵瀏覽。
> 雖然某些控制項有內部標籤 (例如命令按鈕、核取方塊和選項按鈕)，但是其他控制項有外部標籤 (例如清單檢視)。 對於有外部標籤的控制項，便捷鍵會被指派給標籤，若叫用該標籤，則會將焦點設定為相關聯控制項內的元素或值。


以下範例顯示 **Word** 中 [**版面配置**] 索引標籤的便捷鍵

![Word 中 [版面配置] 索引標籤的便捷鍵](images/input-patterns/accesskeys-show.png)

如下所示，在輸入相關聯標籤中識別的便捷鍵後，會反白顯示 [左邊縮排] 文字欄位值。

![在輸入相關聯標籤中識別的便捷鍵後，會反白顯示 [左邊縮排] 文字欄位值。](images/input-patterns/accesskeys-entered.png)

## <span id="Usability_and_accessibility"></span><span id="usability_and_accessibility"></span><span id="USABILITY_AND_ACCESSIBILITY"></span>可用性和協助工具


設計良好的鍵盤互動體驗是軟體協助工具的一個重要層面。 它讓視障使用者或受到某種程度運動神經傷害的使用者能夠瀏覽應用程式並與應用程式的功能互動。 這類使用者有可能無法使用滑鼠，因而必須仰賴各項協助技術 (包含鍵盤增強功能工具和螢幕小鍵盤，以及螢幕放大機、螢幕助讀程式和語音輸入公用程式)。 對於這些使用者而言，完整性是比一致性更重要。

經驗豐富的使用者通常極度偏好使用鍵盤，因為鍵盤式命令的輸入更快速，而且不需要從鍵盤移開其雙手。 對於這些使用者而言，效率與一致性非常重要；完整性只對最常用的命令很重要。

設計可用性和協助工具時有些微差異，這就是為何要支援這兩種不同的鍵盤存取機制。

便捷鍵具有下列特性：

-   便捷鍵是應用程式的 UI 元素捷徑。
-   它們使用 Alt 鍵和英數字元按鍵。
-   它們是主要的協助工具。
-   它們會被指派給所有功能表和大部分的對話方塊控制項。
-   它們不需要被記住，所以會藉由在對應的控制項標籤字元加底線，直接記載於 UI。
-   它們只在目前的視窗中有作用，可瀏覽到對應的功能表項目或控制項。
-   因為它們不一定會一致，所以不會以一致的方式指派。 不過，應針對常用的命令 (尤其是認可按鈕)，以一致的方式指派便捷鍵。
-   它們會當地語系化。

因為便捷鍵不需要被記住，所以會被指派給先前在標籤中的字元以便輕易尋找，即使有稍後出現在標籤中的關鍵字。

相反地，快速鍵具有下列特性：

-   「快速鍵」是應用程式命令的捷徑。
-   它們主要使用 Ctrl 和功能鍵序列 (Windows 系統快速鍵也使用 Alt+非英數字元鍵和 Windows 標誌鍵)。
-   它們主要用於提升進階使用者的效率。
-   它們只會指派給最常用的命令。
-   它們需要被記住，而且只會記載於功能表、工具提示和說明。
-   它們會影響整個程式，但未套用時沒有作用。
-   它們需要被記住且未直接記載，所以必須以一致的方式指派。
-   它們不會當地語系化。

因為快速鍵需要被記住，所以最常用的快速鍵最好使用命令的關鍵字中第一個或最令人印象深刻的字元，例如 Ctrl+C 代表「複製」而 Ctrl+Q 代表「要求」。

使用者必須能夠單獨使用硬體鍵盤或螢幕小鍵盤，完成應用程式支援的所有工作。

請為依賴螢幕助讀程式或其他輔助技術的使用者，提供一種便利的方法，讓他們找到應用程式的快速鍵。 使用工具提示、無障礙名稱、無障礙說明或其他螢幕上的溝通方式，與快速鍵進行溝通。 至少應在應用程式的說明內容中詳細記載便捷鍵和快速鍵。

請勿將知名或標準快速鍵指派給其他功能。 例如，Ctrl + F 通常用於尋找或搜尋。

請不要嘗試將便捷鍵指派給密集 UI 中的所有互動式控制項。 只要確定最重要和最常用的控制項有便捷鍵，或使用控制項群組並將便捷鍵指派給控制項群組標籤。

請勿使用鍵盤輔助鍵變更命令。 這麼做不會被發現並可能造成混淆。

請勿停用具有輸入焦點的控制項。 這可能會干擾鍵盤輸入。

若要確保成功的鍵盤互動體驗，請務必使用鍵盤以獨佔方式徹底測試您的應用程式。

## <span id="Text_input"></span><span id="text_input"></span><span id="TEXT_INPUT"></span>文字輸入


仰賴鍵盤輸入時，一律查詢裝置功能。 在某些裝置 (例如手機) 上，觸控式鍵盤只能用於文字輸入，因為它不像硬體鍵盤會提供許多快速鍵或命令鍵 (例如 alt、功能鍵或 Windows 標誌鍵)。

不要讓使用者使用觸控式鍵盤瀏覽應用程式。 視取得焦點的控制項而定，觸控式鍵盤可能會關閉。

嘗試在與您的表單互動時全程顯示鍵盤。 這可消除會讓使用者在表單或文字輸入流程當中感到迷惘的 UI 變換。

確保使用者一律可以看到正在輸入的輸入欄位。 觸控式鍵盤會遮住一半的畫面，所以取得焦點的輸入欄位應在使用者周遊表單時捲動到檢視中。

標準硬體鍵盤或 OSK 包含七種按鍵，每種按鍵都支援獨特的功能：

-   字元鍵：將常值字元傳送到具有輸入焦點的視窗。
-   輔助鍵：同時按下時，會改變主要按鍵的功能，例如 Ctrl、Alt、Shift 及 Windows 標誌鍵。
-   瀏覽鍵：移動輸入焦點或文字輸入位置，例如 Tab、Home、End、Page Up、Page Down 以及方向鍵。
-   編輯鍵：操作文字，例如 Shift、Tab、Enter、Insert、Backspace 和 Delete 鍵。
-   功能鍵：執行特殊功能，例如 F1 到 F12 鍵。
-   切換鍵：讓系統進入某種模式，例如 Caps Lock、ScrLk 和 Num Lock 鍵。
-   命令鍵：執行系統工作或命令啟用，例如空格鍵、Enter、Esc、Pause/Break 及 Print Screen 鍵。

除了這些類別，還有第二種按鍵類別和按鍵組合可做為應用程式功能的快速鍵：

-   便捷鍵：同時按 Alt 鍵與字元鍵可以公開控制項或功能表項目，其表示方式：在功能表中將便捷鍵字元指派加底線，或重疊顯示便捷鍵字元。
-   快速鍵：同時按功能鍵或 Ctrl 鍵與字元鍵可以公開應用程式命令。 您的 app 不一定會有與命令相對應的 UI。

應用程式無法攔截其他按鍵組合類別 (稱為 Secure Attention Sequence (SAS))。 這是一項安全性功能，用來在登入時保護使用者的系統，並且包括 Ctrl-Alt-Del 與 Win-L。

以下顯示的記事本 app 有已展開的 [檔案] 功能表，其中同時包含便捷鍵和快速鍵。

![記事本 app 有已展開的 [檔案] 功能表，其中同時包含便捷鍵和快速鍵。](images/input-patterns/notepad.png)

## <span id="Keyboard_commands"></span><span id="keyboard_commands"></span><span id="KEYBOARD_COMMANDS"></span>鍵盤命令


以下是支援鍵盤輸入的各種裝置上提供的完整鍵盤互動清單。 有些裝置與平台需要原生按鍵輸入和互動。

設計自訂控制項和互動時，一貫使用此鍵盤語言，讓您的應用程式令人覺得熟悉、可靠且容易學習。

請勿重新定義預設鍵盤快速鍵。

下列表格列出常用的鍵盤命令。 如需完整的鍵盤命令清單，請參閱 [Windows 鍵盤快速鍵](http://go.microsoft.com/fwlink/p/?linkid=325424)

**瀏覽命令**

| 動作                               | 按鍵命令                                      |
|--------------------------------------|--------------------------------------------------|
| 向後                                 | Alt+向左鍵或特殊鍵盤上的返回按鈕 |
| 向前                              | Alt+向右鍵                                        |
| 向上                                   | Alt+向上鍵                                           |
| 取消或離開目前模式   | Esc                                              |
| 在清單中的項目間移動         | 方向鍵 (向左鍵、向右鍵、向上鍵、向下鍵)                |
| 跳至項目的下一個清單           | Ctrl+向左鍵                                        |
| 語意式縮放                        | Ctrl++ 或 Ctrl+-                                 |
| 跳至集合中某個命名的項目 | 開始輸入項目名稱                           |
| 下一頁                            | Page Up、Page Down 或空格鍵                   |
| 下一個 Tab 點                             | Ctrl+Tab                                         |
| 上一個 Tab 點                         | Ctrl+Shift+Tab                                   |
| 開啟應用程式列                         | Windows + Z                                        |
| 啟動或瀏覽到項目    | Enter 鍵                                            |
| 選取                               | 空格鍵                                         |
| 連續選取                  | Shift+方向鍵                                  |
| 全選                           | Ctrl+A                                           |

 

**一般命令**

| 動作                                                 | 按鍵命令     |
|--------------------------------------------------------|-----------------|
| 釘選項目                                            | Ctrl+Shift+1    |
| 儲存                                                   | Ctrl+S          |
| 尋找                                                   | Ctrl+F          |
| 列印                                                  | Ctrl+P          |
| 複製                                                   | Ctrl+C          |
| 剪下                                                    | Ctrl+X          |
| 新增項目                                               | Ctrl+N          |
| 貼上                                                  | Ctrl+V          |
| 開啟                                                   | Ctrl+O          |
| 開啟位址 (例如 Internet Explorer 中的 URL) | Ctrl+L 或 Alt+D |

 

**媒體瀏覽命令**

| 動作       | 按鍵命令 |
|--------------|-------------|
| 播放/暫停   | Ctrl+P      |
| 下一個項目    | Ctrl+F      |
| 預覽項目 | Ctrl+B      |

 

注意：「播放/暫停」和「下一個項目」的媒體瀏覽按鍵命令分別與「列印」和「尋找」的按鍵命令相同。 常用命令應該優先於媒體瀏覽命令。 例如，如果應用程式同時支援播放媒體和列印，按 Ctrl+P 按鍵命令則會列印。
## <span id="Visual_feedback"></span><span id="visual_feedback"></span><span id="VISUAL_FEEDBACK"></span>視覺化回饋


請只搭配鍵盤互動使用焦點矩形。 如果使用者初始化觸控互動，讓鍵盤 UI 逐漸淡出。 這可以讓 UI 保持整齊、不凌亂。

如果元素不支援互動 (例如靜態文字)，請勿顯示視覺化回饋。 同樣地，這可讓 UI 保持整齊、不凌亂。

如果所有元素均代表相同的輸入目標，請試著同時顯示視覺化回饋。

嘗試提供模擬觸控式操作 (例如移動瀏覽、 旋轉、縮放等等) 的螢幕上按鈕 (例如 + 和 -) 做為提示。

如需視覺化回饋的詳細一般指導方針，請參閱[視覺化回饋的指導方針](guidelines-for-visualfeedback.md)


## <span id="keyboard_events"></span><span id="KEYBOARD_EVENTS"></span>鍵盤事件和焦點


硬體和觸控式鍵盤都有可能發生以下的鍵盤事件。

| 事件                                      | 描述                    |
|--------------------------------------------|--------------------------------|
| [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) | 按下按鍵時發生。  |
| [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)     | 放開按鍵時發生。 |


**重要**  
有些 Windows 執行階段控制項可在內部處理輸入事件。 在這種情況下，因為事件接聽程式不會叫用相關處理常式，所以看起來像是沒有發生輸入事件。 這個按鍵子集通常是由類別處理常式處理，為基本鍵盤協助工具提供內建支援。 例如，[**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 類別會覆寫空格鍵與 Enter 鍵的 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) 事件 (以及 [**OnPointerPressed**](https://msdn.microsoft.com/library/windows/apps/hh967989))，並將這些事件路由傳送到控制項的 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 如果按下按鍵是由控制項類別處理，就不會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。

這樣會提供等同叫用按鈕的內建鍵盤，類似使用手指點選或使用滑鼠按一下。 不是空格鍵或 Enter 鍵的按鍵仍會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 如需以類別為基礎的事件處理如何運作的詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584) (特別是＜控制項中的輸入事件處理常式＞一節)


您 UI 中的控制項只有在取得輸入焦點時才會產生鍵盤事件。 個別控制項會在使用者直接按一下或點選配置上的控制項時取得焦點，或是在內容區域內使用 Tab 鍵進入 Tab 順序時取得焦點。

您也可以呼叫控制項的 [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161) 方法來強制取得焦點。 這種方式在實作快速鍵時很必要，因為 UI 載入時預設並沒有設定鍵盤焦點。 如需詳細資訊，請參閱這個主題中後面的[快速鍵範例](#shortcut_keys_example)。

為了讓控制項接收輸入焦點，必須啟用控制項並顯示它，而且 [**IsTabStop**](https://msdn.microsoft.com/library/windows/apps/br209422) 與 [**HitTestVisible**](https://msdn.microsoft.com/library/windows/apps/br208933) 屬性值必須為 **true**。 這是大多數控制項的預設狀態。 當控制項取得輸入焦點時，它就可以引發和回應鍵盤輸入事件，如這個主題後面所述。 處理 [**GotFocus**](https://msdn.microsoft.com/library/windows/apps/br208927) 和 [**LostFocus**](https://msdn.microsoft.com/library/windows/apps/br208943) 事件也可以回應取得或失去焦點的控制項。

根據預設，控制項的 Tab 順序即為它們在 Extensible Application Markup Language (XAML) 中出現的順序。 不過，使用 [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/br209461) 屬性就可以修改這個順序。 如需詳細資訊，請參閱[實作鍵盤協助工具](https://msdn.microsoft.com/library/windows/apps/hh868161)

## <span id="keyboard_event_handlers"></span><span id="KEYBOARD_EVENT_HANDLERS"></span>鍵盤事件處理常式


輸入事件處理常式實作一個提供下列資訊的委派：

-   事件發送者。 發送者會回報附加了事件處理常式的物件。
-   事件資料。 以鍵盤事件來說，該資料將會是 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) 的執行個體。 處理常式的委派是 [**KeyEventHandler**](https://msdn.microsoft.com/library/windows/apps/br227904)。 對大多數處理常式案例來說，最相關的 **KeyRoutedEventArgs** 屬性是 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074)，也有可能是 [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [
            **OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810)。 由於鍵盤事件是路由事件，因此事件資料會提供 **OriginalSource**。 如果您是刻意讓物件透過物件樹反昇，有時 **OriginalSource** (而不是發送者) 就會成為較為重要的物件。 不過這需要視您的設計而定。 如需如何使用 **OriginalSource** 而非發送者的相關資訊，請參閱這個主題的＜鍵盤路由事件＞一節，或參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)

### <span id="attaching_a_keyboard_event_handler"></span><span id="ATTACHING_A_KEYBOARD_EVENT_HANDLER"></span>附加鍵盤事件處理常式

您可以為任何物件附加鍵盤事件處理函式，只要該事件是該物件的成員即可。 這包含任何 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 衍生類別。 下列 XAML 範例示範如何為 [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) 附加 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件的處理常式

```XAML
<Grid KeyUp="Grid_KeyUp">
  ...
</Grid>
```

您也可以在程式碼中附加事件處理常式。 如需詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)

### <span id="defining_a_keyboard_event_handler"></span><span id="DEFINING_A_KEYBOARD_EVENT_HANDLER"></span>定義鍵盤事件處理常式

下面的範例示範前面範例中附加的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件處理常式的不完整事件處理常式定義。

```CSharp
void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    //handling code here
}
```

```VisualBasic
Private Sub Grid_KeyUp(ByVal sender As Object, ByVal e As KeyRoutedEventArgs)
    &#39;handling code here
End Sub
```

```ManagedCPlusPlus
void MyProject::MainPage::Grid_KeyUp(
  Platform::Object^ sender,
  Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{//handling code here}
```

### <span id="using_keyroutedeventargs"></span><span id="USING_KEYROUTEDEVENTARGS"></span>使用 KeyRoutedEventArgs

所有鍵盤事件都是使用 [**KeyRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943072) 代表事件資料，**KeyRoutedEventArgs** 包含下列屬性：

-   [**按鍵**](https://msdn.microsoft.com/library/windows/apps/hh943074)
-   [**KeyStatus**](https://msdn.microsoft.com/library/windows/apps/hh943075)
-   [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073)
-   [
            **OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) (繼承自 [**RoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br208809)

### <span id="key"></span><span id="KEY"></span>按鍵

如果按下按鍵，會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 事件。 同樣的，如果放開按鍵，會引發 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 通常您接聽事件是為了處理特定的按鍵值。 若要判斷按下或放開的是哪一個按鍵，請檢查事件資料中的 [**Key**](https://msdn.microsoft.com/library/windows/apps/hh943074) 值。 **Key** 會傳回 [**VirtualKey**](https://msdn.microsoft.com/library/windows/apps/br241812) 值。 **VirtualKey** 列舉包括所有受支援的按鍵。

### <span id="modifier_keys"></span><span id="MODIFIER_KEYS"></span>輔助按鍵

輔助按鍵是使用者通常會與其他按鍵一起按的按鍵，如 Ctrl 或 Shift。 您的應用程式可以使用這些按鍵組合當作鍵盤快速鍵來叫用應用程式命令。

在您的 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件處理常式中使用程式碼，即可偵測快速鍵組合。 然後，您可以追蹤想了解的輔助按鍵按下狀態。 當非輔助按鍵發生鍵盤事件時，您可以同時檢查輔助按鍵是否處於按下狀態。

**注意** Alt 鍵以 **VirtualKey.Menu** 值表示。

 

## <span id="shortcut_keys_example"></span><span id="SHORTCUT_KEYS_EXAMPLE"></span>快速鍵範例


以下範例示範如何實作快速鍵。 在這個範例中，使用者可以使用「播放」、「暫停」以及「停止」按鈕或 Ctrl+P、Ctrl+A 以及 Ctrl+S 鍵盤快速鍵來控制媒體的播放。 按鈕 XAML 使用工具提示以及按鈕標籤中的 [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081) 屬性顯示捷徑。 這個自我說明文件對於提高 app 的可用性及協助性是很重要的。 如需詳細資訊，請參閱[鍵盤協助工具](https://msdn.microsoft.com/library/windows/apps/mt244347)

另請注意，頁面載入時會將輸入焦點設給本身。 如果沒有這個步驟，就沒有控制項會取得初始輸入焦點，而且除非使用者手動設定輸入焦點 (例如，按 Tab 鍵或按一下控制項)，否則 app 將無法引發輸入事件。

```XAML
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv" 
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A" 
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S" 
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>

  </StackPanel>

</Grid>
```

```ManagedCPlusPlus
//showing implementations but not header definitions
void MainPage::OnNavigatedTo(NavigationEventArgs^ e)
{
    (void) e;    // Unused parameter
    this->Loaded+=ref new RoutedEventHandler(this,&amp;MainPage::ProgrammaticFocus);
}
void MainPage::ProgrammaticFocus(Object^ sender, RoutedEventArgs^ e) {
    this->Focus(Windows::UI::Xaml::FocusState::Programmatic);
}

void KeyboardSupport::MainPage::MediaButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    FrameworkElement^ fe = safe_cast<FrameworkElement^>(sender);
    if (fe->Name == "PlayButton") {DemoMovie->Play();}
    if (fe->Name == "PauseButton") {DemoMovie->Pause();}
    if (fe->Name == "StopButton") {DemoMovie->Stop();}
}


void KeyboardSupport::MainPage::Grid_KeyDown(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
}


void KeyboardSupport::MainPage::Grid_KeyUp(Platform::Object^ sender, Windows::UI::Xaml::Input::KeyRoutedEventArgs^ e)
{
    if (e->Key == VirtualKey::Control) isCtrlKeyPressed = true;
    else if (isCtrlKeyPressed) {
        if (e->Key==VirtualKey::P) {
            DemoMovie->Play();
        }
        if (e->Key==VirtualKey::A) {DemoMovie->Pause();}
        if (e->Key==VirtualKey::S) {DemoMovie->Stop();}
    }
}
```

```CSharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Set the input focus to ensure that keyboard events are raised.
    this.Loaded += delegate { this.Focus(FocusState.Programmatic); };
}

private void Grid_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == VirtualKey.Control) isCtrlKeyPressed = false;
}

private void Grid_KeyDown(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == VirtualKey.Control) isCtrlKeyPressed = true;
    else if (isCtrlKeyPressed)
    {
        switch (e.Key)
        {
            case VirtualKey.P: DemoMovie.Play(); break;
            case VirtualKey.A: DemoMovie.Pause(); break;
            case VirtualKey.S: DemoMovie.Stop(); break;
        }
    }
}

private void MediaButton_Click(object sender, RoutedEventArgs e)
{
    switch ((sender as Button).Name)
    {
        case "PlayButton": DemoMovie.Play(); break;
        case "PauseButton": DemoMovie.Pause(); break;
        case "StopButton": DemoMovie.Stop(); break;
    }
}
```

```VisualBasic
Private isCtrlKeyPressed As Boolean
Protected Overrides Sub OnNavigatedTo(e As Navigation.NavigationEventArgs)

End Sub

Private Sub Grid_KeyUp(sender As Object, e As KeyRoutedEventArgs)
    If e.Key = Windows.System.VirtualKey.Control Then
        isCtrlKeyPressed = False
    End If
End Sub

Private Sub Grid_KeyDown(sender As Object, e As KeyRoutedEventArgs)
    If e.Key = Windows.System.VirtualKey.Control Then isCtrlKeyPressed = True
    If isCtrlKeyPressed Then
        Select Case e.Key
            Case Windows.System.VirtualKey.P
                DemoMovie.Play()
            Case Windows.System.VirtualKey.A
                DemoMovie.Pause()
            Case Windows.System.VirtualKey.S
                DemoMovie.Stop()
        End Select
    End If
End Sub

Private Sub MediaButton_Click(sender As Object, e As RoutedEventArgs)
    Dim fe As FrameworkElement = CType(sender, FrameworkElement)
    Select Case fe.Name
        Case "PlayButton"
            DemoMovie.Play()
        Case "PauseButton"
            DemoMovie.Pause()
        Case "StopButton"
            DemoMovie.Stop()
    End Select
End Sub
```

**注意** 在 XAML 中設定 [**AutomationProperties.AcceleratorKey**](https://msdn.microsoft.com/library/windows/apps/hh759762) 或 [**AutomationProperties.AccessKey**](https://msdn.microsoft.com/library/windows/apps/hh759763) 可提供字串資訊，其中記載用於叫用該特定動作的捷徑。 Microsoft UI 自動化用戶端 (例如朗讀程式) 會擷取此資訊，通常直接提供給使用者。 設定 **AutomationProperties.AcceleratorKey** 或 **AutomationProperties.AccessKey** 本身不會有任何動作。 您還是需要附加 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 或 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件的處理常式，才能實際在 app 中實作鍵盤快速鍵行為。 另外，也不會自動提供便捷鍵的底線文字裝飾。 如果您希望在 UI 中顯示有底線的文字，必須以內嵌 [**Underline**](https://msdn.microsoft.com/library/windows/apps/br209982) 格式明確的為助憶鍵中的特定鍵加上文字底線。

 

## <span id="keyboard_routed_events"></span><span id="KEYBOARD_ROUTED_EVENTS"></span>鍵盤路由事件


有一些特定事件是路由事件，包括 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)。 路由事件使用事件反昇路由策略。 反昇路由策略表示事件源自於子物件，並接著反昇路由到物件樹中相繼的父物件。 這是處理相同事件並與相同事件資料互動的機會。

以下列 XAML 範例為例，它會處理一個 [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) 和兩個 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 物件的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 在這個情況下，如果在任一 **Button** 物件具有焦點時放開按鍵，則會引發 **KeyUp** 事件。 接著，事件會反昇到父 **Canvas**

```XAML
<StackPanel KeyUp="StackPanel_KeyUp">
  <Button Name="ButtonA" Content="Button A"/>
  <Button Name="ButtonB" Content="Button B"/>
  <TextBlock Name="statusTextBlock"/>
</StackPanel>
```

下面的範例示範如何實作前一個範例中對應的 XAML 內容的 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件處理常式。

```CSharp
void StackPanel_KeyUp(object sender, KeyRoutedEventArgs e)
{
    statusTextBlock.Text = String.Format(
        "The key {0} was pressed while focus was on {1}",
        e.Key.ToString(), (e.OriginalSource as FrameworkElement).Name);
}
```

請注意，前面處理常式中有用到 [**OriginalSource**](https://msdn.microsoft.com/library/windows/apps/br208810) 屬性。 這裡的 **OriginalSource** 會回報引發事件的物件。 物件不可以是 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635)，因為 **StackPanel** 不是控制項而且不可以取得焦點。 在 **StackPanel** 內的兩個按鈕中，只有一個可能引發事件，但是是哪一個呢？ 如果您是在處理父物件上的事件，可以使用 **OriginalSource** 來辨別實際的事件來源物件。

### <span id="handled_property"></span><span id="HANDLED_PROPERTY"></span>事件資料中的 Handled 屬性

視您的事件處理策略而定，您可能會只想使用一個事件處理常式來應對反昇事件。 例如，如果您有附加到其中一個 [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 控制項的特定 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 處理常式，該處理常式會先使用它來處理該事件。 在這個情況下，您可能不想讓父面板也同時處理事件。 這時，您可以在事件資料中使用 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 屬性。

使用路由事件資料類別中的 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 屬性是為了報告先前在事件路由上登錄的另一個處理常式已經執行。 這會影響路由事件系統的行為。 將事件處理常式中的 **Handled** 設成 **true** 時，該事件會停止路由而不會傳送給相繼的父元素。

### <span id="addhandler_and_already_handled_keyboard_events"></span><span id="ADDHANDLER_AND_ALREADY_HANDLED_KEYBOARD_EVENTS"></span>AddHandler 和已處理的鍵盤事件

您可以使用一項特別的技術，以附加可以在已標示為處理過的事件上作用的處理常式。 這項技術使用 [**AddHandler**](https://msdn.microsoft.com/library/windows/apps/hh702399) 方法來登錄處理常式，而不是使用 XAML 屬性或語言特定的語法新增處理常式 (例如 C# 中的 +=)。 大致上，這項技術的限制在於 **AddHandler** API 是採用一個 [**RoutedEvent**](https://msdn.microsoft.com/library/windows/apps/br208808) 類型的參數來識別相關的路由事件。 並非所有路由事件都提供 **RoutedEvent** 識別項，因此這項考量也就影響到哪些路由事件仍然可以在 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 案例中處理。 [
            **KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件在 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 上已有路由事件識別項 ([**KeyDownEvent**](https://msdn.microsoft.com/library/windows/apps/hh702416) 與 [**KeyUpEvent**](https://msdn.microsoft.com/library/windows/apps/hh702418))。 不過，其他事件 (例如 [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706)) 並沒有路由事件識別項，因此也就不能與 **AddHandler** 技術搭配使用。

## <span id="commanding"></span><span id="COMMANDING"></span>命令


少數 UI 元素提供命令功能的內建支援。 命令功能在相關實作中使用輸入相關路由事件。 它會叫用單一命令處理常式來處理相關的 UI 輸入，例如特定指標動作或特定快速鍵。

如果某個 UI 元素擁有命令功能，請考慮使用它的命令 API，而非任何特定輸入事件。 如需詳細資訊，請參閱 [**ButtonBase.Command**](https://msdn.microsoft.com/library/windows/apps/br227740)

您也可實作 [**ICommand**](https://msdn.microsoft.com/library/windows/apps/br227885) 來封裝從一般事件處理常式叫用的命令功能。 透過這種方式，即使沒有可用的 **Command** 屬性，您還是可以使用命令功能。

## <span id="text_input_and_controls"></span><span id="TEXT_INPUT_AND_CONTROLS"></span>文字輸入和控制項


特定控制項會以它們自己的處理方式應對鍵盤事件。 例如，[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 這個控制項的設計是擷取使用鍵盤輸入的文字，然後以視覺化方式呈現文字。 它在自己的邏輯中使用 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 與 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 來擷取按鍵輸入動作，然後如果文字真的變更了，就會引發它自己的 [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/br209706) 事件。

一般來說，您仍然可以將 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 和 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 的處理常式加到 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)，或加到任何要用來處理文字輸入的相關控制項。 不過，基於控制項本身的設計目的，控制項可能不會對透過按鍵事件導向它的所有按鍵值都提供回應。 每個控制項都有它的特定行為。

例如，[**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736) ([**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) 的基礎類別) 會處理 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942)，這樣它就可以檢查空格鍵或 Enter 鍵。 **ButtonBase** 將 **KeyUp** 視為等同滑鼠左鍵，可以引發 [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) 事件。 事件的處理會在 **ButtonBase** 覆寫虛擬方法 [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983) 時完成。 在實作中，它會將 [**Handled**](https://msdn.microsoft.com/library/windows/apps/hh943073) 設定成 **true**。 以空格鍵為例，結果是接聽按鍵事件的任何父按鈕，都不會收到自己的處理常式的已處理事件。

另一個範例是 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683)。 **TextBox** 並不將某些按鍵 (例如方向鍵) 視為文字，而是視為控制項 UI 特定的行為。 **TextBox** 會將這些事件案例標示為已處理。

自訂控制項可以透過覆寫 [**OnKeyDown**](https://msdn.microsoft.com/library/windows/apps/hh967982) / [**OnKeyUp**](https://msdn.microsoft.com/library/windows/apps/hh967983)，以實作它們自己類似的按鍵事件覆寫行為。 如果您的自訂控制項會處理特定的快速鍵，或具有類似於針對 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) 所述之情況的控制項或焦點行為，就應該將這個邏輯放入您自己的 **OnKeyDown** / **OnKeyUp** 覆寫中。

## <span id="the_touch_keyboard"></span><span id="THE_TOUCH_KEYBOARD"></span>觸控式鍵盤


文字輸入控制項會自動支援觸控式鍵盤。 當使用者使用觸控輸入將輸入焦點設為文字控制項時，觸控式鍵盤會自動出現。 當輸入焦點不在文字控制項時，觸控式鍵盤就會隱藏起來。

當觸控式鍵盤出現時，它會自動定位您的 UI，以確保取得焦點的元素保持永遠可見。 這可能會造成 UI 中其他重要的區域跑到螢幕外。 不過，您可以停用預設行為，並在觸控式鍵盤出現時調整 UI。 如需詳細資訊，請參閱[回應螢幕小鍵盤外觀的範例](http://go.microsoft.com/fwlink/p/?linkid=231633)

如果建立需要文字輸入但不是從標準輸入控制項衍生的自訂控制項，您可以實作正確的 UI 自動化控制項模式來加入對觸控式鍵盤的支援。 如需詳細資訊，請參閱[回應觸控式鍵盤的出現](respond-to-the-presence-of-the-touch-keyboard.md)和[觸控式鍵盤範例](http://go.microsoft.com/fwlink/p/?linkid=246019)

按下觸控式鍵盤上的按鍵，就像按下硬體鍵盤上的按鍵一樣，都會引發 [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208941) 與 [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208942) 事件。 不過，觸控式鍵盤將不會引發 Ctrl+A、Ctrl+Z、Ctrl+X、Ctrl+C 以及 Ctrl+V 的輸入事件，因為它們是保留給輸入控制項的文字操作。

您可以設定文字控制項的輸入範圍，使其符合您預期使用者輸入的資料類型，讓使用者在您的 app 中輸入資料時更加快速方便。 輸入範圍會提供控制項所預期之文字輸入類型的提示，讓系統可以為該輸入類型提供專用的觸控式鍵盤配置。 例如，如果文字方塊只用來輸入 4 位數 PIN，請將 [**InputScope**](https://msdn.microsoft.com/library/windows/apps/hh702632) 屬性設定為 [**Number**](https://msdn.microsoft.com/library/windows/apps/hh702028)。 這會告訴系統顯示數字鍵台配置，方便使用者輸入 PIN。 如需詳細資訊，請參閱[使用輸入範圍來變更觸控式鍵盤](https://msdn.microsoft.com/library/windows/apps/mt280229)


## 本節中的其他文章
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主題</th>
<th align="left">說明</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[回應觸控式鍵盤的出現](respond-to-the-presence-of-the-touch-keyboard.md)</p></td>
<td align="left"><p>了解如何量身打造顯示或隱藏觸控式鍵盤時 app 的 UI。</p></td>
</tr>
</tbody>
</table>

 


## <span id="related_topics"></span>相關文章


**開發人員**
* [識別輸入裝置](identify-input-devices.md)
* [回應觸控式鍵盤的出現](respond-to-the-presence-of-the-touch-keyboard.md)

**設計人員**
* [鍵盤設計指導方針](https://msdn.microsoft.com/library/windows/apps/hh972345)

**範例**
* [基本輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延遲輸入範例](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [焦點視覺效果範例](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**封存範例**
* [輸入範例](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [輸入：裝置功能範例](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [輸入：觸控式鍵盤範例](http://go.microsoft.com/fwlink/p/?linkid=246019)
* [回應螢幕小鍵盤外觀的範例](http://go.microsoft.com/fwlink/p/?linkid=231633)
* [XAML 文字編輯範例](http://go.microsoft.com/fwlink/p/?LinkID=251417)
 

 






<!--HONumber=May16_HO2-->


