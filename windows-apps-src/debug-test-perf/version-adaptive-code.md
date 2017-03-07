---
author: jwmsft
title: "版本調適型程式碼"
description: "了解如何利用新的 API 並維持與先前版本的相容性"
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 3293e91e-6888-4cc3-bad3-61e5a7a7ab4e
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: f8d6c28daea2a3d5be67ad2b5da5a05a46f736cc
ms.lasthandoff: 02/08/2017

---

# <a name="version-adaptive-code-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>版本調適型程式碼：使用新的 API 並同時維持與先前版本的相容性

每個版本的 Windows 10 SDK 都新增了令人興奮、讓您想要使用的新功能。 不過，並非您所有的客戶都會同時將他們的裝置更新至最新版 Windows 10，且您會想盡可能讓您的 App 可以適用廣泛的裝置類型。 在這裡，我們會說明如何設計您的 App，讓它可以在較早版本的 Windows 10 上執行，同時讓 App 在安裝最新更新的裝置上執行時也會利用新功能。

若要確保您的 App 能支援廣泛的 Windows 10 裝置，需進行 2 個步驟。 首先，請將您的 Visual Studio 專案設定為以最新的 API 為目標。 這會影響當您編譯 App 時的情況。 接著，請執行執行階段檢查，以確保您僅呼叫正在執行您 App 之裝置上的 API。

## <a name="configure-your-visual-studio-project"></a>設定您的 Visual Studio 專案

支援多個 Windows 10 版本的第一個步驟，是在您的 Visual Studio 專案中指定「目標」**與「最低」**支援的 OS/SDK 版本。
- 目標**：Visual Studio 編譯 App 程式碼並執行所有工具的目標 SDK 版本。 於編譯期間，此 SDK 版本中所有的 API 與資源，都會在您的 App 程式碼中提供使用。
- 最低**：支援可執行您 App (且會由市集部署至) 之最早 OS 版本的 SDK 版本，以及 Visual Studio 編譯您 App 標記程式碼的目標版本。 

在執行階段期間，您的 App 將針對它所部署至的 OS 版本執行，因此如果您使用或呼叫該版本中沒有提供使用的資源或 API，您的 App 便會擲回例外狀況。 我們稍後於本文章中會示範如何使用執行階段檢查，以呼叫正確的 API。

[目標] 和 [最低] 設定指定了 OS/SDK 版本範圍的兩端。 不過，如果您在最低版本上測試您的 App，便可以確定它將可以在「最低」和「目標」之間的任何版本上執行。

> [!TIP]
> Visual Studio 不會針對 API 的相容性向您提出警告。 進行測試是您的責任，以確保您的 App 可如預期地在「最低」和「目標」的 OS 版本，以及它們之間的所有 OS 版本上執行。

當您在 Visual Studio 2015 (Update 2 或更新版本) 中建立新的專案，系統會提示您設定 App 支援的「目標」和「最低」版本。 根據預設，「目標」版本是已安裝的最高 SDK 版本，而「最低」版本是已安裝的最低 SDK 版本。 您只能從已安裝在您電腦上的 SDK 版本中挑選「目標」和「最低」版本。 

![在 Visual Studio 中設定目標 SDK](images/vs-target-sdk-1.png)

我們通常會建議您保留預設值。 不過，如果您已安裝 SDK 的預覽版本，且您正在撰寫實際執行程式碼，您應該將 Preview SDK 的「目標」版本變更為最新的正式 SDK 版本。 

若要變更 Visual Studio 中已經建立專案的「最低」和「目標」版本，請移至 [專案] -&gt; [屬性] -&gt; [應用程式] 索引標籤 -&gt; [目標預測]。

![在 Visual Studio 中變更目標 SDK](images/vs-target-sdk-2.png) 

如需參考，以下是各個 SDK 的組建編號：
- Windows 10 版本 1506：SDK 版本 10240
- Windows 10 版本 1511 (11 月更新)：SDK 版本 10586
- Windows 10 版本 1607 (年度更新版)：SDK 版本 14393

