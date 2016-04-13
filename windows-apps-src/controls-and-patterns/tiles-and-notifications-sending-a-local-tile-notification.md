---
Description: 本文說明如何使用彈性磚範本，將本機磚通知傳送到主要磚和次要磚。
title: 傳送本機磚通知
ms.assetid: D34B0514-AEC6-4C41-B318-F0985B51AF8A
label: TBD
template: detail.hbs
---

# 傳送本機磚通知


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Windows 10 中的主要應用程式磚定義在您的應用程式資訊清單中，次要磚則是以程式設計方式建立，而且是由您的應用程式程式碼定義。 本文說明如何使用彈性磚範本，將本機磚通知傳送到主要磚和次要磚。 (本機通知是透過應用程式程式碼傳送，而不是從 Web 伺服器推送或提取)。

![預設磚和含有通知的磚](images/sending-local-tile-01.png)

**注意**：了解[建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)和[彈性磚範本結構描述](tiles-and-notifications-adaptive-tiles-schema.md)。

 

## <span id="Install_the_NuGet_package"> </span> <span id="install_the_nuget_package"> </span> <span id="INSTALL_THE_NUGET_PACKAGE"> </span>安裝 NuGet 套件


我們建議您安裝 [NotificationsExtensions NuGet 套件](https://www.nuget.org/packages/NotificationsExtensions.Win10/)，透過產生含有物件的磚承載 (而非原始 XML) 以簡化項目。

本文中的內嵌程式碼範例是針對已安裝 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) NuGet 封裝的 C#。 (如果您想要建立您自己的 XML，您可以在文章後面找到不含 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) 的程式碼範例。)

## <span id="Add_namespace_declarations"> </span> <span id="add_namespace_declarations"> </span> <span id="ADD_NAMESPACE_DECLARATIONS"> </span>加入命名空間宣告


