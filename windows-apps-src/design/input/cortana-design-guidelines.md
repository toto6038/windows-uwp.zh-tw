---
title: Cortana 設計指導方針-Cortana UWP 設計與開發
description: 這些指導方針和建議會說明您的應用程式如何使用 Cortana 與使用者互動。
ms.assetid: 332ccb95-0e56-410e-ab63-cc028fce4192
label: Cortana
template: detail.hbs
ms.date: 01/27/2021
ms.topic: article
keywords: cortana，設計
ms.localizationpriority: medium
ms.openlocfilehash: ae5f1ce3c481e833ce80d0ebd52d64f6efba7e78
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823272"
---
# <a name="cortana-design-guidelines"></a>Cortana 設計指導方針

>[!WARNING]
> Windows 10 2020 版更新 (2004 版 codename "20H1" ) ，不再支援此功能。
>
> 查看 [Microsoft 365 中](/microsoft-365/admin/misc/cortana-integration) cortana 如何改造新式生產力體驗的 cortana。

這些指導方針和建議會說明您的應用程式如何充分利用 **Cortana** 與使用者互動、協助他們完成工作，以及清楚地傳達其運作情形。

**Cortana** 可讓應用程式在背景中執行，以提示使用者進行確認或去除混淆，並在 [回復] 中提供語音命令狀態的意見反應。 此程式很輕量、快速，而且不會強制使用者離開 **Cortana** 體驗或將內容切換至應用程式。

雖然使用者應該覺得 **cortana** 要盡可能讓程式變得很簡單，但您可能也想要 **cortana** 明確指出您的應用程式完成工作。

我們會使用名為 [ **艾德公司** ] 的行程規劃和管理應用程式，並整合至 **Cortana** UI （如下所示），以示範我們所討論的許多概念和功能。 如需詳細資訊，請參閱 [Cortana voice 命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)。

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 畫布的螢幕擷取畫面":::

## <a name="conversational-writing"></a>對話式撰寫

成功的 **Cortana** 互動需要您在將文字轉換語音 (TTS) 和 GUI 字串時遵循一些基本原則。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">準則</th>
<th align="left">不良範例</th>
<th align="left">良好範例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>有效</dt>
<dd><p>盡可能使用最少的單字，並預先放置最重要的資訊。</p>
</dd>
</dl></td>
<td align="left"><p>的確可以，您希望今天搜尋什麼電影？ 我們有一個大型集合。</p></td>
<td align="left"><p>確定要尋找什麼電影？</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>相關</dt>
<dd><p>僅提供工作、內容和內容的相關資訊。</p>
</dd>
</dl></td>
<td align="left"><p>我已將此新增至播放清單。 您知道，電池的電量很低。</p></td>
<td align="left"><p>我已將此新增至播放清單。</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>清楚</dt>
<dd><p>避免混淆。 使用日常語言，而不是技術術語。</p>
</dd>
</dl></td>
<td align="left"><p>沒有任何查詢行程的結果會向您的拉斯維加斯進行查詢 &quot; &quot; 。</p></td>
<td align="left"><p>我找不到拉斯維加斯的任何旅程。</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>值得 信賴 </dt>
<dd><p>盡可能準確。 對於背景中的內容是透明的—如果工作尚未完成，請不要說它有。 尊重隱私權—請勿大聲閱讀私用資訊。</p>
</dd>
</dl></td>
<td align="left"><p>我找不到該電影，還不能發行。</p></td>
<td align="left"><p>我的目錄中找不到該影片。</p></td>
</tr>
</tbody>
</table>

撰寫人們說話的方式。 不要在發音自然的情況之下強調語法精確度。 例如，像是「想要」或「一定留下後門」的 ear 理解文字快捷方式，對於 TTS 而言很好用。

在可能和自然的情況下使用隱含的 first。 例如，「尋找您的下一間艾德作品」表示有人正在進行搜尋，但不使用「I」這個字來指定。

使用某些變化有助於讓應用程式的音效更自然。 請提供您的 TTS 和 GUI 字串的不同版本，以有效地說出相同的東西。 例如，「您想要查看什麼電影？」 可能有像「您希望觀賞什麼電影？」的替代方案。 人們每次都不會以完全相同的方式說出相同的東西。 您只需確保您的 TTS 和 GUI 版本保持同步。

