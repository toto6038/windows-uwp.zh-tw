---
author: martinekuan
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: "建立 Hello, world 應用程式 (XAML)"
description: "本教學課程會教您如何使用 Extensible Application Markup Language (XAML) 搭配 C# 來建立目標是 Windows 10 上通用 Windows 平台 (UWP) 的簡單 Hello, world 應用程式。"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 0a524d51f713c37ce2069b4e750bf3ed20fe19ab

---

# 建立 "Hello, world" 應用程式 (XAML)

本教學課程會教您如何使用 Extensible Application Markup Language (XAML) 搭配 C# 來建立目標是 Windows 10 上的 Universal Windows Platform (UWP) 簡單 "Hello, world" app。 使用 Microsoft Visual Studio 中的單一專案，您可以建置可在任何 Windows 10 裝置上執行的 app。 這裡的重點是建立可在傳統型裝置和行動裝置上順利執行的 app。

**重要** 本教學課程與 Microsoft Visual Studio 2015 與 Windows 10 搭配使用。 在較早的版本中無法正常運作。

您將在此處了解如何：

-   建立目標是 Windows 10 和 UWP 的新 Visual Studio 專案。
-   將 XAML 內容新增到起始頁。
-   處理觸控、手寫筆以及滑鼠輸入。
-   在本機桌面上和 Visual Studio 的手機模擬器上執行專案。
-   隨不同的螢幕大小調整 UI。

## 開始之前...


