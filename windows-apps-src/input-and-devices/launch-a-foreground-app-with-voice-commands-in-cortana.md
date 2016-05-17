---
author: Karl-Bridge-Microsoft
Description: 除了使用 Cortana 中的語音命令來存取系統功能之外，您也可以透過 Cortana 使用語音命令來啟動前景應用程式，以及指定要在應用程式中執行的動作或命令。
title: 利用 Cortana 語音命令啟動前景應用程式
ms.assetid: 8D3D1F66-7D17-4DD1-B426-DCCBD534EF00
label: Cortana-Launch a foreground app
template: detail.hbs
---

# 利用 Cortana 語音命令啟用前景應用程式

除了透過 **Cortana** 中的語音命令來使用系統功能之外，還可以從您的應用程式擴充 **Cortana** 特性與功能。 使用語音命令，可以將您的應用程式啟用到前景，以及在應用程式內執行動作或命令。 

**重要 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


應用程式在前景中處理語音命令時，會取得焦點，並關閉 Cortana。 如果您想要的話，可以啟用您的應用程式，並執行命令做為背景工作。 在此情況下，Cortana 會保留焦點，而且您的應用程式會透過 **Cortana** 畫布和 **Cortana** 語音來傳回所有回覆和結果。

需要額外內容或使用者輸入的語音命令 (例如，傳送訊息給特定的連絡人) 最好在前景應用程式中處理，而基本命令 (例如，列出即將出發的行程) 可在 **Cortana** 中透過背景應用程式來處理。

如果您想要使用語音命令在背景啟用應用程式，請參閱[利用 Cortana 語音命令啟用背景應用程式](launch-a-background-app-with-voice-commands-in-cortana.md)

> **注意**  
> 語音命令定義在語音命令定義 (VCD) 檔中，它是一種具有特定用途的單次語言表達，會透過 **Cortana** 導向已安裝的應用程式。

> VCD 檔案會定義一或多個語音命令，每一個都具有唯一的用途。

> 語音命令定義的複雜度各有不同。 它的支援範圍可從單一、限制的語句，到更具彈性、自然的語言表達集合，而這全都表示同樣的用途。

為了示範前景應用程式功能，我們將使用 **Adventure Works** 的行程安排和管理應用程式，此應用程式來自 [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)

若要建立新的 **Adventure Works** 行程，但又不想使用 **Cortana**，使用者可以在啟動應用程式後，瀏覽 [**新行程**] 頁面。 若要檢視現有的行程，使用者可以在啟動 app 後，瀏覽 [**即將出發的行程**] 頁面並選取行程。

透過 **Cortana** 使用語音命令時，使用者可以改為說：「Adventure Works 新增行程」或「在 Adventure Works 上新增行程」，即可啟動 app，然後瀏覽到 [**新行程**] 頁面。 接著，說出「Adventure Works，顯示我到倫敦的行程」將會啟動 app，並瀏覽到 [**行程**] 詳細資料頁面，如下所示。

![Cortana 啟動前景 app](images/cortana-foreground-with-adventureworks.png)

以下是新增語音命令功能及使用語音或鍵盤輸入將 Cortana 整合至 app 的基本步驟：

1.  建立 VCD 檔案。 這是一個 XML 文件，定義使用者可以說的所有語音命令，可在您啟用應用程式時啟始動作或叫用命令。 請參閱 [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
2.  應用程式啟動後，註冊 VCD 檔案中的命令集。
3.  處理透過語音命令進行的啟用、app 內的瀏覽，以及命令的執行。

**先決條件：  **

如果您是開發通用 Windows 平台 (UWP) app 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：  **

請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233) 取得相關資訊，來了解如何將您的應用程式與 **Cortana** 整合，您也可以參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)，來取得有關設計既實用又吸引人且支援語音之應用程式的有用提示。

## <span id="Create_a_new_solution_with_project_in_Visual_Studio"></span><span id="create_a_new_solution_with__project_in_visual_studio"></span><span id="CREATE_A_NEW_SOLUTION_WITH__PROJECT_IN_VISUAL_STUDIO"></span>在 Visual Studio 中使用專案建立新的方案


1.  啟動 Microsoft Visual Studio 2015。

    Visual Studio 2015 起始頁隨即顯示。

2.  在 [檔案]**** 功能表上，選取 [新增]**** > [專案]****。

    隨即顯示 [新增專案]**** 對話方塊。 您可以在對話方塊的左窗格中選取要顯示的範本類型。

