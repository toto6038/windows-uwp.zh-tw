---
ms.assetid: ''
title: 在您的 Windows 應用程式中支援筆跡
description: 依照此逐步教學課程，瞭解如何在基本通用 Windows 平臺 (UWP) 應用程式中支援 Windows Ink 撰寫和繪製。
keywords: 筆跡、筆跡、教學課程
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f726f4ab4a422cc94f00493261620ddff8b6122b
ms.sourcegitcommit: d786d084dafee5da0268ebb51cead1d8acb9b13e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91860196"
---
# <a name="tutorial-support-ink-in-your-windows-app"></a>教學課程：在您的 Windows 應用程式中支援筆墨

![Surface 手寫筆主圖影像。](images/ink/ink-hero-small.png)  
*Surface 手寫筆* (可在 [Microsoft 網上商店](https://www.microsoft.com/p/surface-pen/8zl5c82qmg6b)購買)。

本教學課程會逐步說明如何建立支援使用 Windows Ink 撰寫和繪製的基本 Windows 應用程式。 我們會使用範例應用程式的程式碼片段，這您可以從 GitHub 下載 (請參閱[範例程式碼](#sample-code))，來展示每個步驟中所討論的各種不同功能和相關 Windows Ink API (請參閱 [Windows Ink 平台的元件](#components-of-the-windows-ink-platform))。

我們會著重於下列動作︰
* 新增基本筆跡支援
* 新增筆跡工具列
* 支援手寫辨識
* 支援基本圖形辨識
* 儲存和載入筆跡

如需有關如何執行這些功能的詳細資訊，請參閱 [Windows 應用程式中的畫筆互動和 Windows Ink](./pen-and-stylus-interactions.md)。

## <a name="introduction"></a>簡介

您可以藉由 Windows Ink 提供客戶幾乎相當於可想像的所有紙筆體驗的數位手寫功能，從快速的手寫筆記和註解到白板示範，以及從架構和工程繪圖到個人傑作。

## <a name="prerequisites"></a>必要條件

* 執行目前版本的 Windows 10 的電腦 (或虛擬機器)
* [Visual Studio 2019 與 RS2 SDK](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 視您的設定而定，您可能需要安裝 [NETCore Microsoft.netcore.universalwindowsplatform](https://www.nuget.org/packages/Microsoft.NETCore.UniversalWindowsPlatform) NuGet 套件，並在您的系統設定中啟用 **開發人員模式** (設定-> 更新 & 的安全性 > 開發人員-> 使用開發人員功能) 。
* 如果您不熟悉使用 Visual Studio 的 Windows 應用程式開發，請先參閱下列主題，再開始進行本教學課程：  
    * [開始設定](../../get-started/get-set-up.md)
    * [建立 Hello, world 應用程式 (XAML)](../../get-started/create-a-hello-world-app-xaml-universal.md)
* **[選擇性]** 數位手寫筆和配備支援手寫筆輸入的顯示器的電腦。

> [!NOTE] 
> 雖然 Windows Ink 可以支援使用滑鼠與觸控進行繪圖 (我們在本教學課程的步驟 3 中顯示做法) 以擁有最佳的 Windows Ink 體驗，我們仍建議您具有數位手寫筆和一部配備支援該數位手寫筆輸入之顯示器的電腦。

## <a name="sample-code"></a>範例程式碼
在本教學課程中，我們使用範例筆跡 app 來示範所討論的概念和功能。

從 [GitHub](https://github.com/) 的 [windows-appsample-get-started-ink sample](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink) 下載此 Visual Studio 範例和原始程式碼：

1. 選取綠色的 **\[Clone or download\]**(複製或下載) 按鈕  
![複製存放庫。](images/ink/ink-clone.png)
2. 如果您有 GitHub 帳戶，您可以選擇 **\[在 Visual Studio 中開啟\]** 將存放庫複製到您的本機電腦 
3. 如果您沒有 GitHub 帳戶，或者您只想要專案的本機複本，請選擇 **\[下載 ZIP\]**(您必須定期返回以下載最新的更新)

> [!IMPORTANT]
> 範例中大部分的程式碼已標示註解。當我們逐步進行每個步驟時，您會被要求取消註解各個不同區段的程式碼。 在 Visual studio 中，只要反白顯示程式碼行，並按 CTRL-K 然後按 CTRL-U。

## <a name="components-of-the-windows-ink-platform"></a>Windows Ink 平台的元件

這些物件提供大量的 Windows 應用程式筆跡體驗。

| 元件 | 描述 |
| --- | --- |
| [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) | XAML UI 平台控制項，此控制項預設會接收來自畫筆的所有輸入，並顯示為筆墨筆劃或擦去筆劃。 |
| [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) | 程式碼後置物件，連同 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項 (透過 [**InkCanvas.InkPresenter**](/uwp/api/windows.ui.xaml.controls.inkcanvas.InkPresenter) 屬性所公開) 進行具現化。 此物件提供 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas)所公開的所有預設筆墨功能，以及一組完整的 api 來進行額外的自訂和個人化。 |
| [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar) | 包含可自訂與可擴充的按鈕集合 (可在關聯的 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 中啟用筆跡相關功能) 的 XAML UI 平台控制項。 |
| [**IInkD2DRenderer**](/windows/desktop/api/inkrenderer/nn-inkrenderer-iinkd2drenderer)<br/>我們在此不涵蓋此項功能，如需詳細資訊，請參閱[複雜的筆跡範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)。 | 可讓筆墨筆劃轉譯到通用 Windows app 的指定 Direct2D 裝置內容，而不是預設的 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項。 |

## <a name="step-1-run-the-sample"></a>步驟 1：執行範例

在您下載 RadialController 範例 App 之後，請確認它是否執行︰
1. 在 Visual Studio 中開啟範例專案。
2. 設定 **\[方案平台\]** 下拉式清單為非 ARM 選項。
3. 按下 F5 進行編譯、部署和執行。  

   > [!NOTE]
   > 或者，您可以選取 [ **Debug**  >  **開始調試**] 功能表項目，或選取此處顯示的 [**本機電腦**執行] 按鈕。
   > ![Visual Studio 建立專案] 按鈕。](images/ink/ink-vsrun-small.png)

應用程式視窗隨即開啟，並在啟動顯示畫面出現幾秒後，您會看到這個初始畫面。

![空白應用程式的螢幕擷取畫面。](images/ink/ink-app-step1-empty-small.png)

好了，現在我們將在本教學課程的其餘部分使用基本 Windows 應用程式。 在下列步驟中，我們會新增筆跡功能。

## <a name="step-2-use-inkcanvas-to-support-basic-inking"></a>步驟 2︰使用 InkCanvas 支援基本筆跡

或許您可能已經注意到該 App，它的初始形式，不會讓您使用手寫筆 (雖然您可以使用手寫筆做為標準指標裝置來與應用程式互動) 繪製任何項目。 

我們可在此步驟中修正少許缺點。

若要新增基本的筆墨功能，請將 [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) 控制項放在應用程式的適當頁面上。

> [!NOTE]
> InkCanvas 具有預設 [**Height**](/uwp/api/windows.ui.xaml.frameworkelement.Height) 和 [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.Width) 的零屬性，除非它是會自動調整其子項目大小之元素的子項目。 

### <a name="in-the-sample"></a>在範例中︰
1. 請開啟 MainPage.xaml.cs 檔案，
2. 尋找標有此步驟標題的程式碼 (「步驟 2：使用 InkCanvas 支援基本筆跡」)。
3. 取消註解下列行。 (後續步驟中所使用的功能需要這些參考資料)。  

``` csharp
    using Windows.UI.Input.Inking;
    using Windows.UI.Input.Inking.Analysis;
    using Windows.UI.Xaml.Shapes;
    using Windows.Storage.Streams;
```

4. 請開啟 MainPage.xaml 檔案，
5. 尋找以這個步驟的標題標示的程式碼 ( " \<!-- Step 2: Basic inking with InkCanvas --> " ) 。
6. 取消註解下列行。  

``` xaml
    <InkCanvas x:Name="inkCanvas" />
```

就這麼簡單！ 

現在，再次執行應用程式。 請繼續進行並徒手畫、撰寫您的名稱或者 (如果您有一面鏡子或者記性很好) 繪製您自己的自畫像。

![本主題反白顯示之基本筆墨範例應用程式的螢幕擷取畫面。](images/ink/ink-app-step1-name-small.png)

## <a name="step-3-support-inking-with-touch-and-mouse"></a>步驟 3︰使用觸控與滑鼠支援筆跡

您會注意到，預設筆跡功能只支援手寫筆輸入。 如果您嘗試用您的手指、滑鼠或觸控板書寫或繪圖，會讓您失望的。

如想讓自己別再愁眉苦臉，您需要新增第二行程式碼。 這次它是在您要宣告您的 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 的 XAML 檔案程式碼後置中。 

在此步驟中，我們會引入 [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) 物件，在您的 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 上提供較縝密的輸入管理、筆跡輸入 (標準和修改) 的處理和呈現。

> [!NOTE]
> 標準筆跡輸入 (手寫筆筆尖或橡皮擦/按鈕) 不會使用次要硬體能供性 (例如手寫筆筒狀按鈕、滑鼠右鍵或類似的機制) 進行修改。 

若要啟用滑鼠和觸控筆跡，請將 [**InputDeviceTypes**](/uwp/api/windows.ui.input.inking.inkpresenter.InputDeviceTypes) 屬性的 [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) 設定為您想要的 [**CoreInputDeviceTypes**](/uwp/api/windows.ui.core.coreinputdevicetypes) 值的組合。

### <a name="in-the-sample"></a>在範例中︰
1. 請開啟 MainPage.xaml.cs 檔案，
2. 尋找標有此步驟標題的程式碼 (「步驟 3︰使用觸控與滑鼠支援筆跡」)。
3. 取消註解下列行。  

``` csharp
    inkCanvas.InkPresenter.InputDeviceTypes =
        Windows.UI.Core.CoreInputDeviceTypes.Mouse | 
        Windows.UI.Core.CoreInputDeviceTypes.Touch | 
        Windows.UI.Core.CoreInputDeviceTypes.Pen;
```

再次執行的 App，您便會發現您想要在電腦螢幕上用手指畫圖的夢想成真！

> [!NOTE]
> 指定輸入裝置類型時，您必須指示支援每個特定輸入類型 (包括手寫筆)，因為設定這個屬性會覆寫預設 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 設定。

## <a name="step-4-add-an-ink-toolbar"></a>步驟 4︰新增筆跡工具列

[**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 是 UWP 平台控制項，提供可自訂和可擴充的按鈕集合以啟用筆跡相關功能。 

根據預設，[**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 包含一組基本的按鈕，可讓使用者迅速選取手寫筆、鉛筆、螢光筆或橡皮擦，當中的任何一個都可與樣板 (尺規或量角器) 一起使用。 手寫筆、鉛筆和螢光筆按鈕，每一個也都提供選取筆跡色彩和筆觸大小的飛出視窗。

若要新增預設的 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 到手寫筆跡應用程式，只要將它放在與 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 同一個頁面上並關聯兩個控制項即可。

### <a name="in-the-sample"></a>在範例中
1. 請開啟 MainPage.xaml 檔案，
2. 尋找以這個步驟的標題標示的程式碼 ( " \<!-- Step 4: Add an ink toolbar --> " ) 。
3. 取消註解下列行。  

``` xaml
    <InkToolbar x:Name="inkToolbar" 
                        VerticalAlignment="Top" 
                        Margin="10,0,10,0"
                        TargetInkCanvas="{x:Bind inkCanvas}">
    </InkToolbar>
```

> [!NOTE]
> 為盡可能整齊而簡單地保留 UI 和程式碼，請使用基本方格配置並在方格列中 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 的之後宣告 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)。 如果您在 [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) 之前宣告它，[**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 會先呈現在畫布下方，並且使用者無法存取。  

現在，再次執行以查看 [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) 並試試一些工具。

![本主題中以預設 InkToolbar 醒目提示之基本筆墨範例應用程式的螢幕擷取畫面。](images/ink/ink-inktoolbar-default-small.png)

### <a name="challenge-add-a-custom-button"></a>挑戰︰新增自訂按鈕
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>
<td>

以下是自訂 **[InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar)** (從 Windows Ink 工作區的繪圖板) 的範例。

![Ink 工作區中 Sketchpad 的筆墨工具列螢幕擷取畫面。](images/ink/ink-inktoolbar-sketchpad-small.png)

如需有關自訂 [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar)的詳細資訊，請參閱 [將 InkToolbar 新增至 Windows 應用程式筆跡應用程式](ink-toolbar.md)。

</td>
</tr>
</table>

## <a name="step-5-support-handwriting-recognition"></a>步驟 5︰支援手寫辨識

現在，您可以在您的 App 中寫字和繪圖，我們來使用這些徒手畫試試做些實用的東西。

在此步驟中，我們使用 Windows Ink 的手寫辨識功能，嘗試解讀您的筆跡。

> [!NOTE]
> 手寫辨識可以透過**手寫筆和 Windows Ink** 設定改進︰
> 1. 開啟 [開始] 功能表，然後選取 **\[設定\]**。
> 2. 從 [設定] 畫面中，選取 [**裝置**  >  **畫筆 &] Windows Ink**。
> ![[畫筆 & Windows Ink 設定] 頁面的螢幕擷取畫面。](images/ink/ink-settings-small.png)
> 3. 選取 **\[了解我的手寫內容\]** 以開啟 **\[個人化手寫\]** 對話方塊。
> ![[手寫辨識個人化] 對話方塊的螢幕擷取畫面。](images/ink/ink-settings-handwritingpersonalization-small.png)

### <a name="in-the-sample"></a>在範例中︰
1. 請開啟 MainPage.xaml 檔案，
2. 尋找以這個步驟的標題標示的程式碼 ( " \<!-- Step 5: Support handwriting recognition --> " ) 。
3. 取消註解下列行。  

``` xaml
    <Button x:Name="recognizeText" 
            Content="Recognize text"  
            Grid.Row="0" Grid.Column="0"
            Margin="10,10,10,10"
            Click="recognizeText_ClickAsync"/>
    <TextBlock x:Name="recognitionResult" 
                Text="Recognition results: "
                VerticalAlignment="Center" 
                Grid.Row="0" Grid.Column="1"
                Margin="50,0,0,0" />
```

4. 請開啟 MainPage.xaml.cs 檔案，
5. 尋找標有此步驟標題的程式碼 (「步驟 5：支援手寫辨識」)。
6. 取消註解下列行。  

- 這些是此步驟所需的全域變數。

``` csharp
    InkAnalyzer analyzerText = new InkAnalyzer();
    IReadOnlyList<InkStroke> strokesText = null;
    InkAnalysisResult resultText = null;
    IReadOnlyList<IInkAnalysisNode> words = null;
```

- 這是 **\[辨識文字\]** 按鈕的處理常式，也是我們處理辨識的地方。

``` csharp
    private async void recognizeText_ClickAsync(object sender, RoutedEventArgs e)
    {
        strokesText = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
        // Ensure an ink stroke is present.
        if (strokesText.Count > 0)
        {
            analyzerText.AddDataForStrokes(strokesText);

            resultText = await analyzerText.AnalyzeAsync();

            if (resultText.Status == InkAnalysisStatus.Updated)
            {
                words = analyzerText.AnalysisRoot.FindNodes(InkAnalysisNodeKind.InkWord);
                foreach (var word in words)
                {
                    InkAnalysisInkWord concreteWord = (InkAnalysisInkWord)word;
                    foreach (string s in concreteWord.TextAlternates)
                    {
                        recognitionResult.Text += s;
                    }
                }
            }
            analyzerText.ClearDataForAllStrokes();
        }
    }
```

7. 再次執行 App，寫下一些文字，然後再按一下 **\[辨識文字\]** 按鈕
8. 辨識結果會顯示在按鈕旁

### <a name="challenge-1-international-recognition"></a>挑戰 1︰國際性辨識
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>
<td>

Windows Ink 支援許多 Windows 所支援語言的文字辨識。 每個語言套件包括可與語言套件一起安裝的手寫辨識引擎。

藉由查詢安裝的手寫辨識引擎，以特定語言為目標。

如需國際性手寫辨識的詳細資訊，請參閱[將 Windows Ink 筆劃辨識為文字](convert-ink-to-text.md)。

</td>
</tr>
</table>

### <a name="challenge-2-dynamic-recognition"></a>挑戰 2︰動態辨識
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>
<td>

對於本教學課程，我們需要按下後可起始辨識的按鈕。 您也可以使用基本計時函式來執行動態辨識。

如需動態辨識的詳細資訊，請參閱[將 Windows Ink 筆劃辨識為文字](convert-ink-to-text.md)。

</td>
</tr>
</table>

## <a name="step-6-recognize-shapes"></a>步驟 6︰辨識圖形

好了，現在您可以轉換手寫筆記為較能讀取的文字。 但是您早上的流程圖匿名會議中的那些歪歪斜斜的塗鴉又會如何呢？

使用筆跡分析，您的 App 也可以辨識一組核心圖案，包括︰

- Circle
- 菱形
- 繪圖
- 橢圓形
- 等邊三角形
- 六邊形
- 等腰三角形
- 平行四邊形
- 五邊形
- 四邊形
- 矩形
- 直角三角形
- Square
- 梯形
- Triangle

在此步驟中，我們會使用 Windows Ink 的圖形辨識功能嘗試清除塗鴉。

對於此範例，我們不會嘗試重新繪製筆墨筆劃 (但這是可行的)。 而是，我們會在 InkCanvas 下新增標準畫布，其中我們繪製從原始筆跡衍生的對等橢圓形或多邊形物件。 然後我們刪除對應的筆墨筆劃。

### <a name="in-the-sample"></a>在範例中︰
1. 開啟 MainPage.xaml 檔案
2. 尋找以這個步驟的標題標示的程式碼 ( " \<!-- Step 6: Recognize shapes --> " ) 
3. 取消註解此行。  

``` xaml
   <Canvas x:Name="canvas" />

   And these lines.

    <Button Grid.Row="1" x:Name="recognizeShape" Click="recognizeShape_ClickAsync"
        Content="Recognize shape" 
        Margin="10,10,10,10" />
```

4. 開啟 MainPage.xaml.cs 檔案
5. 尋找標有此步驟標題的程式碼 (「步驟 6︰辨識圖形」)
6. 取消註解這些行︰  

``` csharp
    private async void recognizeShape_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private void DrawEllipse(InkAnalysisInkDrawing shape)
    {
        ...
    }

    private void DrawPolygon(InkAnalysisInkDrawing shape)
    {
        ...
    }
```

7. 執行 App、繪製一些圖形，然後按一下 **\[辨識圖形\]** 按鈕

以下是 Digital Napkin 的初步流程圖範例。

![數位 napkin 中基本流程圖的螢幕擷取畫面。](images/ink/ink-app-step6-shapereco1-small.png)

以下是圖形辨識之後的相同流程圖。

![使用者選取 [辨識] 圖形之後之流程圖的螢幕擷取畫面。](images/ink/ink-app-step6-shapereco2-small.png)


## <a name="step-7-save-and-load-ink"></a>步驟 7︰儲存和載入筆跡

現在，您已完成塗鴉，您也喜歡您所看到的，不過您可能會想在稍後做些調整？ 您可以儲存您的筆墨筆劃為筆跡序列化格式 (ISF) 檔並載入它們，每當靈感來時可以進行編輯。 

ISF 檔案是基本的 GIF 圖像，包含描述筆墨筆劃屬性和行為的其他中繼資料。 未啟用筆跡的 App 可以略過額外的中繼資料，並且仍然載入基本 GIF 圖像 (包括 Alpha 色板背景透明度)。

在此步驟中，我們將 **\[儲存\]** 和 **\[載入\]** 按鈕連接到筆跡工具列旁。

### <a name="in-the-sample"></a>在範例中︰
1. 請開啟 MainPage.xaml 檔案，
2. 尋找以這個步驟的標題標示的程式碼 ( " \<!-- Step 7: Saving and loading ink --> " ) 。
3. 取消註解下列行。 

``` xaml
    <Button x:Name="buttonSave" 
            Content="Save" 
            Click="buttonSave_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
    <Button x:Name="buttonLoad" 
            Content="Load"  
            Click="buttonLoad_ClickAsync"
            Width="100"
            Margin="5,0,0,0"/>
```

4. 請開啟 MainPage.xaml.cs 檔案，
5. 尋找標有此步驟標題的程式碼 (「步驟 7︰儲存和載入筆跡」)。
6. 取消註解下列行。  

``` csharp
    private async void buttonSave_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }

    private async void buttonLoad_ClickAsync(object sender, RoutedEventArgs e)
    {
        ...
    }
```

7. 執行 App 並繪製些東西。
8. 選取 **\[儲存\]** 按鈕，然後儲存您的繪圖。
9. 清除筆跡，或重新啟動 App。
10. 選取 **\[載入\]** 按鈕，然後開啟您儲存的筆跡檔案。

### <a name="challenge-use-the-clipboard-to-copy-and-paste-ink-strokes"></a>挑戰︰使用剪貼簿複製並貼上筆墨筆劃 
<table class="wdg-noborder">
<tr>
<td>

:::image type="icon" source="images/challenge-icon.png":::

</td>

<td>

Windows 筆跡也支援從剪貼簿複製並貼上筆墨筆劃或複製並貼到剪貼簿。

如需有關使用剪貼簿與筆跡的詳細資訊，請參閱[儲存和抓取 Windows Ink 筆劃資料](save-and-load-ink.md)。

</td>
</tr>
</table>

## <a name="summary"></a>摘要

恭喜，您已完成 **輸入： Windows 應用程式中的支援筆墨** 教學課程！ 我們向您示範在您的 Windows 應用程式中支援筆墨所需的基本程式碼，以及如何提供 Windows Ink 平臺支援的一些更豐富使用者體驗。

## <a name="related-articles"></a>相關文章

* [Windows 應用程式中的手寫筆互動與 Windows Ink](pen-and-stylus-interactions.md)

### <a name="samples"></a>範例

* [筆跡分析範例 (基本) (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-analysis-basic.zip)
* [筆跡手寫辨識範例 (C#)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-handwriting-reco.zip)
* [儲存和從筆跡序列化格式 (ISF) 檔案載入筆墨筆劃](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store.zip)
* [儲存和從剪貼簿載入筆墨筆劃](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-store-clipboard.zip)
* [筆跡工具列位置和方向範例 (基本)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [筆跡工具列位置和方向範例 (動態)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)
* [簡單的筆跡範例 (C#/C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
* [複雜的筆跡範例 (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
* [筆跡範例 (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
* [開始教學課程：在您的 Windows 應用程式中支援筆墨](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
* [著色本範例](https://github.com/Microsoft/Windows-appsample-coloringbook)
* [家庭記事本範例](https://github.com/Microsoft/Windows-appsample-familynotes)
