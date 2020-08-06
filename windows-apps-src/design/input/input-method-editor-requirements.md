---
Description: " (輸入法開發自訂的輸入法，) 協助使用者輸入以語言呈現的文字，而無法輕易地在標準的鍵盤上顯示。"
title: 輸入法編輯器 (IME) 需求
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: ime、輸入法編輯器、輸入、互動
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ecf150973defb0a431fc7248181ddf648576ac77
ms.sourcegitcommit: 86ce67a03e87fa1282849b2fcb4f89d1cf23a091
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87840015"
---
# <a name="custom-input-method-editor-ime-requirements"></a>自訂輸入法編輯器 (輸入法) 需求

這些指導方針和需求可協助您開發自訂的輸入法編輯器 (輸入法) ，以協助使用者輸入以語言呈現的文字，而無法輕易地在標準的鍵盤上顯示。

如需 Ime 的總覽，請參閱[輸入法編輯器 (IME) ](input-method-editors.md)。

## <a name="default-ime"></a>預設輸入法

使用者可以選取其任何使用中的 Ime (**設定-> 時間 & 語言 > 語言 > 慣用語言-> 語言套件-選項**) 為其慣用語言的預設輸入法。

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="慣用的語言設定":::

選取 [語言選項設定] 畫面上的預設鍵盤，以取得慣用的語言。

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="慣用的語言鍵盤":::

> [!Important]
> 我們不建議您直接寫入登錄，為您的自訂 IME 設定預設鍵盤。

## <a name="compatibility-requirements"></a>相容性需求

以下是自訂 IME 的基本相容性需求。

### <a name="ime-must-be-compatible-with-windows-apps"></a>輸入法必須與 Windows 應用程式相容

使用[文字服務架構 (TSF) ](/windows/win32/tsf/text-services-framework)來執行 ime。 先前，您可以選擇使用輸入[方法管理員 (IMM32) ](/windows/win32/intl/input-method-manager)輸入服務。 現在，系統會封鎖使用輸入法管理員來執行的 Ime (IMM32) 。

當應用程式啟動時，TSF 會為使用者目前所選取的 IME 載入 IME DLL。 載入輸入法時，會受限於應用程式的相同應用程式容器限制。 例如，如果應用程式未在其資訊清單中要求網際網路存取，IME 就無法存取網際網路。 此行為可確保 Ime 不會違反安全性合約。

TSF 是應用程式與您的輸入法之間的媒介。 TSF 會將輸入事件傳達至輸入法，並在使用者選取字元之後，從輸入法接收輸入字元。

這個行為與舊版的 Windows 相同，但載入 Windows 應用程式會影響 IME 的潛在功能。

如果您的輸入法需要在 Windows 應用程式和桌面應用程式之間提供不同的功能或 UI，請確定 TSF 載入的 DLL 會檢查載入的是哪一種類型的應用程式。 在您的輸入法中呼叫[ITfThreadMgrEx：： GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)方法，並檢查 TF_TMF_IMMERSIVEMODE 旗標，讓您的 IME 觸發不同的應用程式邏輯（視結果而定）。

Windows 應用程式不支援資料表文字服務 (TTS) Ime。

> [!NOTE]
> 某些產生 TTS Ime 的工具會產生標示為 Windows 惡意程式碼的輸入法。

### <a name="ime-must-be-compatible-with-the-system-tray"></a>輸入法必須與系統匣相容

沒有用來裝載 IME 圖示的語言列。 相反地，輸入指標會顯示在系統匣上，指出目前的輸入選項。 輸入指示器只會顯示輸入法商標圖示，以指出目前正在執行的輸入法。 此外，還有一個 IME 模式圖示，可顯示在輸入法商標圖示的左邊，讓使用者執行最常用的 IME 模式切換，例如開啟或關閉 IME。

輸入指示器只會顯示相容 Ime 的輸入法商標圖示和模式圖示。 [不相容的 Ime] 不會在系統匣中顯示商標圖示和模式圖示。 相反地，輸入指示器會顯示語言縮寫，而不是輸入法商標圖示。