-   本教學課程直接進入建立簡單通用 app 的步驟。 因此在開始本教學課程之前，我們強烈建議您閱讀並了解 [Windows 10 的新功能](https://dev.windows.com/whats-new-windows-10-dev-preview)和[什麼是通用 Windows 應用程式](whats-a-uwp.md)的概觀資訊。
-   若要完成這個教學課程，您需要 Windows 10 與 Visual Studio 2015。 如需詳細資訊，請參閱[開始設定](get-set-up.md)。
-   我們假設您對 XAML 和 [XAML 概觀](https://msdn.microsoft.com/library/windows/apps/Mt185595)的概念有基本的了解。
-   我們亦假設您使用的是 Visual Studio 中預設的視窗配置。 如果您變更預設配置，您可以使用 [視窗]**** 功能表中的 [重設視窗配置]**** 命令重設它。

##  步驟 1：在 Visual Studio 中建立新專案


1.  啟動 Visual Studio 2015。

   Visual Studio 2015 起始頁隨即顯示。 (以下將 Visual Studio 2015 簡稱為 Visual Studio。)

2.  在 [檔案]**** 功能表上，選取 [新增]**** > [專案]****。

   隨即顯示 [新增專案]**** 對話方塊。 您可以在對話方塊的左窗格中選取要顯示的範本類型。

3.  在左窗格中，依序展開 [已安裝的] &gt; [範本] &gt; [Visual C#] &gt; [Windows]****，然後選取 [通用]**** 範本群組。 對話方塊的中央窗格會顯示通用 Windows 平台 (UWP) app 的專案範本清單。

   ![[新增專案] 視窗 ](images/newproject-cs.png)
   
   (如果您沒有看到這些選項，請確定您已經安裝「通用 Windows 應用程式開發工具」。 如需詳細資訊，請參閱[開始設定](get-set-up.md)。)

4.  在中央窗格中，選取 [空白應用程式 (通用 Windows)]**** 範本。

   [空白應用程式]**** 範本可以建立能夠編譯、執行的最基本 UWP 應用程式，但不包含使用者介面控制項或資料。 在本教學課程中，您會將控制項新增到 app。

5.  在 [名稱]**** 文字方塊中，輸入 "HelloWorld"。
6.  按一下 [確定]**** 來建立專案。

   Visual Studio 會建立您的專案，然後在 [方案總管]**** 中顯示。

   ![HelloWorld 專案的 Visual Studio 方案總管](images/solutionexplorer-cs.png)

雖然 [空白應用程式]**** 是最基本的範本，但是仍然包含些許檔案：

-   一個資訊清單檔案 (Package.appxmanifest)，說明應用程式的名稱、描述、磚、起始頁等，並列出其中包含的檔案。
-   在 [開始] 功能表上顯示的一組標誌影像 (Assets/Square150x150Logo.scale-200.png、Assets/Square44x44Logo.scale-200.png 和 Assets/Wide310x150Logo.scale-200.png)。
-   在 Windows 市集中代表您的應用程式的影像 (Assets/StoreLogo.png)。
-   應用程式啟動時顯示的啟動顯示畫面 (Assets/SplashScreen.scale-200.png)。
-   應用程式的 XAML 和程式碼檔案 (App.xaml 和 App.xaml.cs)。
-   應用程式啟動時執行的起始頁 (MainPage.xaml) 和伴隨程式碼檔案 (MainPage.xaml.cs)。

這些檔案對於所有使用 C# 的 UWP app 都是必要的。 您在 Visual Studio 中建立的任何專案都包含這些檔案。

## 步驟 2：修改起始頁


### 檔案提供哪些內容？

若要在專案中檢視和編輯檔案，請按兩下 [方案總管]**** 中的檔案。 根據預設，您可以像展開資料夾一樣展開 XAML 檔案，檢視它的關聯程式碼檔案。 XAML 檔案會在分割檢視中開啟，同時顯示設計表面與 XAML 編輯器。

在本教學課程中，您僅會使用上述所列的一些檔案：App.xaml、MainPage.xaml 以及 MainPage.xaml.cs。

### App.xaml 和 App.xaml.cs

App.xaml 是宣告整個應用程式使用資源的檔案。 App.xaml.cs 是 App.xaml 的程式碼後置檔案。 程式碼後置是加入 XAML 頁面中部分類別的程式碼。 XAML 與程式碼後置搭配使用構成完整的類別。 App.xaml.cs 是您應用程式的進入點。 它就像所有程式碼後置頁面一樣，包含一個呼叫 `InitializeComponent` 方法的建構函式。 您不需要撰寫 `InitializeComponent` 方法。 它是由 Visual Studio 產生的，而且主要用途是初始化 XAML 檔案中宣告的元素。 App.xaml.cs 也包含處理應用程式啟用和暫停的方法。

### MainPage.xaml

您在 MainPage.xaml 檔案中定義應用程式的 UI。 您可以使用 XAML 標記直接新增元素，或是使用 Visual Studio 提供的設計工具。 MainPage.xaml.cs 是 MainPage.xaml 的程式碼後置頁面。 其正是您新增應用程式邏輯和事件處理常式的位置。

這兩個檔案定義名為 `MainPage` 的新類別，這是繼承自 `HelloWorld` 命名空間中的 [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503)。

MainPage.xaml

```xml
    <Page
    x:Class="HelloWorld.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    </Grid>
</Page>
```

MainPage.xaml.cs

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace HelloWorld
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }
    }
}
```

### 修改起始頁

現在，我們在應用程式新增一些內容。

**修改起始頁**

1.  在 [方案總管]**** 中按兩下 [MainPage.xaml] 以將它開啟。
2.  在 XAML 編輯器中新增 UI 的控制項。

   在根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 中，新增這個 XAML。 它包含含有標題 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/BR209635)、詢問使用者名稱的 **TextBlock**、接受使用者名稱的 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 元素、[**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 以及顯示問候語的另一個 **TextBlock** 元素。 這些控制項的其中一部分具有名稱，以便您稍後在程式碼中參考它們。

```xml    
    <StackPanel x:Name="contentPanel" Margin="8,32,0,0">
        <TextBlock Text="Hello, world!" Margin="0,0,0,40"/>
        <TextBlock Text="What' s your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="280" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
