---
author: mijacobs
Description: "調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。"
title: "調適型和互動式快顯通知"
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: d9808feeabfa4ffce19d0e669352331804dfd751
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adaptive-and-interactive-toast-notifications"></a>調適型和互動式快顯通知

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

調適型和互動式快顯通知可讓您建立包含更多內容、選擇性內嵌影像，及選擇性使用者互動的彈性快顯通知。

相較於舊版快顯通知範本目錄，調適型和互動式快顯通知模型具有以下更新：

-   在通知上包括按鈕和輸入項目選項。
-   針對主要快顯通知和每項動作提供三種不同的啟用類型。
-   針對特定案例 (包括警示、提醒及來電) 建立通知的選項。

**備註**  若要查看來自 Windows 8.1 和 Windows Phone 8.1 的舊版範本，請參閱[舊版快顯通知範本目錄](https://msdn.microsoft.com/library/windows/apps/hh761494)。


## <a name="getting-started"></a>開始使用

**安裝 Notifications 程式庫。** 如果您想要使用 C# 而不是 XML 產生通知，請安裝名稱為 [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) 的 NuGet 套件 (搜尋 "notifications uwp")。 本文中所提供的 C# 範例使用該 NuGet 套件 1.0.0 版本。

**安裝通知視覺化工具。** 這個免費的 UWP 應用程式透過在您編輯快顯通知時提供即時視覺預覽 (類似 Visual Studio 的 XAML 編輯器/設計檢視)，可協助您設計互動式快顯通知。 您可以閱讀[此部落格文章](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx)以取得詳細資訊，您也可以從[這裡](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)下載通知視覺化工具。


## <a name="toast-notification-structure"></a>快顯通知結構


快顯通知是使用 XML 建構，並通常包含下列重要元素：

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

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Sample"
                },
 
                new AdaptiveText()
                {
                    Text = "This is a simple toast notification example"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "oneAlarm.png"
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("check", "check")
            {
                ImageUri = "check.png"
            },
 
            new ToastButton("cancel", "cancel")
            {
                ImageUri = "cancel.png"
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = new Uri("ms-winsoundevent:Notification.Reminder")
    }
};
```

接著需要將快顯通知轉換為 [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx) 物件。 若您在 XML 檔案中定義快顯通知 (在此稱作 "content.xml")，請使用此程式碼：

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

或者，若您使用 C# 定義快顯通知範本，請使用此項：

```CSharp
XmlDocument xmlContent = content.GetXml();
```

無論您建立 XMLDocument 的方式為何，您都可使用此程式碼建立及傳送快顯通知：

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

若要查看可顯示快顯通知的完整應用程式，請參閱[傳送本機快顯通知的快速入門](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)。

此為結構的視覺化呈現方式：

![快顯通知結構](images/adaptivetoasts-structure.jpg)

### <a name="visual"></a>視覺

在視覺元素內部，必須有且只有一個包含快顯通知視覺內容的繫結元素。

通用 Windows 平台 (UWP) 應用程式中的通知依據不同的磚大小支援多種範本。 但是，快顯通知只有一個範本名稱：**ToastGeneric**。 只有一個範本名稱表示：

-   您可以變更快顯內容，例如新增另一行文字、新增內嵌影像，或將應用程式圖示的縮圖影像變更為顯示其他項目，而且執行這些動作時不需要擔心會變更整個範本，或擔心因為範本名稱和內容不符而建立無效的承載。
-   您可以使用相同的程式碼，針對要供不同類型 Microsoft Windows 裝置 (包括手機、平板電腦、電腦及 Xbox One) 使用的**快顯通知**建構相同的承載。 這些裝置每一個都將能夠接受通知，並依據其 UI 原則以適當的視覺能供性和互動模型方式向使用者顯示。

關於視覺區段和其子項目中支援的所有屬性，請參閱下面的＜結構描述＞一節。 如需更多範例，請參閱下面的＜XML 範例＞一節。

您應用程式的身分是透過應用程式圖示來傳達。 不過，如果您使用 appLogoOverride，則會在您的文字行下方顯示應用程式名稱。

| 標準快顯通知                                                                              | 使用 appLogoOverride 的快顯通知                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![未使用 appLogoOverride 的通知](images/adaptivetoasts-withoutapplogooverride.jpg) | ![使用 appLogoOverride 的通知](images/adaptivetoasts-withapplogooverride.jpg) |

### <a name="actions"></a>動作

在 UWP 應用程式中，您可以在快顯通知中加入按鈕或其他輸入項目，讓使用者在應用程式外部執行更多動作。 這些動作是在 &lt;actions&gt; 元素底下指定，有兩種類型可以指定：

-   &lt;action&gt; 這會在傳統型與行動裝置上顯示為按鈕。 您最多可以在快顯通知中指定五個自訂或系統動作。
-   &lt;input&gt; 這可以允許使用者提供輸入，例如快速回覆訊息，或從下拉式功能表中選取選項。

&lt;action&gt; 和 &lt;input&gt; 在 Windows 系列裝置內部皆具備調整彈性。 例如，在行動或傳統型裝置中，提供給使用者的 &lt;action&gt; 是可以點選/按一下的按鈕。 文字 &lt;input&gt; 則是一個方塊，使用者可使用實體鍵盤或螢幕小鍵盤在其中輸入文字。 這些元素也可以針對未來的互動案例 (例如透過語音宣告動作，或透過聽寫方式輸入文字) 調整。

當使用者採取動作時，您可以指定 &lt;action&gt; 元素內部的 [**ActivationType**](https://msdn.microsoft.com/library/windows/desktop/dn408447) 屬性來執行下列其中一項動作：

-   使用可用來瀏覽特定頁面/內容的動作特定引數在前景啟用應用程式。
-   在不影響使用者的情況下啟用應用程式的背景工作。
-   透過通訊協定啟動來啟用另一個應用程式。
-   指定要執行的系統動作。 目前可用的系統動作為延期及解除排定的鬧鐘/提醒，將在下面的小節中進一步說明。

關於視覺區段和其子項目中支援的所有屬性，請參閱下面的＜結構描述＞一節。 如需更多範例，請參閱下面的＜XML 範例＞一節。

### <a name="audio"></a>音訊

自訂音訊一直以來都支援行動裝置，也支援桌上型電腦版本 1511 (組建 10586) 或較新版本。 您可以透過下列方式參考自訂音訊︰

-   ms-appx:///
-   ms-appdata:///

或者，您可以從 [ms-winsoundevents 清單](https://msdn.microsoft.com/library/windows/apps/br230842) (英文) 中挑選，該清單中的項目支援這兩個平台。

如需快顯通知中音訊的相關資訊，請參閱[音訊結構頁面](https://msdn.microsoft.com/library/windows/apps/br230842) (英文)。 如需了解如何使用自訂音效傳送快顯通知，[請參閱此部落格文章](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/) (英文)。

## <a name="alarms-reminders-and-incoming-calls"></a>鬧鐘、提醒及來電


您可以針對鬧鐘、提醒及來電使用快顯通知。 這些特殊快顯通知的外觀和標準快顯通知一致，但是特殊快顯通知具備一些自訂、以案例為基礎的 UI 和圖案：

-   提醒快顯通知將停留在螢幕上，直到使用者將它關閉或採取動作。 在 Windows Mobile 上，提醒快顯通知也會以預先展開的方式顯示。
-   除了和提醒通知共用上述行為之外，鬧鐘通知也會自動播放循環音效。
-   來電通知會在 Windows Mobile 裝置上以全螢幕方式顯示。 這可以透過在以下的快顯通知根元素內部指定 scenario 屬性來完成 – &lt;toast&gt;: &lt;toast scenario=" { default | alarm | reminder | incomingCall } " &gt;

## <a name="xml-examples"></a>XML 範例


**備註**  這些範例的快顯通知螢幕擷取畫面取自傳統型裝置上的應用程式。 在行動裝置上，快顯通知可能會以摺疊的方式顯示，並在快顯通知底部顯示擷取器以展開通知。

 

**包含豐富視覺內容的通知**

此範例說明如何使用多行文字、使用選用小型影像複寫應用程式標誌，以及使用選用內嵌影像縮圖。

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Photo Share</text>
      <text>Andrew sent you a picture</text>
      <text>See it in full size!</text>
      <image src="https://unsplash.it/360/180?image=1043" />
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Photo Share"
                },
 
                new AdaptiveText()
                {
                    Text = "Andrew sent you a picture"
                },
 
                new AdaptiveText()
                {
                    Text = "See it in full size!"
                },
 
                new AdaptiveImage()
                {
                    Source = "https://unsplash.it/360/180?image=1043"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    }
};
```

