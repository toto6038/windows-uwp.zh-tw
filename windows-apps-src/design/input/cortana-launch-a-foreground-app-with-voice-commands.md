---
title: 使用語音命令透過 Cortana 啟用前景應用程式-Cortana UWP 設計與開發
description: 使用語音命令將您的應用程式啟用到前景，並在應用程式內執行動作或命令。
ms.assetid: e4bf3714-6f62-466f-9e7c-3b03ee86a117
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 4b17a93d950d48209637cedf89bb49759ba0cf1f
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824322"
---
# <a name="activate-a-foreground-app-with-voice-commands-through-cortana"></a>透過 Cortana 語音命令啟用前景應用程式

>[!WARNING]
> Windows 10 2020 版更新 (2004 版 codename "20H1" ) ，不再支援此功能。
>
> 查看 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

除了在 **cortana** 內使用語音命令來存取系統功能以外，您也可以使用應用程式的功能來擴充 **cortana** 。 使用語音命令可讓您的應用程式啟動至前景，以及在應用程式內執行的動作或命令。

> [!NOTE]
> **重要 API**
>
> - [**ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和屬性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

當應用程式在前景處理 voice 命令時，會取得焦點，並關閉 Cortana。 如果您想要的話，可以啟用應用程式，並以背景工作的方式執行命令。 在此情況下，Cortana 會保留焦點，而您的應用程式會透過 **cortana** 畫布和 **cortana** 語音傳回所有意見反應和結果。

需要額外內容或使用者輸入的語音命令 (例如，將訊息傳送至特定連絡人) 最適合在前景應用程式中處理，而基本命令 (例如列出即將進行的行程) 可透過背景應用程式在 **Cortana** 中處理。

如果您想要使用語音命令在背景中啟用應用程式，請參閱 [使用語音命令在 Cortana 中啟動背景應用程式](cortana-launch-a-background-app-with-voice-commands.md)。

> [!NOTE]
> 語音命令是具有特定意圖的單一語句，定義于語音命令定義 (VCD) 檔，透過 **Cortana** 導向已安裝的應用程式。
>
> VCD 檔案會定義一或多個語音命令，每個命令都有獨特的意圖。
>
> 語音命令定義可能會有不同的複雜度。 它們可以支援從單一、受限的語句到更具彈性的自然語言語句集合的任何作業，而且全都代表相同的意圖。

為了示範前景應用程式功能，我們將使用「 [Cortana 語音命令」範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)中名為 [**艾德公司**] 的旅程規劃和管理應用程式。

若要在不使用 **Cortana** 的情況下建立新的「**艾德公司**」，使用者會啟動應用程式並流覽至新的「**旅程**」頁面。 若要查看現有的行程，使用者會啟動應用程式、流覽至即將進行的 **旅程** 頁面，然後選取行程。

使用透過 **Cortana** 的語音命令，使用者可以改為使用「艾德作品新增旅程」或「在艾德作品上新增旅程」來啟動應用程式，並流覽至新的 [ **旅程** ] 頁面。 接著，說「艾德公司，顯示我的倫敦旅程」將會啟動應用程式，並流覽至 [ **旅程** 詳細資料] 頁面，如下所示。

:::image type="content" source="images/cortana/cortana-foreground-with-adventureworks.png" alt-text="Cortana 啟動前景應用程式的螢幕擷取畫面":::

以下是新增語音命令功能，以及使用語音或鍵盤輸入將 Cortana 與您的應用程式整合的基本步驟：

1. 建立 VCD 檔案。 這是一份 XML 檔，可定義使用者在啟用您的應用程式時，可以用來起始動作或叫用命令的所有語音命令。 請參閱 [**VCD 元素和屬性-1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)。
2. 當應用程式啟動時，在 VCD 檔案中註冊命令集。
3. 處理啟用語音命令、在應用程式內流覽，以及執行命令。

> [!TIP]
> **先決條件**
>
> 如果您是開發通用 Windows 平台 (UWP) App 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。
>
> - [建立您的第一個應用程式](../../get-started/your-first-app.md)
> - 請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)，以了解事件相關資訊
>
> **使用者經驗指導方針**
>
> 請參閱 [cortana 設計指導方針](cortana-design-guidelines.md)  ，以瞭解如何整合您的應用程式與 **Cortana** 和 [語音互動](speech-interactions.md) ，以取得設計實用且具備語音功能的應用程式的實用秘訣。

## <a name="create-a-new-solution-with-project-in-visual-studio"></a>使用 Visual Studio 中的專案建立新的方案

1. 啟動 Microsoft Visual Studio 2015。

    Visual Studio 2015 的 [開始] 頁面隨即出現。

2. 在 [檔案] 功能表上，選取 [新增] > [專案]。

    [ **新增專案** ] 對話方塊隨即出現。 對話方塊的左窗格可讓您選取要顯示的範本類型。

