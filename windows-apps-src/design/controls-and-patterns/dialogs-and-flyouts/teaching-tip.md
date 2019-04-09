---
ms.openlocfilehash: 15379e51f8c272d0cc1e888684322104186bb200
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249500"
---
# <a name="teaching-tip"></a>教學提示

教學提示是半持續性和內容豐富飛出視窗上提供內容資訊。 它通常用於通知、 提醒，並教導使用者有關可能增強使用者體驗的重要和新功能。

**重要的 Api:**[TeachingTip 類別](https://docs.microsoft.com/en-us/uwp/api/microsoft.ui.xaml.controls.teachingtip)

教學提示可能光關閉，或需要明確的動作，以關閉。 教學提示可以針對特定的 UI 元素，其尾端，而且也用不含結尾或目標。

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？ 

使用**TeachingTip**專注於新的或重要更新和功能，讓使用者注意，下列時間提醒使用者的非必要的選項會改善使用者體驗，或教導使用者應該如何完成工作的控制項。 

教學提示是暫時性的因為它不是提示使用者有關的錯誤或重要的狀態會變更建議使用的控制項。


## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您有<strong style="font-weight: semi-bold">XAML 控制項陳列庫</strong>應用程式安裝，請按一下這裡可<a href="xamlcontrolsgallery:/item/TeachingTip">開啟應用程式，並查看動作中的 TeachingTip</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">取得原始程式碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

教學提示可以有數個組態，包括下列值得注意的項目。

教學提示可以針對特定的 UI 項目，來增強它所要呈現之資訊的內容清楚其尾端。 

![範例應用程式與目標儲存教學提示按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-targeted.png)

時所顯示的資訊不適用於特定的 UI 項目，可以藉由移除結尾建立 nontargeted 教學提示。

![在右下角中，提示使用範例應用程式的教學。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-non-targeted.png)

教學提示可以要求使用者透過在左上角中的"X"按鈕或在下方的 「 關閉 」 按鈕關閉它。 教學提示可能也是淺-關閉啟用在此情況下有沒有 [關閉] 按鈕，而且在使用者捲動或與應用程式的其他項目互動時，會改為關閉授課提示。 由於這個行為，光關閉提示需要放在可捲動區域時，提示會是最佳的解決方案。 

![範例應用程式與光線關閉授課右下角中的提示。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。](../images/teaching-tip-light-dismiss.png)


### <a name="create-a-teaching-tip"></a>建立教學秘訣

以下是針對目標教學 XAML 提示示範具有標題和副標題 TeachingTip 的預設外觀的控制項。 請注意，教導提示可以出現在任何位置的項目樹狀結構或程式碼後置。 在此範例中，它位於 ResourceDictionary 中。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Save automatically"
            Subtitle="When you save your file to OneDrive, we save your changes as you go - so you never have to.">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

C#
```C#
public MainPage()
{
    this.InitializeComponent();

    if(!HaveExplainedAutoSave())
    {
        AutoSaveTip.IsOpen = true;
        SetHaveExplainedAutoSave();
    }
}
```

顯示的頁面包含按鈕也熱衷於提示時，以下是結果：

![範例應用程式與目標儲存教學提示按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-targeted.png)

### <a name="non-targeted-tips"></a>非目標的秘訣

並非所有的提示與螢幕上的項目。 針對這些案例，不會將目標屬性設定，並教導提示將會改為顯示相對於 xaml 根項目的邊緣。 不過，教學提示可以有結尾移除但 TailVisibility 屬性設定為 「 已摺疊 」，以保留 UI 項目相對的位置。 下列範例是非目標教學提示。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to.">
</controls:TeachingTip>
```

請注意，在此範例中 TeachingTip 項目樹狀結構，而非 ResourceDictionary 或程式碼後置。 這已不會影響行為。TeachingTip 只開啟時，會顯示與所佔的未配置空間。

![在右下角中，提示使用範例應用程式的教學。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-non-targeted.png)

### <a name="preferred-placement"></a>慣用的位置

教學提示複寫飛出視窗上的[FlyoutPlacementMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode) TeachingTipPlacementMode 屬性放置行為。 預設位置模式會嘗試放置高於其目標的目標的教學工具提示和底部的 xaml 根置中對齊的非目標教學提示。 為含有彈出式視窗，如果慣用的位置模式不會留出空間給教學秘訣，若要顯示，另一個位置模式將會自動選擇。 

對於預測遊樂器輸入的應用程式，請參閱[遊戲台和遠端控制互動]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)。 建議您測試的使用所有可能的組態，應用程式的 UI 的每個授課提示的遊戲台存取範圍。

置中底部的其目標，而且向左移位的教學提示的主體的結尾會出現目標的教學具有提示設定為"BottomLeft"其 PreferredPlacement。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            PreferredPlacement="BottomLeft">
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![使用範例應用程式為目標的教學提示，其左上角下方的 [儲存] 按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-targeted-preferred-placement.png)


設為"BottomLeft"其 PreferredPlacement 與非目標教學提示會出現在 xaml 根項目的左下角。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft">
</controls:TeachingTip>
```

![在左下角中，提示使用範例應用程式的教學。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-non-targeted-preferred-placement.png)

下圖說明所有可設定的目標教導祕訣 13 PreferredPlacement 模式的結果。
![三個物件加上 「 目標 」 目標教導用於其周圍顯示教學提示目標的慣用的位置模式的秘訣。 置中對齊中間的第一個目標是標示為"Center"指向其結尾的目標的目標的教學工具提示。 置中對齊上方的第一個目標是標示為 「 Top 」 向在其結尾的目標的目標的教學工具提示。 置中的第一個目標是標示為 「 權利 」 左邊指向其結尾的目標的目標的教學工具提示。 置中下方的第一個目標是標示為 「 下 」 指向其結尾的目標的目標的教學工具提示。 置中對齊左邊的第一個目標是標示為在其結尾的目標的 「 左側 」 指的目標的教學工具提示。 左邊的第二個目標教學提示標示為 「 LeftTop"指向位於目標上的權限和具有其主體移往上。 上述第二個目標教學提示標示為 「 TopLeft"指向目標和其主體改變了 leftward。 右邊的第二個目標是標示為 「 RightBottom"點會留在目標，並具有其主體移往下的教學工具提示。 低於第二個目標教學提示標示為 「 BottomRight"指向上方位於目標上的其主體改變了 rightward。 上面的第三個目標教學提示標示為 「 右上"指向目標和其主體改變了 rightward。 右邊的第三個目標是標示為 「 RightTop"點會留在目標，並具有其主體移往上的教學工具提示。 低於第三個目標教學提示標示為 「 BottomLeft"指向上方位於目標上的其主體改變了 leftward。 左邊的第三個目標教學提示標示為 「 LeftBottom"指向位於目標上的權限和其主體改變了向下。](../images/teaching-tip-targeted-preferred-placement-modes.png)

下圖說明所有可設定的非目標教學祕訣 13 PreferredPlacement 模式的結果。
![應用程式視窗，顯示九個非目標教學秘訣來示範教學提示非目標的慣用的位置模式。 在左上角的應用程式的教學提示會標示為 「 TopLeft 或 LeftTop。 」 在應用程式的上邊緣置中的教學提示會標示為 「 頂端 」。 在右上角的應用程式的教學提示會標示為 「 右上或 RightTop。 」 在應用程式的左邊緣置中的教學提示會標示為"Left"。 置中對齊中間應用程式的教學提示會標示為 「 中心 」。
在應用程式的右邊緣置中的教學提示會標示為"Right"。 在左下角的應用程式的教學提示會標示為 「 BottomLeft 或 LeftBottom。 」 在應用程式的下邊緣置中的教學提示會標示為 「 下限 」。 在右下角的應用程式的教學提示會標示為 「 BottomRight 或 RightBottom。 」](../images/teaching-tip-non-targeted-preferred-placement-modes.png)

### <a name="add-a-placement-margin"></a>加入放置邊界  

您可以控制目標的教學提示設定除了其目標的進度，以及多遠的非目標教學秘訣除了 xaml 根項目的邊緣，使用 PlacementMargin 屬性設定。 像是[邊界](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.margin)、 PlacementMargin 有四個值 – 左、 右、 前幾大，和下 – 因此只會使用相關的值。 比方說，PlacementMargin.Left 提示留的目標或 xaml 根項目的左邊緣時適用。

下列範例會示範非目標具有的提示 PlacementMargin 左/上/右/下所有的設定為 80。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomLeft"
    PlacementMargin="80">
</controls:TeachingTip>
```

![使用範例應用程式教學小費放置朝，但不是會完全針對右下角。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-placement-margin.png)


### <a name="add-content"></a>新增內容

內容可以加入教學小費使用 Content 屬性。 如果沒有顯示比教學提示的大小可讓更多的內容，捲軸將會自動啟用以允許使用者捲動內容區域。 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![範例應用程式與目標儲存教學提示按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 教學提示的內容區域中的核取方塊標記為 [不要在啟動時顯示提示]，再下面是 「 您可以變更提示喜好設定如果您改變心意 」 文字 「 設定 」 的應用程式的 [設定] 頁面的連結。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-content.png)

### <a name="add-buttons"></a>加入按鈕

根據預設，會顯示教學提示的標題旁邊的"X"[關閉] 按鈕的標準。 CloseButtonContent 屬性，在其大小寫 按鈕會移至教學提示底部，您可以自訂 關閉 按鈕。

**注意：沒有 [關閉] 按鈕會出現在 light 關閉已啟用的秘訣**

設定 ActionButtonContent 屬性 （並選擇性地 ActionButtonCommand 和 ActionButtonCommandParameter 屬性），可以加入自訂動作按鈕。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            ActionButtonContent="Disable"
            ActionButtonCommand="DisableAutoSave"
            CloseButtonContent="Got it!">
                <StackPanel>
                    <CheckBox x:Name="HideTipsCheckBox" Content="Don't show tips at start up" IsChecked="{x:Bind HidingTips, Mode=TwoWay}" />
                    <TextBlock>You can change your tip preferences in <Hyperlink NavigateUri="app:/item/SettingsPage">Settings</Hyperlink> if you change your mind.</TextBlock>
                </StackPanel>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![範例應用程式與目標儲存教學提示按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 教學提示的內容區域中的核取方塊標記為 [不要在啟動時顯示提示]，再下面是 「 您可以變更提示喜好設定如果您改變心意 」 文字 「 設定 」 的應用程式的 [設定] 頁面的連結。 底部的教學活動有兩個按鈕、 一個灰色左邊 [停用] 會讀取和讀取在右側的藍色一個 」 吧 ！ 」](../images/teaching-tip-buttons.png)

### <a name="hero-content"></a>Hero 內容

最前線內容的邊緣可以新增至教學提示，藉由設定 HeroContent 屬性。 Hero 內容的位置可以設定在頂部或底部教學提示，HeroContentPlacement 屬性設定。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources> 
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to.">
            <controls:TeachingTip.HeroContent>
                <Image Source="Assets/cloud.png" />
            </controls:TeachingTip.HeroContent>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![範例應用程式與目標儲存教學提示按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 教學提示底部是卡通男性，將檔案放在雲端的框線的框線映像。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-hero-content.png)

### <a name="add-an-icon"></a>加入圖示

圖示可以新增標題和副標題使用 IconSource 屬性旁邊。 建議的圖示大小包括 16px、 24px 和 32px。 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save">
    <Button.Resources>
        <controls:TeachingTip x:Name="AutoSaveTip"
            Target="{x:Bind SaveButton}"
            Title="Saving automatically"
            Subtitle="We save your changes as you go - so you never have to."
            <controls:TeachingTip.IconSource>
                <controls:SymbolIconSource Symbol="Save" />
            </controls:TeachingTip.IconSource>
        </controls:TeachingTip>
    </Button.Resources>
</Button>
```

![範例應用程式與目標儲存教學提示按鈕。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 左邊的標題和副標題是磁碟片圖示。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-icon.png)

### <a name="enable-light-dismiss"></a>啟用 light 關閉

Light 解除功能預設為停用，但它可以啟用教學提示會關閉，例如，當使用者捲動或與應用程式的其他項目互動。 由於這個行為，光關閉提示需要放在可捲動區域時，提示會是最佳的解決方案。 

[關閉] 按鈕將會自動移除 light-dismiss 啟用的教學提示，以識別其光線關閉使用者的行為。 

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    IsLightDismissEnabled="True">
</controls:TeachingTip>
```

![範例應用程式與光線關閉授課右下角中的提示。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。](../images/teaching-tip-light-dismiss.png)

### <a name="escaping-the-xaml-root-bounds"></a>逸出的 xaml 根範圍

在 Windows 上版本 19 H 1 與上述教學提示來逸出 xaml 根和畫面的邊界設定 ShouldConstrainToRootBounds; 屬性。 啟用此屬性時，教導提示不會嘗試保持在 xaml 根，也不在螢幕的界限內，並一律會在設定中的位置 PreferredPlacement 模式。 建議您啟用 IsLightDismissEnabled 屬性，並設定最接近的 xaml 根，以確保使用者的最佳體驗中心的 PreferredPlacement 模式。

在舊版 Windows 中，會忽略這個屬性，並教導提示一律會保持 xaml 根項目的範圍內。

XAML
```XAML
<Button x:Name="SaveButton" Content="Save" />

<controls:TeachingTip x:Name="AutoSaveTip"
    Title="Saving automatically"
    Subtitle="We save your changes as you go - so you never have to."
    PreferredPlacement="BottomRight"
    PlacementMargin="-80,-50,0,0"
    ShouldConstrainToRootBounds="False">
</controls:TeachingTip>
```

![與外部應用程式的右下角教學提示範例應用程式。 提示標題會讀取 「 自動儲存 」 和副標題讀取我們 [儲存] 變更您沿用-讓您完全不需要。 沒有教學提示右上角的 [關閉] 按鈕。](../images/teaching-tip-escape-xaml-root.png)

### <a name="canceling-and-deferring-close"></a>取消和延後關閉

Closing 事件可用來取消及/或延遲的教學提示關閉使用中。 這可用來保持開啟教學提示或允許的動作或自訂動畫發生的時間。 當取消教學提示關閉時，IsOpen 會恢復為 true，不過，它會一直保留 false 在延遲期間。 也可以取消以程式設計方式關閉。 

**注意：如果沒有的放置選項會允許完整顯示教學小費，教學提示會逐一查看其事件開發週期，以強制關閉，而不是顯示，而不存取的 [關閉] 按鈕。 如果應用程式取消 Closing 事件，教學提示可能會保持開啟但沒有可存取的 [關閉] 按鈕。**

XAML
```XAML
<controls:TeachingTip x:Name="EnableNewSettingsTip"
    Title="New ways to protect your privacy!"
    Subtitle="Please close this tip and review our updated privacy policy and privacy settings."
    Closing="OnTipClosing">
</controls:TeachingTip>
```

C#
```C#
public void OnTipClosing(object sender, TeachingTipClosingEventArgs args)
{
    if (args.Reason == TeachingTipCloseReason.CloseButton)
    {
        using(args.GetDeferral())
        {
            bool success = await UpdateUserSettings(User thisUsersID);
            if(!success)
            {
                //We were not able to update the settings! Don't close the tip and display the reason why.
                args.Cancel = true;
                ShowLastErrorMessage();
            }
        }
    }
}
```

## <a name="remarks"></a>備註

### <a name="related-articles"></a>相關文章 

* [對話方塊和飛出視窗](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/index)

### <a name="recommendations"></a>建議
* 秘訣是 impermanent 和不應包含的資訊非常重要的應用程式經驗的選項。 
* 請盡量避免太頻繁顯示教學秘訣。 秘訣教學最有可能每個接收個別的注意，當交錯整個很長的工作階段，或跨多個工作階段。    
* 保持簡潔的祕訣和清除其主題。 研究的使用者，平均而言，只會讀取 3 到 5 個字組和只理解 2-3 個單字，再決定是否要提示進行互動。
* 不保證教學提示的遊戲台協助工具。 對於預測遊樂器輸入的應用程式，請參閱[遊戲台和遠端控制互動]( https://docs.microsoft.com/en-us/windows/uwp/design/input/gamepad-and-remote-interactions#xy-focus-navigation-and-interaction)。 建議您測試的使用所有可能的組態，應用程式的 UI 的每個授課提示的遊戲台存取範圍。
* 當啟用來逸出的 xaml 根教學提示，建議您一併啟用 IsLightDismissEnabled 屬性，並設定最接近的 xaml 根中央的 PreferredPlacement 模式。 

### <a name="reconfiguring-an-open-teaching-tip"></a>重新設定在開啟教學提示

雖然教學工具提示開啟時，會立即生效，可以重新設定部分的內容和屬性。 其他的內容和屬性，例如圖示屬性，動作，並關閉 按鈕時，與重新設定之間 light 關閉，並明確關閉將所有需要教學提示關閉並重新開啟變更這些屬性才會生效。 請注意變更 dismissal 行為，從手動-關閉燈光關閉授課提示開啟時，會導致教學提示，將其 [關閉] 按鈕之前移除 light 關閉行為已啟用，而且提示螢幕上可以維持停滯。