將 IME 圖示儲存在 DLL 或 EXE 檔案中，而不是獨立的 .ico 檔案。 IME 圖示的設計必須遵循下列 UI 設計指導方針一節中所述的指導方針。

### <a name="ime-branding-icon"></a>輸入法商標圖示

輸入指示器會使用 ime 在系統上註冊時所定義的資源識別碼，從 IME DLL 取得 IME 商標圖示。

### <a name="ime-mode-icon"></a>輸入法模式圖示

某些 Ime 可能需要依賴顯示在系統匣上的輸入指示器，以顯示 IME 模式圖示。 在此情況下，IME 會使用 GUID_LBI_INPUTMODE，將 IME 模式圖示傳遞至輸入指示器。

將「輸入法」模式圖示傳遞至系統匣上的輸入指標時，IME 模式圖示的預設大小為16x16 圖元。 UI 調整會遵循 DPI。

將輸入法模式圖示傳遞至 UAC 上的輸入指標時 (安全桌面) 中的使用者帳戶控制，IME 模式圖示的預設大小為20x20 圖元。 UAC 上的 IME 模式圖示的 UI 縮放會遵循 PPI。

## <a name="ime-must-work-in-app-container"></a>輸入法必須在應用程式容器中工作

某些 IME 函式會在應用程式容器中受到影響。

- **字典**檔案-通常，ime 具有唯讀字典檔案，可將使用者輸入對應至特定字元。 若要從應用程式容器內部存取這些檔案，您的輸入法必須將它們放在程式檔案或 Windows 目錄底下。 根據預設，您可以從應用程式容器讀取這些目錄，因此 Ime 可以存取儲存在這些位置的字典檔案。 如果您的輸入法必須將字典檔案儲存在其他位置，則必須明確地操作字典檔案[ (ACL) 的存取控制清單](/windows/win32/secauthz/access-control-lists)，以允許從應用程式容器存取。
- **網際網路更新**-如果您的 IME 需要使用來自網際網路的資料來更新其字典，它無法在應用程式容器內可靠地執行，因為不一定允許網際網路存取。 相反地，您的 IME 應該執行個別的桌面進程，負責以網際網路的資料來更新字典檔案。
- 即時**學習**-如果輸入法是在具有網際網路存取的應用程式容器中執行，則輸入法可以進行通訊的端點不會有任何限制。 在此情況下，IME 可以使用雲端伺服器來提供即時學習服務。 有些 Ime 會在使用者輸入時，即時下載並上傳使用者輸入。 由於應用程式容器中不保證網際網路存取，因此不一定會允許這麼做。
- 在**進程之間共用資訊**-ime 可能需要在不同應用程式容器中的應用程式之間共用使用者輸入喜好設定的相關資料。 使用 web 服務在應用程式之間共用資料。

> [!Important]
> 如果您嘗試規避應用程式容器的安全性規則，您的輸入法可能會被視為惡意程式碼並遭到封鎖。

## <a name="ime-and-touch-keyboard"></a>輸入法和觸控式鍵盤

您的輸入法必須確保其候選窗格的 UI 和其他 UI 元素不會在觸控鍵盤底下繪製。 觸控鍵盤會以高於所有應用程式的 z 順序顯示，而輸入法 UI 會顯示在與作用中應用程式相同的 z 順序寬線中。 因此，觸控鍵盤可以重迭並隱藏 IME UI。 在大部分情況下，應用程式應該調整其視窗的大小，以考慮觸控鍵盤。 如果應用程式未調整大小，則輸入法仍然可以使用[InputPane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) API 來取得觸控鍵盤的位置。 IME 會查詢[Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location)屬性，或為觸控鍵盤的 Show 和 Hide 事件註冊處理常式。 每當使用者在編輯欄位中按下時，就會引發顯示事件，即使目前已顯示觸摸鍵盤也一樣。 您的輸入法可以使用此 API 來取得觸控鍵盤使用的螢幕空間，輸入法才會繪製候選 (或其他) UI，以及重新排列 Ime UI，以避免在觸控式鍵盤下方繪製。

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>指定慣用的觸控鍵盤配置

