---
title: 使用語音命令啟用 Cortana 中的背景應用程式 |Cortana UWP 設計與開發
description: 使用語音命令) 的背景工作，利用應用程式中的功能擴充 Cortana (。
ms.assetid: e2c7eae3-6beb-4156-92a5-474bba53451e
ms.date: 09/24/2019
ms.topic: article
keywords: cortana
ms.openlocfilehash: 987a8f4146e5f97c0338c6a8492ace050fbaa3d0
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824632"
---
# <a name="activate-a-background-app-in-cortana-using-voice-commands"></a>使用語音命令在 Cortana 中啟動背景應用程式  

>[!WARNING]
> Windows 10 2020 版更新 (2004 版 codename "20H1" ) ，不再支援此功能。
>
> 查看 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

除了在 **cortana** 內使用語音命令來存取系統功能以外，您也可以使用語音命令（指定要執行的動作或命令）將 **cortana** 與應用程式中的特性和功能延伸 (為背景工作) 。 當應用程式在背景處理語音命令時，不會取得焦點。 相反地，它會透過 **cortana** 畫布和 **cortana** 聲音傳回所有意見反應和結果。  

> [!NOTE]
> **重要 API**
>
> - [**ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和屬性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

應用程式可能會啟用到前景 (應用程式取得焦點) 或在背景中啟動 (**Cortana** 會根據互動的複雜度保留焦點) 。 例如，需要額外內容或使用者輸入的語音命令 (例如，將訊息傳送至特定連絡人) 最適合在前景應用程式中處理，而基本命令 (例如列出即將進行的旅程) 可能會透過背景應用程式在 **Cortana** 中處理。  

如果您想要使用語音命令來啟動應用程式到前景，請參閱使用 [語音命令透過 Cortana 啟用前景應用程式](cortana-launch-a-foreground-app-with-voice-commands.md)。  

> [!NOTE]
> 語音命令是具有特定意圖的單一語句，定義于語音命令定義 (VCD) 檔，透過 **Cortana** 導向已安裝的應用程式。
>
> VCD 檔案會定義一或多個語音命令，每個命令都有獨特的意圖。
>
> 語音命令定義可能會有不同的複雜度。 它們可以支援從單一、受限的語句到更具彈性的自然語言語句集合的任何作業，而且全都代表相同的意圖。

我們會使用名為 [ **艾德公司** ] 的行程規劃和管理應用程式，並整合至 **Cortana** UI （如下所示），以示範我們所討論的許多概念和功能。 如需詳細資訊，請參閱 [Cortana voice 命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)。

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 啟動前景應用程式的螢幕擷取畫面":::

若要在不使用 **Cortana** 的情況下查看 **艾德公司** 的旅程，使用者會啟動應用程式並流覽至 **即將推出** 的 [旅程] 頁面。  

使用語音命令透過 **Cortana** 在背景中啟動您的應用程式時，使用者可能會改為使用 `Adventure Works, when is my trip to Las Vegas?` 。 如果有提供，您的應用程式會處理命令，且 **Cortana** 會顯示結果以及應用程式圖示和其他應用程式資訊。  

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="在背景中使用 AdventureWorks 應用程式之 Cortana 與基本查詢和結果畫面的螢幕擷取畫面":::

下列基本步驟會使用語音或鍵盤輸入，新增語音命令功能，並使用應用程式的背景功能來擴充 **Cortana** 。

1. 建立 app service (請參閱 **Cortana** 在背景中叫用的 [**AppService**](/uwp/api/Windows.ApplicationModel.AppService)) 。  
2. 建立 VCD 檔案。 VCD 檔案是一份 XML 檔，它會定義使用者在啟用應用程式時，可能會想要起始動作或叫用命令的所有語音命令。 請參閱 [**VCD 元素和屬性-1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)。  
3. 當應用程式啟動時，在 VCD 檔案中註冊命令集。  
4. 處理 app service 的背景啟用和語音命令的執行。  
5. 顯示適當的意見反應，並向 **Cortana** 內的 voice 命令說出。  

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

## <a name="create-a-new-solution-with-a-primary-project-in-visual-studio"></a>使用 Visual Studio 中的主要專案建立新的方案  

1. 啟動 Microsoft Visual Studio 2015。  
    Visual Studio 2015 的 [開始] 頁面隨即出現。  

2. 在 [檔案] 功能表上，選取 [新增] > [專案]。  
    [ **新增專案** ] 對話方塊隨即出現。 對話方塊的左窗格可讓您選取要顯示的範本類型。  

3. 在左窗格中，展開 [ **已安裝的 > 範本] > Visual C \# > Windows**，然後挑選 **通用** 範本群組。 對話方塊的中間窗格會顯示適用于通用 Windows 平臺 (UWP) 應用程式的專案範本清單。  
4. 在中央窗格中，選取 **(通用 Windows) 範本的空白應用程式** 。  
    **空白應用程式** 範本會建立可編譯和執行的最基本 UWP 應用程式。 **空白應用程式** 範本不包含使用者介面控制項或資料。 您可以使用此頁面作為指南，將控制項新增至應用程式。  

5. 在 [ **名稱** ] 文字方塊中，輸入您的專案名稱。 範例：使用 `AdventureWorks` 。  
6. 按一下 [ **確定]** 按鈕以建立專案。  
    Microsoft Visual Studio 會建立您的專案，並將其顯示在 **Solution Explorer** 中。  

## <a name="add-image-assets-to-primary-project-and-specify-them-in-the-app-manifest"></a>將影像資產新增至主要專案，並在應用程式資訊清單中指定它們  

UWP 應用程式應該會自動選取最適當的影像。 選取範圍是根據特定的設定和裝置功能， (高對比、有效圖元、地區設定等) 。 您必須提供映射，並確保您在應用程式專案內針對不同的資源版本使用適當的命名慣例和資料夾組織。  
如果您未提供建議的資源版本，則使用者體驗可能會以下列方式受到影響。

- 協助工具選項
- 當地語系化  
- 影像品質  
資源版本是用來調整使用者體驗中的下列變更。  
- 使用者喜好設定  
- 能力  
- 裝置類型  
- Location  

如需高對比和縮放比例之影像資源的詳細資訊，請流覽位於 [msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets](../../app-resources/images-tailored-for-scale-theme-contrast.md)的磚和圖示資產頁面的指導方針。  

您必須使用限定詞來命名資源。 資源限定詞是資料夾和檔案名修飾詞，用來識別應該使用特定版本資源的內容。  

標準命名慣例為 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` 。  
範例： `images/logo.scale-100_contrast-white.png` ，它可能只會使用根資料夾和檔案名來參考程式碼： `images/logo.png` 。  
For more information, visit the How to name resources using qualifiers page located at [msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx](/previous-versions/windows/apps/hh965324(v=win.10)).  

Microsoft 建議您將字串資源檔上的預設語言標示 (例如 `en-US\resources.resw`) 和影像上的預設縮放比例 (例如 `logo.scale-100.png`) ，即使您目前未規劃提供當地語系化或多個解析資源。 不過，Microsoft 至少建議您提供100、200和400規模調整因素的資產。  

>[!IMPORTANT]
> **Cortana** 畫布標題區域中所使用的應用程式圖示是在檔案中指定的 Square44x44Logo 圖示 `Package.appxmanifest` 。  
> 您也可以在 **Cortana** 畫布的 [內容] 區域中，指定每個專案的圖示。 結果圖示的有效影像大小為：
>
> - 68w x 68h
> - 68w x 92h  
> - 280w x 140h  

在 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 物件傳遞至 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 類別之前，不會驗證內容磚。 如果您將 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 物件傳遞至 **Cortana** ，且其中包含的內容圖格具有不符合這些大小比例的影像，則可能會發生例外狀況。  

範例： [**艾德作品** 應用程式] (`VoiceCommandService\\AdventureWorksVoiceCommandService.cs`) 會 `GreyTile.png` 使用 [**TitleWith68x68IconAndText**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType)磚範本，在 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)類別上指定簡單的灰色方形 () 。 標誌變異位於中 `VoiceCommandService\\Images` ，並使用 [**GetFileFromApplicationUriAsync**](/uwp/api/Windows.Storage.StorageFile) 方法進行抓取。

```csharp
var destinationTile = new VoiceCommandContentTile();  

destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png")
);  
```

## <a name="create-an-app-service-project"></a>建立 App Service 專案

1. 以滑鼠右鍵按一下您的方案名稱，然後選取 [ **新增 > 專案**]。  
2. 在 [ **安裝的 > 範本] > Visual C \# > Windows > 通用**] 下，選取 [ **windows 執行時間元件**]。 **Windows 執行時間元件** 是 ([**ApplicationModel AppService**](/uwp/api/Windows.ApplicationModel.AppService)) 執行 app service 的元件。  
3. 輸入專案的名稱，然後按一下 [ **確定]** 按鈕。  
    範例： `VoiceCommandService`.  
4. 在 [ **方案瀏覽器**] 中，選取 `VoiceCommandService` 專案並重新命名 `Class1.cs` Visual Studio 產生的檔案。
    範例：「 **艾德作品** 」使用 `AdventureWorksVoiceCommandService.cs` 。  
5. 按一下 [ **是]** 按鈕;當系統詢問您是否要重新命名所有出現的 `Class1.cs` 。  
6. 在 `AdventureWorksVoiceCommandService.cs` 檔案中：
    1. 新增下列 using 指示詞。  
        `using Windows.ApplicationModel.Background;`  
    2. 當您建立新專案時，專案名稱會用來做為所有檔案中的預設根命名空間。 將命名空間重新命名，以將 app service 程式碼嵌入主要專案下。
        範例： `namespace AdventureWorks.VoiceCommands`.  
    3. 在 [方案瀏覽器] 中，以滑鼠右鍵按一下 app service 專案名稱，然後選取 [ **屬性**]。  
    4. 在 [連結 **庫** ] 索引標籤上，使用相同的值來更新 [ **預設命名空間** ] 欄位。  
        範例： `AdventureWorks.VoiceCommands`) 。  
    5. 建立一個實作 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 介面的新類別。 此類別需要 [**Run**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 方法，這是 Cortana 辨識 voice 命令時的進入點。  

    範例：來自「 **艾德公司** 」應用程式的基本背景工作類別。  

    >[!NOTE]
    > 背景工作類別本身以及背景工作專案中的所有類別都必須是密封的公用類別。  

    ```csharp
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
            /// Background tasks must respond to activation by Cortana within 0.5 second, and must 
            /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
            /// input). There is no running time limit on the background task managed by Cortana,
            /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
            /// on the Cortana app package in order to prevent Cortana timing out the task during
            /// debugging.
            /// 
            /// The Cortana UI is dismissed if Cortana loses focus. 
            /// The background task is also dismissed even if being debugged. 
            /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
            /// Open the project properties for the app package (not the background task project), 
            /// and enable Debug -> "Do not launch, but debug my code when it starts". 
            /// Alternatively, add a long initial progress screen, and attach to the background task process while it runs.
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

7. 將您的背景工作宣告為應用程式資訊清單中的 **AppService** 。  
    1. 在 [ **方案 Explorer**] 中，以滑鼠右鍵按一下檔案 `Package.appxmanifest` ，然後選取 [ **視圖程式碼**]。  
    2. 尋找 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素。  
    3. 將 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素加入至 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素。  
    4. 將 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 元素加入至 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素。  
    5. 將 `Category` 屬性加入至專案 `uap:Extension` ，並將屬性的值設定 `Category` 為 `windows.appService` 。  
    6. 將 `EntryPoint` 屬性加入至專案 `uap: Extension` ，並將屬性的值設定 `EntryPoint` 為所執行之類別的名稱 [`IBackgroundTask`](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 。  
        範例： `AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService`.  
    7. 將 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 元素加入至 `uap:Extension` 元素。  
    8. 將屬性新增至專案， `Name` [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 並將屬性的值設定 `Name` 為 app service 的名稱，在此案例中為 `AdventureWorksVoiceCommandService` 。  
    9. 將第二個 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 元素新增至專案 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 。  
    10. 將 `Category` 屬性加入至這個專案 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) ，並將屬性的值設定 `Category` 為 `windows.personalAssistantLaunch` 。  

    範例：來自「來自艾德作品」應用程式的資訊清單。

    ```xml
    <Package>
        <Applications>
            <Application>
            
                <Extensions>
                    <uap:Extension Category="windows.appService" EntryPoint="CortanaBack1.VoiceCommands.AdventureWorksVoiceCommandService">
                        <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                    </uap:Extension>
                    <uap:Extension Category="windows.personalAssistantLaunch"/>
                </Extensions>
                
            <Application>
        <Applications>
    </Package>
    ```  

8. 將此 app service 專案新增為主要專案中的參考。  
    1. 以滑鼠右鍵按一下 **參考**。  
    2. 選取 [ **加入參考 ...**]。  
    3. 在 [ **參考管理員** ] 對話方塊中，展開 [ **專案** ]，然後選取 [app service] 專案。  
    4. 按一下 [ **確定]** 按鈕。  

## <a name="create-a-vcd-file"></a>建立 VCD 檔案

1. 在 Visual Studio 中，以滑鼠右鍵按一下您的主要專案名稱，然後選取 [ **加入 > 新專案**]。 加入 **XML** 檔案。  
2. 輸入 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 檔案的名稱。  
    範例： `AdventureWorksCommands.xml`.
3. 按一下 [ **新增** ] 按鈕。  
4. 在 [ **方案瀏覽器**] 中，選取 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 檔案。  
5. 在 [ **屬性** ] 視窗中，將 [ **組建動作** ] 設定為 [ **內容**]，然後將 [ **複製到輸出目錄** ] 設定為 [ **更新時複製**]  

## <a name="edit-the-vcd-file"></a>編輯 VCD 檔案  

1. 加入 `VoiceCommands` 具有指向之屬性的元素 `xmlns` `https://schemas.microsoft.com/voicecommands/1.2` 。  
2. 針對您的應用程式所支援的每種語言，建立 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 包含應用程式所支援語音命令的元素。  
    您可以宣告多個 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 元素，每個元素都有不同的 [`xml:lang`](/previous-versions/windows/dn722331(v=win.10)) 屬性，讓您的應用程式可用於不同的市場。 例如，美國的應用程式可能會有 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 適用于英文的，以及 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 用於西班牙文的。  

    >[!IMPORTANT]
    > 若要啟用應用程式並使用 voice 命令起始動作，應用程式必須註冊包含專案的 VCD 檔案，該檔案的 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 語言符合使用者裝置中指出的語音語言。 語音語言位於 [設定] **> 系統 > 語音 > 語音語言**。  

3. `Command`針對您想要支援的每個命令新增元素。  
    `Command`在 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)檔案中宣告的每個都必須包含下列資訊：  
    - `Name`您的應用程式在執行時間用來識別語音命令的屬性。  
    - `Example`元素，其中包含描述使用者如何叫用命令的片語。 **Cortana** 會在使用者說 `What can I say?` 、 `Help` 或點擊 **查看更多** 內容時顯示範例。  
    - `ListenFor`元素，其中包含您的應用程式可辨識為命令的單字或片語。 每個專案都 `ListenFor` 可以包含一個或多個元素的參考，其中 `PhraseList` 包含與命令相關的特定單字。  

       >[!NOTE]
       > `ListenFor` 不得以程式設計的方式修改元素。 不過， `PhraseList` 與專案相關聯的專案 `ListenFor` 可能會以程式設計的方式修改。 應用程式應該 `PhraseList` 根據使用者使用應用程式時所產生的資料集，在執行時間修改元素的內容。
       >
       > 如需詳細資訊，請參閱 [動態修改 CORTANA VCD 片語清單](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)。  

    - `Feedback`元素，其中包含要在應用程式啟動時顯示和說話的 **Cortana** 文字。  

`Navigate`元素表示語音命令會將應用程式啟用至前景。 在此範例中， ```showTripToDestination``` 命令是前景工作。  

`VoiceCommandService`元素表示語音命令會在背景中啟動應用程式。 `Target`這個元素的屬性值應該與 package.appxmanifest 檔案中專案的屬性值相符 `Name` [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 。 在此範例中， `whenIsTripToDestination` 和 `cancelTripToDestination` 命令是背景工作，會將應用程式服務的名稱指定為 `AdventureWorksVoiceCommandService` 。  

如需詳細資訊，請參閱 [**VCD 元素和屬性 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 版參考。  

範例： [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 檔案的一部分，可定義「 `en-us` **艾德作品** 」應用程式的語音命令。  

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

>[!NOTE]
> 語音命令資料不會在應用程式安裝之間保留。 若要確保應用程式的語音命令資料保持不變，請考慮在每次啟動或啟動應用程式時，將您的 VCD 檔案初始化，或維護一個指出是否目前已安裝 VCD 的設定。  

在 `app.xaml.cs` 檔案中：  

1. 加入以下 using 指示詞：

   ```csharp
   using Windows.Storage;
   ```

2. `OnLaunched`使用 async 修飾詞來標記方法。  

   ```csharp
   protected async override void OnLaunched(LaunchActivatedEventArgs e)
   ```  

3. [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)在處理常式中呼叫方法 [`OnLaunched`](/uwp/api/Windows.UI.Xaml.Application) ，以註冊應辨識的語音命令。  
    範例：「艾德作品」應用程式定義 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) 物件。  
    範例：呼叫 [`GetFileAsync`](/uwp/api/Windows.Storage.StorageFolder) 方法來初始化具有檔案的 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) 物件 `AdventureWorksCommands.xml` 。  
    [`StorageFile`](/uwp/api/Windows.Storage.StorageFile)物件接著會傳遞至 [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) 方法。  

   ```csharp
   try {
      // Install the main VCD. 
      StorageFile vcdStorageFile = await Package.Current.InstalledLocation.GetFileAsync(
            @"AdventureWorksCommands.xml"
      );
       
      await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
     
      // Update phrase list.
      ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
      if(locator != null) {
            await locator.TripViewModel.UpdateDestinationPhraseList();
        }
    }
    catch (Exception ex) {
        System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
    }
    ```  

## <a name="handle-activation"></a>處理啟用  

指定您的應用程式如何回應後續的語音命令啟用。

>[!NOTE]
> 您至少必須在安裝了語音命令集之後，啟動您的應用程式一次。  

1. 確認您的應用程式已由語音命令啟用。  

    覆寫 [`Application.OnActivated`](/uwp/api/Windows.UI.Xaml.Application) 事件，並檢查是否 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)。[**種類**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 為 [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。  

2. 判斷命令的名稱以及所說的內容。  

    [`VoiceCommandActivatedEventArgs`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)從 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)取得物件的參考，並查詢 [`Result`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) 物件的屬性 [`SpeechRecognitionResult`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。  

    若要判斷使用者所說的內容，請在字典中檢查已辨識片語的 [**文字**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 值或語義屬性 [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 。  

3. 在您的應用程式中採取適當的動作，例如流覽至所需的頁面。  

    >[!NOTE]
    > 如果您需要參考您的 VCD，請流覽 [編輯 vcd](#edit-the-vcd-file) 檔案一節。

    收到 voice 命令的語音辨識結果之後，您會從陣列中的第一個值取得命令名稱 [`RulePath`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。 因為 VCD 檔案定義了一個以上的語音命令，您必須確認值符合 VCD 中的命令名稱，並採取適當的動作。  

    應用程式最常見的動作是流覽至頁面，其中包含與 voice 命令內容相關的內容。  
    範例：開啟 **TripPage** 頁面，並傳入 voice 命令的值、命令的輸入方式，以及辨識的目的片語 (如果適用) 。 或者，在流覽至 [ **TripPage** ] 頁面時，應用程式可能會將導覽參數傳送至 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。  

    您可以使用 CommandMode 索引鍵，找出啟動您應用程式的語音命令是否真的被說出，或是否以文字的形式輸入 [`SpeechRecognitionSemanticInterpretation.Properties`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 。  該索引鍵的值將會是 `voice` 或 `text` 。 如果索引鍵的值為 `voice` ，請考慮在您的應用程式中使用語音合成 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) ，為使用者提供語音意見反應。  

    您可以使用 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 來找出專案的 `PhraseList` 或條件約束中所說的內容 `PhraseTopic` `ListenFor` 。 字典索引鍵是 `Label` 或元素的屬性值 `PhraseList` `PhraseTopic` 。
    範例：下列程式碼說明如何存取 **{destination}** 片語的值。  

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
    protected override void OnActivated(IActivatedEventArgs args) {
        base.OnActivated(args);
        
        Type navigationToPageType;
        ViewModel.TripVoiceCommand? navigationCommand = null;
        
        // Voice command activation.
        if (args.Kind == ActivationKind.VoiceCommand) {
            // Event args may represent many different activation types. 
            // Cast the args so that you only get useful parameters out.
            var commandArgs = args as VoiceCommandActivatedEventArgs;
            
            Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;
            
            // Get the name of the voice command and the text spoken.
            // See VoiceCommands.xml for supported voice commands.
            string voiceCommandName = speechRecognitionResult.RulePath[0];
            string textSpoken = speechRecognitionResult.Text;
            
            // commandMode indicates whether the command was entered using speech or text.
            // Apps should respect text mode by providing silent (text) feedback.
            string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
            
            switch (voiceCommandName) {
                case "showTripToDestination":
                    // Access the value of {destination} in the voice command.
                    string destination = this.SemanticInterpretation("destination", speechRecognitionResult);
                    
                    // Create a navigation command object to pass to the page.
                    navigationCommand = new ViewModel.TripVoiceCommand(
                        voiceCommandName,
                        commandMode,
                        textSpoken,
                        destination
                    );
              
                    // Set the page to navigate to for this voice command.
                    navigationToPageType = typeof(View.TripDetails);
                    break;
                default:
                    // If not able to determine what page to launch, then go to the default entry point.
                    navigationToPageType = typeof(View.TripListView);
                    break;
            }
        }
        // Protocol activation occurs when a card is selected within Cortana (using a background task).
        else if (args.Kind == ActivationKind.Protocol) {
            // Extract the launch context. In this case, use the destination from the phrase set (passed
            // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
            // identifier is ideal for more complex scenarios. The destination page is left to check if the 
            // destination trip still exists, and navigate back to the trip list if it does not.
            var commandArgs = args as ProtocolActivatedEventArgs;
            Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
            var destination = decoder.GetFirstValueByName("LaunchContext");
            
            navigationCommand = new ViewModel.TripVoiceCommand(
                "protocolLaunch",
                "text",
                "destination",
                destination
            );
            
            navigationToPageType = typeof(View.TripDetails);
        }
        else {
            // If launched using any other mechanism, fall back to the main page view.
            // Otherwise, the app will freeze at a splash screen.
            navigationToPageType = typeof(View.TripListView);
        }
        
        // Repeat the same basic initialization as OnLaunched() above, taking into account whether
        // or not the app is already active.
        Frame rootFrame = Window.Current.Content as Frame;
        
        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active.
        if (rootFrame == null) {
            // Create a frame to act as the navigation context and navigate to the first page.
            rootFrame = new Frame();
            App.NavigationService = new NavigationService(rootFrame);
            
            rootFrame.NavigationFailed += OnNavigationFailed;
            
            // Place the frame in the current window.
            Window.Current.Content = rootFrame;
        }
        
        // Since the expectation is to always show a details page, navigate even if 
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
    private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult) {
        return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
    }
    ```  

## <a name="handle-the-voice-command-in-the-app-service"></a>在 App Service 中處理 Voice 命令  

在 app service 中處理 voice 命令。  

1. 將下列 using 指示詞新增至您的語音命令服務檔案。  
    範例： `AdventureWorksVoiceCommandService.cs`.  

    ```csharp
        using Windows.ApplicationModel.VoiceCommands;
        using Windows.ApplicationModel.Resources.Core;
        using Windows.ApplicationModel.AppService;
    ```  

2. 進行服務延遲，讓您的 app service 不會在處理 voice 命令時終止。  
3. 確認您的背景工作正在以語音命令啟動的 app service 的形式執行。  
    1. 將 [**IBackgroundTaskInstance TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 轉換為 [**ApplicationModel. AppService. AppServiceTriggerDetails**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceTriggerDetails)。  
    2. 檢查 [**IBackgroundTaskInstance.TriggerDetails.Name**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 是否為檔案中的 app service 名稱 `Package.appxmanifest` 。  
4. 使用 [**IBackgroundTaskInstance TriggerDetails**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)來建立 **Cortana** 的 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) ，以取得 voice 命令。
5. 註冊 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)的事件處理常式。  [**VoiceCommandCompleted**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) ，以在應用程式服務因使用者取消而關閉時收到通知。  
6. 註冊 IBackgroundTaskInstance 的事件處理常式。當 app service 因為未預期的失敗而關閉時， [**已取消**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 會收到通知。  
7. 判斷命令的名稱以及所說的內容。  
    1. 使用 [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand)。[**CommandName**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand) 屬性，用來判斷語音命令的名稱。  
    2. 若要判斷使用者所說的內容，請在字典中檢查已辨識片語的 [**文字**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 值或語義屬性 [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 。  
8. 在您的 app service 中採取適當的動作。  
9. 使用 **Cortana** 顯示意見反應，並對語音命令說出意見。  
    1. 判斷您希望 **Cortana** 顯示的字串，並與使用者交談以回應語音命令，並建立 [`VoiceCommandResponse`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 物件。 如需如何選取 **cortana** 顯示和演講的意見反應字串的指引，請參閱 [cortana 設計指導方針](cortana-design-guidelines.md)。  
    2. 使用 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)實例，藉由呼叫 [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)或 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)與物件，來向 **Cortana** 報告進度或完成 `VoiceCommandServiceConnection` 。  

    >[!NOTE]
    > 如果您需要參考您的 VCD，請流覽 [編輯 vcd](#edit-the-vcd-file) 檔案一節。  

    ```csharp
    public sealed class VoiceCommandService : IBackgroundTask {
        private BackgroundTaskDeferral serviceDeferral;
        VoiceCommandServiceConnection voiceServiceConnection;
        
        public async void Run(IBackgroundTaskInstance taskInstance) {
            //Take a service deferral so the service isn&#39;t terminated.
            this.serviceDeferral = taskInstance.GetDeferral();
            
            taskInstance.Canceled += OnTaskCanceled;
            
            var triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    
            if (triggerDetails != null &amp;&amp; 
                triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint") {
                try {
                    voiceServiceConnection = 
                    VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                        triggerDetails);
                    voiceServiceConnection.VoiceCommandCompleted += 
                    VoiceCommandCompleted;
                    
                    VoiceCommand voiceCommand = await 
                    voiceServiceConnection.GetVoiceCommandAsync();
                    
                    switch (voiceCommand.CommandName) {
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
                finally {
                    if (this.serviceDeferral != null) {
                        // Complete the service deferral.
                        this.serviceDeferral.Complete();
                    }
                }
            }
        }
        
        private void VoiceCommandCompleted(VoiceCommandServiceConnection sender,
            VoiceCommandCompletedEventArgs args) {
            if (this.serviceDeferral != null) {
                // Insert your code here.
                // Complete the service deferral.
                this.serviceDeferral.Complete();
            }
        }
        
        private async void SendCompletionMessageForDestination(
            string destination) {
            // Take action and determine when the next trip to destination
            // Insert code here.
            
            // Replace the hardcoded strings used here with strings 
            // appropriate for your application.
            
            // First, create the VoiceCommandUserMessage with the strings 
            // that Cortana will show and speak.
            var userMessage = new VoiceCommandUserMessage();
            userMessage.DisplayMessage = "Here's your trip.";
            userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";
            
            // Optionally, present visual information about the answer.
            // For this example, create a VoiceCommandContentTile with an 
            // icon and a string.
            var destinationsContentTiles = new List<VoiceCommandContentTile>();
            
            var destinationTile = new VoiceCommandContentTile();
            destinationTile.ContentTileType = 
                VoiceCommandContentTileType.TitleWith68x68IconAndText;
            // The user taps on the visual content to launch the app. 
            // Pass in a launch argument to enable the app to deep link to a 
            // page relevant to the item displayed on the content tile.
            destinationTile.AppLaunchArgument = 
                string.Format("destination={0}", "Las Vegas");
            destinationTile.Title = "Las Vegas";
            destinationTile.TextLine1 = "August 3rd 2015";
            destinationsContentTiles.Add(destinationTile);
            
            // Create the VoiceCommandResponse from the userMessage and list    
            // of content tiles.
            var response = VoiceCommandResponse.CreateResponse(
                userMessage, destinationsContentTiles);
            
            // Cortana displays a "Go to app_name" link that the user 
            // taps to launch the app. 
            // Pass in a launch to enable the app to deep link to a page 
            // relevant to the voice command.
            response.AppLaunchArgument = string.Format(
                "destination={0}", "Las Vegas");
            
            // Ask Cortana to display the user message and content tile and 
            // also speak the user message.
            await voiceServiceConnection.ReportSuccessAsync(response);
        }
        
        private async void LaunchAppInForeground() {
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

啟用之後，app service 會有0.5 秒來呼叫 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)。 **Cortana** 會顯示並指出意見字串。  

>[!NOTE]
> 您可以在 VCD 檔案中宣告 **意見** 字串。 此字串不會影響 cortana 畫布上顯示的 UI 文字，只會影響 **cortana** 說出的文字。  

如果應用程式所花的時間超過0.5 秒進行呼叫， **Cortana** 就會插入一個手勢畫面，如下所示。 **Cortana** 會在應用程式呼叫 **ReportSuccessAsync** 或最長5秒的時間內顯示出手的畫面。 如果 app service 未呼叫 **ReportSuccessAsync**，或任何 [`VoiceCommandServiceConnection`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 提供 **Cortana** 與資訊的方法，則使用者會收到錯誤訊息，且會取消 app service。  

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 的螢幕擷取畫面，以及在背景中使用 AdventureWorks 應用程式之進度和結果畫面的基本查詢":::

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [透過 Cortana 語音命令啟用前景應用程式](cortana-launch-a-foreground-app-with-voice-commands.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)