謹慎地在您的回應中使用「確定」和「良好」等片語。 雖然它們可以提供通知和進度的意義，但如果使用太頻繁且沒有變化，也可以重複使用。

> [!NOTE]
> 只在 TTS 中使用通知片語。 由於 **Cortana** 畫布上的空間有限，請不要在對應的 GUI 字串中重複它們。

在您的回應中使用縮寫，以獲得更自然的互動，以及額外儲存在 **Cortana** 畫布上的空間。 例如，「我找不到電影」，而不是「我找不到電影」。 針對 ear 撰寫，而不是眼睛。

使用系統所瞭解的語言。 使用者傾向于重複出現的條款。 知道您所顯示的內容。

從替代回應的集合中旋轉或隨機選取，以在回應中使用某些變化。 例如，「您想要查看什麼電影？」 以及「您希望觀賞什麼電影？」。 這可讓您的應用程式音效更自然且獨一無二。

## <a name="localization"></a>當地語系化

若要使用語音命令起始動作，您的應用程式必須以使用者在裝置上選取的語言來註冊語音命令 (設定 &gt; 系統 &gt; 語音 &gt; 語音語言) 。

您應該將應用程式所回應的語音命令，以及所有的 TTS 和 GUI 字串當地語系化。

您應避免冗長的 GUI 字串。 **Cortana** 畫布提供三行回應，而且會截斷超過該回應的字串。

如需詳細資訊，請參閱 [全球化和當地語系化一節](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)。

## <a name="image-resources-and-scaling"></a>影像資源和縮放比例

 (UWP) 應用程式的通用 Windows 平臺可根據特定設定和裝置功能，自動選取最適當的應用程式標誌影像 (高對比、有效圖元、地區設定等) 。 您只需要提供影像，並確保您在應用程式專案內針對不同的資源版本使用適當的命名慣例和資料夾組織。 如果您未提供建議的資源版本，可存取性、當地語系化和影像品質可能會受到影響，視使用者的喜好設定、能力、裝置類型和位置而定。

如需高對比和縮放比例影像資源的詳細資訊，請參閱 [磚和圖示資產的指導方針](../../app-resources/images-tailored-for-scale-theme-contrast.md)。

您可以使用限定詞來命名資源。 資源限定詞是資料夾和檔案名修飾詞，用來識別應該使用特定版本資源的內容。

標準命名慣例為 "資料夾名稱/qualifiername-值 \[ \_ qualifiername-值 \] /filename.qualifiername-value \[ \_ qualifiername-值 \] ext"。 例如： images/標誌。 scale 1-100 值 \_contrast-white.png 只會在程式碼中使用根資料夾和 filename： images/logo.png 參考。 請參閱 [管理語言和區域](../globalizing/manage-language-and-region.md) 以及 [如何使用限定詞命名資源](/previous-versions/windows/apps/hh965324(v=win.10))。

建議您將字串資源檔上的預設語言標示 (例如 "en-us \\ resources. .resw" ) 以及影像上的預設縮放比例 (例如 "logo.scale-100.png" ) ，即使您目前未規劃提供當地語系化或多個解析資源。 不過，我們至少建議您提供100、200和400規模調整因素的資產。

> [!IMPORTANT]
> Cortana 畫布標題區域中所使用的應用程式圖示是 "package.appxmanifest" 檔案中指定的 Square44x44Logo 圖示。

您也可以為每個結果磚指定使用者查詢的圖示。 結果圖示的有效影像大小為：

- 68w x 68h
- 68w x 92h
- 280w x 140h

## <a name="result-tile-templates"></a>結果磚範本

系統會為 Cortana 畫布上顯示的結果磚提供一組範本。 使用這些範本來指定磚標題，以及磚是否包含文字和結果圖示影像。 根據指定的範本，每個圖格最多可包含三行文字和一個影像。

以下是支援的範本 (與範例) ：

