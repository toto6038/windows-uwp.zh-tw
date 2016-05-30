---
author: Karl-Bridge-Microsoft
Description: 了解使用者如何在執行語音時，透過 Cortana 語音和畫布與背景應用程式互動。
title: 與背景應用程式互動
ms.assetid: 6C60F03C-A242-435D-96BB-736892CC1CA6
label: Interact with a background app
template: detail.hbs
---

# 利用 Cortana 與背景 App 互動

執行語音命令時，透過 **Cortana** 畫布中的語音和文字輸入，來啟用與背景 App 的使用者互動。



**重要 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**語音命令定義 (VCD) 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Cortana 針對您的 App 支援完整的轉向建議導航工作流程。 此工作流程是由您的應用程式所定義，而且可以支援下列這類功能︰ 

-   順利完成
-   遞交
-   進度
-   確認
-   解釋清楚
-   錯誤

**先決條件：**

本主題改編自[利用 Cortana 語音命令啟動背景應用程式](launch-a-background-app-with-voice-commands-in-cortana.md)。 我們在這裡以 **Adventure Works** 這個行程安排和管理 app，繼續示範各項功能。

如果您是開發通用 Windows 平台 (UWP) app 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：  **

請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)取得相關資訊，了解如何將您的 App 與 **Cortana** 整合。您也可以參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)，取得有關設計既實用又吸引人且支援語音之 App 的有用提示。

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>回覆字串

撰寫要透過 **Cortana** 顯示以及說出的回覆字串。

[Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)提供撰寫 **Cortana** 字串的建議。

## <span id="Feedback_strings"></span><span id="feedback_strings"></span><span id="FEEDBACK_STRINGS"></span>回覆字串

內容卡可以為使用者提供其他內容，並協助保持回覆字串的簡潔。

**Cortana** 支援下列的內容卡範本 (只有一個範本可以用於完成畫面)：

    -   Title only
    -   Title with up to three lines of text
    -   Title with image
    -   Title with image and up to three lines of text

映像可以是：

    -   68w x 68h
    -   68w x 92h
    -   280w x 140h

您也可以讓使用者按一下卡或 App 的文字連結，即可在前景啟動 App。

## <span id="Completion_screen"></span><span id="completion_screen"></span><span id="COMPLETION_SCREEN"></span>完成畫面

使用者可以從完成畫面中了解完成的語音命令工作。

應用程式在工作回應上花費的時間小於 500 毫秒，不需要其他的使用者資訊與 **Cortana** 的進一步互動即可完成。 Cortana 只會顯示完成畫面。

我們在這裡使用 **Adventure Works** 應用程式，來顯示可顯示即將前往倫敦的行程的語音命令要求的完成畫面。 

![Cortana 背景應用程式完成畫面](images/cortana-completion-screen-upcomingtrip-small.png)

