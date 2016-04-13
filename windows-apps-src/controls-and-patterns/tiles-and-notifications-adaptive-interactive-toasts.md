---
調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。
調適型和互動式快顯通知
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
調適型和互動式快顯通知
template: detail.hbs
---

# 調適型和互動式快顯通知


\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。

相較於舊版快顯通知範本目錄，調適型和互動式快顯通知模型具有以下更新：

-   在通知上包括按鈕和輸入項目選項。
-   針對主要快顯通知和每項動作提供三種不同的啟用類型。
-   針對特定案例 (包括警示、提醒及來電) 建立通知的選項。

**注意：**若要看到來自 Windows 8.1 和 Windows Phone 8.1 的舊版範本，請參閱[舊版快顯通知範本資料目錄](https://msdn.microsoft.com/library/windows/apps/hh761494)。

 

## <span id="toast_structure"> </span> <span id="TOAST_STRUCTURE"> </span>快顯通知結構


使用 XML 建構的快顯通知通常包含三種重要元素：

-   &lt;visual&gt; 涵蓋可供使用者目視檢視的內容，包括文字和影像
-   &lt;actions&gt; 包含開發人員要在通知中加入的按鈕/輸入項目
-   &lt;audio&gt; 會指定通知快顯時播放的音效

以下是程式碼範例：

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Sample</text>
      <text>This is a simple toast notification example</text>
      <image placement="AppLogoOverride" src="oneAlarm.png" />
    </binding>
  </visual>
  <actions>
    <action content="check" arguments="check" imageUri="check.png" />
    <action content="cancel" arguments="cancel" />
  </actions>
  <audio src="ms-winsoundevent:Notification.Reminder"/>
</toast>
```

以及結構的視覺化呈現方式：

![快顯通知結構](images/adaptivetoasts-structure.jpg)

### <span id="Visual"> </span> <span id="visual"> </span> <span id="VISUAL"> </span>視覺

在視覺元素內部，必須有且只有一個包含快顯通知視覺內容的繫結元素。

通用 Windows 平台 (UWP) App 中的通知依據不同的磚大小支援多種範本。 但是，快顯通知只有一個範本名稱：**ToastGeneric**。 只有一個範本名稱表示：

-   您可以變更快顯內容，例如新增另一行文字、新增內嵌影像，或將應用程式圖示的縮圖影像變更為顯示其他項目，而且執行這些動作時不需要擔心會變更整個範本，或擔心因為範本名稱和內容不符而建立無效的承載。
-   您可以使用相同的程式碼，針對要供不同類型 Microsoft Windows 裝置 (包括手機、平板電腦、電腦及 Xbox One) 使用的 **toast notification** 建構相同的承載。 這些裝置每一個都將能夠接受通知，並依據其 UI 原則以適當的視覺能供性和互動模型方式向使用者顯示。

關於視覺區段和其子項目中支援的所有屬性，請參閱下面的＜結構描述＞一節。 如需更多範例，請參閱下面的＜XML 範例＞一節。

### <span id="Actions"> </span> <span id="actions"> </span> <span id="ACTIONS"> </span>動作

在 UWP App 中，您可以在快顯通知中加入按鈕或其他輸入項目，讓使用者在 App 外部執行更多動作。 這些動作是在 &lt;actions&gt; 元素底下指定，有兩種類型可以指定：

-   &lt;action&gt; 會在桌面與行動裝置上顯示為按鈕。 您最多可以在快顯通知中指定五個自訂或系統動作。
-   &lt;input&gt; 可以允許使用者提供輸入 (例如快速回覆訊息)，或從下拉式功能表中選取選項。

&lt;action&gt; 和 &lt;input&gt; 在 Windows 系列裝置內部皆具備調整彈性。 例如在行動或桌上型裝置中，提供給使用者的 &lt;action&gt; 是可以點選/按一下的按鈕。 文字 &lt;input&gt; 則是一個方塊，使用者可使用實體鍵盤或螢幕小鍵盤在其中輸入文字。 這些元素也可以針對未來的互動案例 (例如透過語音宣告動作，或透過聽寫方式輸入文字) 調整。

當使用者採取動作時，您可以指定 &lt;action&gt; 元素內部的 [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) 屬性來執行下列其中一項動作：

-   使用可用來瀏覽特定頁面/內容的動作特定引數在前景啟用應用程式。
-   在不影響使用者的情況下啟用 app 的背景工作。
-   透過通訊協定啟動來啟用另一個 app。
-   指定要執行的系統動作。 目前可用的系統動作為延期及解除排定的鬧鐘/提醒，將在下面的小節中進一步說明。

關於視覺區段和其子項目中支援的所有屬性，請參閱下面的＜結構描述＞一節。 如需更多範例，請參閱下面的＜XML 範例＞一節。

### <span id="Audio"> </span> <span id="audio"> </span> <span id="AUDIO"> </span>音訊

針對桌面平台開發的 UWP App 目前不支援自訂音效；您可以針對您為桌面平台開發的應用程式從 ms-winsoundevents 的清單中選擇。 行動平台上的 UWP App 支援這兩種 ms-winsoundevents，以及以下格式的自訂音效：

-   ms-appx:///
-   ms-appdata:///

請參閱[音訊結構描述頁面](https://msdn.microsoft.com/library/windows/apps/br230842)了解快顯通知音訊的相關資訊，包括完整的 ms-winsoundevents 清單。

## <span id="Alarms__reminders__and_incoming_calls"> </span> <span id="alarms__reminders__and_incoming_calls"> </span> <span id="ALARMS__REMINDERS__AND_INCOMING_CALLS"> </span>鬧鐘、提醒及來電


您可以針對鬧鐘、提醒及來電使用快顯通知。 這些特殊快顯通知的外觀和標準快顯通知一致，但是特殊快顯通知具備一些自訂、以案例為基礎的 UI 和圖案：

-   提醒快顯通知將停留在螢幕上，直到使用者將它關閉或採取動作。 在 Windows Mobile 上，提醒快顯通知也會以預先展開的方式顯示。
-   除了和提醒通知共用上述行為之外，鬧鐘通知也會自動播放循環音效。
-   來電通知會在 Windows Mobile 裝置上以全螢幕方式顯示。 這可透過在以下的快顯通知根元素內部指定案例屬性來完成 – &lt;toast&gt;:
    &lt;toast scenario=" { default | alarm | reminder | incomingCall } " &gt;

## <span id="xml_examples"> </span> <span id="XML_EXAMPLES"> </span>XML 範例


**注意：**這些範例快顯通知螢幕擷取畫面取自桌面應用程式。 在行動裝置上，快顯通知可能會以摺疊的方式顯示，並在快顯通知底部顯示擷取器以展開通知。

 

**包含豐富視覺內容的通知**

此範例說明如何使用多行文字、使用選用小型影像複寫應用程式標誌，以及使用選用內嵌影像縮圖。

```XML
<toast launch="app-defined-string">
  <visual>
<binding template="ToastGeneric">
    <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image placement="appLogoOverride" src="A.png" />
    <image placement="inline" src="hiking.png" />
    </binding>
  </visual>
</toast>
```

![包含豐富視覺內容的通知](images/adaptivetoasts-xmlsample01.png)

 

**包含動作的通知，範例 1**

這個範例顯示...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="see more details" arguments="details" imageUri="check.png"/>
    <action activationType="background" content="remind me later" arguments="later" imageUri="cancel.png"/>
  </actions>
</toast>
```

![包含動作的通知，範例 1](images/adaptivetoasts-xmlsample02.png)

 

**包含動作的通知，範例 2**

這個範例顯示...

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Cortana</text>
      <text>We noticed that you are near Wasaki.</text>
      <text>Thomas left a 5 star rating after his last visit, do you want to try?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="reviews" arguments="reviews" />
    <action activationType="protocol" content="show map" arguments="bingmaps:?q=sushi" />
  </actions>
</toast>
```

![包含動作的通知，範例 2](images/adaptivetoasts-xmlsample03.png)

 

**包含文字輸入和動作的通知，範例 1**

這個範例顯示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="reply here" />
    <action activationType="background" content="reply" arguments="reply" />
    <action activationType="foreground" content="video call" arguments="video" />
  </actions>
</toast>
```

![包含文字和輸入動作的通知](images/adaptivetoasts-xmlsample04.png)

 

**包含文字輸入和動作的通知，範例 2**

這個範例顯示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="reply here" />
    <action activationType="background" content="reply" arguments="reply" imageUri="send.png" hint-inputId="message"/>
  </actions>
</toast>
```

![包含文字輸入和動作的通知](images/adaptivetoasts-xmlsample05.png)

 

**包含選取項目輸入和動作的通知**

這個範例顯示...

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
      <image placement="appLogoOverride" src="A.png" />
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
  <selection id="1" content="Breakfast" />
  <selection id="2" content="Lunch" />
  <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="background" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

![包含選取項目輸入和動作的通知](images/adaptivetoasts-xmlsample06.png)

 

**提醒通知**

這個範例顯示...

```XML
<toast scenario="reminder" launch="developer-pre-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Adam&#39;s Hiking Camp</text>
      <text>You have an upcoming event for this Friday!</text>
      <text>RSVP before it"s too late.</text>
      <image placement="appLogoOverride" src="A.png" />
      <image placement="inline" src="hiking.png" />
    </binding>
  </visual>
  <actions>
    <action activationType="background" content="RSVP" arguments="rsvp" />
    <action activationType="background" content="Reminder me later" arguments="later" />
  </actions>
</toast>
```

![提醒通知](images/adaptivetoasts-xmlsample07.png)

 

## <span id="Activation_samples"> </span> <span id="activation_samples"> </span> <span id="ACTIVATION_SAMPLES"> </span>啟用範例


如上述說明，快顯通知的內文和動作可以透過不同方式啟動應用程式。 下面的範例將說明如何處理來自快顯通知內文和/或快顯通知動作的不同類型啟用。

**前景**

在這個案例中，app 會透過啟動 app 並瀏覽至正確內容，以使用前景啟用來回應可執行動作之快顯通知內部的動作。

從用來叫用 OnLaunched() 的快顯通知啟用。 在 Windows 10 中，快顯通知有自己的啟用類型且將會叫用 OnActivated()。

```
async protected override void OnActivated(IActivatedEventArgs args)
{
        //Initialize your app if it&#39;s not yet initialized;
    //Find out if this is activated from a toast;
    If (args.Kind == ActivationKind.ToastNotification)
    {
                //Get the pre-defined arguments and user inputs from the eventargs;
        var toastArgs = args as ToastNotificationActivatedEventArgs;
        var arguments = toastArgs.Arguments;
        var input = toastArgs.UserInput["1"]; 
}
     
    //...
}
```

**背景**

在這個案例中，app 會使用背景工作來處理可執行動作之快顯通知內部的動作。 下面的程式碼說明如何宣告此背景工作，以處理您應用程式資訊清單內部的快顯通知啟用，以及說明如何在按鈕被按下時從動作和使用者輸入取得引數。

```
<!-- Manifest Declaration -->
<!-- A new task type toastNotification is added -->
<Extension Category = "windows.backgroundTasks" 
EntryPoint = "Tasks.BackgroundTaskClass" >
  <BackgroundTasks>
    <Task Type="systemEvent" />
  </BackgroundTasks>
</Extension>
```

```
namespace ToastNotificationTask
{
    public sealed class ToastNotificationBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
        //Inside here developer can retrieve and consume the pre-defined 
        //arguments and user inputs;
        var details = taskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
        var arguments = details.Arguments;
        var input = details.Input.Lookup("1");

            // ...
        }        
    }
}
```

## <span id="Schemas___visual__and__audio_"> </span> <span id="schemas___visual__and__audio_"> </span> <span id="SCHEMAS___VISUAL__AND__AUDIO_"> </span>結構描述：&lt;visual&gt; 和 &lt;audio&gt;


在以下的結構描述中，"?" 尾碼表示屬性是選擇性的。

```
<toast launch? duration? activationType? scenario? >
    <visual version? lang? baseUri? addImageQuery? >
        <binding template? lang? baseUri? addImageQuery? >
            <text lang? >content</text>
            <text />
            <image src placement? alt? addImageQuery? hint-crop? />
        </binding>
    </visual>
    <audio src? loop? silent? />
    <actions>
    </actions>
