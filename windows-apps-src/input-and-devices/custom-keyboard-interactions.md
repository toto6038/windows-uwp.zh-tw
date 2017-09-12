---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 010663320b4011d853c53bc4121f86a14bfbfe0c
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2017
---
# <a name="custom-keyboard-interactions"></a>自訂鍵盤互動

可在您的 UWP app 中提供完整且一致的鍵盤互動體驗，以及為鍵盤使用者和行動不便或有其他協助工具需求的人士提供自訂控制項。

本主題中，我們主要的焦點在於，為電腦上的 UWP app 支援使用自訂控制項進行鍵盤輸入。 不過，完善的鍵盤體驗對支援協助工具例如 Windows 朗讀程式，以及使用軟體鍵盤例如觸控式鍵盤和螢幕小鍵盤 (OSK)，也是很重要的。

## <a name="providing-2d-directional-inner-navigation-a-namexyfocuskeyboardnavigation"></a>提供 2D 方向內部瀏覽 <a name="xyfocuskeyboardnavigation">

使用 **XYFocusKeyboardNavigation** 屬性支援使用鍵盤（方向鍵）、Xbox 遊戲台（方向鍵和左搖桿按鍵）與 Xbox 遙控器（方向鍵）之自訂控制項及控制項群組的 2D 方向內部瀏覽。

**注意** 我們將控制項或控制項群組的內部瀏覽區域稱為*方向區域*。

**XYFocusKeyboardNavigation** 有類型 **XYFocusKeyboardNavigationMode**，其可能值是 **Auto**（預設）、**Enabled** 或 **Disabled**。

這個屬性不會影響 tab 瀏覽，只會影響控制項或控制項群組內子元素的內部瀏覽。 方向區域的子元素不應該包含在 tab 瀏覽。

### <a name="default-behavior"></a>預設行為

方向瀏覽行為是根據元素的上階或繼承階層。 如果所有上階都是預設模式，或設定為 **Auto**，鍵盤不支援方向瀏覽行為 (遊樂器和遠端控制一定支援方向瀏覽，除非明確設定為 **Disabled**)。

### <a name="custom-behavior"></a>自訂行為

這個屬性設為 **Enabled**，可讓您的控制項在控制項的每個 UIElement 上支援 2D 內部瀏覽（使用鍵盤方向鍵）。

當您使用鍵盤方向鍵，瀏覽限制在方向區域內（按下 tab 鍵，會將焦點設至方向區以外的下一個可設定焦點項目）。

**注意** 使用遊戲台與遙控器時不是這種情況，瀏覽持續在方向區域之外的下一個可設定焦點控制項。

這個屬性只影響使用方向鍵的內部瀏覽，不會影響 tab 鍵瀏覽。 所有控制項都維持其預期的 Tab 順序階層。

下圖顯示三個按鈕（A、B 和 C）包含在方向區內，第四個按鈕 (D) 在方向區外。

![方向區域](images/keyboard/directional-area.png)

*鍵盤方向鍵可以將焦點在按鈕 A-B-C 之間移動，但無法移至 D*

下列程式碼範例示範，**XYFocusKeyboardNavigation** 啟用時，如何影響瀏覽。 使用先前的影像，A 有最初的焦點，並方向鍵限制在方向區域內時，透過 tab 鍵循環所有控制項 (A -&gt; B -&gt; C -&gt; D，然後回到 A)。

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" />
      </StackPanel>
      <Button Content="D" />
</StackPanel>
```

#### <a name="override-directional-navigation"></a>覆寫方向瀏覽

使用 XYFocusRight/XYFocusLeft 日 XYFocusTop/XYFocusDown 屬性，覆寫預設的瀏覽行為。

以下是和上一個範例相同的影像，顯示三個按鈕（A、B 和 C）包含在方向區內，第四個按鈕 (D) 在方向區外。

![方向區域](images/keyboard/directional-area.png)

*鍵盤方向鍵可以將焦點在按鈕 A-B-C 之間移動，也可以移出至 D*

此程式碼範例示範如何覆寫向右鍵的預設瀏覽行為，方法是讓它瀏覽到方向區域外的控制項。 請注意，方向區域無法使用向左鍵重新進入。

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" XYFocusRight="{x:Bind ButtonD}" />
      </StackPanel>
      <Button Content="D" x:Name="ButtonD"/>
</StackPanel>
```