IME 可以指定要使用的觸控鍵盤配置，並啟用輸入法來使用觸控優化的配置。 此功能僅限於韓文、日文、簡體中文和繁體中文輸入語言的 Ime。

觸控鍵盤支援七個版面配置，其中三個是傳統配置，而四個是觸控優化的配置。 傳統版面配置的外觀和行為與實體鍵盤類似。

這三種傳統版面配置都是用來以不同形式輸入繁體中文：

- 以語音為基礎的輸入
- 倉頡輸入
- Dayi 輸入

除了傳統版面配置，每個韓文、日文、簡體中文和繁體中文輸入語言都有一個觸控優化的版面配置。

若要使用這項功能，您的輸入法必須執行[ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)介面，並使用文字服務架構[ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) API 來匯出 ime。

如果您的輸入法不支援 ITfFnGetPreferredTouchKeyboardLayout 介面，則使用 IME 會產生觸控鍵盤所顯示之語言的預設傳統版面配置。

如果您的輸入法必須將其中一個傳統版面配置設定為慣用的版面配置，則輸入法端不需要額外的工作，除了支援 ITfFnGetPreferredTouchKeyboardLayout 和 ITfFunctionProvider 介面之外。 但是輸入法需要進行其他工作，才能使用觸控優化的配置，下一節將會說明這點。

### <a name="touch-optimized-layout"></a>觸控優化版面配置

適用于韓文、日文、簡體中文和繁體中文輸入語言的觸控優化鍵盤，會針對輸入法和輸入法關閉轉換模式，顯示不同的版面配置。 觸控鍵盤上有一個索引鍵，可將 IME 轉換模式設定為 [開啟] 或 [關閉]，但鍵盤的 IME 模式也可能會隨著編輯控制項間的焦點變更而改變。

日文、簡體中文和繁體中文輸入語言的觸控優化鍵盤包含按鍵或按鍵，IME 會使用這些金鑰來流覽候選頁面。 若為日文和簡體中文，候選頁面索引鍵會顯示在觸控優化的版面配置上。 針對繁體中文，先前和下一個候選頁面有不同的索引鍵。

當按下這些按鍵時，觸控鍵盤會呼叫[SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)函式，將下列 Unicode 私用使用區字元傳送至焦點應用程式，IME 可以攔截並採取行動：

- **下一個頁面 (0xF003) ** -當候選頁面索引鍵在日文和簡體中文的觸控優化鍵盤上按下，或在繁體中文的觸控優化鍵盤上按下下一頁鍵時傳送。
- **上一頁 (0xF004) ** -當候選頁面索引鍵與日文和簡體中文的觸控優化鍵盤上的 Shift 鍵同步選取時，或在繁體中文的觸控優化鍵盤上按下上一頁鍵時，就會傳送此頁面。

這些字元會以 Unicode 輸入的形式傳送。 下一段將詳細說明如何在文字服務架構輸入法收到的關鍵事件接收通知期間，解壓縮字元資訊。 這些字元值不會在任何標頭檔中定義，因此您必須在程式碼中定義它們。

若要攔截鍵盤輸入，您的 IME 必須註冊為金鑰事件接收。 對於使用 SendInput 函式所產生的 Unicode 輸入， [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink)回呼的 WPARAM 參數 (OnKeyDown，OnKeyUp，OnTestKeyDown，OnTestKeyUp) 一律包含虛擬索引鍵 VK_PACKET 而且不會直接識別該字元。

執行下列呼叫順序以存取字元：

```cpp
// Keyboard state
BYTE abKbdState[256];
if (!GetKeyboardState(abKbdState))
{
   return 0;
}

// Map virtual key to character code
WCHAR wch;
if (ToUnicode(VK_PACKET, 0, abKbdState, &wch, 1, 0) == 1)
{
   return wch;
}
```

## <a name="ime-search-integration"></a>輸入法搜尋整合

透過搜尋合約並與 [搜尋] 窗格整合，為使用者提供搜尋功能。

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="搜尋窗格和輸入法建議":::<br/>
*搜尋窗格和輸入法建議*