3.  在左窗格中，依序展開 **[已安裝的] > [範本] > [Visual C\#] > [Windows]**，然後選取 [**通用**] 範本群組。 對話方塊的中央窗格會顯示通用 Windows 平台 (UWP) app 的專案範本清單。
4.  在中央窗格中，選取 [空白應用程式 (通用 Windows)]**** 範本。

    [空白應用程式]**** 範本可以建立能夠編譯、執行的最基本 UWP 應用程式，但不包含使用者介面控制項或資料。 在本教學課程中，您會將控制項新增到 app。

5.  在 [**名稱**] 文字方塊中，輸入您的專案名稱。 在這個範例中，我們使用 "AdventureWorks"。
6.  按一下 [確定]**** 來建立專案。

    Microsoft Visual Studio 會建立您的專案，然後在 [**方案總管**] 中顯示

## <span id="Add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="add_image_assets_to_project_and_specify_them_in_the_app_manifest"></span><span id="ADD_IMAGE_ASSETS_TO_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"></span>將影像資產新增至專案，並在應用程式資訊清單中指定它們
      
UWP 應用程式可以根據特定的設定和裝置功能 (高對比、有效像素及地區設定等)，自動選取最適當的影像。 您需要的就是提供影像，並確保您在應用程式專案內為不同的資源版本使用適當的命名慣例和資料夾組織。 如果您未提供建議的資源版本，則視使用者的喜好設定、能力、裝置類型及位置而定，協助工具、當地語系化及影像品質可能會有問題。

如需高對比和縮放比例的影像資源詳細資訊，請參閱[磚和圖示資產的指導方針](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets)

您要使用限定詞命名資源。 資源限定詞是資料夾和檔案名稱的修飾詞，用來識別內容中應該使用的特定資源版本。

標準命名慣例為 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`。 例如，`images/en-US/logo.scale-100_contrast-white.png` 只是指在程式碼中使用的根資料夾和檔案名稱：`images/logo.png`。 請參閱[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx)

即使您目前不打算提供當地語系化或多種解析度資源，仍建議您在字串資源檔上標示預設語言 (如 `en-US\resources.resw`)，並在影像上標示預設縮放比例 (如 `logo.scale-100.png`)。 不過，我們建議您至少為 100、200 及 400 的縮放比例提供資產。

> *重要

> **Cortana** 畫布的標題區域中使用的應用程式圖示是 "Package.appxmanifest" 檔案中指定的 Square44x44Logo 圖示。 
    
## <span id="Create_a_VCD_file"></span><span id="create_a_vcd_file"></span><span id="CREATE_A_VCD_FILE"></span>建立 VCD 檔案

1. 在 Visual Studio 中，於主要專案名稱上按一下滑鼠右鍵，選取 **[新增] > [新增項目]**。 新增 **XML 檔案**
2. 輸入 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案的名稱 (在此範例中，為 "AdventureWorksCommands.xml")，然後按一下 [新增]。 
3. 在 [**方案總管**] 中，選取該 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案。
4.  在 [**屬性**] 視窗中，將 [**建置動作**] 設定為 [**內容**]，然後將 [**複製到輸出目錄**] 設定為 [**有更新時才複製**]

## <span id="Edit_the_VCD_file"></span><span id="edit_the_vcd_file"></span><span id="EDIT_THE_VCD_FILE"></span>編輯 VCD 檔案


新增 **VoiceCommands** 元素，其 **xmlns** 屬性指向

2. 針對應用程式所支援的每一種語言，建立 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素，內含應用程式所支援的語音命令。

  您可以宣告多個 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素，且各自擁有不同的 [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) 屬性，讓您的應用程式可以在不同的市場中使用。 例如，美國的應用程式可以有英文的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 和西班牙文的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)。

  >  **注意**  
  若要使用語音命令來啟用應用程式並起始動作，應用程式必須註冊一個包含 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 且其語言與使用者針對裝置所選語音功能語言相符的 VCD 檔案。 這個語音功能語言位於 **[設定] > [系統] > [語音] > [語音功能的語言]**

3. 針對您想要支援的每個命令新增 **Command** 元素。

  [
            **VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案中宣告的每一個 **Command** 都必須包含以下資訊：

  - **Name** 屬性，應用程式在執行期間用來識別語音命令。 
  - **Example** 元素，包含一個片語，描述使用者如何叫用命令。 當使用者說：「我可以說什麼？」、「求助」或者點選 [**查看更多**] 後，**Cortana** 會顯示這個範例    
  -   **ListenFor** 元素，包含應用程式可辨識為命令的字詞或片語。 每個 **ListenFor** 元素都可以包含一個或多個 **PhraseList** 元素參考，而後面這個元素包含與命令相關的特定字詞。
  > **注意**  
  **ListenFor** 元素無法透過程式設計方式修改。 不過，與 **ListenFor** 元素關聯的 **PhraseList** 元素則可以透過程式設計方式修改。 應用程式應該在執行期間，根據使用者使用 app 時所產生的資料集修改 **PhraseList** 的內容。 請參閱[動態修改語音命令定義 (VCD) 片語清單](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)

  -   **Feedback** 元素，包含當應用程式啟動後，**Cortana** 會顯示以及說出的文字。

**Navigate** 元素表示語音命令會在前景啟動應用程式。 在此範例中，```showTripToDestination``` 命令是前景工作。

**VoiceCommandService** 元素指出語音命令會在背景啟動應用程式。 此元素的 **Target** 屬性值應該符合 package.appxmanifest 中 [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) 元素的 **Name** 屬性值。 在這個範例中，```whenIsTripToDestination``` 和 ```cancelTripToDestination``` 命令是背景工作，可將應用程式服務的名稱指定為 "AdventureWorksVoiceCommandService"。

如需詳細資料，請參閱 [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593) 參考。

以下是 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案的一個部分，定義 **Adventure Works** 應用程式的 en-us 語音命令。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
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

## <span id="Install_the_VCD_commands"></span><span id="install_the_vcd_commands"></span><span id="INSTALL_THE_VCD_COMMANDS"></span>安裝 VCD 命令


您的應用程式必須執行一次，才能安裝 VCD。 

>  **注意**  
不會跨應用程式安裝來保留語音命令資料。 為確保您應用程式的語音命令資料完整無缺，請考慮在每次應用程式啟動或啟用時將您的 VCD 檔案初始化，或是維護指出目前是否已安裝 VCD 的設定。

在 "app.xaml.cs" 檔案中：

1. 新增下列 using 指示詞：  
```csharp
using Windows.Storage;
```
2. 使用 async 修飾詞，標示 "OnLaunched" 方法。  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. 呼叫 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 處理常式中的 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)，以註冊系統應該辨識到的語音命令。

  在 Adventure Works 範例中，我們先定義 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件。 

  然後我們呼叫 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 將它與我們的 "AdventureWorksCommands.xml" 檔案初始化。

  此 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件會接著傳遞至 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)    
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

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>處理啟用及執行語音命令

指定您的 app 應如何回應後續的語音命令啟用 (在已將 app 至少啟動一次，以及已安裝語音命令集之後)。

1.  確認已使用語音命令啟用您的應用程式。

    覆寫 [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件並檢查 [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 是否為 [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693)

2.  判斷命令的名稱和所說出的內容。

    從 [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) 取得對 [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) 物件的參考，並查詢 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) 物件的 [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) 屬性。

    要判斷使用者說什麼，請檢查 [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) 字典中 [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) 的值或已辨識片語的語意屬性。

3.  在您的 app 中採取適當的動作，例如瀏覽到想要的頁面。

在這個範例中，我們會回頭參考 VCD 的步驟 3：編輯 VCD 檔案。

在取得語音命令的語音辨識結果之後，我們會從 [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) 陣列中的第一個值取得命令名稱。 當 VCD 檔案定義多個可能的語音命令時，我們需要將這個值與 VCD 中的命令名稱的進行比對，然後採取適當的動作。

應用程式可以採取的最常見動作，就是瀏覽某個提供語音命令上下文相關內容的頁面。 這個範例中，我們瀏覽 **TripPage** 頁面，然後傳入語音命令的值、如何輸入命令，以及辨識的「目的地」片語 (如果適用)。 或者，應用程式在瀏覽該頁面時，可以傳送瀏覽參數到 [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432)。

您可以使用 **commandMode** 索引鍵，從 [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) 字典中查出啟動您 app 的語音命令是藉由實際說出還是藉由文字輸入所下達的命令。 該索引鍵的值將會是 "voice" 或 "text"。 如果索引鍵的值是 "voice"，請考慮在您的 app 中使用語音合成 ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951))，為使用者提供語音回覆。

利用 [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) 到 **ListenFor** 元素的 **PhraseList** 或 **PhraseTopic** 限制式中查明語音內容。 字典索引鍵是 **PhraseList** 或 **PhraseTopic** 元素的 **Label** 屬性值。 現在我們示範如何存取 **{destination}** 片語的值。

``` csharp
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

## <span id="related_topics"></span>相關文章


**開發人員**
* [Cortana 互動](cortana-interactions.md)
* [定義自訂辨識限制式](define-custom-recognition-constraints.md)
* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**設計人員**
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


