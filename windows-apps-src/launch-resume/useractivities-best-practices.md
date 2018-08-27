---
author: PatrickFarley
title: 使用者活動的最佳作法
description: 本指南概要說明建立和更新使用者活動的建議的作法。
keywords: user activity, user activities, timeline, cortana pick up where you left off, cortana pick up where i left off, project rome, 使用者活動, 時間軸, cortana 從先前離開的地方開始, cortana 接續未完成的部分, project rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "2854981"
---
# <a name="user-activities-best-practices"></a>使用者活動的最佳作法

本指南概要說明建立和更新使用者活動的建議的作法。 如需 windows 使用者活動功能的概觀，請參閱 ＜[繼續使用者活動，甚至是不同裝置](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities)。 或者，請參閱 Project 羅馬的[使用者活動] 區段中](https://docs.microsoft.com/windows/project-rome/user-activities/)的其他開發平台上實作的活動。

## <a name="when-to-create-or-update-user-activities"></a>建立或更新使用者活動的時機

因為每個應用程式是不同，它是每個開發人員能夠判斷要對應至使用者活動的應用程式中的動作的最佳方式。 使用者活動會貿易 Cortana 和時間表，以協助他們得到他們在過去瀏覽過的內容著重於增加時的使用者產能和效率。

**一般指導方針**

* **記錄一組相關的使用者動作的單一的活動。** 這是特別相關的等候音樂播放清單或 TV 顯示： 單一活動可進行更新以反映使用者的進度固定時間間隔。 在此例中，您必須跨多個天或數週代表期間或參與的多個歷程記錄項目與單一使用者活動。 相同套用至使用者在其進行逐步進行您的應用程式內的文件型的活動。
* **儲存在雲端中的使用者資料。** 如果您想要支援跨裝置活動，您需要確定才能重新參與此活動的內容會儲存至雲端的位置。 裝置專用活動會出現在時間表上的裝置上建立活動時所在但可能不會出現在其他裝置。
* **請勿建立活動的使用者不需要以繼續執行的動作。** 如果您的應用程式用來完成不保留狀態的簡單、 以一次性作業，可能不需要建立使用者活動。
* **請勿建立活動由其他使用者完成動作。** 如果外部帳戶將使用者傳送郵件或@-mentions它們在您的應用程式，您不應建立活動這。 這種類型的動作更妥善地都由動作中心通知。
  * 共同作業案例會產生例外狀況： 當多位使用者正在處理相同活動一起 （例如 Word 文件），將會是在其中另一位使用者具有之後您的使用者進行變更的情況。 在此例中，您可能會想要更新以反映至文件所做的變更現有的活動。 這會涉及更新現有的使用者活動內容資料不會建立新的歷程記錄項目。

**特定類型的應用程式的指導方針**

其他每個應用程式時，大部分的應用程式將會分成下列互動模式。
* **文件為主的應用程式**-建立一個活動每份文件，以反映出期間使用的一或多個記錄項目。 請務必所做的變更會在文件更新您的活動。
* **遊樂場**— 建立的每個遊戲儲存] 或 [world 一個活動。 如果您的遊戲支援層級的單一順序，您可以重新發佈相同活動一段時間，雖然您可能會想要更新的內容資料顯示最新的 [進度] 或 [成就。
* **公用程式的應用程式**-如果沒有在您的使用者需要保留並繼續執行的應用程式內，您不需要使用使用者活動。 良好的範例是計算器類似的簡單應用程式。
* **企業營運系統應用程式**-許多應用程式存在管理簡單的任務或工作流程。 建立一個透過您的應用程式存取每個不同的工作流程活動 （例如費用報告每是不同的活動，以便使用者然後按一下 [查看特定的報表已核准活動）。
* **媒體播放應用程式**-建立邏輯群組內容 （例如播放清單、 程式或獨立的內容） 的每一個活動。 應用程式開發人員的基礎問題是是否每一段內容 （TV 第、 歌曲） 算作獨立內容或集合的一部分。 依照通則，如果使用者不將加入播放集合或循序內容、 整個集合就是活動。 如果他們選擇播放一段單一內容，該一段內容是活動。 請參閱下面的更具體的指導方針。
  * **音樂： 內容類型專輯/藝人**— 如果使用者選取專輯、 人員、 或內容類型及拜訪人次**播放**，該集合是活動;不要撰寫每個歌曲個別的活動。 單一專輯類似的簡短集合或正在回播放隨機的順序的集合，您可能不需要更新以反映使用者的目前位置的活動。 例如專輯] 或 [播放清單的長循序播放，錄製您專輯內的位置可能意義。
  * **等候音樂： 智慧播放清單**— 隨機順序中播放音樂的應用程式應該記錄的播放清單的單一活動。 如果使用者播放播放清單的第二個的時間，您可以建立額外的歷程記錄的相同的活動。 錄製播放清單中的使用者的目前位置不需要因為隨機排序。
  * **TV 數列**— 如果您的應用程式設定為在下一個第播放目前的交談完成後，您應寫入 TV 數列的單一活動。 當您在各種尋找播放的影片播放跨多個檢視工作階段，您將會更新以反映目前的位置之數列中，您活動和會建立多個歷程記錄。
  * **影片**-影片單一內容的一部分，且應該有其專屬的歷程記錄。 如果使用者停止觀看這支影片組件-方式透過，最好是取捨記錄其位置。 當他們想要繼續執行它未來時、 活動無法繼續地方關閉、 影片或甚至是要求使用者是否他們想要繼續執行或開頭開始。

## <a name="user-activity-design"></a>使用者活動設計

使用者活動是由三個元件構成： 啟用 URI、 視覺資料及內容的中繼資料。
* 啟用 URI 是可以傳遞至應用程式或經驗才能繼續執行的特定內容的應用程式的 URI。 一般而言，這些連結採取的配置 (例如"my-app://page2?action=edit") 的通訊協定處理常式的表單。 它是最多個開發人員能夠判斷 URI 參數依據其應用程式的處理方式。 如需詳細資訊，請參閱[處理 URI 啟用](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)。
* 視覺資料、 組成一組必要及選用的屬性 (例如： 標題、 描述或調適型卡元素)，允許使用者以視覺方式識別活動。 請參閱下建立調適型卡片視覺效果您活動的指導方針。
* 內容的中繼資料是 JSON 資料可以用來分組和擷取特定的內容下的活動。 這通常採用的表單http://schema.org資料。 請參閱下面上填寫此資料的指導方針。

### <a name="adaptive-card-design-guidelines"></a>調適型的名片設計方針

當活動會出現在時間表時，他們會顯示使用[調適型卡片架構](https://docs.microsoft.com/adaptive-cards/)。 如果開發人員沒有提供每個活動調適型卡片、 時間表會自動建立根據應用程式名稱/圖示、 所需的標題] 欄位中及選擇性描述] 欄位中的簡單卡片。 

應用程式開發人員的建議來提供自訂卡使用簡單調適型卡片 JSON 結構描述。 請參閱技術的指示的[調適型卡文件](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started)如何建構調適型卡片物件。 請參閱下列設計調適型卡在使用者活動的準則。
* 使用圖像
  * 請盡可能使用每一項活動，唯一的圖像。 您的應用程式名稱和圖示會自動顯示旁活動的卡片;其他圖像可協助使用者尋找所要尋找的活動。
  * 圖像不應該包含文字讀取預期使用者。 此文字不是供使用者使用協助工具需求和無法搜尋。
  * 如果圖像不包含文字，並可以裁剪成有關 2:1 的比例，您應該將其作為背景影像。 這會產生粗體活動卡片以將突出時間表中。 圖像將會是調暗稍微以確定文字會保持在卡片顯示且建議僅使用活動名稱在此例中為較小的文字會變得難以閱讀。
  * 如果映像無法裁剪成 2:1，您應該將它放活動卡片中。  
    * 如果長寬比為正方形或直向、 錨定無邊界卡片右側圖像。
    * 如果長寬比為橫向、 錨定右上角的名片圖像。
* 每個活動才可提供活動名稱] 中，應一律顯示。
  * 這個名稱應該顯示卡片使用大型粗體文字] 選項的左上角。 很重要的名稱是易於辨識這就只有部分使用者會看到 Cortana 案例時顯示活動。 在時間表中顯示相同的名稱會瀏覽大量的活動的使用者更容易。
* 使用相同的視覺樣式所有從您的應用程式的活動，讓使用者可以輕鬆找到您的應用程式活動的時間表中。
  * 例如，活動應全部使用相同的背景色彩。
* 謹慎使用補充文字資訊。 
  * 避免填入名片的文字，並只能使用補充資訊可協助使用者尋找右活動或反映狀態資訊 （例如在特定工作的目前進度）。

### <a name="content-metadata-guidelines"></a>內容中繼資料的指導方針

使用者活動也可包含 Windows 和 Cortana 用來分類 （英文） 活動及產生推斷的內容中繼資料。 （如果使用者跨不同的應用程式和網站購物特定產品） 活動可然後分組周圍特定主題，例如位置 （如果使用者新聞稿休假）、 物件 （如果使用者新聞稿某個項目） 或巨集指令。 它是不錯的選項來表示名詞和活動的相關動詞。 

在下列範例中，內容的中繼資料 JSON、 追蹤的[Schema.org](https://schema.org/)、 標準代表分析藍本:"John 會播放生氣鳥類與 Steve"。

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>重要 API

* [UserActivities 命名空間](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>相關主題

* [使用者活動 （Project 羅馬文件）](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [調適型卡片](https://docs.microsoft.com/adaptive-cards/)
* [調適型卡片視覺化工具範例](http://adaptivecards.io/)
* [處理 URI 啟用](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [使用 Microsoft Graph、活動摘要及調適型卡片在任何平台上與您的客戶互動](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
