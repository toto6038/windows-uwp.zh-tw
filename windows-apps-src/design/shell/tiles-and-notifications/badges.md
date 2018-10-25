---
author: mijacobs
Description: Learn how to use tiles, badges, toasts, and notifications to provide entry points into your app and keep users up-to-date.
title: UWP app 的徽章通知
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fc09e7a9d98d04c53aac0d76293b9d8e4dc6ed91
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "5515803"
---
# <a name="badge-notifications-for-uwp-apps"></a>UWP app 的徽章通知

 

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
        <td><img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /></td>
        <td>`<badge value="1"/>`</td>
    </tr>
    <tr>
        <td>任何大於 99 的數字徽章。</td>
        <td><img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td></td>
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
    <td><img src="images/badges/badge-activity.png" alt="Glyph" /></td>
    <td>`<badge value="activity"/>`</td>
</tr>
<tr>
    <td>鬧鐘 (alarm)</td>
    <td><img src="images/badges/badge-alarm.png" alt="Glyph" /></td>
    <td>`<badge value="alarm"/>`</td>
</tr>
<tr>
    <td>提醒 (alert)</td>
    <td><img src="images/badges/badge-alert.png" alt="Glyph" /></td>
    <td>`<badge value="alert"/>`</td>
</tr>
<tr>
    <td>注意 (attention)</td>
    <td><img src="images/badges/badge-attention.png" alt="Glyph" /></td>
    <td>`<badge value="attention"/>`</td>
</tr>
<tr>
    <td>線上 (available)</td>
    <td><img src="images/badges/badge-available.png" alt="Glyph" /></td>
    <td>`<badge value="available"/>`</td>
</tr>
<tr>
    <td>離開 (away)</td>
    <td><img src="images/badges/badge-away.png" alt="Glyph" /></td>
    <td>`<badge value="away"/>`</td>
</tr>
<tr>
    <td>忙碌 (busy)</td>
    <td><img src="images/badges/badge-busy.png" alt="Glyph" /></td>
    <td>`<badge value="busy"/>`</td>
</tr>
<tr>
    <td>錯誤 (error)</td>
    <td><img src="images/badges/badge-error.png" alt="Glyph" /></td>
    <td>`<badge value="error"/>`</td>
</tr>
<tr>
    <td>新訊息 (newMessage)</td>
    <td><img src="images/badges/badge-newMessage.png" alt="Glyph" /></td>
    <td>`<badge value="newMessage"/>`</td>
</tr>
<tr>
    <td>暫停 (paused)</td>
    <td><img src="images/badges/badge-paused.png" alt="Glyph" /></td>
    <td>`<badge value="paused"/>`</td>
</tr>
<tr>
    <td>正在播放 (playing)</td>
    <td><img src="images/badges/badge-playing.png" alt="Glyph" /></td>
    <td>`<badge value="playing"/>`</td>
</tr>
<tr>
    <td>離線 (unavailable)</td>
    <td><img src="images/badges/badge-unavailable.png" alt="Glyph" /></td>
    <td>`<badge value="unavailable"/>`</td>
</tr>
</table>

## <a name="create-a-badge"></a>建立徽章

這些範例說明如何建立徽章更新。

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

* [調適型和互動式快顯通知](adaptive-interactive-toasts.md)
* [建立磚](creating-tiles.md)
* [建立彈性磚](create-adaptive-tiles.md)
