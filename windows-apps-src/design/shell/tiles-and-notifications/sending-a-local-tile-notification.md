---
Description: This article describes how to send a local tile notification to a primary tile and a secondary tile using adaptive tile templates.
title: 傳送本機磚通知
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5752a7bf18d785121258ea3fe75afe8383be2aff
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8348580"
---
# <a name="send-a-local-tile-notification"></a>傳送本機磚通知
 

Windows 10 中的主要的應用程式磚被定義在您的應用程式資訊清單中，以程式設計方式建立和由您的應用程式程式碼定義次要磚。 本文說明如何使用彈性磚範本，將本機磚通知傳送到主要磚和次要磚。 (本機通知是透過應用程式程式碼傳送，而不是從 Web 伺服器推送或提取)。

![預設磚和含有通知的磚](images/sending-local-tile-01.png)

> [!NOTE] 
>了解[建立調適性磚](create-adaptive-tiles.md)及[磚內容結構描述](../tiles-and-notifications/tile-schema.md)。

 

## <a name="install-the-nuget-package"></a>安裝 NuGet 套件


我們建議您安裝 [Notifications 程式庫 NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，以物件 (而非原始 XML) 來產生磚承載，讓事情較為簡化。

此文章中的內嵌程式碼範例適用於使用 Notifications 程式庫的 C#。 (如果您想要建立您自己的 XML，您可以在文章後面找到不含 Notifications 程式庫的程式碼範例。)

## <a name="add-namespace-declarations"></a>加入命名空間宣告


若要存取磚 API，請包含 [**Windows.UI.Notifications**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications) 命名空間。 我們也建議您包含 **Microsoft.Toolkit.Uwp.Notifications** 命名空間，這樣您就能利用我們的磚協助程式 API (您必須安裝 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet 套件才能存取這些 API)。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```

## <a name="create-the-notification-content"></a>建立通知內容


在 windows 10，磚承載被定義使用彈性磚範本，可讓您建立自訂的視覺配置，針對您的通知。 (若要深入了解彈性磚可以做什麼，請參閱[建立彈性磚](create-adaptive-tiles.md)。)

這個程式碼範例會建立中型和寬形磚的彈性磚內容。

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// Construct the tile content
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        },

        TileWide = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = from,
                        HintStyle = AdaptiveTextStyle.Subtitle
                    },

                    new AdaptiveText()
                    {
                        Text = subject,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    },

                    new AdaptiveText()
                    {
                        Text = body,
                        HintStyle = AdaptiveTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

通知內容在中型磚上顯示時看起來就像下面這樣：

![中型磚上的通知內容](images/sending-local-tile-02.png)

## <a name="create-the-notification"></a>建立通知


擁有通知內容後，您需要建立一個新的 [**TileNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileNotification)。 **TileNotification** 建構函式採用 Windows 執行階段 [**XmlDocument**](https://docs.microsoft.com/uwp/api/windows.data.xml.dom.xmldocument) 物件，如果您使用 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，即可透過 **TileContent.GetXml** 方法取得。

這個程式碼範例會為新的磚建立通知。

```csharp
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <a name="set-an-expiration-time-for-the-notification-optional"></a>設定通知的到期時間 (選擇性)


根據預設，本機磚和徽章通知不會到期，而推播、定期以及排程通知則會在三天後到期。 磚內容持續的時間不應太長，最佳做法是設定適合您的應用程式的到期時間，特別是針對本機磚和徽章通知。

這個程式碼範例會建立一個到期的通知，而它會在 10 分鐘後移除。

```csharp
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);
```

## <a name="send-the-notification"></a>傳送通知


雖然在本機傳送磚通知很簡單，但傳送通知到主要或次要磚則稍有不同。

**主要磚**

若要將通知傳送到主要磚，請使用 [**TileUpdateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdateManager) 為主要磚建立磚更新程式，並呼叫 "Update" 來傳送通知。 無論是否可見，您的應用程式主要磚一律會存在，因此即使未釘選它，也可以傳送通知給它。 如果使用者之後釘選您的主要磚，屆時就會顯示您傳送的通知。

這個程式碼範例會傳送通知到主要磚。


```csharp
// Send the notification to the primary tile
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**次要磚**

若要傳送通知到次要磚，請先確定次要磚已經存在。 如果您嘗試為不存在的次要磚建立磚更新程式 (例如，如果使用者取消釘選次要磚) ，就會擲回例外狀況。 您可以使用 [**SecondaryTile.Exists**](https://docs.microsoft.com/uwp/api/Windows.UI.StartScreen.SecondaryTile#Windows_UI_StartScreen_SecondaryTile_Exists_System_String_)(tileId) 探索您的次要磚是否已釘選，然後再為次要磚建立磚更新程式並傳送通知。

這個程式碼範例會傳送通知到次要磚。

```csharp
// If the secondary tile is pinned
if (SecondaryTile.Exists("MySecondaryTile"))
{
    // Get its updater
    var updater = TileUpdateManager.CreateTileUpdaterForSecondaryTile("MySecondaryTile");
 
    // And send the notification
    updater.Update(notification);
}
```

![預設磚和含有通知的磚](images/sending-local-tile-01.png)

## <a name="clear-notifications-on-the-tile-optional"></a>清除磚上的通知 (選擇性)


在大部分情況下，在使用者與通知內容互動後就該清除通知。 例如，使用者啟動您的應用程式後，您可能要清除磚的所有通知。 如果通知有時間限制，建議您設定通知的到期時間，而不是明確清除通知。

這個程式碼範例會清除主要磚的磚通知。 您可以建立次要磚的磚更新程式，以針對次要磚達到同樣的目的。

```csharp
TileUpdateManager.CreateTileUpdaterForApplication().Clear();
```

對於啟用了通知佇列的磚以及佇列中的通知，請呼叫 Clear 方法清除佇列。 不過，您無法透過您的應用程式伺服器清除通知；只有本機應用程式程式碼才可以清除通知。

定期或推播通知只能新增通知或取代現有的通知。 不論通知本身的來源是推播、定期或本機，本機呼叫 Clear 方法將會清除磚。 這個方法不會清除尚未顯示的排程通知。

![含有通知的磚與清除後的磚](images/sending-local-tile-03.png)

## <a name="next-steps"></a>後續步驟


**使用通知佇列**

您已經完成第一次的磚更新，現在可以透過啟用[通知佇列](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)來擴充磚的功能。

**其他通知傳遞方法**

本文說明如何以通知傳送磚更新。 若要探索其他通知傳遞方法，包括排程、定期及推播，請參閱[傳遞通知](choosing-a-notification-delivery-method.md)。

**XmlEncode 傳遞方法**

如果您不是使用 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)，這個通知傳遞方法是另一個替代方案。


```csharp
public string XmlEncode(string text)
{
    StringBuilder builder = new StringBuilder();
    using (var writer = XmlWriter.Create(builder))
    {
        writer.WriteString(text);
    }

    return builder.ToString();
}
```

## <a name="code-examples-without-notifications-library"></a>不含 Notifications 程式庫的程式碼範例


如果您偏好使用原始的 XML 而非 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) NuGet 套件，請將這些替代程式碼範例運用到本文的前三個範例。 其餘的程式碼範例則可搭配 [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)或原始 XML 使用。

加入命名空間宣告

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

建立通知內容

```csharp
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template='TileMedium'>
            <text>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
        <binding template='TileWide'>
            <text hint-style='subtitle'>{from}</text>
            <text hint-style='captionSubtle'>{subject}</text>
            <text hint-style='captionSubtle'>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

建立通知

```csharp
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <a name="related-topics"></a>相關主題

* [建立彈性磚](create-adaptive-tiles.md)
* [磚內容結構描述](../tiles-and-notifications/tile-schema.md)
* [Notifications 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)
* [GitHub 上的完整程式碼](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Windows.UI.Notifications 命名空間**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications)
* [如何使用通知佇列 (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [傳遞通知](choosing-a-notification-delivery-method.md)
 

 




