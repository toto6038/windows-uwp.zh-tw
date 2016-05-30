---
author: Jwmsft
Description: 進度控制項為使用者提供回饋，告知正在進行長時間執行的操作。
title: 進度控制項的指導方針
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
---
# 進度控制項

進度控制項為使用者提供回饋，告知正在進行長時間執行的操作。 「確定的」**進度列會顯示操作的完成百分比。 「不確定的」**進度列或進度環會顯示操作正在進行。

進度控制項是唯讀的，無法互動。

<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**ProgressBar 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.aspx)
-   [**IsIndeterminate 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)
-   [**ProgressRing 類別**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.aspx)
-   [**IsActive 屬性**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx)

![Windows 應用程式：不確定的進度列、進度環、確定的進度列](images/ProgressBar.png)

Windows 應用程式：不確定的進度列、進度環、確定的進度列

![Windows Phone app：狀態列進度指示器和進度列](images/wp_progress_bar.png)

Windows Phone app：狀態列進度指示器和進度列

## 範例

這裡提供啟動顯示畫面上進度環控制項的範例。

![說明標準進度環控制項的螢幕擷取畫面](images/ProgressBar_Standard.png)

進度列也是適合狀態或位置的指示器。 用於音樂曲目的進度列會對應到歌曲的時間軸：列的值是歌曲的位置；暫停狀態表示目前已暫停播放。

![Xbox 音樂應用程式在播放歌曲時會顯示進度列](images/ProgressBar_MusicTimeline.png)

## 這是正確的控制項嗎？

應用程式不一定需要進度控制項。 當工作進度非常明顯，或工作快速完成時，出現進度控制項反而會讓人分心。 以下為決定是否顯示進度控制項時的幾個考慮重點。

-   **作業是否需要兩秒以上的時間才能完成？**

    如果是，作業一開始就顯示進度控制項。 如果大部分情況下作業需要兩秒以上的時間才能完成，但是有時候會在兩秒以內完成，請在顯示控制項前等待 500 毫秒以避免閃爍。

-   **作業需要等待使用者才能完成工作嗎？**

    如果是，不要使用進度列。 進度列是表示電腦的進度，而不是使用者的進度。

-   **使用者需要知道發生什麼事嗎？**

    例如，如果應用程式正在背景進行下載，而使用者並未起始下載，則使用者不需要知道這項下載作業。

-   **作業是不會阻擋使用者活動且使用者不感興趣 (但仍有一些興趣) 的背景活動嗎？**

    當應用程式執行不用一直看到的工作但仍然需要顯示狀態時，使用文字和省略符號。

    ![以文字當作進度指示器的範例](images/textprogress.png)

    使用省略符號指示工作正在進行。 如果有多個工作或項目，您可以指示剩下工作的數目。 當所有的工作完成後，關閉指示器。

-   **您可以使用作業的內容顯示視覺化進度嗎？**

    如果是，不要顯示進度控制項。 例如，顯示從磁碟載入的 /src/assets 時，/src/assets 會在載入時逐一顯示在螢幕上。 顯示進度控制項並沒有好處，只會讓 UI 顯得雜亂。

-   **您可以相對地判斷操作正在進行時整個工作的完成百分比嗎？**

    如果可以，請使用確定的進度列，特別是針對會阻止使用者的操作。 否則請使用不確定的進度列或進度環。 即使所有使用者都知道有作業正在執行，但還是有所幫助。

## 建立確定的進度列

確定的進度列會顯示應用程式已完成的進度。 工作進行時，進度列會漸漸填滿。 如果可以用時間、位元組、檔案或者其他可以量化的測量單位來預估剩餘的工作量，請使用確定的進度列。

進度列提供數個屬性用於設定和判斷進度：
- [
            **IsIndeterminate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx)：指定是否為不確定的進度列。 設定為 **false** 以建立確定的進度列。
- [
            **Minimum**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.minimum.aspx)︰值範圍的開始值。 預設為 0.0。
- [
            **Maximum**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.maximum.aspx)︰值範圍的結束值。 預設為 1.0。 
- [
            **Value**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.rangebase.value.aspx)：指定目前進度的數字。 如果顯示的是檔案下載進度，這個值可能是已下載的位元組數目 (然後，將 Maximum 設為要下載的位元組總數)。
 
下列範例顯示以值為基礎的確定進度列。 

```xaml
<ProgressBar IsIndeterminate="False" Maximum="100" Width="200"/>
```