![包含豐富視覺內容的通知](images/adaptivetoasts-xmlsample01.jpg)

 

**包含動作的通知**

這個範例使用兩種可能的回應動作建立通知。

```XML
<toast launch="app-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Microsoft Company Store</text>
      <text>New Halo game is back in stock!</text>
    </binding>
  </visual>
  <actions>
    <action activationType="foreground" content="See more details" arguments="details"/>
    <action activationType="background" content="Remind me later" arguments="later"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Microsoft Company Store"
                },
 
                new AdaptiveText()
                {
                    Text = "New Halo game is back in stock!"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "details"),
 
            new ToastButton("Remind me later", "later")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![包含動作的通知，範例 1](images/adaptivetoasts-xmlsample02.jpg)



**包含文字輸入和動作的通知，範例 1**

此範例建立會接受文字輸入連同兩個回應動作的通知。

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" />
    <action activationType="foreground" content="Video call" arguments="video" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Video call", "video")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![包含文字和輸入動作的通知](images/adaptivetoasts-xmlsample04.jpg)

 

**包含文字輸入和動作的通知，範例 2**

此範例建立會接受文字輸入及單一動作的通知。

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Andrew B.</text>
      <text>Shall we meet up at 8?</text>
      <image placement="appLogoOverride" src="https://unsplash.it/64?image=883" hint-crop="circle" />
    </binding>
  </visual>
  <actions>
    <input id="message" type="text" placeHolderContent="Type a reply" />
    <action activationType="background" content="Reply" arguments="reply" hint-inputId="message" imageUri="Assets/Icons/send.png"/>
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Andrew B."
                },
 
                new AdaptiveText()
                {
                    Text = "Shall we meet up at 8?"
                }
            },
 
            AppLogoOverride = new ToastGenericAppLogo()
            {
                Source = "https://unsplash.it/64?image=883",
                HintCrop = ToastGenericAppLogoCrop.Circle
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("message")
            {
                PlaceholderContent = "Type a reply"
            }
        },
 
        Buttons =
        {
            new ToastButton("Reply", "reply")
            {
                TextBoxId = "message",
                ImageUri = "Assets/Icons/send.png",
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

![包含文字輸入和動作的通知](images/adaptivetoasts-xmlsample05.jpg)

 

**包含選取項目輸入和動作的通知**

此範例建立含有下拉式選取範圍功能表以及兩個可能動作的通知。

```XML
<toast launch="developer-defined-string">
  <visual>
    <binding template="ToastGeneric">
      <text>Spicy Heaven</text>
      <text>When do you plan to come in tomorrow?</text>
    </binding>
  </visual>
  <actions>
    <input id="time" type="selection" defaultInput="2" >
      <selection id="1" content="Breakfast" />
      <selection id="2" content="Lunch" />
      <selection id="3" content="Dinner" />
    </input>
    <action activationType="background" content="Reserve" arguments="reserve" />
    <action activationType="foreground" content="Call Restaurant" arguments="call" />
  </actions>