| 名稱 | 範例 |
| --- | --- |
| 僅標題  | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titleonly-small.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中只顯示標題"::: |
| 具有文字的標題 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewithtext-small.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中顯示具有文字的標題"::: |
| 具有68x68 圖示的標題 | 沒有映射 |
| 具有68x68 圖示和文字的標題 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x68iconandtext-small.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中顯示具有68x68 圖示和文字的標題"::: |
| 具有68x92 圖示的標題 | 沒有映射 |
| 具有68x92 圖示和文字的標題 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x92iconandtext-small.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中顯示具有68x92 圖示和文字的標題"::: |
| 具有280x140 圖示的標題 | 沒有映射 |
| 具有280x140 圖示和文字的標題 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith280x140iconandtext-small.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中顯示具有280x140 圖示和文字的標題"::: |

如需 Cortana 範本的詳細資訊，請參閱 [VoiceCommandContentTileType](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) 。

## <a name="example"></a>範例

此範例示範 **Cortana** 中背景應用程式的端對端工作流程。 我們會使用「 **艾德作品** 」應用程式來取消前往拉斯維加斯的旅程。 此範例會使用 "Title with 68x68 icon and text" 範本。

:::image type="content" source="images/cortana/e2e-canceltrip.png" alt-text="適用于端對端 Cortana 背景應用程式流程 Cortana 畫布的螢幕擷取畫面":::

以下是此圖所述的步驟：

1. 使用者會按麥克風來起始 **Cortana**。
2. 使用者顯示「取消我的艾德公司的工作旅程」以在背景中啟動「 **艾德作品** 」應用程式。 應用程式會使用 **Cortana** speech 和 canvas 來與使用者互動。
3. **Cortana** 會轉換成可提供使用者通知意見反應 ( 「我將會在此工作) 、狀態列和 [取消] 按鈕。
4. 在此情況下，使用者有多個與查詢相符的行程，因此應用程式會提供混淆畫面來列出所有相符的結果，並詢問「您要取消哪一個？」。
5. 使用者指定 "Tech 會議" 專案。
6. 當取消無法復原時，應用程式會提供確認畫面，要求使用者確認其意圖。
7. 使用者顯示「是」。
8. 然後，應用程式會提供完成畫面來顯示作業的結果。

我們將在這裡更詳細地探索這些步驟。

### <a name="handoff"></a>遞交

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中使用 adventureworks 即將推出的「適用于端對端 cortana 背景應用程式流程」的螢幕擷取畫面":::**

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中使用 adventureworks 即將推出":::的「使用 adventureworks 的往返」背景應用程式流程，並透過 *遞交畫面推出 Adventureworks 「即將推出*」

針對您的應用程式所花費的時間不超過500毫秒，且不需要使用者提供任何額外資訊，就可以在不需進一步參與 **Cortana** 的情況下完成，除了顯示完成畫面之外。

如果您的應用程式需要超過500毫秒來回應， **Cortana** 會提供一個切換畫面。 應用程式圖示和名稱隨即顯示，而您必須同時提供 GUI 和 TTS 的交付字串，以表示已正確理解語音命令。 [遞交] 畫面會顯示最多5秒;如果您的應用程式在這段時間內沒有回應， **Cortana** 會呈現一般錯誤畫面。

### <a name="gui-and-tts-guidelines-for-handoff-screens"></a>切換畫面的 GUI 和 TTS 指導方針

明確指出工作正在進行中。

使用現在時時態。

使用動作動詞命令來確認正在啟動的工作，並參考特定的實體。

使用未認可至要求之未完成動作的泛型動詞命令。 例如，「尋找您的旅程」，而不是「取消您的旅程」。 在此情況下，如果未傳回任何結果，使用者就不會聽到像「正在取消您的拉斯維加斯 我找不到拉斯維加斯的旅程」。

請注意，如果應用程式仍需要解析要求的實體，則該工作尚未發生。 例如，請注意，我們說的是「尋找您的旅程」，而不是「取消您的旅程」，因為可以比對零或多個行程，而且我們也不知道結果。

GUI 和 TTS 字串可以相同，但不需要。 請嘗試保持 GUI 字串的簡短，以避免其他視覺效果資產的截斷和重複。

| TTS | GUI |
| --- | --- |
| 正在尋找您下一個艾德公司的工作旅程。 | 正在尋找下一個旅程 .。。 |
| 搜尋您的艾德公司旅程，使其落在城市。 | 正在搜尋前往城市的行程 .。。 |

### <a name="progress"></a>進度

:::image type="content" source="images/cortana/e2e-canceltrip-progress.png" alt-text="使用 adventureworks cancel 旅程進度 adventureworks 「取消行程」進度的 cortana 畫布的螢幕擷取畫面，其適用于端對端 cortana 背景應用程式流程":::**

