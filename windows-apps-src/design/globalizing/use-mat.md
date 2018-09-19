---
author: stevewhims
Description: The Multilingual App Toolkit (MAT) 4.0 integrates with Microsoft Visual Studio 2017 to provide UWP apps with translation support, translation file management, and editor tools.
title: 使用多語應用程式工具組
template: detail.hbs
ms.author: stwhi
ms.date: 01/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: 39e002247cabb6389ddf23860499ebf1f166b03a
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2018
ms.locfileid: "4059724"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>使用多語應用程式工具組 4.0

多語應用程式工具組 (MAT) 4.0 與 Microsoft Visual Studio 2017 整合，提供 UWP app 翻譯支援、翻譯檔案管理及編輯器工具。 以下是一些工具組的價值主張。

- 可協助您在開發期間管理資源變更與翻譯狀態。
- 提供根據設定的翻譯提供者選擇語言的 UI。
- 支援當地語系化業界標準的 XLIFF 檔案格式。
- 提供虛擬語言引擎，在開發期間協助識別翻譯問題。
- 與 Microsoft 語言入口網站連線，輕鬆存取翻譯字串及術語。
- 與 Microsoft Translator 連線，可快速取得翻譯建議。

## <a name="how-to-use-the-toolkit"></a>如何使用工具組

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>步驟 1. 將您的應用程式設計為支援全球化和當地語系化

在能有效使用 MAT 之前，您的應用程式必須要是可當地語系化的。 具體而言，您的專案應具備一或多個資源檔 (.resw)，其中包含您應用程式的預設語言字串。 如需詳細資料，請參閱[當地語系化您 UI 和應用程式封裝資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)。 完成此操作後，使用工具組便可快速、輕鬆的新增其他語言。

如需取得全球化及當地語系化&mdash;其中包括 **「全球化」**、**「可當地語系化性」** 及 **「當地語系化」** 等術語的定義&mdash;的價值主張，請參閱[全球化與當地語系化](globalizing-portal.md)。

另請參閱[全球化方針](guidelines-and-checklist-for-globalizing-your-app.md)及[讓您的應用程式可當地語系化](prepare-your-app-for-localization.md)。

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>步驟 2. 下載並安裝多語應用程式工具組 4.0

多語應用程式工具組 4.0 (MAT 4.0) 有兩個部分，每個部分都有自己的安裝程式。

- [Visual Studio 2017 多語應用程式工具組 4.0 延伸模組](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)。 其中包含適用於 Visual Studio 2017 的 MAT 4.0 延伸模組，其形式為 .vsix 安裝程式。
- [多語應用程式工具組 4.0 編輯器](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)。 其中包含 MAT 4.0 獨立多語編輯器工具，其形式為 .msi 安裝程式。 它同時也包含適用於 Visual Studio 2015 和 Visual Studio 2013 的 MAT 4.0 延伸模組。

若您是使用 Visual Studio 2017，請接連下載並執行這兩種安裝程式。 若您是使用 Visual Studio 2015 或 Visual Studio 2013，請下載並執行 .msi 安裝程式。

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>步驟 3. 為您的專案啟用多語應用程式工具組

您必須為您的專案啟用 MAT，才能開始當地語系化應用程式。 以下是啟用工具組的方法。

- 在 Visual Studio 中開啟專案解決方案。
- 在 \[方案總管\] 中選擇想要的專案。
- 在 **\[工具\]** 功能表上，選取 **\[多語應用程式工具組\]** > **\[啟用選取\]**。 

在 \[輸出\] 視窗中 (顯示多語應用程式工具組的輸出)，注意是否有以下訊息：`Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`。 如果出現此訊息，表示 MAT 已準備就緒可供使用。

### <a name="step-4-add-languages-to-your-project"></a>步驟 4. 將語言新增至您的專案

請依照下列步驟來將語言新增到您的專案。

1. 在 \[方案總管\] 中，以滑鼠右鍵按一下專案節點。
2. 按一下 **\[多語應用程式工具組\]** > **\[新增翻譯語言...\]**。
3. 在 \[翻譯語言\] 對話方塊中，選取您想要支援的語言，然後按一下 \[確定\]。

工具組會反映並執行下列作業。

- 針對每個您新增的語言，會使用該語言的 [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)建立新的資料夾。 在該資料夾中，會建立新的資源檔 (.resw)，對應每個包含預設語言字串的檔案。
- 若這次您第一次新增語言，名為 `MultilingualResources` 的新資料夾便會新增到專案中。 在該資料夾中，會為每個語言新增一個 .xlf 檔案。 .xlf 檔案包含您專案中每個資源檔 (.resw) 中每個字串的翻譯單位。
- \[輸出\] 視窗會確認您新增的語言。

