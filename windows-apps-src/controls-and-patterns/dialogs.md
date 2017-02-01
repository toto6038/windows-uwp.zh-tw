---
author: mijacobs
Description: "對話方塊和飛出視窗會在使用者要求暫時性 UI 元素，或發生需要通知或核准的情況時顯示暫時性 UI 元素。"
title: "對話方塊和飛出視窗"
label: Dialogs
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: b258771c887d4422433522344b11130b7e9ed1e6
ms.openlocfilehash: bc428b42324cd584dfaee1db3c9eb834d30cd69d

---
# <a name="dialogs-and-flyouts"></a>對話方塊和飛出視窗

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

對話方塊和飛出視窗為在發生需要來自使用者的通知、核准或是其他資訊的情況時，所顯示的暫時性 UI 元素。

<div class="important-apis" >
<b>重要 API</b><br/>
<ul>
<li>[ContentDialog 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)</li>
<li>[Flyout 類別](https://msdn.microsoft.com/library/windows/apps/dn279496)</li>
</ul>
</div>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>對話方塊</b> <br/><br/>
    ![對話方塊範例](images/dialogs/dialog-delete-file-example.png)</p>
<p>對話方塊是提供內容相關應用程式資訊的強制回應 UI 重疊項目。 對話方塊會阻擋與應用程式視窗的互動，直到對話方塊確實關閉為止， 而且它們通常會需要使用者執行某種動作。   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>飛出視窗</b> <br/><br/>
   ![飛出視窗的範例](images/flyout-example.png)</p>
<p>飛出視窗是一種輕量型的內容相關快顯視窗，會顯示與使用者動作相關的 UI。 它包含放置和調整大小邏輯，可以用來顯示隱藏的控制項、顯示項目的詳細資料，或是要求使用者確認動作。 
</p><p>與對話方塊不同的是，飛出視窗可以透過點選或按一下飛出視窗以外的地方、按 Esc 鍵或 [上一頁] 按鈕、重新調整 App 視窗的大小，或是變更裝置的方向，來快速將它關閉。
</p><br/>

  </div>
</div>
</div>

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

* 使用對話方塊和飛出視窗來通知使用者重要的資訊，或是在完成動作之前要求確認或其他資訊。 
* 不要使用飛出視窗來取代[工具提示](tooltips.md)或[操作功能表](menus.md)。 使用工具提示來顯示會在特定時間之後隱藏的簡短說明。 使用操作功能表來執行與 UI 元素相關聯的內容相關動作，例如複製和貼上。  


對話方塊和飛出視窗可確保使用者知曉重要的資訊，但它們也會干擾使用者體驗。 因為對話方塊為強制回應 (封鎖)，所以會打擾使用者，並防止他們繼續執行動作，直到他們與對話方塊互動為止。 飛出視窗能提供較不突兀的體驗，但是顯示太多的飛出視窗也可能會造成困擾。 

請考慮您想要分享資訊的重要性，它的重要程度是否有必要打擾使用者？ 也請考慮需顯示資訊的頻率。如果您每隔幾分鐘便需要顯示對話方塊或通知，您可以考慮將這項資訊配置在主要 UI 上。 例如，在聊天用戶端中，與其在有朋友登入時就顯示飛出視窗，您可以顯示當時在線上的朋友清單，並在有朋友登入時針對他們進行醒目提示。 

飛出視窗和對話方塊常用來在執行動作之前確認它們 (例如刪除檔案)。 如果您預期使用者會頻繁執行某項特定的動作，請考慮讓使用者能夠在錯誤地做出該動作時進行復原，而不是強制使用者每次都必須確認該動作。 



## <a name="dialogs-vs-flyouts"></a>對話方塊與飛出視窗

在您決定要使用對話方塊或飛出視窗之後，您必須選擇要使用哪一個。 

由於對話方塊會阻止互動，而飛出視窗不會，您應該將對話方塊用在想讓使用者放下手上的工作，並將注意力放到特定資訊或是回答問題的情況。 相反地，飛出視窗適合在您想要引起使用者注意，但即使他們想要忽略它也無妨的情況下使用。 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>適合使用對話方塊的情況...</b> <br/>
<ul>
<li>表示使用者「必須」先閱讀並確認才能繼續執行工作的重要資訊。 範例包含：
<ul>
  <li>當使用者的安全性可能受到破壞時</li>
  <li>當使用者將要永久修改重要資產時</li>
  <li>當使用者將要刪除重要資產時</li>
  <li>確認 App 內購買</li>
</ul>

</li>
<li>套用到整個應用程式內容的錯誤訊息，例如連線錯誤。</li>
<li>問題，當應用程式需要詢問使用者關於造成應用程式無法繼續執行的問題時 (例如當應用程式無法代替使用者選擇時)。 封鎖性問題不能忽略或延遲回應，因此應該向使用者提供明確定義的選擇。</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>適合使用飛出視窗的情況...</b> <br/>
<ul>
<li>用來在完成動作之前收集所需的其他資訊。</li>
<li>顯示僅於些情況下相關的資訊。 例如，在影像中心 App 中，當使用者按一下影像縮圖時，您可以使用飛出視窗顯示該影像的大尺寸版本。</li>
<li>警告和確認，包含與可能具有破壞性的動作相關的警告和確認。</li>
<li>顯示詳細資訊，例如頁面上項目的詳細資料或較長的描述。</li>
</ul></p>
  </div>
</div>
</div>

<div class="microsoft-internal-note">
消失關閉控制項會將鍵盤和遊戲台焦點困在暫時性 UI 內，直到關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的可見度變暗。 此行為可以使用新的 `LightDismissOverlayMode` 屬性進行修改。 根據預設，暫時性 UI 將在 Xbox 上繪製消失關閉重疊，但不會在其他裝置系列上繪製，不過應用程式可以選擇將重疊強制為一律 [開啟]**** 或一律 [關閉]****。

```xaml
<MenuFlyout LightDismissOverlayMode=\"Off\">
```
</div>

## <a name="dialogs"></a>對話方塊
### <a name="general-guidelines"></a>一般指導方針

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
    - 依下列順序呈現認可按鈕： 
        -   確定/[執行]/是
        -   [不執行]/否
        -   取消
        
        (其中，[執行] 和 [不執行] 是主要指令的特定回應。)
   
-   錯誤對話方塊會在對話方塊中顯示錯誤訊息，以及任何相關資訊。 錯誤對話方塊中應該只使用 [關閉] 按鈕或執行類似動作的按鈕。
-   針對頁面上特定位置的內容相關錯誤，例如 (密碼欄位中的) 驗證錯誤，請使用 App 本身的畫布顯示內嵌錯誤，而不要使用對話方塊。

### <a name="confirmation-dialogs-okcancel"></a>確認對話方塊 (確定/取消)
確認對話方塊可讓使用者確認要執行的動作。 使用者可以確認動作或選擇取消。  
典型的確認對話方塊有兩個按鈕︰確認 (確定) 按鈕和取消按鈕。  

<ul>
    <li>
        <p>一般而言，確認按鈕應放在左邊 (主要按鈕)，取消按鈕 (次要按鈕) 應放在右邊。</p>
         ![確定/取消對話方塊](images/dialogs/dialog-delete-file-example.png)
        
    </li>
    <li>如一般建議一節所述，將按鈕與可識別針對主要指示或內容做出明確回應的文字搭配使用。
    </li>
</ul>

> 某些平台將確認按鈕放在右邊，而非左邊。 那麼，為什麼建議將它放在左邊？  如果您假設大部分使用者習慣使用右手，而且是以右手拿手機，那麼確認按鈕位於左邊時實際上會更好按，因為它更有可能在使用者的拇指弧形內。 按鈕位於螢幕右邊時，使用者就必須將其拇指向內拉到較不舒服的位置。

### <a name="create-a-dialog"></a>建立對話方塊
若要建立對話方塊，請使用 [ContentDialog 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)。 您可以利用程式碼或標記建立對話方塊。 雖然在 XAML 中定義 UI 元素通常會比較容易，但針對簡單的對話方塊，單純使用程式碼會較為容易。 這個範例會建立一個對話方塊，以通知使用者沒有 WiFi 連線，然後使用 [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) 方法來加以顯示。

```csharp
private async void displayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

當使用者按一下對話方塊按鈕時，[ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) 方法會傳回 [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx)，讓您知道使用者按下哪一個按鈕。 

這個範例中的對話方塊會提問，並使用傳回的 [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) 來判斷使用者的回應。 

```csharp
private async void displayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog()
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        SecondaryButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();
    
    // Delete the file if the user clicked the primary button. 
    /// Otherwise, do nothing. 
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file. 
    }
}
```

## <a name="flyouts"></a>飛出視窗
###  <a name="create-a-flyout"></a>建立飛出視窗

飛出視窗為開放式容器，可顯示任意 UI 作為其內容。 

<div class="microsoft-internal-note">
這包含可巢狀於其他飛出視窗內的飛出視窗和操作功能表。
</div>

飛出視窗會附加至特定的控制項。 您可以使用 [Placement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx) 屬性，以指定飛出視窗的顯示位置︰Top、Left、Bottom、Right 或 Full。 如果您選取[「Full」位置模式](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx)，App 會延展飛出視窗，並將它置於 App 視窗的中央。 顯示這些項目時應將它錨定至叫用的物件，並指定其慣用的相對物件位置：Top、Left、Bottom 或 Right。 飛出視窗亦具備「Full」位置模式，並會嘗試延展飛出視窗並將它置於 App 視窗的中央。 一些像 [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) 的控制項會提供 [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) 屬性，讓您用來與飛出視窗相關聯。 

這個範例會建立簡單的飛出視窗，並在按下按鈕時顯示一些文字。 
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

如果控制項沒有 flyout 屬性，您可以改為使用 [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) 附加屬性。 當您這樣做時，也需要呼叫 [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) 方法以顯示飛出視窗。 

這個範例會將簡單的飛出視窗新增至影像。 當使用者點選影像時，App 會顯示飛出視窗。 

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50" 
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock TextWrapping="Wrap" Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

先前的範例會以內嵌方式定義飛出視窗。 您也可以將飛出視窗定義為靜態資源，然後和多個元素搭配使用。 這個範例會建立更複雜的飛出視窗，並會在點選影像縮圖時顯示該影像的大尺寸版本。 

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source 
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}" 
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
        <Flyout.FlyoutPresenterStyle>
            <Style TargetType="FlyoutPresenter">
                <Setter Property="ScrollViewer.ZoomMode" Value="Enabled"/>
                <Setter Property="Background" Value="Black"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="5"/>
                <Setter Property="MinHeight" Value="300"/>
                <Setter Property="MinWidth" Value="300"/>
            </Style>
        </Flyout.FlyoutPresenterStyle>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50" 
           Margin="10" Tapped="Image_Tapped" 
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

### <a name="style-a-flyout"></a>設定飛出視窗樣式
若要設定飛出視窗的樣式，請修改其 [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx)。 這個範例會顯示一段換行的文字，並使文字區塊可供螢幕助讀程式存取。

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode" 
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## <a name="get-the-samples"></a>取得範例
*   [XAML UI 基本知識](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章
- [工具提示](tooltips.md)
- [功能表和操作功能表](menus.md)
- [**Flyout 類別**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**ContentDialog 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)



<!--HONumber=Dec16_HO2-->


