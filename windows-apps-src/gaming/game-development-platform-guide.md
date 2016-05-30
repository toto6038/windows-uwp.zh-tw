---
author: mtoepke
title: 適用於通用 Windows 平台 (UWP) app 的遊戲技術
description: 在本指南中，您將深入了解可用於開發通用 Windows 平台 (UWP) 遊戲的技術。
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
---

# 適用於通用 Windows 平台 (UWP) app 的遊戲技術


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

在本指南中，您將深入了解可用於開發通用 Windows 平台 (UWP) 遊戲的技術。

##  Windows 10 的遊戲開發優勢


在 Windows 10 中引入 UWP，您的 Windows 10 標題將能夠橫跨所有的 Microsoft 平台。 經由從舊版 Windows 免費移轉，Windows 10 用戶端數目將會穩定增加。 組合上述兩項，表示您的 Windows 10 標題將可透過 Windows 市集吸引大量的客戶。

此外，Windows 10 提供了許多對遊戲特別實用的新功能：

-   減少的記憶體分頁和減少的整體記憶體系統大小
-   改進的圖形記憶體管理會主動為前景遊戲配置更多的記憶體並提供保護

## 使用 C++ 和 DirectX 的 UWP 遊戲


需要高效能的即時遊戲應使用 DirectX API。 DirectX 是原生 API 集合，可供建立需要高效能的遊戲和多媒體 app (例如 3D 遊戲)。 因為 DirectX API 是原生的，所以 C++ 是唯一可搭配 DirectX 使用的支援語言。

## 開發環境


若要建立 UWP 遊戲，您需要安裝一份 Visual Studio 2015 以便設定您的開發環境。 Visual Studio 2015 可讓您建立 UWP app 並提供遊戲開發的工具：

-   可供 DX 遊戲程式設計的 Visual Studio 工具 - Visual Studio 提供的工具可用來建立、編輯、預覽及匯出影像、模型及著色器資源。 同時還有工具可讓您在建置期間用來轉換資源和偵錯 DirectX 圖形程式碼。 如需詳細資訊，請參閱[使用 Visual Studio 工具進行遊戲程式設計](set-up-visual-studio-for-game-development.md)。
-   Visual Studio 圖形診斷功能 - 圖形診斷工具目前已在 Windows 內做為選用功能提供。 診斷工具讓您 進行圖形偵錯、圖形框架分析以及即時監視 GPU 使用量。 如需詳細資訊，請參閱[使用 DirectX 執行階段與 Visual Studio 圖形診斷功能](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md)。

如需詳細資訊，請參閱＜準備通用 Windows 平台和 DirectX 遊戲程式設計環境＞。

## 開始使用 DirectX 遊戲專案範本


設定開發環境之後，您可以使用其中一個 DirectX 相關專案範本來建立 UWP DirectX 遊戲。 Visual Studio 2015 有三個範本可用於建立新的 UWP DirectX 專案：**DirectX 11 App (通用 Windows)**、 **DirectX 12 App (通用 Windows)** 以及 **DirectX 11 和 XAML App (通用 Windows)**。 如需詳細資訊，請參閱[從範本建立通用 Windows 平台和 DirectX 遊戲專案](user-interface.md)。

## Windows 10 API


Windows 10 提供適合用於遊戲開發的廣泛 API 集合。 裡面幾乎包含所有遊戲可以使用的 API：2D 和 3D 圖形、音訊、輸入、文字資源、使用者介面和網路功能。

有許多 API 與遊戲開發相關，但並非所有遊戲都需要使用所有的 API。 例如，某些遊戲只會使用 3D 圖形和只使用 Direct3D、 某些遊戲只使用 2D 圖形和只使用 Direct2D，而其他遊戲則可能會使用這兩種。 下列圖表顯示依功能類型分類的遊戲開發相關 API。

![遊戲平台技術](images/gameplatformtechnologies.png)

