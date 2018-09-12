---
author: Karl-Bridge-Microsoft
Description: Learn how accelerator keys can improve the usability and accessibility of UWP apps.
title: 鍵盤快速操作
label: Keyboard accelerators
template: detail.hbs
keywords: 鍵盤, 快速操作, 快速鍵, 鍵盤快速鍵, 協助工具, 瀏覽, 焦點, 文字, 輸入, 使用者互動, 遊戲台, 遠端
ms.author: kbridge
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: chigy
design-contact: miguelrb
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: ce84debc3422f923c7c88aae1fa216665ef1ef0f
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2018
ms.locfileid: "3932863"
---
# <a name="keyboard-accelerators"></a>鍵盤快速操作

![Surface 鍵盤](images/accelerators/accelerators_hero2.png)

快速鍵 (鍵盤快速鍵) 為鍵盤捷徑，提供直覺的操作方式，讓使用者不需瀏覽應用程式 UI 也能叫用常用動作或命令，藉以改善 Windows 應用程式的可用性和協助工具。

請參閱[便捷鍵](access-keys.md)主題，以了解有關使用鍵盤快速鍵瀏覽 Windows 應用程式 UI 的詳細資訊。

> [!NOTE]
> 鍵盤對身障使用者而言是不可或缺的工具 (請參閱[鍵盤協助工具](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility))，對於希望透過它而能更有效率地與應用程式互動的使用者而言，也是重要工具。

## <a name="overview"></a>概觀

快速鍵通常包括功能鍵 F1 到 F12，或是一些標準按鍵與一個或多個輔助按鍵 (CTRL、Shift) 配對的組合。

> [!NOTE]
> UWP 平台控制項會有內建鍵盤快速鍵。 例如，ListView 支援用於選取清單中所有項目的 Ctrl+A，而 RichEditBox 支援在文字方塊中插入 Tab 的 Ctrl+Tab。 這些內建鍵盤快速鍵稱為**控制項快速鍵**，只有在焦點位於元素或其中一個子系時才會執行。 使用此處討論之鍵盤快速鍵 API 所定義的鍵盤快速鍵稱為**應用程式快速鍵**。

鍵盤快速鍵並非所有的動作都適用，但通常與功能表中公開的命令有關聯 (而且應該以功能表項目內容來指定)。 快速鍵也可以和沒有對等功能表項目的動作建立關聯。 不過，使用者依賴應用程式的功能表來探索並了解可用的命令集，因此必須盡可能嘗試讓探索快速鍵的方式變得簡單 (使用標籤或已建立的模式會很有幫助)。

![功能表項目標籤中描述的鍵盤快速鍵](images/accelerators/accelerators_menuitemlabel.png)  
*功能表項目標籤中描述的鍵盤快速鍵*

## <a name="when-to-use-keyboard-accelerators"></a>使用鍵盤快速鍵的時機

我們建議您在 UI 任何適當的地方指定鍵盤快速鍵，並在所有自訂控制項中支援快速鍵。

- 鍵盤快速鍵讓您的應用程式更容易存取，適用於運動功能障礙的使用者，包括一次只能按一個按鍵或使用滑鼠有困難的使用者。**

  設計良好的鍵盤 UI 是軟體協助工具的一個重要層面。 它讓視障使用者或受到某種程度運動神經傷害的使用者能夠瀏覽應用程式並與應用程式的功能互動。 這類使用者有可能無法使用滑鼠，因而必須仰賴像鍵盤增強功能工具、螢幕小鍵盤、螢幕放大機、螢幕助讀程式以及語音輸入公用程式這類協助技術。 對於這些使用者，完整命令涵蓋範圍很重要。

- 鍵盤快速鍵讓您的應用程式更有用，適用於偏好使用鍵盤進行互動的進階使用者。

  經驗豐富的使用者通常極度偏好使用鍵盤，因為鍵盤式命令的輸入更快速，而且不需要從鍵盤移開其雙手。 對於這些使用者而言，效率與一致性非常重要；完整性只對最常用的命令很重要。

