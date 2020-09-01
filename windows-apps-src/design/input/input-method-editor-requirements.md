---
Description: 開發自訂輸入方法編輯器 (IME) 來協助使用者輸入無法在標準的標準鍵盤上輕鬆表示的語言文字。
title: 輸入法編輯器 (IME) 需求
label: Input Method Editor (IME) requirements
template: detail.hbs
keywords: 輸入法、輸入法編輯器、輸入、互動
ms.date: 07/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5a34c15826bff757b7c4277b87cc5fed53a6f109
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160002"
---
# <a name="custom-input-method-editor-ime-requirements"></a>自訂輸入方法編輯器 (IME) 需求

這些指導方針和需求可協助您開發自訂輸入方法編輯器 (IME) 協助使用者輸入無法在標準的標準鍵盤上輕鬆呈現的文字。

如需 Ime 的總覽，請參閱 [輸入法編輯器 (IME) ](input-method-editors.md)。

## <a name="default-ime"></a>預設 IME

使用者可以選取任何使用中的 Ime (**設定-> 時間 & 語言 > 語言 > 慣用語言 > 語言套件-選項**) 為慣用語言的預設 IME。

:::image type="content" source="images/IMEs/ime-preferred-languages.png" alt-text="慣用語言設定":::

在慣用語言的 [語言選項設定] 畫面上，選取預設鍵盤。

:::image type="content" source="images/IMEs/ime-preferred-languages-keyboard.png" alt-text="慣用語言鍵盤":::

> [!Important]
> 我們不建議直接寫入登錄來為您的自訂 IME 設定預設鍵盤。

## <a name="compatibility-requirements"></a>相容性需求

以下是自訂 IME 的基本相容性需求。

### <a name="ime-must-be-compatible-with-windows-apps"></a>IME 必須與 Windows 應用程式相容

使用 [文字服務架構 (TSF) ](/windows/win32/tsf/text-services-framework) 來執行 ime。 先前，您可以選擇使用輸入服務的 [輸入方法管理員 (IMM32) ](/windows/win32/intl/input-method-manager) 。 現在系統會封鎖使用輸入方法管理員執行的 Ime (IMM32) 。

當應用程式啟動時，TSF 會針對使用者目前選取的 IME 載入 IME DLL。 當 IME 載入時，會受限於與應用程式相同的應用程式容器限制。 例如，如果應用程式未在其資訊清單中要求網際網路存取，則 IME 無法存取網際網路。 此行為可確保 Ime 無法違反安全性合約。

TSF 是應用程式與您的 IME 之間的媒介。 TSF 會將輸入事件傳達給輸入法，並在使用者選取字元之後，從 IME 接收輸入字元。

這種行為與舊版 Windows 相同，但載入至 Windows 應用程式會影響 IME 的潛在功能。

如果您的輸入需要在 Windows 應用程式與桌面應用程式之間提供不同的功能或 UI，請確定 TSF 所載入的 DLL 會檢查所載入的應用程式類型。 在您的 IME 中呼叫 [ITfThreadMgrEx：： GetActiveFlags](/windows/win32/api/msctf/nf-msctf-itfthreadmgrex-getactiveflags) 方法，並檢查 TF_TMF_IMMERSIVEMODE 旗標，讓您的 ime 依據結果觸發不同的應用程式邏輯。

Windows 應用程式不支援 (TTS) Ime 的資料表文字服務。

> [!NOTE]
> 產生 TTS Ime 的一些工具會產生由 Windows 標示為惡意程式碼的 Ime。

### <a name="ime-must-be-compatible-with-the-system-tray"></a>IME 必須與系統匣相容

沒有可裝載 IME 圖示的語言列。 相反地，系統匣會顯示指出目前輸入選項的輸入指標。 輸入指標只會顯示 IME 標記圖示，以指出目前執行中的 IME。 此外，也有一個 IME 模式圖示會顯示在輸入法商標圖示的左邊，讓使用者可以執行最常用的 IME 模式參數，例如開啟或關閉 IME。

輸入指標只會顯示相容 Ime 的輸入法商標圖示和模式圖示。 不相容的 Ime 沒有顯示在系統匣中的商標圖示和模式圖示。 相反地，輸入指標會顯示語言縮寫，而不是輸入法商標圖示。

