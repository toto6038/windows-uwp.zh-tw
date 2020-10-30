---
description: 調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。
title: 快顯通知內容
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 快顯通知, 互動式快顯通知, 調適性快顯通知, 快顯通知內容, 快顯通知裝載
ms.localizationpriority: medium
ms.openlocfilehash: f148938f8c8e3bb5ac305a82d1863545005fd802
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034291"
---
# <a name="toast-content"></a>快顯通知內容

調適型和互動式快顯通知可讓您使用文字、影像和按鈕/輸入建立彈性通知。

> **重要 API** ： [UWP 社群工具組通知 NuGet 套件](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> 若要查看來自 Windows 8.1 和 Windows Phone 8.1 的舊版範本，請參閱[舊版快顯通知範本目錄](/previous-versions/windows/apps/hh761494(v=win.10))。


## <a name="getting-started"></a>開始使用

**安裝 Notifications 程式庫。** 如果您想要使用 C# 而不是 XML 產生通知，請安裝名稱為 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 套件 (搜尋 "notifications uwp")。 本文中所提供的 C# 範例使用該 NuGet 套件 1.0.0 版本。

**安裝通知視覺化工具。** 這個免費的 Windows 應用程式可協助您設計互動式快顯通知，方法是在編輯時提供快顯通知的立即視覺預覽，類似于 Visual Studio 的 XAML 編輯器/設計檢視。 如需詳細資訊，請參閱[通知視覺化工具](notifications-visualizer.md)或[從 Microsoft Store 下載通知視覺化工具](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)。


## <a name="sending-a-toast-notification"></a>傳送快顯通知

若要了解如何傳送通知，請參閱[傳送本機快顯通知](send-local-toast.md)。 本文件僅探討建立快顯通知內容。


## <a name="toast-notification-structure"></a>快顯通知結構

快顯通知是一些如 Tag/Group 等資料屬性 (讓您識別通知) 與 *快顯通知內容* 的組合。

快顯通知內容的核心元件為...
* **launch** ：這會定義使用者按一下快顯通知時，要將哪些引數傳回到 App，讓您深度連結至快顯通知已在顯示的正確內容中。 若要深入了解，請參閱[傳送本機快顯通知](send-local-toast.md)。
* **visual** ：快顯通知的視覺效果部分，其中會有包含文字和影像的泛型繫結。
* **actions** ：快顯通知的互動部分，包括輸入和動作。
* **audio** ：控制向使用者顯示快顯通知時播放的音效。

快顯通知內容是以原始 XML 定義，但是您可以使用我們的 [NuGet 程式庫](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 取得 C# (or C++) 物件模型來建構快顯通知內容。 本文記載快顯通知內容的相關資訊，應有盡有。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddToastActivationInfo("app-defined-string", ToastActivationType.Foreground)
    .AddText("Some text")
    .AddButton("Archive", ToastActivationType.Background, "archive")
    .AddAudio(new Uri("ms-appx:///Sound.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

以下是以快顯通知內容的視覺呈現方式：

![快顯通知結構](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Visual

每個快顯通知都必須指定視覺效果，您必須在其中提供可包含文字、影像等項目的泛型快顯通知繫結。 這些元素會呈現在各種不同 Windows 裝置，包括桌上型電腦、手機、平板電腦和 Xbox。

如需了解視覺區段及其子元素支援的所有屬性，請參閱[結構描述](toast-schema.md#toastvisual)。

快顯通知上的 App 身分識別會透過 App 圖示來傳達。 不過，如果您使用 App 標頭覆寫，則會在文字行下方顯示 App 名稱。

| 一般快顯通知的應用程式身分識別 | 具備 appLogoOverride 的應用程式身分識別 |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>文字元素

每個快顯都必須至少有一個文字專案，而且可以包含兩個額外的文字元素，全部都是 [**AdaptiveText**](toast-schema.md#adaptivetext)類型。

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

從 Windows 10 年度更新版以後，您可以使用文字的 **HintMaxLines** 屬性控制顯示多少行文字。 預設 (及上限) 為標題最多 2 行文字，以及兩個額外描述元素 (第二個和第三個 **AdaptiveText** ) 最多 4 行 (合併)。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddText("Adaptive Tiles Meeting", hintMaxLines: 1)
    .AddText("Conf Room 2001 / Building 135")
    .AddText("10:00 AM - 10:30 AM");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```

---


## <a name="app-logo-override"></a>App 標誌覆寫

根據預設，快顯通知會顯示 App 的標誌。 不過，您可以使用自己的 [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo) 映射覆寫此標誌。 例如，如果這是來自某個人的通知，我們建議以那個人的相片來覆寫 App 標誌。

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

您可以使用 **HintCrop** 屬性來變更影像的裁剪。 例如， **Circle** 會產生圓形裁剪的影像。 否則，影像為正方形。 100% 縮放比例的影像尺寸為 48x48 像素。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAppLogoOverride(new Uri("https://picsum.photos/48?image=883"), NotificationAppLogoCrop.Circle);
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```

---



## <a name="hero-image"></a>主角圖像

**年度更新版的新功能** ：快顯通知可以顯示主角影像，這是在快顯通知橫幅和控制中心內顯示得很醒目的精選 [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage)。 100% 縮放比例的影像尺寸為 364x180 像素。

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddHeroImage(new Uri("https://picsum.photos/364/180?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```

---


## <a name="inline-image"></a>內嵌影像

您可以提供會在您展開快顯通知時出現的全寬內嵌影像。

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```

---


## <a name="image-size-restrictions"></a>影像大小限制

您在快顯通知中使用的影像可源自...

 - http://
 - ms-appx:///
 - ms-appdata:///

對於 http 和 https 遠端網頁影像，每個個別影像的檔案大小有一些限制。 在 Fall Creators Update (16299) 中，我們將一般連線的限制提高到 3 MB，而計量付費連線的限制提高到 1 MB。 在此之前，影像一律限制於 200 KB。

| 一般連線 | 計量付費連線 | Fall Creators Update 之前 |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

如果影像超過檔案大小、無法下載或逾時，影像將被捨棄，並顯示通知的其餘部分。


## <a name="attribution-text"></a>屬性文字

**年度更新版的新功能** ：如果需要參考內容的來源，您可以使用屬性文字。 此文字永遠和通知身分識別或通知時間戳記一起顯示在通知的底部。

舊版 Windows 不支援文字屬性，文字只是簡單顯示成另一個文字元素 (假設文字元素尚未到達最多三個的限制)。

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAttributionText("Via SMS");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```

---


## <a name="custom-timestamp"></a>自訂時間戳記

**Creators Update 的新功能** ：您現在可以將系統提供的時間戳記覆寫為您自己的準確表示何時產生訊息/資訊/內容的時間戳記。 此時間戳記可顯示在控制中心內。

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

若要深入了解使用自訂時間戳記，請參閱[快顯通知上的自訂時間戳記](custom-timestamps-on-toasts.md)。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---


## <a name="progress-bar"></a>進度列

建立 **者更新的新** 功能：您可以提供快顯通知的進度列，讓使用者知道下載作業的進度。

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

若要深入了解使用進度列，請參閱[快顯通知進度列](toast-progress-bar.md)。


## <a name="headers"></a>標題

**Creators Update 的新功能** ：您可以將通知分組在控制中心的標頭下方。 例如，您可以將來自某個群組聊天的群組訊息分組在一個標頭下，或將常見主題的通知分組在一個標頭下，以此類推。

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

若要深入了解使用標頭，請參閱[快顯通知標頭](toast-headers.md)。


## <a name="adaptive-content"></a>調適性內容

**年度更新版的新功能** ：除了以上所指定的內容之外，您還可以顯示會在展開快顯通知時顯示的其他調適性內容。

這個額外內容是使用 Adaptive 所指定，您可以閱讀[調適型磚文件](create-adaptive-tiles.md) 進行深入了解。

請注意，任何自我調整內容都必須包含在 [**AdaptiveGroup**](./toast-schema.md#adaptivegroup)內。 否則無法使用 Adaptive 呈現出來。


### <a name="columns-and-text-elements"></a>欄和文字元素

以下是一個使用欄和一些進階調適性文字元素的範例。 由於文字元素是在 **AdaptiveGroup** 內，因此支援所有的豐富調適性樣式屬性。

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddVisualChild(new AdaptiveGroup()
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
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


## <a name="buttons"></a>按鈕

按鈕讓快顯通知具有互動功能，使用者可以用來對快顯通知採取快速動作，而不會中斷他們目前的工作流程。 例如，使用者可以直接從快顯通知中回覆訊息，甚至在沒有開啟電子郵件 App 的情況下刪除電子郵件。 按鈕顯示在通知的展開部分。

若要深入了解端對端實作按鈕，請參閱[傳送本機快顯通知](send-local-toast.md)。

按鈕可以執行下列其他動作...

-   使用可用來瀏覽至特定頁面/內容的引數，在前景啟用應用程式。
-   針對快速回覆或類似案例，啟用 App 的背景工作。
-   透過通訊協定啟動來啟用另一個應用程式。
-   執行系統動作，例如正在延遲或關閉通知。

> [!NOTE]
> 您最多只能有 5 個按鈕 (包括稍後會討論的操作功能表項目)。

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton("See more details", ToastActivationType.Foreground, "action=viewdetails&contentId=351")
    .AddButton("Remind me later", ToastActivationType.Background, "action=remindlater&contentId=351");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```

---


### <a name="buttons-with-icons"></a>具有圖示的按鈕

您可以將圖示新增到您的按鈕。 這些圖示是白色透明、100% 縮放比例的 16 x 16 像素影像，且影像本身不應包含邊框間距。 如果您選擇在快顯通知上提供圖示，您必須為通知中的所有按鈕都提供圖示，因為這會將按鈕樣式轉換為圖示按鈕。

> [!NOTE]
> 對於協助工具，請務必包含圖示的高對比白色版本 (白色背景的黑色圖示)，以便當使用者開啟 [高對比白色] 模式時，您的圖示可以顯示出來。 請至[快顯通知協助工具頁面](tile-toast-language-scale-contrast.md)深入了解。

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton(
        "Dismiss",
        ToastActivationType.Foreground,
        "dismiss", new Uri("Assets/NotificationButtonIcons/Dismiss.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<action
    content="Dismiss"
    imageUri="Assets/NotificationButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```

---


### <a name="buttons-with-pending-update-activation"></a>具有擱置中更新啟用的按鈕

**Fall Creators Update 的新功能** ：在背景啟動按鈕上，您可以使用 **PendingUpdate** 的啟用後行為在快顯通知中建立多步驟互動。 當使用者按下您的按鈕時，便會啟用背景工作，快顯通知將進入「擱置中的更新」狀態，它會停留在螢幕上，直到背景工作使用新的快顯通知來取代快顯通知。

若要了解如何執行此程序，請參閱[快顯通知擱置中的更新](toast-pending-update.md)。

![具有擱置更新的快顯通知](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>內容功能表動作

**年度更新版的新功能** ：您可以新增額外的內容功能表動作至現有的內容功能表 (當使用者以滑鼠右鍵按一下控制中心中的快顯通知，便會顯示此內容功能表)。 請注意，只有在控制中心內以滑鼠右鍵按一下時，才會出現這個功能表。 以滑鼠右鍵按一下快顯通知快顯橫幅並不會出現此功能表。

> [!NOTE]
> 在舊款裝置上，這些額外的內容功能表動作會在您的快顯通知上顯示為一般按鈕。

您新增 (的其他內容功能表動作（例如「變更位置」 ) 會出現在兩個預設系統專案上方。

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

產生器語法不支援內容功能表動作，因此建議使用初始化運算式語法。

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

---

> [!NOTE]
> 額外內容功能表項目計入快顯通知上的 5 個按鈕總限制。

額外內容功能表項目的啟用處理方式與快顯通知按鈕相同。


## <a name="inputs"></a>輸入

輸入是在快顯通知的 [動作] 區域中指定，也就是只有在展開快顯通知時才能看得到。


### <a name="quick-reply-text-box"></a>快速回覆文字方塊

若要啟用快速回復文字方塊 (例如，在訊息應用程式中) 新增文字輸入和按鈕，並參考文字輸入欄位的識別碼，讓按鈕顯示在輸入欄位的旁邊。 按鈕的圖示應為32x32 圖元影像，沒有填補、白色圖元設定為透明，以及100% 縮放比例。

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton(
        textBoxId: "tbReply", // To place button next to text box, reference text box's id
        content: "Reply",
        activationType: ToastActivationType.Background,
        arguments: "action=reply&convId=9318",
        imageUri: new Uri("Assets/Reply.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```

---



### <a name="inputs-with-buttons-bar"></a>使用按鈕列的輸入

您也可以讓一個 (或多個) 輸入與一般按鈕一起顯示在輸入下方。

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton("Reply", ToastActivationType.Background, "action=reply&threadId=9218")
    .AddButton("Video call", ToastActivationType.Foreground, "action=videocall&threadId=9218");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```

---


### <a name="selection-input"></a>選取輸入

除了文字方塊之外，您還可以使用選取功能表。

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddToastInput(new ToastSelectionBox("time")
    {
        DefaultSelectionBoxItemId = "lunch",
        Items =
        {
            new ToastSelectionBoxItem("breakfast", "Breakfast"),
            new ToastSelectionBoxItem("lunch", "Lunch"),
            new ToastSelectionBoxItem("dinner", "Dinner")
        }
    })

    .AddButton(...)
    .AddButton(...);
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---



### <a name="snoozedismiss"></a>延遲/關閉

我們可以使用選取功能表，建立採用系統延遲及關閉動作的提醒通知。 請務必將案例設定為 Reminder，才能讓通知的行為表現得像提醒。

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

我們在快顯通知按鈕上使用 **SelectionBoxId** 屬性，將 [延遲] 按鈕連結至選項功能表輸入。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    
    ...
    
    .AddToastInput(new ToastSelectionBox("snoozeTime")
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
    })

    .AddButton(new ToastButtonSnooze() { SelectionBoxId = "snoozeTime" })
    .AddButton(new ToastButtonDismiss());
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

若要使用系統延遲和關閉動作：

-   指定 **ToastButtonSnooze** 或 **ToastButtonDismiss**
-   選擇性指定自訂內容字串：
    -   如果沒有提供字串，我們會自動使用 "Snooze" (延遲) 和 "Dismiss" (關閉) 的當地語系化字串。
-   選擇性指定 **SelectionBoxId** ：
    -   如果您不想讓使用者選取延遲間隔，而只想讓您的通知延遲一段系統定義的時間間隔 (整個系統一致)，那就不要建置任何 &lt;input&gt;。
    -   如果您想要提供延遲間隔選取項目：
        -   在延遲動作中指定 **SelectionBoxId**
        -   將輸入的 id 與延遲動作的 **SelectionBoxId** 比對
        -   將 **ToastSelectionBoxItem** 的值指定為 nonNegativeInteger，這表示以分鐘為單位的延遲間隔。



## <a name="audio"></a>音訊

自訂音訊一直以來都支援行動裝置，也支援桌上型電腦版本 1511 (組建 10586) 或較新版本。 您可以透過下列方式參考自訂音訊︰

-   ms-appx:///
-   ms-appdata:///

或者，您可以從 [ms-winsoundevents 清單](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements) (英文) 中挑選，該清單中的項目支援這兩個平台。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAudio(new Uri("ms-appx:///Assets/NewMessage.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

---


如需快顯通知中音訊的相關資訊，請參閱[音訊結構頁面](/uwp/schemas/tiles/toastschema/element-audio) (英文)。 如需了解如何使用自訂音效傳送快顯通知，請參閱[自訂快顯通知的音效](custom-audio-on-toasts.md)。


## <a name="alarms-reminders-and-incoming-calls"></a>鬧鐘、提醒及來電

若要建立鬧鐘、提醒和來電通知，只需使用一般快顯通知並指派其案例值即可。 案例會調整一些行為，以便建立一致且整合的使用者體驗。

> [!IMPORTANT]
> 使用提醒或鬧鐘時，您必須在您的快顯通知上至少提供一個按鈕。 否則，快顯通知會被視為一般的快顯通知。

* **Reminder** ：提醒快顯通知將停留在螢幕上，直到使用者將它關閉或採取動作。 在 Windows Mobile 上，快顯通知也會以預先展開的方式顯示。 將會播放提醒音效。
* **Alarm** ：除了提醒行為之外，鬧鐘還會另外使用預設鬧鐘音效，循環重複播放音效。
* **IncomingCall** ：來電通知會在 Windows Mobile 裝置上以全螢幕方式顯示。 否則，除了使用鈴聲音效以外，其行為與鬧鐘相同，其按鈕也會以不同方式設定樣式。

#### <a name="builder-syntax"></a>[Builder 語法](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```

---


## <a name="localization-and-accessibility"></a>當地語系化和協助工具

您的磚和快顯通知可以載入針對顯示語言、顯示縮放比例、高對比及其他執行階段內容量身打造的字串與影像。 如需詳細資訊，請參閱[對語言、縮放比例及高對比的磚與快顯通知支援](tile-toast-language-scale-contrast.md)。


## <a name="handling-activation"></a>處理啟用
如需了解如何處理快顯通知啟用 (使用者按一下您的快顯通知或快顯通知上的按鈕)，請參閱[傳送本機快顯通知](send-local-toast.md)。
 
## <a name="related-topics"></a>相關主題

* [傳送本機快顯通知及處理啟用](send-local-toast.md)
* [GitHub 上的 Notifications 程式庫 (屬於 UWP 社群工具組)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [對語言、縮放比例及高對比的磚和快顯通知支援](tile-toast-language-scale-contrast.md)