</toast>
```

**&lt;toast&gt; 中的屬性**

launch?

-   launch? = string
-   這是選擇性屬性。
-   快顯通知啟動應用程式時，傳遞給應用程式的字串。
-   視 activationType 的值而定，此值可由在前景的 app 於背景工作內部 app 接收，或由透過通訊協定從原始 app 啟動的其他 app 接收。
-   此字串的格式和內容是由 app 定義以供 app 自己使用。
-   當使用者點選或按一下快顯通知來啟動其相關聯應用程式時，啟動字串會提供相關內容給應用程式，以允許應用程式向使用者顯示與快顯通知內容有關的檢視，而非以其預設方式啟動。
-   如果是因為使用者按下某個動作 (而非按一下快顯通知內文) 而啟用，開發人員可取回該 &lt;action&gt; 標記中預先定義的 "arguments"，而非取回 &lt;toast&gt; 標記中預先定義的 "launch"。

duration?

-   duration? = "short|long"
-   這是選擇性屬性。 預設值為 "short"。
-   這個屬性僅供特定案例和 appCompat 使用。 對於鬧鐘案例，您已經不需要再用到這個屬性。
-   我們不建議使用這個屬性。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   這是選擇性屬性。
-   預設值為 "foreground"。

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   這是選擇性屬性，預設值為 "default"。
-   除非您的案例是要快顯鬧鐘、提醒或來電，否則您不需要使用此屬性。
-   不要只因為要讓通知在螢幕上持續顯示而使用此屬性。

**&lt;visual&gt; 中的屬性**

version?

-   version? = nonNegativeInteger
-   此屬性非必要，因為在 &lt;visual&gt; 中將會取代版本設定。 如有需要，請持續留意您將可從較高階層指定的新版本設定模型。

lang?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

baseUri?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

addImageQuery?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

**&lt;binding&gt; 中的屬性**

template?

-   [Important] template? = "ToastGeneric"
-   如果您使用新的調適型和互動式通知，請確定您是從使用 "ToastGeneric" 範本開始，而非使用舊版範本。
-   目前可能可以搭配新動作使用舊版範本，但那並非預期的使用情況，因此我們無法保證該使用方式將會持續有效。

lang?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

baseUri?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

addImageQuery?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

**&lt;text&gt; 中的屬性**

lang?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

**&lt;image&gt; 中的屬性**

src

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230844)，以了解此必要屬性的詳細資料。

placement?

-   placement? = "inline" | "appLogoOverride"
-   這是選擇性屬性。
-   此屬性可指定將顯示影像的位置。
-   "inline" 表示位於快顯通知內文內部、文字下方; "appLogoOverride" 表示取代應用程式圖示 (顯示於快顯通知左上角)。
-   您可以為每個位置值最多提供一個影像。

alt?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230844)，以了解此選擇性屬性的詳細資料。

addImageQuery?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230844)，以了解此選擇性屬性的詳細資料。

hint-crop?

-   hint-crop? = "none" | "circle"
-   這是選擇性屬性。
-   "none" 為預設值，表示沒有裁剪。
-   "circle" 會將影像裁剪為圓形。 請針對連絡人設定檔影像、人員影像等項目使用此屬性。

**&lt;audio&gt; 中的屬性**

src?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230842)，以了解此選擇性屬性的詳細資料。

loop?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230842)，以了解此選擇性屬性的詳細資料。

silent?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230842)，以了解此選擇性屬性的詳細資料。

## <span id="Schemas___action_"> </span> <span id="schemas___action_"> </span> <span id="SCHEMAS___ACTION_"> </span>結構描述：&lt;action&gt;


在以下的結構描述中，"?" 尾碼表示屬性是選擇性的。

```
<toast>
    <visual>
    </visual>
    <audio />
    <actions>
        <input id type title? placeHolderContent? defaultInput? >
            <selection id content />
        </input>
        <action content arguments activationType? imageUri? hint-inputId />
    </actions>
