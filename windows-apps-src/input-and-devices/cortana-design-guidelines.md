---
author: Karl-Bridge-Microsoft
Description: "使用語音命令，延伸 Cortana 與您的應用程式所提供的功能。"
title: "Cortana 設計指導方針"
ms.assetid: A92C084B-9913-4718-9A04-569D51ACE55D
label: Guidelines
template: detail.hbs
ms.sourcegitcommit: 077fcc6ff462a771ed56f875d960e46e6f4420fc
ms.openlocfilehash: 31442ed17b9b463cbf10cecb564278b86086bbf2

---

# Cortana 設計指導方針


這些指導方針與建議會說明您的 app 如何充分利用 **Cortana** 與使用者互動，以協助他們完成工作，並清楚傳達發生的過程。

**Cortana** 可讓應用程式在背景執行，並提示使用者進行確認或去除混淆，然後根據語音命令的狀態提供使用者回饋。 過程輕量且快速，而且不會強迫使用者離開 **Cortana** 體驗或切換內容到應用程式。

雖然使用者應該感覺是 **Cortana** 協助讓過程變得盡可能輕鬆，但您可能希望 **Cortana** 也指明是您的應用程式完成了工作。

我們使用已整合到 **Cortana** UI，名為 **Adventure Works** 的旅行計畫及管理應用程式 (如下所示)，示範我們討論過的多種概念和功能

![Cortana 畫布概觀](images/speech/cortana-overview.png)

## <span id="Conversational_writing_"></span><span id="conversational_writing_"></span><span id="CONVERSATIONAL_WRITING_"></span>對話式寫入


成功的 **Cortana** 互動需要您在製作文字轉換語音 (TTS) 和 GUI 字串時遵循一些基礎原則。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">原則</th>
<th align="left">錯誤範例</th>
<th align="left">最佳範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt><span id="Efficient"></span><span id="efficient"></span><span id="EFFICIENT"></span>效率</dt>
<dd><p>盡可能使用最少的文字，並將重要資訊放在最前面。</p>
</dd>
</dl></td>
<td align="left"><p>當然可以，您今天想要搜尋什麼電影？ 我們有大量的收藏。</p></td>
<td align="left"><p>沒問題，您在找什麼電影？</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt><span id="Relevant"></span><span id="relevant"></span><span id="RELEVANT"></span>相關</dt>
<dd><p>提供只和工作、內容及前後文相關的資訊。</p>
</dd>
</dl></td>
<td align="left"><p>我已經將此項目新增到您的撥方清單。 順帶一提，您的電池電量變低了。</p></td>
<td align="left"><p>我已經將此項目新增到您的撥方清單。</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt><span id="Clear"></span><span id="clear"></span><span id="CLEAR"></span>明確</dt>
<dd><p>避免混淆。 使用日常用語，而不是技術專業用語。</p>
</dd>
</dl></td>
<td align="left"><p>沒有 &quot;到拉斯維加斯的行程&quot; 的查詢結果。</p></td>
<td align="left"><p>我找不到任何到拉斯維加斯的行程。</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt><span id="Trustworthy_"></span><span id="trustworthy_"></span><span id="TRUSTWORTHY_"></span>可信 </dt>
<dd><p>盡可能正確。 清楚表明正在背景執行的工作—若工作未完成，不要說它已經完成。 尊重隱私權—不要唸出個人資訊。</p>
</dd>
</dl></td>
<td align="left"><p>我找不到該電影，它應該還沒上映。</p></td>
<td align="left"><p>我在我們的目錄中找不到該電影。</p></td>
</tr>
</tbody>
</table>

 

依人們說話的方式撰寫。 不要在自然語調上過度地強調文法準確性。 舉例來說，我們聽來順耳的口語簡寫，如「想要」或「要去」，對於文字轉換語音 (TTS) 唸出是可以接受的。

