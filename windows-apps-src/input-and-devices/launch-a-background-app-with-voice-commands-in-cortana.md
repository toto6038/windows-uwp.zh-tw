---
Description: 除了透過 Cortana 中的語音命令來使用系統功能之外，有一些具備語音命令功能的背景應用程式，可以指定要在應用程式中執行的動作或命令，利用這些背景應用程式，就可以擴充 Cortana 特性與功能。
title: 利用 Cortana 語音命令啟動背景應用程式
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
label: Launch a background app
template: detail.hbs
---

# 利用 Cortana 語音命令啟用背景應用程式


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

除了透過 **Cortana** 中的語音命令來使用系統功能之外，還可以使用可指定要執行的動作或命令的語音命令，從應用程式擴充 **Cortana** 特性與功能 (當成背景工作)。 應用程式在背景中處理語音命令時，不會取得焦點。 而是透過 **Cortana** 畫布和 **Cortana** 語音來傳回所有回覆和結果。

根據互動的複雜性，應用程式可以在前景 (應用程式取得焦點) 或背景 (**Cortana** 保持焦點) 啟用。 舉例來說，需要額外內容或使用者輸入的語音命令 (例如，傳送訊息給特定的連絡人) 最好在前景應用程式中處理，而基本命令 (例如，列出即將出發的行程) 可在 **Cortana** 中透過背景應用程式來處理。

如果您想要使用語音命令將應用程式啟用到前景，請參閱[利用 Cortana 語音命令啟用前景應用程式](launch-a-foreground-app-with-voice-commands-in-cortana.md)。

> **注意**  
> 語音命令定義在語音命令定義 (VCD) 檔中，它是一種具有特定用途的單次語言表達，會透過 **Cortana** 導向已安裝的應用程式。

> VCD 檔案會定義一或多個語音命令，每一個都具有唯一的用途。

> 語音命令定義的複雜度各有不同。 它的支援範圍可從單一、限制的語句，到更具彈性、自然的語言表達集合，而這全都表示同樣的用途。

為了示範背景應用程式功能，我們將使用 **Adventure Works** 的行程安排和管理應用程式，此應用程式來自 [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)。

以下簡介 **Adventure Works** 應用程式與 **Cortana** 畫布之間的功能整合。

![Cortana 畫布概觀 ](images/cortana-overview.png)

若要檢視 **Adventure Works** 行程，但又不想使用 **Cortana**，使用者可以在啟動 app 後，瀏覽到 [**即將出發的行程**] 頁面。

透過 **Cortana** 使用語音命令並在背景中啟動您的 app 時，使用者只要說：「Adventure Works，我什麼時候要到拉斯維加斯旅行？」就可以了。 app 會處理命令，然後 **Cortana** 會顯示結果、您的 app 圖示，而且如果還有其他 app 資訊的話，也會一併顯示。 以下是基本的旅行計劃查詢和 **Cortana** 結果畫面範例，二者顯示並說出：「您下次前往拉斯維加斯的旅行是 8 月 1 日。」

![在背景使用 Adventure Works app 的基本查詢和結果畫面](images/cortana-backgroundapp-result.png)

以下基本步驟示範如何新增語音命令功能，以及利用語音或鍵盤輸入的方式，從您的 app 擴充 **Cortana** 的背景功能：

1.  建立一個從 **Cortana** 背景呼叫的 app 服務 (請參閱 [**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731))。
2.  建立 VCD 檔案。 這是一個 XML 文件，定義使用者可以說的所有語音命令，可在您啟用應用程式時啟始動作或叫用命令。 請參閱 [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)。
3.  應用程式啟動後，註冊 VCD 檔案中的命令集。
4.  處理 app 服務的背景啟動以及語音命令的執行。
5.  在 **Cortana** 中顯示並說出對語音命令合適的回覆。

**先決條件：**

如果您是開發通用 Windows 平台 (UWP) app 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：**

請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)取得相關資訊，來了解如何將您的應用程式與 **Cortana** 整合，您也可以參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)，來取得有關設計既實用又吸引人且支援語音之應用程式的有用提示。

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"> </span> <span id="create_a_new_solution_with_a_primary_project_in_visual_studio"> </span> <span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"> </span>在 Visual Studio 中使用主要專案建立新的方案