將輸入法圖示儲存在 DLL 或 EXE 檔案中，而不是獨立的 .ico 檔案。 輸入法圖示的設計必須遵循下列 UI 設計指導方針一節所述的指導方針。

### <a name="ime-branding-icon"></a>IME 商標圖示

輸入指標會使用 IME 在系統上註冊時所定義的資源識別碼，從 IME DLL 取得輸入法商標圖示。

### <a name="ime-mode-icon"></a>IME 模式圖示

某些 Ime 可能需要依賴顯示在系統匣上的輸入指標來顯示 IME 模式圖示。 在此情況下，IME 會使用 GUID_LBI_INPUTMODE，將輸入法模式圖示傳遞給輸入指標。

將 IME 模式圖示傳遞至系統匣上的輸入指標時，IME 模式圖示的預設大小為16x16 圖元。 UI 調整會遵循 DPI。

當將輸入法模式圖示傳遞至 UAC 上的輸入指標時 (在安全桌面) 的使用者帳戶控制，IME 模式圖示的預設大小為20x20 圖元。 UAC 上輸入法模式圖示的 UI 調整會遵循 PPI。

## <a name="ime-must-work-in-app-container"></a>IME 必須在應用程式容器中運作

某些 IME 函數會在應用程式容器中受到影響。

- **字典** 檔-通常，ime 有唯讀的字典檔案，可將使用者輸入對應至特定字元。 若要從應用程式容器記憶體取這些檔案，您的 IME 必須將它們放在 Program Files 或 Windows 目錄下。 根據預設，這些目錄可以從應用程式容器讀取，因此 Ime 可以存取儲存在這些位置中的字典檔。 如果您的輸入檔必須將字典檔案儲存在其他位置，則必須明確操作字典檔案 [ (ACL) 的存取控制清單 ](/windows/win32/secauthz/access-control-lists) ，以允許從應用程式容器進行存取。
- **網際網路更新** -如果您的 IME 需要使用來自網際網路的資料來更新其字典，它無法在應用程式容器內可靠地這麼做，因為不一定會允許網際網路存取。 相反地，您的 IME 應該執行個別的桌面進程，負責以網際網路的資料更新字典檔案。
- 即時**學習**-如果輸入法是在可存取網際網路的應用程式容器中執行，則輸入法可以進行通訊的端點沒有限制。 在此情況下，IME 可以使用雲端伺服器來提供即時學習服務。 某些 Ime 會在使用者輸入時，即時下載並上傳使用者輸入。 由於應用程式容器無法保證網際網路存取，因此不一定會允許這種情況。
- 在**進程之間共用資訊**-ime 可能需要在不同應用程式容器中的應用程式之間共用使用者輸入喜好設定的相關資料。 使用 web 服務在應用程式之間共用資料。

> [!Important]
> 如果您嘗試規避應用程式容器安全性規則，您的 IME 可能會被視為惡意程式碼和封鎖。

## <a name="ime-and-touch-keyboard"></a>IME 和觸控鍵盤

您的 IME 必須確保其候選窗格的 UI 和其他 UI 元素不會在觸控鍵盤底下繪製。 觸控鍵盤會顯示在與所有應用程式較高的 z 順序頻外，而且 IME UI 會顯示在其作用中應用程式所在的相同 z 順序寬線中。 如此一來，觸控鍵盤可以重迭並隱藏 IME UI。 在大部分情況下，應用程式應調整其視窗的大小，以考慮觸控鍵盤。 如果應用程式未調整大小，則 IME 仍可使用 [InputPane](/windows/win32/api/shobjidl_core/nn-shobjidl_core-iframeworkinputpane) API 來取得觸控鍵盤的位置。 IME 會查詢 [Location](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iframeworkinputpane-location) 屬性，或註冊觸控鍵盤的顯示和隱藏事件的處理常式。 每次使用者按下編輯欄位時都會引發 Show 事件，即使目前顯示的是觸摸鍵盤也是一樣。 您的 IME 可以使用此 API 來取得觸控鍵盤用的螢幕空間，在 IME 繪製候選 (或其他) UI 之前，以及重新排列 Ime UI，以避免在觸摸鍵盤下繪製。

### <a name="specifying-the-preferred-touch-keyboard-layout"></a>指定慣用的觸控鍵盤配置

IME 可指定要使用的觸控鍵盤配置，並啟用 IME 以使用觸控優化的版面配置。 這項功能僅適用于韓文、日文、簡體中文和繁體中文輸入語言的 Ime。

