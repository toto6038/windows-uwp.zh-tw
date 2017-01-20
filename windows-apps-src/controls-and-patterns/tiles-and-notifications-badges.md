---
author: mijacobs
Description: "了解如何使用磚、徽章、快顯通知以及通知提供您應用程式的進入點，並將使用者維持在最新狀態。"
title: "磚、徽章及通知"
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: 465c75ccb2af9b162202a79025aa292fbd626a58

---
# <a name="badge-notifications-for-uwp-apps"></a>UWP 應用程式的徽章通知

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>有顯示數字徽章的磚<br/> 數字 63 表示 63 封未讀取的郵件。</div>

通知徽章可傳遞您應用程式專屬的摘要或狀態資訊。 這些資訊可以是數值 (1-99) 或是一組系統提供的字符。 最適合徽章傳遞的資訊範例包括線上遊戲的網路連線狀態、立即訊息應用程式的使用者狀態、郵件應用程式中未讀取郵件的數目，以及社交媒體應用程式中的新文章數目。 

不論應用程式是否在執行，通知徽章都會顯示在應用程式工作列圖示上，以及其開始磚的右下角。 徽章能在所有大小的磚上顯示。  

> [!NOTE]
> 您不能提供自己的徽章影像，只能使用系統提供的徽章影像。


## <a name="numeric-badges"></a>數字徽章

<table>
    <tr>
        <th>值 (Value)</th>
        <th>徽章</th>
        <th>XML</th>
    </tr>
    <tr>
        <td>從 1 到 99 的數字。 值為 0 相當於字符的 value 屬性為 "none"，而且將會清除徽章。</td>
        <td>![小於 100 的數字徽章。](images/badges/badge-numeric.png)</td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>任何大於 99 的數字徽章。</td>
        <td>![大於 99 的數字徽章。](images/badges/badge-numeric-greater.png)</td></td>
        <td>`<badge value="100"/>`</td>
    </tr>    
</table>

## <a name="glyph-badges"></a>字符徽章
除了數字之外，徽章也可以顯示一個字符，其屬於一組不可延伸之狀態字符。 

<table>
<tr>
    <th>狀態</th>
    <th>字符</th>
    <th>XML</th>
</tr>
<tr>
    <td>無 (none)</td>
    <td>(沒有顯示徽章。)</td>
    <td>`<badge value="none"/>`</td>
</tr>
<tr>
    <td>活動 (activity)</td>
    <td>![字符](images/badges/badge-activity.png)</td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>鬧鐘 (alarm)</td>
    <td>![字符](images/badges/badge-alarm.png)</td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>提醒 (alert)</td>
    <td>![字符](images/badges/badge-alert.png)</td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>注意 (attention)</td>
    <td>![字符](images/badges/badge-attention.png)</td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>線上 (available)</td>
    <td>![字符](images/badges/badge-available.png)</td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>離開 (away)</td>
    <td>![字符](images/badges/badge-away.png)</td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>忙碌 (busy)</td>
    <td>![字符](images/badges/badge-busy.png)</td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>錯誤 (error)</td>
    <td>![字符](images/badges/badge-error.png)</td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>新訊息 (newMessage)</td>
    <td>![字符](images/badges/badge-newMessage.png)</td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>暫停 (paused)</td>
    <td>![字符](images/badges/badge-paused.png)</td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>正在播放 (playing)</td>
    <td>![字符](images/badges/badge-playing.png)</td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>離線 (unavailable)</td>
    <td>![字符](images/badges/badge-unavailable.png)</td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## <a name="create-a-badge"></a>建立徽章

以下範例名如何建立徽章更新。

### <a name="create-a-numeric-badge"></a>建立數字徽章

````csharp
private void setBadgeNumber(int num)
{

    // Get the blank badge XML payload for a badge number
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeNumber);

    // Set the value of the badge in the XML to our number
    XmlElement badgeElement = badgeXml.SelectSingleNode("/badge") as XmlElement;
    badgeElement.SetAttribute("value", num.ToString());

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="create-a-glyph-badge"></a>建立字符徽章
````csharp
private void updateBadgeGlyph()
{
    string badgeGlyphValue = "alert";

    // Get the blank badge XML payload for a badge glyph
    XmlDocument badgeXml = 
        BadgeUpdateManager.GetTemplateContent(BadgeTemplateType.BadgeGlyph);

    // Set the value of the badge in the XML to our glyph value
    Windows.Data.Xml.Dom.XmlElement badgeElement = 
        badgeXml.SelectSingleNode("/badge") as Windows.Data.Xml.Dom.XmlElement;
    badgeElement.SetAttribute("value", badgeGlyphValue);

    // Create the badge notification
    BadgeNotification badge = new BadgeNotification(badgeXml);

    // Create the badge updater for the application
    BadgeUpdater badgeUpdater = 
        BadgeUpdateManager.CreateBadgeUpdaterForApplication();

    // And update the badge
    badgeUpdater.Update(badge);

}
````

### <a name="clear-a-badge"></a>清除徽章

````csharp
private void clearBadge()
{
    BadgeUpdateManager.CreateBadgeUpdaterForApplication().Clear();
}
````

## <a name="get-the-sample-code"></a>取得範例程式碼

* [通知範例](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/Notifications)<br/> 說明如何建立動態磚、傳送徽章更新，以及顯示快顯通知。 

## <a name="related-articles"></a>相關文章

* [調適型和互動式快顯通知](tiles-and-notifications-adaptive-interactive-toasts.md)
* [建立磚](tiles-and-notifications-creating-tiles.md)
* [建立彈性磚](tiles-and-notifications-create-adaptive-tiles.md)


<!--HONumber=Dec16_HO2-->