```csharp
ProgressBar progressBar1 = new ProgressBar();
progressBar1.IsIndeterminate = false;
progressBar1.Maximum = 100;
progressBar1.Width = 200;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressBar1);
```

您通常不會在標記中指定進度列的值。 您會改用程序性程式碼或資料繫結來更新進度列的值，做為某種進度指示器的回應。 例如，如果進度列指出已下載多少檔案，則您會在每次下載另一個檔案時更新這個值。

## 建立不確定的進度控制項

當您無法估計還剩下多少未完成的工作，而且工作不會封鎖使用者的互動時，可使用不確定的進度列或進度環。 不確定的進度列不會在進度完成時顯示漸漸填滿，而是顯示點由左往右移動的動畫。 不確定的進度環會以圓圈顯示一連串移動的點。 

若要使進度列成為不確定的進度列，請將其 [**IsIndeterminate**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressbar.isindeterminate.aspx) 屬性設定為 **true**。

```xaml
<ProgressBar IsIndeterminate="True" Width="200"/>
```

```csharp
ProgressBar progressBar1 = new ProgressBar();
progressBar1.IsIndeterminate = true;
progressBar1.Width = 200;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressBar1);
```

若要在您的 app 中顯示進度環，請將其 [**IsActive**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.progressring.isactive.aspx) 屬性設定為 **true**。

```xaml
<ProgressRing IsActive="True"/>
```

```csharp
ProgressRing progressRing1 = new ProgressRing();
progressRing1.IsActive = true;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(progressRing1);
```

## 建議

-   當工作確定，也就是有明確定義的持續時間或可預測的結束時間，適合使用確定的進度列。 例如，如果可以用時間、位元組、檔案或者其他可以量化的測量單位來預估剩餘的工作量，請使用確定的進度列。 以下是確定的工作範例：

    -   應用程式正在下載 500k 的相片，目前已接收 100k。
    -   應用程式正在播放 15 秒的廣告，已經過 2 秒。

    ![確定的進度列範例](images/progress_determinate_bar.png)

-   對於不是確定的且強制回應 (阻止使用者互動) 的工作使用不確定的進度環。

    ![進度環範例](images/progress_ring.png)

-   對於不是確定的且非強制回應 (不會阻止使用者互動) 的工作使用不確定的進度列。

    ![不確定的進度列範例](images/progress_indeterminate_bar.png)

-   如果強制回應狀態持續不到 2 秒，請將部分強制回應的工作視為非強制回應。 部分工作會阻止互動，直到完成某些進度以後，使用者才能再次與應用程式互動。 例如，當使用者執行搜尋查詢時，必須等到顯示第一個結果後才能開始互動。 如果強制回應狀態持續不超過 2 秒，請將這類工作視為非強制回應，並且使用不確定的進度列樣式。 如果強制回應狀態會持續超過 2 秒，請在工作的強制回應階段中使用不確定的進度環，然後在非強制回應階段中使用不確定的進度列。
-   請考慮提供取消或暫停進行中操作的方法，特別是使用者被阻止互動而要等待作業完成時，讓使用者清楚了解作業還需要多少時間執行。
-   不要使用「等待游標」來指示活動，因為使用觸控與系統互動的使用者看不到「等待游標」，而且使用滑鼠的使用者不需要以兩種方式來視覺化活動 (游標和進度控制項)。
-   為多個使用中相關工作顯示單一進度控制項。 如果螢幕上的多個相關項目都同時執行某種活動，不要顯示多個進度控制項。 相反地，請顯示一個在最後一項工作完成時也結束的進度控制項。 例如，如果應用程式下載多張相片，請顯示一個進度控制項，而不是每張相片顯示一個進度控制項。
-   請不要在工作執行時變更進度控制項的位置或大小。

### 確定的工作的指導方針

