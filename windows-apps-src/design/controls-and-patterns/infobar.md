---
description: 資訊列是基本全應用程式訊息的內嵌通知。
title: 資訊列
template: detail.hbs
ms.date: 11/30/2020
ms.topic: article
keywords: windows 10, winui, uwp
pm-contact: gabilka
design-contact: kimsea
dev-contact: ranjeshj
ms.custom: 20H2
ms.localizationpriority: medium
ms.openlocfilehash: 422d2cb0874abe2fbe767a75d718cd1f0637ccee
ms.sourcegitcommit: b99fe39126fbb457c3690312641f57d22ba7c8b6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2020
ms.locfileid: "96604903"
---
# <a name="infobar"></a>資訊列
[資訊列] 控制項是用來將全應用程式狀態訊息顯示給高度可見但非侵入的使用者。 包含內建的嚴重性層級，可輕鬆地指出所顯示的訊息類型，以及包含您自己喚起行動或超連結按鈕的選項。 因為資訊列會與其他 UI 內容一起內嵌，所以該選項可讓控制項一律顯示或由使用者關閉。 

**取得 Windows UI 程式庫**

:::row:::
   :::column:::
      ![WinUI 標誌](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      [資訊列] 控制項需要 Windows UI 程式庫，該程式庫是 NuGet 套件，其中包含適用於 UWP 應用程式的新控制項和 UI 功能。 如需詳細資訊 (包括安裝指示)，請參閱 [Windows UI 程式庫](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 程式庫 API：** [資訊列類別](/uwp/api/microsoft.ui.xaml.controls.infobar)

> [!TIP]
> 在這整份文件中，我們使用 XAML 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已將此新增至我們的 [Page](/uwp/api/windows.ui.xaml.controls.page) 元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在後方的程式碼中，我們也使用 C# 中的 **muxc** 別名來代表我們已加入專案中的 Windows UI 程式庫 API。 我們已在檔案頂端新增了此 **using** 陳述式：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？
當使用者應該收到、認可或對已變更的應用程式狀態採取動作時，請使用資訊列控制項。 根據預設，通知會保留在內容區域中，直到使用者關閉為止，但不一定會中斷使用者流程。

資訊列會佔用您配置中的空間，而且其行為就像任何其他子項元素一樣。 資訊列並不會涵蓋其他內容或其頂端的浮動。

請勿使用資訊列控制項來確認或直接回應不會變更應用程式狀態的使用者動作、勿用於時間緊迫的警示，或用於非必要的訊息。

### <a name="remarks"></a>備註
使用使用者所關閉的資訊列，或在「直接」影響應用程式認知或體驗的案例狀態為解決時，使用資訊列。


以下是一些範例：
- 網際網路連線中斷
- 自動觸發時儲存文件發生錯誤，與特定使用者動作無關
- 嘗試錄製時未插入任何麥克風
- 無法連線到伺服器
- 應用程式的訂用帳戶已過期


針對「間接」影響應用程式認知或體驗的案例，使用使用者關閉的資訊列

以下是一些範例：
- 呼叫已開始錄製
- 已套用包含「版本資訊」連結的更新
- 服務條款已更新且需要確認
- 全應用程式備份已成功、非同步完成
- 應用程式的訂用帳戶即將到期

### <a name="when-should-a-different-control-be-used"></a>何時應該使用不同的控制項？

在某些案例中，可能更適合使用 [ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[飛出視窗](/uwp/api/Windows.UI.Xaml.Controls.Flyout) 或 [TeachingTip](/uwp/api/Microsoft.UI.Xaml.Controls.TeachingTip)。

- 在不需要持續性通知的情況下 (例如，顯示特定 UI 元素內容中的資訊)，[飛出視窗](/uwp/design/controls-and-patterns/dialogs-and-flyouts/flyouts)是較好的選項。 
- 在應用程式確認使用者動作的情況下，如需顯示使用者 *「必須」_讀取的資訊，請使用 [ContentDialog](/uwp/design/controls-and-patterns/dialogs-and-flyouts/dialogs)。
  - 此外，如果應用程式的狀態變更太嚴重，而需要封鎖使用者與應用程式互動的所有進一步功能，則請使用 ContentDialog。
- 在通知為暫時性教學短片的案例中，[TeachingTip](/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip) 是較佳的選擇。


如需選擇正確通知控制項的詳細資訊，請參閱[對話方塊和飛出視窗](/uwp/design/controls-and-patterns/dialogs-and-flyouts/)一文。


## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/InfoBar">開啟應用程式，並查看 InfoBar 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-an-infobar"></a>建立資訊列
下列 XAML 描述內嵌資訊列，其中包含資訊通知的預設樣式。 資訊列可以像任何其他元素一樣放置，並且會遵循基礎版面配置行為。
例如，在垂直 StackPanel 中，資訊列會水平展開以填滿可用的寬度。 


根據預設，將不會顯示資訊列。 將 XAML 中或程式碼後置的 IsOpen 屬性設為 true，以顯示資訊列。


```xaml
<muxc:InfoBar x:Name="UpdateAvailableNotification"
    Title="Update available."
    Message="Restart the application to apply the latest update.">
</muxc:InfoBar>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(IsUpdateAvailable())
    {
        UpdateAvailableNotification.IsOpen = true;
    }
}
```

![具有關閉按鈕和訊息的預設狀態資訊列範例](images/infobar-default-title-message.png)

### <a name="using-pre-defined-severity-levels"></a>使用預先定義的嚴重性層級
您可以透過 [嚴重性] 屬性來設定資訊列的類型，以根據通知的重要性來自動設定一致的狀態色彩、圖示和輔助技術設定。 如果未設定任何 [嚴重性]，則會套用預設的資訊樣式。


```xaml
<muxc:InfoBar x:Name="SubscriptionExpiringNotification"
    Severity="Warning"
    Title="Your subscription is expiring in 3 days."
    Message="Renew your subscription to keep all functionality" />
```
![具有關閉按鈕和訊息的警告狀態資訊列範例](images/infobar-warning-title-message.png)

### <a name="programmatic-close-in-infobar"></a>在資訊列中以程式設計方式關閉
使用者可以透過關閉按鈕或以程式設計方式關閉資訊列。 如果通知在狀態為 [已解決] 之前必須處於檢視，而您想要移除使用者關閉資訊列的功能，則可以將 IsClosable 屬性設定為 false。


這個屬性的預設值為 true，在此情況下，關閉按鈕會出現並採用 'X' 的格式。


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working."
    IsClosable="False" />
```
![處於 [錯誤] 狀態且沒有關閉按鈕的資訊列範例](images/infobar-error-no-close.png)

### <a name="customization-background-color-and-icon"></a>自訂：背景色彩和圖示
在預先定義的嚴重性層級之外，您可以設定 Background 和 IconSource 屬性，來自訂圖示和背景色彩。 資訊列會保留所定義嚴重性的輔助技術設定，如果未定義，則為預設值。


您可以透過標準 Background 屬性來設定自訂背景色彩，並將覆寫依 [嚴重性] 設定的色彩。
設定您自己的色彩時，請記住內容可讀性和可存取性。


您可以透過 IconSource 屬性來設定自訂圖示。 根據預設，圖示會是可見的 (假設控制項未摺疊)。
藉由將 IsIconVisible 屬性設定為 false，即可移除此圖示。 針對自訂圖示，建議的圖示大小為20 px。


```xaml
<muxc:InfoBar x:Name="CallRecordingNotification"
    Title="Recording started"
    Message="Your call has begun recording."
    Background="{StaticResource LavenderBackgroundBrush}">
    <muxc:InfoBar.IconSource>
        <muxc:SymbolIconSource Symbol="Phone" />
    </muxc:InfoBar.IconSource>
</muxc:InfoBar>
```

![預設狀態中具有自訂背景色彩、自訂圖示和關閉按鈕的資訊列範例](images/infobar-custom-icon-color.png)

### <a name="add-an-action-button"></a>新增動作按鈕

您可以定義自己的按鈕 (繼承 [ButtonBase](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) 並在 ActionButton 屬性中加以設定)，以新增其他動作按鈕。 自訂樣式會套用至[按鈕](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)和 [HyperlinkButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 類型的動作按鈕，以取得一致性和可存取性。 除了 ActionButton 屬性之外，還可以透過自訂內容新增其他動作按鈕，且這些按鈕將出現在訊息下方。


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Network Settings" Click="InternetInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![處於 [錯誤] 狀態且含單行訊息和動作按鈕的資訊列範例](images/infobar-error-action-button.png)

```xaml
<muxc:InfoBar x:Name="TermsAndConditionsUpdatedNotification"
    Title="Terms and Conditions Updated"
    Message="Dismissal of this message implies agreement to the updated Terms and Conditions. Please view the changes on our website.">
    <muxc:InfoBar.ActionButton>
        <HyperlinkButton Content="Terms and Conditions Sep 2020" 
            NavigateUri="https://www.example.com"
            Click="UpdateInfoBarHyperlinkButton_Click" />
    <muxc:InfoBar.ActionButton />
</muxc:InfoBar>
```
![包含展開多行和超連結訊息的資訊列範例](images/infobar-default-hyperlink.png)

### <a name="content-wrapping"></a>內容換行
如果資訊列內容 (不包括自訂內容) 無法放在一條水平線上，則會垂直配置。 [標題]、[訊息] 和 [ActionButton] (如果有的話) 都會出現在不同的行上。


```xaml
<muxc:InfoBar x:Name="BackupCompleteNotification"
    Severity="Success"
    Title="Backup complete: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."  
    Message="All documents were uploaded successfully. Ultrices sagittis orci a scelerisque. Aliquet risus feugiat in ante metus dictum at tempor commodo. Auctor augue mauris augue neque gravida.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Action"
            Click="BackupInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![[成功] 狀態且含多行標題和訊息資訊列的資訊列範例](images/infobar-success-content-wrapping.png)


### <a name="custom-content"></a>自訂內容
可以使用 Content 屬性將 XAML 內容新增至資訊列。 其會在控制項內容其餘部分下方顯示於自己的行中。 資訊列將會展開，以符合所定義的內容。


```xaml
<muxc:InfoBar x:Name="BackupInProgressNotification"
    Title="Backup in progress"  
    Message="Your documents are being saved to the cloud"
    IsClosable="False">
    <muxc:InfoBar.Content>
        <ProgressBar IsIndeterminate="True" Margin="0,0,0,6"/>
    </muxc:InfoBar.Content>
</muxc:InfoBar>
```

![預設狀態包含不確定進度列的資訊列範例](images/infobar-default-custom-content.gif)

### <a name="lightweight-styling"></a>輕量型樣式設定

您可以修改預設樣式和 ControlTemplate，為控制項提供獨特的外觀。 如需詳細資訊，請參閱[樣式控制項](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles)文章的[輕量型樣式設定](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles#lightweight-styling)一節。

例如，下列各項會造成頁面資訊列上的標題列字型大小為 22pt：

```xaml
<Page.Resources>
    <x:Double x:Key="InfoBarTitleFontSize">22</x:Double>
</Page.Resources>
```
### <a name="canceling-close"></a>取消關閉
Closing 事件可用來取消和/或延遲資訊列的關閉。 這可用來保持資訊列開啟，或允許自訂動作有時間顯示。 取消關閉資訊列時，IsOpen 會回到 true。 也可以取消程式設計方式的關閉。


```xaml
<muxc:InfoBar x:Name="UpdateAvailable"
    Title="Update Available"
    Message="Please close this tip to apply required security updates to this application"
    Closing="InfoBar_Closing">
</muxc:InfoBar>
```

```csharp
public void InfoBar_Closing(InfoBar sender, InfoBarClosingEventArgs args)
{
    if (args.Reason == InfoBarCloseReason.CloseButton) {
        if (!ApplyUpdates()) {
            // could not apply updates - do not close
            args.Cancel = true;
        }
    }   
}
```

## <a name="recommendations"></a>建議

### <a name="enter-and-exit-usability"></a>進入並結束可用性
#### <a name="flashing-content"></a>閃爍內容
資訊列不應該快速顯示及消失，以防止螢幕上的閃爍。 避免有光敏感反應的人員看到閃爍，從而改善應用程式的可用性。


對於透過應用程式狀態條件自動輸入及結束檢視的資訊列，建議您在應用程式中加入邏輯，以防止內容在資料列中快速或是多次出現或消失。 不過，一般而言，此控制項應用於長期的狀態訊息。


#### <a name="updating-the-infobar"></a>更新資訊列
控制項開啟之後，對各種屬性進行的任何變更 (例如更新訊息或嚴重性) 都不會引發通知事件。 如果您想要通知使用者使用資訊列更新內容的螢幕助讀程式，建議您關閉並重新開啟控制項來觸發事件。


#### <a name="inline-messages-offsetting-content"></a>內嵌訊息抵銷內容
針對與其他 UI 內容內嵌的資訊列，請記住頁面的其餘部分將如何反應式回應元素新增。


具有巨幅高度的資訊列，可能會大幅改變頁面上其他元素的版面配置。 如果資訊列快速出現或消失，特別是連續的情況下，可能會讓使用者對於變更的視覺狀態造成混淆。


### <a name="color-and-icon"></a>色彩和圖示
自訂預設 [嚴重性] 層級以外的色彩和圖示時，請記住使用者對於一組標準圖示和色彩所期望的含義。


此外，預設的嚴重性色彩已針對主題變更、高對比模式、色彩混淆協助工具，以及與前景色彩的對比而設計。 建議您盡可能使用這些色彩，並在應用程式中包含自訂邏輯，以適應各種色彩狀態和可存取性功能。


請參閱適用於[標準圖示](/windows/win32/uxguide/vis-std-icons)和[色彩](/windows/win32/uxguide/vis-color)的 UX 指導方針，以確保您將訊息清楚地傳達給使用者並可供使用者存取。


#### <a name="severity"></a>嚴重性
 針對不符合 [標題]、[訊息] 或自訂內容中所傳達資訊的通知，請避免設定 [嚴重性] 屬性。
 

 隨附的資訊應著重於傳達下列內容，以使用該嚴重性。
 - 錯誤：發生錯誤或問題。
 - 警告：未來可能會造成問題的狀況。
 - 成功：長期和/或背景工作已完成。
 - Default：需要使用者注意的一般資訊。

圖示和色彩不應該是表示通知意義的唯一 UI 元件。 應該包含通知 [標題] 和/或 [訊息] 中的文字，以顯示資訊。


### <a name="message"></a>訊息 

您通知中的文字在所有語言中將不會是固定的長度。 對於 [標題] 和 [訊息] 屬性，這可能會影響您的通知是否會擴展到第二行。 建議您避免根據訊息長度或設定為特定語言的其他 UI 元素來進行定位。

當以從右至左 (RTL) 或從左至右 (LTR) 的語言進行當地語系化時，通知會遵循標準鏡像行為。 僅在具有方向性時，才會對圖示進行鏡像。

如需有關通知中的文字當地語系化詳細資訊，請參閱[調整配置和字型並支援 RTL](/uwp/design/globalizing/adjust-layout-and-fonts--and-support-rtl)。

## <a name="related-articles"></a>相關文章

_ [對話方塊和飛出視窗](./dialogs-and-flyouts/index.md)