當工作在各步驟之間花費一段時間時，您的應用程式必須逐步執行，並更新使用者的進度畫面上發生的情況。 應用程式圖示隨即顯示，您必須同時提供 GUI 和 TTS 進度字串，以指出工作正在進行中。

您應該提供具有啟動參數的應用程式連結，以在適當的狀態啟動應用程式。 這可讓使用者自行查看或完成工作。 **Cortana** 提供連結文字 (例如「移至艾德作品」 ) 。

進度畫面會顯示5秒，之後必須接著另一個畫面，否則工作將會超時。

這些畫面可以追蹤進度畫面：

- 進度
- 確認 (明確，稍後會說明) 
- 去除混淆
- Completion

### <a name="gui-and-tts-guidelines-for-progress-screens"></a>適用于進度畫面的 GUI 和 TTS 指導方針

使用現在時時態。

使用動作動詞命令來確認工作正在進行中。

**GUI**：如果顯示實體，請使用它的參考 ( 「正在取消此旅程」 ) ;如果未顯示任何實體，請明確地呼叫實體 ( 「取消「Tech 會議」」 ) 。

**Tts**：您只能在第一個進度畫面上包含一個 TTS 字串。 如果需要進一步的進度畫面，請傳送空字串， {} 做為您的 TTS 字串，並只提供 GUI 字串。

| 條件  | TTS | GUI |
| --- | --- | --- |
| 顯示時顯示的先前回合/實體上的實體讀取     | 正在取消此旅程 .。。          | 正在取消此旅程 .。。          |
| 在顯示時顯示的先前回合/實體未讀取實體 | 正在取消您的旅程 | 正在取消此旅程 .。。          |
| 未顯示先前回合/實體上的實體未讀取        | 正在取消您的旅程 | 正在取消您的旅程 |

### <a name="confirmation"></a>確認

:::image type="content" source="images/cortana/e2e-canceltrip-confirmation.png" alt-text="使用 adventureworks 取消行程確認 adventureworks 「取消行程」確認的 cortana 畫布的螢幕擷取畫面，其適用于端對端 cortana 背景應用程式流程":::**

某些工作可由使用者命令的本質隱含確認;有些人可能更機密，而且需要明確確認。 以下是使用明確與隱含確認的時機的一些指導方針。

確認畫面上的 GUI 和 TTS 字串都是由您的應用程式所指定，並會顯示應用程式圖示（如有提供），而不是 **Cortana** 的圖片。

當客戶回應確認之後，您的應用程式必須在500毫秒內提供下一個畫面，以避免進入進度畫面。

使用明確時機 .。。

- 內容會離開使用者 (例如文字訊息、電子郵件或社交文章) 
- 動作無法復原 (例如，進行購買或刪除某些內容) 
- 結果可能會尷尬 (例如，呼叫錯誤的人員) 
- 需要更複雜的辨識 (例如，開放式轉譯) 

使用隱含時機 .。。

- 只會為使用者儲存內容 (例如，附注至自我) 
- 有一個簡單的方法可以退出 (，例如開啟或關閉鬧鐘) 
- 這項工作必須快速 (例如，在忘記之前快速取得構想) 
- 精確度很高 (例如簡單的功能表) 

### <a name="gui-and-tts-guidelines-for-confirmation-screens"></a>確認畫面的 GUI 和 TTS 指導方針

使用現在時時態。

要求使用者一個明確的問題，可使用「是」或「否」回答。 問題應該明確確認使用者嘗試做什麼，而且應該沒有其他明顯的選項。

提供重新提示問題的變化，以防您第一次無法理解語音命令。

**GUI**：如果顯示實體，請使用它的參考。 如果未顯示任何實體，請明確地呼叫實體。

**TTS**：為了清楚起見，一律會參考特定的專案或實體，除非系統在先前的回合中已將其讀取。