1.  啟動 Microsoft Visual Studio 2015。

    Visual Studio 2015 起始頁隨即顯示。

2.  在 [檔案]**** 功能表上，選取 [新增]**** > [專案]****。

    隨即顯示 [新增專案]**** 對話方塊。 您可以在對話方塊的左窗格中選取要顯示的範本類型。

3.  在左窗格中，依序展開 **[已安裝的] > [範本] > [Visual C\#] > [Windows]**，然後選取 [**通用**] 範本群組。 對話方塊的中央窗格會顯示通用 Windows 平台 (UWP) 應用程式的專案範本清單。
4.  在中央窗格中，選取 [空白應用程式 (通用 Windows)]**** 範本。

    [空白應用程式]**** 範本可以建立能夠編譯、執行的最基本 UWP 應用程式，但不包含使用者介面控制項或資料。 在本教學課程中，您會將控制項新增到 app。

5.  在 [**名稱**] 文字方塊中，輸入您的專案名稱。 在這個範例中，我們使用 "AdventureWorks"。
6.  按一下 [**確定**] 來建立專案。

    Microsoft Visual Studio 會建立您的專案，然後在 [**方案總管**] 中顯示。


## <span id="Add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="ADD_IMAGE_ASSETS_TO_PRIMARY_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"> </span>將影像資產新增至主要專案，並在應用程式資訊清單中指定它們
      
UWP 應用程式可以根據特定的設定和裝置功能 (高對比、有效像素及地區設定等)，自動選取最適當的影像。 您需要的就是提供影像，並確保您在應用程式專案內為不同的資源版本使用適當的命名慣例和資料夾組織。 如果您未提供建議的資源版本，則視使用者的喜好設定、能力、裝置類型及位置而定，協助工具、當地語系化及影像品質可能會有問題。

如需高對比和縮放比例的影像資源詳細資訊，請參閱[磚和圖示資產的指導方針](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets)。

您要使用限定詞命名資源。 資源限定詞是資料夾和檔案名稱的修飾詞，用來識別內容中應該使用的特定資源版本。

標準命名慣例為 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`。 例如，`images/en-US/logo.scale-100_contrast-white.png` 只是指在程式碼中使用的根資料夾和檔案名稱：`images/logo.png`。 請參閱[如何使用限定詞命名資源](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh965324.aspx)。

即使您目前不打算提供當地語系化或多種解析度資源，仍建議您在字串資源檔上標示預設語言 (如 `en-US\resources.resw`)，並在影像上標示預設縮放比例 (如 `logo.scale-100.png`)。 不過，我們建議您至少為 100、200 及 400 的縮放比例提供資產。

> *重要

> **Cortana** 畫布的標題區域中使用的應用程式圖示是 "Package.appxmanifest" 檔案中指定的 Square44x44Logo 圖示。 

> 您也可以在 **Cortana** 畫布的內容區域中指定每個項目的圖示。 結果圖示的有效影像大小為︰

> - 68w x 68h 
> - 68w x 92h 
> - 280w x 140h 

在將 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 傳送到 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 之後，才會驗證內容磚。 如果您將 **VoiceCommandResponse** 物件傳送到 **Cortana** (其中包含未遵守這些大小比例之影像的內容磚)，就可能發生例外狀況。 

在這個來自 **Adventure Works** 應用程式 (VoiceCommandService\\AdventureWorksVoiceCommandService.cs) 的範例中，我們在使用 [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169) 磚範本的 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) 上指定簡單的灰色方塊 ("GreyTile.png")。 標誌變體位於 VoiceCommandService\\Images 中，並可使用 [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 方法來抓取。

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="Create_an_app_service_project"> </span> <span id="create_an_app_service_project"> </span> <span id="CREATE_AN_APP_SERVICE_PROJECT"> </span>建立應用程式服務專案

<ol>
    <li>
    Right-click your Solution name, select **New > Project**.
    </li>
    <li>
    Under **Installed > Templates > Visual C# > Windows > Universal**, select **Windows Runtime Component**. This is the component that implements the app service (**[Windows.ApplicationModel.AppService](https://msdn.microsoft.com/library/windows/apps/dn921731)**).
    </li>
    <li>
    Type a name for the project (for example, "VoiceCommandService") and click **OK**.
    </li>
    <li>
    In **Solution Explorer**, select the "VoiceCommandService" project and rename the "Class1.cs" file generated by Visual Studio. For the **Adventure Works** example we use "AdventureWorksVoiceCommandService.cs".
    </li>
    <li>
    Click **Yes** when asked if you want to rename all occurrences of "Class1.cs". 
    </li>
    <li>
    In the "AdventureWorksVoiceCommandService.cs" file:
        <ol type="i">
 <li>
 新增下列 using 指示詞。  
 ```using Windows.ApplicationModel.Background;```
 </li>
 <li>
 當您建立新的專案時，會使用專案名稱做為所有檔案中的預設根命名空間。 重新命名命名空間，以將應用程式服務程式碼 巢狀於主要專案下方。 例如，`namespace AdventureWorks.VoiceCommands`。 
 </li>
 <li>
 在 [方案總管] 中的應用程式服務專案名稱上按一下滑鼠右鍵，然後選取 [**屬性**]。 
 </li>
 <li>
 在 [**程式庫**] 索引標籤上，將 [**預設命名空間**] 欄位更新為相同的值 (在此範例中，為 "AdventureWorks.VoiceCommands")。 
 </li>
 <li>
 建立一個實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 介面的新類別。 此類別需要 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法 (這是 Cortana 辨識語音命令時的進入點)。 
 </li>
        </ol>
    </li>
</ol>

以下是 **Adventure Works** 應用程式中的基本背景工作類別。 我們稍後將填入更多詳細資料。
> **注意**   
> 背景工作類別本身及背景工作專案中的所有其他類別都必須是密封的公用類別。
 
``` csharp
namespace AdventureWorks.VoiceCommands
{
    ...
    
    /// <summary>
    /// The VoiceCommandService implements the entry point for all voice commands.
    /// The individual commands supported are described in the VCD xml file. 
    /// The service entry point is defined in the appxmanifest.
    /// </summary>
    public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
    {
        ...
        
        /// <summary>
        /// The background task entrypoint. 
        /// 
        /// Background tasks must respond to activation by Cortana within 0.5 seconds, and must 
        /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
        /// input). There is no execution time limit on the background task managed by Cortana,
        /// but developers should use plmdebug (https://msdn.microsoft.com/en-us/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
        /// on the Cortana app package in order to prevent Cortana timing out the task during
        /// debugging.
        /// 
        /// The Cortana UI is dismissed if Cortana loses focus. 
        /// The background task is also dismissed even if being debugged. 
        /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
        /// Open the project properties for the app package (not the background task project), 
        /// and enable Debug -> "Do not launch, but debug my code when it starts". 
        /// Alternatively, add a long initial progress screen, and attach to the background task process while it executes.
        /// </summary>
        /// <param name="taskInstance">Connection to the hosting background service process.</param>
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        
          //
          // TODO: Insert code 
          //
          //
        
    }        
  }
}
```

<ol start="7">
    <li>
    Declare your background task as an **AppService** in the app manifest.
    <ol type="i">
        <li>
        In **Solution Explorer**, right click the "Package.appxmanifest" file and select **View Code**. 
        </li>
        <li>
        Find the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add an [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element to the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add a [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to the [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element.
        </li>
        <li>將 **Category** 屬性新增至 **uap:Extension** 元素，然後將 **Category** 屬性的值設定為 "windows.appService"。
        </li>
        <li>
        Add an **EntryPoint** attribute to the **uap:Extension** element and set the value of the **EntryPoint** attribute to the name of the class that implements [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794), in this case "AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element to the **uap:Extension** element.
        </li>
        <li>
        Add a **Name** attribute to the [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element and set the value of the **Name** attribute to a name for the app service, in this case "AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a second [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720).
        </li>
        <li>
        Add a **Category** attribute to this [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element and set the value of the **Category** attribute to "windows.personalAssistantLaunch".
        </li>
    </li> 
    </ol>
    </li>    
</ol>  

以下是從 Adventure Works 應用程式中的資訊清單︰
```xml
<Package>
  <Applications>
    <Application>

      <Extensions>
        <uap:Extension Category="windows.appService" 
          EntryPoint="CortanaBack1.VoiceCommands.CortanaBack1VoiceCommandService">
          <uap:AppService Name="CortanaBack1VoiceCommandService"/>
        </uap:Extension>
        <uap:Extension Category="windows.personalAssistantLaunch"/>
      </Extensions>

    <Application>
  <Applications>