觸控鍵盤支援七種版面配置，其中三種是傳統版面配置，其中四個是觸控優化的版面配置。 傳統版面配置的外觀和行為就像實體鍵盤一樣。

這三種典型的版面配置都是用不同的形式來輸入繁體中文：

- 以語音為基礎的輸入
- 倉頡輸入
- Dayi 輸入

除了傳統版面配置，每個韓文、日文、簡體中文和繁體中文輸入語言都有一個觸控優化的版面配置。

若要使用此功能，您的 IME 必須執行 [ITfFnGetPreferredTouchKeyboardLayout](/windows/win32/api/ctffunc/nn-ctffunc-itffngetpreferredtouchkeyboardlayout) 介面，該介面是由 ime 使用文字服務架構 [ITfFunctionProvider](/windows/win32/api/msctf/nn-msctf-itffunctionprovider) API 所匯出。

如果您的輸入法不支援 ITfFnGetPreferredTouchKeyboardLayout 介面，則使用 IME 會產生觸控鍵盤所顯示之語言的預設傳統版面配置。

如果您的輸入法必須將其中一個傳統版面配置設定為慣用版面配置，則輸入端不需要任何額外的工作，超出支援 ITfFnGetPreferredTouchKeyboardLayout 和 ITfFunctionProvider 介面的範圍。 但是，輸入法需要額外的工作才能使用觸控優化的版面配置，而且下一節將說明這一點。

### <a name="touch-optimized-layout"></a>觸控優化的版面配置

適用于韓文、日文、簡體中文和繁體中文輸入語言的觸控優化鍵盤，會在轉換模式中顯示不同的輸入和 IME 版面配置。 觸控鍵盤上有一個按鍵，可將 IME 轉換模式設定為 [開啟] 或 [關閉]，但鍵盤的 IME 模式也可能隨著編輯控制項之間的焦點變更而變更。

日文、簡體中文和繁體中文輸入語言的觸控優化鍵盤包含一個按鍵，也就是 IME 用來流覽候選頁面的按鍵。 若為日文和簡體中文，候選頁面索引鍵會顯示在觸控優化的版面配置上。 若是繁體中文，則會有不同的金鑰用於上一頁和下一個候選頁面。

當按下這些按鍵時，觸控鍵盤會呼叫 [SendInput](/windows/win32/api/winuser/nf-winuser-sendinput) 函式，將下列 Unicode 私用區域字元傳送給焦點應用程式，讓 IME 可以攔截並採取動作：

- **下一頁 (0xF003) ** -當在日文和簡體中文的觸控優化鍵盤上按下候選頁面按鍵時，或在繁體中文的觸控優化鍵盤上按下一個頁面按鍵時傳送。
- **上一頁 (0xF004) ** -在按下候選頁面按鍵時傳送，以在日文和簡體中文的觸控優化鍵盤上同步選取 Shift 鍵，或在繁體中文觸控優化鍵盤上按下一個頁面按鍵時傳送。

這些字元會以 Unicode 輸入的形式傳送。 下一段會詳細說明如何在文字服務架構 IME 將接收的重要事件接收通知期間解壓縮字元資訊。 這些字元值不會在任何標頭檔中定義，因此您必須在程式碼中定義它們。

若要攔截鍵盤輸入，您的 IME 必須註冊為主要事件接收器。 針對使用 SendInput 函式所產生的 Unicode 輸入， [ITfKeyEventSink](/windows/win32/api/msctf/nn-msctf-itfkeyeventsink) 回呼的 WPARAM 參數 (OnKeyDown、OnKeyUp、OnTestKeyDown、OnTestKeyUp) 一律包含虛擬機器碼 VK_PACKET 且不會直接識別該字元。

執行下列呼叫順序來存取字元：

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

## <a name="ime-search-integration"></a>IME 搜尋整合

透過搜尋合約提供使用者搜尋功能，並與 [搜尋] 窗格整合。

:::image type="content" source="images/IMEs/ime-search-pane.png" alt-text="搜尋窗格和輸入法的建議":::<br/>
*搜尋窗格和輸入法的建議*

搜尋窗格是一個中心位置，可讓使用者在其所有應用程式中執行搜尋。 針對 IME 使用者，Windows 提供獨特的搜尋體驗，可讓相容的 Ime 與 Windows 整合，以提高效率和可用性。

