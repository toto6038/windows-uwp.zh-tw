---
author: Karl-Bridge-Microsoft
Description: "了解如何利用更具彈性且自然的語音命令來延伸 Cortana 的功能，讓使用者可以在命令中的任何地方說出應用程式名稱。"
title: "支援 Cortana 的自然語音命令"
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
ms.sourcegitcommit: 077fcc6ff462a771ed56f875d960e46e6f4420fc
ms.openlocfilehash: c9cd6894c61f3d6cfe770d197317a97a804b4119

---

# 支援 Cortana 的自然語音命令

利用更具彈性且自然的語音命令來延伸 **Cortana** 的功能，讓使用者在命令中的任何地方說出應用程式名稱。

**重要 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**語音命令定義 (VCD) 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


使用語音命令以透過您應用程式的功能延伸 Cortana，使用者必須指出要執行的應用程式和命令或功能。 您可以在語音命令的開頭或結尾宣告應用程式名稱，通常就可以搞定了。 例如「Adventure Works，新增一趟前往拉斯維加斯的行程安排。」

不過，以此方式指定應用程式名稱，聽起來可能很怪異、生硬或者根本不知所云。 在許多情況下，我們可以在命令的其他地方，自然順口說出應用程式名稱，而且使用者會感覺互動時更直接了當、更生動有趣。 我們先前的範例「Adventure Works，新增一趟到拉斯維加斯的行程。」 或許可以是「新增一趟到拉斯維加斯的 Adventure Works 行程。」 或「用 Adventure Works 新增一趟到拉斯維加斯的行程。」

您可以設定語音命令來支援將應用程式名稱做為：

-   前置詞 - 命令片語前面
-   中置詞 - 命令片語裡面
-   後置詞 - 命令片語後面

**先決條件：  **

本主題的基礎是[利用 Cortana 語音命令啟動背景應用程式](launch-a-background-app-with-voice-commands-in-cortana.md)。 我們在這裡以 **Adventure Works** 這個行程安排和管理 app，繼續示範各項功能。

如果您是開發通用 Windows 平台 (UWP) app 的新手，請仔細閱讀這些主題以熟悉這裡討論的技術。

-   [建立您的第一個 App](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584)，以了解事件相關資訊

**使用者體驗指導方針：  **

請參閱 [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)取得相關資訊，了解如何將您的 App 與 **Cortana** 整合。您也可以參閱[語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)，取得有關設計既實用又吸引人且支援語音之 App 的有用提示。

## <span id="Specify_an_AppName_element_in_the_VCD"></span><span id="specify_an_appname_element_in_the_vcd"></span><span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"></span>指定 VCD 中的 **AppName** 元素


**AppName** 元素是用來在語音命令中，指定應用程式好聽又容易記得住的名稱。
```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"></span>指定應該在語音命令的什麼地方說出應用程式名稱。


**ListenFor** 元素有一個 **RequireAppName** 屬性，它可以指定應用程式名稱可以在語音命令中的出現位置。 這個屬性支援四個值。

1.  **BeforePhrase**

    預設值。

    指示使用者必須在命令片語前面說出應用程式名稱。

    這樣，Cortana 會聆聽「Adventure Works 我到拉斯維加斯的行程是什麼時候」
```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    指示使用者必須在命令片語後面說出應用程式名稱。

    系統會提供介係詞和連接詞的當地語系化片語清單。 例如，其中其中包括「用」、「以」和「在」等片語。

    這樣，Cortana 會聆聽「在 Adventure Works 顯示我下一趟到拉斯維加斯的行程」以及「用 Adventure Works 顯示我下一趟到拉斯維加斯的行程」。
```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    指示使用者必須在命令片語前面或後面說出應用程式名稱。

    至於後置詞版本，系統會提供介係詞和連接詞的當地語系化片語清單。 例如，其中其中包括「用」、「以」和「在」等片語。

    這樣，Cortana 會聆聽「Adventure Works，顯示我下一趟到拉斯維加斯的行程」或「在 Adventure Works 顯示我下一趟到拉斯維加斯的行程」等命令。
``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    指示使用者嚴格按照您在命令片語中指定的地方，說出應用程式名稱。 使用者不需要在片語前面或後面，說出應用程式名稱。

    您必須利用 **{builtin:AppName}** 標記，明確引用您的應用程式名稱。

    這樣，Cortana 會聆聽「Adventure Works，顯示我下一趟到拉斯維加斯的行程」或「顯示我下一趟到拉斯維加斯的 Adventure Works 行程」等命令。
```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"></span><span id="special_cases"></span><span id="SPECIAL_CASES"></span>特殊情況