</Package>
```

<ol start="8">
    <li>
    Add this app service project as a reference in the primary project. 
    <ol type="i">
        <li>
        Right click **References**. 
        </li>
        <li>
        Select **Add Reference...** 
        </li>
        <li>
        In the **Reference Manager** dialog, expand **Projects** and select the app service project. 
        </li>
        <li>
        Click OK. 
        </li>
    </ol>
    </li>
</ol>

## <span id="Create_a_VCD_file"> </span> <span id="create_a_vcd_file"> </span> <span id="CREATE_A_VCD_FILE"> </span>建立 VCD 檔案


1. 在 Visual Studio 中，於主要專案名稱上按一下滑鼠右鍵，選取 **[新增] > [新增項目]**。 新增 **XML 檔案**。
2. 輸入 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案的名稱 (在此範例中，為 "AdventureWorksCommands.xml")，然後按一下 [新增]。 
3. 在 [**方案總管**] 中，選取該 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案。
4.  在 [**屬性**] 視窗中，將 [**建置動作**] 設定為 [**內容**]，然後將 [**複製到輸出目錄**] 設定為 [**有更新時才複製**]。

## <span id="Edit_the_VCD_file"> </span> <span id="edit_the_vcd_file"> </span> <span id="EDIT_THE_VCD_FILE"> </span>編輯 VCD 檔案

1. 新增 **xmlns** 屬性指向 "http://schemas.microsoft.com/voicecommands/1.2" 的 **VoiceCommands** 元素。

2. 針對應用程式所支援的每一種語言，建立 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素，內含應用程式所支援的語音命令。

  您可以宣告多個 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 元素，且各自擁有不同的 [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) 屬性，讓您的應用程式可以在不同的市場中使用。 例如，美國的應用程式可以有英文的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 和西班牙文的 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)。

  >  **注意**  
  若要使用語音命令來啟用應用程式並起始動作，應用程式必須註冊一個包含 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 且其語言與使用者針對裝置所選語音功能語言相符的 VCD 檔案。 這個語音功能語言位於 **[設定] > [系統] > [語音] > [語音功能的語言]**。

3. 針對您想要支援的每個命令新增 **Command** 元素。

  [
            **VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 檔案中宣告的每一個 **Command** 都必須包含以下資訊：

  - **Name** 屬性，應用程式在執行期間用來識別語音命令。 
  - **Example** 元素，包含一個片語，描述使用者如何叫用命令。 當使用者說：「我可以說什麼？」、「求助」或者點選 [**查看更多**] 後，**Cortana** 會顯示這個範例。    
  -   **ListenFor** 元素，包含應用程式可辨識為命令的字詞或片語。 每個 **ListenFor** 元素都可以包含一個或多個 **PhraseList** 元素參考，而後面這個元素包含與命令相關的特定字詞。
  > **注意**  
  **ListenFor** 元素無法透過程式設計方式修改。 不過，與 **ListenFor** 元素關聯的 **PhraseList** 元素則可以透過程式設計方式修改。 應用程式應該在執行期間，根據使用者使用 app 時所產生的資料集修改 **PhraseList** 的內容。 請參閱[動態修改語音命令定義 (VCD) 片語清單](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)。

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

## <span id="Install_the_VCD_commands"> </span> <span id="install_the_vcd_commands"> </span> <span id="INSTALL_THE_VCD_COMMANDS"> </span>安裝 VCD 命令

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

  此 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 物件會接著傳遞至 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)。    
```CSharp
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
}```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Handle activation

Specify how your app responds to subsequent voice command activations (after it has been launched at least once and the voice command sets have been installed).

1.  Confirm that your app was activated by a voice command.

    Override the [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) event and check whether [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) is [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determine the name of the command and what was spoken.

    Get a reference to a [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) object from the [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) and query the [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) property for a [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) object.

    To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

3.  Take the appropriate action in your app, such as navigating to the desired page.

For this example, we refer back to the VCD in Step 3: Edit the VCD file.

Once we get the speech-recognition result for the voice command, we get the command name from the first value in the [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) array. As the VCD file defined more than one possible voice command, we need to compare the value against the command names in the VCD and take the appropriate action.

The most common action an application can take is to navigate to a page with content relevant to the context of the voice command. For this example, we navigate to a **TripPage** page and pass in the value of the voice command, how the command was input, and the recognized "destination" phrase (if applicable). Alternatively, the app could send a navigation parameter to the [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) when navigating to the page.

You can find out whether the voice command that launched your app was actually spoken, or whether it was typed in as text, from the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) dictionary using the **commandMode** key. The value of that key will be either "voice" or "text". If the value of the key is "voice", consider using speech synthesis ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in your app to provide the user with spoken feedback.

Use the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) to find out the content spoken in the **PhraseList** or **PhraseTopic** constraints of a **ListenFor** element. The dictionary key is the value of the **Label** attribute of the **PhraseList** or **PhraseTopic** element. Here, we show how to access the value of **{destination}** phrase.

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

## <span id="Handle_the_voice_command_in_the_app_service"> </span> <span id="handle_the_voice_command_in_the_app_service"> </span> <span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"> </span>處理應用程式服務中的語音命令


處理應用程式服務中的語音命令。


1.  在此範例中，將下列 using 指示詞新增至語音命令服務檔案："AdventureWorksVoiceCommandService.cs"。
```csharp
using Windows.ApplicationModel.VoiceCommands;
using Windows.ApplicationModel.Resources.Core;
using Windows.ApplicationModel.AppService;
```

2.  讓服務暫緩執行，這樣一來當應用程式服務在處理語音命令的時候，就不會終止您的應用程式。
3.  確定系統會將您執行中的背景工作當做由語音命令所啟動的應用程式服務。

    1.  將 [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) 轉換成 [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727)。
    2.  檢查 [**IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807) 是否為 "Package.appxmanifest" 檔案中的 app 服務名稱。

4.  使用 [**IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802) 來將 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 建立至 **Cortana**，以抓取語音命令。
5.  註冊 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584) 的事件處理常式，以在因使用者取消而造成 app 服務結束後收到通知。
6.  註冊 [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 的事件處理常式，以在因發生意外故障而造成應用程式服務關閉後收到通知。
7.  判斷命令的名稱和所說出的內容。

    1.  使用 [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589) 屬性來判斷語音命令的名稱。
    2.  要判斷使用者說什麼，請檢查 [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) 字典中 [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) 的值或已辨識片語的語意屬性。

7.  在您的 app 服務中採取適當的動作。
8.  在 **Cortana** 中顯示並說出對語音命令的回覆。

    1.  決定當 **Cortana** 回覆語音命令時，要讓使用者看到和聽到的字串，然後建立 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 物件。 請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)，了解如何選取 **Cortana** 會顯示和說出的回覆字串。
    2.  透過使用 **VoiceCommandServiceConnection** 物件呼叫 [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) 或 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 來使用 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 執行個體向 **Cortana** 報告進度或報告工作已完成。

    在這個範例中，我們會回頭參考 VCD 的步驟 3：編輯 VCD 檔案。

```csharp
public sealed class VoiceCommandService : IBackgroundTask
    {
      private BackgroundTaskDeferral serviceDeferral;
      VoiceCommandServiceConnection voiceServiceConnection;

      public async void Run(IBackgroundTaskInstance taskInstance)
      {
      //Take a service deferral so the service isn&#39;t terminated.
        this.serviceDeferral = taskInstance.GetDeferral();

        taskInstance.Canceled += OnTaskCanceled;

        var triggerDetails = 
          taskInstance.TriggerDetails as AppServiceTriggerDetails;

        if (triggerDetails != null &amp;&amp; 
          triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint")
        {
          try
          {
 voiceServiceConnection = 
   VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
     triggerDetails);

 voiceServiceConnection.VoiceCommandCompleted += 
   VoiceCommandCompleted;

 VoiceCommand voiceCommand = await
 voiceServiceConnection.GetVoiceCommandAsync();

 switch (voiceCommand.CommandName)
 {
   case "whenIsTripToDestination":
   {
     var destination = 
       voiceCommand.Properties["destination"][0];
     SendCompletionMessageForDestination(destination);
     break;
   }

   // As a last resort, launch the app in the foreground.
   default:
     LaunchAppInForeground();
     break;
 }
          }
          finally
          {
 if (this.serviceDeferral != null)
 {
   // Complete the service deferral.
   this.serviceDeferral.Complete();
 }
          }
        }
      }

      private void VoiceCommandCompleted(
        VoiceCommandServiceConnection sender, 
        VoiceCommandCompletedEventArgs args)
      {
        if (this.serviceDeferral != null)
        {
          // Insert your code here.
          // Complete the service deferral.
          this.serviceDeferral.Complete();
        }
      }

      private async void SendCompletionMessageForDestination(
        string destination)
      {
        // Take action and determine when the next trip to destination
        // Insert code here.
        
        // Replace the hardcoded strings used here with strings 
        // appropriate for your application.

        // First, create the VoiceCommandUserMessage with the strings 
        // that Cortana will show and speak.
        var userMessage = new VoiceCommandUserMessage();
        userMessage.DisplayMessage = "Here’s your trip.";
        userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";

        // Optionally, present visual information about the answer.
        // For this example, create a VoiceCommandContentTile with an 
        // icon and a string.
        var destinationsContentTiles = new List<VoiceCommandContentTile>();

        var destinationTile = new VoiceCommandContentTile();
        destinationTile.ContentTileType = 
          VoiceCommandContentTileType.TitleWith68x68IconAndText;
        // The user can tap on the visual content to launch the app. 
        // Pass in a launch argument to enable the app to deep link to a 
        // page relevant to the item displayed on the content tile.
        destinationTile.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        destinationTile.Title = "Las Vegas";
        destinationTile.TextLine1 = "August 3rd 2015";
        destinationsContentTiles.Add(destinationTile);

        // Create the VoiceCommandResponse from the userMessage and list    
        // of content tiles.
        var response = 
          VoiceCommandResponse.CreateResponse(
 userMessage, destinationsContentTiles);

        // Cortana will present a “Go to app_name” link that the user 
        // can tap to launch the app. 
        // Pass in a launch to enable the app to deep link to a page 
        // relevant to the voice command.
        response.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        
        // Ask Cortana to display the user message and content tile and 
        // also speak the user message.
        await voiceServiceConnection.ReportSuccessAsync(response);
      }

      private async void LaunchAppInForeground()
      {
        var userMessage = new VoiceCommandUserMessage();
        userMessage.SpokenMessage = "Launching Adventure Works";

        var response = VoiceCommandResponse.CreateResponse(userMessage);

        // When launching the app in the foreground, pass an app 
        // specific launch parameter to indicate what page to show.
        response.AppLaunchArgument = "showAllTrips=true";

        await voiceServiceConnection.RequestAppLaunchAsync(response);
      }
    }
