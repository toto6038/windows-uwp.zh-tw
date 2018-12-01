---
ms.assetid: 88e16ec8-deff-4a60-bda6-97c5dabc30b8
description: 本主題提供的案例研究的對等運作的測驗遊戲 WinRT 8.1 範例 app 移植到 windows 10 通用 Windows 平台 (UWP) 應用程式。
title: Windows Runtime 8.x 至 UWP 的案例研究，QuizGame 對等應用程式範例
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9ce39e87f3c5c9e11f3e9ddb1424d606356ee3c8
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8336502"
---
# <a name="windows-runtime-8x-to-uwp-case-study-quizgame-sample-app"></a>Windows 執行階段 8.x 至 UWP 案例研究： QuizGame 應用程式範例




本主題提供的案例研究的對等運作的測驗遊戲 WinRT 8.1 範例 app 移植到 Windows10Universal Windows 平台 (UWP) 應用程式。

通用 8.1 應用程式是一個組建相同的應用程式的兩個版本： Windows8.1，一個應用程式套件和適用於 Windows Phone 8.1 的不同的應用程式套件。 WinRT 8.1 版的 QuizGame 會使用通用 Windows 應用程式專案排列方式，但會採用不同的處理方式，並針對這兩個平台建置功能完全相同的應用程式。 Windows8.1 應用程式套件做為主機測驗遊戲工作階段，而 Windows Phone 8.1 應用程式套件則扮演主機的用戶端的角色。 這兩個測驗遊戲工作階段的部分會透過對等網路進行通訊。

分別為電腦和手機量身訂做這兩個部分，非常合理。 但是，如果您不只能在所選擇的任何裝置上執行，而且還能在主機和用戶端上執行，就更理想了吧？ 在此案例研究中，我們將這兩個應用程式移植到其中他們將會每個建置為單一應用程式套件，讓使用者可以安裝到各種裝置上的 windows 10。

應用程式會利用使用檢視與檢視模型的模式。 誠如所見，由於這個清楚的區隔，使得適用於此 app 的移植程序變得非常簡單。

**注意：** 此範例假設您的網路已設定為傳送和接收自訂 UDP 群組多點傳送封包 （大部分的家用網路都，雖然可能不到您的公司網路）。 此範例也會傳送和接收 TCP 封包。

 

**注意：** 時在 Visual Studio 中，開啟 QuizGame10，如果您看見 「 需要 Visual Studio 更新 」，則請依照[TargetPlatformVersion](w8x-to-uwp-troubleshooting.md)中的步驟。

 

## <a name="downloads"></a>下載

