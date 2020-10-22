---
description: 了解如何使用磚、徽章、快顯通知以及通知提供您應用程式的進入點，並將使用者維持在最新狀態。
title: Windows 應用程式的徽章通知
ms.assetid: 48ee4328-7999-40c2-9354-7ea7d488c538
label: Tiles, badges, and notifications
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 64d5595bcc315b24228401ff23a1a59d29282eae
ms.sourcegitcommit: cbdfac0e2d8bead6c225e815e7d6dffe1f5ef864
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344962"
---
# <a name="badge-notifications-for-windows-apps"></a>Windows 應用程式的徽章通知

 

<div style="float:left; font-size:80%; text-align:left; margin: 0px 15px 15px 0px;">
<img src="images/badge-example.png" alt="A tile with a numeric badge displaying the number 63 to indicate 63 unread mails." style="padding-bottom:0.0em; margin-bottom: 2px" /><br/>有顯示數字徽章的磚<br/> 數字 63 表示 63 封未讀取的郵件。</div>

通知徽章可傳遞您應用程式專屬的摘要或狀態資訊。 這些資訊可以是數值 (1-99) 或是一組系統提供的字符。 最適合徽章傳遞的資訊範例包括線上遊戲的網路連線狀態、立即訊息應用程式的使用者狀態、郵件應用程式中未讀取郵件的數目，以及社交媒體應用程式中的新文章數目。 

不論應用程式是否在執行，通知徽章都會顯示在應用程式工作列圖示上，以及其開始磚的右下角。 徽章能在所有大小的磚上顯示。  

> [!NOTE]
> 您不能提供自己的徽章影像，只能使用系統提供的徽章影像。


## <a name="numeric-badges"></a>數字徽章

值 | 徽章 | XML
--|--|--
從 1 到 99 的數字。 值為 0 相當於字符的 value 屬性為 "none"，而且將會清除徽章。 | <img src="images/badges/badge-numeric.png" alt="A numeric badge less than 100." /> | `<badge value="1"/>`
任何大於 99 的數字徽章。 | <img src="images/badges/badge-numeric-greater.png" alt="A numeric badge greater than 99." /></td> | `<badge value="100"/>`

## <a name="glyph-badges"></a>字符徽章
除了數字之外，徽章也可以顯示一個字符，其屬於一組不可延伸之狀態字符。 

狀態 | 圖像 | XML
--|--|--
無 | (沒有顯示徽章。) | `<badge value="none"/>`
activity | :::image type="icon" source="images/badges/badge-activity.png"::: | `<badge value="activity"/>`
鬧鐘 (alarm) | :::image type="icon" source="images/badges/badge-alarm.png"::: | `<badge value="alarm"/>`
警示 | :::image type="icon" source="images/badges/badge-alert.png"::: | `<badge value="alert"/>`
注意 (attention) | :::image type="icon" source="images/badges/badge-attention.png"::: | `<badge value="attention"/>`
可供使用 | :::image type="icon" source="images/badges/badge-available.png"::: | `<badge value="available"/>`
離開 (away) | :::image type="icon" source="images/badges/badge-away.png"::: | `<badge value="away"/>`
忙碌 (busy) | :::image type="icon" source="images/badges/badge-busy.png"::: | `<badge value="busy"/>`
錯誤 | :::image type="icon" source="images/badges/badge-error.png"::: | `<badge value="error"/>`
新訊息 (newMessage) | :::image type="icon" source="images/badges/badge-newMessage.png"::: | `<badge value="newMessage"/>`
暫停 (paused) | :::image type="icon" source="images/badges/badge-paused.png"::: | `<badge value="paused"/>`
正在播放 (playing) | :::image type="icon" source="images/badges/badge-playing.png"::: | `<badge value="playing"/>`
離線 (unavailable) | :::image type="icon" source="images/badges/badge-unavailable.png"::: | `<badge value="unavailable"/>`</td>

## <a name="create-a-badge"></a>建立徽章

這些範例會示範如何建立徽章更新。

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

* [通知範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)<br/> 說明如何建立動態磚、傳送徽章更新，以及顯示快顯通知。 

## <a name="related-articles"></a>相關文章

* [調適型和互動式快顯通知](adaptive-interactive-toasts.md)
* [建立磚](creating-tiles.md)
* [建立彈性磚](create-adaptive-tiles.md)