```

啟用之後，應用程式服務有 .5 秒鐘的時間呼叫 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)。 **Cortana** 會使用應用程式所提供的資料，來顯示並說出 VCD 檔案中指定的回覆內容。 如果 app 的呼叫時間超過 .5 秒，**Cortana** 就會插入一個遞交畫面，如下所示。 **Cortana** 會一直顯示遞交畫面，直到應用程式呼叫 **ReportSuccessAsync** 或達到最高 5 秒的時間為止。 如果 app 服務未呼叫 **ReportSuccessAsync** 或任何可以向 **Cortana** 提供資訊的 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 方法，則使用者會收到錯誤訊息，然後 app 服務會跟著取消。

![在背景使用 Adventure Works 應用程式執行基本查詢，並提供進度和結果畫面](images/cortana-backgroundapp-progress-result.png)

## <span id="related_topics"> </span>相關文章


**開發人員**
* [Cortana 互動](cortana-interactions.md)
* [定義自訂辨識限制式](define-custom-recognition-constraints.md)
* [利用 Cortana 與背景應用程式互動](interact-with-a-background-app-in-cortana.md)
* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [快速入門：使用檔案或影像資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)

**設計人員**
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [適用於 UWP App 的回應式設計 101](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [磚和圖示資產的指導方針](https://msdn.microsoft.com/library/windows/apps/mt412102)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


