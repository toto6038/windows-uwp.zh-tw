---
author: TylerMSFT
title: 繼續使用者活動，甚至是在各個裝置之間
description: 本主題描述如何協助使用者繼續使用您的應用程式執行作業，甚至是在多個裝置之間作業。
keywords: user activity, user activities, timeline, cortana pick up where you left off, cortana pick up where i left off, project rome, 使用者活動, 時間軸, cortana 從先前離開的地方開始, cortana 接續未完成的部分, project rome
ms.author: twhitney
ms.date: 04/27/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e99796decfa5ed434fddee3be4340380e2376a2
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2018
ms.locfileid: "6851921"
---
# <a name="continue-user-activity-even-across-devices"></a>繼續使用者活動，甚至是在各個裝置之間

本主題描述如何協助使用者在其電腦上及各個裝置之間繼續使用您的應用程式執行作業。

## <a name="user-activities-and-timeline"></a>使用者活動和時間軸

我們在一天之內會使用到多種裝置。 我們可能會在公車上使用手機，白天使用電腦，到了晚上則使用手機或平板電腦。 從 Windows 10 組建 1803 或更新版本起，可建立 [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) 讓活動顯示在 Windows 時間軸與 Cortana 的「從先前離開的地方開始」功能中。 時間軸是一種豐富的工作檢視，能充分利用「使用者活動」以顯示進行中作業的時間順序檢視。 它也會包含您過去在各個裝置上進行的作業。

![Windows 時間軸圖](images/timeline.png)

同樣地，將您的手機連結至您的 Windows 電腦，可讓您繼續進行先前在您 iOS 或 Android 裝置進行的作業。

將 **UserActivity** 想成使用者在您應用程式中進行的特定事物。 例如，如果您正在使用 RSS 閱讀程式，**UserActivity** 會是您正在閱讀的摘要。 如果您正在玩遊戲，**UserActivity** 會是您正在玩的關卡。 如果您正在聆聽音樂應用程式，**UserActivity** 會是您正在聆聽的播放清單。 如果您正在處理文件，**UserActivity** 會是您停止作業或是其他等等的地方。  簡言之，**UserActivity** 代表您應用程式中的一個目的地，讓使用者能從此處繼續進行作業。

當您呼叫 [UserActivity.CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession) 以使用 **UserActivity** 時，系統會建立歷程記錄指出該 **UserActivity** 的開始與結束時間。 隨著您不斷使用 **UserActivity** 一段時間後，系統為它記錄多個歷程記錄。

## <a name="add-user-activities-to-your-app"></a>將使用者活動新增至您的應用程式

[UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) 是使用者參與 Windows 的單位。 它有三個部分，分別是：用於啟用活動所屬應用程式的 URI、視覺效果，以及描述活動的中繼資料。

