---
title: 朋友圈通知
description: 說明如何建立與使用朋友圈通知；朋友圈通知是一種新的快顯通知。
author: muhsinking
ms.author: mukin
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 943d236699ccab6d61e5394426077a32d7249592
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434812"
---
# <a name="my-people-notifications"></a>朋友圈通知

朋友圈通知還提供新的方法讓使用者能透過快速的表達手勢與關注的人聯繫。 本文章示範如何設計和實作應用程式中的朋友圈通知。 如需完整實作，請參閱 [朋友圈通知範例。](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![心型表情符號通知](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>需求

+ Windows 10 和 Microsoft Visual Studio 2017。 如需安裝詳細資訊，請參閱[開始設定 Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up)。
+ C# 或類似物件導向程式設計語言的基本知識。 若要開始使用 C#，請參閱[建立 "Hello, world" 應用程式](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal)。

## <a name="how-it-works"></a>運作方式

朋友圈功能是一般快顯通知的替代方法，您現在可透過此功能傳送通知，以提供給使用者更個人化的體驗。 這種新的快顯通知可使用朋友圈通知，從釘選在使用者工作列上的連絡人傳送出。 當收到通知時，會在工作列上顯示寄件者的連絡人圖片動畫，並播放聲音，表示通知正在啟動。 在裝載中指定的動畫或影像將顯示 5 秒鐘 (或如果裝載是持續不到 5 秒的動畫，則會一直循環直到 5 秒鐘過後)。

## <a name="supported-image-types"></a>支援的影像類型

+ GIF
+ 靜態影像 (JPEG、PNG)
+ Spritesheet (僅限垂直)

> [!NOTE]
> Spritesheet 是衍生自靜態影像 (JPEG 或 PNG) 的動畫。 各個畫面會垂直排列，因此第一個畫面會在頂端 (雖然您可以在快顯通知裝載中指定不同的開始畫面)。 每個畫面的高度必須相同，程式會以此高度循環播放畫面，以建立動畫順序 (例如 flipbook 將其頁面垂直排列)。 以下為 Spritesheet 的範例。

![彩虹 Spritesheet](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>通知參數
朋友圈通知使用 [快顯通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) 架構，但在快顯通知裝載中必須有額外的繫結節點。 第二個繫結必須包含下列參數：

```xml
experienceType=”shoulderTap”
```

這表示應將快顯通知視為朋友圈通知。

繫結內的影像節點應包含下列參數：

+ **src**
    + 資產的 URI。 這可以是 HTTP/HTTPS Web URI、msappx URI，或本機檔案的路徑。
+ **spritesheet-src**
    + 資產的 URI。 這可以是 HTTP/HTTPS Web URI、msappx URI，或本機檔案的路徑。 僅 Spritesheet 動畫需要。
+ **spritesheet-height**
    + 畫面高度 (以像素為單位)。 僅 Spritesheet 動畫需要。
+ **spritesheet-fps**
    + 每秒畫面數 (FPS)。 僅 Spritesheet 動畫需要。 僅支援值 1-120。
+ **spritesheet-startingFrame**
    + 動畫開始的畫面編號。 僅用於 Spritesheet 動畫，若未提供則預設為 0。
+ **alt**
    + 用於螢幕助讀程式旁白的文字字串。

> [!NOTE]
> 製作一個動畫的通知時，您仍應在 "src" 參數中指定一個靜態影像。 如果無法顯示動畫，將會使用它做為遞補。

此外，最上層的快顯通知節點必須包含 **hint-people** 參數，以指定傳送連絡人。 這個參數可以有以下任何的值：

+ **電子郵件地址** 
    + 例如 mailto:johndoe@mydomain.com
+ **電話號碼** 
    + 例如 tel:888-888-8888
+ **遠端識別碼** 
    + 例如 remoteid:1234

> [!NOTE]
> 如果您的應用程式使用 [ContactStore API](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) 並使用 [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) 屬性將儲存在 PC 上的連絡人與遠端儲存的連絡人連結在一起，則 RemoteId 屬性的值必須是固定且唯一的。 這表示遠端識別碼必須能一致地識別單一使用者帳戶，且應包含唯一的標記以保證不會與 PC 上其他連絡人的遠端識別碼衝突，包括其他應用程式擁有的連絡人。
> 如果您應用程式使用的遠端識別碼不保證固定且唯一，您可以使用 [RemoteIdHelper 類別](https://msdn.microsoft.com/en-us/library/windows/apps/jj207024(v=vs.105).aspx#BKMK_UsingtheRemoteIdHelperclass)，先將唯一標記新增至您所有的遠端識別碼，再新增到系統。 或者您可以選擇完全不使用 RemoteId 屬性，而是建立自訂延伸屬性，讓連絡人的遠端識別碼儲存在此屬性中。

除了第二個繫結與裝載外，您必須在第一個繫結包含另一個裝載，以做為遞補快顯通知  若強制還原回一般通知時將使用該通知 (在 [這份文件的結尾](https://review.docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications#falling-back-to-toast)進一步解釋)。

## <a name="creating-the-notification"></a>建立通知
您可以如同建立[快顯通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)般的建立朋友圈通知範本。

以下是如何使用靜態影像裝載建立朋友圈通知的範例：

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

您啟動通知時，則應如下所示：

![靜態影像通知](images/static-image-notification-small.gif)

以下是如何使用動畫 Spritesheet 裝載建立通知的範例： 此 Spritesheet 的畫面高度為 80 像素，我們將以每秒 25 個畫面的速度播放動畫。 我們將開始畫面設定為 15，並以 “src” 參數將靜態遞補影像提供給此畫面。 如果 Spritesheet 動畫無法顯示，則會使用遞補影像。

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

您啟動通知時，則應如下所示：

![spritesheet 通知](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>啟動通知
為了啟動朋友圈通知，我們必須將快顯通知範本轉換為 [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx) 物件。 當您已在 XML 檔案中定義快顯通知 (在此名為 "content.xml")，則可以使用此程式碼啟動快顯通知：

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

接著您可以使用此程式碼來建立並傳送快顯通知：

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>遞補快顯通知
有時候朋友圈通知會改為顯示成一般快顯通知。 在以下狀況下，朋友圈通知將遞補快顯通知：

+ 無法顯示通知
+ 收件者未啟用朋友圈通知
+ 寄件者的連絡人未釘選到接收者的工作列

如果朋友圈通知遞補快顯通知，則會忽略第二個朋友圈特有的繫結，並只使用第一個繫結來顯示快顯通知。 這就是在第一個快顯通知繫結中提供遞補裝載如此重要的原因。

## <a name="see-also"></a>也請參閱
+ [朋友圈通知範例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [新增朋友圈支援](my-people-support.md)
+ [調適性快顯通知](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [ToastNotification 類別](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)