語音命令定義在 AdventureWorksCommands.xml 中︰
```
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

## <span id="Hand-off_screen"></span><span id="hand-off_screen"></span><span id="HAND-OFF_SCREEN"></span>遞交畫面

語音命令辨識成功後，**Cortana** 必須呼叫 ReportSuccessAsync，並在 500 毫秒左右的時間內提供回覆內容。 如果應用程式服務無法在 500 毫秒內完成語音命令指定的動作，**Cortana** 就會在應用程式呼叫 ReportSuccessAsync 之前顯示一個遞交畫面 (最長 5 秒)。

如果應用程式服務未呼叫 ReportSuccessAsync 或任何其他 VoiceCommandServiceConnection 方法，則使用者會收到錯誤訊息，然後取消應用程式服務呼叫。

以下是 **Adventure Works** 應用程式的遞交畫面範例。 在這個範例中，使用者已向 **Cortana** 查詢即將出發的旅遊。 遞交畫面有一則訊息特別標上利用 app 服務名稱，以及在 VCD 檔案中宣告的 **Feedback** 字串。

![Cortana 背景 App 遞交畫面](images/cortana-backgroundapp-progress-result.png)


## <span id="Progress_screen"></span><span id="progress_screen"></span><span id="PROGRESS_SCREEN"></span>進度畫面


如果 App 服務呼叫 ReportSuccessAsync 超過 500 毫秒，**Cortana** 就會向使用者提供進度畫面。 應用程式圖示將會顯示，而您必須同時提供 GUI 與 TTS 進度字串，指出正在主動處理工作。

**Cortana** 顯示一個最多 5 秒鐘的進度畫面。 5 秒鐘之後，**Cortana** 會顯示錯誤訊息，然後結束 app 服務。 如果應用程式服務需要超過 5 秒的時間才能完成此動作，它可以繼續更新 **Cortana** 的進度畫面。

以下是 **Adventure Works** 應用程式的進度畫面範例。 在這個範例中，使用者已取消前往拉斯維加斯的行程。 進度畫面有一個為專門為這個動作設計的訊息、一個圖示，還有一個內容磚提供目前取消中的行程相關資訊。

![Cortana 背景應用程式進度畫面 ](images/cortana-progress-screen.png)

AdventureWorksVoiceCommandService.cs 包含下列進度訊息方法，可呼叫 [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) 以在 **Cortana** 中顯示進度畫面。


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

## <span id="Confirmation_screen"></span><span id="confirmation_screen"></span><span id="CONFIRMATION_SCREEN"></span>確認畫面


當語音命令所指定的動作為無法復原、會造成顯著影響，或是辨識可信度不高時，App 服務可以要求進行確認。

以下是 **Adventure Works** app 的確認畫面範例。 在這個範例中，使用者已透過 **Cortana** 指示 app 服務取消前往拉斯維加斯的行程。 app 服務已經提供 **Cortana** 並附帶一個確認畫面，在取消行動之前提示使用者輸入 yes 或 no。

如果使用者回答的不是 "Yes" 或 "No"，**Cortana** 便無法判斷問題的答案。 在這種情況下，**Cortana** 提示使用者回答 app 服務提供的類似問題。

在第二個提示中，如果使用者說的仍然不是 "Yes" 或 "No"，**Cortana** 會向使用者第三次提示相同的問題，而且前面會加上道歉。 如果使用者說的依舊不是 "Yes" 或 "No"，**Cortana** 會停止接聽語音輸入，然後要求使用者點選其中一個按鈕。

確認畫面有一個為專門為這個動作設計的訊息、一個圖示，還有一個內容磚提供目前取消中的行程相關資訊。

![Cortana 背景應用程式確認畫面](images/cortana-confirmation-screen.png)

AdventureWorksVoiceCommandService.cs 包含下列取消行程方法，這些方法會呼叫 [**RequestConfirmationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706582) 以在 **Cortana** 中顯示確認畫面。

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

## <span id="Disambiguation_screen"></span><span id="disambiguation_screen"></span><span id="DISAMBIGUATION_SCREEN"></span>解釋清楚畫面


當語音命令指定的的動作有多個可能的結果時，App 服務會要求使用者提供更詳細的資訊。

以下是 **Adventure Works** app 的解釋清楚畫面範例。 在這個範例中，使用者已透過 **Cortana** 指示 app 服務取消前往拉斯維加斯的行程。 不過，使用者在不同的日期會分別兩次飛往拉斯維加斯，但是如果使用者不選取希望的行程，app 服務便無法完成此動作。

App 服務為 **Cortana** 提供一個解釋清楚畫面，提示使用者從相符的行程清單進行選擇，然後取消不合適的行程。

在這種情況下，**Cortana** 提示使用者回答 app 服務提供的類似問題。

在第二個提示中，如果使用者提供的答案仍然無法判別正確的選擇，**Cortana** 會向使用者第三次提示相同的問題，而且前面會加上道歉。 如果使用者提供的答案還是無法判別正確的選擇，**Cortana** 會停止接聽語音輸入，然後要求使用者點選其中一個按鈕。

解釋清楚畫面有一個為專門為這個動作設計的訊息、一個圖示，還有一個內容磚提供目前取消中的行程相關資訊。

![Cortana 背景應用程式解釋清單畫面 ](images/cortana-disambiguation-screen.png)

AdventureWorksVoiceCommandService.cs 包含下列取消行程方法，這些方法會呼叫 [**RequestDisambiguationAsync**](https://msdn.microsoft.com/library/windows/apps/dn706583) 以在 **Cortana** 中顯示解釋清楚畫面。

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

## <span id="Error_screen"></span><span id="error_screen"></span><span id="ERROR_SCREEN"></span>錯誤畫面


當無法完成語音命令指定的動作時，App 服務可以提供錯誤畫面。

以下是 **Adventure Works** app 的錯誤畫面範例 。 在這個範例中，使用者已透過 **Cortana** 指示 app 服務取消前往拉斯維加斯的行程。 不過，使用者沒有任何前往拉斯維加斯的行程。

應用程式服務向 **Cortana** 提供一個錯誤畫面，其中包含專門為該動作設計的訊息、一個 圖示以及具體的錯誤訊息。

呼叫 [**ReportFailureAsync**](https://msdn.microsoft.com/library/windows/apps/dn706578)，然後在 **Cortana** 中顯示錯誤畫面。

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <span id="related_topics"></span>相關文章


**開發人員**
* [Cortana 互動](cortana-interactions.md)
* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**設計人員**
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