您可以從 [Windows SDK 與模擬器封存](https://developer.microsoft.com/downloads/sdk-archive)下載任何已發行版本的 SDK 。 您可以從 [Windows 測試人員](https://insider.windows.com/)網站的開發人員區段下載最新的 Windows Insider Preview SDK。

## <a name="write-adaptive-code"></a>撰寫調適型程式碼

您可以考慮撰寫調適型程式碼 (和您針對[建立調適型 UI](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml) 所進行的考量類似)。 您可以將基底 UI 設計為在最小的螢幕上執行，然後在偵測到 App 正在較大的螢幕上執行時移動或新增元素。 使用調適型程式碼，您只要撰寫在最低 OS 版本上執行的基底程式碼，並在偵測到 App 正在具有新功能的較新版本上執行時，新增選取功能。

### <a name="runtime-api-checks"></a>執行階段 API 檢查

使用程式碼條件中的 [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx) 類別，以測試您要呼叫的 API 是否存在。 此條件會在您的 App 每次執行時受到評估，但它只會在存在該 API (因而可供呼叫) 的裝置上被評估為 **true**。 這可讓您撰寫版本調適型程式碼，以建立使用特定 OS 版本提供之 API 的 App。

這裡我們要看看以 Windows Insider Preview 中的新功能為目標的特定範例。 如需使用 **ApiInformation** 的一般概觀，請參閱 [UWP App 指南](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)，以及 [Dynamically detecting features with API contracts (利用 API 協定動態偵測功能)](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) 這篇部落格文章。

> [!TIP]
> 過多執行階段 API 檢查可能會影響您 App 的效能。 我們會在這些範例中示範這些內嵌檢查。 在實際執行程式碼中，您應該執行檢查一次，並且快取結果，然後在整個 App 中使用快取的結果。 

### <a name="unsupported-scenarios"></a>不支援的案例

在大部分情況下，您可以將 App 的「最低」版本設定保持為 SDK 版本 10240，並在您的 App 於較新版本上執行時，使用執行階段檢查來啟用任何新的 API。 不過，在有某些情況下，您必須提高您 App 的最低版本，才能使用新功能。

如果您使用下列項目，便必須提高您 App 的「最低」版本：
- 需要無法在較早版本中使用之功能的新 API。 您必須將支援的最低版本增加至包括該功能的其中一個版本。 如需詳細資訊，請參閱[應用程式功能宣告](../packaging/app-capability-declarations.md)。
- 任何新增至 generic.xaml 且在先前版本中無法使用的新資源識別碼。 在執行階段所使用的 generic.xaml 版本取決於裝置執行的 OS 版本。 您無法使用執行階段 API 檢查來判斷 XAML 資源的存在。 因此，您必須只使用您 App 所支援的最低版本中可以使用的資源識別碼，否則 [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx) 會導致您的 App 在執行階段當機。

### <a name="adaptive-code-options"></a>調適型程式碼選項

建立調適型程式碼有兩種方式。 在大部分情況下，您會將您的 App 標記撰寫為在「最低」版本上執行，然後在有較新的 OS 功能時，透過 App 程式碼來使用那些功能。 不過，如果您需要更新視覺狀態中的屬性，而且 OS 版本之間僅有一個屬性或列舉值變更，您可以建立一個根據 API 是否存在而啟動的可延伸狀態觸發程序。

在這裡，我們會比較這些選項。

**App 程式碼**

使用時機：
- 建議您針對所有的調適型程式碼案例使用，除了以下針對可延伸觸發程序定義的特定情況以外。

優點：
- 避免將 API 差異性連結至標記中而導致的開發人員額外負荷/複雜性。

缺點：
- 沒有設計工具的支援。

**狀態觸發程序**

使用時機：
- 在 OS 版本之間僅有不需要邏輯變更且連接至視覺狀態的單一屬性或列舉變更時使用。

優點：
- 可讓您建立根據 API 是否存在而觸發的特定視覺狀態。
- 可使用部分設計工具支援。

缺點：
- 使用自訂觸發程序將受限於視覺狀態，無法適用複雜的調適型配置。
- 必須使用 Setters 來指定值變更，因此只可能進行簡單的變更。
- 設定及使用自訂狀態觸發程序相當複雜。

## <a name="adaptive-code-examples"></a>調適型程式碼範例

在本節中，我們會示範數個使用 Windows 10 版本 1607 (Windows Insider Preview) 中新 API 的調適型程式碼範例。

### <a name="example-1-new-enum-value"></a>範例 1：新列舉值

Windows 10 版本 1607 新增了一個新值至 [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx) 列舉：**ChatWithoutEmoji**。 這個新的輸入範圍具有與 **Chat** 輸入範圍相同的輸入行為 (拼字檢查、自動完成、自動大寫)，但是它會對應到沒有 Emoji 按鈕的觸控式鍵盤。 如果您建立自己的 Emoji 選擇器，並想要停用觸控式鍵盤中內建的 Emoji 按鈕，這會很有用。 

這個範例示範如何檢查 **ChatWithoutEmoji** 列舉值是否存在，並在存在的情況下設定 **TextBox** 的[InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx) 屬性。 如果它沒有出現在 App 所執行的系統上，**InputScope** 會改成設定為 **Chat**。 顯示的程式碼可以放置在 Page 建構子或 Page.Loaded 事件處理常式中。

> [!TIP]
> 當您檢查 API 時，請使用靜態字串，而不倚賴 .NET 語言功能，否則您的 App 可能會嘗試存取未定義的類型，且在執行階段當機。

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

在上一個範例中，我們已建立 TextBox，且所有的屬性都已在程式碼中設定。 不過，如果您有現有的 XAML，並且只需要在支援新值的系統上變更 InputScope 屬性，您可以在不變更 XAML 的情況下達成它，如下所示。 您在 XAML 中將預設值設定為 **Chat**，但是在**ChatWithoutEmoji** 值存在的情況下於程式碼中覆寫它。

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

現在我們已經有具體的範例，讓我們來看看如何套用「目標」和「最低」版本設定。

在這些範例中，您可以在 XAML 中或不含檢查的程式碼中使用 Chat 列舉值，因為它已存在於支援的「最低」OS 版本中。 

如果您在 XAML 中或不含檢查的程式碼中使用 ChatWithoutEmoji 值，它會進行編譯且不會發生錯誤，因為它已存在於「目標」OS 版本中。 它也會在使用「目標」OS 版本的系統上執行而不會發生錯誤。 不過，當 App 在使用「最低」版本 OS 的系統上執行時，它會在執行階段當機，因為 ChatWithoutEmoji 列舉值不存在。 因此，您必須只在程式碼中使用這個值，並將它包裝在執行階段 API 檢查中，那麼就只會在目前系統支援的情況下呼叫它。

### <a name="example-2-new-control"></a>範例 2：新控制項

新版本的 Windows 一般會將新的控制項加入將新功能帶至平台的 UWP API 表面。 若要利用新的控制項，請使用 [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) 方法。

Windows 10 版本 1607 導入了新的媒體控制項，稱為 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx)。 這個控制項以 [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) 類別建置，因此帶來像是輕鬆地連結背景音訊的功能，並會利用媒體堆疊中的架構改進功能。

