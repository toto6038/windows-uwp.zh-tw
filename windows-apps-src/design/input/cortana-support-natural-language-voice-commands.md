---
title: 在 Cortana 中支援更自然的語音命令-Cortana UWP 設計與開發
description: 使用更具彈性且自然的語音命令來擴充 Cortana，讓使用者可以在命令中的任何位置說出您的應用程式名稱。
author: kbridge
label: Conceptual
ms.assetid: c2959c1b-c2f2-4a8d-8f3e-79585f69afcf
ms.date: 01/28/2021
ms.topic: article
keywords: cortana
ms.openlocfilehash: c6777a0c7ad9a8e2658b759e516b659bbd4eb9a9
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824502"
---
# <a name="support-natural-language-voice-commands-in-cortana"></a>支援 Cortana 的自然語音命令

>[!WARNING]
> Windows 10 2020 版更新 (2004 版 codename "20H1" ) ，不再支援此功能。
>
> 查看 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

使用更具彈性且自然的語音命令來擴充 **Cortana** ，讓使用者可以在命令中的任何位置說出您的應用程式名稱。

> [!NOTE]
> **重要 API**
>
> - [**ApplicationModel. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和屬性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

使用語音命令以您應用程式中的功能延伸 Cortana，需要使用者指定要執行的應用程式和命令或函式。 這通常是藉由在 voice 命令的開頭或結尾宣告應用程式名稱來完成。 例如，「艾德公司」會將新的旅程加入至拉斯維加斯。」

不過，以這種方式指定應用程式名稱，可能會讓您的麻煩、生硬或甚至沒有意義。 在許多情況下，可以在命令中的其他地方說出應用程式名稱是更舒適且自然的，並且有助於讓互動更加直覺且吸引使用者。 我們先前的範例「艾德公司將新旅程加入至拉斯維加斯」。 可以改換措辭為「加入新的艾德公司工作旅程給拉斯維加斯」。 或「使用艾德公司的工作，將新旅程新增至拉斯維加斯。」

您可以設定語音命令，以支援應用程式名稱作為：

- 前置詞-在命令片語之前
- 內置-在命令片語中
- 尾碼-在命令片語之後

> [!TIP]
> **先決條件**
>
> 本主題是 [以使用語音命令在 Cortana 中啟用背景應用程式為](cortana-launch-a-background-app-with-voice-commands.md)基礎。 我們將在這裡繼續示範具有旅程計畫和管理應用程式（名為「 **艾德作品**」）的功能。
>
> 如果您是開發通用 Windows 平台 (UWP) App 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。
>
> - [建立您的第一個應用程式](../../get-started/your-first-app.md)
> - 請參閱[事件與路由事件概觀](../../xaml-platform/events-and-routed-events-overview.md)，以了解事件相關資訊
>
> **使用者經驗指導方針**
>
> 請參閱 [cortana 設計指導方針](cortana-design-guidelines.md)  ，以瞭解如何整合您的應用程式與 **Cortana** 和 [語音互動](speech-interactions.md) ，以取得設計實用且具備語音功能的應用程式的實用秘訣。

## <a name="specify-an-appname-element-in-the-vcd"></a>在 VCD 中指定 **AppName** 元素

**AppName** 元素可用來在 voice 命令中指定應用程式的使用者易記名稱。

```XML
<AppName>Adventure Works</AppName>
```

## <a name="specify-where-the-app-name-can-be-spoken-in-the-voice-command"></a>指定可在 voice 命令中說出應用程式名稱的位置

**ListenFor** 元素具有 **RequireAppName** 屬性，可指定應用程式名稱在 voice 命令中的顯示位置。 這個屬性支援四個值。

1. **BeforePhrase**

   預設值。

   指出使用者必須在命令片語之前說出您的應用程式名稱。

   在這裡，Cortana 會接聽「當我的旅程到拉斯維加斯的時候」。

   ```xml
   <ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
   ```

2. **AfterPhrase**

    指出使用者在命令片語之後必須說出您的應用程式名稱。

    介係詞結合的當地語系化片語清單是由系統所提供。 這包括「使用」和「開啟」這類片語。

    在這裡，Cortana 會接聽像是「顯示我的下一步至內華達州的拉斯維加斯」的命令，以及「使用艾德作品向拉斯維加斯顯示下一步」。

    ```xml
    <ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
    ```

3. **BeforeOrAfterPhrase**

    指出使用者必須說出您的應用程式名稱在命令片語之前或之後。

    針對尾碼版本，系統會提供介係詞結合的當地語系化片語清單。 這包括「使用」和「開啟」這類片語。

    在這裡，Cortana 會接聽「艾德公司」、顯示下一次前往拉斯維加斯的旅程」或「顯示我的下一步到上一次的艾德作品」等命令。

    ``` xml
    <ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
    ```

4. **ExplicitlySpecified**

    指出使用者必須確切地說出您的應用程式名稱，您在命令片語中指定的位置。 使用者不需要說出片語之前或之後的應用程式名稱。

    您必須使用 **{內建： AppName}** 標記明確參考您的應用程式名稱。

    在這裡，Cortana 會接聽「艾德公司」、顯示下一次前往拉斯維加斯的旅程，或「顯示我的下一步到拉斯維加斯的工作旅程」等命令。

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
    ```

## <a name="special-cases"></a>特殊案例

當您宣告 **RequireAppName** 為 "AfterPhrase" 或 "ExplicitlySpecified" 的 **ListenFor** 專案時，您必須確定符合特定的需求：

1. **{內建： AppName}** 必須在 **RequireAppName** 為 "ExplicitlySpecified" 時，才會出現一次。

    使用此值時，系統無法推斷應用程式名稱可以出現在 voice 命令中的位置。 您必須明確指定位置。

2. 您不能使用 **PhraseTopic** 元素開頭的語音命令，這通常用於大型詞彙語音辨識。 至少必須要有一個字。

    這有助於將 **Cortana** 啟動應用程式的機會降到最低，如果命令中包含您的應用程式名稱或部分，語句中的任何位置。

    以下是不正確宣告，如果使用者說的是「針對 Kinect 艾德作品顯示評論」，可能會導致 **Cortana** 啟動「 **艾德作品** 」應用程式。
  
    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
    ```

3. 除了您的應用程式名稱和 **PhraseTopic** 元素的參考外， **ListenFor** 字串中必須至少有兩個單字。

    類似于案例2，您必須確保您的命令包含足夠的語音內容，以將您的應用程式意外啟動的機會降到最低。

    這可協助您設定應用程式，以獲得最佳的成功，讓您的應用程式不會在使用者說「尋找 Kinect 艾德公司」時，不會發生錯誤地啟動。

    以下是不正確宣告，如果使用者說的是「嗨，艾德公司」或「尋找 Kinect 艾德作品」之類的內容，可能會導致 **Cortana** 啟動「 **冒險」工作** 應用程式。

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
    ```

## <a name="remarks"></a>備註

在 **Cortana** 中的使用者從未提到語音命令的方式中支援更大的變化，也會增加應用程式的一般可用性。

避免將「嘿 \[ 應用程式名稱」 \] 作為 **AppName**。 使用者更可能會說：「嗨 Cortana」透過語音啟用叫用 Cortana，而 \[ 語句中的「嗨應用程式名稱」並 \] 不自然。 例如，「嗨，Cortana，請向我的下一步說，嗨

請考慮將中置/後置詞的變化新增至現有的語音命令。 如這裡所示，將額外的屬性新增至您現有的 **ListenFor** 專案並支援尾碼變異並不需要太多努力。 更自然的說：「嘿，「嗨 Cortana」、「我的下一次旅程」與「嘿」、「您好的 Cortana」、「您好的公司」、「我的下一次旅程」。

請考慮使用您的應用程式名稱做為前置詞，以在語音命令與現有 **Cortana** 功能相衝突的情況下， (呼叫、訊息傳送等) 。 例如，「艾德公司，訊息 \[ 旅遊代理程式 \] 關於內華達州旅程」。

## <a name="complete-example"></a>完整範例

以下是一個 VCD 檔案，示範提供更自然語言語音命令的各種方式。

> [!NOTE]
> 具有多個 **ListenFor** 元素（每個專案都有不同的 **RequireAppName** 屬性值）是有效的。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [Cortana 設計指導方針](cortana-design-guidelines.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)