如需詳細資訊，請參閱本主題稍後的 [XYFocus 瀏覽策略](#set-the-tab-navigation-behavior)。

#### <a name="restrict-navigation-with-disabled"></a>使用 Disabled 來限制瀏覽

設定 **XYFocusKeyboardNavigation** 為 **Disabled**，將方向鍵瀏覽限制在方向區域內。

**注意** 此屬性設定不會影響控制項本身的鍵盤瀏覽，只會影響控制項子元素的鍵盤瀏覽。

在下列程式碼範例，父項 StackPanel 將 **XYFocusKeyboardNavigation** 設為 **Enabled**，而子元素 C 將**XYFocusKeyboardNavigation** 設定為 **Disabled**。 只有子元素 C 已停用方向鍵瀏覽。

```XAML
<StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
        <Button Content="A" />
        <Button Content="B" />
        <Button Content="C" XYFocusKeyboardNavigation="Disabled" >
            ...
        </Button>
</StackPanel>
```

#### <a name="use-nested-directional-areas"></a>使用巢狀方向區域

您可以有多個層級的巢狀方向區域。 如果所有父元素都將 **XYFocusKeyboardNavigation** 設為 **Enabled**，方向鍵瀏覽會忽略區域邊界。

下圖顯示三個按鈕（A、B 和 C）包含在巢狀方向區內，第四個按鈕 (D) 在方向區外。

![巢狀方向區域](images/keyboard/nested-directional-area.png)

*鍵盤方向鍵可以將焦點在按鈕 A-B-C 之間移動，但無法移至 D*

此程式碼範例示範如何指定瀏覽巢狀方向區域支援方向鍵瀏覽跨區域邊界。

```XAML
<StackPanel Orientation="Horizontal">
        <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
            <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
                <Button Content="A" />
                <Button Content="B" />
            </StackPanel>
            <Button Content="C" />
        </StackPanel>
        <Button Content="D" />
 </StackPanel>
```

下圖顯示四個按鈕（A、B、C 和 D），其中 A 和 B 包含在巢狀方向區域中，而 C 和 D 包含不同的區域中。 因為父元素將 **XYFocusKeyboardNavigation** 設為 **Disabled**，無法使用方向鍵跨越每個巢狀區域的邊界。

![非方向區域](images/keyboard/non-directional-area.png)

*鍵盤方向鍵可以將焦點在按鈕 A-B 之間和按鈕 C-D 之間移動，但無法在不同地區之間移動*

此程式碼範例示範如何指定不支援方向鍵瀏覽跨區域邊界的瀏覽巢狀方向區域。

```XAML
<StackPanel Orientation="Horizontal">
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="A" />
    <Button Content="B" />
  </StackPanel>
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="C" />
    <Button Content="D" />
  </StackPanel>
</StackPanel>
```

以下是巢狀方向區域更複雜的範例，其中：

-   如果 A 有焦點時，只能瀏覽到 E（反之亦然），因為非方向區域邊界讓 B、C 和 D 無法使用方向鍵存取
-   如果 B 有焦點時，只能瀏覽到 C（反之亦然），因為 D 在方向區域之外，而非方向區域邊界封鎖存取 A 和 E
-   如果 D 有焦點時，必須使用 Tab 鍵在不同的控制項之間瀏覽，因為方向鍵瀏覽不可能

![巢狀非方向區域](images/keyboard/nested-non-directional-area.png)

*鍵盤方向鍵可以將焦點在按鈕 A-E 之間和按鈕 B-C 之間移動，但無法在其他地區之間移動*

此程式碼範例示範如何指定支援複雜方向鍵瀏覽跨區域邊界的瀏覽巢狀方向區域。

```XAML
<StackPanel  Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
  <Button Content="A" />
    <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Disabled">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
        <Button Content="B" />
        <Button Content="C" />
      </StackPanel>
        <Button Content="D" />
    </StackPanel>
  <Button Content="E" />
</StackPanel>
```

## <a name="set-the-tab-navigation-behavior-a-nametab-navigation"></a>設定 tab 瀏覽行為 <a name="tab-navigation">

UIElement.[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) 屬性指定其整個物件樹（或方向區域）的 tab 鍵瀏覽行為。

[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) 有類型 **TabFocusNavigationMode**，其可能值是 **Once**、**Cycle** 或 **Local**（預設）。

在下列影像中，根據方向區域的 tab 瀏覽，焦點依照下列方法移動：

-   Once：A、B1、C、A
-   Local：A、B1、B2、B3、B4、B5、C、A
-   Cycle：A、B1、B2、B3、B4、B5、B1、B2、B3、B4、B5、(所有 B 的循環)

![Tab 瀏覽](images/keyboard/tab-navigation.png)

*根據 tab 瀏覽模式的焦點行為*

下列程式碼範例示範如何使用模式為 Once 的 TabFocusNavigation。

```XAML
<Button Content="X" Click="OnAClick"/>
<StackPanel Orientation="Horizontal" XYFocusNavigation ="Local"
   TabFocusNavigation ="Local">
   <Button Content="A" Click="OnAClick"/>
   <StackPanel Orientation="Horizontal" TabFocusNavigation ="Once">
        <Button Content="B" Click="OnBClick"/>
        <Button Content="C" Click="OnCClick"/>
        <Button Content="D" Click="OnDClick"/>
   </StackPanel>
   <Button Content="E" Click="OnBClick"/>
</StackPanel>
```

*當焦點位於 X 時，Tab 瀏覽是：A、B、E、X*

#### <a name="about-tabfocusnavigation-and-tabindex"></a>關於 TabFocusNavigation 和 TabIndex

UIElement.TabFocusNavigation 屬性有 Control.TabNavigation 屬性相同的行為，包括如何與 TabIndex 搭配運作。

當控制項沒有指定任何 TabIndex 時，架構會指派給它比目前最高指數值（和最低的優先順序）更高的指數值。 它選擇視覺化樹狀結構上的第一個項目，使控制項解析意義清楚。 架構在每個範圍解析 Tab 索引。 控制項的子系被視為一個範圍，而如果其中一個子系有子系，後者是另一個範圍的一部分。

在下列影像中，根據方向區域的 Tab 瀏覽和元素的 Tab 索引，焦點依照下列方法移動：

-   Once：A、B3、C、A。
-   Local：A、B3、B4、B5、B1、B2、C、A。
-   Cycle：A、B3、B4、B5、B1、B2、B3、B4、B5、B1、B2、(所有 B 的循環)

![tab 索引](images/keyboard/tab-index.png)

*根據 tab 瀏覽模式和 tab 索引的焦點行為*

請注意方向區域如何視為範圍，以及焦點瀏覽如何移到最先具有最高優先順序的控制項：B3。 事實上，有兩個範圍：一個用於 A、方向區域和 C。另一個用於方向區域。 因為方向區域不是 TabStop，架構切換範圍以尋找最佳的候選項目，然後以遞迴方式尋找任何子元素。