-   如果是強制回應的作業 (阻止使用者互動) 且花費時間超過 10 秒，請提供取消作業的方式。 作業開始時就應該提供可以取消的選項。
-   平均分配進度更新。 避免發生進度增加到超過 80% 後停止一段長時間的狀況。 您希望進度完成時加速，而不是減慢。 避免極度跳躍，例如從 0% 跳躍到 90%。
-   將進度設定為 100% 後，等待到確定的進度列完成動畫後才將它隱藏。
-   如果您的工作由於使用者或外在條件而停止，但是使用者可以繼續，請以視覺方式指示進度已暫停。 在 JavaScript 應用程式中，使用 win-paused CSS 樣式執行這個動作。 而在 C\#/C++/VB 應用程式中，則是將 ShowPaused 屬性設定為 true 來執行。 在進度列下方提供狀態文字，告知使用者目前的情況。
-   如果工作停止而且不能繼續或必須從頭開始，請以視覺方式指示出現錯誤。 在 JavaScript 應用程式中，使用 win-error CSS 樣式執行這個動作。 而在 C\#/C++/VB 應用程式中，則是將 ShowError 屬性設定為 true 來執行。 以訊息取代狀態文字 (在進度列下方)，告知使用者發生的狀況以及修正問題的方法 (如果有的話)。
-   如果可以提供確定的進度前需要一些時間 (或動作)，請先使用不確定的進度列，然後切換到確定的進度列。 例如，如果下載工作的第一步是連線伺服器，則無法預估需要多長時間。 連線建立後，再切換為確定的進度列以顯示下載進度。 切換後將進度列保持在完全相同的位置以及相同的大小。

    ![從不確定的進度列變更為確定的進度列](images/progress_changing.png)

-   如果您有一個項目清單 (如印表機清單)，而特定動作可以在該清單中的項目啟動一項作業 (例如為其中一個印表機安裝驅動程式)，請在項目旁邊顯示確定的進度列。

    在進度列上方顯示工作的主題 (標籤) 並在下方顯示狀態。 如果執行的動作很明確，不用提供狀態文字。 在工作完成後，隱藏進度列。 使用狀態文字溝通項目的新狀態。

    ![顯示內嵌狀態的狀態](images/progress_multiplebars.png)

-   若要顯示工作清單，請對齊格線中的內容，讓使用者可以一眼就看清楚狀態。 顯示所有項目的進度列，包括擱置的項目。

    因為此清單的目的是顯示正在進行的作業，所以在作業完成時從清單移除作業。

    ![顯示多個進度列](images/progress_bar_multiple.png)

-   如果使用者從應用程式列起始工作，而工作阻止使用者互動，請在應用程式列中顯示進度控制項。

    如果進度列顯示進度的目的很清楚，您可以將進度列對齊應用程式列的頂端，並且省略標籤和狀態，否則請提供標籤和狀態文字。

    停用應用程式列中的控制項並忽略內容區域中的輸入，在工作期間停用互動。

-   不要遞減進度。 一律遞增進度值。 如果需要回復動作，請顯示回復進度，如同顯示任何其他動作進度一樣的方式。
-   不要重新開始進度 (從 100% 到 0%)，除非使用者明顯看到目前的階段或工作不是最後一個。 例如，假設工作包含兩個部分：下載一些資料，然後進行處理及顯示資料。 下載完成後，將進度列重設為 0% 並開始顯示資料的處理進度。 如果使用者不清楚工作中有多個步驟，請將工作摺疊成一個 0-100% 的進度比例，並在您從一個工作移至下一個時更新狀態文字。

### 使用進度環的強制回應、不確定工作的指導方針

-   在動作內容中顯示進度環：在使用者起始動作的位置或是將要顯示結果資料的位置附近顯示進度環。
-   在進度環的右邊提供狀態文字。
-   讓進度環使用與狀態文字一樣的色彩。
-   停用使用者不應在工作執行時與其互動的控制項。
-   如果工作出現錯誤，請隱藏進度指示器和狀態文字，並在其位置顯示錯誤訊息。
-   在對話方塊中，如果有一個操作必須在移至下一個畫面前完成，請將進度環放置在按鈕區域的正上方，靠左對齊對話方塊的內容。

    ![對話方塊中的進度](images/prog_ring_dialog.png)

-   在控制項靠右對齊的應用程式視窗中，將進度環放在造成動作執行的控制項左側或正上方。 將進度環靠左對齊相關內容。

    ![在應用程式視窗中顯示進度，控制項靠右對齊](images/prog_right_aligned_controls.png)

-   在控制項靠左對齊的應用程式視窗中，將進度環放在造成動作執行的控制項右側或正下方。

    ![控制項靠左對齊的進度環](images/prog_left_aligned_1.png)

    ![控制項靠左對齊下方的進度環](images/prog_left_aligned_2.png)

-   如果顯示多個項目，將進度環和狀態文字放置在項目標題的下方。 如果發生錯誤，請用錯誤文字取代進度環和狀態文字。

    ![多個項目清單中的進度環](images/prog_ring_multiple.png)

### 使用進度列的非強制回應、不確定工作的指導方針