</toast>
```

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Spicy Heaven"
                },
 
                new AdaptiveText()
                {
                    Text = "When do you plan to come in tomorrow?"
                }
            }
        }
    },
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "2",
                Items =
                {
                    new ToastSelectionBoxItem("1", "Breakfast"),
                    new ToastSelectionBoxItem("2", "Lunch"),
                    new ToastSelectionBoxItem("3", "Dinner")
                }
            }
        },
 
        Buttons =
        {
            new ToastButton("Reserve", "reserve")
            {
                ActivationType = ToastActivationType.Background
            },
 
            new ToastButton("Call Restaurant", "call")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

![包含選取項目輸入和動作的通知](images/adaptivetoasts-xmlsample06.jpg)

 

**提醒通知**

如同先前範例使用選取範圍功能表及兩個動作，我們便可建立提醒通知。

```XML
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  <visual>
    <binding template="ToastGeneric">
      <text>Adaptive Tiles Meeting</text>
      <text>Conf Room 2001 / Building 135</text>
      <text>10:00 AM - 10:30 AM</text>
    </binding>
  </visual>
 
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

```CSharp
ToastContent content = new ToastContent()
{
    Launch = "action=viewEvent&eventId=1983",
    Scenario = ToastScenario.Reminder,
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Adaptive Tiles Meeting"
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
    },
 
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

![提醒通知](images/adaptivetoasts-xmlsample07.jpg)

 

## <a name="handling-activation-foreground-and-background"></a>處理啟用 (前景和背景)

若要了解如何處理快顯通知啟用 (使用者按一下快顯通知或快顯通知上的按鈕)，請參閱[快速入門：傳送本機快顯通知及處理啟用](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10/)。


## <a name="schemas-ltvisualgt-and-ltaudiogt"></a>結構描述：&lt;visual&gt; 和 &lt;audio&gt;


在以下的 XML 結構描述中，"?" 尾碼表示屬性是選擇性的。

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Launch = ?,
    Duration = ?,
    ActivationType = ?,
    Scenario = ?,
 
    Visual = new ToastVisual()
    {
        Language = ?,
        BaseUri = ?,
        AddImageQuery = ?,
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = ?,
                    Language = ?,
                    HintMaxLines = ?
                },
 
                new AdaptiveGroup()
                {
                    Children =
                    {
                        new AdaptiveSubgroup()
                        {
                            HintWeight = ?,
                            HintTextStacking = ?,
                            Children =
                            {
                                new AdaptiveText(),
                                new AdaptiveImage()
                            }
                        }
                    }
                },
 
                new AdaptiveImage()
                {
                    Source = ?,
                    AddImageQuery = ?,
                    AlternateText = ?,
                    HintCrop = ?
                }
            }
        }
    },
 
    Audio = new ToastAudio()
    {
        Src = ?,
        Loop = ?,
        Silent = ?
    }
};
```