## <a name="specify-a-keyboard-accelerator"></a>指定鍵盤快速鍵

使用 [KeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.input.keyboardaccelerator.-ctor) API 來建立 UWP app 中的鍵盤快速鍵。 使用這些 API 後，您就不需要處理多個 KeyDown 事件來偵測按下的按鍵組合，而且您可以將應用程式資源中的快速鍵當地語系化。

建議您在應用程式中設定最常用動作的鍵盤快速鍵，並使用功能表項目標籤或工具提示來說明這些快速鍵。 在此範例中，我們僅宣告 [重新命名] 和 [複製] 命令的鍵盤快速鍵。

``` xaml
<CommandBar Margin="0,200" AccessKey="M">
  <AppBarButton 
    Icon="Share" 
    Label="Share" 
    Click="OnShare" 
    AccessKey="S" />
  <AppBarButton 
    Icon="Copy" 
    Label="Copy" 
    ToolTipService.ToolTip="Copy (Ctrl+C)" 
    Click="OnCopy" 
    AccessKey="C">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="Control" 
        Key="C" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="Delete" 
    Label="Delete" 
    Click="OnDelete" 
    AccessKey="D" />
  <AppBarSeparator/>
  <AppBarButton 
    Icon="Rename" 
    Label="Rename" 
    ToolTipService.ToolTip="Rename (F2)" 
    Click="OnRename" 
    AccessKey="R">
    <AppBarButton.KeyboardAccelerators>
      <KeyboardAccelerator 
        Modifiers="None" Key="F2" />
    </AppBarButton.KeyboardAccelerators>
  </AppBarButton>

  <AppBarButton 
    Icon="SelectAll" 
    Label="Select" 
    Click="OnSelect" 
    AccessKey="A" />
  
  <CommandBar.SecondaryCommands>
    <AppBarButton 
      Icon="OpenWith" 
      Label="Sources" 
      AccessKey="S">
      <AppBarButton.Flyout>
        <MenuFlyout>
          <ToggleMenuFlyoutItem Text="OneDrive" />
          <ToggleMenuFlyoutItem Text="Contacts" />
          <ToggleMenuFlyoutItem Text="Photos"/>
          <ToggleMenuFlyoutItem Text="Videos"/>
        </MenuFlyout>
      </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarToggleButton 
      Icon="Save" 
      Label="Auto Save" 
      IsChecked="True" 
      AccessKey="A"/>
  </CommandBar.SecondaryCommands>

</CommandBar>
```

![工具提示中描述的鍵盤快速鍵](images/accelerators/accelerators_tooltip.png)  
***工具提示中描述的鍵盤快速鍵***