請盡可能且自然地使用隱含的第一人稱時態。 例如，「正在尋找您的下一個 Adventure Works 行程」表示某人正在進行尋找，但並未使用「我」這個字來說明。

使用一些變化讓您的應用程式聽起來更加自然。 提供 TTS 與圖形化使用者介面 (GUI) 不同版本的字串，以更有效地來說出同樣的事情。 例如，「您想要看什麼電影？」 可以替換成像是「您想要觀賞哪部電影？」。 人們在說同樣的事情時，不會每次都是完全相同的方式。 只要確定您的 TTS 和 GUI 版本保持同步。

在您的回應中謹慎使用「好」和「好的」這類的片語。 雖然它們可以提供通知和進度上的感受，但頻繁使用且沒有變化會變成重複的感受。

**注意** 只在 TTS 中使用通知片語。 因為 **Cortana** 畫布上的空間有限，請不要在對應的 GUI 字串中重複它們。

 

在您的回應中使用縮寫，讓互動更自然且節省 **Cortana** 畫布上的空間。 例如，「我找不到該電影」而不是「我沒有辦法找到該電影」。 寫給耳朵，而不是眼睛。

使用系統了解的語言。 使用者通常會重複要表達的字彙。 了解您的顯示項目。

在您的回應中透過從集合內或替代性的回應，輪流或隨機選取來產生變化。 例如，「您想要看什麼電影？」 與「您想要觀賞哪部電影？」。 這樣能讓您的應用程式聽起來更自然且獨特。

## <span id="Localization_"></span><span id="localization_"></span><span id="LOCALIZATION_"></span>當地語系化


若要使用語音命令啟動動作，您的 app 必須在使用者於裝置上選取的語言中登錄語音命令 (\[設定\]  \[系統\]  \[語音\]  \[語音語言\])。

您應該當地語系化 app 回應的語音命令，以及所有 TTS 和 GUI 字串。

您應該避免冗長的 GUI 字串。 **Cortana** 畫布提供三行的回應，並會截斷超過的字串。

如需詳細資訊，請參閱[全球化和當地語系化區段](../globalizing/globalizing-portal.md)。

## <span id="Image_resources_and_scaling"></span><span id="image_resources_and_scaling"></span><span id="IMAGE_RESOURCES_AND_SCALING"></span>影像資源和縮放


通用 Windows 平台 (UWP) app 可以根據特定的設定和裝置功能，自動選取最適當的應用程式標誌影像 (高對比、有效像素及地區設定等)。 您需要的就是提供影像，並確保您在應用程式專案內為不同的資源版本使用適當的命名慣例和資料夾組織。 如果您未提供建議的資源版本，則視使用者的喜好設定、能力、裝置類型及位置而定，協助工具、當地語系化及影像品質可能會有問題。

如需高對比和縮放比例的影像資源詳細資訊，請參閱[磚和圖示資產的指導方針](../controls-and-patterns/tiles-and-notifications-app-assets.md)。

您要使用限定詞命名資源。 資源限定詞是資料夾和檔案名稱的修飾詞，用來識別內容中應該使用的特定資源版本。

