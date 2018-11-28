---
Description: This topic provides answers to frequently-asked questions and issues related to the Multilingual App Toolkit (MAT) 4.0.
title: 多語應用程式工具組常見問題集與疑難排解
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化
ms.localizationpriority: medium
ms.openlocfilehash: a39d2b3133714ab784309e131a71219beae4e3c0
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/27/2018
ms.locfileid: "7841322"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>多語應用程式工具組 4.0 常見問題集與疑難排解

本主題提供與多語應用程式工具組 (MAT) 4.0 相關的常見問題解答。

另請參閱[使用多語應用程式工具組 4.0](use-mat.md)。

**注意**：工具組同時支援 .resw (XAML) 和 .resjson (JavaScript) 檔案。 但本主題中我們只會使用 .resw 檔案。 .resw 檔案也稱為資源檔。 其中包含預設語言或翻譯成其他語言的字串。 包含 .resw 檔案的資料夾通常會為語言標記的值命名。

## <a name="do-i-need-resw-files-in-multiple-languages"></a>我需要多種語言的 .resw 檔案嗎？

不。 工具組的其中一個主要利益，便是您不需要多種語言的 .resw 檔案。 工具組會使用 .xlf 檔案管理及同步您應用程式的資源。 這個避免使多個 .resw 檔案的內容保持同步時可能會發生的相關問題。

包含相符 .resw 及 .xlf 檔案的專案會忽略來自 .xlf 檔案的翻譯。 當發生這種情形時，會在建置時顯示警告，告知您最終應用程式中將不會包含 .xlf 翻譯。 當具有相同語言代碼的目標語言時，.resw 檔案與 .xlf 檔案便相符。 其中一個相符配對的範例便是 `Strings\de-DE\Resources.resw` 及 `<project-name>.de-DE.xlf` 檔案 (包含 `target-language="de-DE"`)。

## <a name="can-i-have-resw-files-in-multiple-languages"></a>我可以擁有多種語言的 .resw 檔案嗎？

可以，但是我們不建議。 若您想要在您的專案中包含多種語言的 .resw 檔案及使用工具組，請確認您沒有相符的 .resw 及 .xlf 檔案。

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>我在 \[工具\] 功能表中找不到啟用多語應用程式工具組的選項

試試這些步驟。

- 請確認您在開啟 **\[工具\]** 功能表前選取的是專案節點，而非解決方案節點。
- 確認您已使用 Visual Studio 延伸模組管理員安裝工具組延伸模組。
- 確認您的專案為 UWP 專案。

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>當我建置專案時，我沒看到顯示「多語應用程式工具組建置已啟動」的訊息

確認您已為您的專案啟用 MAT。 在 **\[工具\]** 功能表上，選取 **\[多語應用程式工具組\]** > **\[啟用選取\]**。 若您的專案是使用先前版本啟用的，請使用 **\[工具\]** 功能表停用並重新啟用 MAT。 這會更新專案，使用新版本的工具組。

請確認您已安裝「為所有 Visual Studio 版本建置工作」元件。 這項建置元件會和延伸模組一同安裝，但可在安裝過程中手動取消選取。 這項元件為更新 .xlf 檔案並將翻譯新增到 PRI 檔案的必要元件。 若已安裝這項元件並且正常運作時，您便會看到這些建置訊息。

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>工具組回報在建置時並未找到任何 XLIFF 語言檔

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

當工具組在專案中找不到任何附檔名為 .xlf 的檔案時，便會顯示這個訊息。 根據預設，工具組會產生並將這些檔案放置在 `MultilingualResources` 資料夾中。 您可以移動這些檔案，但我們建議您將他們留在該資料夾中，因為這可讓多語編輯器找到相關的中繼資料檔案。

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>我的 .xlf 檔案並未包含在工具組於建置時處理的檔案清單中

若您手動將 .xlf 檔案從專案中排除，之後又重新加入，檔案類型元素的設定可能會不正確。 在 Visual Studio 中，選取檔案，然後查看 \[屬性\] 視窗。 檔案的 \[建置動作\] 必須設定為 \[XliffResource\]，而 \[複製到輸出目錄\] 則應設定為 \[不要複製\]。 您專案檔案中的參考看起來應該要像是這樣。

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>我已新增以 .xlf 作為基礎的語言。 我的字串在哪裡？

您的預設語言資源檔 (.resw) 是您應用程式使用字串的正式「結構描述」。 翻譯後的資源檔可能會包含這些字串的全部，或是一部分的子集。

當您建置專案時，您的資源檔和您的 .xlf 檔案便會同步。

- .xlf 檔案會更新，以反映任何新增或移除的字串，或是新增或移除的資源檔。
- 資源檔也會更新，以反映任何 .xlf 檔案中的翻譯字串。

這會在[使用多語應用程式工具組 4.0](use-mat.md) 中詳細解釋。

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>當我建置專案時，.xlf 檔案仍是空白的

在能有效使用 MAT 之前，您的應用程式必須要是可當地語系化的。 這會在[使用多語應用程式工具組 4.0](use-mat.md) 中詳細解釋。

## <a name="what-is-microsoft-translator"></a>什麼是 Microsoft Translator？

Microsoft Translator 是機器式翻譯的雲端式服務。 機器翻譯適合在無法合理取得人類翻譯時存取翻譯。 您可以在 [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) 深入了解。

工具組會使用 Microsoft Translator 服務為您提供翻譯建議。 您可以在 Microsoft Translator 圖示出現在 \[翻譯語言\] 對話方塊中時查看 Microsoft Translator 支援哪些語言。

您可以藉由選取字串並按一下 **\[翻譯\]** ，來在多語編輯器中使用 Microsoft Translator 快速翻譯您的應用程式。

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>虛擬語言是什麼，虛擬資源追蹤器又是什麼？