[下載 QuizGame 通用 8.1 應用程式](http://go.microsoft.com/fwlink/?linkid=532953)。 這是應用程式移植之前的初始狀態。 

[下載 QuizGame10 windows 10 應用程式](http://go.microsoft.com/fwlink/?linkid=532954)。 這是剛移植後的應用程式的狀態。 

[在 GitHub 上檢視此範例的最新版本](https://github.com/Microsoft/Windows-appsample-quizgame)。

## <a name="the-winrt-81-solution"></a>WinRT 8.1 方案


以下是我們即將移植的 app，QuizGame的外觀

![在 Windows 上執行的 QuizGame 主機應用程式](images/w8x-to-uwp-case-studies/c04-01-win81-how-the-host-app-looks.png)

在 Windows 上執行的 QuizGame 主機應用程式

 

![在 Windows Phone 上執行的 QuizGame 用戶端應用程式](images/w8x-to-uwp-case-studies/c04-02-wp81-how-the-client-app-looks.png)

在 Windows Phone 上執行的 QuizGame 用戶端應用程式

## <a name="a-walkthrough-of-quizgame-in-use"></a>逐步解說使用中的 QuizGame

這是針對使用中的應用程式短暫假設的帳戶，但是當您想要透過無線網路自行測試應用程式時，它能提供非常有用的資訊。

酒吧裡正在舉行有趣的測驗遊戲。 酒吧裡有一台每個人都能看見的大型電視。 測驗遊戲主持人會有一部電腦將其輸出顯示於電視上。 該電腦上正在執行「主機應用程式」。 任何想要參加測驗的人，只需在其手機或 Surface 上安裝「用戶端應用程式」即可。

主機應用程式處於大廳模式，並且正在大型電視上宣告它已經準備好可供用戶端連線。 Joan 在她的行動裝置上啟動用戶端應用程式。 她在 [玩家名稱] 文字方塊中輸入她的名稱，然後點選 [加入遊戲]。 主機應用程式會透過顯示 Joan 的名稱來確認她已加入，而 Joan 的用戶端應用程式會指出其正在等待遊戲開始。 接下來，Maxwell 會在其行動裝置上執行這些相同步驟。

測驗遊戲主持人按一下 [開始遊戲]，主機應用程式便會顯示問題和可能的答案 (它也會使用一般字體粗細但色彩呈現灰色的方式來顯示已加入的玩家清單)。 在此同時，答案會顯示在已加入用戶端裝置的按鈕上。 Joan 點選答案 "1975" 的按鈕，接著她的所有按鈕都會變成停用狀態。 在主機應用程式中，Joan 的名稱會塗上綠色 (並變成粗體)，以做為收到其答案的通知。 Maxwell 也回答了問題。 當測驗遊戲主持人注意到所有玩家名稱都變成綠色之後，會按 [下一個問題]。

問題將以這個相同循環繼續進行提問與解答。 當主機應用程式上顯示最後一個問題時，按鈕的內容會是 [顯示結果]，而非 [下一個問題]。 按一下 [顯示結果] 時，即會顯示結果。 按一下 [返回大廳]，以返回遊戲週期的開頭，並產生已加入的玩家仍維持加入狀態的例外狀況。 但是，返回大廳讓新玩家有機會加入，並方便已加入的玩家有適當時機可以離開 (儘管已加入的玩家隨時都可點選 [離開遊戲] 來離開)。

## <a name="local-test-mode"></a>本機測試模式

若要在單一電腦上 (而不是分散於裝置上) 嘗試執行應用程式並與其互動，您可以在本機測試模式中建置主機應用程式。 這個模式完全不會使用網路。 相反地，主機應用程式的 UI 會在視窗左邊顯示主機部分，並在右邊顯示兩個垂直堆疊的用戶端應用程式 UI 複本 (請注意，在這個版本中，本機測試模式 UI 在電腦顯示器上是固定的；它不適合小型裝置)。 這些 UI 區段 (全部都在同一個 app 中) 會透過模擬用戶端 Communicator 彼此通訊，其會模擬透過網路執行的互動。

若要啟用本機測試模式，請定義 **LOCALTESTMODEON** (在專案屬性中) 做為條件式編譯符號，並進行重建。

## <a name="porting-to-a-windows10-project"></a>移植到 windows 10 專案

QuizGame 含有下列項目。

-   P2PHelper。 這是一個可移植的類別庫，其中包含對等網路邏輯。
-   QuizGame.Windows。 這是建置主機應用程式，針對目標為 Windows8.1 應用程式套件的專案。
-   QuizGame.WindowsPhone。 這個專案會針對目標為 Windows Phone 8.1 的用戶端應用程式建置應用程式套件。
-   QuizGame.Shared。 這是包含其他兩個專案也會用到之原始程式碼、標記檔案及其他資產與資源的專案。

在這個案例研究中，我們擁有[如果您有通用 8.1 App](w8x-to-uwp-root.md)中所述之與支援哪些裝置有關的常見選項。

根據這些選項，我們會 QuizGame.Windows 移植到名為 QuizGameHost 的新 windows 10 專案。 而且，我們將 QuizGame.WindowsPhone 移植到名為 QuizGameClient 的新 windows 10 專案。 這些專案會將目標設為通用裝置系列，因此它們將可在任何裝置上執行。 而我們會在它們自己的資料夾中保留 QuizGame.Shared 來源檔案等項目，並將這些共用檔案連結到這兩個新專案。 就像之前一樣，我們會將所有項目保留於單一方案中，並將它命名為 QuizGame10。

**QuizGame10 方案**

-   建立新方案 (**新的專案** &gt; **其他專案類型** &gt; **Visual Studio 方案**)，並命名為 QuizGame10。

**P2PHelper**

-   在方案中，建立新的 windows 10 類別庫專案 (**新的專案** &gt; **Windows 通用** &gt; **類別庫 （Windows 通用）**) 並將它命名為 P2PHelper。
-   從新專案刪除 Class1.cs。
-   將 P2PSession.cs、P2PSessionClient.cs 及 P2PSessionHost.cs 複製到新專案的資料夾，並在新專案中包含複製的檔案。
-   不需進一步變更即可建置專案。

**共用檔案**

-   將資料夾 Common、Model、View 及 ViewModel 從 \\QuizGame.Shared\\ 複製到 \\QuizGame10\\。
-   Common、Model、View 及 ViewModel 是我們在指稱磁碟上的共用資料夾時所表示的意思。

**QuizGameHost**

-   建立新的 windows 10 app 專案 (**新增** &gt; **新專案** &gt; **Windows 通用** &gt; **（Windows 通用） 的空白應用程式**) 並將它命名為 QuizGameHost。
-   新增 P2PHelper 的參考 (**加入參考]** &gt; **專案** &gt; **方案** &gt; **命名為 P2PHelper**)。
-   在 [方案總管] 中，於磁碟上的每個共用資料夾中建立新的資料夾。 接著，以滑鼠右鍵按一下您剛建立的每個資料夾，然後按一下 [**新增**] &gt; **現有項目**和瀏覽資料夾。 開啟適當的共用資料夾，並選取所有檔案，然後按一下 [加入做為連結]。
-   將 MainPage.xaml 從 \\QuizGame.Windows\\ 複製到 \\QuizGameHost\\，然後將命名空間變更為 QuizGameHost。
-   將 App.xaml 從 \\QuizGame.Shared\\ 複製到 \\QuizGameHost\\，然後將命名空間變更為 QuizGameHost。
-   我們不會覆寫 app.xaml.cs，而是在新專案中保留該版本，只需對它進行一個目標變更，就能支援本機測試模式。 在 app.xaml.cs 中，取代這行程式碼：

```CSharp
rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

取代為：

```CSharp
#if LOCALTESTMODEON
    rootFrame.Navigate(typeof(TestView), e.Arguments);
#else
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
#endif
```

-   在**屬性** &gt; **建置** &gt; **條件式編譯符號**，新增 LOCALTESTMODEON。
-   現在可以回到您新增到 app.xaml.cs 的程式碼，並解析 TestView 類型。
-   在 package.appxmanifest 中，將功能名稱從 internetClient 變更為 internetClientServer。

**QuizGameClient**

-   建立新的 windows 10 app 專案 (**新增** &gt; **新專案** &gt; **Windows 通用** &gt; **（Windows 通用） 的空白應用程式**) 並將它命名為 QuizGameClient。
-   新增 P2PHelper 的參考 (**加入參考]** &gt; **專案** &gt; **方案** &gt; **命名為 P2PHelper**)。
-   在 [方案總管] 中，於磁碟上的每個共用資料夾中建立新的資料夾。 接著，以滑鼠右鍵按一下您剛建立的每個資料夾，然後按一下 [**新增**] &gt; **現有項目**和瀏覽資料夾。 開啟適當的共用資料夾，並選取所有檔案，然後按一下 [加入做為連結]。
-   將 MainPage.xaml 從 \\QuizGame.WindowsPhone\\ 複製到 \\QuizGameClient\\，然後將命名空間變更為 QuizGameClient。
-   將 App.xaml 從 \\QuizGame.Shared\\ 複製到 \\QuizGame.Shared\\，然後將命名空間變更為 QuizGameClient。
-   在 package.appxmanifest 中，將功能名稱從 internetClient 變更為 internetClientServer。

您現在可以進行建置並執行。

## <a name="adaptive-ui"></a>彈性 UI

應用程式 （也就是只有配有大型螢幕的裝置） 在寬型視窗中執行時，QuizGameHost windows 10 應用程式看起來就足夠。 若應用程式的視窗較窄 (這會發生於小型裝置上，也會發生於大型裝置上)，UI 便會受到擠壓而使其變得難以閱讀。

我們可以使用彈性的 Visual State Manager 功能來解決這個問題，如[案例研究：Bookstore2](w8x-to-uwp-case-study-bookstore2.md) 中所述。 首先，在視覺元素上設定屬性，讓 UI 配置預設是窄型狀態。 這所有的變更都是在 \\View\\HostView.xaml 中進行。

-   在主要的 **Grid** 中，將第一個 **RowDefinition** 的 **Height** 從 "140" 變更為 "Auto"。
-   在包含 **TextBlock** (名稱為 `pageTitle`) 的 **Grid** 上，設定 `x:Name="pageTitleGrid"` 和 `Height="60"`。 這前兩個步驟是讓我們能夠透過視覺狀態中的 setter，有效控制該 **RowDefinition** 的高度。
-   在 `pageTitle` 中，設定 `Margin="-30,0,0,0"`。
-   在註解 `<!-- Content -->` 指示的 **Grid** 上，設定 `x:Name="contentGrid"` 和 `Margin="-18,12,0,0"`。
-   隨即在註解 `<!-- Options -->` 上的 **TextBlock**，設定 `Margin="0,0,0,24"`。
-   在預設的 **TextBlock** 樣式 (檔案中的第一個資源) 中，將 **FontSize** setter 的值變更為 "15"。
-   在 `OptionContentControlStyle` 中，將 **FontSize** setter 的值變更為 "20"。 這個步驟和前一步驟會提供可在所有裝置上良好運作的字體坡形。 這些是比我們針對 Windows8.1 應用程式所使用的"30"還要有彈性的大小。
-   最後，將適當的 Visual State Manager 標記新增到根 **Grid**。

```xml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="WideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="548"/>
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="pageTitleGrid.Height" Value="140"/>
                <Setter Target="pageTitle.Margin" Value="0,0,30,40"/>
                <Setter Target="contentGrid.Margin" Value="40,40,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

## <a name="universal-styling"></a>通用樣式


您會注意到在 windows 10，按鈕將不具有 padding 在其範本中相同觸控目標。 只需進行兩個小變更，就能補救這一點。 首先，將這個標記新增到 QuizGameHost 和 QuizGameClient 中的 app.xaml。

```xml
<Style TargetType="Button">
    <Setter Property="Margin" Value="12"/>
</Style>
```

接著，將這個 setter 新增到 \\View\\ClientView.xaml 中的 `OptionButtonStyle`。

```xml
<Setter Property="Margin" Value="6"/>
```

透過最後一個調校，app 將能運作，而且看起來就和它在移植之前一樣，透過這個額外的值，讓它能夠在任何地方執行。

## <a name="conclusion"></a>總結

我們在此研究案例中移植的應用程式相對複雜許多，因為它涉及了數個專案、一個類別庫，以及相當大量的程式碼與使用者介面。 即便如此，移植還是簡單易懂的。 部分的輕鬆移植可直接歸因 windows 10 開發人員平台及 Windows8.1 和 Windows Phone 8.1 的平台間的相似性。 有些則是因為原始應用程式的設計方式是個別保留模型、檢視模型及檢視。