-   3D 圖形 - Windows 10 支援兩個 3D 圖形 API 集合：Direct3D 11 和 [Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899121)。 這兩種 API 均提供建立 3D 和 2D 圖形的功能。 Direct3D 11 與 Direct3D 12 不會一起使用，但任一種都可以搭配任何 2D 圖形與 UI 群組中的 API 使用。 如需在您的遊戲中使用圖形 API 的詳細資訊，請參閱 [DirectX 遊戲的基本 3D 圖形](an-introduction-to-3d-graphics-with-directx.md)。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12 引進了下一版的 Direct3D，也就是 DirectX 核心的 3D 圖形 API。 相較於舊版 Direct3D，此 Direct3D 版本的設計更快且更有效率。 Direct3D 12 提高速度的缺點是它是較低層級，而且需要您自行管理圖形資源， 還要有更廣泛的圖形程式設計經驗才能實現提高的速度。</p>
    <p><strong>使用時機</strong></p>
    <p>當您需要將遊戲效能最大化且您的遊戲佔用龐大 CPU 資源時，請使用 Direct3D 12。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [Direct3d 12](https://msdn.microsoft.com/library/windows/desktop/dn899121) 文件。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D 11 是舊版的 Direct3D，可讓您 使用高於 D3D 12 的硬體抽象層級建立 3D 圖形。</p>
    <p><strong>使用時機</strong></p>
    <p>如果您有現有的 Direct3D 11 程式碼、您的遊戲沒有佔用龐大 CPU 資源，或您想要擁有為您管理資源的優勢，請使用 Direct3D 11。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080) 文件。</p></td>
    </tr>
    </tbody>
    </table>

     

