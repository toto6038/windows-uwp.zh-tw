---
author: DelfCo
Description: "藉由使用 Windows 所提供的各種語言及地區設定，控制 Windows 如何選取 UI 資源，以及如何將應用程式的 UI 元素格式化。"
title: "管理語言及地區"
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
label: Manage language and region
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a3924fef520d7ba70873d6838f8e194e5fc96c62
ms.openlocfilehash: f9c30e68d5cc94769c9304234db0276e34e1d945

---

# <a name="manage-language-and-region"></a>管理語言及地區

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

藉由使用 Windows 所提供的各種語言及地區設定，控制 Windows 如何選取 UI 資源，以及如何將應用程式的 UI 元素格式化。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)</li>
<li>[**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)</li>
<li>[**WinJS.Resources 命名空間**](https://msdn.microsoft.com/library/windows/apps/br229779)</li>
</ul>
</div>


## <a name="introduction"></a>簡介


如需示範如何管理語言及地區設定的範例應用程式，請參閱[應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=231501)。

Windows 使用者不需要從一組有限的語言中只選擇一種語言。 使用者可以告訴 Windows 他們使用世界上的任一種語言，即使 Windows 本身並未翻譯為該種語言。 使用者甚至可以指定他們可以使用多種語言。

Windows 使用者可以指定他們的位置，可以是世界上的任一處。 此外，使用者還可以指定他們是在任何位置使用任何語言。 位置和語言不會彼此限制。 只因為使用者使用法文，並不表示他們位於法國，或者只因為使用者身處法國，並不代表他們慣用法文。

Windows 使用者可以利用與 Windows 完全不同的語言來執行應用程式。 例如，使用者可以執行西班牙文版的應用程式，而 Windows 執行的是英文版。

對於 Windows 市集應用程式，語言是以 [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)代表的。 Windows 執行階段、HTML 以及 XAML 中大部分的 API 可以傳回或接受這些 BCP-47 語言標記的字串表示法。 另請參閱 [IANA 語言清單](http://go.microsoft.com/fwlink/p/?linkid=227303)。

如需 Windows 市集具體支援的語言標記清單，請參閱[支援的語言](https://msdn.microsoft.com/library/windows/apps/jj657969)。

## <a name="tasks"></a>工作


### <a name="users-can-set-their-language-preferences"></a>使用者可以設定他們的語言喜好設定。

使用者語言喜好設定清單是排序的語言清單，依照使用者慣用的語言順序來說明它們。

使用者可以在 **[設定]** &gt; **[時間與語言]** &gt; **[地區與語言]** 中設定這個清單。 或者，他們也可以使用 **[控制台]** &gt; **[時鐘、語言和區域]** 來設定這個清單。

使用者的語言喜好設定清單可包含多種語言，以及地區變體或特定變體。 例如，使用者可能慣用 fr-CA，但是也可以了解 en-GB。

### <a name="specify-the-supported-languages-in-the-apps-manifest"></a>在應用程式資訊清單中指定支援的語言。

請在應用程式資訊清單檔 (通常是 Package.appxmanifest) 中的 [**Resources element**](https://msdn.microsoft.com/library/windows/apps/dn934770) 指定您應用程式支援的語言，否則 Visual Studio 會根據在專案中找到的語言，自動在資訊清單檔中產生語言清單。 資訊清單應該在適當的精細度層級正確說明支援的語言。 資訊清單中列出的語言，便是使用者在 Windows 市集中所見的語言。

### <a name="specify-the-default-language"></a>指定預設語言。

在 Visual Studio 開啟 package.appxmanifest，前往 **[應用程式]** 索引標籤，並將您的預設語言設為撰寫應用程式所使用的語言。

當應用程式不支援使用者選擇的任何語言時，會使用預設語言。 Visual Studio 會使用預設語言將中繼資料加入以該語言標記的資產中，以便在執行階段選擇適當的資產。

預設語言屬性也必須設定為資訊清單中的第一個語言，以便適當設定應用程式語言 (如下列的＜建立應用程式語言清單＞步驟所述)。 預設語言中的資源仍然必須符合它們的語言資格 (例如，en-US/logo.png)。 預設語言不會指定不符合資產的隱含語言。 若要深入了解，請參閱[如何使用限定詞命名資源](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)。

### <a name="qualify-resources-with-their-language"></a>使用他們的語言限定資源。

仔細考量您的對象以及您的目標使用者的語言和位置。 許多同住在一個地區的人，並不是慣用該地區的主要語言。 例如，美國有數百萬家庭的主要語言是西班牙文。

當您使用語言限定資源時：

-   如果語言沒有定義抑制指令碼值，則包含指令碼。 如需語言標記詳細資訊，請參閱 [IANA 子標記登錄](http://go.microsoft.com/fwlink/p/?linkid=227303)。 例如，使用 zh-Hant、zh-Hant-TW 或 zh-Hans，不使用 zh-CN 或 zh-TW。
-   使用某種語言標示所有語言內容。 預設的語言專案屬性不是未標示之資源的語言 (即中性語言)；它會指定在沒有其他標示的語言資源符合使用者的情況下，應該選擇哪個標示的語言資源。

使用正確的內容表示法來標示資產。

-   Windows 會執行複雜比對 (包括跨地區變體 (例如 en-US 對 en-GB) 在內)，因此應用程式可以自由使用正確的內容表示法來標示資產，然後讓 Windows 為每位使用者進行適當的比對。
-   Windows 市集會向查看應用程式的使用者顯示資訊清單中的內容。
-   請注意，某些工具和其他元件 (如電腦翻譯工具) 可能會找到特定語言標記 (如地區方言資訊)，這有助於了解資料。
-   請務必以完整詳細資料標示資產，尤其在有多種變體存在時。 例如，請標示 en-GB 和 en-US (如果兩者都是該地區的特定語言)。
-   如果語言只有單一標準方言，則不需要新增地區。 在某些情況下，一般標記是合理的，例如使用 ja (而非 ja-JP) 標示資產。

有時候，可能不需要當地語系化所有資源。

-   對於含有所有語言的資源 (如 UI 字串)，請以它們的適當語言標示，並確認這些資源全都使用預設語言。 不需要指定中性資源 (未標示語言的資源)。
-   對於含有整個應用程式的語言集之子集的資源 (部分當地語系化)，請指定資產確實含有的語言集，並確定這些資源全都使用預設語言。 Windows 會依照使用者喜好設定順序查看他們使用的所有語言，藉此為使用者挑選最適合的語言。 例如，如果應用程式含有西班牙文的完整資源集合，則並非所有應用程式的 UI 都可以當地語系化成卡達隆尼亞文。 對於優先使用卡達隆尼亞文、其次為西班牙文的使用者來說，未以卡達隆尼亞文提供的資源會以西班牙文顯示。
-   對於某些語言有特定例外，而其他所有語言則對應至某個共通資源的資源來說，應用在所有語言的資源應以未定語言標記 'und' 標示。 Windows 會以類似 '\*' 的方式解譯 'und' 語言標記，即它會比對任何其他特定的相符項目後的最上層應用程式語言。 例如，如果芬蘭文的少數資源 (如元素的寬度) 不相同，但所有語言的其餘資源均相同，則芬蘭文資源應以芬蘭文語言標記標示，其餘則應以 'und' 標示。
-   對於以語言的指令碼 (而非語言) 為基礎的資源 (例如字型或文字高度)，請使用含有指定之指令碼 'und-&lt;script&gt;' 的未定語言標記。 例如，對於拉丁文字型，請使用 und-Latn\fonts.css，對於斯拉夫文字型，請使用 und-Cryl\fonts.css。

### <a name="create-the-application-language-list"></a>建立應用程式語言清單。

在執行階段，系統會判斷應用程式在其資訊清單宣告支援的使用者語言喜好設定，然後建立*應用程式語言清單*。 它使用這個清單判斷應用程式應使用的語言。 這個清單會判斷針對應用程式和系統資源、日期、時間及數字，以及其他元件使用的語言。 例如，資源管理系統 ([**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)、[**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) 和 [**WinJS.Resources**](https://msdn.microsoft.com/library/windows/apps/br229779) 命名空間) 會根據應用程式語言載入 UI 資源。 [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 也會根據應用程式語言清單選擇格式。 應用程式語言清單可以使用 [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396) 取得。

比對語言和資源並不容易。 建議您讓 Windows 處理比對，因為一種語言標記有許多選用元件會影響比對的優先順序，而實際上可能會遇到這些元件。

語言標記中的選用元件範例有：

-   抑制指令碼語言的指令碼。 例如，en-Latn-US 與 en-US 相符。
-   地區。 例如，en-US 與 en 相符。
-   變體。 例如，de-DE-1996 與 de-DE 相符。
-   -x 和其他延伸。 例如，en-US-x-Pirate 與 en-US 相符。

語言標記還有許多元件不是 xx 或 xx-yy 格式，而且並非全部相符。

-   zh-Hant 與 zh-Hans 不相符。

Windows 會以標準且眾所周知的方式來排列語言相符結果的優先順序。 例如，依照優先順序，en-US 與 en-US、en、en-GB 等相符。

-   Windows 會執行跨地區比對。 例如，en-US 與 en-US 相符，然後是 en，然後是 en-\*。
-   Windows 含有用於地區內之親和性比對的其他資料，例如某種語言的主要地區。 例如，fr-FR 與 fr-BE 的相符性比 fr-CA 更高。
-   當您仰賴 Windows API 時，將可免費取得 Windows 中任何的後續語言比對改良功能。

系統會先比對清單中的第一個語言，然後再比對清單中的第二個語言，即使是其他地區變體也一樣。 例如，如果應用程式語言為 en-US，則會優先選擇 en-GB 資源，而不是 fr-CA 資源。 只在 en 形式都沒有資源時，才會選擇 fr-CA 資源。

應用程式語言清單會設定為使用者的地區變體，即使它與應用程式提供的地區變體不同也一樣。 例如，如果使用者使用的是 en-GB，但是應用程式支援 en-US，則應用程式語言清單會包含 en-GB。 這確保日期、時間及數字的格式化方式會更貼近使用者的預期 (en-GB)，但是仍然會在應用程式支援的語言 (en-US) 中載入 UI 資源 (由於語言比對的緣故)。

應用程式語言清單由以下項目組成：

1.  **(選擇性) 主要語言覆寫** [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398) 是一個簡單的應用程式覆寫設定，可讓使用者選擇自己的語言，或是讓應用程式有充分的理由可覆寫預設語言選擇。 若要深入了解，請參閱[應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=231501)。
2.  **應用程式支援的使用者語言。** 這是一份使用者的語言喜好設定清單，以語言喜好設定排序。 此清單是由應用程式資訊清單中的支援語言清單所篩選出。 依應用程式支援的語言篩選使用者的語言可在軟體開發套件 (SDK)、類別庫、相依架構套件及應用程式之間維持一致性。
3.  **如果第 1 項和第 2 項空白，則為預設語言或應用程式所支援的第一個語言。** 如果使用者使用的是應用程式不支援的語言，選擇的應用程式語言會是應用程式支援的第一個語言。

如需範例，請參閱下方的＜備註＞一節。

### <a name="set-the-http-accept-language-header"></a>設定 HTTP Accept Language 標頭。

在典型 Web 要求和 XMLHttpRequest (XHR) 中，從 Windows 市集應用程式和傳統型應用程式提出的 HTTP 要求，會使用標準的 HTTP Accept-Language 標頭。 HTTP 標頭預設會設定為使用者的語言喜好設定 (依據使用者慣用的順序)，按照在 **[設定]** &gt; **[時間與語言]** &gt; **[地區與語言]** 中所指定。 清單中的每個語言都可進一步展開，以包含中性語言和加權 (q)。 例如，fr-FR 和 en-US 的使用者語言清單會產生 fr-FR、fr、en-US、en ("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3") 的 HTTP Accept-Language 標頭。

### <a name="use-the-apis-in-the-windowsglobalization-namespace"></a>在 Windows.Globalization 命名空間中使用 API。

通常，[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 命名空間中的 API 元素會使用應用程式語言清單來判斷語言。 如果沒有找到具有相符格式的語言，就會使用使用者地區設定。 這是用於系統時鐘的相同地區設定。 使用者地區設定可以從 **[設定]** &gt; **[時間與語言]** &gt; **[地區與語言]** &gt; **[其他日期、時間及區域設定]** &gt; **[地區: 變更日期、時間或數字格式]** 中取得。 **Windows.Globalization** API 也接受覆寫以指定要使用的語言清單，取代應用程式語言清單。

[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 也有提供做為協助程式物件的 [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) 物件。 這個物件讓應用程式可以檢查有關語言的詳細資料，例如語言的指令碼、顯示名稱及原始名稱。

### <a name="use-geographic-region-when-appropriate"></a>適當使用地理區域。

您可以利用使用者的住家地理區域設定 (而不使用語言) 來選擇要為使用者顯示哪些內容。 例如，新聞應用程式可以預設為顯示來自使用者住家位置的內容，該位置是在安裝 Windows 時所設定，如先前的工作中所述，可以從 **[地區: 變更日期、時間或數字格式]** 中取得。 您可以使用 [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829) 來抓取目前使用者的住家地區設定。

[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) 也有提供做為協助程式物件的 [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795) 物件。 這個物件可讓應用程式檢查有關特定地區的詳細資料，例如它的顯示名稱、原始名稱及使用中的貨幣。

## <a name="remarks"></a>備註


下表包含一些範例，說明使用者在各種語言與地區設定下，在應用程式 UI 中會看見的內容。

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">應用程式支援的語言 (定義於資訊清單中)</th>
<th align="left">使用者語言喜好設定 (設定於控制台中)</th>
<th align="left">應用程式的主要語言覆寫 (選擇性)</th>
<th align="left">應用程式語言</th>
<th align="left">使用者在應用程式中看見的內容</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">英文 (英國) (預設值)； 德文 (德國)</td>
<td align="left">英文 (英國)</td>
<td align="left">無</td>
<td align="left">英文 (英國)</td>
<td align="left">UI：英文 (英國)<br>日期/時間/數字：英文 (英國)</td>
</tr>
<tr>
<td align="left">德文 (德國) (預設值)；法文 (法國)；義大利文 (義大利)</td>
<td align="left">法文 (奧地利)</td>
<td align="left">無</td>
<td align="left">法文 (奧地利)</td>
<td align="left">UI：法文 (法國) (從法文 (奧地利) 後援)<br>日期/時間/數字：法文 (奧地利)</td>
</tr>
<tr>
<td align="left">英文 (美國) (預設值)；法文 (法國)；英文 (英國)</td>
<td align="left">英文 (加拿大)；法文 (加拿大)</td>
<td align="left">無</td>
<td align="left">英文 (加拿大)；法文 (加拿大)</td>
<td align="left">UI：英文 (美國) (從英文 (加拿大) 後援)<br>日期/時間/數字：英文 (加拿大)</td>
</tr>
<tr>
<td align="left">西班牙文 (西班牙) (預設值)；西班牙文 (墨西哥)；西班牙文 (拉丁美洲)；葡萄牙文 (巴西)</td>
<td align="left">英文 (美國)</td>
<td align="left">無</td>
<td align="left">西班牙文 (西班牙)</td>
<td align="left">UI：西班牙文 (西班牙) (使用預設值，因為英文沒有可用的後援)<br>日期/時間/數字：西班牙文 (西班牙)</td>
</tr>
<tr>
<td align="left">卡達隆尼亞文 (預設值)；西班牙文 (西班牙)；法文 (法國)</td>
<td align="left">卡達隆尼亞文；法文 (法國)</td>
<td align="left">無</td>
<td align="left">卡達隆尼亞文；法文 (法國)</td>
<td align="left">UI：大部分卡達隆尼亞文和部分法文 (法國)，因為不是所有字串都是卡達隆尼亞文<br>日期/時間/數字：卡達隆尼亞文</td>
</tr>
<tr>
<td align="left">英文 (英國) (預設值)；法文 (法國)；德文 (德國)</td>
<td align="left">德文 (德國)； 英文 (英國)</td>
<td align="left">英文 (英國) (使用者在應用程式的 UI 中選擇)</td>
<td align="left">英文 (英國)；德文 (德國)</td>
<td align="left">UI：英文 (英國) (語言覆寫)<br>日期/時間/數字：英文 (英國)</td>
</tr>
</tbody>
</table>

 

## <a name="related-topics"></a>相關主題


* [BCP-47 語言標記](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [IANA 語言清單](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [應用程式資源和當地語系化範例](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [支援的語言](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 






<!--HONumber=Dec16_HO2-->