[搜尋] 窗格是一個集中位置，可讓使用者在其所有應用程式中執行搜尋。 針對 IME 使用者，Windows 提供獨特的搜尋體驗，可讓相容的 Ime 與 Windows 整合，以獲得更高的效率和可用性。

輸入與搜尋相容的輸入法的使用者有兩個主要優點：

- 輸入法與搜尋體驗之間的順暢互動。 IME 候選項目會以內嵌方式顯示在搜尋方塊下方，而不會 occluding 搜尋建議。 使用者可以使用鍵盤，在搜尋方塊、IME 轉換候選項目和搜尋建議之間順暢地流覽。
- 更快速地存取應用程式所提供的相關結果和建議。 應用程式可以存取所有目前的轉換候選項目，以提供更多相關的建議。 為了更妥善設定搜尋建議的優先順序，會以相關性的順序將轉換提供給應用程式。 使用者可以在不轉換的情況下尋找並選取所要的結果，只要輸入拼音即可。

如果輸入法符合下列準則，則會與整合式搜尋體驗相容：

- 與 Windows 樣式 shell 相容。
- 執行 TSF UILess 模式 Api。 如需詳細資訊，請參閱[UILess 模式總覽](/windows/win32/tsf/uiless-mode-overview)。
- 執行 TSF 搜尋整合 Api [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)和[ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)。

在 [搜尋] 窗格中啟用時，會將相容的 IME 置於 UIless 模式，而且無法顯示其 UI。 相反地，它會將轉換候選物件傳送至 Windows，這會顯示在內嵌候選清單控制項中，如先前的螢幕擷取畫面所示。

此外，IME 會傳送應用來執行目前搜尋的候選項目。 這些候選項目可以與轉換候選項目相同，也可以針對搜尋量身打造。

良好的搜尋候選人符合下列準則：

- 沒有前置詞重迭。 例如，北京大學 and北京是多餘的，因為其中一個是另一個的前置詞。
- 沒有多餘的候選項目。 任何多餘的候選不適合用于搜尋，因為它無法協助篩選結果。 例如，任何符合北京大學的結果也會符合北京。
- 沒有預測候選，只有轉換。 例如，如果使用者輸入 "as"，則 IME 可以傳回北做為候選，但不能傳回北京大學。 通常，預測候選項目的限制過於嚴格。

不符合準則的 Ime 與其他控制項的搜尋顯示不相容，而且無法利用 UI 整合和搜尋候選項目。 只有在使用者完成撰寫之後，應用程式才會接收查詢。

當支援搜尋合約的應用程式收到查詢時，查詢事件會包含 "queryTextAlternatives" 陣列，其中包含所有已知的替代專案，並從最相關的 (可能) 到最低的 (不太可能) 。

當提供替代專案時，應用程式應該將每個替代專案視為查詢，並傳回符合任何替代專案的所有結果。 應用程式的行為應該會如同使用者同時發出多個查詢一樣，基本上是對提供結果的服務發出 "or" 查詢。 基於效能考慮，應用程式通常會將比對限制為5到20個最相關的替代專案。

## <a name="ui-design-guidelines"></a>UI 設計指導方針

所有的 Ime 都必須遵循[設計和程式碼 Windows 應用程式](/windows/uwp/design/)中所述的使用者經驗指導方針。

### <a name="dont-use-sticky-windows"></a>不要使用粘滯視窗

您的輸入法視窗應該只在必要時才會出現，而且不應該隨時顯示。 當使用者不需要輸入時，IME windows 就不應該顯示。 IME 視窗不應該是全螢幕視窗。 輸入法 windows 不應彼此重迭。 Windows 應以 Windows 樣式設計，並遵循 UI 調整。

### <a name="ime-icons"></a>輸入法圖示

有兩種類型的 IME 圖示、商標圖示和模式圖示。 所有的輸入法圖示都必須設計成僅使用黑色和白色色彩。 新的 IME 圖示會從 glyphic 系統匣圖示的外觀借用。 此樣式已建立，因此所有語言都可以使用它來補充 familial 的外觀，同時也能彼此區別。