[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement) 物件具有 [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 集合 [KeyboardAccelerators](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAccelerators)，您可在其他指定自訂 KeyboardAccelerator 物件，並定義鍵盤快速鍵的按鍵輸入：

-   **[Key](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Key)** - 鍵盤快速鍵使用的 [VirtualKey](https://docs.microsoft.com/uwp/api/windows.system.virtualkey)。

-   **[Modifiers](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Modifiers)** – 鍵盤快速鍵使用的 [VirtualKeyModifiers](https://docs.microsoft.com/uwp/api/windows.system.virtualkeymodifiers)。 如果 Modifiers 未設定，則預設值為 None。

> [!NOTE]
> 支援單一按鍵 (A、Delete、F2、空格鍵、Esc、多媒體鍵) 快速鍵以及多按鍵快速鍵 (Ctrl+Shift+M)。 不過，不支援遊戲台虛擬按鍵。

## <a name="scoped-accelerators"></a>限定範圍的快速鍵

有些快速鍵僅適用於特定範圍，而其他快速鍵則可在整個應用程式中使用。

例如，Microsoft Outlook 包含下列快速鍵：
-   Ctrl+B、Ctrl+I 和 ESC 只能在傳送電子郵件表單的範圍使用
-   Ctrl+1 和 Ctrl+2 適用於整個應用程式

### <a name="context-menus"></a>操作功能表

操作功能表動作只會影響特定區域或元素，例如文字編輯器中選取的字元，或是播放清單中的歌曲。 基於這個原因，我們建議將操作功能表項目的鍵盤快速鍵範圍設為操作功能表的上層。

使用 [ScopeOwner](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.ScopeOwner) 屬性來指定鍵盤快速鍵的範圍。 此程式碼示範如何在 ListView 上實作鍵盤快速鍵已限定範圍的操作功能表：

``` xaml
<ListView x:Name="MyList">
  <ListView.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Share" Icon="Share"/>
      <MenuFlyoutItem Text="Copy" Icon="Copy">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="Control" 
            Key="C" 
            ScopeOwner="{x:Bind MyList }" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Delete" Icon="Delete" />
      <MenuFlyoutSeparator />
      
      <MenuFlyoutItem Text="Rename">
        <MenuFlyoutItem.KeyboardAccelerators>
          <KeyboardAccelerator 
            Modifiers="None" 
            Key="F2" 
            ScopeOwner="{x:Bind MyList}" />
        </MenuFlyoutItem.KeyboardAccelerators>
      </MenuFlyoutItem>
      
      <MenuFlyoutItem Text="Select" />
    </MenuFlyout>
    
  </ListView.ContextFlyout>
    
  <ListViewItem>Track 1</ListViewItem>
  <ListViewItem>Alternative Track 1</ListViewItem>

</ListView>
```

MenuFlyoutItem.KeyboardAccelerators 元素的 ScopeOwner 會將快速鍵標示為已限定範圍，而不是全域 (預設為 null 或全域)。 如需詳細資訊，請參閱本主題稍後的**解析快速鍵**一節。

## <a name="invoke-a-keyboard-accelerator"></a>叫用鍵盤快速鍵 

[KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 物件利用[使用者介面自動化 (UIA) 控制項模式](https://msdn.microsoft.com/library/windows/desktop/ee671194(v=vs.85).aspx)，在叫用快速鍵時執行動作 。

UIA [控制項模式] 會公開常見的控制項功能。 例如，Button 控制項實作[叫用](https://msdn.microsoft.com/library/windows/desktop/ee671279(v=vs.85).aspx)控制項模式來支援 Click 事件 (控制項通常是藉由按一下、按兩下，或是按下 Enter、預先定義的鍵盤快速鍵或一些其他按鍵輸入組合來叫用)。 使用鍵盤快速鍵時叫用控制項時，XAML 架構會查詢控制項是否實作叫用控制項模式，若是如此則加以啟動 (不需要接聽 KeyboardAcceleratorInvoked 事件)。

在下列範例中，因為按鈕實作叫用模式，Control+S 會觸發 Click 事件按一下。

``` xaml 
<Button Content="Save" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator Key="S" Modifiers="Control" />
  </Button.KeyboardAccelerators>
</Button>
```

如果元素實作多個控制項模式，則只有其中一個可以透過快速鍵來啟動。 控制項模式的優先順序如下：
1.  叫用 (Button)
2.  切換 (Checkbox)
3.  選取 (ListView)
4.  展開/摺疊 (ComboBox) 

如果沒有識別出相符項目，則快速鍵無效，系統會提供偵錯訊息 (「找不到此元件的自動化模式。 請在 Invoked 事件中實作所有想要的行為。 將事件處理常式中的 Handled 設定為 true 將會隱藏此訊息。」)

## <a name="custom-keyboard-accelerator-behavior"></a>自訂鍵盤快速鍵行為

執行快速鍵時會引發 [KeyboardAccelerator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator) 物件的 Invoked 事件。 [KeyboardAcceleratorInvokedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs) 事件物件包含下列屬性：
- **Handled** (布林值)：將此屬性設定為 true 可防止事件觸發控制項模式，並停止加速鍵執行事件反昇。 預設值為 false。
- **Element** (DependencyObject)：包含快速鍵的物件。

以下示範如何定義鍵盤快速鍵的集合，以及如何處理 Invoked 事件。

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" Modifiers="Control,Shift" Invoked="SelectAllInvoked" />
    <KeyboardAccelerator Key="F5" Invoked="RefreshInvoked"  />
  </ListView.KeyboardAccelerators>
</ListView>   
```

``` csharp
void SelectAllInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  CustomSelectAll(MyListView);
  args.Handled = true;
}

void RefreshInvoked(KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
  Refresh(MyListView);
  args.Handled = true;
}
```

## <a name="disable-a-keyboard-accelerator"></a>停用鍵盤快速鍵 

如果停用控制項，也會停用相關聯的快速鍵。 下列範例中，ListView 的 IsEnabled 屬性已設定為 false，因此無法叫用相關聯的 Control+A 快速鍵。

``` xaml
<ListView >
  <ListView.KeyboardAccelerators>
    <KeyboardAccelerator Key="A" 
      Modifiers="Control"
      Invoked="CustomListViewSelecAllInvoked" />
  </ListView.KeyboardAccelerators>
  
  <TextBox>
    <TextBox.KeyboardAccelerators>
      <KeyboardAccelerator 
        Key="A" 
        Modifiers="Control" 
        Invoked="CustomTextSelecAllInvoked" 
        IsEnabled="False" />
    </TextBox.KeyboardAccelerators>
  </TextBox>

<ListView>
```

父控制項和子控制項可以共用同一個快速鍵。 此時，即使子控制項擁有焦點且其快速鍵已停用，還是可以叫用父控制項。

## <a name="screen-readers-and-keyboard-accelerators"></a>螢幕助讀程式和鍵盤快速鍵 

螢幕助讀程式 (例如朗讀器) 可以向使用者播報鍵盤快速鍵按鍵組合。 根據預設，這是每個輔助按鍵 (依照 VirtualModifiers 列舉順序) 後面跟著按鍵 (並以 "+" 加號分隔)。 您可以透過 [AcceleratorKey](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.AcceleratorKeyProperty) AutomationProperties 附加屬性自訂此播報方式。 如果指定了多個快速鍵，只會播報的第一個快速鍵。

在此範例中，AutomationProperty.AcceleratorKey 會傳回字串 "Control+Shift+A"：

``` xaml
<ListView x:Name="MyListView">
  <ListView.KeyboardAccelerators>

    <KeyboardAccelerator 
      Key="A" 
      Modifiers="Control,Shift" 
      Invoked="CustomSelectAllInvoked" />
      
    <KeyboardAccelerator 
      Key="F5" 
      Modifiers="None" 
      Invoked="RefreshInvoked" />

  </ListView.KeyboardAccelerators>

</ListView>   
```

> [!NOTE] 
> 設定 AutomationProperties.AcceleratorKey 無法啟用鍵盤功能，只是向 UIA 架構指出使用了哪些按鍵。

## <a name="common-keyboard-accelerators"></a>常見鍵盤快速操作

建議您讓鍵盤快速鍵在所有 UWP 應用程式之間都是一致的。 使用者必須記憶鍵盤快速鍵，並預期相同 (或類似) 結果。

由於功能在應用程式之間各有不同，此建議不一定可行。

| **編輯** | **常見鍵盤快速操作** |
| ------------- | ----------------------------------- |
| 開始編輯模式 | Ctrl + E |
| 選取焦點所在控制項或視窗中的所有項目 | Ctrl + A |
| 搜尋和取代 | Ctrl + H |
| 復原 | Ctrl + Z |
| 重做 | Ctrl + Y |
| 將選取範圍刪除後複製到剪貼簿 | Ctrl + X |
| 將選取範圍複製到剪貼簿 | Ctrl + C、Ctrl + Insert |
| 貼上剪貼簿的內容 | Ctrl + V、Shift + Insert |
| 貼上剪貼簿的內容 (提供選項) | Ctrl + Alt + V |
| 重新命名項目 | F2 |
| 新增項目 | Ctrl + N |
| 新增次要項目 | Ctrl + Shift + N |
| 刪除選取的項目 (可以復原) | Del、Ctrl+D |
| 刪除選取的項目 (無法復原) | Shift + Del |
| 粗體 | Ctrl + B |
| 底線 | Ctrl + U |
| 斜體 | Ctrl + I |

| **瀏覽** | |
| ------------- | ----------------------------------- |
| 尋找焦點所在控制項或視窗中的內容 | Ctrl + F |
| 移至下一個搜尋結果 | F3 |

| **其他動作** | |
| ------------- | ----------------------------------- |
| 新增我的最愛 | Ctrl + D | 
| 重新整理 | F5 或 Ctrl + R | 
| 放大 | Ctrl + + | 
| 縮小 | Ctrl + - | 
| 縮放至預設檢視大小 | Ctrl + 0 | 
| 儲存 | Ctrl + S | 
| 關閉 | Ctrl + W | 
| 列印 | Ctrl + P | 

請注意，有些組合不適用於 Windows 的當地語系化版本。 例如，在 Windows 的西班牙文版本中，設定粗體要使用 Ctrl+N 而不是 Ctrl+B。 如果應用程式已當地語系化，建議您提供當地語系化的鍵盤快速鍵。

## <a name="usability-affordances-for-keyboard-accelerators"></a>鍵盤快速操作的可用性能供性

### <a name="tooltips"></a>工具提示

鍵盤快速操作通常不會直接在您 UWP 應用程式的 UI 中描述，您可以透過[工具提示](../controls-and-patterns/tooltips.md)改善發現性。工具提示會在使用者將焦點移動到控制項、按住不放，或滑鼠指標在控制項上暫留時自動顯示。 工具提示可識別控制項是否有相關聯的鍵盤快速操作，以及快速按鍵組合為何 (若有的話)。

**Windows 10，版本 1803年 （2018 年 4 月更新） 和更新版本**

根據預設，當宣告鍵盤快速鍵時，（除了[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)和[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)） 的所有控制項都顯示對應的按鍵組合在工具提示中。

> [!NOTE] 
> 如果控制項有一個以上的快速鍵定義，會呈現的第一個。

![快速鍵工具提示](images/accelerators/accelerators_tooltip_savebutton_small.png)

*工具提示中的快速按鍵組合*

[按鈕](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button)、 [AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)，及[AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton)物件，鍵盤快速操作會附加到控制項的預設工具提示。 適用於[MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton)和[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)） 物件，鍵盤快速鍵會顯示飛出視窗文字。

> [!NOTE]
> 指定工具提示 （請參閱 Button1 在下列範例中） 會覆寫這個行為。

```xaml
<StackPanel x:Name="Container" Grid.Row="0" Background="AliceBlue">
    <Button Content="Button1" Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto" 
            ToolTipService.ToolTip="Tooltip">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="A" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button2"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="B" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
    <Button Content="Button3"  Margin="20"
            Click="OnSave" 
            KeyboardAcceleratorPlacementMode="Auto">
        <Button.KeyboardAccelerators>
            <KeyboardAccelerator  Key="C" Modifiers="Windows"/>
        </Button.KeyboardAccelerators>
    </Button>
</StackPanel>
```

![快速鍵工具提示](images/accelerators/accelerators-button-small.png)

*附加到按鈕的預設工具提示的快速按鍵組合*

```xaml
<AppBarButton Icon="Save" Label="Save">
    <AppBarButton.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control"/>
    </AppBarButton.KeyboardAccelerators>
</AppBarButton>
```

![快速鍵工具提示](images/accelerators/accelerators-appbarbutton-small.png)

*附加到 AppBarButton 的預設工具提示的快速按鍵組合*

```xaml
<AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
    <AppBarButton.Flyout>
        <MenuFlyout>
            <MenuFlyoutItem AccessKey="A" Icon="Refresh" Text="Refresh A">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="R" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </MenuFlyoutItem>
            <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
            <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
            <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            <ToggleMenuFlyoutItem AccessKey="E" Icon="Globe" Text="ToggleMe">
                <MenuFlyoutItem.KeyboardAccelerators>
                    <KeyboardAccelerator Key="Q" Modifiers="Control"/>
                </MenuFlyoutItem.KeyboardAccelerators>
            </ToggleMenuFlyoutItem>
        </MenuFlyout>
    </AppBarButton.Flyout>
</AppBarButton>
```

![快速鍵工具提示](images/accelerators/accelerators-appbar-menuflyoutitem-small.png)

*附加到 MenuFlyoutItem 的文字的快速按鍵組合*

藉由使用 [KeyboardAcceleratorPlacementMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.KeyboardAcceleratorPlacementMode) 控制展示行為，其接受兩種值：[Auto](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode) 或 [Hidden](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardacceleratorplacementmode)。    

```xaml
<Button Content="Save" Click="OnSave" KeyboardAcceleratorPlacementMode="Auto">
    <Button.KeyboardAccelerators>
        <KeyboardAccelerator Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
</Button>
```

在某些案例中，您可能需要在相對於其他元素的位置顯示工具提示 (通常是容器物件)。 例如，使用 Pivot 標頭顯示 PivotItem 工具提示的 Pivot 控制項。 

在這裡，我們示範如何使用 KeyboardAcceleratorPlacementTarget 屬性顯示附帶 Grid 容器之 Save 按鈕 (而非按鈕) 的鍵盤快速按鍵組合。

```xaml
<Grid x:Name="Container" Padding="30">
  <Button Content="Save"
    Click="OnSave"
    KeyboardAcceleratorPlacementMode="Auto"
    KeyboardAcceleratorPlacementTarget="{x:Bind Container}">
    <Button.KeyboardAccelerators>
      <KeyboardAccelerator  Key="S" Modifiers="Control" />
    </Button.KeyboardAccelerators>
  </Button>
</Grid>
```

### <a name="labels"></a>標籤

在某些案例中，我們建議使用控制項的標籤識別控制項是否有相關聯的鍵盤快速操作，以及快速按鍵組合為何 (若有的話)。 

某些平台控制項會根據預設執行此作業，特別是 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem) 和 [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem) 物件，[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 和 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 則會在出現在 [CommandBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.commandbar) 的溢位功能表時執行此作業。

![功能表項目標籤中描述的鍵盤快速鍵](images/accelerators/accelerators_menuitemlabel.png)  
*功能表項目標籤中描述的鍵盤快速鍵*

您可以透過 [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyoutItem)、[ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem)、[AppBarButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton) 和 [AppBarToggleButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbartogglebutton) 控制項的 [KeyboardAcceleratorTextOverride](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.appbarbutton.KeyboardAcceleratorTextOverride) 屬性覆寫標籤的預設快速操作文字 (若沒有文字則可使用單一空格鍵)。 

> [!NOTE] 
> 若系統無法偵測到已連結的鍵盤 (您可以透過 [KeyboardPresent](https://docs.microsoft.com/uwp/api/windows.devices.input.keyboardcapabilities.KeyboardPresent) 屬性檢查)，便不會顯示覆寫文字。

## <a name="advanced-concepts"></a>進階概念

以下我們來檢閱鍵盤快速鍵的一些低階層面。

### <a name="when-an-accelerator-is-invoked"></a>叫用快速鍵時

加速鍵由兩種類型的按鍵組成：輔助按鍵和非輔助按鍵。 輔助按鍵包括 Shift、Menu、Control 以及 Windows 鍵，這些按鍵透過 [VirtualKeyModifiers](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.System.VirtualKeyModifiers) 來公開。 非輔助按鍵是任何虛擬按鍵，例如 Delete、F3、空格鍵、Esc，以及所有英數字元與標點符號按鍵。 當使用者按住一個或多個輔助按鍵不放再按下非輔助按鍵時，會叫用鍵盤快速鍵。 例如，如果使用者按下 Ctrl+Shift+M，當使用者按 M 時，架構會檢查輔助按鍵 (Ctrl 和 Shift)，若有，即引發快速鍵。

> [!NOTE]
> 依設計，快速鍵會自動重複 (例如，當使用者按下 Ctrl+Shift 後再按住 M 時，快速鍵重複叫用直到放開 M 為止)。 無法修改這種行為。

### <a name="input-event-priority"></a>輸入事件優先順序
輸入事件會依特定順序出現，您可根據應用程式需求加以攔截並處理。 

#### <a name="the-keydownkeyup-bubbling-event"></a>KeyDown/KeyUp 反昇事件 

在 XAML 中，處理按鍵輸入就好像只有一個輸入反昇管線一樣。 KeyDown/KeyUp 事件和字元輸入使用這個輸入管線。 例如，如果元素具有焦點且使用者按下按鍵，就會在元素上引發 KeyDown 事件，後面跟著引發元素的上層，繼而依此沿樹狀結構向上類推，直到 args.Handled 屬性是 true 為止。

有些控制項也會使用 KeyDown 事件實作內建控制項快速鍵。 當控制項有鍵盤快速鍵時，會處理 KeyDown 事件，這表示不會有 KeyDown 事件反昇。 例如，RichEditBox 支援使用 Ctrl+C 複製。 按下 Ctrl 時，會引發 KeyDown 事件並執行事件反昇，但使用者又同時按 C 時，反而會將 KeyDown 事件標示為 Handled 並且不加以引發 (除非 [UIElement.AddHandler](http://msdn.microsoft.com/library/windows/apps/xaml/Windows.UI.Xaml.UIElement.AddHandler) 的 handledEventsToo 參數設定為 true)。

#### <a name="the-characterreceived-event"></a>CharacterReceived 事件

在文字控制項 (例如 TextBox) 的 [KeyDown](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.KeyDown) 事件之後引發 [CharacterReceived](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.CharacterReceived) 事件時，您可以在 KeyDown 事件處理常式中取消字元輸入。

#### <a name="the-previewkeydown-and-previewkeyup-events"></a>PreviewKeyDown 和 PreviewKeyUp 事件

預覽輸入事件會在任何其他事件之前引發。 如果不處理這些事件，就會引發焦點所在元素的快速鍵，後面接著引發 KeyDown 事件。 兩個事件都會反昇直到已處理為止。


![按鍵事件順序](images/accelerators/accelerators_keyevents.png)
***按鈕事件順序***

事件的順序：

預覽 KeyDown 事件...
應用程式快速鍵、OnKeyDown 方法、KeyDown 事件、上層之上層 KeyDown 事件的上層 OnKeyDown 方法的應用程式快速鍵 (執行事件反昇至根)…
CharacterReceived 事件、PreviewKeyUp 事件、KeyUpEvents

處理快速鍵事件時，也會將 KeyDown 事件標示為已處理。 KeyUp 事件仍保持未處理狀態。

### <a name="resolving-accelerators"></a>解析快速鍵

鍵盤快速鍵事件從焦點所在元素向上反昇至根。 如果沒有處理事件，XAML 架構會在反昇路徑以外尋找其他未限定範圍的應用程式快速鍵。

兩個鍵盤快速鍵以相同按鍵組合來定義時，會叫用視覺化樹狀結構上找到的第一個鍵盤快速鍵。

只有在焦點位於特定範圍內時，才會叫用限定範圍的鍵盤快速鍵。 例如，在包含許多控制項的方格中，只有在焦點位於方格 (範圍擁有者) 內時，才能叫用控制項的鍵盤快速鍵。

### <a name="scoping-accelerators-programmatically"></a>以程式設計方式限定快速鍵範圍

[UIElement.TryInvokeKeyboardAccelerator](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.tryinvokekeyboardaccelerator) 方法會叫用任何在元素樹狀子目錄中比對相符的快速鍵。

[UIElement.OnProcessKeyboardAccelerators](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.uielement.onprocesskeyboardaccelerators) 方法會在鍵盤快速鍵之前執行。 此方法傳遞包含按鍵、輔助按鍵以及表示鍵盤快速鍵是否已處理之布林值的 [ProcessKeyboardAcceleratorArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs) 物件。 如果標示為已處理，鍵盤快速鍵會執行事件反昇 (因此絕不會叫用外部鍵盤快速鍵)。

> [!NOTE]
> OnProcessKeyboardAccelerators 永遠會引發，不論是否已處理 (類似於 OnKeyDown 事件)。 您必須檢查事件是否已標示為已處理。

在此範例中，我們使用 OnProcessKeyboardAccelerators 和 TryInvokeKeyboardAccelerator 將鍵盤快速鍵的範圍限定為 Page 物件：

``` csharp
protected override void OnProcessKeyboardAccelerators(
  ProcessKeyboardAcceleratorArgs args)
{
  if(args.Handled != true)
  {
    this.TryInvokeKeyboardAccelerator(args);
    args.Handled = true;
  }
}
```

### <a name="localize-the-accelerators"></a>當地語系化快速鍵

我們建議將所有的鍵盤快速鍵當地語系化。 您可以在 XAML 宣告中使用標準 UWP 資源 (.resw) 檔案和 x:Uid 屬性來進行此作業。 在此範例中，Windows 執行階段會自動載入資源。

![使用 UWP 資源檔案將鍵盤快速鍵當地語系化](images/accelerators/accelerators_localization.png)
***使用 UWP 資源檔案將鍵盤快速鍵當地語系化***

``` xaml
<Button x:Uid="myButton" Click="OnSave">
  <Button.KeyboardAccelerators>
    <KeyboardAccelerator x:Uid="myKeyAccelerator" Modifiers="Control"/>
  </Button.KeyboardAccelerators>
</Button>
```

### <a name="setup-an-accelerator-programmatically"></a>以程式設計方式設定快速鍵

以下是透過程式設計方式定義快速鍵的範例：

``` csharp
void AddAccelerator(
  VirtualKeyModifiers keyModifiers, 
  VirtualKey key, 
  TypedEventHandler<KeyboardAccelerator, KeyboardAcceleratorInvokedEventArgs> handler )
  {
    var accelerator = 
      new KeyboardAccelerator() 
      { 
        Modifiers = keyModifiers, Key = key
      };
    accelerator.Invoked = handler;
    this.KeyAccelerators.Add(accelerator);
  }
```

> [!NOTE]
> KeyboardAccelerator 無法共用，您不能將相同的 KeyboardAccelerator 加入至多個元素。

### <a name="override-keyboard-accelerator-behavior"></a>覆寫鍵盤快速快速操作行為

您可以處理 [KeyboardAccelerator.Invoked](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyboardaccelerator.Invoked) 事件覆寫預設 KeyboardAccelerator 行為。

此範例示範如何在自訂 ListView 控制項中覆寫「選擇全部」命令 (Ctrl+A 鍵盤快速操作)。 我們也會將 Handled 屬性設為 true，以停止事件繼續反昇。

```csharp
public class MyListView : ListView
{
  …
  protected override void OnKeyboardAcceleratorInvoked(KeyboardAcceleratorInvokedEventArgs args) 
  {
    if(args.Accelerator.Key == VirtualKey.A 
      && args.Accelerator.Modifiers == KeyboardModifiers.Control)
    {
      CustomSelectAll(TypeOfSelection.OnlyNumbers); 
      args.Handled = true;
    }
  }
  …
}
```

## <a name="related-articles"></a>相關文章

* [鍵盤互動](keyboard-interactions.md)
* [便捷鍵](access-keys.md)

**範例**
* [XAML 控制項庫 (亦即 XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


 

