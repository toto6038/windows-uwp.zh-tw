---
author: stevewhims
Description: Design your app to provide bidirectional text support (BiDi) so that you can combine script from left-to-right (LTR) and right-to-left (RTL) writing systems, which generally contain different types of alphabets.
title: 將您的應用程式設計為支援雙向文字
template: detail.hbs
ms.author: stwhi
ms.date: 11/10/2017
ms.topic: article
keywords: windows 10, uwp, 全球化, 可當地語系化性, 當地語系化, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: 24e4c5dfce4aa3e773ab8c334ca732ac5ed53030
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "5694091"
---
# <a name="design-your-app-for-bidirectional-text"></a>將您的應用程式設計為支援雙向文字

設計您的應用程式，使其提供雙向文字支援 (BiDi)，以結合由右至左 (RTL) 及由左至右 (LTR) 書寫系統的文字。這類書寫系統通常也包含不同類型的字母。

由右至左的書寫系統，例如在中東、中亞及南亞和非洲等國使用的系統，具有獨特的設計需求。 這些書寫系統需要雙向文字支援 (BiDi)。 雙向支援為可輸入和顯示由右至左 (RTL) 及由左至右 (LTR) 順序文字配置的功能。

Windows 內含九種雙向語言。
- 兩種已完整當地語系化的語言。 阿拉伯文和希伯來文。
- 七種適用於新興市場的語言介面套件。 波斯文、烏都文、達利文、中庫德文、信德文、旁遮普文 (巴基斯坦) 和維吾爾文。

本主題包含 Windows 雙向設計哲學，以及顯示雙向設計考量項目的案例研究。

## <a name="bidi-design-elements"></a>雙向設計元素

四種元素會影響 Windows 中的雙向設計決策。

- **使用者介面 (UI) 鏡像**。 使用者介面 (UI) 流程允許由右至左內容以其原生配置顯示。 UI 設計必須要讓雙向市場感覺足夠本地化。
- **使用者體驗的一致性**。 設計在以由右至左的方向呈現時也要感覺自然。 UI 元素共用一致的配置方向，並且要以使用者預期的方式呈現。
- **觸控最佳化**。 與非鏡像 UI 中的觸控 UI 相似，元素必須要在可輕易觸及的範圍內，並且要與觸控自然的進行互動。
- **混合文字支援**。 文字方向性的支援必須要能良好呈現混合文字 (位於雙向組建上的英文文字，反之亦然)。

## <a name="feature-design-overview"></a>功能設計概觀

Windows 支援四種雙向設計元素。 讓我們查看一些 Windows 中的主要相關功能，並提供一些內容，了解他們如何影響您的應用程式。

### <a name="navigate-in-the-direction-that-feels-natural"></a>以感覺自然的方向巡覽

Windows 會調整字型格線的方向，使其能夠從右流向左，表示格線上的第一個磚會位於右上角，最後一個磚則會位於左下角。 這符合印刷出版品，例如書籍和雜誌的 RTL 模式。使用者的閱讀方式總是從右上角開始往左閱讀。

![雙向 [開始] 功能表](images/56283_BIDI_01_startscreen_resized.png)
![雙向 [開始] 功能表 (附帶常用鍵)](images/56283_BIDI_02_startscreen_charm_resized.png)

若要保留一致的 UI 流程，使磚上的內容維持在由右至左的配置，表示應用程式的名稱和標誌會位於磚的右下角，無論應用程式的 UI 語言為何。

#### <a name="bidi-tile"></a>雙向磚

![雙向磚](images/56284_BIDI_03_tile_callouts_withKey.png)

#### <a name="english-tile"></a>英文磚

![英文磚](images/56284_BIDI_03_tile_callouts_en-us.png)

### <a name="get-tile-notifications-that-read-correctly"></a>取得正確閱讀的磚通知

具備混合文字支援的磚 通知區域具備內建彈性，可根據通知的語言調整文字的對齊。  當應用程式傳送阿拉伯文、希伯來文或其他雙向語言通知時，文字便會靠右對齊。 當英文 (或其他 LTR) 通知抵達時，便會靠左對齊。

![磚通知](images/56285_BIDI_04_bidirectional_tiles_white.png)

### <a name="a-consistent-easy-to-touch-rtl-user-experience"></a>一致且輕鬆觸控的 RTL 使用者體驗

Windows UI 中的每個元素都可和 RTL 方向結合。 常用鍵及飛出視窗的位置位於畫面的左側邊緣，不會與搜尋結果重疊或降低觸控最佳化。 他們可以輕鬆使用大拇指操控。

![雙向螢幕擷取畫面](images/56286_BIDI_05_search_flyout_resized.png)
![雙向螢幕擷取畫面](images/56286_BIDI_06_print_flyout_resized.png)