```    

    The controls that you added in the XAML editor show up in the design view.

## 步驟 3：啟動應用程式


到目前為止，您已經建立了一個非常簡單的 App。 您可以趁現在建置、部署和啟動您的應用程式，並看看它的外觀。 您可以在本機電腦、模擬器或遠端裝置上進行應用程式的偵錯。 以下是在 Visual Studio 中的目標裝置功能表。

![用於偵錯應用程式的裝置目標下拉式清單](images/uap-debug.png)

### 在傳統型裝置上啟動應用程式

根據預設，應用程式會在本機電腦上執行。 目標裝置功能表提供從傳統型裝置系列的裝置偵錯應用程式的數個選項。

-   **模擬器**
-   **本機電腦 
**
-   **遠端電腦**

**在本機電腦上開始偵錯**

1.  在目標裝置功能表 (![開始偵錯功能表](images/startdebug-full.png)) 的 [標準]**** 工具列上，確定已選取 [本機電腦]****。 (這是預設選項)。
2.  按一下工具列上的 [開始偵錯]**** 按鈕 (![開始偵錯按鈕](images/startdebug-sm.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [開始偵錯]****。

   –或–

   按 F5。

應用程式會在視窗中開啟，預設啟動顯示畫面會先顯示。 啟動顯示畫面是由影像 (SplashScreen.png) 和背景色彩 (在 app 的資訊清單檔案中指定) 所定義。

啟動顯示畫面消失之後，接著就出現您的應用程式。 它的外觀如下。

![最初的應用程式畫面](images/helloworld-1-cs.png)

按下 Windows 鍵以開啟 [開始]**** 功能表，然後顯示所有 app。 請注意，在本機部署應用程式會在 [開始] ****功能表上新增磚。 若要再次執行應用程式 (不在偵錯模式)，請點選或按一下 [開始]**** 功能表中的磚。

應用程式還沒有太多功能—但是，恭喜您，您已經建置第一個 UWP app 了！

**停止偵錯**

-   按一下工具列中的 [停止偵錯]**** 按鈕 (![停止偵錯按鈕](images/stopdebug.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [停止偵錯]****。

   –或–

   關閉應用程式視窗。

### 在行動裝置模擬器上啟動 app

您的應用程式會在所有 Windows 10 裝置上執行，因此，我們來看看它在 Windows Phone 上的外觀如何。

除了在傳統型裝置上偵錯的選項外，Visual Studio 還提供在連接到電腦的實體行動裝置或在行動裝置模擬器上部署和偵錯應用程式的選項。 您可以為有不同記憶體和顯示器組態的裝置選擇不同的模擬器。

-   **裝置**
-   **模擬器 <SDK version> WVGA 4 吋 512MB**
-   **模擬器 <SDK version> WVGA 4 吋 1GB**
-   等等... (其他組態中的各種模擬器)

(如果您沒有看到模擬器，請確定您已經安裝「通用 Windows 應用程式開發工具」。 如需詳細資訊，請參閱[開始設定](get-set-up.md)。)

在小螢幕和記憶體有限的裝置上測試您的應用程式是不錯的想法，因此，請使用 [模擬器 10.0.10240.0 WVGA 4 inch 512MB]**** 選項。
**在行動裝置模擬器上開始偵錯**

1.  在目標裝置功能表 (![開始偵錯功能表](images/startdebug-full.png)) 的 [標準]**** 工具列上，選擇 [模擬器 10.0.10240.0 WVGA 4 inch 512MB]****。
2.  按一下工具列中的 [開始偵錯]**** 按鈕 (![開始偵錯按鈕](images/startdebug-sm.png))。

   –或–

   在 [偵錯]**** 功能表中，按一下 [開始偵錯]****。

   –或–

   按 F5。

Visual Studio 會啟動選取的模擬器，然後部署和啟動您的應用程式。 在行動裝置模擬器上，應用程式看起來會像這樣。

![行動裝置上最初的應用程式畫面](images/helloworld-1-cs-phone.png)

您會注意到的第一件事就是按鈕推入行動裝置較小的螢幕。 在本教學課程後面，您會學習如何隨著不同的螢幕大小調整 UI，讓您的 app 永遠保持美觀。

您可能也會注意到您可以在 [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 中輸入文字，但是在這個時候，按一下或輕觸 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 並不會執行任何動作。 在後續步驟中，您要為顯示個人化問候語的按鈕 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件建立一個事件處理常式。 您要將事件處理常式程式碼新增到 MainPage.xaml.cs 檔案。

## 步驟 4：建立事件處理常式


XAML 元素可以在特定事件發生時傳送訊息。 這些事件訊息讓您有機會採取動作來回應事件。 您可以將回應事件的程式碼放置在事件處理常式方法中。 在許多應用程式中，其中一個最常見的事件就是使用者按一下 [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)。

讓我們為您按鈕的 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件建立一個事件處理常式。 這個事件處理常式會從 `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 控制項取得使用者的名稱，並使用它將問候語輸出到 `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)。

### 使用適用於觸控、滑鼠及手寫筆輸入的事件

您應該處理什麼事件？ 因為這些事件可以在各種裝置上執行，所以設計具有觸控輸入的 Windows 市集應用程式時，請記住這一點。 您的應用程式也必須能夠處理滑鼠或手寫筆的輸入。 幸運的是，[**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 和 [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/BR208922) 這類的事件與裝置無關。 如果您熟悉 Microsoft .NET 程式設計，可能已經看過針對滑鼠、觸控及手寫筆輸入的個別事件，如 **MouseMove**、**TouchMove** 以及 **StylusMove**。 在 Windows 市集應用程式中，這些分開的事件會由同樣適用於觸控、滑鼠及手寫筆輸入的單一 [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/BR208970) 事件取代。

**新增事件處理常式**

1.  在 XAML 或設計檢視中，選取新增到 MainPage.xaml 的 "Say Hello" [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265)。
2.  在 [屬性]**** 視窗中，按一下 [事件] 按鈕 (![事件按鈕](images/eventsbutton.png))。
3.  在事件清單的最上方尋找 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件。 在事件的文字方塊中，輸入處理 **Click** 事件的函式名稱。 在這個範例中，輸入 "Button\_Click"。

   ![[屬性] 視窗中的事件清單](images/xaml-hw-event.png)

4.  按 Enter 鍵。 這時候會建立事件處理常式方法並在程式碼編輯器中開啟，如此一來，您就可以新增事件發生要執行的程式碼。

    在 XAML 編輯器中，[**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) 的 XAML 已更新來宣告 [**Click**](https://msdn.microsoft.com/library/windows/apps/BR227737) 事件處理常式，像這樣。

```xml   
   <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