3. 在左窗格中，展開 [ **已安裝的 > 範本] > Visual C \# > Windows**，然後挑選 **通用** 範本群組。 對話方塊的中央窗格會顯示適用于通用 Windows 平臺 (UWP) 應用程式的專案範本清單。
4. 在中央窗格中，選取 **(通用 Windows) 範本的空白應用程式** 。

    **空白應用** 程式範本會建立可編譯和執行的最基本 UWP 應用程式，但不包含任何使用者介面控制項或資料。 您可以在本教學課程的過程中，將控制項新增至應用程式。

5. 在 [ **名稱** ] 文字方塊中，輸入您的專案名稱。 在此範例中，我們使用 "AdventureWorks"。
6. 按一下 [確定]  以建立專案。

    Microsoft Visual Studio 會建立您的專案，並將其顯示在 **Solution Explorer** 中。

## <a name="add-image-assets-to-project-and-specify-them-in-the-app-manifest"></a>將影像資產新增至專案，並在應用程式資訊清單中指定它們

UWP 應用程式可以根據特定設定和裝置功能，自動選取最適當的映射 (高對比、有效圖元、地區設定等) 。 您只需要提供影像，並確保您在應用程式專案內針對不同的資源版本使用適當的命名慣例和資料夾組織。 如果您未提供建議的資源版本，可存取性、當地語系化和影像品質可能會受到影響，視使用者的喜好設定、能力、裝置類型和位置而定。

如需高對比和縮放比例影像資源的詳細資訊，請參閱 [磚和圖示資產的指導方針](../../app-resources/images-tailored-for-scale-theme-contrast.md)。

您可以使用限定詞來命名資源。 資源限定詞是資料夾和檔案名修飾詞，用來識別應該使用特定版本資源的內容。

標準命名慣例為 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` 。 例如，您 `images/logo.scale-100_contrast-white.png` 可以在程式碼中，只使用根資料夾和檔案名：來參考 `images/logo.png` 。 請參閱 [如何使用限定詞命名資源](/previous-versions/windows/apps/hh965324(v=win.10))。

建議您將字串資源檔上的預設語言標示 (例如 `en-US\resources.resw`) 和影像上的預設縮放比例 (例如 `logo.scale-100.png`) ，即使您目前未規劃提供當地語系化或多個解析資源。 不過，我們至少建議您提供100、200和400規模調整因素的資產。

> [!IMPORTANT]
> **Cortana** 畫布標題區域中所使用的應用程式圖示是 "package.appxmanifest" 檔案中指定的 Square44x44Logo 圖示。

## <a name="create-a-vcd-file"></a>建立 VCD 檔案

1. 在 Visual Studio 中，以滑鼠右鍵按一下您的主要專案名稱，然後選取 [ **加入 > 新專案**]。 加入 **XML** 檔案。
2. 輸入此範例中的 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 檔案名稱 ([AdventureWorksCommands.xml] ) ，然後按一下 [新增]。
3. 在 [ **方案瀏覽器**] 中，選取 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 檔案。
4. 在 [ **屬性** ] 視窗中，將 [ **組建動作** ] 設定為 [ **內容**]，然後將 [ **複製到輸出目錄** ] 設定為 [ **更新時複製**]

## <a name="edit-the-vcd-file"></a>編輯 VCD 檔案

加入 **VoiceCommands** 元素，其 **xmlns** 屬性指向 `https://schemas.microsoft.com/voicecommands/1.2` 。

1. 針對您的應用程式所支援的每種語言，建立 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 專案，其中包含應用程式所支援的語音命令。

   您可以宣告多個 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 專案，每個專案都有不同的 [**xml： lang**](/previous-versions/windows/dn722331(v=win.10)) 屬性，讓您的應用程式在不同市場中使用。 例如，美國的應用程式可能會有英文的 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 和西班牙文的 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 。

   > [!CAUTION]
   > 若要啟用應用程式並使用 voice 命令起始動作，應用程式必須註冊包含 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 的 VCD 檔案，該檔案的語言符合使用者為其裝置選取的語音語言。 語音語言位於 [設定] **> 系統 > 語音 > 語音語言**。

2. 針對您想要支援的每個命令新增 **命令** 元素。 在 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)檔案中宣告的每個 **命令** 都必須包含下列資訊：

   - 應用程式在執行時間用來識別語音命令的 **AppName** 屬性。
   - **範例** 元素，其中包含描述使用者可以如何叫用命令的片語。 **Cortana** 會在使用者說「我可以說什麼？」、「說明」時顯示此範例，或他們可以 **看到更多**。
   - **ListenFor** 元素，其中包含您的應用程式可辨識為命令的單字或片語。 每個 **ListenFor** 專案都可以包含一個或多個 **PhraseList** 專案的參考，其中包含與命令相關的特定字組。
  
