---
author: Karl-Bridge-Microsoft
Description: "啟用鍵盤存取以使用 Tab 瀏覽和便捷鍵，讓使用者可以利用鍵盤瀏覽整個 UI 元素。"
title: "便捷鍵"
ms.assetid: C2F3F3CE-737F-4652-98B7-5278A462F9D3
label: Access keys
template: detail.hbs
keywords: "便捷鍵, 鍵盤, 協助工具"
translationtype: Human Translation
ms.sourcegitcommit: 2b6b1d7b1755aad4d75a29413d989c6e8112128a
ms.openlocfilehash: dfe89e4d4fd089dde6b7b307325b8fe43de82c10

---

# <a name="access-keys"></a>便捷鍵

不方便使用滑鼠的使用者 (例如受到某種程度運動神經傷害的使用者) 通常依賴鍵盤來瀏覽 app 並與之互動。  XAML 架構可讓您透過 Tab 瀏覽和便捷鍵，提供對 UI 元素的鍵盤存取。

- Tab 瀏覽是基本的鍵盤協助工具能供性 (預設為啟用)，可讓使用者利用鍵盤上的 Tab 鍵和方向鍵，在 UI 元素之間移動焦點。
- 便捷鍵是補充性質的協助工具能供性 (您要在 app 中實作)，可以使用鍵盤功能鍵 (Alt 鍵) 與一或多個英數字元鍵 (通常是與命令相關聯的字母) 的組合來快速存取應用程式命令。 常用的便捷鍵包括 _Alt+F_ 以開啟 [檔案] 功能表，和 _Alt +AL_ 以靠左對齊。  