標準命名慣例是 "foldername/qualifiername-value\[\_qualifiername-value\]/filename.qualifiername-value\[\_qualifiername-value\].ext"。 例如：images/en-US/logo.scale-100\_contrast-white.png 只是指在程式碼中使用的根資料夾和檔案名稱：images/logo.png。 請參閱[全球化和當地語系化](../globalizing/globalizing-portal.md)與[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。

即使您目前不打算提供當地語系化或多種解析度資源，仍建議您在字串資源檔上標示預設語言 (如 "en-US\\resources.resw")，並在影像上標示預設縮放比例 (如 "logo.scale-100.png")。 不過，我們建議您至少為 100、200 及 400 的縮放比例提供資產。

**重要**  
Cortana 畫布的標題區域中使用的應用程式圖示是 "Package.appxmanifest" 檔案中指定的 Square44x44Logo 圖示。 

您也可以針對使用者查詢，指定每個結果磚的圖示。 結果圖示的有效影像大小為︰

-   68w x 68h
-   68w x 92h
-   280w x 140h

## <span id="Result_tile_templates"></span><span id="result_tile_templates"></span><span id="RESULT_TILE_TEMPLATES"></span>結果磚範本

針對 Cortana 畫布上顯示的結果磚，提供一組範本。 使用這些範本，指定磚的標題以及磚是否包含文字和結果圖示影像。 每個磚可以包含最多三行的文字和一張影像，取決於指定的範本。

以下是支援的範本 (含範例)︰

| 名稱 | 範例 |
| --- | --- |
| 只有標題  | ![只有標題](images/cortana/voicecommandcontenttiletype_titleonly_small.png) |
| 標題含文字   | ![標題含文字](images/cortana/voicecommandcontenttiletype_titlewithtext_small.png) |
| 標題含 68 x 68 圖示   | 無影像 |
| 標題含 68 x 68 圖示與文字   | ![標題含 68 x 68 圖示與文字](images/cortana/voicecommandcontenttiletype_titlewith68x68iconandtext_small.png) |
| 標題含 68 x 92 圖示   | 無影像 |
| 標題含 68 x 92 圖示與文字    | ![標題含 68 x 92 圖示與文字](images/cortana/voicecommandcontenttiletype_titlewith68x92iconandtext_small.png) |
| 標題含 280 x 140 圖示   | 無影像 |
| 標題含 280 x 140 圖示與文字    | ![標題含 280 x 140 圖示與文字](images/cortana/voicecommandcontenttiletype_titlewith280x140iconandtext_small.png) |

如需關於 Cortana 範本的詳細資訊，請參閱 [VoiceCommandContentTileType](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.voicecommands.voicecommandcontenttiletype.aspx)。

## <span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>範例


這個範例示範在 **Cortana** 中背景應用程式的端對端工作流程。 我們使用 **Adventure Works** 應用程式來取消到拉斯維加斯的行程。 此範例使用「標題含 68 x 68 圖示與文字」範本。

![端對端 Cortana 背景應用程式流程](images/speech/e2e-canceltrip.png)

以下透過影像敘述步驟：

1.  使用者點選麥克風以起始 **Cortana**。
2.  使用者說出「取消我的 Adventure Works 拉斯維加斯行程」，以在背景中啟動 **Adventure Works** 應用程式。 應用程式同時使用 **Cortana** 語音和畫布兩者與使用者互動。
3.  **Cortana** 轉換到遞交畫面，給予使用者通知回饋 (「我會讓 Adventure Works 進行。」)、一個狀態列，和一個取消按鈕。
4.  在這個範例中，使用者有許多行程符合查詢，因此應用程式提供去除混淆畫面列出所有符合的結果，並詢問「您想要取消哪一個？」
5.  使用者指定「拉斯維加斯技術會議」項目。
6.  由於取消無法復原，應用程式提供確認畫面，詢問使用者確認意圖。
7.  使用者說：「是」。
8.  應用程式提供完成畫面顯示操作結果。

我們將探索這些步驟的詳細資料如下。

### <span id="Handoff"></span><span id="handoff"></span><span id="HANDOFF"></span>遞交

| ![端對端：尋找行程，沒有遞交畫面 ](images/speech/cortana-backgroundapp-result.png) |
|--- |
| 尋找行程，沒有遞交畫面 |

| ![端對端：取消行程 (包含遞交畫面) ](images/speech/cortana-backgroundapp-progress-result.png) |
|--- |
| 包含遞交畫面的取消行程 | 

應用程式在工作回應上花費小於 500 毫秒，不需要其他的使用者資訊與 **Cortana** 的進一步參與即可完成 (除了顯示完成畫面)。

如果應用程式需要 500 毫秒以上才能回應，**Cortana** 提供遞交畫面。 應用程式圖示與名稱將會顯示，且您必須同時提供 GUI 與 TTS 遞交字串，以指出語音命令已經正確地了解。 遞交畫面最多顯示 5 秒；如果您的應用程式沒有在這段時間內回應，**Cortana** 會顯示一般錯誤畫面。

### <span id="GUI_and_TTS_guidelines_for_handoff_screens"></span><span id="gui_and_tts_guidelines_for_handoff_screens"></span><span id="GUI_AND_TTS_GUIDELINES_FOR_HANDOFF_SCREENS"></span>遞交畫面的 GUI 與 TTS 指導方針

清楚地指出工作正在進行。

使用現在式時態。

使用動作動詞，確認哪些工作正在初始化以及參照的指定實體。

使用一般動詞，不要認可請求的、未完成的動作。 例如，「正在尋找您的行程」而不是「正在取消您的行程」。 在此情況下，如果沒有傳回結果，使用者不會聽到類似「正在取消您前往拉斯維加斯的行程… 我找不到任何到拉斯維加斯的行程。」

假如應用程式仍須解析請求的實體，請務必清楚描述工作尚未開始。 例如請注意我們說「正在尋找您的行程」而不是「正在取消您的行程」，因為可能沒有符合的行程，或是有多個符合的行程，而我們還不清楚結果如何。

GUI 與 TTS 字串可以相同，但並非必要。 嘗試讓 GUI 字串保持簡短，避免截斷以及與其他視覺資產重複。

| TTS                                                    | GUI                                 |
|--------------------------------------------------------|-------------------------------------|
| 正在尋找您的下一個 Adventure Works 行程。            | 正在尋找您的下一個行程...         |
| 正在尋找您的 Adventure Works 前往福爾斯市的行程。 | 正在搜尋福爾斯市行程... |

 

### <span id="Progress"></span><span id="progress"></span><span id="PROGRESS"></span>進度

| ![端對端：取消行程 (包含進度畫面) ](images/speech/e2e-canceltrip-progress.png) |
| --- |
| 包含進度畫面的取消行程 |  

當工作在步驟間要花費一點時間時，應用程式必須在進度畫面上介入並更新資訊，告知使用者正在發生的事情。 應用程式圖示將會顯示，且您必須同時提供 GUI 與 TTS 進度字串，以指出工作正在進行。

您應該提供一個包含啟動參數的連結到您的應用程式，以在適當的狀態下啟動應用程式。 這可讓使用者檢視或完成工作本身。 **Cortana** 提供連結文字 (例如「移至 Adventure Works」)。

進度畫面會分別顯示 5 秒鐘，之後必須緊接著另一個畫面，否則工作將會逾時。

這些畫面可以接在進度畫面之後：

-   進度
-   確認 (明確，稍後說明)
-   去除混淆
-   完成

### <span id="GUI_and_TTS_guidelines_for_progress_screens"></span><span id="gui_and_tts_guidelines_for_progress_screens"></span><span id="GUI_AND_TTS_GUIDELINES_FOR_PROGRESS_SCREENS"></span>進度畫面的 GUI 與 TTS 指導方針

使用現在式時態。

使用動作動詞命令，以確認工作正在進行。

**GUI**：如果有顯示實體，使用它的參照 (「正在取消這個行程...」)；如果沒有顯示實體，明確地呼叫實體 (「正在取消拉斯維加斯技術會議」)。

**TTS**：在第一個進度畫面，應只包括一個 TTS 字串。 如果需要進一步的進度畫面，傳送一個空字串 ({}) 作為您的 TTS 字串，並只提供一個 GUI 字串。

| 條件                                              | TTS                            | GUI                            |
|---------------------------------------------------------|--------------------------------|--------------------------------|
| 在先前的切換讀取到實體/ 實體顯示在顯示器上     | 正在取消這個行程…          | 正在取消這個行程…          |
| 在先前的切換未讀取到實體 / 實體顯示在顯示器上 | 正在取消您前往拉斯維加斯的行程… | 正在取消這個行程…          |
| 在先前的切換未讀取到實體/ 沒有顯示實體        | 正在取消您前往拉斯維加斯的行程… | 正在取消您前往拉斯維加斯的行程… |

 

### <span id="Confirmation"></span><span id="confirmation"></span><span id="CONFIRMATION"></span>確認

| ![端對端：取消行程 (包含確認畫面) ](images/speech/e2e-canceltrip-confirmation.png) |
| --- |
| 包含確認畫面的取消行程 | 

有些工作可由使用者命令的性質隱含確認；其他可能更具敏感性的則需要明確確認。 下列是何時使用明確確認與隱含確認的指導方針。

在確認畫面上的 GUI 與 TTS 字串，是由您的應用程式指定 (如果有提供應用程式圖示，就會取代 **Cortana** 虛擬人偶的顯示)。

客戶回應確認之後，您的應用程式必須在 500 毫秒內提供下一個畫面，避免進入進度畫面。

使用明確確認當...

-   內容正離開使用者 (例如傳送文字訊息、電子郵件，或社交文章等等)
-   無法復原的動作 (例如購買或刪除項目等等)
-   結果可能是令人尷尬的 (例如撥號給錯誤的人)
-   要求比較複雜的辨識 (例如開放端點的轉譯)

使用隱含確認當...

-   只儲存給使用者的內容 (例如給自己的筆記)
-   沒有簡單的方式往返 (例如開啟或關閉鬧鐘)
-   工作必須要快速 (例如要快速地擷取想法，以免之後忘記)
-   準確度很高 (例如簡單的功能表)

### <span id="GUI_and_TTS_guidelines_for_confirmation_screens"></span><span id="gui_and_tts_guidelines_for_confirmation_screens"></span><span id="GUI_AND_TTS_GUIDELINES_FOR_CONFIRMATION_SCREENS"></span>確認畫面的 GUI 與 TTS 指導方針

使用現在式時態。

使用明確的、可用「是」或「否」回答的問題詢問使用者。 問題應明確地確認使用者正要進行的動作，並且不應該有其他不明確的選項。

針對重新提示給予問題的變化，以防在第一次的語音命令沒有被了解。

**GUI**：如果有顯示實體，使用它作為參照。 如果沒有顯示實體，明確地呼叫實體。

**TTS**：為了清晰起見，一律參照特定的項目或實體，除非它在先前的切換由系統讀出。

| 條件                                              | TTS                                        | GUI                                           |
|---------------------------------------------------------|--------------------------------------------|-----------------------------------------------|
| 在先前的切換未讀取到實體 / 實體顯示在顯示器上 | 您想取消拉斯維加斯技術會議嗎？ | 取消這個行程嗎？                             |
| 在先前的切換未讀取到實體/ 沒有顯示實體        | 您想取消拉斯維加斯技術會議嗎？ | 取消拉斯維加斯技術會議嗎？                 |
| 在先前的切換讀取到實體/ 沒有顯示實體            | 您想取消這個行程嗎？             | 取消這個行程嗎？                             |
| 包含顯示實體的重新提示                              | 您想取消這個行程嗎？            | 您想要取消這個行程嗎？             |
| 不包含顯示實體的重新提示                          | 您想取消這個行程嗎？            | 您想要取消拉斯維加斯技術會議嗎？ |

 

### <span id="Disambiguation"></span><span id="disambiguation"></span><span id="DISAMBIGUATION"></span>去除混淆

| ![端對端：取消行程 (包含去除混淆畫面)](images/speech/cortana-disambiguation-screen.png) |
| --- |
| 取消行程 (包含去除混淆畫面) | 

有些工作可能需要使用者從清單選取實體以完成工作。

在去除混淆畫面上的 GUI 與 TTS 字串兩者，是由您的應用程式指定 (如果有提供應用程式圖示，就會取代 **Cortana** 虛擬人偶的顯示)。

客戶回應去除混淆問題之後，您的應用程式必須在 500 毫秒內提供下一個畫面，避免進入進度畫面。

### <span id="GUI_and_TTS_guidelines_for_disambiguation_screens"></span><span id="gui_and_tts_guidelines_for_disambiguation_screens"></span><span id="GUI_AND_TTS_GUIDELINES_FOR_DISAMBIGUATION_SCREENS"></span>去除混淆畫面的 GUI 與 TTS 指導方針

使用現在式時態。

詢問使用者可回答的明確問題 (含有標題或任何實體顯示的文字行)。

最多 10 個實體可以顯示。

每個實體都應該有唯一的標題。

針對重新提示給予問題的變化，以防在第一次的語音命令沒有被了解。

**TTS**：為了清晰起見，一律參照特定的項目或實體，除非它在先前的切換被說出。

**TTS**：請不要讀出實體清單，除非它們少於三個且很簡短。

| 條件                 | TTS                                                                            | GUI                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| 提示 - 3 個或更少的項目  | 您想取消哪一個拉斯維加斯行程？ 「拉斯維加斯技術會議」或「拉斯維加斯派對」？ | 您想要取消哪一個？ |
| 提示 - 多於 3 個的項目 | 您想取消哪一個拉斯維加斯行程？                                          | 您想要取消哪一個？ |
| 重新提示                   | 您想取消哪一個拉斯維加斯行程？                                         | 您想要取消哪一個？ |

 

### <span id="Completion"></span><span id="completion"></span><span id="COMPLETION"></span>完成

| ![端對端：取消行程 (包含完成畫面) ](images/speech/e2e-canceltrip-completion.png) |
| --- |
| 包含完成畫面的取消行程 |

 

當工作成功完成，應用程式應通知使用者請求的工作已經成功完成。

在完成畫面上的 GUI 與 TTS 字串兩者，是由您的應用程式指定 (如果有提供應用程式圖示，就會取代 **Cortana** 虛擬人偶的顯示)。

您應該提供一個包含啟動參數的連結到您的應用程式，以在適當的狀態下啟動應用程式。 這可讓使用者檢視或完成工作本身。 **Cortana** 提供連結文字 (例如「移至 Adventure Works」)。

### <span id="GUI_and_TTS_guidelines_for_completion_screens"></span><span id="gui_and_tts_guidelines_for_completion_screens"></span><span id="GUI_AND_TTS_GUIDELINES_FOR_COMPLETION_SCREENS"></span>完成畫面的 GUI 與 TTS 指導方針

使用過去式時態。

使用動作動詞明確地聲明工作已完成。

如果有顯示實體，或它在先前的切換已經有參照，那麼只參照到它。

| 條件                                       | TTS                                             | GUI                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| 有顯示實體/在先前的切換讀取到實體         | 我已取消這個行程。                       | 已取消這個行程。               |
| 沒有顯示實體/ 在先前的切換未讀取到實體 | 我已取消您的「拉斯維加斯技術會議」行程。 | 已取消「拉斯維加斯技術會議」。 |

 

### <span id="Error"></span><span id="error"></span><span id="ERROR"></span>錯誤

| ![端對端：取消行程 (包含錯誤畫面)](images/speech/e2e-canceltrip-error.png) |
| --- |
| 取消行程 (包含錯誤畫面) |

 

當下列其中一個錯誤發生時，**Cortana** 會顯示相同的一般錯誤訊息。

-   應用程式服務意外地終止。
-   **Cortana** 與應用程式服務通訊失敗。
-   在 **Cortana** 顯示遞交畫面或進度畫面 5 秒後，應用程式無法提供畫面。

## <span id="related_topics"></span>相關文章


* [語音互動](speech-interactions.md)  

**開發人員**
* [Cortana 互動](https://msdn.microsoft.com/library/windows/apps/mt185598)
* [語音互動](https://msdn.microsoft.com/library/windows/apps/mt185614)
 

 







<!--HONumber=Jun16_HO3-->