1. [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri#Windows_ApplicationModel_UserActivities_UserActivity_ActivationUri) 用於以特定的內容繼續應用程式。 一般而言，此連結所採用的格式為配置的通訊協定處理常式 (例如「my-app://page2?action=edit」) 或 AppUriHandler (例如 http://constoso.com/page2?action=edit)。
2. [VisualElements](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.visualelements) 會公開一個類別，讓使用者能夠依照標題、描述或調適型卡片元素目測識別活動。
3. 最後，[Content](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.content#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_Content) 是您可以儲存活動中繼資料的位置，可用於群組與擷取特定內容下的活動。 通常，會採用 [http://schema.org](http://schema.org) 資料的格式。

若要將 **UserActivity** 新增至應用程式：

1. 當您使用者的內容在應用程式內變更 (例如網頁瀏覽、新遊戲關卡等) 時，會產生 **UserActivity** 物件
2. 在 **UserActivity** 物件中至少填入以下必填欄位：[ActivityId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activityid#Windows_ApplicationModel_UserActivities_UserActivity_ActivityId)、 [ActivationUri](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.activationuri) 及 [UserActivity.VisualElements.DisplayText](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements.displaytext#Windows_ApplicationModel_UserActivities_UserActivityVisualElements_DisplayText)。
3. 將自訂配置處理常式新增至您的應用程式，讓它可由 **UserActivity** 重新啟用。

只要幾行程式碼，就能將 **UserActivity** 整合至應用程式。 例如，想像一下 MainPage 類別內部 MainPage.xaml.cs 中的這個程式碼 (注意：假設 `using Windows.ApplicationModel.UserActivities;`)：

```csharp
UserActivitySession _currentActivity;
private async Task GenerateActivityAsync()
{
    // Get the default UserActivityChannel and query it for our UserActivity. If the activity doesn't exist, one is created.
    UserActivityChannel channel = UserActivityChannel.GetDefault();
    UserActivity userActivity = await channel.GetOrCreateUserActivityAsync("MainPage");
 
    // Populate required properties
    userActivity.VisualElements.DisplayText = "Hello Activities";
    userActivity.ActivationUri = new Uri("my-app://page2?action=edit");
     
    //Save
    await userActivity.SaveAsync(); //save the new metadata
 
    // Dispose of any current UserActivitySession, and create a new one.
    _currentActivity?.Dispose();
    _currentActivity = userActivity.CreateSession();
}
```

在上述 `GenerateActivityAsync()` 方法中的第一行會取得使用者的 [UserActivityChannel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)。 此應用程式的活動將會發佈到此摘要。 下一行會查詢名為 `MainPage` 之活動的通道。

* 您的應用程式應以每次使用者在應用程式特定位置中時即產生相同 ID 的方式，為活動命名。 例如，如果您的應用程式是網頁型，則使用網頁識別碼；如果是文件型，則使用文件的名稱 (或名稱的雜湊)。
* 如果摘要中有識別碼相同的現有活動，則會從 `UserActivity.State` 設為 [Published](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitystate)) 的通道傳回該活動。 如果沒有活動採用該名稱，則會傳回 `UserActivity.State` 設為 **New** 的新活動。
* 活動的範圍限於您的應用程式。 您不必擔心您的活動 ID 與其他應用程式中的 ID 相衝突。

在取得或建立 **UserActivity** 後，請指定另外兩個必填欄位：`UserActivity.VisualElements.DisplayText` 和 `UserActivity.ActivationUri`。

接著，透過呼叫 [SaveAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.saveasync)，最後呼叫 [CreateSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity.createsession)，以儲存 **UserActivity** 中繼資料，這會傳回 [UserActivitySession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitysession)。 **UserActivitySession** 是當使用者真正與 **UserActivity** 互動時用於管理的物件。 例如，當使用者離開網頁時，我們應在 **UserActivitySession** 上呼叫 `Dispose()`。 在上述範例中，我們也會先在 `_currentActivity` 上呼叫 `Dispose()`，再呼叫 `CreateSession()`。 這是因為我們讓 `_currentActivity` 成為我們網頁的成員欄位，而且我們想要先停止任何現有的活動，再開始新的活動 (注意：`?` 是 [null 條件式運算子](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-conditional-operators)，會先測試是否為 null 值，再執行成員存取)。

因此在此狀況下， `ActivationUri` 是自訂配置，我們也必須在應用程式資訊清單中登錄通訊協定。 這會在 Package.appmanifest XML 檔案中完成，或使用設計工具完成。

若要使用設計工具進行變更，請按兩下專案中的 Package.appmanifest 檔案啟動設計工具，選取 \[宣告\]**** 索引標籤，然後新增 \[通訊協定\]**** 定義。 目前唯一需要填寫的屬性為 \[名稱\]****。 它應該符合我們上述指定的 URI，`my-app`。

現在我們需要撰寫一些程式碼告訴應用程式在由通訊協定啟用後該執行的動作。 我們將覆寫 App.xaml.cs 中的 `OnActivated` 方法，將 URI 傳遞至主要網頁，如下所示：

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    if (e.Kind == ActivationKind.Protocol)
    {
        var uriArgs = e as ProtocolActivatedEventArgs;
        if (uriArgs != null)
        {
            if (uriArgs.Uri.Host == "page2")
            {
                // Navigate to the 2nd page of the  app
            }
        }
    }
    Window.Current.Activate();
}
```

此程式碼的功能就是偵測是否是透過通訊協定啟用應用程式。 如果是，它會進一步瞭解應用程式應執行什麼動作，以繼續進行系統啟用此程式碼所針對的工作。 就一個簡單的應用程式而言，此應用程式的唯一活動就是在應用程式啟動時將您放在第二頁。

## <a name="use-adaptive-cards-to-improve-the-timeline-experience"></a>使用調適型卡片改善時間軸體驗

使用者活動會顯示在 Cortana 與時間軸中。 當活動顯示在時間軸中時，我們會使用[調適型卡片](http://adaptivecards.io/)架構顯示活動。 如果您沒有為每個活動提供調適型卡片，則時間軸會自動根據您的應用程式名稱與圖示、標題欄位及選用的描述欄位建立一個簡單的活動卡。 以下是調適型卡片承載與其所產生卡片的範例。

![調適型卡片](images/adaptivecard.png)]

調適型卡片承載 JSON 字串範例：

```json
{ 
  "$schema": "http://adaptivecards.io/schemas/adaptive-card.json", 
  "type": "AdaptiveCard", 
  "version": "1.0",
  "backgroundImage": "https://winblogs.azureedge.net/win/2017/11/eb5d872c743f8f54b957ff3f5ef3066b.jpg", 
  "body": [ 
    { 
      "type": "Container", 
      "items": [ 
        { 
          "type": "TextBlock", 
          "text": "Windows Blog", 
          "weight": "bolder", 
          "size": "large", 
          "wrap": true, 
          "maxLines": 3 
        }, 
        { 
          "type": "TextBlock", 
          "text": "Training Haiti’s radiologists: St. Louis doctor takes her teaching global", 
          "size": "default", 
          "wrap": true, 
          "maxLines": 3 
        } 
      ] 
    } 
  ]
}
```

將調適型卡片承載作為 JSON 字串新增至 **UserActivity**，如下所示：

```csharp
activity.VisualElements.Content = 
Windows.UI.Shell.AdaptiveCardBuilder.CreateAdaptiveCardFromJson(jsonCardText); // where jsonCardText is a JSON string that represents the card
```

## <a name="cross-platform-and-service-to-service-integration"></a>跨平台與服務對服務整合

如果您的應用程式跨平台執行 (例如在 Android 與 iOS 上)，或在雲端中維護使用者狀態，您可以透過 [Microsoft Graph](https://developer.microsoft.com/graph/) 發佈 UserActivity。
在透過 Microsoft 帳戶驗證應用程式或服務後，只要兩個簡單的 REST 呼叫，就能使用與上述相同的資料產生[活動](https://developer.microsoft.com/graph/docs/api-reference/beta/api/projectrome_put_activity)與[歷程記錄](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/projectrome_historyitem)物件。

## <a name="summary"></a>摘要

您可以使用 [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities) API 讓您的應用程式顯示在時間軸與 Cortana 中。
* 深入了解[**UserActivity** API](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)
* 請查看[範例程式碼](https://github.com/Microsoft/project-rome)。
* 請參閱[更複雜的調適型卡片](http://adaptivecards.io/)。
* 透過 [Microsoft Graph](https://developer.microsoft.com/graph/) 從 iOS、Android 或您的 Web 服務發佈 **UserActivity**。
* 深入了解 [GitHub 上的 Project Rome](https://github.com/Microsoft/project-rome)。

## <a name="key-apis"></a>重要 API

* [Useractivity 命名空間](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>相關主題

* [使用者活動 （專案 Rome 文件）](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [調適型卡片](https://docs.microsoft.com/adaptive-cards/)
* [調適型卡片視覺化工具範例](http://adaptivecards.io/)
* [處理 URI 啟用](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [使用 Microsoft Graph、活動摘要及調適型卡片在任何平台上與您的客戶互動](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)