當您宣告 **ListenFor** 元素而且 **RequireAppName** 是「AfterPhrase」或「ExplicitlySpecified」時，您必須確定符合特定規定：

1.  當 **RequireAppName** 為 "ExplicitlySpecified" 的時候，**{builtin:AppName}** 必須出現一次，而且只有一次。

    使用這個值的時候，系統無法推斷應用程式名稱出現在語音命令中的位置。 您必須明確指定位置。

2.  語音命令的開頭不可以是 **PhraseTopic** 元素，通常是只有大型詞彙語音辨識才會用到。 它的前面至少要有一個單字。

    這可以儘量避免當您說出語音命令時，其中包含您的應用程式名稱或名稱的某個部分，而 **Cortana** 啟動您的應用程式。

    如果使用者說出的語音命令類似於「顯示我的 Kinect adventure works 評論」，則其為無效的宣告，可能會導致 **Cortana** 啟動 **Adventure Works** 應用程式。
```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  除了您的應用程式名稱以及引用 **PhraseTopic** 元素之外，**ListenFor** 中至少還要有兩個單字。

    和第 2 種情況類似，當您說出語音命令時，內容一定要十分具體而且清楚，這樣不小心啟動應用程式的機會就會大減。

    這樣一來您安裝的應用程式就會有最佳的表現，而且例如使用者說：「尋找 Kinect Adventure works」，應用程式也不會意外啟動。

    這些是無效的宣告，而且假設使用者說：「嗨，adventure works」或「尋找 Kinect adventure works」，還會導致 **Cortana** 啟動 **Adventure Works** app。
```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>備註

在 **Cortana** 中支援更多種使用者可以表達的語音命令，也可以增加您 app 整體的可用性。

請不要將「嗨 \[應用程式名稱\]」當做您的 **AppName** 或 **CommandPrefix**。 使用者比較有可能會說：「嗨 Cortana」，為的就是想透過語音啟動的方式來呼叫 Cortana。但是，整句話中的「嗨 \[應用程式名稱\]」，聽起來不自然。 例如，「嗨 Cortana，在 Hey Adventure Works 顯示我下一趟到拉斯維加斯的行程。」

您可以在現有的語音命令中，加上中置詞/尾置詞變化版本。 如這裡所述，您不需大費周章將其他屬性加到現有的 **ListenFor** 元素以及支援尾置詞變化版本。 說「嗨 Cortana，在 Adventure works 顯示我下一趟到拉斯維加斯的行程」，會比說「嗨 Cortana、Adventure Works 顯示我下一趟到拉斯維加斯的行程」，聽起來更自然一些。

有時候語音命令會和現有的 **Cortana** 功能 (打電話、傳送簡訊以及其他等等) 發生衝突，遇到這種情況，您可以將應用程式名稱當做前置詞。 例如，「Adventure Works，傳簡訊給 \[旅行社\]，告訴對方有關拉斯維加斯的行程。」

## <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>完整範例


這個 VCD 檔案會示範各種方法，讓語音命令更聴起來像是人在說話一樣。

**注意：**使用多個 **ListenFor** 元素是沒問題的，但是每一個元素的 **RequireAppName** 屬性值不要一樣。 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
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

## <span id="related_topics"></span>相關文章


**開發人員**
* [Cortana 互動](cortana-interactions.md)
* [定義自訂辨識限制式](define-custom-recognition-constraints.md)
* [**VCD 元素和屬性 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**設計人員**
* [Cortana 設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [語音設計指導方針](https://msdn.microsoft.com/library/windows/apps/dn596121)

**範例**
* [Cortana 語音命令範例](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 







<!--HONumber=Jun16_HO3-->