每當您新增/移除預設語言資源檔 (.resw) 時，或是在預設語言資源檔 (.resw) 中新增/移除字串時，請重新建置專案以重新同步 .xlf 檔案。 這可確保 .xlf 檔案包含預設語言中字串的聯集。

已安裝的翻譯提供者&mdash;例如 [Microsoft 語言入口網站](http://go.microsoft.com/fwlink/p/?LinkId=330295)和 [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)&mdash;可用來翻譯您應用程式的資源。 當提供者支援特定語言時，提供者的圖示便會顯示在 \[翻譯語言\] 對話方塊中語言名稱的旁邊。

在 \[翻譯語言\] 對話方塊中，任何工具組找到的現有 .xlf 式語言的選取方塊都會預先勾選，表示該語言已包含在專案中。

語言新增到專案後，便無法藉由取消勾選在 \[翻譯語言\] 對話方塊中的核取方塊來移除。 若要移除語言，請在特定語言的 .xlf 檔案上按一下滑鼠右鍵，然後選取 **\[刪除\]**。 在確認之後，便會刪除對應的資源檔 (.resw)。

### <a name="step-5-test-your-app-using-pseudo-language"></a>步驟 5. 使用虛擬語言測試您的應用程式

虛擬語言是軟體產品的人造修改，用來模擬真正的語言當地語系化，但仍會維持在可供母語人士閱讀的狀態。 虛擬翻譯會取代字元並展開資源字串長度，以在專案週期的初期並在當地語系化正式開始前偵測潛在的可當地語系化性問題或 Bug。

請依照下列步驟來虛擬當地語系化並測試您的專案。

1. 使用 \[翻譯語言\] 對話方塊來將虛擬語言 (Pseudo) \[qps-ploc\] 新增到您的專案。
2. 在方案總管中以滑鼠右鍵按一下 `<project-name>.qps-ploc.xlf` 檔案，然後按一下 **\[多語應用程式工具組\]** > **\[產生機器翻譯\]**。
3. 在 **\[設定\]** > **\[時間與語言\]** > **\[地區與語言\]** > **\[語言\]** 中，按一下 **\[新增語言\]**。
5. 在搜尋方塊中鍵入 `qps-ploc`。
6. 按一下 `English (qps-ploc)` 以新增它。
7. 從語言清單中，選取 `English (qps-ploc)`，然後按一下 **\[設定成預設值\]**。
8. 測試您的虛擬當地語系化應用程式。 例如，尋找並未顯示所有字串 (字串遭到截斷) 的 UI 配置問題，或是並未翻譯的字串 (硬式編碼)。

除了字元取代和擴充外，虛擬引擎還為每個資源提供唯一的追蹤識別碼。 此追蹤器會加到每個字串的開頭，並以中括弧環繞 `[xxxxx]`。 您可以在視覺 UI 檢查測試過程中使用這些追蹤器。 他們可協助追蹤產品中的特定資源，尤其是當多個資源有相似或重複的文字時。

在此案例中為 "Hello, World!" 文字範例，虛擬翻譯會展開並多占據約 30% 的螢幕空間，然後套用資源追蹤器。

```
"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"
```

### <a name="step-6-translate-your-app-into-selected-languages"></a>步驟 6. 將您的應用程式翻譯成選取的語言

多語應用程式工具組已整合至建置處理序。 在建置過程中，更新的字串會自動新增到每個語言的 .xlf 檔案。
在您使用虛擬語言測試應用程式之後，有三種選項可以將您的應用程式翻譯成其他語言以供發行。

#### <a name="option-1-translate-the-strings-yourself"></a>選項 1. 自行翻譯字串

您可以使用多語編輯器翻譯個別字串。 如前所述，此工具隨附於 [.msi 安裝程式](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)中。

- 在您想要翻譯的 .xlf 檔案上按一下滑鼠右鍵。
- 按一下 **\[開啟檔案...\]**，然後按一下多語編輯器。 您也可以選擇性的按一下 **\[設定為預設值\]**。
- 針對每個字串，**\[來源\]** 會以預設語言顯示原始字串。 在 **\[翻譯\]** 中，鍵入翻譯成您正在編輯之 .xlf 檔案適當語言的字串。
- 完成時，請儲存並關閉檔案。

重新建置您的專案會將已翻譯的字串複製到對應您方才編輯之 .xlf 檔案的資源檔 (.resw)。

您也可以使用此方法啟動多語編輯器。 移至 \[開始\]，顯示所有應用程式，開啟多語應用程式工具組資料夾，然後按一下多語編輯器來啟動它。

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>選項 2. 將 .xlf 檔案傳送到協力廠商進行翻譯

若要將翻譯及編輯工作外包給負責當地語系化的人員，請在方案總管中選取想要的 .xlf 檔案，以滑鼠右鍵按一下他們，然後按一下 **\[多語應用程式工具組\]** > **\[匯出翻譯...\]**。

在匯出字串資源對話方塊中選取 **\[輸出: 郵件收件者\]**，然後按一下 \[確定\]，您的檔案便會進行壓縮並附加到新的電子郵件。 選取 **\[輸出: 檔案資料夾位置\]**，瀏覽資料夾並按一下 \[確定\]，選擇性的選擇要壓縮的檔案，再次按一下 \[確定\]，您的檔案便會 (在壓縮之後) 儲存到您選擇的位置，位於以您的專案命名的新資料夾中。

在您負責當地語系化的人員完成工作，並將翻譯完的 .xlf 檔案傳送給您之後，您便可以將檔案匯入到您的專案中。 在方案總管中選取想要的 .xlf 檔案，以滑鼠右鍵按一下他們，按一下 **\[多語應用程式工具組\]** > **\[匯入/回收翻譯\]**。按一下 **\[新增\]**，巡覽至 .xlf 或 .zip 檔案，然後按一下 **\[匯入\]**。

**注意**：匯入處理序會在匯入前執行基本的驗證。 這會確保匯入檔案中的目標文化資訊與現有的 .xlf 檔案相符。

重新建置您的專案會將已翻譯的字串複製到對應您方才匯入之 .xlf 檔案的資源檔 (.resw)。

以下協力廠商提供當地語系化服務，且可能可以協助您。

- [Elanex](https://www.elanex.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.moravia.com/)
- [SDL](https://www.sdl.com/languagecloud/managed-translation/ilp/instantquote)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> 上述清單僅供參考，並非背書。 Microsoft 不代表這些廠商或為其服務提供保證，並且對於您和這些廠商合作或使用其服務，Microsoft 在任何情況下皆不負任何責任。 若對這些廠商或其服務有任何問題、投訴或索賠，必須直接連絡該廠商。

#### <a name="option-3-use-the-integrated-translation-services"></a>選項 3. 使用整合式的翻譯服務

翻譯服務已整合至 Visual Studio IDE 及多語編輯器中。 這可在開發您的產品及當地語系化您的資源時提供翻譯服務的輕鬆存取。 若要使用這項服務，您將需要 Azure 帳戶訂用帳戶，如 [Microsoft Translator 已移動到 Azure 入口網站](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal)中所述。

若要在 Visual Studio 中存取翻譯服務，請在方案總管中選取並以滑鼠右鍵按一下一或多個 .xlf 檔案，然後按一下 **\[產生機器翻譯\]**。

多語編輯器提供相同的翻譯支援及互動式的翻譯建議，可讓您選取最符合您資源字串的翻譯。 在提供翻譯建議之後，您便可以微調字串，使其更符合您的翻譯風格。

兩個提供者都隨附於多語應用程式工具組中。

- [Microsoft 語言入口網站](http://go.microsoft.com/fwlink/p/?LinkId=330295)提供者可根據 Microsoft 產品及服務的使用者介面文字翻譯，啟用翻譯回收並提供術語比對支援。
- [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) 提供者可啟用隨選機器翻譯服務。

您和您的翻譯人員可以在多語編輯器中管理翻譯狀態，並在稍後檢閱不確定的翻譯。 您可以在 **\[屬性\]** 索引標籤中設定每個字串的狀態。狀態值為：**\[新字串\]**、**\[需要檢閱\]**、**\[已翻譯\]**、**\[最終版本\]** 及 **\[已完結\]**。 資料列左側的指標會顯示狀態。 當所有資料列在多語編輯器中都已顯示為綠色時，您的翻譯工作便已完成。

重新建置您的專案會將已翻譯的字串複製到對應您方才編輯之 .xlf 檔案的資源檔 (.resw)。

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>步驟 7. 將您的應用程式上傳到 Microsoft Store

在您啟動 Microsoft Store 認證處理序前，您必須先從專案中排除 `<project-name>.qps-ploc.xlf` 檔案。 虛擬語言是用來偵測潛在的可當地語系化性問題或 Bug，並非有效的 Microsoft Store 語言。 若您並未移除它，您的應用程式便無法通過 Microsoft Store 認證處理序。

## <a name="related-topics"></a>相關主題

* [當地語系化您 UI 及應用程式封裝資訊清單中的字串](../../app-resources/localize-strings-ui-manifest.md)
* [全球化和當地語系化](globalizing-portal.md)
* [全球化指導方針](guidelines-and-checklist-for-globalizing-your-app.md)
* [讓您的應用程式可當地語系化](prepare-your-app-for-localization.md)
* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>下載

* [多語應用程式工具組 4.0 .vsix 安裝程式](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [多語應用程式工具組 4.0 .msi 安裝程式](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>翻譯服務

* [Microsoft 語言入口網站](http://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