</toast>
```

**&lt;input&gt; 中的屬性**

id

-   id = string
-   這是必要屬性。
-   id 是必要屬性，並且是由開發人員用來在 app 啟用之後抓取使用者輸入 (於前景或背景)。

type

-   type = "text | selection"
-   這是必要屬性。
-   這個屬性是用來指定文字輸入或來自預先定義選取項目之清單的輸入。
-   在行動裝置與桌上型裝置上，這是用來指定您是要使用文字方塊輸入或清單方塊輸入。

title?

-   title? = string
-   title 是選擇性屬性，且是供開發人員在有能供性時，指定要轉譯之 Shell 的輸入的標題。
-   對於行動裝置和桌上型裝置，此標題將顯示在輸入上方。

placeHolderContent?

-   placeHolderContent? = string
-   placeHolderContent 是選擇性屬性，且是文字輸入類型的灰色提示文字。 當輸入類型不是 "text" 時，會忽略此屬性。

defaultInput?

-   defaultInput? = string
-   defaultInput 是選擇性屬性，且是用來提供預設輸入值。
-   如果輸入類型是 "text"，此屬性將被視為字串輸入。
-   如果輸入類型是 "selection"，此屬性會預期為此輸入元素內部其中一個可用選取項目的 id。

**&lt;selection&gt; 中的屬性**

id

-   這是必要屬性。 這個屬性是用來識別使用者選取項目。 會傳回 id 給您的 app。

content

-   這是必要屬性。 這個屬性可為此選取項目元素提供字串以供顯示。

**&lt;action&gt; 中的屬性**

content

-   content = string
-   content 是必要屬性。 它會提供在按鈕上顯示的文字字串。

arguments

-   arguments = string
-   arguments 是必要屬性。 這個屬性描述使用者採取此動作以啟用 app 之後，app 可抓取之 app 定義的資料。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   activationType 是選擇性屬性，且其預設值為 "foreground"。
-   這個屬性描述此動作將造成的啟動類型：前景、背景或透過通訊協定啟動來啟動其他 app，或叫用系統動作。

imageUri?

-   imageUri? = string
-   imageUri 是選擇性屬性，且是用來為此動作提供影像圖示，以供在按鈕內部和文字內容一起顯示。

hint-inputId

-   hint-inputId = string
-   hint-inpudId 是必要屬性。 這個屬性專門用於快速回覆案例。
-   值必須是要關聯之輸入屬性的 id。
-   在行動裝置與桌上型裝置中，這個屬性會將按鈕放在輸入方塊旁邊。

## <span id="Attributes_for_system-handled_actions"> </span> <span id="attributes_for_system-handled_actions"> </span> <span id="ATTRIBUTES_FOR_SYSTEM-HANDLED_ACTIONS"> </span>適用於系統處理動作的屬性


如果您不想要由應用程式以背景工作的方式處理通知的延遲/重新排程工作，系統可以處理延遲和關閉通知的動作。 系統處理的動作可以合併 (或個別指定)，但是我們不建議在沒有關閉動作的情況下實作延期動作。

系統命令組合：SnoozeAndDismiss

```
<toast>
    <visual>
    </visual>
    <audio />
    <actions hint-systemCommands? = "SnoozeAndDismiss" />
