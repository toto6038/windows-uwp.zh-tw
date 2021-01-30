---
title: 從 Cortana 中的背景應用程式到前景應用程式的深層連結-Cortana UWP 設計與開發
description: 從 **Cortana** 中的背景應用程式提供深層連結，以在特定狀態或內容中啟動應用程式至前景。
ms.assetid: 6fe5fcc5-9ee4-4c04-92f4-7b1bf7ef5651
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 5d0f841ed653adac6770089a9ec9c1be0335e346
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057777"
---
# <a name="deep-link-from-a-background-app-in-cortana-to-a-foreground-app"></a>從 Cortana 中的背景應用程式到前景應用程式的深層連結

>[!WARNING]
> 這項功能已不再支援，因為 Windows 10 2020 版更新 (2004 版（codename "20H1" ) ）。

從 **Cortana** 中的背景應用程式提供深層連結，以在特定狀態或內容中啟動應用程式至前景。

> [!NOTE]
> 當前景應用程式啟動時， **Cortana** 和背景應用程式服務都會終止。

**Cortana** 完成畫面預設會顯示深層連結，如下所示 ( [移至 AdventureWorks] ) ，但您可以在其他畫面上顯示深層連結。

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="針對即將進行的旅程進行 Cortana 背景應用程式完成的螢幕擷取畫面":::

> [!NOTE]
> **重要 API**
>
> - [**ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和屬性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

## <a name="overview"></a>概觀

使用者可以透過 **Cortana** 存取您的應用程式，方法如下：

- 將其啟用為前景應用程式 (請參閱 [透過 Cortana) 以語音命令啟用前景應用程式](cortana-launch-a-foreground-app-with-voice-commands.md) 。
- 將特定功能公開為背景 app service (請參閱 [使用語音命令在 Cortana 中啟動背景應用程式](cortana-launch-a-background-app-with-voice-commands.md)) 。
- 深層連結至特定頁面、內容和狀態或內容。

我們將在這裡討論深層連結。

當 Cortana 和您的 app service 做為您的全功能應用程式的閘道時，深層連結會很有用 (而不需要使用者透過 [開始] 功能表) 啟動應用程式，或在您的應用程式內提供無法透過 Cortana 存取更豐富的詳細資料和功能。 深層連結是增加可用性及推廣應用程式的另一種方式。

有三種方式可以提供深層連結：

- &lt;各種 Cortana 畫面上的 [移至應用程式 &gt; ] 連結。 
- 內嵌于各種 **Cortana** 畫面上內容磚的連結。
- 以程式設計方式從背景 app service 啟動前景應用程式。

## <a name="go-to-ltappgt-deep-link"></a>「移至 &lt; 應用程式」 &gt; 深層連結

**Cortana** 會 &lt; &gt; 在大部分畫面上的內容卡片下方顯示「移至應用程式」深層連結。

:::image type="content" source="images/cortana/cortana-completion-screen.png" alt-text="螢幕擷取畫面：背景應用程式完成畫面上 Cortana 的 [移至應用程式] 深層連結。":::

您可以為此連結提供啟動引數，以在與 app service 類似的內容中開啟您的應用程式。 如果您未提供啟動引數，則會將應用程式啟動至主畫面。

在此範例中，我們會 **將指定的目的地** (`destination`) 字串傳遞至 SendCompletionMessageForDestination 方法，以抓取所有相符的行程，並提供應用程式的深層連結。

首先，我們會建立一個 [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) (```userMessage```) ，它是由 **cortana** 說出，並顯示在 **cortana** 畫布上。 接著會建立 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) 清單物件，以便在畫布上顯示結果卡的集合。

這兩個物件接著會傳遞至 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)物件的 [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)方法 (`response`) 。 接著，我們會將回應物件的 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 屬性值設定為此函式的 `destination` passsed 值。 當使用者在 Cortana 畫布上按下內容圖格時，會透過回應物件將參數值傳遞至應用程式。

最後，我們會呼叫 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)的 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)方法。

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

## <a name="content-tile-deep-link"></a>內容磚深層連結

您可以將深層連結新增至各種 **Cortana** 畫面上的內容卡。

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中使用 adventureworks 即將推出":::的「使用 adventureworks 的往返」背景應用程式流程，並透過 *遞交畫面推出 Adventureworks 「即將推出*」

如同 [移至 &lt; 應用程式 &gt; ] 連結，您可以提供啟動引數來開啟您的應用程式，其內容與 app service 類似。 如果您未提供啟動引數，內容磚就不會連結到您的應用程式。

在此範例中，從 **AdventureWorks** 範例的 AdventureWorksVoiceCommandService.cs，我們會將指定的目的地傳遞給 SendCompletionMessageForDestination 方法，以抓取所有相符的行程，並提供具有應用程式深層連結的內容卡。

首先，我們會建立一個  [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) (```userMessage```) ，它是由 **cortana** 說出，並顯示在 **cortana** 畫布上。 接著會建立 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) 清單物件，以便在畫布上顯示結果卡的集合。 

這兩個物件接著會傳遞至 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)物件的 [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)方法 (```response```) 。 接著，我們會將 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 屬性值設定為 voice 命令中的目的地值。

最後，我們會呼叫 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)的 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)方法。
在這裡，我們會將具有不同 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)參數值的兩個內容磚新增至 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)物件的 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)呼叫中所使用的 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)清單。

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

## <a name="programmatic-deep-link"></a>程式設計深層連結

您也可以使用啟動引數以程式設計方式啟動您的應用程式，以開啟應用程式，其內容與 app service 類似。 如果您未提供啟動引數，則會將應用程式啟動至主畫面。

在這裡，我們會將值為 "拉斯維加斯" 的 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)參數新增至 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)物件的 [**RequestAppLaunchAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)呼叫中所使用的 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)物件。

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <a name="app-manifest"></a>應用程式資訊清單

若要啟用應用程式的深層連結，您必須 `windows.personalAssistantLaunch` 在應用程式專案的 package.appxmanifest 檔案中宣告此延伸模組。

在這裡，我們宣告了「 `windows.personalAssistantLaunch` **艾德作品** 」應用程式的擴充功能。

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <a name="protocol-contract"></a>通訊協定合約

您的應用程式會使用 [**通訊協定**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 合約，透過統一資源識別項 (URI) 啟用來啟動至前景。 您的應用程式必須覆寫應用程式的 [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application)事件，並檢查是否有 **通訊協定** **ActivationKind** 。 如需詳細資訊，請參閱 [控制碼 URI 啟用](/windows/uwp/launch-resume/handle-uri-activation)。

在這裡，我們會將 [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) 提供的 URI 解碼，以存取啟動引數。 在此範例中， [**Uri**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) 會設定為 "personalassistantlaunch：？LaunchCoNtext = 拉斯維加斯（拉斯維加斯）」。

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

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)
