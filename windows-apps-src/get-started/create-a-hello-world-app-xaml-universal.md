---
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: 了解如何建立 "Hello, world" 應用程式 (XAML)
description: 使用 Extensible Application Markup Language (XAML) 搭配 C# 來建立以 Windows 10 上通用 Windows 平台 (UWP) 為目標的簡單 Hello, world 應用程式。
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, first app, hello world, 第一個應用程式
ms.localizationpriority: medium
ms.openlocfilehash: 93c78845a218620a8a46fc4439733734099b9853
ms.sourcegitcommit: 13ce25364201223e21e2e5e89f99bc7aa4d93f56
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847608"
---
# <a name="create-a-hello-world-app-xaml"></a>建立 Hello, world App (XAML)

本教學課程會教您如何使用 XAML 和 C# 建立適用於 Windows 10 上通用 Windows 平台 (UWP) 的簡單 Hello, world 應用程式。 透過 Microsoft Visual Studio 中的單一專案，您便可以建置可在任何 Windows 10 裝置上執行的應用程式。

您將在此處了解如何：

-   建立以 **Windows 10** 和 **UWP** 為目標的新 **Visual Studio** 專案。
-   撰寫 XAML 以變更您開始頁面上的 UI。
-   在本機電腦上於 Visual Studio 中執行專案。
-   使用 SpeechSynthesizer 讓 App 在您按下按鈕時說話。


## <a name="before-you-start"></a>開始之前...

-   [什麼是 Windows 通用應用程式？](universal-application-platform-guide.md)
-   [下載 Visual Studio 2017 (和 Windows 10)](https://developer.microsoft.com/windows/downloads)。 如果您需要協助，請了解如何[開始設定](get-set-up.md)。
-   我們亦假設您使用的是 Visual Studio 中預設的視窗配置。 如果您變更預設配置，您可以在 [視窗]  功能表中使用 [重設視窗配置]  命令來重設它。

> [!NOTE]
> 本教學課程使用 Visual Studio Community 2017。 如果您使用不同版本的 Visual Studio，它的外觀可能會略有不同。

## <a name="video-summary"></a>影片摘要

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Writing-Your-First-Windows-10-App/player]

## <a name="step-1-create-a-new-project-in-visual-studio"></a>步驟 1：在 Visual Studio 中建立新專案。

1.  啟動 Visual Studio。

2.  從 [檔案]  功能表，選取 [新增] > [專案]  來開啟 [新增專案]  對話方塊。