不過，如果 App 在比 Windows 10 版本 1607 還舊的版本上執行，您必須使用[**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx) 控制項而非新的 **MediaPlayerElement** 控制項。 您可以使用 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) 方法檢查 MediaPlayerElement 控制項在執行階段是否存在，並載入適用於執行 App 之系統的任何一個控制項。

這個範例示範如何建立能根據 MediaPlayerElement 類型是否存在來使用新的 MediaPlayerElement 或舊的 MediaElement 的 App。 在這個程式碼中，您可以使用 [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx) 類別，以組件化控制項和其相關 UI 和程式碼，好讓您可以根據 OS 版本對它們進行移入和移出的切換。 另外，您也可以使用自訂的控制項，以提供更多此簡易範例用不到的功能及自訂行為。
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl` 會封裝 **MediaPlayerElement** 和數個用來逐格略過媒體的按鈕。 UserControl 可讓您將這些控制項視為單一項目，並讓在舊版系統上切換 MediaElement 變得更容易。 這個使用者控制項應該僅於存在 MediaPlayerElement 的系統上使用，因此您不必在此使用者控制項的程式碼中使用 ApiInformation 檢查。

> [!NOTE]
> 為了讓此範例簡單且聚焦，畫面速度按鈕將放置在媒體播放程式之外。 若要取得較佳的使用者體驗，您應該自訂 MediaTransportControls 以包含您的自訂按鈕。 如需詳細資訊，請參閱[自訂傳輸控制項](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls)。 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl` 會封裝 **MediaElement** 控制項。

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> `MediaElementUserControl` 的程式碼頁面只包含產生的程式碼，因此它不會顯示。