![雙向螢幕擷取畫面](images/56286_BIDI_07_settings_flyout_resized.png)
![雙向螢幕擷取畫面](images/56286_BIDI_08_app_bars_resized.png)

### <a name="text-input-in-any-direction"></a>任何方向的文字輸入

Windows 提供螢幕觸控式鍵盤，簡潔而不紊亂。 針對雙向語言，其中還具備了文字方向 Ctrl 鍵，可隨需要切換。

![適用於雙向語言的觸控式鍵盤](images/56287_BIDI_09_keyboard_layout_resized.png)

### <a name="use-any-app-in-any-language"></a>在任何語言中使用任何應用程式

在任何語言中安裝及使用您最愛的應用程式。 應用程式的呈現方式及功能與在非雙向版本 Windows 上如出一轍。 應用程式中的元素永遠都會位在一致且可預測的位置。

![顯示雙向內容的英文應用程式](images/56288_BIDI_10_english_app_resized.png)

### <a name="display-parentheses-correctly"></a>正確顯示括弧

透過引進雙向括弧演算法 (BPA)，成對的括弧永遠都會正常顯示，無論語言或文字的對齊屬性為何。

#### <a name="incorrect-parentheses"></a>不正確的括弧

![括弧顯示不正確的雙向應用程式](images/56289_BIDI_11_parentheses_resized.png)

#### <a name="correct-parentheses"></a>正確的括弧

![括弧顯示正確的雙向應用程式](images/56289_BIDI_12_parentheses_fixed_resized.png)

### <a name="typography"></a>印刷樣式

Windows 會針對所有雙向語言使用 Segoe UI 字型。 這個字型的形狀和縮放比例是針對 Windows UI 設計的。

![適用於雙向語言的 Segoe UI 字型](images/56290_BIDI_13_start_screen_segoe.png)
![適用於雙向語言的 Segoe UI 字型](images/56290_BIDI_13_start_screen_segoe_arabic.png)

## <a name="case-study-1-a-bidi-music-app"></a>案例研究 #1：雙向音樂應用程式

### <a name="overview"></a>概觀

設計多媒體應用程式是一項相當有趣的挑戰，因為媒體控制項通常都會預期擁有由左至右的配置，與非雙向語言相似。

![由左至右的媒體控制項](images/56291_BIDI_1415_music_player_layouts_left-withcallouts.png)

![由右至左的媒體控制項](images/56291_BIDI_1415_music_player_layouts_right-withcallouts.png)

### <a name="establishing-ui-directionality"></a>建立 UI 方向性

保留由右至左的 UI 流程對雙向市場中的一致設計來說是很重要的。 在此內容中新增由左至右流程的元素相當困難，因為有些巡覽式元素 (例如返回按鈕) 可能會與音訊控制項中的返回按鈕方向產生矛盾。

![音樂應用程式曲目頁面](images/56292_BIDI_16_app_layout_callouts_resized.png)

這個音樂應用程式保留了由右至左的格線。 這可讓已經在 Windows UI 中使用此方向巡覽的使用者對此應用程式產生非常自然的感覺。 流程乃是藉由確保主要元素不僅只是方向為由右至左，同時還要能適當對齊區段的標頭，以協助維持 UI 流程保留的。

![音樂應用程式專輯頁面](images/56292_BIDI_17_app_layout_callouts_resized.png)

### <a name="text-handling"></a>文字處理

上方螢幕擷取畫面中演出者的簡歷是靠左對齊的，其他與演出者相關的文字，例如專輯和曲目名稱都維持在靠右對齊。 簡歷欄位是一個相當大的文字元素，其在靠右對齊時閱讀效果相當差，因為在閱讀較寬的文字區塊時很難分辨每一行文字。 一般而言，任何大於兩行或三行，且每一行包含五個或更多單字的文字元素都應考慮採用相同的例外對齊方式，即文字區塊的對齊方式與整體應用程式的方向配置相反。

在應用程式中操控對齊看起來似乎很簡單，但其實通常會公開轉譯引擎在雙向字串中配置中性字元時所具備的邊界和限制。 例如，下列字串在不同的對齊方式下會有不同的顯示效果。

| | 英文字串 (LTR) | 希伯來文字串 (RTL) |
| -------------- | ------------------- | ------------------- |
| **靠左對齊** | Hello, World! | בוקר טוב! |
| **靠右對齊** | !Hello, World | !בוקר טוב |

若要確保演出者的資訊在整個音樂應用程式中都能正確顯示，開發團隊會將文字配置屬性與對齊方式分開。 換句話說，演出者資訊在許多案例下會以靠右對齊的方式顯示，但字串配置調整是根據自訂背景處理設定的。 背景處理會根據字串的內容判斷最佳方向配置設定。

