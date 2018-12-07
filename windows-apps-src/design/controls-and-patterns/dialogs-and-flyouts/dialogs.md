---
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: 對話方塊控制項
label: Dialogs
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 520f4bdd72c51cd1508c9e655107ae909f6e4243
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8786862"
---
## <a name="dialog-controls"></a>對話方塊控制項

對話方塊控制項是強制回應 UI 重疊項目提供內容相關應用程式的資訊。 它們阻擋與應用程式視窗的互動，直到為止。 而且它們通常會需要使用者執行某種動作。

![對話方塊範例](../images/dialogs/dialog_RS2_delete_file.png)


> **重要 Api**: [ContentDialog 類別](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

使用對話方塊來通知使用者重要的資訊，或是在完成動作之前要求確認或其他資訊。

如需使用的時機建議[對話方塊和飛出視窗](index.md)，請參閱對話方塊與飛出視窗 （類似控制項），使用的時機。 

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡開啟應用程式並查看 <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> 或 <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> 運作情形。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>一般指導方針

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
- 使用 [ContentDialog 類別](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog) 建置您的對話方塊體驗。 請勿使用已過時的 MessageDialog API。

## <a name="how-to-create-a-dialog"></a>如何建立對話方塊
若要建立對話方塊，請使用 [ContentDialog 類別](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)。 您可以利用程式碼或標記建立對話方塊。 雖然在 XAML 中定義 UI 元素通常會比較容易，但針對簡單的對話方塊，單純使用程式碼會較為容易。 這個範例會建立一個對話方塊，以通知使用者沒有 WiFi 連線，然後使用 [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法來加以顯示。

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

當使用者按一下對話方塊按鈕時，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)，讓您知道使用者按下哪一個按鈕。

這個範例中的對話方塊會提問，並使用傳回的 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) 來判斷使用者的回應。

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

## <a name="provide-a-safe-action"></a>提供安全的動作
因為對話方塊會封鎖使用者互動，又因為按鈕是使用者關閉對話方塊的主要機制，所以務必在對話方塊中包含至少一個「安全」且不具破壞性的按鈕，例如 [關閉] 或 [了解!]。 **所有對話方塊都必須至少包含一個要關閉對話方塊的安全動作按鈕。** 這可確保使用者放心地在不執行動作的情況下關閉對話方塊。<br>![單按鈕對話方塊](../images/dialogs/dialog_RS2_one_button.png)

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

![雙按鈕對話方塊](../images/dialogs/dialog_RS2_two_button.png)

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

![三按鈕對話方塊](../images/dialogs/dialog_RS2_three_button.png)

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

## <a name="the-three-dialog-buttons"></a>三按鈕對話方塊
ContentDialog 有三個不同類型的按鈕，您可以用來建置對話方塊體驗。

- **CloseButton** (必要)：表示可讓使用者結束對話方塊的安全、非破壞性動作。 顯示為最右側按鈕。
- **PrimaryButton** (選擇性)：表示第一個「執行」動作。 顯示為最左側按鈕。
- **SecondaryButton** (選擇性)：表示第二個「執行」動作。 顯示為中間按鈕。

使用內建按鈕會適當安排按鈕位置、確保其正確回應鍵盤事件、確認即使即使螢幕小鍵盤已啟動也能保持命令區域在可見狀態，並且讓對話方塊與其他對話方塊保持一致的外觀。

### <a name="closebutton"></a>CloseButton
每個對話方塊都必須包含可讓使用者放心結束對話方塊的安全、非破壞性按鈕。

使用 ContentDialog.CloseButton API 來建立此按鈕。 這可讓您為所有輸入 (包括滑鼠、鍵盤、觸控和遊戲台) 建立適當的使用者體驗。 這個體驗出現的時機：
<ol>
    <li>使用者按一下或點選 CloseButton </li>
    <li>使用者按下系統返回按鈕 </li>
    <li>使用者按下鍵盤的 ESC 按鈕 </li>
    <li>使用者按下遊戲台 B 按鈕 </li>
</ol>

當使用者按一下對話方塊按鈕時，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult)，讓您知道使用者按下哪一個按鈕。 按下 CloseButton 會傳回 ContentDialogResult.None。

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton 和 SecondaryButton
除了 CloseButton 之外，還可以選擇性地向使用者顯示一個或兩個與主要指示相關的動作按鈕。
將 PrimaryButton 運用在第一個「執行」動作，並將 SecondaryButton 運用在第二個「執行」動作。 在三按鈕對話方塊中，PrimaryButton 通常表示肯定「執行」動作，而 SecondaryButton 通常表示中性或次要「執行」動作。
例如，App 可能會提示使用者訂閱一項服務。 負責肯定「執行」動作的 PrimaryButton 會主控「訂閱」文字，而負責中性「執行」動作的 SecondaryButton 則主控「試用」文字。 CloseButton 可讓使用者不執行任一動作就取消。

當使用者按一下 PrimaryButton 時，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 ContentDialogResult.Primary。
當使用者按一下 SecondaryButton 時，[ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) 方法會傳回 ContentDialogResult.Secondary。

![三按鈕對話方塊](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
您可以隨意選擇將三個按鈕其中之一區分為預設按鈕。 指定預設按鈕會導致發生下列情況：
- 按鈕將會獲得輔色按鈕視覺效果處理
- 按鈕將會自動回應 ENTER 鍵
    - 當使用者按下鍵盤的 ENTER 鍵時，與預設按鈕相關聯的按一下處理常式將會觸發，而 ContentDialogResult 則會傳回與預設按鈕相關聯的值
    - 如果使用者將鍵盤焦點放在處理 ENTER 的控制項上，預設按鈕便不會回應 ENTER 按下動作
- 除非對話方塊內容包含可設定焦點的 UI，否則按鈕在開啟對話方塊時都會自動取得焦點

使用 ContentDialog.DefaultButton 屬性來表示預設按鈕。 預設不會設定任何預設按鈕。

![包含預設按鈕的三按鈕對話方塊](../images/dialogs/dialog_RS2_three_button_default.png)

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

## <a name="confirmation-dialogs-okcancel"></a>確認對話方塊 (確定/取消)
確認對話方塊可讓使用者確認要執行的動作。 使用者可以確認動作或選擇取消。  
典型的確認對話方塊有兩個按鈕︰確認 (確定) 按鈕和取消按鈕。  

<ul>
    <li>
        <p>一般而言，確認按鈕應放在左邊 (主要按鈕)，取消按鈕 (次要按鈕) 應放在右邊。</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>如一般建議一節所述，將按鈕與可識別針對主要指示或內容做出明確回應的文字搭配使用。
    </li>
</ul>

> 某些平台將確認按鈕放在右邊，而非左邊。 那麼，為什麼建議將它放在左邊？  如果您假設大多數使用者慣用右手並且用這隻手拿手機，那麼確認按鈕位於左邊時實際上會更好按，因為按鈕比較有可能在使用者的拇指弧形內。按鈕位於螢幕右邊時，使用者必須將其拇指內扣，以致形成不太舒適的姿勢。





## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章
- [工具提示](../tooltips.md)
- [功能表和操作功能表](../menus.md)
- [Flyout 類別](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog 類別](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