**根據 IsTypePresent 初始化控制項**

在執行階段，呼叫 **ApiInformation.IsTypePresent** 來檢查是否有 MediaPlayerElement。 如果存在，將載入 `MediaPlayerUserControl`，如果沒有，則會載入 `MediaElementUserControl`。

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> 請記住，這項檢查只會將 `mediaControl` 物件設為 `MediaPlayerUserControl` 或 `MediaElementUserControl`。 您必須在程式碼中需要判斷使用 MediaPlayerElement 或 MediaElement API 的所有位置上執行這些條件式檢查。 您應該執行檢查一次並且快取結果，然後在整個 App 中使用快取的結果。

## <a name="state-trigger-examples"></a>狀態觸發程序範例

可延伸的狀態觸發程序可讓您根據您在程式碼中檢查的條件 (在這個案例中為特定 API 的存在)，同時使用標記和程式碼觸發視覺狀態變更。 因為涉及的額外負荷，以及僅限視覺狀態的限制，我們不建議針對常見的調適型程式碼案例使用狀態觸發程序。 

您應該只有在不同 OS 版本之間有不會影響其他 UI 的小型 UI 變更時 (例如控制項上的屬性或列舉值變更)，才針對調適型程式碼使用狀態觸發程序。

### <a name="example-1-new-property"></a>範例 1：新屬性

設定可延伸狀態觸發程序的第一個步驟，就是子類別化 [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx) 類別來建立會根據 API 的存在啟用的自訂觸發程序。 這個範例示範如果存在的屬性符合 XAML 中設定的 `_isPresent` 變數時，便會啟用的觸發程序。

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

下一個步驟是在 XAML 中設定視覺狀態觸發程序，根據 API 是否存在產生兩個不同的視覺狀態。 

Windows 10 版本 1607 在 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) 類別上導入了新的屬性，稱為 [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx)，它可判斷使用者與控制項互動時，控制項是否會接受焦點。 如果您想要在使用者按一下按鈕時，將焦點保持在資料輸入的文字方塊上 (並維持讓觸控式鍵盤顯示)，這會很有用。

在這個範例中的觸發程序會檢查屬性是否存在。 如果該屬性存在，它會將 Button 上的 **AllowFocusOnInteraction** 屬性設為 **false**，如果該屬性不存在，Button 會保留它的原始狀態。 TextBox 會包含在內，讓您可以在執行程式碼時更容易地查看這個屬性的效果。

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="example-2-new-enum-value"></a>範例 2：新列舉值

這個範例會示範如何根據值是否存在，設定不同的列舉值。 它會使用自訂狀態觸發程序來達成與先前的 Chat 範例相同的結果。 在這個範例中，如果裝置執行的是 Windows 10 版本 1607，您會使用新的 ChatWithoutEmoji 輸入範圍，否則便會使用 **Chat** 輸入範圍。 使用這個觸發程序的視覺狀態會以 *if-else* 樣式設定，其中會根據是否有新的列舉值來選擇輸入範圍。

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```
## <a name="related-articles"></a>相關文章

- [UWP App 指南](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [利用 API 協定動態偵測功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)

