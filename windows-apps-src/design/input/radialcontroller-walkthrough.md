---
ms.assetid: ''
title: 支援您 Windows 應用程式中的介面撥號（和其他輪子裝置）
description: 逐步教學課程，說明如何將介面撥號（和其他輪子裝置）的支援新增至您的 Windows 應用程式。
keywords: dial, 轉盤, 弧形, 教學
ms.date: 03/11/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3972e04c59748efabd51b423f6f24fc22291a6d1
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234897"
---
# <a name="tutorial-support-the-surface-dial-and-other-wheel-devices-in-your-windows-app"></a>教學課程：支援您 Windows 應用程式中的介面撥號（和其他輪子裝置）

![Surface Dial 與 Surface Studio 的影像](images/radialcontroller/dial-pen-studio-600px.png)  
*配備 Surface Studio 和 Surface 手寫筆的 Surface Dial* (可在 [Microsoft 網上商店](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116)購買)。

本教學課程逐步解說如何自訂轉盤裝置 (例如 Surface Dial) 所支援的使用者互動體驗。 我們會使用範例應用程式的程式碼片段，這您可以從 GitHub 下載 (請參閱[範例程式碼](#sample-code))，來展示每個步驟中所討論的各種不同功能和相關 [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) API。

我們會著重於下列動作︰
* 指定哪些內建工具要顯示在 [**RadialController**](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 功能表上
* 新增自訂工具至功能表
* 控制觸覺回饋技術
* 自訂按一下互動
* 自訂旋轉互動

如需有關如何執行這些和其他功能的詳細資訊，請參閱[Windows 應用程式中的介面撥號互動](windows-wheel-interactions.md)。

## <a name="introduction"></a>簡介

Surface Dial 是次要輸入裝置，可協助使用者在一起使用主要輸入裝置，例如手寫筆、觸控板或滑鼠時，更有效率。 做為次要輸入裝置，Dial 通常搭配使用非慣用手來提供系統命令和其他更有關聯的工具和功能的存取。 

Dial 支援三個基本手勢︰ 
- 按住以顯示命令的內建功能表。
- 旋轉以反白顯示功能表項目 (如果功能表作用中)，或修改 App 中目前的動作 (如果功能表不在作用中)。
- 按一下以選取反白顯示的功能表項目 (如果功能表作用中) 或在 App 中叫用命令 (如果功能表不在作用中)。

## <a name="prerequisites"></a>先決條件

* 執行 Windows 10 Creators Update 或更新版本的電腦 (或虛擬機器)
* [Visual Studio 2019](https://developer.microsoft.com/windows/downloads)
* [Windows 10 SDK (10.0.15063.0)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* 轉盤裝置 (這次僅限 [Surface Dial](https://www.microsoft.com/store/d/Surface-Dial/925R551SKTGN?icid=Surface_Accessories_ModB_Surface_Dial_103116))
* 如果您不熟悉使用 Visual Studio 進行 Windows 應用程式開發，請先查看這些主題，再開始進行本教學課程：  
    * [開始設定](https://docs.microsoft.com/windows/uwp/get-started/get-set-up)
    * [建立 Hello, world 應用程式 (XAML)](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)

## <a name="set-up-your-devices"></a>設定您的裝置

1. 確定您的 Windows 裝置開啟。
2. 移至 **\[開始\]**，選取 **\[設定\]** > **\[裝置\]** > **\[藍牙與其他裝置\]**，然後開啟 **\[藍牙\]**。
3. 移除 Surface Dial 底部，打開電池槽，並確定裡面有兩個 AAA 電池。
4. 如果 [電池] 索引標籤出現在 Dial 下方，請將它移除。
5. 按住電池旁的小小嵌入按鈕，直到藍牙燈閃爍為止。
6. 返回您的 Windows 裝置並選取 **\[新增藍牙或其他裝置\]**。
7. 在 **\[新增裝置\]** 對話方塊中，選取 **\[藍牙\]** > **\[Surface Dial\]**。 您的 Surface Dial 現在應該連接，而且新增到 **\[藍牙與其他裝置\]** 設定頁面上 **\[滑鼠、鍵盤和手寫筆\]** 下的裝置清單。
8. 按住幾秒鐘以顯示內建功能表，測試 Dial。
9. 如果功能表未顯示在您的螢幕上（也就是撥號也會震動），請回到藍牙設定，移除裝置，然後再次嘗試連接裝置。

> [!NOTE]
> 轉盤裝置可以透過 **\[轉盤\]** 設定進行設定︰
> 1. 在 **\[開始\]** 功能表上，選取 **\[設定\]**。
> 2. 選取 [**裝置**] [  >  **滾輪**]。    
> ![轉盤設定畫面](images/radialcontroller/wheel-settings.png)

現在就可以開始本教學課程。 

## <a name="sample-code"></a>範例程式碼
在本教學課程中，我們使用範例 App 來示範所討論的概念和功能。

從 [GitHub](https://github.com/) 的 [windows-appsample-get-started-radialcontroller sample](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-RadialController) 下載此 Visual Studio 範例和原始程式碼：

1. 選取綠色的 [**複製] 或 [下載**] 按鈕。  
![複製存放庫](images/radialcontroller/wheel-clone.png)
2. 如果您有 GitHub 帳戶，您可以選擇 [**在 Visual Studio 中開啟**]，將存放庫複製到本機電腦。 
3. 如果您沒有 GitHub 帳戶，或只是想要專案的本機複本，請選擇 [**下載 ZIP** ] （您必須定期回來查看以下載最新更新）。

> [!IMPORTANT]
> 範例中大部分的程式碼已標示註解。當我們逐步執行本主題中的每個步驟時，您會被要求取消註解各個不同區段的程式碼。 在 Visual studio 中，只要反白顯示程式碼行，並按 CTRL-K 然後按 CTRL-U。

## <a name="components-that-support-wheel-functionality"></a>支援轉盤功能的元件

這些物件可為 Windows 應用程式提供大量的滾輪裝置體驗。

| 元件 | 描述 |
| --- | --- |
| [**RadialController** 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)及相關 | 表示轉盤輸入裝置或配件，例如 Surface Dial。 |
| [**IRadialControllerConfigurationInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerconfigurationinterop)  / [ **IRadialControllerInterop**](https://docs.microsoft.com/previous-versions/windows/desktop/api/radialcontrollerinterop/nn-radialcontrollerinterop-iradialcontrollerinterop)<br/>我們在此不涵蓋此項功能，如需詳細資訊，請參閱 [Windows 的傳統桌面範例](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)。 | 啟用與 Windows 應用程式的互通性。 |

## <a name="step-1-run-the-sample"></a>步驟 1：執行範例

在您下載 RadialController 範例 App 之後，請確認它是否執行︰
1. 在 Visual Studio 中開啟範例專案。
2. 設定 **\[方案平台\]** 下拉式清單為非 ARM 選項。
3. 按下 F5 進行編譯、部署和執行。 

> [!NOTE]
> 或者，您可以選取 [ **Debug**]  >  [**開始調試**] 功能表項目，或選取 [**本機電腦**執行] 按鈕，如下所示： ![ Visual Studio 組建專案] 按鈕](images/radialcontroller/wheel-vsrun.png)

應用程式視窗隨即開啟，並在啟動顯示畫面出現幾秒後，您會看到這個初始畫面。

![空的 App](images/radialcontroller/wheel-app-step1-empty.png)

好了，我們現在在本教學課程的其餘部分將使用基本 Windows 應用程式。 我們將在下列步驟中新增我們的 **RadialController** 功能。

## <a name="step-2-basic-radialcontroller-functionality"></a>步驟 2︰基本 RadialController 功能

當應用程式執行並在前景中，按住介面撥號以顯示 [ **RadialController** ] 功能表。

我們尚未完成自訂我們的 App，所以功能表包含預設的內容工具組。 

這些影像顯示兩個不同的預設功能表。 還有許多其他工具，包括 Windows 桌面作用中時的基本系統工具，並且前景中沒有任何 App，當 InkToolbar 出現時的其它筆跡工具，還有當您在使用地圖 App 時的對應工具。

| RadialController 功能表 (預設)  | RadialController 功能表 (預設播放媒體)  |
|---|---|
| ![預設 RadialController 功能表](images/radialcontroller/wheel-app-step2-basic-default.png) | ![含有音樂的預設 RadialController 功能表](images/radialcontroller/wheel-app-step2-basic-withmusic.png) |

現在，我們將開始一些基本自訂。

## <a name="step-3-add-controls-for-wheel-input"></a>步驟 3︰新增轉盤輸入的控制項

首先，加入我們的 App 的 UI：

1. 請開啟 MainPage_Basic.xaml 檔案，
2. 尋找以此步驟的標題標示的程式碼（「 \< !--步驟3：為滾輪輸入新增控制項-->」）。
3. 取消註解下列行。

    ```xaml
    <Button x:Name="InitializeSampleButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Initialize sample" />
    <ToggleButton x:Name="AddRemoveToggleButton"
                    HorizontalAlignment="Center" 
                    Margin="10" 
                    Content="Remove Item"
                    IsChecked="True" 
                    IsEnabled="False"/>
    <Button x:Name="ResetControllerButton" 
            HorizontalAlignment="Center" 
            Margin="10" 
            Content="Reset RadialController menu" 
            IsEnabled="False"/>
    <Slider x:Name="RotationSlider" Minimum="0" Maximum="10"
            Width="300"
            HorizontalAlignment="Center"/>
    <TextBlock Text="{Binding ElementName=RotationSlider, Mode=OneWay, Path=Value}"
                Margin="0,0,0,20"
                HorizontalAlignment="Center"/>
    <ToggleSwitch x:Name="ClickToggle"
                    MinWidth="0" 
                    Margin="0,0,0,20"
                    HorizontalAlignment="center"/>
    ```
此時，只有啟用 **\[初始化範例\]** 按鈕、滑桿以及切換開關。 其他按鈕會在稍後步驟中使用，來新增或移除讓您存取滑桿和切換開關的 **\[RadialController\]** 功能表項目。

![基本範例 App UI](images/radialcontroller/wheel-app-step3-basicui.png)

## <a name="step-4-customize-the-basic-radialcontroller-menu"></a>步驟 4︰自訂基本 RadialController 功能表

現在我們來新增所需的程式碼，以啟用 **RadialController** 的控制項存取權。

1. 請開啟 MainPage_Basic.xaml.cs 檔案，
2. 尋找標有此步驟標題的程式碼 (「步驟 4︰基本 RadialController 功能表自訂」)。
3. 取消註解下列行：
    - [Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input) 和 [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) 類型參考用於後續步驟的功能︰  
    
        ```csharp
        // Using directives for RadialController functionality.
        using Windows.UI.Input;
        ```

    - 這些全域物件 ([RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller)、[RadialControllerConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration)、[RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)) 會在我們的 App 中使用。
    
        ```csharp
        private RadialController radialController;
        private RadialControllerConfiguration radialControllerConfig;
        private RadialControllerMenuItem radialControllerMenuItem;
        ```

    - 以下我們指定 **Click** 處理常式給按鈕，可啟用我們的控制項並初始化我們的自訂 **RadialController** 功能表項目。

        ```csharp
        InitializeSampleButton.Click += (sender, args) =>
        { InitializeSample(sender, args); };
        ``` 

    - 接下來，我們初始化我們的 [RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 物件並設定 [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 和 [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 事件的處理常式。

        ```csharp
        // Set up the app UI and RadialController.
        private void InitializeSample(object sender, RoutedEventArgs e)
        {
            ResetControllerButton.IsEnabled = true;
            AddRemoveToggleButton.IsEnabled = true;

            ResetControllerButton.Click += (resetsender, args) =>
            { ResetController(resetsender, args); };
            AddRemoveToggleButton.Click += (togglesender, args) =>
            { AddRemoveItem(togglesender, args); };

            InitializeController(sender, e);
        }
        ```

    - 以下，我們初始化我們的自訂 RadialController 功能表項目。 我們使用 [CreateForCurrentView](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.CreateForCurrentView) 取得[RadialController](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller) 物件的參照，我們使用 [RotationResolutionInDegrees](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationResolutionInDegrees) 屬性將旋轉敏感度設定為「1」，然後我們使用 [CreateFromFontGlyph](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem.CreateFromFontGlyph) 建立我們的 [RadialControllerMenuItem](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollermenuitem)，我們新增功能表項目到 **RadialController** 功能表項目集合，最後我們使用 [SetDefaultMenuItems](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontrollerconfiguration.setdefaultmenuitems) 清除預設功能表項目，並只保留我們的自訂工具。 

        ```csharp
        // Configure RadialController menu and custom tool.
        private void InitializeController(object sender, RoutedEventArgs args)
        {
            // Create a reference to the RadialController.
            radialController = RadialController.CreateForCurrentView();
            // Set rotation resolution to 1 degree of sensitivity.
            radialController.RotationResolutionInDegrees = 1;

            // Create the custom menu items.
            // Here, we use a font glyph for our custom tool.
            radialControllerMenuItem =
                RadialControllerMenuItem.CreateFromFontGlyph("SampleTool", "\xE1E3", "Segoe MDL2 Assets");

            // Add the item to the RadialController menu.
            radialController.Menu.Items.Add(radialControllerMenuItem);

            // Remove built-in tools to declutter the menu.
            // NOTE: The Surface Dial menu must have at least one menu item. 
            // If all built-in tools are removed before you add a custom 
            // tool, the default tools are restored and your tool is appended 
            // to the default collection.
            radialControllerConfig =
                RadialControllerConfiguration.GetForCurrentView();
            radialControllerConfig.SetDefaultMenuItems(
                new RadialControllerSystemMenuItemKind[] { });

            // Declare input handlers for the RadialController.
            // NOTE: These events are only fired when a custom tool is active.
            radialController.ButtonClicked += (clicksender, clickargs) =>
            { RadialController_ButtonClicked(clicksender, clickargs); };
            radialController.RotationChanged += (rotationsender, rotationargs) =>
            { RadialController_RotationChanged(rotationsender, rotationargs); };
        }

        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees >= RotationSlider.Maximum)
            {
                RotationSlider.Value = RotationSlider.Maximum;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < RotationSlider.Minimum)
            {
                RotationSlider.Value = RotationSlider.Minimum;
            }
            else
            {
                RotationSlider.Value += args.RotationDeltaInDegrees;
            }
        }

        // Connect wheel device click to toggle switch control.
        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ClickToggle.IsOn = !ClickToggle.IsOn;
        }
        ```
4. 現在，再次執行 App。
5. 選取 **\[初始化星形控制器\]** 按鈕。  
6. App 在前景中，按住 \[Surface Dial\] 以顯示功能表。 請注意，所有預設工具已經移除 (使用**RadialControllerConfiguration.SetDefaultMenuItems**方法)，只保留自訂工具。 以下是含有我們的自訂工具的功能表。 

| RadialController 功能表 (自訂)  | 
|---|
| ![自訂 RadialController 功能表](images/radialcontroller/wheel-app-step3-custom.png) |

7. 選取自訂工具，並試試現在透過 Surface Dial 支援的互動︰
    * 旋轉動作會移動滑桿。 
    * 按一下將設定開關的切換。

好了，我們來連結這些按鈕。

## <a name="step-5-configure-menu-at-runtime"></a>步驟 5︰在執行階段設定功能表

在此步驟中，我們將 **\[新增/移除項目\]** 和 **\[重設 RadialController 功能表\]** 按鈕連結，以顯示動態自訂功能表的方式。

1. 請開啟 MainPage_Basic.xaml.cs 檔案，
2. 尋找標有此步驟標題的程式碼 (「步驟 5︰在執行階段設定功能表」)。
3. 以下列方法取消註解程式碼並再次執行 App，但不選取任何按鈕 (保留下一步使用)。  

    ``` csharp
    // Add or remove the custom tool.
    private void AddRemoveItem(object sender, RoutedEventArgs args)
    {
        if (AddRemoveToggleButton?.IsChecked == true)
        {
            AddRemoveToggleButton.Content = "Remove item";
            if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Add(radialControllerMenuItem);
            }
        }
        else if (AddRemoveToggleButton?.IsChecked == false)
        {
            AddRemoveToggleButton.Content = "Add item";
            if (radialController.Menu.Items.Contains(radialControllerMenuItem))
            {
                radialController.Menu.Items.Remove(radialControllerMenuItem);
                // Attempts to select and activate the previously selected tool.
                // NOTE: Does not differentiate between built-in and custom tools.
                radialController.Menu.TrySelectPreviouslySelectedMenuItem();
            }
        }
    }

    // Reset the RadialController to initial state.
    private void ResetController(object sender, RoutedEventArgs arg)
    {
        if (!radialController.Menu.Items.Contains(radialControllerMenuItem))
        {
            radialController.Menu.Items.Add(radialControllerMenuItem);
        }
        AddRemoveToggleButton.Content = "Remove item";
        AddRemoveToggleButton.IsChecked = true;
        radialControllerConfig.SetDefaultMenuItems(
            new RadialControllerSystemMenuItemKind[] { });
    }
    ```
4. 選取 **\[移除項目\]** 按鈕，然後按住 \[Dial\] 以再次顯示功能表。

    請注意，功能表現在包含工具的預設集合。 回想步驟 3，設定我們的自訂功能表時，我們已移除所有預設工具，並且只新增我們的自訂工具。 我們也注意到，當功能表設定為空白集合時，會恢復目前內容的預設項目  (我們在移除預設工具之前新增了我們的自訂工具)。

5. 選取 **\[新增項目\]** 按鈕，然後按住 \[Dial\]。

    請注意，功能表現在包含預設工具集合和我們的自訂工具。

6. 選取 **\[重設 RadialController 功能表\]** 按鈕，然後按住 \[Dial\]。

    請注意，功能表回復其原始狀態。

## <a name="step-6-customize-the-device-haptics"></a>步驟 6︰自訂裝置觸覺回饋技術
Surface Dial 及其他轉盤裝置，可以提供使用者觸覺回饋技術對應目前的互動 (根據按一下或旋轉)。

在此步驟中，我們向您展示您可以如何透過關聯滑桿和切換開關控制項來自訂觸覺回饋技術，並使用它們來動態指定觸覺回饋的行為。 例如，對於要啟用的觸覺回饋技術，切換開關必須設定為開啟，同時滑桿值指定點按回饋的重複頻率。 

> [!NOTE]
> 使用者可以在 [**設定**] [裝置] [  >   **Devices**  >  **輪子**] 頁面中停用 Haptic 意見反應。

1. 請開啟 App.xaml.cs 檔案，
2. 尋找標有此步驟標題的程式碼 (「步驟 6︰自訂裝置觸覺回饋技術」)。
3. 為第一行和第三行加註註解（「MainPage_Basic」和「MainPage」)，並取消註解第二個 (「MainPage_Haptics」)。  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
4. 請開啟 MainPage_Haptics.xaml 檔案，
5. 尋找以此步驟的標題標示的程式碼（「 \< !--步驟6：自訂裝置 haptics-->」）。
6. 取消註解下列行。 (此 UI 程式碼只指出目前裝置所支援的觸覺回饋技術功能)。    

    ```xaml
    <StackPanel x:Name="HapticsStack" 
                Orientation="Vertical" 
                HorizontalAlignment="Center" 
                BorderBrush="Gray" 
                BorderThickness="1">
        <TextBlock Padding="10" 
                    Text="Supported haptics properties:" />
        <CheckBox x:Name="CBDefault" 
                    Content="Default" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsChecked="True" />
        <CheckBox x:Name="CBIntensity" 
                    Content="Intensity" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayCount" 
                    Content="Play count" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPlayDuration" 
                    Content="Play duration" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBReplayPauseInterval" 
                    Content="Replay/pause interval" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBBuzzContinuous" 
                    Content="Buzz continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBClick" 
                    Content="Click" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBPress" 
                    Content="Press" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRelease" 
                    Content="Release" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
        <CheckBox x:Name="CBRumbleContinuous" 
                    Content="Rumble continuous" 
                    Padding="10" 
                    IsEnabled="False" 
                    IsThreeState="True" 
                    IsChecked="{x:Null}" />
    </StackPanel>
    ```
7. 請開啟 MainPage_Haptics.xaml.cs 檔案
8. 尋找標有此步驟標題的程式碼 (「步驟 6：觸覺回饋技術自訂」)。
9. 取消註解下列行：  

    - [Windows.Devices.Haptics](https://docs.microsoft.com/uwp/api/windows.devices.haptics) 類型參考適用於後續步驟中的功能。  
    
        ```csharp
        using Windows.Devices.Haptics;
        ```

    - 在此，我們指定 [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 事件的處理常式，該事件是在選取了我們的自訂 **RadialController** 功能表項目時觸發。

        ```csharp
        radialController.ControlAcquired += (rc_sender, args) =>
        { RadialController_ControlAcquired(rc_sender, args); };
        ``` 

    - 接下來，我們定義 [ControlAcquired](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ControlAcquired) 處理常式，我們停用預設觸覺回饋技術並初始化我們觸覺回饋技術 UI。

        ```csharp
        private void RadialController_ControlAcquired(
            RadialController rc_sender,
            RadialControllerControlAcquiredEventArgs args)
        {
            // Turn off default haptic feedback.
            radialController.UseAutomaticHapticFeedback = false;

            SimpleHapticsController hapticsController =
                args.SimpleHapticsController;

            // Enumerate haptic support.
            IReadOnlyCollection<SimpleHapticsControllerFeedback> supportedFeedback =
                hapticsController.SupportedFeedback;

            foreach (SimpleHapticsControllerFeedback feedback in supportedFeedback)
            {
                if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.BuzzContinuous)
                {
                    CBBuzzContinuous.IsEnabled = true;
                    CBBuzzContinuous.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Click)
                {
                    CBClick.IsEnabled = true;
                    CBClick.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Press)
                {
                    CBPress.IsEnabled = true;
                    CBPress.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.Release)
                {
                    CBRelease.IsEnabled = true;
                    CBRelease.IsChecked = true;
                }
                else if (feedback.Waveform == KnownSimpleHapticsControllerWaveforms.RumbleContinuous)
                {
                    CBRumbleContinuous.IsEnabled = true;
                    CBRumbleContinuous.IsChecked = true;
                }
            }

            if (hapticsController?.IsIntensitySupported == true)
            {
                CBIntensity.IsEnabled = true;
                CBIntensity.IsChecked = true;
            }
            if (hapticsController?.IsPlayCountSupported == true)
            {
                CBPlayCount.IsEnabled = true;
                CBPlayCount.IsChecked = true;
            }
            if (hapticsController?.IsPlayDurationSupported == true)
            {
                CBPlayDuration.IsEnabled = true;
                CBPlayDuration.IsChecked = true;
            }
            if (hapticsController?.IsReplayPauseIntervalSupported == true)
            {
                CBReplayPauseInterval.IsEnabled = true;
                CBReplayPauseInterval.IsChecked = true;
            }
        }
        ```

    - 在我們的 [RotationChanged](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.RotationChanged) 和 [ButtonClicked](https://docs.microsoft.com/uwp/api/windows.ui.input.radialcontroller.ButtonClicked) 事件處理常式中，我們將對應的滑桿和切換按鈕控制項連接到我們的自訂觸覺回饋技術。 

        ```csharp
        // Connect wheel device rotation to slider control.
        private void RadialController_RotationChanged(
            object sender, RadialControllerRotationChangedEventArgs args)
        {
            ...
            if (ClickToggle.IsOn && 
                (RotationSlider.Value > RotationSlider.Minimum) && 
                (RotationSlider.Value < RotationSlider.Maximum))
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.BuzzContinuous);
                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedback(waveform);
                }
            }
        }

        private void RadialController_ButtonClicked(
            object sender, RadialControllerButtonClickedEventArgs args)
        {
            ...

            if (RotationSlider?.Value > 0)
            {
                SimpleHapticsControllerFeedback waveform = 
                    FindWaveform(args.SimpleHapticsController, 
                    KnownSimpleHapticsControllerWaveforms.Click);

                if (waveform != null)
                {
                    args.SimpleHapticsController.SendHapticFeedbackForPlayCount(
                        waveform, 1.0, 
                        (int)RotationSlider.Value, 
                        TimeSpan.Parse("1"));
                }
            }
        }
        ```
    - 最後，我們取得所要求觸覺回饋的 **[Waveform](https://docs.microsoft.com/uwp/api/windows.devices.haptics.simplehapticscontrollerfeedback.Waveform)** (如有支援)。 

        ```csharp
        // Get the requested waveform.
        private SimpleHapticsControllerFeedback FindWaveform(
            SimpleHapticsController hapticsController,
            ushort waveform)
        {
            foreach (var hapticInfo in hapticsController.SupportedFeedback)
            {
                if (hapticInfo.Waveform == waveform)
                {
                    return hapticInfo;
                }
            }
            return null;
        }
        ```

現在，再次執行 App，藉由變更滑桿值和切換開關狀態，嘗試自訂觸覺回饋。

## <a name="step-7-define-on-screen-interactions-for-surface-studio-and-similar-devices"></a>步驟 7︰定義 Surface Studio 和類似裝置的螢幕互動
與 Surface Studio 配對後，Surface Dial 可以提供更獨特的使用者體驗。 

除了上述預設的長按功能表體驗之外，Surface Dial 也可以直接放置在 Surface Studio 的螢幕上。 這會產生特殊的「螢幕上」功能表。 

系統會偵測 Surface Dial 的接觸位置和界限，處理裝置的遮蔽範圍，並沿著 Dial 外圍迴旋顯示較大版本的功能表。 您的應用程式也可以使用這個相同的資訊，針對裝置是否存在及其預期的使用方式 (例如使用者的手與手臂的放置位置) 打造 UI。 

本教學課程隨附的範例，包括示範這些功能的一些稍微更複雜的範例。

若要了解其動作 (您需要 Surface Studio)︰

1. 在 Surface Studio 裝置上下載範例 (已安裝 Visual Studio)
2. 在 Visual Studio 中開啟範例
3. 開啟 App.xaml.cs 檔案
4. 尋找標有此步驟標題的程式碼 (「步驟 7︰定義 Surface Studio 和類似裝置的螢幕互動」)
5. 為第一行和第二行加註註解 (「MainPage_Basic」和「MainPage_Haptics」)，並取消註解第三個 (「MainPage」)。  

    ``` csharp
    rootFrame.Navigate(typeof(MainPage_Basic), e.Arguments);
    rootFrame.Navigate(typeof(MainPage_Haptics), e.Arguments);
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
    ```
6. 執行 App，並將 Surface Dial 置於兩個控制項地區的每一個中，在這當中輪替。    
![螢幕上 RadialController](images/radialcontroller/wheel-app-step5-onscreen2.png) 

    以下是執行此範例的影片︰  

    <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="600" height="400" allowFullScreen frameBorder="0"></iframe>  

## <a name="summary"></a>摘要

恭喜您，您已完成*開始使用教學課程：在您的 Windows 應用程式中支援介面撥號（和其他輪子裝置）*！ 我們向您示範了在 Windows 應用程式中支援輪子裝置所需的基本程式碼，以及如何提供**RadialController** api 支援的一些更豐富的使用者體驗。

## <a name="related-articles"></a>相關文章

[Surface Dial 互動](windows-wheel-interactions.md)

### <a name="api-reference"></a>API 參考資料

- [**RadialController**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialController)
- [**RadialControllerButtonClickedEventArgs**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [**RadialControllerConfiguration**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerConfiguration) 
- [**RadialControllerControlAcquiredEventArgs**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [**RadialControllerMenu**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenu) 
- [**RadialControllerMenuItem**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuItem) 
- [**RadialControllerRotationChangedEventArgs**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [**RadialControllerScreenContact**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContact) 
- [**RadialControllerScreenContactContinuedEventArgs**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [**RadialControllerScreenContactStartedEventArgs**類別](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [**RadialControllerMenuKnownIcon**列舉](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [**RadialControllerSystemMenuItemKind**列舉](https://docs.microsoft.com/uwp/api/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>範例

#### <a name="topic-samples"></a>主題範例

[RadialController 自訂](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-radialcontroller-customization.zip)

#### <a name="other-samples"></a>其他範例
[著色書籍範例](https://github.com/Microsoft/Windows-appsample-coloringbook)

[通用 Windows 平台範例 (C# 和 C++)](https://github.com/Microsoft/Windows-universal-samples/tree/b78d95134ce2d57c848e0a8dc339fc362748fb9c/Samples/RadialController)

[Windows 傳統桌面範例](https://github.com/Microsoft/Windows-classic-samples/tree/master/Samples/RadialController)