**&lt;toast 中的屬性&gt;**

launch?

-   launch? = string
-   這是選擇性屬性。
-   快顯通知啟動應用程式時，傳遞給應用程式的字串。
-   視 activationType 的值而定，此值可由在前景的應用程式於背景工作內部應用程式接收，或由透過通訊協定從原始應用程式啟動的其他應用程式接收。
-   此字串的格式和內容是由應用程式定義以供應用程式自己使用。
-   當使用者點選或按一下快顯通知來啟動其相關聯應用程式時，啟動字串會提供相關內容給應用程式，以允許應用程式向使用者顯示與快顯通知內容有關的檢視，而非以其預設方式啟動。
-   如果是因為使用者按下某個動作 (而非按一下快顯通知內文) 而啟用，開發人員會取回在該 &lt;action&gt; 標記中預先定義的 "arguments"，而非取回 &lt;toast&gt; 標記中預先定義的 "launch"。

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
-   不要只為了要讓通知在螢幕上持續顯示而使用此屬性。

**&lt;visual 中的屬性&gt;**

lang?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

baseUri?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

addImageQuery?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

**&lt;binding 中的屬性&gt;**

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

**&lt;text 中的屬性&gt;**

lang?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230847)，以了解此選擇性屬性的詳細資料。

**&lt;image 中的屬性&gt;**

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

**&lt;audio 中的屬性&gt;**

src?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230842)，以了解此選擇性屬性的詳細資料。

loop?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230842)，以了解此選擇性屬性的詳細資料。

silent?