IME 圖示的檔案格式為 .ICO。 您必須提供下列圖示大小。

- 16x16 圖元
- 20x20 圖元
- 24x24 圖元
- 32x32 圖元
- 40x40 圖元
- 48x48 圖元

請確定所有解析度都提供具有 Alpha 色板的32點陣圖標。

輸入法品牌圖示是由白色方塊所定義，其中會放置以新式字型呈現的印刷樣式圖像。 每個語言小組都會選擇每個定義的圖像。 字型為黑色。 此方塊會在50% 不透明度的情況中包含1個圖元的外部筆劃（黑色）。 「新」版本是由方塊左上角的圓角所定義。

IME 模式圖示是由新式字型中的白色印刷色圖像所定義，其中包含黑色的1圖元外部筆劃（50% 不透明度）。

| 圖示 | 描述 |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="繁體中文 ChangeJie 的 IME 品牌圖示範例。"::: | 繁體中文 ChangeJie 的 IME 品牌圖示範例。 |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="繁體中文新 ChangeJie 的輸入法品牌圖示範例。"::: | 繁體中文 ChangeJie 的 IME 品牌圖示範例。 |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="中文模式圖示"::: | 範例輸入法模式圖示。 |

### <a name="owned-window"></a>擁有的視窗

若要顯示候選 UI，IME 必須將其視窗設定為 [擁有] 視窗，讓它可以顯示在目前執行的應用程式上。 使用[ITfCoNtextView：： GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)方法來抓取要擁有的視窗。 如果 GetWnd 傳回錯誤或 NullHWND，請呼叫[GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus)函式。

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>IME 候選視窗與 light 關閉表面的互動

快顯視窗的關閉模型稱為「light 關閉」，因為使用者很容易就能關閉這類視窗。 為了讓 Ime 在 Windows 互動模型中正常運作，IME Windows 必須參與 light 解除模型。

若要參與 light 解除模型，您的 IME 必須使用[NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent)函數或類似的函式，引發三個新的 Windows 事件。 這些新事件包括：

- **EVENT_OBJECT_IME_SHOW** -當輸入法變成可見時，引發此事件。
- **EVENT_OBJECT_IME_HIDE** -隱藏輸入法時，引發這個事件。
- **EVENT_OBJECT_IME_CHANGE** -當輸入法移動或變更大小時，引發此事件。

### <a name="declaring-compatibility"></a>宣告相容性

Ime 藉由使用[ITfCategoryMgr：： RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory)註冊其 IME 的分類 GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT，宣告它們相容。

### <a name="set-the-default-ime-mode-to-on"></a>將預設的 IME 模式設定為 [開啟]

我們為 Ime 提供了更好的 UX。

## <a name="dpi-scaling-support-for-desktop-applications"></a>桌面應用程式的 DPI 縮放比例支援

增強的 DPI 縮放比例支援可查詢每個桌面進程的已宣告 DPI 感知層級，以判斷它是否需要調整 UI。 在多重監視器案例中，Windows 會針對每個監視器上的不同 DPI 設定適當地調整 UI。

因為您的輸入法是在每個應用程式的進程內容中執行，所以您不應該為您的 IME 宣告 DPI 感知層級。 這可確保您的輸入法會以目前進程的 DPI 感知層級執行。

若要確保所有 IME UI 元素都與您執行之進程的 UI 元素調整同位，您必須適當地回應不同的 DPI 值。

> [!NOTE]
> 為了確保與新的桌面應用程式的同位，您的 IME 應支援每個監視器– DPI 感知，但不應宣告本身的感知程度。 系統會決定每個案例中適當的調整需求。

如需桌面應用程式 DPI 縮放支援需求的詳細資訊，請參閱[高 DPI](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)。

## <a name="ime-installation"></a>輸入法安裝

如果您使用 Microsoft Visual Studio 建立您的輸入法，請使用協力廠商安裝程式（例如從參閱 flexera software Software InstallShield）為您的 IME 建立安裝體驗。

下列步驟示範如何使用 InstallShield 來為您的 IME DLL 建立安裝專案。