3.  從左邊的範本清單中，選擇 [已安裝] > [Visual C#] > [Windows 通用]  來查看 UWP 專案範本的清單。

    (如果您沒有看到任何「通用」範本，表示您可能遺失用於建立 UWP 應用程式的元件。 您可以重複安裝程序並加入 UWP 支援，方法是按一下 [新增專案]  對話方塊上的 [開啟 Visual Studio 安裝程式]  。 請參閱[開始設定](get-set-up.md)。)

    ![如何重複安裝程序](images/win10-cs-install.png)

4.  選擇 [空白應用程式 (通用 Windows)]  範本，然後輸入 "HelloWorld" 作為 [名稱]  。 選取 [確定]  。

    ![[新增專案] 視窗](images/win10-cs-01.png)

> [!NOTE]
> 如果這是您第一次使用 Visual Studio，您可能會看到 [設定] 對話方塊要求您啟用 [開發人員模式]  。 開發人員模式是啟用某些功能的特殊設定，例如，直接執行應用程式的權限，而非只執行來自 Microsoft Store 的。 如需詳細資訊，請閱讀[啟用您的裝置以進行開發](enable-your-device-for-development.md)。 若要繼續使用此指南，請選取 [開發人員模式]  ，按一下 [是]  ，並關閉對話方塊。

 ![啟用開發人員模式對話方塊](images/win10-cs-00.png)

5.  [目標版本/最小版本] 對話方塊隨即出現。 在這個教學課程中使用預設設定即可，因此請選取 [確定]  來建立專案。

    ![[方案總管] 視窗](images/win10-cs-02.png)

6.  當您的新專案開啟時，其檔案會顯示在右邊的 [方案總管]  窗格中。 您可能需要選擇 [方案總管]  索引標籤 (而不是 [屬性]  索引標籤)，才能看到您的檔案。

    ![[方案總管] 視窗](images/win10-cs-03.png)

雖然 [空白應用程式 (通用 Windows)]  是最基本的範本，它仍然包含許多檔案。 這些檔案對於所有使用 C# 的 UWP app 都是必要的。 您在 Visual Studio 中建立的每個專案都包含這些檔案。


### <a name="whats-in-the-files"></a>檔案提供哪些內容？

若要檢視和編輯專案中的檔案，請在 [方案總管]  中按兩下該檔案。 像展開資料夾一樣展開 XAML 檔案，以檢視它的關聯程式碼檔案。 XAML 檔案會在分割檢視中開啟，同時顯示設計介面與 XAML 編輯器。
> [!NOTE]
> 什麼是 XAML？ Extensible Application Markup Language (XAML) 是一種用來定義您 App 使用者介面的語言。 您可以手動輸入它，或使用 Visual Studio 設計工具來建立它。 .xaml 檔案具有一個包含邏輯的 .xaml.cs 程式碼後置檔案。 XAML 與程式碼後置一起構成完整的類別。 如需詳細資訊，請參閱 [XAML 概觀](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview)。

「App.xaml 和 App.xaml.cs」 

-   App.xaml 是宣告整個應用程式使用資源的檔案。
-   App.xaml.cs 是 App.xaml 的程式碼後置檔案。 它就像所有程式碼後置頁面一樣，包含一個呼叫 `InitializeComponent` 方法的建構函式。 您不需要撰寫 `InitializeComponent` 方法。 它是由 Visual Studio 產生的，而且主要用途是初始化 XAML 檔案中宣告的元素。
-   App.xaml.cs 是您 App 的進入點。
-   App.xaml.cs 也包含處理應用程式[啟用](../launch-resume/activate-an-app.md)和[暫停](../launch-resume/suspend-an-app.md)的方法。

「MainPage.xaml」 

-   MainPage.xaml 是您為 App 定義 UI 的地方。 您可以使用 XAML 標記直接新增元素，或是使用 Visual Studio 提供的設計工具。
-   MainPage.xaml.cs 是 MainPage.xaml 的程式碼後置頁面。 其正是您新增應用程式邏輯和事件處理常式的位置。
-   這兩個檔案一起定義名為 `MainPage` 的新類別，這是繼承自 `HelloWorld` 命名空間中的 [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page)。

「Package.appxmanifest」 
-   一個描述您 App 的資訊清單檔案：其名稱、描述、磚、開始頁面等。
-   包含您應用程式所包含的相依性、資源和檔案的清單。

「一組標誌影像」 
-   Assets/Square150x150Logo.scale-200.png 和 Wide310x150Logo.scale-200.png 會在 [開始] 功能表中代表您的應用程式 (無論是 [中] 還是 [寬] 大小)。
-   Assets/Square44x44Logo.png 會在 [開始] 功能表的應用程式清單、工作列，以及工作管理員中代表您的應用程式。
-   Assets/StoreLogo.png 會在 Microsoft Store 中代表您的應用程式。
-   Assets/SplashScreen.scale-200.png 是您 App 啟動時顯示的啟動顯示畫面。
-   Assets/LockScreenLogo.scale-200.png 可以用來在系統鎖定時的鎖定畫面上代表應用程式。

## <a name="step-2-adding-a-button"></a>步驟 2：新增按鈕

### <a name="using-the-designer-view"></a>使用設計工具檢視

讓我們將按鈕新增到我們的頁面中。 在本教學課程中，您僅會使用上述所列的一些檔案：App.xaml、MainPage.xaml 以及 MainPage.xaml.cs。

1.  按兩下 **MainPage.xaml** 以在 [設計] 檢視中開啟它。

    您會注意到畫面上半部有一個圖形檢視，而 XAML 程式碼檢視則在下方。 您可以對任一檢視進行變更，但目前我們會使用圖形檢視。

    ![[方案總管] 視窗](images/win10-cs-04.png)

2.  按一下左邊的垂直 [工具箱]  索引標籤來開啟 UI 控制項清單。 (您可以按一下其標題列中的釘選圖示來讓它保持可見)。

    ![[方案總管] 視窗](images/win10-cs-05.png)

3.  展開 [通用 XAML 控制項]  ，然後將 [按鈕]  拖曳到設計畫布的中間。

    ![[方案總管] 視窗](images/win10-cs-06.png)

    如果您看一下 XAML 程式碼視窗，您就會看到那裡也已新增該「按鈕」。

 ```XAML
<Button x:Name="button" Content="Button" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
 ```

4.  變更按鈕的文字。

    在 XAML 程式碼檢視中按一下，然後將內容從「按鈕」變更為 "Hello, world!"。

```XAML
<Button x:Name="button" Content="Hello, world!" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
```

請注意設計畫布中顯示的按鈕如何更新成顯示新的文字。

![[方案總管] 視窗](images/win10-cs-07.png)

## <a name="step-3-start-the-app"></a>步驟 3：啟動應用程式


到目前為止，您已經建立了一個非常簡單的 App。 您可以趁現在建置、部署和啟動您的應用程式，並看看它的外觀。 您可以在本機電腦、模擬器或遠端裝置上進行應用程式的偵錯。 以下是在 Visual Studio 中的目標裝置功能表。

![用於偵錯應用程式的裝置目標下拉式清單](images/uap-debug.png)

### <a name="start-the-app-on-a-desktop-device"></a>在傳統型裝置上啟動應用程式

根據預設，應用程式會在本機電腦上執行。 目標裝置功能表提供從傳統型裝置系列的裝置偵錯應用程式的數個選項。

-   **模擬器**
-   **本機電腦**
-   **遠端電腦**

**在本機電腦上開始偵錯**

1.  在 [標準]  工具列上的目標裝置功能表 (![開始偵錯功能表](images/startdebug-full.png)) 上，確定已選取 [本機電腦]  。 (這是預設選項)。
2.  按一下工具列上的 [開始偵錯]  按鈕 (![開始偵錯按鈕](images/startdebug-sm.png))。

   –或–

   從 [偵錯]  功能表，按一下 [開始偵錯]  。

   –或–

   按 F5。

應用程式會在視窗中開啟，預設啟動顯示畫面會先顯示。 啟動顯示畫面是由影像 (SplashScreen.png) 和背景色彩 (在 app 的資訊清單檔案中指定) 所定義。

啟動顯示畫面消失之後，接著就出現您的應用程式。 它的外觀如下。

![最初的應用程式畫面](images/win10-cs-08.png)

按下 Windows 鍵以開啟 [開始]  功能表，然後顯示所有應用程式。 請注意，在本機部署應用程式會將其磚加入 [開始]  功能表。 若要在稍後再次執行該應用程式 (不使用偵錯模式)，請點選或按一下它在 [開始]  功能表中的磚。

應用程式還沒有太多功能，但是恭喜您，您已經建置您的第一個 UWP app 了！

**停止偵錯**

   按一下工具列中的 [停止偵錯]  按鈕 (![停止偵錯按鈕](images/stopdebug.png))。

   –或–

   從 [偵錯]  功能表，按一下 [停止偵錯]  。

   –或–

   關閉應用程式視窗。

## <a name="step-4-event-handlers"></a>步驟 4：事件處理常式

「事件處理常式」聽起來很複雜，但它只是事件發生時 (例如使用者按一下按鈕) 所呼叫程式碼的另一個名稱。

1.  如果您尚未停止 App 執行，請停止 App 執行。

2.  按兩下設計畫布上的按鈕控制項，讓 Visual Studio 為您的按鈕建立事件處理常式。

  當然，您也可以手動建立所有的程式碼。 或者，您可以按一下按鈕來選取它，然後查看右下方的 [屬性]  窗格。 如果您切換到 [事件]  (小閃電圖示)，則可以新增事件處理常式的名稱。

3.  編輯 *MainPage.xaml.cs* 程式碼後置頁面中的事件處理常式程式碼。 這是開始有趣的地方。 預設的事件處理常式看起來就像這樣：

```cs
private void Button_Click(object sender, RoutedEventArgs e)
{

}
```

  讓我們來變更它，讓它看起來像這樣：

```cs
private async void Button_Click(object sender, RoutedEventArgs e)
{
    MediaElement mediaElement = new MediaElement();
    var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
    Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream = await synth.SynthesizeTextToStreamAsync("Hello, World!");
    mediaElement.SetSource(stream, stream.ContentType);
    mediaElement.Play();
}
```

請確定方法簽章現在也包含 **async** 關鍵字，否則您在嘗試執行應用程式時將會收到錯誤。

### <a name="what-did-we-just-do"></a>我們剛才做了什麼？

這個程式碼使用一些 Windows API 來建立語音合成物件，然後提供一些讓它說出的文字。 (如需有關使用 SpeechSynthesis 的詳細資訊，請參閱 [SpeechSynthesis 命名空間](https://docs.microsoft.com/uwp/api/windows.media.speechsynthesis)文件)。

當您執行 App 並按一下按鈕時，您的電腦 (或手機) 就會逐字說出 "Hello, World!"。


## <a name="summary"></a>摘要

恭喜！您已經建立您的第一個適用於 Windows 10 和 UWP 的應用程式！

若要了解如何使用 XAML 來配置應用程式將使用的控制項，請嘗試進行[格線教學課程](../design/layout/grid-tutorial.md)，或是直接進行[後續步驟](learn-more.md)？

## <a name="see-also"></a>另請參閱

* [您的第一個應用程式](your-first-app.md)
* [發佈您的 UWP 應用程式](https://docs.microsoft.com/windows/uwp/publish/)。
* [開發 UWP 應用程式的操作說明文章](https://docs.microsoft.com/windows/uwp/develop/)
* [適用於 UWP 開發人員的程式碼範例](https://developer.microsoft.com/windows/samples)
* [什麼是 Windows 通用應用程式？](universal-application-platform-guide.md)
* [註冊 Windows 帳戶](sign-up.md)
