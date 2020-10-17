---
Description: 使用可追蹤式磚通知來了解當使用者按下動態磚，您的應用程式會顯示什麼內容。
title: 可追蹤式磚通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: windows 10, uwp, 可追蹤式磚, 動態磚, 可追蹤式磚通知
ms.localizationpriority: medium
ms.openlocfilehash: 951dc891fb34ae4be7551c08ff47eabc19ae9eb6
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100276"
---
# <a name="chaseable-tile-notifications"></a>可追蹤式磚通知

可追蹤式磚通知可讓您判斷當使用者按下磚時，您的應用程式動態磚所顯示的磚通知。  
例如，新聞應用程式可使用此功能來判斷當使用者啟動它時，其動態磚會顯示什麼新聞故事，藉以確保以醒目的方式顯示某些新聞，讓使用者可以找到它。 

> [!IMPORTANT]
> **需要年度更新版**：若要搭配 C#、C++ 或 VB 型 UWP app 使用可追蹤式磚通知，您的目標必須是 SDK 14393 並執行組建 14393 或更新版本。 對於 JavaScript 型 UWP app，您的目標必須是 SDK 17134 並執行組建 17134 或更新版本。 


> **重要 API**：[LaunchActivatedEventArgs.TileActivatedInfo 屬性](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)、[TileActivatedInfo 類別](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>運作方式

若要使用可追蹤式磚通知，請使用磚通知裝載上的 **Arguments** 屬性 (類似於快顯通知裝載上的 launch 屬性) 來嵌入磚通知內容的相關資訊。

當您的應用程式透過動態磚啟動時，系統會從目前/最近顯示的磚通知傳回引數清單。


## <a name="when-to-use-chaseable-tile-notifications"></a>使用可追蹤式磚通知的時機

當您在動態磚上使用通知佇列 (這表示您輪流使用最多 5 個不同通知) 時，通常就能使用可追蹤式磚通知。 如果動態磚上的內容可能與應用程式的最新內容不同步時，它們也很有用。 例如，「新聞」應用程式每隔 30 分鐘重新整理其動態磚，但當應用程式啟動時，它會載入最新新聞 (其中可能不包括最後一次輪詢間隔的磚上內容)。 發生這種情形時，使用者可能會找不到他們剛剛在動態磚上看到的新聞，因而感到沮喪。 這時可追蹤式磚通知就能派上用場，可讓您確保使用者能夠輕鬆找到他們在磚上所看到的內容。

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>如何使用可追蹤式磚通知

最重要的是要注意在大部分案例中，**您不應直接瀏覽到當使用者按下時磚上的特定通知**。 動態磚可做為應用程式的進入點。 使用者在兩種情形下會按動態磚：(1) 他們想以正常方式啟動您的應用程式，或 (2) 他們想查看剛剛出現在動態磚上的特定通知的相關資訊。 由於使用者無法明確表明他們想要哪種操作，理想的經驗是**正常啟動您的應用程式，並確認可以輕鬆找到使用者看到的通知**。

例如，按一下「MSN 新聞」應用程式的動態磚會正常啟動應用程式：它會顯示首頁，或使用者先前閱讀的文章。 不過，在首頁中，應用程式可確保能輕鬆找到動態磚上的故事。 如此一來就能同時支援這兩種情形：您只是想要啟動/繼續應用程式，以及您想檢視特定故事。


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>如何將 Arguments 屬性加入您的磚通知裝載

在通知裝載中，引數屬性可讓您的應用程式提供稍後用來找出通知的資料。 例如，引數可能包含故事的識別碼，以便啟動時，您可以擷取並顯示故事。 屬性接受字串，這可用您喜歡的方式序列化 (查詢字串、JSON 等)，但通常建議使用查詢字串格式，因為它比較輕量，並能正確進行 XML 編碼。

屬性可以在 **TileVisual** 和 **TileBinding** 元素上設定，並能重疊顯示。 如果您想在每個磚大小上使用相同的引數，只需在 **TileVisual** 上設定引數。 如果您需要為特定的磚大小使用特定引數，您可以在個別 **TileBinding** 元素上設定引數。

此範例會建立使用引數屬性的通知承載，以便稍後識別通知。 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>當您的應用程式啟動時如何查看引數屬性

大部分應用程式有 App.xaml.cs 檔案，其中包含 [OnLaunched](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) 方法的覆寫。 正如其名，您的應用程式啟動時會呼叫這個方法。 它採用單一引數，一個 [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) 物件。

LaunchActivatedEventArgs 物件具有可啟用可追蹤式通知的屬性：[TileActivatedInfo 屬性](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo)，這可供存取 [TileActivatedInfo 物件](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)。 當使用者從磚啟動您的應用程式 (而不是從應用程式清單、搜尋或任何其他進入點)，您的應用程式會初始化此屬性。

[TileActivatedInfo 物件](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)包含名為 [RecentlyShownNotifications](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications) 的屬性，其中包含過去 15 分鐘內顯示在磚上的通知清單。 清單中的第一個項目表示磚上的目前通知，後續項目代表目前通知之前使用者看到的先前通知。 如果您的磚已清除，這份清單會是空的。

每個 ShownTileNotification 都有 Arguments 屬性。 Arguments 屬性會使用磚通知裝載上的引數字串來初始化，而如果裝載未包含引數字串則為 null。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


### <a name="accessing-onlaunched-from-desktop-applications"></a>從桌面應用程式存取 >onlaunched

桌面應用程式 (例如 WPF 等) 使用 [傳統型橋接器](https://developer.microsoft.com/windows/bridges/desktop)，也可以使用 chaseable 圖格！ 唯一的差異在於存取 >onlaunched 引數。 請注意，您必須先 [使用傳統型橋接器封裝您的應用程式](/windows/msix/desktop/source-code-overview)。

> [!IMPORTANT]
> **需要2018年10月更新**：若要使用 `AppInstance.GetActivatedEventArgs()` API，您必須以 SDK 17763 為目標，並執行組建17763或更新版本。

針對桌面應用程式，若要存取啟動引數，請執行下列動作 .。。

```csharp

static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // API only available on build 17763 or higher
    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:

            var launchArgs = args as LaunchActivatedEventArgs;

            // If clicked on from tile
            if (launchArgs.TileActivatedInfo != null)
            {
                // If tile notification(s) were present
                if (launchArgs.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
                {
                    // Get arguments from the notifications that were recently displayed
                    string[] allTileArgs = launchArgs.TileActivatedInfo.RecentlyShownNotifications
                    .Select(i => i.Arguments)
                    .ToArray();
     
                    // TODO: Highlight each story in the app
                }
            }
    
            break;
```


## <a name="raw-xml-example"></a>原始 XML 範例

如果您使用原始 XML 而非通知程式庫，以下是 XML。

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>相關文章

- [LaunchActivatedEventArgs.TileActivatedInfo 屬性](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [TileActivatedInfo 類別](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)