| 條件 | TTS | GUI |
| --- | --- | --- |
| 在顯示時顯示的先前回合/實體未讀取實體 | 您要取消 Tech 大會嗎？ | 要取消此行程嗎？                             |
| 未顯示先前回合/實體上的實體未讀取        | 您要取消 Tech 大會嗎？ | 要取消 Tech 大會嗎？                 |
| 先前開啟的實體讀取/實體未顯示            | 您要取消此行程嗎？             | 要取消此行程嗎？                             |
| 顯示具有實體的重新提示                              | 您要取消此行程嗎？            | 您要取消此行程嗎？             |
| 未顯示實體的重新提示                          | 您要取消此行程嗎？            | 您要取消 Tech 大會嗎？ |

### <a name="disambiguation"></a>去除混淆

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="Cortana 畫布的螢幕擷取畫面，其中使用 adventureworks 取消行程消除了 adventureworks 「取消行程」混淆的「端對端 cortana 背景應用程式」流程的螢幕擷取畫面":::**

某些工作可能需要使用者從實體清單中選取才能完成工作。

混淆畫面上的 GUI 和 TTS 字串都是由您的應用程式所指定，並會顯示應用程式圖示（如有提供），而不是 **Cortana** 的圖片。

當客戶回應去除混淆問題之後，您的應用程式必須在500毫秒內提供下一個畫面，以避免進入進度畫面。

### <a name="gui-and-tts-guidelines-for-disambiguation-screens"></a>混淆畫面的 GUI 和 TTS 指導方針

使用現在時時態。

詢問使用者一個明確的問題，以顯示任何實體的標題或文字行來回答這些問題。

最多可顯示10個實體。

每個實體都應該有唯一的標題。

提供重新提示問題的變化，以防您第一次無法理解語音命令。

**TTS**：為了清楚起見，一律會參考特定的專案或實體，除非在先前的回合中說出它。

**TTS**：除非有三個或更少，而且很短，否則不會讀取實體清單。

| 條件                 | TTS                                                                            | GUI                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| 提示-3 個或更少的專案  | 您要取消哪一趟？ 在拉斯維加斯的 Tech 大會或合作物件 | 您要取消哪一個？ |
| 提示-3 個以上的專案 | 您要取消哪一趟？                                          | 您要取消哪一個？ |
| 重新提示                   | 您要取消哪一趟？                                         | 您要取消哪一個？ |

### <a name="completion"></a>Completion

:::image type="content" source="images/cortana/e2e-canceltrip-completion.png" alt-text="使用 adventureworks 取消行程完成 adventureworks 「取消行程」完成的 cortana 畫布背景應用程式流程的螢幕擷取畫面":::**

當工作成功完成時，您的應用程式應該會通知使用者要求的工作已順利完成。

完成畫面上的 GUI 和 TTS 字串都是由您的應用程式所指定，並會顯示應用程式圖示（如有提供），而不是 **Cortana** 的圖片。

您應該提供具有啟動參數的應用程式連結，以在適當的狀態啟動應用程式。 這可讓使用者自行查看或完成工作。 **Cortana** 提供連結文字 (例如「移至艾德作品」 ) 。

### <a name="gui-and-tts-guidelines-for-completion-screens"></a>完成畫面的 GUI 和 TTS 指導方針

使用過去的時態。

使用動作動詞來明確指出工作已完成。

如果實體已顯示，或已在先前的回合中參考，請只參考它。

| 條件                                       | TTS                                             | GUI                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| 實體顯示/實體在先前回合時讀取         | 我已取消這個旅程。                       | 已取消此行程。               |
| 實體未顯示/實體未在先前回合時讀取 | 我已取消您的 Tech Tech 會議旅程。 | 已取消「拉斯維加斯 Tech 會議」。 |

### <a name="error"></a>錯誤

:::image type="content" source="images/cortana/e2e-canceltrip-error.png" alt-text="使用 adventureworks 取消行程錯誤 adventureworks 「取消行程」錯誤的 cortana 畫布的螢幕擷取畫面，其適用于端對端 cortana 背景應用程式流程":::**

發生下列其中一個錯誤時， **Cortana** 會顯示相同的一般錯誤訊息。

- App service 意外終止。
- **Cortana** 無法與 app service 進行通訊。
- 應用程式無法在 **Cortana** 顯示切換畫面或進度畫面5秒後提供螢幕。

## <a name="related-articles"></a>相關文章

- [Windows 應用程式中的 Cortana 互動](cortana-interactions.md)
- [VCD 元素和屬性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 語音命令範例](https://go.microsoft.com/fwlink/p/?LinkID=619899)