如需鍵盤瀏覽與協助工具的詳細資訊，請參閱[鍵盤互動](https://msdn.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)與[鍵盤協助工具](https://msdn.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)。 本文假設您了解這些文章中討論的概念。

## <a name="access-key-overview"></a>便捷鍵概觀

便捷鍵可讓使用者利用鍵盤直接叫用按鈕或設定焦點，而不需要重複地按方向鍵或 Tab。 便捷鍵的用意在於能輕鬆地被搜尋，因此您應該將它們直接記載在 UI 中；例如，在控制項上含有便捷鍵的浮動徽章。

![Microsoft Word 中便捷鍵的範例與相關聯的按鍵提示](images/keyboard/accesskeys-keytips.png)

_圖 1：Microsoft Word 中便捷鍵的範例與相關聯的按鍵提示。_

便捷鍵是與 UI 元素相關聯的一或多個英數字元。 例如，Microsoft Word 針對 [常用] 索引標籤使用 _H_，針對 [復原] 按鈕使用 _2_，或是針對 [繪製] 索引標籤使用 _JI_。

**便捷鍵範圍**

便捷鍵屬於特定的範圍。 例如，在圖 1 中，_F_、_H_、_N_ 和 _JI_ 屬於頁面的範圍。  當使用者按下 _H_ 時，範圍會變更為 [常用] 索引標籤的範圍，而其便捷鍵會如圖 2 所示。 便捷鍵 _V_、_FP_、_FF_ 和 _FS_ 屬於 [常用] 索引標籤的範圍。

![Microsoft Word 中 [常用] 索引標籤範圍的便捷鍵範例與相關聯的按鍵提示](images/keyboard/accesskeys-keytips-hometab.png)

_圖 2：Microsoft Word 中 [常用] 索引標籤範圍的便捷鍵範例與相關聯的按鍵提示。_

如果兩個元素屬於不同的範圍，則元素可以使用相同的便捷鍵。 例如，_2_ 是頁面範圍的 [復原] 便捷鍵 (圖 1)，同使也是 [常用] 索引標籤範圍中的 [斜體] (圖 2)。 所有的便捷鍵都屬於預設的範圍，除非另有指定另一個範圍。

**便捷鍵組合**

便捷鍵組合通常是一次按一個按鍵來達成動作，而不是同時按下多個按鍵。 (但其中有例外，我們將在下一節討論。) 達成動作所需的按鍵輸入順序就是「便捷鍵組合」。 使用者按下 Alt 鍵以起始便捷鍵組合。 當使用者按下便捷鍵組合中的最後一個按鍵時，就會叫用便捷鍵。 例如，若要在 Word 中開啟 [檢視] 索引標籤，使用者會按下 _Alt、W_ 便捷鍵組合。

使用者可以在一個便捷鍵組合中叫用數個便捷鍵。 例如，若要在 Word 文件中開啟 [複製格式]，使用者會按下 Alt 以起始組合，然後按下 _H_ 來瀏覽 [常用] 區段並變更便捷鍵範圍，然後按下 _F_，最後是 _P_。_H_ 和 _FP_ 分別為 [常用] 索引標籤和 [複製格式] 的便捷鍵。

有些項目在叫用之後會完成便捷鍵組合 (例如 [複製格式] 按鈕) 而某些則不會 (例如 [常用] 索引標籤)。 叫用便捷鍵會產生執行命令、移動焦點、變更便捷鍵範圍，或某些其他相關聯的動作。

## <a name="access-key-user-interaction"></a>便捷鍵使用者互動

若要了解便捷鍵 API，則必須先了解使用者互動模型。 您可以在下面找到便捷鍵使用者互動模型的摘要：

- 當使用者按下 Alt 鍵時，就會啟動便捷鍵組合，即使焦點位在輸入控制項時亦同。 然後，使用者可以按便捷鍵來叫用相關聯的動作。 這種使用者互動需要您在 UI 中搭配一些視覺能供性，記載可用的便捷鍵，例如當按下 Alt 鍵時會顯示的浮動徽章
- 當使用者同時按下 Alt 鍵加上便捷鍵時，會立即叫用便捷鍵。 這類似於使用 Alt+「便捷鍵」定義的鍵盤快速鍵。 在此情況下，不會顯示便捷鍵視覺能供性。 不過，叫用便捷鍵可能導致變更便捷鍵範圍。 在此情況下，會起始便捷鍵組合，並且顯示新範圍的視覺能供性。
    > [!NOTE]
    > 只有使用單一字元的便捷鍵可以利用這種使用者互動。 Alt+「便捷鍵」組合不支援使用多個字元的便捷鍵。    
- 當多字元便捷鍵共用某些字元時，在使用者按下共用的字元時，便會篩選便捷鍵。 例如，假設有顯示三個便捷鍵：_A1_、_A2_ 和 _C_。如果使用者按下 _A_，則只會顯示 _A1_ 和 _A2_ 便捷鍵，並隱藏 C 的視覺能供性。
- Esc 鍵會移除一層篩選。 例如，如果有便捷鍵為 _B_、_ABC_、_ACD_ 和 _ABD_，而使用者按下 _A_，則只會顯示 _ABC_、_ACD_ 和 _ABD_。 如果使用者接著按下 _B_、則只會顯示 _ABC_ 與 _ABD_。 如果使用者按下 Esc，則會移除一層篩選，並顯示 _ABC_、_ACD_ 和 _ABD_ 便捷鍵。 如果使用者再按一次 Esc，則會再移除另一層篩選，而所有的便捷鍵 - _B_、_ABC_、_ACD_ 和 _ABD_ – 都會啟用，並顯示它們的視覺能供性。
- Esc 鍵瀏覽回先前的範圍。 便捷鍵可能屬於不同的範圍，以便在有許多命令的 app 中能更輕鬆地瀏覽。 便捷鍵組合一律在主要範圍開始。 所有的便捷鍵都屬於主要範圍，除了那些指定為範圍擁有者的特定 UI 元素。 當使用者叫用其為範圍擁有者元素的便捷鍵時，XAML 架構會自動將範圍移動到該元素，並將它加入內部便捷鍵瀏覽堆疊。 Esc 鍵可在各便捷鍵瀏覽堆疊之間向後移動。
- 有數種方式可以關閉便捷鍵組合：
    - 使用者可以按 Alt 來關閉進行中的便捷鍵組合。 請記住，按 Alt 也可以起始便捷鍵組合。
    - 如果便捷鍵在主要範圍中且未經篩選，則 Esc 鍵會關閉便捷鍵組合。
        > [!NOTE]
        > Esc 按鍵輸入也會傳遞至要在該處處理的的 UI 層。
- Tab 鍵會關閉便捷鍵組合，並返回 Tab 瀏覽。
- Enter 鍵會關閉便捷鍵組合，並傳送按鍵輸入到具有焦點的元素。
- 方向鍵會關閉便捷鍵組合，並傳送按鍵輸入到具有焦點的元素。
- 指標向下事件 (例如按一下滑鼠或觸碰) 會關閉便捷鍵組合。
- 根據預設，當叫用便捷鍵之後，會關閉便捷鍵組合。  不過，您可以藉由將 [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 屬性設定為 **false** 來覆寫這個行為。
- 當決定性的有限自動化無法進行時，便會會發生便捷鍵衝突。 便捷鍵衝突令人困擾，但仍可能因為大量的命令、當地語系化問題或便捷鍵的執行階段世代而產生。

 有兩種情況會發生衝突：
 - 當兩個 UI 元素具有相同的便捷鍵值，並屬於相同便捷鍵範圍時。 例如，`button1` 的便捷鍵 _A1_ 和 `button2` 的便捷鍵 _A1_ 均屬於預設範圍。 在此情況下，系統會處理第一個加入到視覺化樹狀結構中的便捷鍵來解決衝突。 其餘的部分將會忽略。
 - 當相同的便捷鍵範圍中有多個計算選項。 例如，_A_ 和 _A1_。 當使用者按下 _A_，系統有兩種選項：叫用 _A_ 便捷鍵，或是繼續並使用 _A1_ 便捷鍵的 A 字元。 在此情況下，系統只會處理自動機制中第一個到達的便捷鍵叫用。 例如，_A_ 和 _A1_，系統只會叫用 _A_ 便捷鍵。
-   當使用者在便捷鍵組合中按下無效的便捷鍵值時，則沒有作用。 在便捷鍵組合中，有兩種類別的按鍵可視為有效便捷鍵：
 - 可結束便捷鍵組合的特殊按鍵：Esc、Alt、方向鍵、Enter 和 Tab。
 - 指派給便捷鍵的英數字元。

## <a name="access-key-apis"></a>便捷鍵 API

為了支援便捷鍵使用者互動，XAML 架構提供 API，如下所述。

**AccessKeyManager**

[AccessKeyManager](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.aspx) 是一種協助程式類別，當顯示或是隱藏便捷鍵時，可用來管理 UI。 [IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) 事件每當 app 輸入和結束便捷鍵組合時便會引發。 您可以查詢 [IsDisplayModeEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabled.aspx) 屬性來判斷視覺能供性是否為顯示或隱藏。  您也可以呼叫 [ExitDisplayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.exitdisplaymode.aspx) 來強制關閉便捷鍵組合。

> [!NOTE]
> 便捷鍵的視覺項目沒有任何內建的實作；您必須提供實作。  

**AccessKey**

[AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 屬性可讓您指定 UIElement 或 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.accesskey.aspx) 上的便捷鍵。 如果兩個元素有相同的便捷鍵和範圍，則只會處理第一個加入到視覺化樹狀結構的元素。

若要確保 XAML 架構會處理便捷鍵，UI 元素必須要能在視覺化樹狀結構中辨識。 如果視覺化樹狀結構中沒有任何元素含有便捷鍵，則不會引發任何便捷鍵事件。

便捷鍵 API 不支援需要兩個按鍵輸入才能產生的字元。 個別的字元必須對應到特定語言原生鍵盤配置上的按鍵。  

**AccessKeyDisplayRequested/Dismissed**

[AccessKeyDisplayRequested](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplayrequested.aspx) 和 [AccessKeyDisplayDismissed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeydisplaydismissed.aspx) 事件會在便捷鍵視覺能供性應該顯示或關閉時引發。 當元素的 [Visibility](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility.aspx) 屬性設定為 **Collapsed** 時，不會引發這些事件。 每次使用者按下便捷鍵所使用的字元時，在便捷鍵組合期間會引發 AccessKeyDisplayRequested 事件。 例如，如果有一設定為 _AB_ 的便捷鍵，在使用者按下 Alt 就會引發此事件，並在使用者按下 _A_ 時再次引發。當使用者按下 _B_ 時，會引發 AccessKeyDisplayDismissed 事件

**AccessKeyInvoked**

[AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) 事件會在使用者到達便捷鍵的最後一個字元時引發。 便捷鍵可以有一或數個字元。 例如，若有便捷鍵 _A_ 和 _BC_，當使用者按下 _Alt、A_ 或 _Alt、B、C_時，就會引發事件，但不會在使用者僅按下 _Alt、B_ 時引發。這個事件在按鍵按下時就會引發，而非放開時。

**IsAccessKeyScope**

[IsAccessKeyScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.isaccesskeyscope.aspx) 屬性可讓您指定其為便捷鍵範圍根項目的 UIElement。 對於此元素會引發 AccessKeyDisplayRequested 事件，但其子項不會。 當使用者叫用此元素時，XAML 架構會自動變更範圍，並在其子項上引發 AccessKeyDisplayRequested 事件，以及在其他 UI 元素上 (包括父項) 引發 AccessKeyDisplayDismissed 事件。  當範圍變更後，不會結束便捷鍵組合。

**AccessKeyScopeOwner**

若要讓元素參與另一個元素 (來源) 的範圍，而另一個元素在視覺化樹狀結構中並非該元素的父項，您可以設定 [AccessKeyScopeOwner](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyscopeowner.aspx) 屬性。 繫結到 AccessKeyScopeOwner 屬性的元素必須將 IsAccessKeyScope 設定為 **true**。 否則，會擲回例外狀況。

**ExitDisplayModeOnAccessKeyInvoked**

根據預設，當叫用便捷鍵且元素不是範圍擁有者時，就會完成便捷鍵組合，並引發 [AccessKeyManager.IsDisplayModeEnabledChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeymanager.isdisplaymodeenabledchanged.aspx) 事件。 您可以將 [ExitDisplayModeOnAccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 屬性設定為 **false** 來覆寫此行為，並防止便捷鍵組合在叫用後結束。 ([UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.exitdisplaymodeonaccesskeyinvoked.aspx) 和 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.exitdisplaymodeonaccesskeyinvoked.aspx) 都有這項屬性)。

> [!NOTE]
> 如果元素是範圍擁有者 (`IsAccessKeyScope="True"`)，app 進入新的便捷鍵範圍時不會引發 IsDisplayModeEnabledChanged 事件。

**當地語系化**

便捷鍵可以使用多種語言當地語系化，並使用 [ResourceLoader](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.resources.resourceloader.aspx) API 在執行階段載入。

## <a name="control-patterns-used-when-an-access-key-is-invoked"></a>叫用便捷鍵時使用的控制項模式

控制項模式是公開常用控制項功能的介面實作；例如，按鈕會實作 **Invoke** 控制項模式，而這會引發 **Click** 事件。 當叫用便捷鍵時，XAML 架構會查詢叫用的元素是否實作控制項模式，並在有實作時執行控制項模式。 如果元素有一個以上的控制項模式，則只會叫用其中一個，其餘的部分將會忽略。 控制項模式會以下列順序來搜尋：

1.  叫用。 例如 Button。
2.  切換。 例如 Checkbox。
3.  選項。 例如 RadioButton。
4.  展開/摺疊。 例如 ComboBox。

如果找不到控制項模式，便捷鍵叫用會顯示為 no-op，並且會記錄偵錯訊息以協助您偵錯此情況：「找不到此元件的自動化模式。 請在 AccessKeyInvoked 的事件處理常式中實作所需的行為。 將事件處理常式中的 Handled 設定為 true 將會隱藏此訊息。」

> [!NOTE]
> Visual Studio 的 [偵錯設定] 中的偵錯程式 [應用程式處理類型] 必須為 [混合 (Managed 和原生)] 或 [原生] 才能看到此訊息。

如果您不想要讓便捷鍵執行其預設控制項模式，或元素不具有控制項模式，則您應該處理 [AccessKeyInvoked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskeyinvoked.aspx) 事件，並實作所需的行為。
```csharp
private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
{
    args.Handled = true;
    //Do something
}
```

如需控制項模式的詳細資訊，請參閱[使用者介面自動化控制項模式概觀](https://msdn.microsoft.com/library/windows/desktop/ee671194.aspx)。

## <a name="access-keys-and-narrator"></a>便捷鍵和朗讀程式

Windows 執行階段具有使用者介面自動化提供者，它會公開 Microsoft UI 自動化元素上的屬性。 這些屬性可讓使用者介面自動化用戶端應用程式探索關於使用者介面部分的相關資訊。 [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) 屬性可讓用戶端 (例如朗讀程式) 探索與元素相關聯的便捷鍵。 每當元素取得焦點時，朗讀程式將會讀取此屬性。 如果 AutomationProperties.AccessKey 不具有值，則 XAML 架構會從 UIElement 或 TextElement 傳回 [AccessKey](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.accesskey.aspx) 屬性值。 如果 AccessKey 屬性已經有值，您就不需要設定 AutomationProperties.AccessKey。

## <a name="example-access-key-for-button"></a>範例：按鈕的便捷鍵

本範例說明如何為按鈕建立便捷鍵。 範例使用 Tooltip 做為視覺能供性，以實作包含便捷鍵的浮動徽章。

> [!NOTE]
> Tooltip 是為了簡單起見而使用，但建議您使用如 [Popup](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.popup.aspx) 類別建立您自己的控制項來顯示它。

XAML 架構會自動呼叫 Click 事件的處理常式，因此您不需要處理 AccessKeyInvoked 事件。 範例使用 [AccessKeyDisplayRequestedEventArgs.PressedKeys](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.accesskeydisplayrequestedeventargs.pressedkeys.aspx) 屬性，只針對叫用便捷鍵剩餘的字元提供視覺能供性。 例如，如果有顯示三個便捷鍵：_A1_、_A2_ 和 _C_，而使用者按下 _A_，則只會有 _A1_ 和 _A2_ 便捷鍵未經篩選，並且會顯示為 _1_ 和 _2_，而非 _A1_ 和 _A2_。

```xaml
<StackPanel
        VerticalAlignment="Center"
        HorizontalAlignment="Center"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Button Content="Press"
                AccessKey="PB"
                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                Click="DoSomething" />
        <TextBlock Text="" x:Name="textBlock" />
    </StackPanel>
```

```csharp
 public sealed partial class ButtonSample : Page
    {
        public ButtonSample()
        {
            this.InitializeComponent();
        }

        private void DoSomething(object sender, RoutedEventArgs args)
        {
            textBlock.Text = "Access Key is working!";
        }

        private void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(sender, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        private void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            var tooltip = ToolTipService.GetToolTip(sender) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(sender, null);
            }
        }
    }
```

## <a name="example-scoped-access-keys"></a>範例：限定範圍的便捷鍵

本範例說明如何建立限定範圍的便捷鍵。 PivotItem 的 IsAccessKeyScope 屬性可在使用者按下 Alt 時，避免顯示 PivotItem 子元素的便捷鍵。 這些便捷鍵只有在使用者叫用 PivotItem 時才會顯示，因為 XAML 架構會自動切換範圍。 架構也會隱藏其他範圍的便捷鍵。

本範例也會示範如何處理 AccessKeyInvoked 事件。 PivotItem 不會實作任何控制項模式，因此 XAML 架構預設不會叫用任何動作。 這個實作會示範如何使用便捷鍵選取已叫用的 PivotItem。

最後，範例會示範 IsDisplayModeChanged 事件，當顯示模式變更時，您可以在其中執行一些動作。 在本範例中，Pivot 控制項已摺疊，直到使用者按下 Alt 為止。 當使用者完成與 Pivot 的互動之後，它會再次摺疊。 您可以使用 IsDisplayModeEnabled 來檢查便捷鍵顯示模式為啟用或停用。

```xaml   
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Pivot x:Name="MyPivot" VerticalAlignment="Center" HorizontalAlignment="Center" >
            <Pivot.Items>
                <PivotItem
                    x:Name="PivotItem1"
                    AccessKey="A"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    IsAccessKeyScope="True">
                    <PivotItem.Header>
                        <TextBlock Text="A Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal" >
                        <Button Content="ButtonAA" AccessKey="A"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested" />
                        <Button Content="ButtonAD1" AccessKey="D1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button Content="ButtonAD2" AccessKey="D2"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
                <PivotItem
                    x:Name="PivotItem2"
                    AccessKeyInvoked="OnAccessKeyInvoked"
                    AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                    AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"
                    AccessKey="B"
                    IsAccessKeyScope="true">
                    <PivotItem.Header>
                        <TextBlock Text="B Options"/>
                    </PivotItem.Header>
                    <StackPanel Orientation="Horizontal">
                        <Button AccessKey="B" Content="ButtonBB"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F1" Content="ButtonBF1"
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"  />
                        <Button AccessKey="F2" Content="ButtonBF2"  
                                AccessKeyDisplayDismissed="OnAccessKeyDisplayDismissed"
                                AccessKeyDisplayRequested="OnAccessKeyDisplayRequested"/>
                    </StackPanel>
                </PivotItem>
            </Pivot.Items>
        </Pivot>
    </Grid>
```

```csharp
public sealed partial class ScopedAccessKeys : Page
    {
        public ScopedAccessKeys()
        {
            this.InitializeComponent();
            AccessKeyManager.IsDisplayModeEnabledChanged += OnDisplayModeEnabledChanged;
            this.Loaded += OnLoaded;
        }

        void OnLoaded(object sender, object e)
        {
            //To let the framework discover the access keys, the elements should be realized
            //on the visual tree. If there are no elements in the visual
            //tree with access key, the framework won't raise the events.
            //In this sample, if you define the Pivot as collapsed on the constructor, the Pivot
            //will have a lazy loading and the access keys won't be enabled.
            //For this reason, we make it visible when creating the object
            //and we collapse it when we load the page.
            MyPivot.Visibility = Visibility.Collapsed;
        }

        void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
        {
            args.Handled = true;
            MyPivot.SelectedItem = sender as PivotItem;
        }
        void OnDisplayModeEnabledChanged(object sender, object e)
        {
            if (AccessKeyManager.IsDisplayModeEnabled)
            {
                MyPivot.Visibility = Visibility.Visible;
            }
            else
            {
                MyPivot.Visibility = Visibility.Collapsed;

            }
        }

        DependencyObject AdjustTarget(UIElement sender)
        {
            DependencyObject target = sender;
            if (sender is PivotItem)
            {
                PivotItem pivotItem = target as PivotItem;
                target = (sender as PivotItem).Header as TextBlock;
            }
            return target;
        }

        void OnAccessKeyDisplayRequested(UIElement sender, AccessKeyDisplayRequestedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);
            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;

            if (tooltip == null)
            {
                tooltip = new ToolTip();
                tooltip.Background = new SolidColorBrush(Windows.UI.Colors.Black);
                tooltip.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                tooltip.Padding = new Thickness(4, 4, 4, 4);
                tooltip.VerticalOffset = -20;
                tooltip.Placement = PlacementMode.Bottom;
                ToolTipService.SetToolTip(target, tooltip);
            }

            if (string.IsNullOrEmpty(args.PressedKeys))
            {
                tooltip.Content = sender.AccessKey;
            }
            else
            {
                tooltip.Content = sender.AccessKey.Remove(0, args.PressedKeys.Length);
            }

            tooltip.IsOpen = true;
        }
        void OnAccessKeyDisplayDismissed(UIElement sender, AccessKeyDisplayDismissedEventArgs args)
        {
            DependencyObject target = AdjustTarget(sender);

            var tooltip = ToolTipService.GetToolTip(target) as ToolTip;
            if (tooltip != null)
            {
                tooltip.IsOpen = false;
                //Fix to avoid show tooltip with mouse
                ToolTipService.SetToolTip(target, null);
            }
        }
    }
```



<!--HONumber=Dec16_HO1-->


