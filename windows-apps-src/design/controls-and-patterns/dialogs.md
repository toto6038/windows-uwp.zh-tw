---
author: mijacobs
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 對話方塊和飛出視窗
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 9ceb698bfbe95693ff9d5785b4bea94f1ec3070c
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675375"
---
# <a name="dialogs-and-flyouts"></a>對話方塊和飛出視窗



對話方塊和飛出視窗為在發生需要來自使用者的通知、核准或是其他資訊的情況時，所顯示的暫時性 UI 元素。

> **重要 API**：[ContentDialog 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[Flyout 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::列::: :::欄::: **對話方塊**
        
        ![Example of a dialog](images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::列結束:::


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

* 使用對話方塊來通知使用者重要的資訊，或是在完成動作之前要求確認或其他資訊。
* 不要使用飛出視窗來取代[工具提示](tooltips.md)或[操作功能表](menus.md)。 使用工具提示來顯示會在特定時間之後隱藏的簡短說明。 使用操作功能表來執行與 UI 元素相關聯的內容相關動作，例如複製和貼上。  


對話方塊和飛出視窗可確保使用者知曉重要的資訊，但也會干擾使用者體驗。 對話方塊是強制回應 (阻斷式) 對話方塊，因此會干擾使用者，並在他們與對話方塊互動之前阻擋其繼續執行動作。 飛出視窗能提供較不突兀的體驗，但是顯示太多的飛出視窗也可能會造成困擾。

考量您要分享之資訊的重要性：是否重要到必須打擾使用者？ 也請考慮需顯示資訊的頻率。如果您每隔幾分鐘便需要顯示對話方塊或通知，您可以考慮將這項資訊配置在主要 UI 上。 例如，在聊天用戶端中，與其在有朋友登入時就顯示飛出視窗，您可以顯示當時在線上的朋友清單，並在有朋友登入時針對他們進行醒目提示。

對話方塊常用來在執行動作之前確認該動作 (例如刪除檔案)。 如果您預期使用者會頻繁執行某項特定的動作，請考慮讓使用者能夠在錯誤地做出該動作時進行復原，而不是強制使用者每次都必須確認該動作。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="dialogs-vs-flyouts"></a>對話方塊與飛出視窗

在您決定要使用對話方塊或飛出視窗之後，您必須選擇要使用哪一個。

由於對話方塊會阻止互動，而飛出視窗不會，您應該將對話方塊用在想讓使用者放下手上的工作，並將注意力放到特定資訊或是回答問題的情況。 相反地，飛出視窗適合在您想要引起使用者注意，但即使他們想要忽略它也無妨的情況下使用。

:::列::: :::欄:::
   <p><b>適合使用對話方塊的情況...</b> <br/>
<ul>
<li>表示使用者<b>必須</b>先閱讀並確認才能繼續執行工作的重要資訊。 範例包含：
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
:::欄結束::: :::欄::: <p><b>適合使用飛出視窗的情況...</b> <br/>
<ul>
<li>在完成動作之前收集所需的其他資訊。</li>
<li>顯示僅在某些情況下有關聯的資訊。 例如，在影像中心 App 中，當使用者按一下影像縮圖時，您可以使用飛出視窗顯示該影像的大尺寸版本。</li>
<li>顯示詳細資訊，例如頁面上項目的詳細資料或較長描述。</li>
</ul></p>
:::欄結束::: :::列結束:::



## <a name="dialogs"></a>對話方塊
### <a name="general-guidelines"></a>一般指導方針

-   在對話方塊文字的第一行中明確識別問題或使用者的目標。
-   對話方塊標題是主要指示，而且是選擇性的。
    -   使用簡短標題說明使用者需要使用對話方塊執行什麼動作。
    -   如果您使用對話方塊來傳達簡單的訊息、錯誤或問題，可以選擇性地省略標題。 只需要內容文字傳達核心資訊即可。
    -   確定標題與按鈕選項直接相關。
-   對話方塊內容包含描述性文字，而且是必要的。
    -   盡可能簡要地呈現訊息、錯誤或造成應用程式無法繼續執行的問題。
    -   使用對話方塊標題時，請利用內容區域提供更多詳細資料或定義詞彙。 不要以只有些微差異的用字重複標題。
-   至少必須顯示一個對話方塊按鈕。
    -   確保對話方塊至少有一個對應於安全、非破壞性動作的按鈕，例如 [了解!]、[關閉] 或 [取消]。 使用 CloseButton API 來新增此按鈕。
    -   使用對主要指示或內容的明確回應做為按鈕文字。 例如，「是否允許 AppName 存取您的位置?」，後面接 [允許] 和 [封鎖] 按鈕。 明確回應可讓使用者更快速了解，更有效率地做出決定。
    - 確保動作按鈕的文字簡潔扼要。 簡短字串可讓使用者快速且放心地進行選擇。
    - 除了安全、不具破壞性的動作之外，還可以選擇性地向使用者顯示一個或兩個與主要指示相關的動作按鈕。 這些「執行」動作按鈕可確認對話方塊的要點。 使用 PrimaryButton 和 SecondaryButton API 來新增這些「執行」動作。
    - 「執行」動作按鈕應該顯示為最左側按鈕。 安全、不具破壞性的動作應該顯示為最右側按鈕。
    - 您可以隨意選擇將三個按鈕其中之一區分為對話方塊的預設按鈕。 使用 DefaultButton API 來區分其中一個按鈕。  
-   針對頁面上特定位置的內容相關錯誤，例如 (密碼欄位中的) 驗證錯誤，請使用 App 本身的畫布顯示內嵌錯誤，而不要使用對話方塊。
- 使用 [ContentDialog 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) 建置您的對話方塊體驗。 請勿使用已過時的 MessageDialog API。

### <a name="dialog-scenarios"></a>對話方塊案例
因為對話方塊會封鎖使用者互動，又因為按鈕是使用者關閉對話方塊的主要機制，所以務必在對話方塊中包含至少一個「安全」且不具破壞性的按鈕，例如 [關閉] 或 [了解!]。 **所有對話方塊都必須至少包含一個要關閉對話方塊的安全動作按鈕。** 這可確保使用者放心地在不執行動作的情況下關閉對話方塊。<br>![單按鈕對話方塊](images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

使用對話方塊來顯示阻斷式問題時，對話方塊應該向使用者顯示與問題相關的動作按鈕。 「安全」且不具破壞性的按鈕可能會伴隨一個或兩個「執行」動作按鈕。 向使用者顯示多個選項時，請確定按鈕已清楚說明與所提問題相關的「執行」和安全/「不執行」動作。

![雙按鈕對話方塊](images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

向使用者提供兩個「執行」動作和一個「不執行」動作時，您會使用三按鈕對話方塊。 三按鈕對話方塊應在明確區分次要動作與安全/關閉動作的情況下謹慎使用。

![三按鈕對話方塊](images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="the-three-dialog-buttons"></a>三按鈕對話方塊
ContentDialog 有三個不同類型的按鈕，您可以用來建置對話方塊體驗。

- **CloseButton** (必要)：表示可讓使用者結束對話方塊的安全、非破壞性動作。 顯示為最右側按鈕。
- **PrimaryButton** (選擇性)：表示第一個「執行」動作。 顯示為最左側按鈕。
- **SecondaryButton** (選擇性)：表示第二個「執行」動作。 顯示為中間按鈕。

使用內建按鈕會適當安排按鈕位置、確保其正確回應鍵盤事件、確認即使即使螢幕小鍵盤已啟動也能保持命令區域在可見狀態，並且讓對話方塊與其他對話方塊保持一致的外觀。

#### <a name="closebutton"></a>CloseButton
每個對話方塊都必須包含可讓使用者放心結束對話方塊的安全、非破壞性按鈕。

使用 ContentDialog.CloseButton API 來建立此按鈕。 這可讓您為所有輸入 (包括滑鼠、鍵盤、觸控和遊戲台) 建立適當的使用者體驗。 這個體驗出現的時機：
<ol>
    <li>使用者按一下或點選 CloseButton </li>
    <li>使用者按下系統返回按鈕 </li>
    <li>使用者按下鍵盤的 ESC 按鈕 </li>
    <li>使用者按下遊戲台 B 按鈕 </li>
</ol>

當使用者按一下對話方塊按鈕時，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)，讓您知道使用者按下哪一個按鈕。 按下 CloseButton 會傳回 ContentDialogResult.None。

#### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton 和 SecondaryButton
除了 CloseButton 之外，還可以選擇性地向使用者顯示一個或兩個與主要指示相關的動作按鈕。
將 PrimaryButton 運用在第一個「執行」動作，並將 SecondaryButton 運用在第二個「執行」動作。 在三按鈕對話方塊中，PrimaryButton 通常表示肯定「執行」動作，而 SecondaryButton 通常表示中性或次要「執行」動作。
例如，App 可能會提示使用者訂閱一項服務。 負責肯定「執行」動作的 PrimaryButton 會主控「訂閱」文字，而負責中性「執行」動作的 SecondaryButton 則主控「試用」文字。 CloseButton 可讓使用者不執行任一動作就取消。

當使用者按一下 PrimaryButton 時，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 ContentDialogResult.Primary。
當使用者按一下 SecondaryButton 時，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 ContentDialogResult.Secondary。

![三按鈕對話方塊](images/dialogs/dialog_RS2_three_button.png)

#### <a name="defaultbutton"></a>DefaultButton
您可以隨意選擇將三個按鈕其中之一區分為預設按鈕。 指定預設按鈕會導致發生下列情況：
- 按鈕將會獲得輔色按鈕視覺效果處理
- 按鈕將會自動回應 ENTER 鍵
    - 當使用者按下鍵盤的 ENTER 鍵時，與預設按鈕相關聯的按一下處理常式將會觸發，而 ContentDialogResult 則會傳回與預設按鈕相關聯的值
    - 如果使用者將鍵盤焦點放在處理 ENTER 的控制項上，預設按鈕便不會回應 ENTER 按下動作
- 除非對話方塊內容包含可設定焦點的 UI，否則按鈕在開啟對話方塊時都會自動取得焦點

使用 ContentDialog.DefaultButton 屬性來表示預設按鈕。 預設不會設定任何預設按鈕。

![包含預設按鈕的三按鈕對話方塊](images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="confirmation-dialogs-okcancel"></a>確認對話方塊 (確定/取消)
確認對話方塊可讓使用者確認要執行的動作。 使用者可以確認動作或選擇取消。  
典型的確認對話方塊有兩個按鈕︰確認 (確定) 按鈕和取消按鈕。  

<ul>
    <li>
        <p>一般而言，確認按鈕應放在左邊 (主要按鈕)，取消按鈕 (次要按鈕) 應放在右邊。</p>
         ![確定/取消對話方塊](images/dialogs/dialog_RS2_delete_file.png)

    </li>
    <li>如一般建議一節所述，將按鈕與可識別針對主要指示或內容做出明確回應的文字搭配使用。
    </li>
</ul>

> 某些平台將確認按鈕放在右邊，而非左邊。 那麼，為什麼建議將它放在左邊？  如果您假設大多數使用者慣用右手並且用這隻手拿手機，那麼確認按鈕位於左邊時實際上會更好按，因為按鈕比較有可能在使用者的拇指弧形內。按鈕位於螢幕右邊時，使用者必須將其拇指內扣，以致形成不太舒適的姿勢。

### <a name="create-a-dialog"></a>建立對話方塊
若要建立對話方塊，請使用 [ContentDialog 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)。 您可以利用程式碼或標記建立對話方塊。 雖然在 XAML 中定義 UI 元素通常會比較容易，但針對簡單的對話方塊，單純使用程式碼會較為容易。 這個範例會建立一個對話方塊，以通知使用者沒有 WiFi 連線，然後使用 [ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法來加以顯示。

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

當使用者按一下對話方塊按鈕時，[ShowAsync](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)，讓您知道使用者按下哪一個按鈕。

這個範例中的對話方塊會提問，並使用傳回的 [ContentDialogResult](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 來判斷使用者的回應。

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="flyouts"></a>飛出視窗
###  <a name="create-a-flyout"></a>建立飛出視窗

飛出視窗是一個會消失關閉的容器，可顯示任意 UI 做為其內容。 飛出視窗可包含其他飛出視窗或操作功能，以建立巢狀體驗。

![巢狀內嵌於飛出視窗內的操作功能表](images/flyout-nested.png)

飛出視窗已附加至特定控制項。 您可以使用 [Placement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) 屬性來指定飛出視窗的顯示位置︰Top、Left、Bottom、Right 或 Full。 如果您選取[[Full] 位置模式](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode)，App 會延展飛出視窗，並將其置於 App 視窗的中央。 有些控制項 (例如 [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)) 會提供可用來與飛出視窗或[操作功能表](menus.md)產生關聯的 [Flyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) 屬性。

此範例建立簡單的飛出視窗，並在按下按鈕時顯示一些文字。
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

如果控制項沒有 flyout 屬性，您可以改為使用 [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty) 附加屬性。 當您這樣做時，也需要呼叫 [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) 方法以顯示飛出視窗。

這個範例會將簡單的飛出視窗新增至影像。 當使用者點選影像時，App 會顯示飛出視窗。

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
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
若要設定飛出視窗的樣式，請修改其 [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle)。 此範例顯示一段換行的文字，並使文字區塊可供螢幕助讀程式存取。

![包含換行文字的協助工具飛出視窗](images/flyout-wrapping-text.png)

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

#### <a name="styling-flyouts-for-10-foot-experience"></a>設定 10 英呎體驗的飛出視窗樣式

像飛出視窗這樣的消失關閉控制項會將鍵盤和遊戲台焦點圈限在暫時性 UI 內，直到其關閉為止。 若要提供此行為的視覺提示，Xbox 上的消失關閉控制項將會繪製重疊，以使超出範圍 UI 的對比度和可見度變暗。 此行為可以使用 [`LightDismissOverlayMode`](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode) 屬性進行修改。 根據預設，飛出視窗將會在 Xbox 上繪製消失關閉重疊，但不會在其他裝置系列上繪製，不過應用程式可以選擇將重疊強制為一律 **\[開啟\]** 或一律 **\[關閉\]**。

![包含變暗重疊的飛出視窗](images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

### <a name="light-dismiss-behavior"></a>消失關閉行為
飛出視窗可以使用快速消失關閉動作來關閉，動作包括：
-   點選飛出視窗外部
-   按下 Esc 鍵盤按鍵
-   按下硬體或軟體系統返回按鈕
-   按下遊戲台 B 按鈕

以點選來關閉時，這個手勢通常會沒入而無法傳遞至下面的 UI。 舉例說，如果有按鈕隱現於已開啟的飛出視窗背後，使用者的第一個點選動作會關閉飛出視窗，但不會啟用此按鈕。 按下按鈕需要第二次點選。

您可以指定按鈕做為飛出視窗的輸入傳遞元素，來變更此行為。 飛出視窗將會因為上述消失關閉動作而關閉，還會將點選事件傳遞至其所指定的`OverlayInputPassThroughElement`。 請考慮採用此行為，在功能類似的項目上加速使用者互動。 如果 App 有我的最愛集合，而且集合中的每個項目都包含附加飛出視窗時，您可以合理預料使用者可能會希望與接連多個飛出視窗進行互動。

[!NOTE] 請小心，不要指定會產生破壞性動作的重疊輸入傳遞元素。 使用者已經習慣於不會啟用主要 UI 的慎重消失關閉動作。 不能讓 [關閉]、[刪除] 或類似具破壞性的按鈕在消失關閉時啟動，以免發生非預期和破壞性的行為。

在下列範例中，FavoritesBar 所有的三個按鈕都會在第一次點選時啟動。

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章
- [工具提示](tooltips.md)
- [功能表和操作功能表](menus.md)
- [Flyout 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 類別](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
