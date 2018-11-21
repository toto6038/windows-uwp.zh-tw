---
author: lex
Description: The following article describes all of the properties and elements within the toast content XML payload.
title: 快顯通知內容 XML 結構描述
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcfc56264ab3063995fd9f2b06bd93e9406cd37e
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7439947"
---
# <a name="toast-content-xml-schema"></a>快顯通知內容 XML 結構描述

 

以下說明快顯通知內容 XML 承載中的所有屬性和元素。

在以下的 XML 結構描述中，"?" 尾碼表示屬性是選擇性的。

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;visual&gt; 和 &lt;audio&gt;

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

**&lt;toast&gt; 中的屬性**

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

**&lt;visual&gt; 中的屬性**

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

**&lt;input&gt; 中的屬性**

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

**&lt;selection&gt; 中的屬性**

id

-   這是必要屬性。 這個屬性是用來識別使用者選取項目。 會傳回 id 給您的應用程式。

content

-   這是必要屬性。 這個屬性可為此選取項目元素提供字串以供顯示。

**&lt;action&gt; 中的屬性**

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