- 安裝 Visual Studio。
- 啟動 Visual Studio。
- **在 [檔案**] 功能表上，指向 [**新增**]，然後選取 [**專案**]。 [**新增專案**] 對話方塊隨即開啟。
- 在左窗格中，流覽至 [**範本] > 其他專案類型 > 安裝和部署**]，按一下 [**啟用 InstallShield 限量版**]，然後按一下 **[確定]**。 遵循安裝指示進行。
- 重新啟動 Visual Studio。
- 開啟 [IME] 方案 ( .sln) 檔案。
- 在方案總管中，以滑鼠右鍵按一下方案，指向 [**加入**]，然後選取 [**新增專案**]。 [**加入新的專案**] 對話方塊隨即開啟。
- 在左側樹狀檢視控制項中，流覽至 [**範本] > [其他專案類型] > [InstallShield 限量版**]。
- 在中央視窗中，按一下 [ **InstallShield 限量版專案**]。
- 在 [**名稱**] 文字方塊中，輸入 "SetupIME"，然後按一下 **[確定]**。
- 在 [**專案助理**] 對話方塊中，按一下 [**應用程式資訊**]。
- 填入您的公司名稱和其他欄位。
- 按一下 [**應用程式檔**]。
- 在左窗格中，以滑鼠右鍵按一下 **[INSTALLDIR]** 資料夾，然後選取 [**新增資料夾**]。 將資料夾命名為「外掛程式」。
- 按一下 [**新增**檔案]。 流覽至您的輸入法 DLL，並將它新增至 [**外掛程式**] 資料夾。 針對 IME 字典重複此步驟。
- 以滑鼠右鍵按一下輸入法 DLL，然後選取 [**屬性**]。 [**屬性**] 對話方塊隨即開啟。
- 在 [**屬性**] 對話方塊中，按一下 [ **COM & .net 設定**] 索引標籤。
- 在 [**註冊類型**] 底下，選取 [**自我註冊**]，然後按一下 **[確定]**。
- 建置方案。 輸入法 DLL 隨即建立，而 InstallShield 會建立一個 setup.exe 檔案，讓使用者能夠在 Windows 上安裝您的輸入法。

若要建立您自己的安裝體驗，請呼叫[ITfInputProcessorProfileMgr：： RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile)方法，在安裝期間註冊輸入法。 不要直接寫入登錄專案。

如果在安裝後必須立即可以使用輸入法，請呼叫[InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) ，使用 psz 參數的下列格式，將 ime 新增至使用者啟用的輸入方法：

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>輸入法存取範圍

執行下列慣例，讓您的 Ime 符合協助工具需求，並與 [朗讀程式] 搭配使用。 若要讓候選清單可供存取，您的 Ime 必須遵循此慣例。

- 候選清單必須有一個**UIA_AutomationIdPropertyId**等於「IME_Candidate_Window」的轉換候選清單，或是預測候選清單的「IME_Prediction_Window」。
- 當候選清單出現並消失時，它會分別引發**UIA_MenuOpenedEventId**和**UIA_MenuClosedEventId**類型的事件
- 當目前選取的候選項變更時，候選清單會引發**UIA_SelectionItem_ElementSelectedEventId**。 選取的元素應具有**UIA_SelectionItemIsSelectedPropertyId**等於**TRUE**的屬性。
- 候選清單中每個專案的**UIA_NamePropertyId**都必須是候選名稱。 （選擇性）您可以透過**UIA_HelpTextPropertyId**，提供其他資訊來區分候選項目。

## <a name="related-topics"></a>相關主題

- [輸入法 (IME)](input-method-editors.md)
- [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout)
- [ITfCompartmentEventSink](/windows/win32/api/msctf/nn-msctf-itfcompartmenteventsink)
- [ITfThreadMgrEx::GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags)
- [ITfCoNtextView::GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd)
- [TF_INPUTPROCESSORPROFILE](/windows/win32/api/msctf/ns-msctf-tf_inputprocessorprofile)
- [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider)
- [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)
- [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput)
- [協助工具](/windows/uwp/design/accessibility/accessibility)
