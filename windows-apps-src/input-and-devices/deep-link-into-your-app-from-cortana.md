---
Description: 在 Cortana 中提供來自背景應用程式服務的深層連結，以便將應用程式啟動至處於特定狀態或內容的前景。
title: 從 Cortana 到背景應用程式的深層連結
ms.assetid: BE811A87-8821-476A-90E4-2E20D37E4043
label: Deep link to a background app
template: detail.hbs
---

# 從 Cortana 到背景應用程式的深層連結


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**語音命令定義 (VCD) 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

在 **Cortana** 中提供來自背景 app 的深層連結，以將 app 啟動至處於特定狀態或內容的前景。

> **注意**  
**Cortana** 與背景應用程式服務會在啟動前景應用程式時終止。

深層連結預設會顯示於此處所示的 **Cortana** 完成畫面 ("Go to AdventureWorks")，但您可在其他各個畫面上顯示深層連結。 

![Cortana 背景 app 完成畫面](images/cortana-completion-screen-upcomingtrip-small.png)

**先決條件：**

本主題位於[利用 Cortana 與背景應用程式互動](interact-with-a-background-app-in-cortana.md)中。 我們將繼續使用規劃與管理應用程式 (名為 **Adventure Works**) 的行程來示範各種 **Cortana** 功能。

如果您是開發通用 Windows 平台 (UWP) 應用程式的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個應用程式](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：**

請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)取得相關資訊，來了解如何將您的應用程式與 **Cortana** 整合，您也可以參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)，來取得有關設計既實用又吸引人且支援語音之應用程式的有用提示。

## <span id="Overview"> </span> <span id="overview"> </span> <span id="OVERVIEW"> </span>概觀


使用者可以使用下列方式，透過 **Cortana** 來存取您的應用程式：

-   將其啟用為前景 app (請參閱[利用 Cortana 語音命令啟用前景 app](launch-a-foreground-app-with-voice-commands-in-cortana.md))。
-   將特定功能公開為背景 app 服務 (請參閱 [利用 Cortana 語音命令啟用背景 app](launch-a-background-app-with-voice-commands-in-cortana.md))。
-   深層連結到特定頁面、內容，以及狀態或內容。

我們將在此處討論深層連結。

當 Cortana 和您的 app 服務是完整功能 app 的閘道 (而不需使用者透過 [開始] 功能表來啟動 app) 時，或者可用來存取您 app 內無法透過 Cortana 存取之更豐富的詳細資料與功能時，深層連結會非常有用。 深層連結是提高可用性和存取您 app 的另一種方式。

有三種方式可以提供深層連結：

-   位於各種 **Cortana** 畫面上的 [移至 &lt;app&gt;] 連結。
-   內嵌於各種 **Cortana** 畫面上之內容磚的連結。
-   透過程式設計方式，從背景 app 服務啟動前景 app。

## <span id="Go_to__app__deep_link"> </span> <span id="go_to__app__deep_link"> </span> <span id="GO_TO__APP__DEEP_LINK"> </span>「移至 &lt;app&gt;」深層連結


**Cortana** 會在大部分畫面的內容卡下方，顯示「移至 &lt;app&gt;」深層連結。

![Cortana 背景 app 完成畫面](images/cortana-completion-screen.png)

您可提供此連結的啟動引數，利用與 app 服務類似的環境來開啟您的 app。 如果您未提供啟動引數，app 就會啟動到主畫面。

在此示例的 AdventureWorksVoiceCommandService.cs **AdventureWorks** 範例中，我們會傳送指定目的地至 SendCompletionMessageForDestination 方法，其會擷取所有相符的行程，並提供 app 的深層連結。

