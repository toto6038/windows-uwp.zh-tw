---
Description: 自訂適用於 UWP 的文字控制項的文字方塊中，例如 RichEditBox （和控制項，如 AutoSuggestBox 提供類似的文字輸入的體驗） 所支援的文字輸入的筆墨的內建的手寫 檢視。
title: 使用 [手寫] 檢視的文字輸入
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634913"
---
# <a name="text-input-with-the-handwriting-view"></a>使用 [手寫] 檢視的文字輸入

![文字方塊在使用手寫筆點選時展開](images/handwritingview/handwritingview2.gif)

自訂內建的手寫檢視適用於 UWP 文字控制項，例如支援的文字輸入的筆墨[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)， [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)，和控制項這類衍生自這些[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)。

## <a name="overview"></a>概觀

XAML 文字輸入的方塊功能的手寫筆輸入 使用內嵌的支援來[Windows Ink](../input/pen-and-stylus-interactions.md)。 當使用者點選使用 Windows 畫筆的文字輸入方塊中時，文字方塊將轉換成手寫介面，而不用開啟個別的輸入的面板中。

當使用者撰寫的任何位置中的文字 方塊中，與候選視窗會顯示辨識結果，是已辨識文字。 使用者可以點選結果來選擇，或繼續書寫以接受建議的候選字。 候選字視窗會包含逐字 (逐個字母) 辨識結果，因此辨識範圍並不局限於字典中的單字。 當使用者書寫時，接受的文字輸入會轉換為保有自然書寫風格的書寫體字型。

> [!NOTE]
> 根據預設，啟用 [手寫] 檢視，但是您可以停用每個控制項為基礎，並改成文字輸入面板。

![筆跡與建議的文字方塊](images/handwritingview/handwritingview-inksuggestion1.gif)

使用者可以使用如下所示的標準手勢及動作來編輯他們的文字：

- _刪除線_或_劃掉_：一筆劃過，可刪除一個字或字的一部分
- _聯結_：在單字之間劃弧線，可刪除其間的空格
- _插入_：劃插入號可插入空格
- _覆寫_：在現有文字上面書寫，可加以取代

![包含筆墨更正的文字方塊](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>停用 [手寫] 檢視

預設會啟用內建的手寫檢視。

您可能想要停用 [手寫] 檢視，如果您已在您的應用程式，提供對等的筆墨轉換文字功能，或您的文字輸入的體驗依賴某種類型的格式或特殊字元 （例如索引標籤） 無法提供的手寫。

在此範例中，我們設定停用 [手寫] 檢視[IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled)屬性[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)控制項設為 false。 支援 [手寫] 檢視的所有文字控制項都支援類似的屬性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>指定 [手寫] 檢視的對齊方式

手寫檢視位於基礎文字控制項上方，並調整大小以配合使用者的手寫喜好設定 (請參閱**設定]-> [裝置]-> [畫筆與 Windows Ink]-> [手寫]-> [字型的大小，直接寫入時文字欄位**)。  檢視相對於文字控制項和其應用程式內的位置也會自動對齊。

應用程式 UI 不會不會自動重排以容納較大的控制，因此系統可能會造成要 occlude 重要 UI 的檢視。

在這裡，我們示範如何使用[PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment)屬性[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)指定基礎文字控制項上的錨點用來對齊手寫 檢視中。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>停用自動完成候選項目

預設會啟用文字建議快顯視窗，提供一份最上層的筆墨辨識使用者可以從中選取萬一前幾個候選不正確的候選項目。

如果您的應用程式中已提供強大的自訂辨識功能，您可以使用[AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled)停用的內建的建議，如下列範例所示的屬性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>使用手寫字型的喜好設定

使用者可以選擇從預先定義的集合時要使用的手寫字型的轉譯文字為基礎的筆墨辨識 (請參閱**設定]-> [裝置]-> [畫筆與 Windows Ink]-> [手寫]-> [字型時使用手寫**).

> [!NOTE]
> 使用者甚至可以建立根據他們自己的手寫字型。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

您的應用程式可以存取這項設定，並使用已辨識的文字，文字控制項中選取的字型。

在此範例中，我們接聽[TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged)事件[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)並套用使用者選取的字型，如果此文字變更源自 HandwritingView （或如果不是預設字型）。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>複合控制項中 HandwritingView 的存取

複合控制項，使用[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)或是[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)控制項，例如[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)也支援[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

若要存取[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)在複合控制項中，使用[VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API。

下列 XAML 程式碼片段會顯示[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)控制項。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

對應程式碼後置中，我們會示範如何停用[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)上[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)。

1. 首先，我們處理應用程式的載入的事件，我們呼叫來啟動視覺化樹狀結構周遊的 FindInnerTextBox 函式。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 我們接著會開始逐一查看的視覺化樹狀結構 （起價 AutoSuggestBox） 藉由呼叫 FindVisualChildByName FindInnerTextBox 函式。

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. 最後，此函式逐一查看的視覺化樹狀結構之前擷取的文字方塊中。

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>重新定位 HandwritingView

在某些情況下，您可能需要以確保[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)涵蓋它否則可能不的 UI 項目。

在這裡，我們會建立支援 （將文字方塊和聽寫按鈕放到 StackPanel 實作） 的聽寫的文字方塊。

![使用聽寫的文字方塊](images/handwritingview/textbox-with-dictation.png)

在 StackPanel 大於現在文字方塊中，如[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)可能不 occlude 所有複合 cotnrol。

![使用聽寫的文字方塊](images/handwritingview/textbox-with-dictation-handwritingview.png)

若要解決此問題，請將 PlacementTarget 的[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)至 UI 項目，它應該對齊。

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>調整大小 HandwritingView

您也可以設定的大小[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)，可能會很有用時您必須確定檢視不 occlude 重要的 UI。

如上述範例中，我們會建立支援 （將文字方塊和聽寫按鈕放到 StackPanel 實作） 的聽寫的文字方塊。

![使用聽寫的文字方塊](images/handwritingview/textbox-with-dictation.png)

在此情況下，我們想要確保聽寫按鈕為永遠可見。

![使用聽寫的文字方塊](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

若要這樣做，我們的屬性繫結 MaxWidth [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)它應該 occlude 的 UI 元素的寬度。

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>重新定位的自訂 UI

如果您有自訂 UI，會出現在回應的文字輸入，例如資訊快顯視窗中，您可能需要重新定位該 UI，讓它不 occlude [手寫] 檢視。

![具有自訂 UI 的 TextBox](images/handwritingview/textbox-with-customui.png)

下列範例顯示如何接聽[Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)， [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)，和[SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)事件[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)設定位置[快顯](https://docs.microsoft.com/uwp/api/windows.ui.popups)。

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>Retemplate HandwritingView 控制項

因為透過所有 XAML 架構的控制項，您可以自訂視覺結構和視覺化行為[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)針對您特定的需求。

若要查看建立的自訂範本簽出的完整範例[建立自訂的闇熁勂](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)how-to 或[自訂編輯控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)。