若要存取磚 API， 請包含 [**Windows.UI.Notifications**](https://msdn.microsoft.com/library/windows/apps/br208661) 命名空間。 我們也建議您包含 **NotificationsExtensions.Tiles** 命名空間，這樣您就能利用我們的磚協助程式 API (您必須安裝 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) NuGet 套件才能存取這些 API)。

```
using Windows.UI.Notifications;
using NotificationsExtensions.Tiles; // NotificationsExtensions.Win10
```

## <span id="Create_the_notification_content"> </span> <span id="create_the_notification_content"> </span> <span id="CREATE_THE_NOTIFICATION_CONTENT"> </span>建立通知內容


在 Windows 10 中，磚承載是彈性磚範本定義的，可讓您為您的通知建立自訂的視覺配置。 (若要深入了解彈性磚可以做什麼，請參閱[建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)和[彈性磚範本](tiles-and-notifications-adaptive-tiles-schema.md)文章。)

這個程式碼範例會建立中型和寬形磚的彈性磚內容。

```
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
                    new TileText()
                    {
                        Text = from
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
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
                    new TileText()
                    {
                        Text = from,
                        Style = TileTextStyle.Subtitle
                    },
 
                    new TileText()
                    {
                        Text = subject,
                        Style = TileTextStyle.CaptionSubtle
                    },
 
                    new TileText()
                    {
                        Text = body,
                        Style = TileTextStyle.CaptionSubtle
                    }
                }
            }
        }
    }
};
```

通知內容在中型磚上顯示時看起來就像下面這樣：

![中型磚上的通知內容](images/sending-local-tile-02.png)

## <span id="Create_the_notification"> </span> <span id="create_the_notification"> </span> <span id="CREATE_THE_NOTIFICATION"> </span>建立通知


擁有通知內容後，您需要建立一個新的 [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)。 **TileNotification** 建構函式採用 Windows 執行階段 [**XmlDocument**](https://msdn.microsoft.com/library/windows/apps/br208620) 物件，如果您使用 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)，即可透過 **TileContent.GetXml** 方法取得。

這個程式碼範例會為新的磚建立通知。

```
// Create the tile notification
var notification = new TileNotification(content.GetXml());
```

## <span id="Set_an_expiration_time_for_the_notification__optional_"> </span> <span id="set_an_expiration_time_for_the_notification__optional_"> </span> <span id="SET_AN_EXPIRATION_TIME_FOR_THE_NOTIFICATION__OPTIONAL_"> </span>設定通知的到期時間 (選擇性)


根據預設，本機磚和徽章通知不會到期，而推播、定期以及排程通知則會在三天後到期。 磚內容持續的時間不應太長，最佳做法是設定適合您的應用程式的到期時間，特別是針對本機磚和徽章通知。

這個程式碼範例會建立一個到期的通知，而它會在 10 分鐘後移除。

```
tileNotification.ExpirationTime = DateTimeOffset.UtcNow.AddMinutes(10);</code></pre></td>
</tr>
</tbody>
</table>
```

## <span id="Send_the_notification"> </span> <span id="send_the_notification"> </span> <span id="SEND_THE_NOTIFICATION"> </span>傳送通知


雖然在本機傳送磚通知很簡單，但傳送通知到主要或次要磚則稍有不同。

**主要磚**

若要將通知傳送到主要磚，請使用 [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622) 為主要磚建立磚更新程式，並呼叫 "Update" 來傳送通知。 無論是否可見，您的應用程式主要磚一律會存在，因此即使未釘選它，也可以傳送通知給它。 如果使用者之後釘選您的主要磚，屆時就會顯示您傳送的通知。

這個程式碼範例會傳送通知到主要磚。

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
// And send the notification
TileUpdateManager.CreateTileUpdaterForApplication().Update(notification);
```

**次要磚**

若要傳送通知到次要磚，請先確定次要磚已經存在。 如果您嘗試為不存在的次要磚建立磚更新程式 (例如，如果使用者取消釘選次要磚) ，就會擲回例外狀況。 您可以使用 [**SecondaryTile.Exists**](https://msdn.microsoft.com/library/windows/apps/br242205)(tileId) 探索您的次要磚是否已釘選，然後再為次要磚建立磚更新程式並傳送通知。

這個程式碼範例會傳送通知到次要磚。

```
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

## <span id="Clear_notifications_on_the_tile__optional_"> </span> <span id="clear_notifications_on_the_tile__optional_"> </span> <span id="CLEAR_NOTIFICATIONS_ON_THE_TILE__OPTIONAL_"> </span>清除磚上的通知 (選擇性)


在大部分情況下，在使用者與通知內容互動後就該清除通知。 例如，使用者啟動您的應用程式後，您可能要清除磚的所有通知。 如果通知有時間限制，建議您設定通知的到期時間，而不是明確清除通知。

這個程式碼範例會清除磚通知。

```
TileUpdateManager.CreateTileUpdaterForApplication().Clear();</code></pre></td>
</tr>
</tbody>
</table>
```

對於啟用了通知佇列的磚以及佇列中的通知，請呼叫 Clear 方法清除佇列。 不過，您無法透過您的應用程式伺服器清除通知；只有本機應用程式程式碼才可以清除通知。

定期或推播通知只能新增通知或取代現有的通知。 不論通知本身的來源是推播、定期或本機，本機呼叫 Clear 方法將會清除磚。 這個方法不會清除尚未顯示的排程通知。

![含有通知的磚與清除後的磚](images/sending-local-tile-03.png)

## <span id="Next_steps"> </span> <span id="next_steps"> </span> <span id="NEXT_STEPS"> </span>後續步驟


**使用通知佇列**

您已經完成第一次的磚更新，現在可以透過啟用[通知佇列](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)來擴充磚的功能。

**其他通知傳遞方法**

本文說明如何以通知傳送磚更新。 若要探索其他通知傳遞方法，包括排程、定期及推播，請參閱[傳遞通知](tiles-and-notifications-choosing-a-notification-delivery-method.md)。

**XmlEncode 傳遞方法**

如果您不是使用 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)，這個通知傳遞方法是另一個替代方案。

<span codelanguage=""></span>
```
<colgroup>
<col width="100%" />
</colgroup>
<tbody>
<tr class="odd">
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

## <span id="Code_examples_without_NotificationsExtensions"> </span> <span id="code_examples_without_notificationsextensions"> </span> <span id="CODE_EXAMPLES_WITHOUT_NOTIFICATIONSEXTENSIONS"> </span>不含 NotificationsExtensions 的程式碼範例


如果您偏好使用原始的 XML 而非 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) NuGet 套件，請將這些替代程式碼範例運用到本文的前三個範例。 其餘的程式碼範例則可搭配 [NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki) 或原始 XML 使用。

加入命名空間宣告

```
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
```

建立通知內容

```
// In a real app, these would be initialized with actual data
string from = "Jennifer Parker";
string subject = "Photos from our trip";
string body = "Check out these awesome photos I took while in New Zealand!";
 
 
// TODO - all values need to be XML escaped
 
 
// Construct the tile content as a string
string content = $@"
<tile>
    <visual>
 
        <binding template=&#39;TileMedium&#39;>
            <text>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
        <binding template=&#39;TileWide&#39;>
            <text hint-style=&#39;subtitle&#39;>{from}</text>
            <text hint-style=&#39;captionSubtle&#39;>{subject}</text>
            <text hint-style=&#39;captionSubtle&#39;>{body}</text>
        </binding>
 
    </visual>
</tile>";
```

建立通知

```
// Load the string into an XmlDocument
XmlDocument doc = new XmlDocument();
doc.LoadXml(content);
 
// Then create the tile notification
var notification = new TileNotification(doc);
```

## <span id="related_topics"> </span>相關主題


* [建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)
* [彈性磚範本：結構描述和文件](tiles-and-notifications-adaptive-tiles-schema.md)
* [NotificationsExtensions.Win10 (NuGet 套件)](https://www.nuget.org/packages/NotificationsExtensions.Win10/)
* [GitHub 上的 NotificationsExtensions](https://github.com/WindowsNotifications/NotificationsExtensions/wiki)
* [GitHub 上的完整程式碼範例](https://github.com/WindowsNotifications/quickstart-sending-local-tile-win10)
* [**Windows.UI.Notifications 命名空間**](https://msdn.microsoft.com/library/windows/apps/br208661)
* [如何使用通知佇列 (XAML)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868234)
* [傳遞通知](tiles-and-notifications-choosing-a-notification-delivery-method.md)
 

 






<!--HONumber=Mar16_HO1-->