虛擬語言是軟體產品的人造修改，用來模擬真正的語言當地語系化。 您可以在[使用多語應用程式工具組 4.0](use-mat.md) 中找到更多關於虛擬語言和虛擬資源追蹤器的詳細資料。

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>我該如何將我的語言喜好設定設為虛擬語言，以便測試我的虛擬當地語系化字串？

這會在[使用多語應用程式工具組 4.0](use-mat.md) 中解釋。

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>在使用虛擬語言時，可能會發生何種可當地語系化性問題？

這會在[使用多語應用程式工具組 4.0](use-mat.md) 中解釋。

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>我在啟動我的應用程式時並未看到任何翻譯，或是我的應用程式只有部分翻譯

請在多語編輯器中開啟 .xlf 檔案，查看其中是否有翻譯。 當預設語言 .resw 檔案中的字串明確變更時，任何對應的翻譯都會從 .xlf 檔案中移除。 這是為了確保翻譯與其來源字串相符。 請翻譯 .xlf 檔案中的字串，重新建置以更新非預設語言的 .resw 檔案。

若您已在 .xlf 檔案中翻譯字串，但他們仍然沒有出現在您的應用程式中，請重新建置您的專案，更新非預設語言的 .resw 檔案。 Visual Studio 會最佳化 \[建置\] 命令，只建置任何自上一次建置後發生變更的檔案。

檢查您的語言喜好設定順序。 確保您想要測試的語言已列在 **\[設定\]** 中您的語言喜好設定清單的頂部。

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>工具組在建置輸出中回報錯誤 0x80004004

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

當地區格式與工具組的建置作業產生衝突時，便會顯示這個訊息。 因應措施是在建置時在 **\[設定\]** 中將您的語言變更為 zh-TW。


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>工具組在建置輸出中回報錯誤 0x80004005

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

當 .xlf 檔案包含不支援的目標語言時，便會顯示這個訊息。 例如，"zh-cht" 為不正確 (請變更為 "zh-hant")，"zh-chs" 也不正確 (請變更為 "zh-hans")。

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>有什麼方法可以找到更多我看到的錯誤的相關資訊嗎？

有，您可以在 Visual Studio 中開啟詳細記錄。 按一下 **\[工具\]** > **\[選項\]** > **\[專案及解決方案\]** > **\[建置及執行\]**。 將 **\[MSBuild 專案建置輸出詳細資訊\]** 從 \[最小\] 變更為 \[標準\] 或更高。

從命令列執行 MSBuild 也會產生額外的訊息。

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>匯入翻譯失敗

匯入處理序會在匯入前執行基本的驗證。 這會確保匯入檔案中的目標文化資訊與現有的 .xlf 檔案相符。 在多語編輯器中開啟 .xlf 檔案，確認文化資訊相符。

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>如果我的翻譯人員沒有安裝 Windows 10，和/或 Visual Studio 和/或多語應用程式工具組的話怎麼辦？

當您在匯出字串資源對話方塊中選取 **\[輸出: 郵件收件者\]** 時，電子郵件會包含一個可讓對方下載並安裝多語應用程式工具組 (MAT) 4.0 的連結。 即使沒有 Windows 10 和 Visual Studio，您的翻譯人員仍然可以安裝 MAT 4.0 獨立多語編輯器工具。

如需詳細資料，請參閱[使用多語應用程式工具組 4.0](use-mat.md)。

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>`MarkupRules.xml` 和 `ResourcesLocks.xml` 檔案怎麼了？

多語應用程式工具組 4.0 不再使用專用資源鎖定檔案。 而是會將 XLIFF 1.2 標籤 `<mrk>` 直接新增到 .xlf 檔案，識別在機器翻譯過程中並未修改的字串。 這可讓 XLIFF 檔案獨立，並允許以每個檔案為基礎的資源鎖定。

由於您不再需要這些額外的支援檔案，您可以安心的刪除他們 (若有的話)。

## <a name="what-happened-to-the-tpx-file"></a>.tpx 檔案怎麼了？

.tpx 檔案提供一種簡單的方式，可在送出 .xlf 檔案進行翻譯時於其中包含 `MarkupRules.xml` 和 `ResourcesLocks.xml` 檔案。 這項功能已不再需要。

若 .tpx 檔案中有您想要擷取的翻譯，您只需要將 .tpx 檔案附檔名重新命名為 .zip。 這可讓您使用檔案總管或任何 .zip 相容工具開啟並解壓縮內容。

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>我覺得我所有的操作都正確，但仍然無法運作

試試這些步驟。

1. 使用其中一種上述方式新增翻譯。
2. 傾印 .pri 檔案 (請參閱 [MakePri.exe 命令列選項](../../app-resources/makepri-exe-command-options.md)) 以查看您的翻譯是否在 .pri 檔案中。 翻譯會與語言代碼和翻譯值一同顯示如下。
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. 從命令提示字元建置；其產生的錯誤可能會顯示比建置輸出更多的詳細資料。

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>我的應用程式在提交到 Microsoft Store 時認證失敗

在您啟動 Microsoft Store 認證處理序前，您必須先從專案中排除 `<project-name>.qps-ploc.xlf` 檔案。 虛擬語言是用來偵測潛在的可當地語系化性問題或 Bug，並非有效的 Microsoft Store 語言。 若您並未移除它，您的應用程式便無法通過 Microsoft Store 認證處理序。

## <a name="related-topics"></a>相關主題

* [使用多語應用程式工具組 4.0](use-mat.md)
* [Microsoft Translator ](http://go.microsoft.com/fwlink/p/?LinkId=258220)
* [MakePri.exe 命令列選項](../../app-resources/makepri-exe-command-options.md)
