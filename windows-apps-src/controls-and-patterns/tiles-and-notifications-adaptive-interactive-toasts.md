---
author: mijacobs
Description: "調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。"
title: "調適型和互動式快顯通知"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: c8e77773b9118c3177dc958ddc7b51d32a452fa5
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2017
---
# <a name="adaptive-and-interactive-toast-notifications"></a>調適型和互動式快顯通知

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

調適型和互動式快顯通知可讓您使用文字、影像和按鈕/輸入建立彈性通知。

> **重要 API**：[UWP Community Toolkit Notifications NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> 若要查看來自 Windows 8.1 和 Windows Phone 8.1 的舊版範本，請參閱[舊版快顯通知範本目錄](https://msdn.microsoft.com/library/windows/apps/hh761494)。


## <a name="getting-started"></a>開始使用

**安裝 Notifications 程式庫。** 如果您想要使用 C# 而不是 XML 產生通知，請安裝名稱為 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 套件 (搜尋 "notifications uwp")。 本文中所提供的 C# 範例使用該 NuGet 套件 1.0.0 版本。

**安裝通知視覺化工具。** 這個免費的 UWP 應用程式透過在您編輯快顯通知時提供即時視覺預覽 (類似 Visual Studio 的 XAML 編輯器/設計檢視)，可協助您設計互動式快顯通知。 您可以閱讀[此部落格文章](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)以取得詳細資訊，也可以從[這裡](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)下載通知視覺化工具。


## <a name="sending-a-toast-notification"></a>傳送快顯通知

若要了解如何傳送通知，請參閱[傳送本機快顯通知](tiles-and-notifications-send-local-toast.md)。 本文件僅探討建立快顯通知內容。


## <a name="toast-notification-structure"></a>快顯通知結構

快顯通知是一些如 Tag/Group 等資料屬性 (讓您識別通知) 與*快顯通知內容*的組合。

快顯通知內容的核心元件為...
* **launch**：這會定義使用者按一下快顯通知時，要將哪些引數傳回到 App，讓您深度連結至快顯通知已在顯示的正確內容中。 若要深入了解，請參閱[傳送本機快顯通知](tiles-and-notifications-send-local-toast.md)。
* **visual**：快顯通知的視覺效果部分，其中會有包含文字、影像和 App 標誌的泛型繫結。
* **actions**：快顯通知的互動部分，包括輸入和動作。
* **audio**：控制向使用者顯示快顯通知時播放的音效。

快顯通知內容是以原始 XML 定義，但是您可以使用我們的 [NuGet 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 取得 C# (or C++) 物件模型來建構快顯通知內容。 本文記載快顯通知內容的相關資訊，應有盡有。

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

以下是以快顯通知內容的視覺呈現方式：

![快顯通知結構](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>視覺效果

每個快顯通知都必須指定視覺效果，您必須在其中提供可包含文字、影像、標誌等項目的泛型快顯通知繫結。 這些元素會呈現在各種不同 Windows 裝置，包括桌上型電腦、手機、平板電腦和 Xbox。

如需了解視覺區段及其子元素支援的所有屬性，請參閱[結構描述](tiles-and-notifications-toast-schema.md#toastvisual)。

快顯通知上的 App 身分識別會透過 App 圖示來傳達。 不過，如果您使用 App 標頭覆寫，則會在文字行下方顯示 App 名稱。

| 標準快顯通知                                                                              | 使用 appLogoOverride 的快顯通知                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![未使用 appLogoOverride 的通知](images/adaptivetoasts-withoutapplogooverride.jpg) | ![使用 appLogoOverride 的通知](images/adaptivetoasts-withapplogooverride.jpg) |


## <a name="text-elements"></a>文字元素

每個快顯通知都必須至少有一個文字元素，並且可以包含兩個類型皆為 [AdaptiveText](tiles-and-notifications-toast-schema.md#adaptivetext) 的額外文字元素。

![使用標題及描述的快顯通知](images/toast-title-and-description.jpg)

從年度更新版以後，您可以使用文字的 **HintMaxLines** 屬性控制顯示多少行文字。 標題預設會顯示最多 2 行文字，而且每一描述行顯示最多 4 行文字。

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>App 標誌覆寫

根據預設，快顯通知會顯示 App 的標誌。 不過，您可以將此標誌覆寫為您自己的 [ToastGenericAppLogo](tiles-and-notifications-toast-schema.md#toastgenericapplogo) 影像。 例如，如果這是來自某個人的通知，我們建議以那個人的相片來覆寫 App 標誌。

![使用 App 標誌覆寫的快顯通知](images/toast-applogooverride.jpg)

您可以使用 **HintCrop** 屬性來變更影像的裁剪。 例如，*circle* 會產生圓形裁剪的影像。 否則，影像為正方形。 100% 縮放比例的影像尺寸為 64x64 像素。

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://unsplash.it/64?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://unsplash.it/64?image=883"/>
</binding>
```


## <a name="hero-image"></a>主角圖像

**年度更新版的新功能**：快顯通知可以顯示主角影像，這是在快顯通知橫幅和控制中心內顯示得很醒目的精選 [ToastGenericHeroImage](tiles-and-notifications-toast-schema.md#toastgenericheroimage)。 100% 縮放比例的影像尺寸為 360x180 像素。

![使用主角影像的快顯通知](images/toast-heroimage.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastHeroImage()
    {
        Source = "https://unsplash.it/360/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://unsplash.it/360/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>內嵌影像

您可以提供會在您展開快顯通知時出現的全寬內嵌影像。

![使用其他影像的快顯通知](images/toast-additionalimage.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://unsplash.it/360/180?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://unsplash.it/360/180?image=1043" />
</binding>
```


## <a name="attribution-text"></a>屬性文字

**年度更新版的新功能**：如果需要參考內容的來源，您可以使用屬性文字。 此文字永遠和通知身分識別或通知時間戳記一起顯示在通知的底部。

舊版 Windows 不支援文字屬性，文字只是簡單顯示成另一個文字元素 (假設文字元素尚未到達最多三個的限制)。

![使用文字屬性的快顯通知](images/toast-attributiontext.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>自訂時間戳記

**Creators Update 的新功能**：您現在可以將系統提供的時間戳記覆寫為您自己的準確表示何時產生訊息/資訊/內容的時間戳記。 此時間戳記可顯示在控制中心內。

![使用自訂時間戳記的快顯通知](images/toast-customtimestamp.jpg)

若要深入了解使用自訂時間戳記，請參閱此[部落格文章](https://blogs.msdn.microsoft.com/tiles_and_toasts/2017/01/09/custom-timestamp-on-toast-notifications-windows-10-creators-update/)。

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="adaptive-content"></a>調適性內容

**年度更新版的新功能**：除了以上所指定的內容之外，您還可以顯示會在展開快顯通知時顯示的其他調適性內容。

這個額外內容是使用 Adaptive 所指定，您可以閱讀[調適型磚文件](tiles-and-notifications-create-adaptive-tiles.md) 進行深入了解。

請注意，任何調適性內容都必須包含在 AdaptiveGroup 中。 否則無法使用 Adaptive 呈現出來。


### <a name="columns-and-text-elements"></a>欄和文字元素

以下是一個使用欄和一些進階調適性文字元素的範例。 文字元素是在 AdaptiveGroup 內，因此支援所有的豐富調適性樣式屬性。

![使用其他文字的快顯通知](images/toast-additionaltext.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="inputs-and-buttons"></a>輸入和按鈕

輸入與按鈕是在快顯通知的 [動作] 區域中指定，也就是只有在展開快顯通知時才能看得到。


### <a name="quick-reply-text-box"></a>快速回覆文字方塊

若要啟用快速回覆文字方塊 (例如針對訊息中心案例啟用)，請新增輸入文字和按鈕，並參考文字輸入的 id，讓按鈕顯示在輸入旁。

![包含文字輸入和動作的通知](images/adaptivetoasts-xmlsample05.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Send",
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>使用按鈕列的輸入

您也可以讓一個 (或多個) 輸入與一般按鈕一起顯示在輸入下方。

![包含文字和輸入動作的通知](images/adaptivetoasts-xmlsample04.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Reply",
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call",
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>選取輸入

除了文字方塊之外，您還可以使用選取功能表。

![包含選取輸入和動作的通知](images/adaptivetoasts-xmlsample06.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


## <a name="buttons"></a>按鈕

按鈕讓快顯通知具有互動功能，使用者可以用來對快顯通知採取快速動作，而不會中斷他們目前的工作流程。 例如，使用者可以直接從快顯通知中回覆訊息，甚至在沒有開啟電子郵件 App 的情況下刪除電子郵件。

按鈕可以執行下列其他動作...

-   使用可用來瀏覽至特定頁面/內容的引數，在前景啟用應用程式。
-   針對快速回覆或類似案例，啟用 App 的背景工作。
-   透過通訊協定啟動來啟用另一個應用程式。
-   執行系統動作，例如延遲或關閉通知。

請注意，您最多只能有 5 按鈕 (包括稍後會討論的操作功能表項目)。

![包含動作的通知，範例 1](images/adaptivetoasts-xmlsample02.jpg)

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details",
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later",
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="snoozedismiss-buttons"></a>延遲/關閉按鈕

我們可以使用選取功能表，建立採用系統延遲及關閉動作的提醒通知。 請務必將案例設定為 Reminder，才能讓通知的行為表現得像提醒。

![提醒通知](images/adaptivetoasts-xmlsample07.jpg)

我們在快顯通知按鈕上使用 *SepectionBoxId* 屬性，將 [延遲] 按鈕連結至選項功能表輸入。

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

若要使用系統延遲和關閉動作，請執行下列動作：

-   指定 ToastButtonSnooze 或 ToastButtonDismiss
-   選擇性指定自訂內容字串：
    -   如果沒有提供字串，我們會自動使用 "Snooze" (延遲) 和 "Dismiss" (關閉) 的當地語系化字串。
-   選擇性指定 *SelectionBoxId*：
    -   如果您不想讓使用者選取延遲間隔，而只想讓您的通知延遲一段系統定義的時間間隔 (整個系統一致)，那就不要建置任何 &lt;input&gt;。
    -   如果您想要提供延遲間隔選取項目：
        -   在延遲動作中指定 *SelectionBoxId*
        -   將輸入的 id 與延遲動作的 *SelectionBoxId* 比對
        -   將 *ToastSelectionBoxItem* 的值指定為 nonNegativeInteger，這表示以分鐘為單位的延遲間隔。



## <a name="audio"></a>音效

自訂音訊一直以來都支援行動裝置，也支援桌上型電腦版本 1511 (組建 10586) 或較新版本。 您可以透過下列方式參考自訂音訊︰

-   ms-appx:///
-   ms-appdata:///

或者，您可以從 [ms-winsoundevents 清單](https://msdn.microsoft.com/library/windows/apps/br230842) (英文) 中挑選，該清單中的項目支援這兩個平台。

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

如需快顯通知中音訊的相關資訊，請參閱[音訊結構頁面](https://msdn.microsoft.com/library/windows/apps/br230842) (英文)。 如需了解如何使用自訂音效傳送快顯通知，[請參閱此部落格文章](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/) (英文)。


## <a name="alarms-reminders-and-incoming-calls"></a>鬧鐘、提醒及來電

若要建立鬧鐘、提醒和來電通知，只需使用一般快顯通知並指派其案例值即可。 案例會調整一些行為，以便建立一致且整合的使用者體驗。

* **Reminder**：提醒快顯通知將停留在螢幕上，直到使用者將它關閉或採取動作。 在 Windows Mobile 上，快顯通知也會以預先展開的方式顯示。 將會播放提醒音效。
* **Alarm**：除了提醒行為之外，鬧鐘還會另外使用預設鬧鐘音效，循環重複播放音效。
* **IncomingCall**：來電通知會在 Windows Mobile 裝置上以全螢幕方式顯示。 否則，除了使用鈴聲音效以外，其行為與鬧鐘相同。

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="handling-activation"></a>處理啟用
如需了解如何處理快顯通知啟用 (使用者按一下您的快顯通知或快顯通知上的按鈕)，請參閱[傳送本機快顯通知](tiles-and-notifications-send-local-toast.md)。


 
## <a name="related-topics"></a>相關主題

* [傳送本機快顯通知及處理啟用](tiles-and-notifications-send-local-toast.md)
* [GitHub 上的 Notifications 程式庫](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)