> [!NOTE]
> 無法以程式設計方式修改 **ListenFor** 元素。 不過，您可以透過程式設計的方式修改與 **ListenFor** 專案相關聯的 **PhraseList** 元素。 應用程式應該根據使用者使用應用程式時所產生的資料集，在執行時間修改 **PhraseList** 的內容。 請參閱 [動態修改 CORTANA VCD 片語清單](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)。

**意見** 元素，其中包含 **Cortana** 在應用程式啟動時顯示和說話的文字。

**導覽** 元素表示語音命令會將應用程式啟用至前景。 在此範例中， `showTripToDestination` 命令是前景工作。

**VoiceCommandService** 元素表示 voice 命令會在背景中啟動應用程式。 這個元素的 **Target** 屬性值應該符合 package.appxmanifest 檔案中 [**uap： AppService**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice)專案的 **Name** 屬性值。 在此範例中， `whenIsTripToDestination` 和 `cancelTripToDestination` 命令是背景工作，會將應用程式服務的名稱指定為 "AdventureWorksVoiceCommandService"。

如需詳細資訊，請參閱 [**VCD 元素和屬性 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 版參考。

以下是一些 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 檔案的一部分，可定義「 **艾德作品** 」應用程式的 en-us 語音命令。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <a name="install-the-vcd-commands"></a>安裝 VCD 命令

您的應用程式必須執行一次，才能安裝 VCD。

> [!NOTE]
> 語音命令資料不會在應用程式安裝之間保留。 若要確保應用程式的語音命令資料保持不變，請考慮在每次啟動或啟動應用程式時，將您的 VCD 檔案初始化，或維護一個指出是否目前已安裝 VCD 的設定。

在 "app.xaml.cs" 檔案中：

1. 加入以下 using 指示詞：

    ```csharp
    using Windows.Storage;
    ```

1. 使用 async 修飾詞標記 ">onlaunched" 方法。  

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    ```

1. 在 [**>onlaunched**](/uwp/api/Windows.UI.Xaml.Application)處理常式中呼叫 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) ，以註冊系統應辨識的語音命令。

  在「艾德作品」範例中，我們會先定義 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 物件。

  然後再呼叫 [**GetFileAsync**](/uwp/api/Windows.Storage.StorageFolder) ，使用我們的 "AdventureWorksCommands.xml" 檔案將它初始化。

  這個 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 物件接著會傳遞至 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)。

```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
  await Package.Current.InstalledLocation.GetFileAsync(
  @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <a name="handle-activation-and-execute-voice-commands"></a>處理啟用和執行語音命令

指定您的應用程式在啟動至少一次之後，如何回應後續的語音命令啟用 (，並已) 安裝語音命令集。

1. 確認您的應用程式已由語音命令啟用。

    覆寫 [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application) 事件並檢查是否有 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)。[**種類**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 為 [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。

2. 判斷命令的名稱以及所說的內容。

    從 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)取得 [**VoiceCommandActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)物件的參考，並查詢 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)物件的 [**Result**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)屬性。

    若要判斷使用者所說的內容，請在 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)字典中檢查 [**文字**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)的值或辨識的片語的語義屬性。

3. 在您的應用程式中採取適當的動作，例如流覽至所需的頁面。

在此範例中，我們回頭看看步驟3：編輯 VCD 檔案中的 VCD。

一旦取得 voice 命令的語音辨識結果，我們就會從 [**RulePath**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 陣列中的第一個值取得命令名稱。 當 VCD 檔案定義了多個可能的 voice 命令時，我們需要將值與 VCD 中的命令名稱進行比較，並採取適當的動作。

應用程式可以採取的最常見動作是流覽至頁面，其中包含與 voice 命令內容相關的內容。 在此範例中，我們會流覽至 **TripPage** 頁面，並傳入 voice 命令的值、命令的輸入方式，以及可辨識的「目的地」片語 (如果適用) 。 或者，在流覽至頁面時，應用程式可以將導覽參數傳送至 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。

您可以使用 **commandMode** 索引鍵，找出啟動您應用程式的語音命令，或是是否以文字形式 [**輸入的語音**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)命令。 該索引鍵的值將會是 "voice" 或 "text"。 如果索引鍵的值為 "voice"，請考慮在您的應用程式中使用語音合成 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) ，以提供使用者說話的意見反應。

您可以 [**使用 SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)來找出 **ListenFor** 元素的 **PhraseList** 或 **PhraseTopic** 條件約束中所說的內容。 字典索引鍵是 **PhraseList** 或 **PhraseTopic** 元素之 **Label** 屬性的值。 在這裡，我們會示範如何存取 **{destination}** 片語的值。

```csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [使用語音命令在 Cortana 中啟動背景應用程式](cortana-launch-a-background-app-with-voice-commands.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)