-   如果在飛出視窗中顯示進度，在飛出視窗頂端放置不確定的進度列並設定其寬度，讓它橫跨整個飛出視窗。 這種放置方式不會分散注意力但仍能顯示正在進行的活動。 不要為飛出視窗指定標題，因為標題讓您無法將進度列放置在飛出視窗的頂端。

    ![飛出視窗中的不確定進度列](images/prog_flyout_indeterminate_bar.png)

-   如果在應用程式視窗中顯示進度，將不確定的進度列放置在應用程式視窗頂端，使其橫跨整個視窗。

    ![位於應用程式視窗頂端的進度列](images/prog_indeterminate_bar_app_window.png)

### 狀態文字的指導方針

-   當您使用確定的進度列時，請不要在狀態文字中顯示進度百分比。 控制項已經提供該資訊。
-   如果您使用文字而不是進度控制項來表示活動，請使用省略符號傳達活動正在進行。
-   如果您使用進度控制項，則不要在狀態文字中使用省略符號，因為進度控制項已經表示作業正在進行。

### 外觀和配置的指導方針

-   確定的進度列顯示為有色彩的列，漸漸填滿灰色背景列。 上色的總長度部分相對表示已完成多少作業。
-   不確定的進度列或進度環是由持續移動的色彩點組成。
-   根據進度控制項的重要性選擇它的位置和顯著性質。

    重要的進度控制項可以具有「採取行動」的作用，告知使用者在系統完成工作之後繼續特定作業。 某些內建的 Windows Phone 應用程式會針對重要情況在畫面頂端使用狀態列進度指示器。 您也可以這樣做，並將它設定為確定的或不確定的。

    對於較不重要的情況 (如下載) 則顯示較小的指示器，並限制在一個檢視。

-   使用標籤來顯示進度值，或描述程序正在進行，或指示作業已被中斷。 標籤是選擇性的，但是強烈建議您使用。

    若要描述程序正在進行，請使用進行式，例如，「連線中」、「下載中」或「傳送中」。

    若要指示進度已暫停或發生例外狀況，請使用過去式，例如，「已暫停」、「下載已失敗」或「已取消」。

-   包含標籤和狀態的確定的進度列

    ![包含標籤和狀態資訊的確定的進度列](images/progress_bar_determinate_redline.png)

-   多個進度列

    ![多個進度列的建議配置](images/progress_bar_multi_redline.png)

-   包含狀態文字的不確定的進度環

    ![包含狀態文字之不確定的進度環的配置](images/progress_ring_status_text.png)

-   不確定的進度列

    ![不確定的進度列的配置](images/progress_indeterminate_bar_redline.png)

## 其他用法指導方針

### 選擇進度樣式的決策樹狀結構

-   **使用者需要知道發生什麼事嗎？**

    如果回答為否，不要顯示進度控制項。

-   **是否要顯示有關完成作業所需花費之時間的資訊？**
    -   **是：****工作是否需要兩秒以上的時間才能完成？**
        -   **是：**使用確定的進度列。 對於需要 10 秒以上的時間才能完成的作業，提供取消作業的方法。
        -   **否：**不要顯示進度控制項。

    -   **否：****是否會封鎖使用者與 UI 的互動，直到作業完成為止？**
        -   **是：****這個作業是否為多個步驟程序的一部分，使用者需要知道有關該作業的特定詳細資料？**
            -   **是：**使用不確定的進度環，並在畫面水平位置中央放置狀態文字。
            -   **否：**使用不確定的進度環，但不要在畫面中央放置文字。
        -   **否：****這是否為主要活動？**
            -   **是：****這個進度是否與 UI 中單一特定的元素有關？**
                -   **是：**使用內嵌之不確定的進度環，並在相關的 UI 元素旁邊放置狀態文字。
                -   **否：****是否會在清單中載入大量的資料？**
                    -   **是：**使用上方的不確定進度列以及預留位置來代表傳入內容。
                    -   **否：**使用在畫面或表面上方的不確定進度列。
            -   **否：**使用畫面上方角落的狀態文字。

## 相關文章


- [**ProgressBar 類別**](https://msdn.microsoft.com/library/windows/apps/br227529)
- [**ProgressRing 類別**](https://msdn.microsoft.com/library/windows/apps/br227538)

**適用於開發人員 (XAML)**
- [新增進度控制項](https://msdn.microsoft.com/library/windows/apps/xaml/hh780651)
- [如何為 Windows Phone 建立自訂不確定的進度列](http://go.microsoft.com/fwlink/p/?LinkID=392426)


<!--HONumber=May16_HO2-->