使用與搜尋相容的 IME 輸入的使用者，有兩個主要優點：

- 輸入法與搜尋體驗之間的緊密互動。 IME 候選項目會以內嵌方式顯示在搜尋方塊下方，而不會遮蔽搜尋建議。 使用者可以使用鍵盤在搜尋方塊、IME 轉換候選項目和搜尋建議之間順暢地流覽。
- 更快速地存取應用程式所提供的相關結果和建議。 應用程式可存取所有目前的轉換候選項目，以提供更相關的建議。 為了更妥善設定搜尋建議的優先順序，會以相關性的順序提供應用程式的轉換。 使用者可以在不轉換的情況下尋找並選取所要的結果，只要輸入拼音就可以了。

如果 IME 符合下列準則，則 IME 與整合式搜尋體驗相容：

- 與 Windows 樣式 shell 相容。
- 執行 TSF UILess 模式 Api。 如需詳細資訊，請參閱 [UILess 模式總覽](/windows/win32/tsf/uiless-mode-overview)。
- 執行 TSF 搜尋整合 Api （ [ITfFnSearchCandidateProvider](/windows/win32/api/ctffunc/nn-ctffunc-itffnsearchcandidateprovider) 和 [ITfIntegratableCandidateListUIElement](/windows/win32/api/ctffunc/nn-ctffunc-itfintegratablecandidatelistuielement)）。

在 [搜尋] 窗格中啟用時，相容的 IME 會置於 UIless 模式，而且無法顯示其 UI。 相反地，它會將轉換候選項目傳送給 Windows，以將它們顯示在內嵌候選清單控制項中，如先前的螢幕擷取畫面所示。

此外，IME 也會傳送應用來執行目前搜尋的候選項目。 這些候選項目可以與轉換候選項目相同，或可針對搜尋量身打造。

良好的搜尋候選人符合下列準則：

- 沒有前置詞重迭。 例如，北京大學 and北京是多餘的，因為它是另一個的前置詞。
- 無多餘的候選項目。 任何多餘的候選項對搜尋都沒有説明，因為它不會協助篩選結果。 例如，任何符合北京大學的結果也會符合北京。
- 無預測候選，僅限轉換。 例如，如果使用者輸入 "as"，IME 可以傳回北做為候選項，但無法傳回北京大學。 預測候選項目通常太嚴格。

不符合準則的 Ime 與其他控制項的方式相同，無法與搜尋顯示相容，而且無法利用 UI 整合和搜尋候選項目。 應用程式只會在使用者完成撰寫之後才接收查詢。

當支援搜尋合約的應用程式收到查詢時，查詢事件會包含 "queryTextAlternatives" 陣列，其中包含所有已知的替代方案，並根據最相關的 (可能) 最相關的 (不太) 。

提供替代方案時，應用程式應該將每個替代方案視為查詢，並傳回符合任何替代專案的所有結果。 應用程式的行為應該與使用者同時發出多個查詢相同，基本上是對提供結果的服務發出「或」查詢。 基於效能考慮，應用程式通常會限制與最相關的替代專案5和20之間的相符專案。

## <a name="ui-design-guidelines"></a>UI 設計指導方針

所有 Ime 都必須遵循 [設計和程式碼 Windows 應用程式](../index.md)中所述的使用者經驗指導方針。

### <a name="dont-use-sticky-windows"></a>不要使用粘滯視窗

您的 IME 視窗應該只在需要時才會出現，而且不應該隨時顯示。 當使用者不需要輸入時，就不應該顯示 IME。 IME 視窗不應該是全螢幕視窗。 IME windows 不應彼此重迭。 Windows 應以 Windows 樣式來設計，並遵循 UI 縮放比例。

### <a name="ime-icons"></a>輸入法圖示

有兩種類型的 IME 圖示、商標圖示和模式圖示。 所有的輸入法圖示都必須僅使用黑色和白色色彩來設計。 新的輸入法圖示借用系統匣圖示的 glyphic 外觀。 已建立此樣式，因此所有語言都可以使用它來補充 familial 外觀，同時也能彼此區分。

輸入法圖示的檔案格式為 .ICO。 您必須提供下列圖示大小。

- 16x16 圖元
- 20x20 圖元
- 24x24 圖元
- 32x32 圖元
- 40x40 圖元
- 48x48 圖元