</toast>
```

個別的系統處理的動作

```
<toast>
    <visual>
    </visual>
    <audio />
<actions>
<input id="snoozeTime" type="selection" defaultInput="10">
  <selection id="5" content="5 minutes" />
  <selection id="10" content="10 minutes" />
  <selection id="20" content="20 minutes" />
  <selection id="30" content="30 minutes" />
  <selection id="60" content="1 hour" />
</input>
<action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
<action activationType="system" arguments="dismiss" content=""/>
</actions>
</toast>
```

若要建構個別的延期和關閉動作，請執行以下作業：

-   指定 activationType = "system"
-   指定 arguments = "snooze" | "dismiss"
-   指定內容：
    -   如果您想要在動作上顯示當地語系化的 "snooze" 和 "dismiss" 字串，請將內容指定為空白字串：&lt;action content = ""/&gt;
    -   如果您想要自訂字串，只要提供其值即可：&lt;action content="Remind me later" /&gt;
-   指定輸入：
    -   如果您不想讓使用者選取延遲間隔，而只想讓您的通知延遲一段系統定義的時間間隔 (整個系統一致)，那就不要建置任何 &lt;input&gt;。
    -   如果您想要提供延遲間隔選取項目：
        -   請在延遲動作中指定 hint-inputId
        -   讓輸入的 id 和延遲動作的 hint-inputId 相符：&lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   將選取項目 id 指定為代表延遲間隔 (分鐘) 的 nonNegativeInteger：&lt;selection id="240" /&gt; 表示延遲 4 小時
        -   確定 &lt;input&gt; 中 defaultInput 的值與 &lt;selection&gt; 子元素的其中一個 id 相符
        -   提供最多 (但不要超過) 5 個 &lt;selection&gt; 值

 

 






<!--HONumber=Mar16_HO1-->


