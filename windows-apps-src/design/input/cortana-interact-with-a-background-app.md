---
title: 與 Cortana 中的背景應用程式互動-Cortana UWP 設計與開發
description: 執行語音命令時，透過 Cortana 畫布中的語音和文字輸入，讓使用者與背景應用程式互動。
ms.assetid: e42917dc-aece-4880-813f-80b897f9126c
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 835a2f60d2b86e5bef49195d4f937fa844f4d921
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606073"
---
# <a name="interact-with-a-background-app-in-cortana"></a>利用 Cortana 與背景應用程式互動

>[!WARNING]
> 這項功能已不再支援，因為 Windows 10 2020 版更新 (2004 版（codename "20H1" ) ）。
>
> 請參閱 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

執行語音命令時，透過 **Cortana** 畫布中的語音和文字輸入，讓使用者與背景應用程式互動。

> [!NOTE]
> **重要 API**
>
> - [**ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和屬性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Cortana 支援使用您的應用程式完成輪流工作流程。 此工作流程是由您的應用程式所定義，而且可以支援下列功能： 

- 成功完成
- 交付
- 進度
- 確認
- 去除混淆
- 錯誤

## <a name="composing-feedback-strings"></a>撰寫意見字串

> [!TIP]
> **先決條件**
>
> 如果您是開發通用 Windows 平台 (UWP) App 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。
>
> - [建立您的第一個應用程式](/windows/uwp/get-started/your-first-app)
> - 請參閱[事件與路由事件概觀](/windows/uwp/xaml-platform/events-and-routed-events-overview)，以了解事件相關資訊
>
> **使用者經驗指導方針**
>
> 請參閱 [cortana 設計指導方針](cortana-design-guidelines.md)  ，以瞭解如何整合您的應用程式與 **Cortana** 和 [語音互動](speech-interactions.md) ，以取得設計實用且具備語音功能的應用程式的實用秘訣。

撰寫 **Cortana** 顯示及讀出的意見反應字串。

[Cortana 設計指導方針](cortana-design-guidelines.md)提供撰寫 **Cortana** 字串的建議。

內容卡片可以為使用者提供額外的內容，並協助您將意見字串保持簡潔。

**Cortana** 支援下列內容卡片範本 (在完成畫面) 只能使用一個範本：

- 僅標題
- 具有最多三行文字的標題
- 具有影像的標題
- 具有影像的標題和最多三行文字

映射可以是：

- 68w x 68h
- 68w x 92h
- 280w x 140h

您也可以讓使用者在前景啟動您的應用程式，方法是按一下卡片或您應用程式的文字連結。

## <a name="completion-screen"></a>完成畫面

完成畫面可為使用者提供已完成之語音命令工作的相關資訊。

您的應用程式所需的時間不超過500毫秒，且使用者不需要額外的資訊，也不會與  **Cortana** 進行進一步的互動。 Cortana 只會顯示完成畫面。

在這裡，我們會使用「 **艾德作品** 」應用程式來顯示語音命令要求的完成畫面，以顯示即將前往倫敦的電話。

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="針對即將進行的旅程進行 Cortana 背景應用程式完成的螢幕擷取畫面":::

語音命令是在 AdventureWorksCommands.xml 中定義：

```xml
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs 包含完成訊息方法：

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination, expected to be in the phrase list.</param>
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

## <a name="hand-off-screen"></a>手形畫面

語音命令一旦辨識之後， **cortana** 就必須呼叫 ReportSuccessAsync 並提出意見反應，大約 500If app service 無法完成500毫秒內 voice 命令所指定的動作， **Cortana** 呈現的畫面會顯示，直到您的應用程式呼叫 ReportSuccessAsync 或最多5秒為止。

如果 app service 未呼叫 ReportSuccessAsync 或任何其他 VoiceCommandServiceConnection 方法，則使用者會收到錯誤訊息，且會取消 app service 呼叫。

以下是「 **艾德作品** 」應用程式的手形畫面範例。 在此範例中，使用者已查詢 **Cortana** 以進行近期的旅程。 [退出] 畫面包含以 app service 名稱、圖示和 **意見** 字串自訂的訊息。 

[!NOTE] 您可以在 VCD 檔案中宣告 **意見** 字串。 此字串不會影響 cortana 畫布上顯示的 UI 文字，只會影響 **cortana** 說出的文字。

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 背景應用程式手寫畫面的螢幕擷取畫面":::

## <a name="progress-screen"></a>進度畫面

如果 app service 需要500毫秒來呼叫 ReportSuccessAsync， **Cortana** 會提供使用者一個進度畫面。 應用程式圖示隨即顯示，而您必須同時提供 GUI 和 TTS 進度字串，以指出正在主動處理工作。

**Cortana** 會顯示最多5秒的進度畫面。 5秒之後， **Cortana** 會向使用者呈現錯誤訊息，並關閉 app service。 如果 app service 需要5秒以上才能完成此動作，它可以繼續以進度畫面更新 **Cortana** 。

以下是「 **艾德作品** 」應用程式的進度畫面範例。 在此範例中，使用者已取消了拉斯維加斯的旅程。 進度畫面包含針對動作自訂的訊息、圖示，以及包含所要取消行程相關資訊的內容磚。

:::image type="content" source="images/cortana/cortana-progress-screen.png" alt-text="使用背景應用程式進度畫面顯示 Cortana 的螢幕擷取畫面":::

AdventureWorksVoiceCommandService.cs 包含下列進度訊息方法，它會呼叫 [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 來顯示 **Cortana** 中的進度畫面。

```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <a name="confirmation-screen"></a>確認畫面

當語音命令指定的動作無法復原時，會造成重大影響，或辨識信賴度不高，因此 app service 可以要求確認。

以下是「 **艾德作品** 」應用程式的確認畫面範例。 在此範例中，使用者已指示 app service 透過 **Cortana** 取消前往拉斯維加斯的旅程。 App service 為 **Cortana** 提供了確認畫面，在取消行程之前，會提示使用者輸入 yes 或 no 解答。

如果使用者說的是「是」或「否」， **Cortana** 就無法判斷問題的答案。 在此情況下， **Cortana** 會以 app service 所提供的類似問題提示使用者。

第二次提示時，如果使用者仍未顯示「是」或「否」， **Cortana** 會第三次提示使用者第三次，且前面有一個道歉啟事的相同問題。 如果使用者仍然沒有說「是」或「否」， **Cortana** 就會停止接聽語音輸入，並要求使用者改為按下其中一個按鈕。

確認畫面包含針對動作所自訂的訊息、圖示，以及包含所要取消行程相關資訊的內容磚。

:::image type="content" source="images/cortana/cortana-confirmation-screen.png" alt-text="Cortana 與背景應用程式確認畫面的螢幕擷取畫面":::

AdventureWorksVoiceCommandService.cs 包含下列行程取消方法，它會呼叫 [**RequestConfirmationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 來顯示 **Cortana** 中的確認畫面。

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <a name="disambiguation-screen"></a>消除混淆畫面

當語音命令指定的動作有一個以上可能的結果時，app service 可以向使用者要求更多的資訊。

以下是「 **艾德作品** 」應用程式的混淆畫面範例。 在此範例中，使用者已指示 app service 透過 **Cortana** 取消前往拉斯維加斯的旅程。 不過，使用者有兩次在不同日期的拉斯維加斯，而 app service 無法在使用者選取想要的行程的情況下完成動作。

App service 會為 **Cortana** 提供混淆畫面，提示使用者在取消任何之前，從相符的行程清單中進行選取。

在此情況下， **Cortana** 會以 app service 所提供的類似問題提示使用者。

第二次提示時，如果使用者仍未說出可以用來識別選取專案的專案， **Cortana** 會第三次提示使用者第三次，且前面有一個道歉啟事的相同問題。 如果使用者仍未說出可以用來識別選取專案的專案， **Cortana** 就會停止接聽語音輸入，並要求使用者改為按下其中一個按鈕。

消除混淆畫面包含針對動作自訂的訊息、圖示，以及包含所要取消行程相關資訊的內容磚。

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="使用背景應用程式去除混淆畫面的 Cortana 螢幕擷取畫面":::

AdventureWorksVoiceCommandService.cs 包含下列行程取消方法，它會呼叫 [**RequestDisambiguationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 來顯示 **Cortana** 中的去除混淆畫面。

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <a name="error-screen"></a>錯誤畫面

當語音命令指定的動作無法完成時，app service 可以提供錯誤畫面。

以下是「 **艾德作品** 」應用程式的錯誤畫面範例。 在此範例中，使用者已指示 app service 透過 **Cortana** 取消前往拉斯維加斯的旅程。 不過，使用者沒有排定到拉斯維加斯的任何旅程。

App service 為 **Cortana** 提供的錯誤畫面包含了針對動作自訂的訊息、圖示，以及特定的錯誤訊息。

呼叫 [**ReportFailureAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 以顯示 **Cortana** 中的錯誤畫面。

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)
