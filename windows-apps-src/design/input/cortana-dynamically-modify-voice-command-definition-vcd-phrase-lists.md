---
title: 動態修改 Cortana VCD 片語清單-Cortana UWP 設計與開發
description: 使用語音辨識結果，在執行時間 (VCD) 檔中，存取並更新支援的片語清單 (PhraseList 元素) 在語音命令定義中。
ms.assetid: b497145b-c7a0-454a-8329-6bc1228953bb
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: 5c28330a975b66c2f04baba1d0df58b38d684356
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824412"
---
# <a name="dynamically-modify-cortana-vcd-phrase-lists"></a>動態修改 Cortana VCD 片語清單

>[!WARNING]
> Windows 10 2020 版更新 (2004 版 codename "20H1" ) ，不再支援此功能。
>
> 查看 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

使用語音辨識結果，在執行時間 (VCD) 檔中，存取並更新支援的片語清單 (**PhraseList** 元素) 在語音命令定義中。

> [!NOTE]
> 語音命令是具有特定意圖的單一語句，定義于語音命令定義 (VCD) 檔，透過 **Cortana** 導向已安裝的應用程式。
>
> VCD 檔案會定義一或多個語音命令，每個命令都有獨特的意圖。
>
> 語音命令定義可能會有不同的複雜度。 它們可以支援從單一、受限的語句到更具彈性的自然語言語句集合的任何作業，而且全都代表相同的意圖。

如果語音命令特定于涉及某種使用者定義或暫時性應用程式資料的工作，則在執行時間動態修改片語清單會很有用。

> [!NOTE]
> **重要 API**
>
> - [**ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和屬性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

例如，假設您有一個旅遊應用程式，使用者可以在其中輸入目的地，而您想要讓使用者能夠藉由說出應用程式名稱，後面接著「顯示前往目的地」來啟動應用程式 &lt; &gt; 。 在 **ListenFor** 元素本身中，您會指定如下的內容： `<ListenFor> Show trip to {destination}  </ListenFor>` ，其中 "Destination" 是 **PhraseList** 的 **Label** 屬性值。

在執行時間更新片語清單，就不需要為每個可能的目的地建立個別的 **ListenFor** 元素。 相反地，您可以使用使用者在輸入路線時所指定的目的地，以動態方式填入 **PhraseList** 。

如需 **PhraseList** 和其他 vcd 元素的詳細資訊，請參閱 [**VCD 元素和屬性 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 版參考。

> [!TIP]
> **先決條件**
>
> 如果您是開發通用 Windows 平台 (UWP) App 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。
>
> - [建立您的第一個應用程式](../../get-started/your-first-app.md)
> - 請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)，以了解事件相關資訊
>
> **使用者經驗指導方針**
>
> 請參閱 [cortana 設計指導方針](cortana-design-guidelines.md)  ，以瞭解如何整合您的應用程式與 **Cortana** 和 [語音互動](speech-interactions.md) ，以取得設計實用且具備語音功能的應用程式的實用秘訣。

## <a name="identify-the-command-and-update-the-phrase-list"></a>識別命令並更新片語清單

以下是範例的 VCD 檔案，它會定義 **命令**"showTripToDestination"，以及在我們的 **艾德公司** 旅遊應用程式中定義目的地三個選項的 **PhraseList** 。 當使用者儲存及刪除應用程式中的目的地時，應用程式會更新 **PhraseList** 中的選項。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
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

若要更新 VCD 檔案中的 **PhraseList** 元素，請取得包含片語清單的 **CommandSet** 元素。 使用該 **CommandSet** 專案的 **Name** 屬性 (**名稱** 在 VCD 檔案中必須是唯一的，) 為存取 [**VoiceCommandManager InstalledCommandSets**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandManager)屬性和取得 [**VoiceCommandSet**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)參考的索引鍵。

在您識別命令集之後，請取得您想要修改之片語清單的參考，然後呼叫 [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)方法。使用 **PhraseList** 專案的 **Label** 屬性和字串陣列做為片語清單的新內容。

> [!NOTE]
> 如果您修改片語清單，則會取代整個片語清單。 如果您想要將新的專案插入片語清單中，您必須在 [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)的呼叫中指定現有的專案和新專案。

在此範例中，我們會將前一個範例中所示的 **PhraseList** 更新為 Phoenix 的其他目的地。

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <a name="remarks"></a>備註

使用 **PhraseList** 來限制辨識適用于較小的一組或單字。 當單字組太大 (上百個字組（例如) ，或根本不受限制）時，請使用 **PhraseTopic** 元素和 **主旨元素來** 精簡語音辨識結果的相關性，以改善擴充性。

在我們的範例中，我們有一個具有「搜尋」**案例** 的 **PhraseTopic** ，並以「城市狀態」**主體** 進一步調整 \\ 。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
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

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [透過 Cortana 語音命令啟用前景應用程式](cortana-launch-a-foreground-app-with-voice-commands.md)
- [使用語音命令在 Cortana 中啟動背景應用程式](cortana-launch-a-background-app-with-voice-commands.md)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)