```    

5.  將程式碼新增到在程式碼後置頁面中建立的事件處理常式。 在事件處理常式中，從 `nameInput` [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) 控制項擷取使用者的名稱，並用它來建立問候語。 使用 `greetingOutput` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 顯示結果。
    
```csharp    
    private void Button_Click(object sender, RoutedEventArgs e)
    {
        greetingOutput.Text = "Hello, " + nameInput.Text + "!";
    }
```    

6.  在本機電腦上偵錯應用程式。 在文字方塊中輸入您的名稱並按一下按鈕時，App 會顯示個人化問候語。

## 步驟 5：隨不同的視窗大小調整 UI。


現在要讓 UI 可隨著不同的螢幕大小進行調整，使其在行動裝置上看起來很美觀。 若要這樣做，您要新增 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)，並設定不同視覺狀態套用的屬性。

**調整 UI 配置**

1.  在 XAML 編輯器中，將 XAML 的這個區塊新增到根 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 元素的開頭標記之後。

```xml    
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
```    

2.  在本機電腦上偵錯應用程式。 請注意，除非視窗的寬度小於 641 畫素，否則 UI 看起來會和之前的一樣。
3.  在行動裝置模擬器上偵錯應用程式。 請注意，UI 會使用您在 `narrowState` 中定義的屬性，並正確地顯示在小型螢幕上。

![行動 app 畫面](images/helloworld-2-cs-phone.png)

如果您已在舊版的 XAML 中使用 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/BR209021)，您可能會注意到這裡的 XAML 使用簡化的語法。

名為 `wideState` 的 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 有一個 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382) 的[**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) 屬性設為 641。 這表示只有在視窗寬度不小於最小值 641 畫素時才會套用狀態。 您不需為此狀態定義任何 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 物件，因此它會使用您在頁面內容的 XAML 中定義的配置屬性。

第二個 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) (`narrowState`) 有一個 [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn890382) 的 [**MinWindowWidth**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.adaptivetrigger.minwindowwidth) 屬性設為 0。 當視窗寬度大於 0 但小於 641 畫素時，會套用這個狀態。 (在 641 像素時，會套用 `wideState`)。在這個狀態中，您要定義一些 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 物件，以變更 UI 中的控制項配置屬性：

-   將 `inputPanel` 元素的 [**Orientation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.orientation) 從 **Horizontal** 變更為 **Vertical**。
-   新增上邊界為 4 至 `inputButton` 元素。

## 摘要


恭喜您！您已經完成適用於 Windows 10 和 UWP 的第一個 app 了！



<!--HONumber=Jul16_HO2-->