確定已在所有解析度中提供具有 Alpha 通道的32點陣圖標。

IME 品牌圖示是由白色方塊所定義，其中會放置以新式字型轉譯的印刷樣式符號。 每個語言小組都會選擇每個定義圖像。 圖像是黑色。 方塊中包含1圖元的外部筆劃（以50% 不透明度為黑色）。 「新的」版本是由方塊左上角的圓角所定義。

IME 模式圖示是由新式字型中的白色印刷字元所定義，其中包含1圖元的外部筆劃（以50% 的不透明度為黑色）。

| 圖示 | 描述 |
| --- | --- |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese.png" alt-text="繁體中文 ChangeJie 的範例 IME 品牌圖示。"::: | 繁體中文 ChangeJie 的範例 IME 品牌圖示。 |
| :::image type="content" source="images/IMEs/ime-brand-icon-traditional-chinese-new.png" alt-text="繁體中文新 ChangeJie 的範例 IME 品牌圖示。"::: | 繁體中文 ChangeJie 的範例 IME 品牌圖示。 |
| :::image type="content" source="images/IMEs/ime-mode-icon-chinese.png" alt-text="中文模式圖示"::: | 範例輸入法模式圖示。 |

### <a name="owned-window"></a>擁有的視窗

若要顯示候選的 UI，IME 必須將其視窗設定為所擁有的視窗，讓它可以顯示在目前正在執行的應用程式上。 您可以使用 [ITfCoNtextView：： GetWnd](/windows/win32/api/msctf/nf-msctf-itfcontextview-getwnd) 方法來取出要擁有的視窗。 如果 GetWnd 傳回錯誤或 NullHWND，請呼叫 [GetFocus](/windows/win32/api/msctf/nf-msctf-itfthreadmgr-getfocus) 函數。

`if (FAILED(pView->GetWnd(&parentWndHandle)) || (parentWndHandle == nullptr)) { parentWndHandle = GetFocus(); }`

### <a name="ime-candidate-window-interaction-with-light-dismiss-surfaces"></a>與淺色關閉表面互動的 IME 候選視窗

快顯視窗的關閉模型稱為「淺色關閉」，因為使用者很容易就能關閉這類視窗。 為了讓 Ime 在 Windows 互動模型中正常運作，IME windows 必須參與 light 解除模型。

為了參與 light 解除模型，您的 IME 必須使用 [NotifyWinEvent](/windows/win32/api/winuser/nf-winuser-notifywinevent) 函式或類似的函式，引發三個新的 Windows 事件。 這些新事件包括：

- **EVENT_OBJECT_IME_SHOW** -當 IME 變成可見時引發這個事件。
- **EVENT_OBJECT_IME_HIDE** -隱藏 IME 時引發這個事件。
- **EVENT_OBJECT_IME_CHANGE** -當 IME 移動或變更大小時，引發這個事件。

### <a name="declaring-compatibility"></a>宣告相容性

Ime 宣告它們是相容的，其方式是使用 [ITfCategoryMgr：： RegisterCategory](/windows/win32/api/msctf/nf-msctf-itfcategorymgr-registercategory)註冊其 IME GUID_TFCAT_TIPCAP_IMMERSIVESUPPORT 的分類。

### <a name="set-the-default-ime-mode-to-on"></a>將預設的 IME 模式設定為開啟

我們為 Ime 提供了更好的 UX。

## <a name="dpi-scaling-support-for-desktop-applications"></a>桌面應用程式的 DPI 縮放比例支援

增強的 DPI 縮放支援可讓您查詢每個桌面進程的宣告 DPI 感知層級，以判斷是否需要調整 UI 規模。 在多重監視器案例中，Windows 會在每個監視器上適當地調整 UI 以進行不同的 DPI 設定。

由於您的 IME 是在每個應用程式的程式內容中執行，因此您不應該為 IME 宣告 DPI 感知層級。 這可確保您的 IME 在目前進程的 DPI 感知層級執行。

為了確保所有 IME UI 元素都與您正在執行之進程的 UI 元素有所調整，您必須適當地回應不同的 DPI 值。

> [!NOTE]
> 為了確保與新的桌面應用程式具有相同的功能，您的 IME 應該支援每個監視器– DPI 感知，但不應宣告某種程度的認知本身。 系統會在每個案例中決定適當的調整需求。