-   2D 圖形與 UI - 與 2D 圖形相關的 API，例如文字和使用者介面。 所有 2D 圖形與 UI API 都是選用的。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D 是一種硬體加速的即時模式 2D 圖形 API，能夠以高效能和高品質來呈現 2D 幾何、點陣圖和文字。 Direct2D API 是以 Direct3D 為建置基礎，設計用來與 GDI、GDI+ 和 Direct3D 交互操作。</p>
    <p><strong>使用時機</strong></p>
    <p>Direct2D (而非 Direct3D) 可用於提供純 2D 遊戲的圖形，例如側捲動或棋盤遊戲，或可搭配 Direct3D 使用來簡化在 3D 遊戲中建立 2D 圖形，例如使用者介面或抬頭顯示器。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) 文件。</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite 提供其他處理文字的功能，並可搭配 Direct3D 與 Direct2D 使用， 以提供使用者介面或其他需要文字之區域的文字輸出。 DirectWrite 支援多格式文字的測量、繪圖和點擊測試。 DirectWrite 可使用全域和當地語系化應用程式的所有支援語言來處理文字。 DirectWrite 也提供低階字符轉譯 API，適用於想要執行自己的版面配置和 Unicode 對字符處理的開發人員。</p>
    <p><strong>使用時機</strong></p>
    <p></p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 文件。</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition 是一個 Windows 元件，能夠利用轉換、效果和動畫來產生高效能的 點陣圖組合。 應用程式開發人員可以使用 DirectComposition API 來建立視覺上吸引人的使用者介面，以提供豐富和流暢的動畫轉換 (從一種視覺效果轉換成另一種視覺效果)。</p>
    <p><strong>使用時機</strong></p>
    <p>DirectComposition 設計用來簡化撰寫視覺效果及建立動畫轉換的程序。 如果您的遊戲需要複雜的使用者介面，您可以使用 DirectComposition 來簡化 UI 的建立和管理。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [DirectComposition](https://msdn.microsoft.com/library/windows/desktop/hh437371) 文件。</p></td>
    </tr>
    </tbody>
    </table>

     

-   音訊 - 與播放音訊和套用音訊效果相關的 API。 如需在您的遊戲中使用音訊 API 的詳細資訊，請參閱[遊戲的音訊](working-with-audio-in-your-directx-game.md)。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 是低階的音訊 API，可提供訊號處理和混音基礎。 XAudio 的設計對遊戲音訊引擎非常敏感， 而且還能夠建立自訂音訊效果以及複雜的音訊效果和篩選器鏈結。</p>
    <p><strong>使用時機</strong></p>
    <p>當您的遊戲需要以最低的負荷和延遲來播放音效時，請使用 XAudio2。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) 文件。</p></td>
    </tr>
    <tr class="even">
    <td align="left">媒體基礎</td>
    <td align="left"><p>Microsoft 媒體基礎是設計來播放音訊和視訊的媒體檔案和串流，但需要高於 XAudio2 的功能層級並可接受一些額外的負荷時，也可以在遊戲中使用。</p>
    <p><strong>使用時機</strong></p>
    <p>媒體基礎對於遊戲中的電影場景或非互動式元件特別實用。 媒體基礎也很適合用於解碼音訊檔案以便使用 XAudio2 播放。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [Microsoft 媒體基礎概觀](https://msdn.microsoft.com/library/windows/desktop/ms694197)。</p></td>
    </tr>
    </tbody>
    </table>

     

-   輸入 - 有關從鍵盤、滑鼠、遊戲板及其他使用者輸入來源輸入的 API。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>XInput 遊戲控制器 API 可讓應用程式接收遊戲控制器的輸入。</p>
    <p><strong>使用時機</strong></p>
    <p>如果您的遊戲需要支援 gampad 輸入，而且您有現有的 XInput 程式碼，即可繼續使用 XInput。 UWP 的 Windows.Gaming.Input 已取代 XInput，而如果您正在撰寫新的輸入程式碼，則應該使用 Windows.Gaming.Input 而不是 XInput。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [XInput](https://msdn.microsoft.com/library/windows/desktop/hh405053) 文件。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>Windows.Gaming.Input API 取代 XInput 並以下列 Xinput 優勢提供相同的功能：</p>
    <ul>
    <li>較低的資源使用量</li>
    <li>可供擷取輸入的較低 API 呼叫延遲</li>
    <li>能夠一次處理 4 個以上的遊戲板</li>
    <li>能夠存取其他 Xbox One 遊戲板功能，例如觸發程序震動馬達</li>
    <li>能夠在控制器透過事件 (而非輪詢) 連線/中斷連線時收到通知</li>
    <li>能夠將輸入歸屬於特定使用者 (Windows.System.User)</li>
    </ul>
    <p><strong>使用時機</strong></p>
    <p>如果您的遊戲需要支援遊戲板輸入但不是使用現有的 XInput 程式碼或您需要以上所列的其中一個優勢，則應使用 Windows.Gaming.Input。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [<strong>Windows.Gaming.Input</strong>](https://msdn.microsoft.com/library/windows/apps/dn707817) 文件。</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>Windows.UI.Core.CoreWindow 類別提供的事件可用於追蹤按下指標和移動、按下和放開按鍵事件。</p>
    <p><strong>使用時機</strong></p>
    <p>當您需要追蹤您遊戲中的按下滑鼠或按鍵時，請使用 Windows.UI.Core.CoreWindows 事件。</p>
    <p><strong>其他資訊</strong></p>
    <p>如需在您的遊戲中使用滑鼠或鍵盤的詳細資訊，請參閱[適用於遊戲的移動視角控制項](tutorial--adding-move-look-controls-to-your-directx-game.md)。</p></td>
    </tr>
    </tbody>
    </table>

     

-   數學 - 與簡化常用數學運算相關的 API。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>DirectXMath API 針對遊戲常用的一般線性代數和圖形數學運算，提供適用於 SIMD 架構的 C++ 類型和函式。</p>
    <p><strong>使用時機</strong></p>
    <p>使用 DirectXMath 是選擇性的，可簡化一般數學運算。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) 文件。</p></td>
    </tr>
    </tbody>
    </table>

     

-   網路功能 - 有關透過網際網路或私人網路與其他電腦或裝置通訊的 API。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>Windows.Networking.Sockets 命名空間提供的 TCP 與 UDP 通訊端可允許可靠或不可靠的網路通訊。</p>
    <p><strong>使用時機</strong></p>
    <p>如果您的遊戲需要透過網路與其他電腦或裝置通訊，請使用 Windows.Networking.Sockets。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱[在您的遊戲中使用網路功能](work-with-networking-in-your-directx-game.md)。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>Windows.Web.HTTP 命名空間提供對 HTTP 伺服器的可靠連線，可用來存取網站。</p>
    <p><strong>使用時機</strong></p>
    <p>當您的遊戲需要存取網站才能抓取或儲存資訊時，請使用 Windows.Web.HTTP。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱[在您的遊戲中使用網路功能](work-with-networking-in-your-directx-game.md)。</p></td>
    </tr>
    </tbody>
    </table>

     

-   支援公用程式 - 見置於 Windows 10 API 上的程式庫。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">程式庫</th>
    <th align="left">說明</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectX 工具組</td>
    <td align="left"><p>DirectX 工具組 (DirectXTK) 是協助程式類別集合，可供以 C++ 撰寫 DirectX 11.x 程式碼。</p>
    <p><strong>使用時機</strong></p>
    <p>如果您是尋找舊版 D3DX 公用程式程式碼的新式替代程式碼的 C++ 開發人員，或您是轉換到原生 C++ 的 XNA Game Studio 開發人員，請使用 DirectX 工具組。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 DirectX 工具組專案頁面 ([https://github.com/Microsoft/DirectXTK](https://github.com/Microsoft/DirectXTK))。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D 是方便使用的 Windows 執行階段 API，適用於直接模式 2D 圖形轉譯。</p>
    <p><strong>使用時機</strong></p>
    <p>如果您是 C++ 開發人員並想要適用於 Direct2D 和 DirectWrite 且更易於使用的 WinRT 包裝函式，或您是想要使用 Direct2D 和 DirectWrite 的 C# 開發人員，請使用 Win2D。</p>
    <p><strong>其他資訊</strong></p>
    <p>請參閱 Win2D 專案頁面 ([https://github.com/Microsoft/Win2D](https://github.com/Microsoft/Win2D))。</p></td>
    </tr>
    </tbody>
    </table>

     

## Xbox Live 服務


Xbox Live 功能集 - 使用 Xbox 跨平台遊戲、成就、玩家分數等 – 即將隨著 Windows 10 上市。 您很快就能夠使用 ID@Xbox 在您的 UWP 遊戲中包含動態服務！ 在未來，我們也將協助您在 Xbox One 上提供您的通用 app 平台遊戲。 如需詳細資訊，請參閱 [ID@Xbox](http://www.xbox.com/developers/id) 頁面。

##  使用 DirectX 與 UWP 撰寫遊戲的替代方法


### 不使用 DirectX 的 UWP 遊戲

不需 DirectX 即可撰寫最低效能需求的較簡單遊戲 (例如撲克牌遊戲或棋盤遊戲)，而不需使用 C++ 撰寫。 這幾種遊戲可使用 UWP 支援的任何語言，例如 C#、Visual Basic、C++ 和 HTML/JavaScript 。 如果您的遊戲不需要效能與大量的圖形，請參閱 [JavaScript 與 HTML5 觸控遊戲範例](http://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)做為參考。

### 遊戲引擎

除了使用 Windows 遊戲開發 API 撰寫自己的遊戲引擎以外，還有許多以 Windows 遊戲開發 API 為基礎的高品質遊戲引擎可供在 Windows 平台上 開發遊戲。 考慮遊戲引擎或程式庫時，您有多個選項：

-   完整的遊戲引擎 - 完整的遊戲引擎囊括從頭撰寫遊戲引擎會使用的大部分或全部 Windows 10 API，例如圖形、音訊、輸入和網路功能。 完整的遊戲引擎也會提供遊戲邏輯功能，例如人工智慧和pathfinding。
-   圖形引擎 - 圖形引擎可封裝 Windows 10 圖形 API、管理圖形資源，以及支援各種不同的模型和全球格式。
-   音訊引擎 - 音訊引擎可封裝 Windows 10 音訊 API、管理音訊資源，以及提供進階音訊處理和效果。
-   網路引擎 - 網路引擎可封裝 Windows 10 網路功能 API，以便對您的遊戲新增對等或伺服器型多人遊戲支援，而且包含進階網路功能以支援大量的玩家。
-   人工智慧和路徑搜尋引擎 - AI 和路徑搜尋引擎提供的架構可用於控制您遊戲中的代理程式行為。
-   特殊用途引擎 - 另外還有各種不同的引擎，幾乎可讓您處理可能遇到的任何遊戲開發相關工作，例如建立清查系統與對話方塊樹狀結構。

## 將遊戲提交至市集


一旦準備好發佈您的遊戲，您就需要建立開發人員帳戶並將遊戲提交至 Windows 市集。

如需將遊戲提交至 Windows 市集的相關資訊，請參閱 <https://dev.windows.com/publish>。

 

 






<!--HONumber=May16_HO2-->


