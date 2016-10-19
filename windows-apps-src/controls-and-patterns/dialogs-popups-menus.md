---
author: Jwmsft
redirect_url: https://msdn.microsoft.com/windows/uwp/controls-and-patterns/dialogs
Description: "飛出視窗是輕量型的快顯視窗，用來暫時顯示與使用者目前正在執行的操作相關的 UI。"
title: "操作功能表和對話方塊"
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 6572acefa25e464b6edaca9fee5b2b3e3b46ff3f

---
# 功能表、對話方塊、飛出視窗和快顯

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

當使用者要求 UI 元素或當發生某個狀況需要通知或核准時，功能表、對話方塊、飛出視窗和快顯會顯示暫時性 UI 元素。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn299030">MenuFlyout 類別</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn279496">Flyout 類別</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx">ContentDialog 類別</a></li>
</ul>

</div>
</div>






操作功能表提供使用者能立即執行的動作。 您可以在其中填入文字命令。 只要點選或按一下功能表外的地方，就可以關閉操作功能表。

對話方塊是提供內容相關應用程式資訊的強制回應 UI 重疊項目。 對話方塊會阻擋與應用程式視窗的互動，直到對話方塊確實關閉為止， 而且通常需要使用者執行某種類型的動作來關閉對話方塊。

飛出視窗是一種輕量型的內容相關快顯視窗，可顯示與使用者的動作相關的 UI。 它包含放置和調整大小邏輯，可以用來顯示隱藏的控制項、顯示項目的詳細資料，或是要求使用者確認動作。 只要點選或按一下快顯視窗外的地方，就可以關閉飛出視窗。


## 這是正確的控制項嗎？

操作功能表可用來：

-   內容相關動作。
-   必須對其執行動作，但無法選取之物件的命令。

對話方塊可用來：

- 表示使用者必須先閱讀並確認才能繼續執行工作的重要資訊。
- 要求使用者執行清除動作，或傳達使用者應該確認的重要訊息。 範例包含：
  - 當使用者的安全性可能受到破壞時
  - 當使用者將要永久修改重要資產時
  - 當使用者將要刪除重要資產時
  - 確認 App 內購買
- 套用到整個應用程式內容的錯誤訊息，例如連線錯誤。
- 問題，當應用程式需要詢問使用者關於造成應用程式無法繼續執行的問題時 (例如當應用程式無法代替使用者選擇時)。 會造成應用程式無法繼續執行的問題，且該問題不能忽略或延遲回應，並應該向使用者提供明確定義的選擇。

飛出視窗可以使用於：

-   內容相關的暫時性 UI。
-   警告和確認，包含與可能具有破壞性的動作相關的警告和確認。
-   顯示詳細資訊，例如頁面上項目的相關詳細資料或較長的說明。


## 範例

以下是具有簡易命令簡要清單的一般單一窗格操作功能表。 需要時可捲動瀏覽。 請視需要使用分隔字元群組類似的命令。

![一般操作功能表的範例](images/controls_contextmenu_singlepane.png)

階層式的操作功能表可用於更廣泛的命令集合。 其提供多層的飛出視窗，而且可以捲動。 請視需要使用分隔字元群組類似的命令。

![階層式操作功能表的範例](images/controls_contextmenu_cascading.png)

這是全螢幕單一按鈕確認對話方塊的範例。 這類對話方塊會向使用者顯示相當多希望使用者閱讀的資訊，使用者必須按下按鈕才能繼續操作。

![全螢幕單一對按鈕對話方塊的範例。](images/controls_dialog_singlebutton.png)

此範例是雙按鈕對話方塊，向使用者顯示 A/B 選擇。 通常此對話方塊中顯示的資訊量較少。

![全按鈕對話方塊的範例](images/controls_dialog_twobutton.png)

## 強制回應與消失關閉

對話方塊是強制回應，這表示其會封鎖與 app 的所有互動，直到使用者選取對話方塊按鈕為止。 為了以視覺化方式強化其強制回應行為，對話方塊會繪製重疊層其會部分遮蔽暫時無法存取的應用程式 UI。

**注意** 取消其中一個可用的對話方塊選項時，app 可選擇讓使用者按下 Esc 鍵來關閉對話方塊。 此行為並未內建至控制項，但為通用實作的捷徑。

飛出視窗和操作功能表為消失關閉控制項，這表示使用者可以選擇透過各種不同的動作，快速關閉暫時性 UI。 這些互動的用意在於輕量，並非封鎖。 消失關閉的動作包括
- 按一下或點選暫時性 UI 的外部位置
- 按下 Esc 鍵
- 按下 [返回] 按鈕
- 調整應用程式視窗大小。
- 變更裝置方向


## 對話方塊使用指導方針

-   在對話方塊文字的第一行中明確識別問題或使用者的目標。
-   對話方塊標題是主要指示，而且是選擇性的。
    -   使用簡短標題來說明使用者需使用對話方塊執行什麼動作。 太長的標題不會斷行，且會被截斷。
    -   如果您使用對話方塊來傳達簡單的訊息、錯誤或問題，可以選擇性地省略標題。 只需要內容文字傳達核心資訊即可。
    -   確定標題與按鈕選項直接相關。
-   對話方塊內容包含描述性文字，而且是必要的。
    -   盡可能簡要地呈現訊息、錯誤或造成應用程式無法繼續執行的問題。
    -   使用對話方塊標題時，請利用內容區域提供更多詳細資料或定義詞彙。 不要以只有些微差異的用字重複標題。
-   至少必須顯示一個對話方塊按鈕。
    -   按鈕都是唯一的機制，可讓使用者關閉對話方塊。
    -   將按鈕與可識別針對主要指示或內容做出明確回應的文字搭配使用。 例如，「是否允許 AppName 存取您的位置?」，後面接 [允許] 和 [封鎖] 按鈕。 明確的回應讓人更快速理解，可以更有效率地做出決定。
-   錯誤對話方塊會在對話方塊中顯示錯誤訊息，以及任何相關資訊。 錯誤對話方塊中應該只使用 [關閉] 按鈕或執行類似動作的按鈕。
-   針對頁面上特定位置的內容相關錯誤，例如 (密碼欄位中的) 驗證錯誤，請使用 App 本身的畫布顯示內嵌錯誤，而不要使用對話方塊。

## 飛出視窗

飛出視窗為開放式容器，可顯示任意 UI 做為其內容。  飛出視窗並無專屬視覺化部分，其純粹只是一個內容控制項。 飛出視窗具有邊界和選用捲軸，其可新增至內容周圍。 若要設定飛出視窗的樣式，請修改其 [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx)。

下列程式碼會顯示一段文字換行，並讓螢幕助讀程式能夠存取文字區塊。

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

### 叫用和位置

飛出視窗和操作功能表會附加至特定的控制項。 顯示這些項目時應將它錨定至叫用的物件，並指定其慣用的相對物件位置：Top、Left、Bottom 或 Right。 飛出視窗亦具備「Full」位置模式，並會嘗試延展飛出視窗並將它置於 App 視窗的中央。

[Button 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)包含 [**Flyout**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 屬性，可讓您指定當使用者按一下或點選按鈕時開啟的暫時性 UI。

````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="Yay!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````


## 相關文章

**適用於開發人員**
- [**MenuFlyout 類別**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Flyout 類別**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Aug16_HO3-->