-   請參閱[此元素結構描述文章](https://msdn.microsoft.com/library/windows/apps/br230842)，以了解此選擇性屬性的詳細資料。

## <a name="schemas-ltactiongt"></a>結構描述：&lt;action&gt;


在以下的 XML 結構描述中，"?" 尾碼表示屬性是選擇性的。

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

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("id")
            {
                Title = ?
                DefaultSelectionBoxItemId = ?,
                Items =
                {
                    new ToastSelectionBoxItem("id", "content")
                }
            },
 
            new ToastTextBox("id")
            {
                Title = ?,
                PlaceholderContent = ?,
                DefaultInput = ?
            }
        },
 
        Buttons =
        {
            new ToastButton("content", "args")
            {
                ActivationType = ?,
                ImageUri = ?,
                TextBoxId = ?
            },
 
            new ToastButtonSnooze("content")
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss("content")
        }
    }
};
```

**&lt;input 中的屬性&gt;**

id

-   id = string
-   這是必要屬性。
-   id 是必要屬性，並且是由開發人員用來在應用程式啟用之後抓取使用者輸入 (於前景或背景)。

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
-   如果輸入類型是 "selection"，此屬性預期為此輸入元素內部其中一個可用選取項目的 id。

**&lt;selection 中的屬性&gt;**

id

-   這是必要屬性。 這個屬性是用來識別使用者選取項目。 會傳回 id 給您的應用程式。

content

-   這是必要屬性。 這個屬性可為此選取項目元素提供字串以供顯示。

**&lt;action 中的屬性&gt;**

content

-   content = string
-   content 是必要屬性。 它會提供在按鈕上顯示的文字字串。

arguments

-   arguments = string
-   arguments 是必要屬性。 這個屬性描述使用者採取此動作以啟用應用程式之後，應用程式可抓取之應用程式定義的資料。

activationType?

-   activationType? = "foreground | background | protocol | system"
-   activationType 是選擇性屬性，且其預設值為 "foreground"。
-   這個屬性描述此動作將造成的啟動類型：前景、背景或透過通訊協定啟動來啟動其他應用程式，或叫用系統動作。

imageUri?

-   imageUri? = string
-   imageUri 是選擇性屬性，且是用來為此動作提供影像圖示，以供在按鈕內部和文字內容一起顯示。

hint-inputId

-   hint-inputId = string
-   hint-inpudId 是必要屬性。 這個屬性專門用於快速回覆案例。
-   值必須是要關聯之輸入屬性的 id。
-   在行動裝置與桌上型裝置中，這個屬性會將按鈕放在輸入方塊旁邊。

## <a name="attributes-for-system-handled-actions"></a>系統處理之動作的屬性


如果您不想要由應用程式以背景工作的方式處理通知的延期/重新排程工作，系統可以處理延期和關閉通知的動作。 系統處理的動作可以合併 (或個別指定)，但是我們不建議在沒有關閉動作的情況下實作延期動作。

系統命令組合：SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
    Actions = new ToastActionsSnoozeAndDismiss()
};
```

個別的系統處理的動作

```
<toast>
  <visual>
  </visual>
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

```
ToastContent content = new ToastContent()
{
    Visual = ...
 
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
                    new ToastSelectionBoxItem("10", "10 minutes"),
                    new ToastSelectionBoxItem("20", "20 minutes"),
                    new ToastSelectionBoxItem("30", "30 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour")
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
        -   讓輸入的 ID 和延遲動作的 hint-inputId 相符：&lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   將選取項目 id 指定為代表延期間隔 (分鐘) 的 nonNegativeInteger：&lt;selection id="240" /&gt; 表示延期 4 小時
        -   確定 &lt;input&gt; 中 defaultInput 的值與 &lt;selection&gt; 子元素的其中一個 id 相符
        -   提供最多 (但不要超過) 5 個 &lt;selection&gt; 值

 

 
## <a name="related-topics"></a>相關主題

* [快速入門：傳送本機快顯通知及處理啟用](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/07/08/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10.aspx)
* [GitHub 上的 Notifications 程式庫](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)