如需桌面應用程式的 DPI 縮放支援需求的詳細資訊，請參閱 [高 DPI](/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows)。

## <a name="ime-installation"></a>輸入法安裝

如果您使用 Microsoft Visual Studio 建立您的輸入法，請使用協力廠商安裝程式（例如從 >flexera Software InstallShield）為您的 IME 建立安裝體驗。

下列步驟顯示如何使用 InstallShield 來為您的 IME DLL 建立安裝專案。

- 安裝 Visual Studio。
- 啟動 Visual Studio。
- **在 [檔案**] 功能表上，指向 [**新增**]，然後選取 [**專案**]。 [ **新增專案** ] 對話方塊隨即開啟。
- 在左窗格中，流覽至 [ **範本 > 其他專案類型] > 安裝和部署**]，按一下 [ **啟用 InstallShield 限量版**]，然後按一下 **[確定]**。 遵循安裝指示進行。
- 重新啟動 Visual Studio。
-  ( .sln) 檔案中開啟 IME 方案。
- 在方案總管中，以滑鼠右鍵按一下方案，指向 [ **加入**]，然後選取 [ **新增專案**]。 [ **加入新專案** ] 對話方塊隨即開啟。
- 在左側樹狀檢視控制項中，流覽至 [ **範本 > 其他專案類型] > [InstallShield 限量版**]。
- 在中央視窗中，按一下 [ **InstallShield 限量版專案**]。
- 在 [ **名稱** ] 文字方塊中，輸入 "SetupIME"，然後按一下 **[確定]**。
- 在 [ **專案助理** ] 對話方塊中，按一下 [ **應用程式資訊**]。
- 填寫您的公司名稱和其他欄位。
- 按一下 [ **應用程式檔**]。
- 在左窗格中，以滑鼠右鍵按一下 **[INSTALLDIR]** 資料夾，然後選取 [ **新增資料夾**]。 將資料夾命名為 "外掛程式"。
- 按一下 [ **新增**檔案]。 流覽至您的 IME DLL，然後將它新增至 [ **外掛程式** ] 資料夾。 針對 IME 字典重複此步驟。
- 以滑鼠右鍵按一下 IME DLL，然後選取 [ **屬性**]。 [ **屬性** ] 對話方塊隨即開啟。
- 在 [ **屬性** ] 對話方塊中，按一下 [ **COM & .net 設定** ] 索引標籤。
- 在 [ **註冊類型**] 下，選取 [ **自我註冊** ]，然後按一下 **[確定]**。
- 建置方案。 IME DLL 是建立的，InstallShield 會建立一個 setup.exe 檔案，讓使用者可以在 Windows 上安裝您的 IME。

若要建立您自己的安裝體驗，請在安裝期間呼叫 [ITfInputProcessorProfileMgr：： RegisterProfile](/windows/win32/api/msctf/nf-msctf-itfinputprocessorprofilemgr-registerprofile) 方法來註冊 IME。 請勿直接寫入登錄專案。

如果 IME 必須在安裝後立即可供使用，請呼叫 [InstallLayoutOrTip](/windows/win32/tsf/installlayoutortip) 將輸入法新增至使用者啟用的輸入方法，並針對 psz 參數使用下列格式：

`<LangID 1>:{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}`

## <a name="ime-accessibility"></a>IME 協助工具

執行下列慣例，讓您的 Ime 符合協助工具需求，以及使用朗讀程式。 若要讓候選清單成為可存取的，您的 Ime 必須遵循此慣例。

- 候選清單的 **UIA_AutomationIdPropertyId** 必須等於「IME_Candidate_Window」，才能取得轉換候選清單或「IME_Prediction_Window」以取得預測候選清單。
- 當候選清單出現並消失時，會分別引發 **UIA_MenuOpenedEventId** 和 **UIA_MenuClosedEventId**類型的事件
- 當目前選取的候選項變更時，候選清單會引發 **UIA_SelectionItem_ElementSelectedEventId**。 選取的元素應該有屬性 **UIA_SelectionItemIsSelectedPropertyId** 等於 **TRUE**。
- 候選清單中每個專案的 **UIA_NamePropertyId** 都必須是候選項目的名稱。 （選擇性）您可以提供其他資訊，透過 **UIA_HelpTextPropertyId**來區分候選項目。

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
- [協助工具](../accessibility/accessibility.md)