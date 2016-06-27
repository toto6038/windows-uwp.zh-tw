---
author: Karl-Bridge-Microsoft
Description: "了解如何在執行階段利用語音辨識結果，以存取和更新語音命令定義 (VCD) 檔案中支援的片語清單 (PhraseList 元素)。"
title: "動態修改 VCD 片語清單"
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modify VCD phrase lists
template: detail.hbs
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 623243b94cf8ef6b276f8f2971af7bbdbdece81c

---

# 動態修改 VCD 片語清單





**重要 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

在執行階段利用語音辨識結果來存取和更新語音命令定義 (VCD) 檔案中支援的片語清單 (**PhraseList** 元素)。

如果語音命令是專門針對涉及某種使用者定義或暫時性應用程式資料的工作，則在執行階段動態修改片語清單會相當有用。 

例如，假設您有一款旅遊 app，使用者可以輸入目的地，但您希望使用者只要說出：應用程式名稱 +「顯示行程&lt;目的地&gt;」，就可以啟動 app。 在 **ListenFor** 元素本身，您可以指定如下內容：`<ListenFor> Show trip to {destination}  </ListenFor>`，其中 "destination" 是 **PhraseList** 的 **Label** 屬性值。

在執行階段更新片語清單可評估為每個可能的目的地建立個別 **ListenFor** 元素的需求。 您可以改為在 **PhraseList** 中動態填入使用者輸入行程時指定的目的地。 

如需有關 **PhraseList** 及其他 VCD 元素的詳細資訊，請參閱 [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593) 參考資料。

**先決條件：  **

本主題改編自[利用 Cortana 語音命令啟動前景應用程式](launch-a-foreground-app-with-voice-commands-in-cortana.md)。 我們在這裡以 **Adventure Works** 這個行程安排和管理 app，繼續示範各項功能。

如果您是開發通用 Windows 平台 (UWP) app 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：  **

請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)取得相關資訊，了解如何將您的 App 與 **Cortana** 整合。您也可以參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)，取得有關設計既實用又吸引人且支援語音之 App 的有用提示。

## <span id="Identify_the_command"></span><span id="identify_the_command"></span><span id="IDENTIFY_THE_COMMAND"></span>識別命令及更新片語清單

這裡是一個 VCD 檔案檔案範例，它會定義一個「showTripToDestination」**命令**以及一個 **PhraseList** (此元素會在我們的 **Adventure Works** 旅遊應用程式中定義三個目的地選項)。 當使用者儲存和刪除應用程式中的目的地時，應用程式會更新 **PhraseList** 的選項。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>

```

若要更新 VCD 檔案中的 **PhraseList** 元素，請取得包含片語清單的 **CommandSet** 元素。 請使用該 **CommandSet** 元素的 **Name** 屬性 (**Name** 在 VCD 檔案中必須是唯一的) 做為存取 [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257) 屬性及取得 [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258) 參照的索引鍵。

在識別命令集之後，請取得您想要修改之片語清單的參照，然後使用 **PhraseList** 元素的 **Label** 屬性並以字串陣列做為新的片語清單內容來呼叫 [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) 方法。

**注意：**如果您修改片語清單，就會取代整個片語清單。 如果您想要將新項目插入到片語清單中，就必須在對 [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261) 的呼叫中同時指定現有的項目和新項目。

在這個範例中，我們會示範如何再加上鳳凰城目的地，以更新先前範例中所示的 **PhraseList**。

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>備註


如果集合不大或單字不多，比較適合利用 **PhraseList** 來限制辨識結果。 當字組太大 (例如，數百個文字)，或根本不應該限制時，可以使用 **PhraseTopic** 元素和 **Subject** 元素來縮小語音辨識結果的相關性，以提高延展性。

在我們的範例中，**PhraseTopic** 的 **Scenario** 是 "Search"，並以 "City\\State" 的 **Subject** 進一步精簡。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <span id="related_topics"></span>相關文章


**開發人員**
* [Cortana 互動](cortana-interactions.md)
* [利用 Cortana 語音命令啟動前景應用程式](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [利用 Cortana 語音命令啟動背景 app](launch-a-background-app-with-voice-commands-in-cortana.md)
* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**設計人員**
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