![音樂應用程式演出者頁面](images/56292_BIDI_18_app_layout_callouts_resized.png)

例如，在沒有自訂字串配置處理的情況下，演出者的名稱 "The Contoso Band." 會顯示為 ".The Contoso Band"。

### <a name="specialized-string-direction-preprocessing"></a>特殊字串方向前置處理

當應用程式連絡伺服器取得媒體中繼資料時，它會在向使用者顯示前先前置處理每個字串。 在這個前置處理的過程中，應用程式也會執行轉換，使文字方向保持一致。 為執行此作業，它會檢查在字串的結尾是否有中性字元。 此外，若字串的文字方向與 Windows 語言設定設定的應用程式方向相反，它便會附加 (有時候會在開頭加上) Unicode 方向標記。 轉換功能看起來就像這樣。

```csharp
string NormalizeTextDirection(string data) 
{
    if (data.Length > 0) {
        var lastCharacterDirection = DetectCharacterDirection(data[data.Length - 1]);

        // If the last character has strong directionality (direction is not null), then the text direction for the string is already consistent.
        if (!lastCharacterDirection) {
            // If the last character has no directionality (neutral character, direction is null), then we may need to add a direction marker to
            // ensure that the last character doesn't inherit directionality from the outside context.
            var appTextDirection = GetAppTextDirection(); // checks the <html> element's "dir" attribute.
            var dataTextDirection = DetectStringDirection(data); // Run through the string until a non-neutral character is encountered,
                                                                 // which determines the text direction.

            if (appTextDirection != dataTextDirection) {
                // Add a direction marker only if the data text runs opposite to the directionality of the app as a whole,
                // which would cause the neutral characters at the ends to flip.
                var directionMarkerCharacter =
                    dataTextDirection == TextDirections.RightToLeft ?
                        UnicodeDirectionMarkers.RightToLeftDirectionMarker : // "\u200F"
                        UnicodeDirectionMarkers.LeftToRightDirectionMarker; // "\u200E"

                data += directionMarkerCharacter;

                // Prepend the direction marker if the data text begins with a neutral character.
                var firstCharacterDirection = DetectCharacterDirection(data[0]);
                if (!firstCharacterDirection) {
                    data = directionMarkerCharacter + data;
                }
            }
        }
    }

    return data;
}
```

新增的 Unicode 字元寬度為零，因此不會影響字串的間距。 此程式碼可能會潛在性的降低效能，因為偵測字串的方向需要逐一檢查字串，直到遇到非中性字元為止。 每個檢查是否為中性的字元都會先和幾個 Unicode 範圍比較，因此其並非簡單的檢查。

## <a name="case-study-2-a-bidi-mail-app"></a>案例研究 #2：雙向郵件應用程式

### <a name="overview"></a>概觀

當論及 UI 配置需求時，郵件用戶端的設計便相當簡單。 Windows 中的「郵件」應用程式根據預設便是鏡像的。 從文字處理角度而言，郵件應用程式必須具備更多強固的文字顯示和撰寫功能，才能容納混合文字的情節。

### <a name="establishing-ui-directionality"></a>建立 UI 方向性

「郵件」應用程式的 UI 配置是鏡像的。 三個窗格都已重新調整方向，使資料夾窗格位於畫面的右側邊緣，其左側為郵件項目清單窗格，最後則是電子郵件撰寫窗格。

![鏡像的郵件應用程式](images/56293_BIDI_19_icon_realignment_cropped_resized.png)

其他項目也已重新調整方向，以符合整體 UI 流程及觸控最佳化。 這些包含應用程式列及撰寫、回覆和刪除圖示。

![鏡像的郵件應用程式與應用程式列](images/56294_BIDI_20_email_orientation_email_resized.png)

### <a name="text-handling"></a>文字處理

#### <a name="ui"></a>UI

UI 上的文字對齊通常都是靠右對齊。 這包括資料夾窗格和項目窗格。 項目窗格限制在兩行文字 (地址和標題)。 這對維持由右至左對齊方式是很重要的，無須引進當內容方向與 UI 方向流程相反時便難以閱讀的文字區塊。

#### <a name="text-editing"></a>文字編輯

文字編輯需要支援由右至左及由左至右的撰寫。 此外，撰寫配置必須要能藉由使用可儲存方向資訊的格式&mdash;例如 RTF 文字&mdash;進行保留。

![由左至右的郵件應用程式](images/56294_BIDI_21_email_orientation_LtR_resized.png)

![由右至左的郵件應用程式](images/56294_BIDI_22_email_orientation_RtL_resized.png)