首先，我們會建立 [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```)，其是由 **Cortana** 說出並顯示於 **Cortana** 畫布。 接著會建立 [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) 清單物件，以在畫布上顯示結果卡的集合。 

這兩個物件接著會傳送至 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 物件的 [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) 方法 (```response```)。 接著我們會將 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 屬性值，設為語音命令中的目的地值。

最後，我們會呼叫 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 的 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 方法 。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```


## <span id="Content_tile_deep_link"> </span> <span id="content_tile_deep_link"> </span> <span id="CONTENT_TILE_DEEP_LINK"> </span>內容磚深層連結


您可在各個 **Cortana** 畫面上，新增深層連結至內容卡。

![Cortana 背景 app 遞交畫面 ](images/cortana-backgroundapp-progress-result.png)

與「移至 &lt;app&gt;」連結類似，您可以提供啟動引數，利用與 app 服務類似的環境來開啟您的 app。 如果您未提供啟動引數，內容磚就不會連結到您的 app。

在此示例之 **AdventureWorks** 範例的 AdventureWorksVoiceCommandService.cs 中，我們會傳送指定目的地至 SendCompletionMessageForDestination 方法，其會擷取所有相符的行程，並為內容卡提供 app 的深層連結。

首先，我們會建立 [**VoiceCommandUserMessage**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandusermessage.aspx) (```userMessage```)，其是由 **Cortana** 說出並顯示於 **Cortana** 畫布。 接著會建立 [**VoiceCommandContentTile**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttile.aspx) 清單物件，以在畫布上顯示結果卡的集合。 

這兩個物件接著會傳送至 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 物件的 [CreateResponse](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandresponse.createresponse.aspx) 方法 (```response```)。 接著我們會將 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 屬性值，設為語音命令中的目的地值。

最後，我們會呼叫 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 的 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 方法 。
我們在此處將兩個具有不同 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 參數值的內容磚新增到 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 物件的 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580) 呼叫中所使用的 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168) 清單。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```
## <span id="Programmatic_deep_link"> </span> <span id="programmatic_deep_link"> </span> <span id="PROGRAMMATIC_DEEP_LINK"> </span>透過程式設計方式的深層連結


您也可以利用程式設計的方式，使用啟動引數來啟動應用程式，利用類似的內容來開啟您的應用程式以做為應用程式服務。 如果您未提供啟動引數，app 就會啟動到主畫面。

現在，將值為 "Las Vegas" 的 [**AppLaunchArgument**](https://msdn.microsoft.com/library/windows/apps/dn974183) 參數新增到 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 物件之 [**RequestAppLaunchAsync**](https://msdn.microsoft.com/library/windows/apps/dn706581) 呼叫中所使用的 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 物件。

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <span id="App_manifest"> </span> <span id="app_manifest"> </span> <span id="APP_MANIFEST"> </span>應用程式資訊清單


若要啟用應用程式的深層連結，您必須在應用程式專案的 Package.appxmanifest 檔案中宣告 `windows.personalAssistantLaunch` 延伸。

我們在此處針對 **Adventure Works**應用程式宣告 `windows.personalAssistantLaunch` 延伸。

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <span id="Protocol_contract"> </span> <span id="protocol_contract"> </span> <span id="PROTOCOL_CONTRACT"> </span>通訊協定的協定


您的應用程式是使用 [**Protocol**](https://msdn.microsoft.com/library/windows/apps/br224693) 協定，透過統一資源識別項 (URI) 的啟用，啟動到前景。 您的應用程式必須覆寫應用程式的 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件，並檢查 **Protocol** 的 **ActivationKind**。 如需詳細資訊，請參閱[處理 URI 啟用](https://msdn.microsoft.com/library/windows/apps/mt228339)。

我們在此處針對 [**ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742) 所提供的 URI 進行解碼，以存取啟動引數。 在此範例中，[**Uri**](https://msdn.microsoft.com/library/windows/apps/br224746) 是設定為 "windows.personalassistantlaunch:?LaunchContext=Las Vegas"。

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <span id="related_topics"> </span>相關文章


**開發人員**
* [Cortana 互動](cortana-interactions.md)
* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**設計人員**
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


