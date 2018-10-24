---
author: Karl-Bridge-Microsoft
Description: Customize the built-in handwriting view for ink to text input that is supported by UWP text controls such as the TextBox, RichEditBox (and controls like the AutoSuggestBox that provide a similar text input experience).
title: 文字輸入使用手寫檢視
label: Text input with the handwriting view
template: detail.hbs
ms.author: kbridge
ms.date: 10/13/18
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3117cde7b8b00973c135fbc759fa99b6a48ec6ac
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "5434820"
---
# <a name="text-input-with-the-handwriting-view"></a>文字輸入使用手寫檢視

![文字方塊在使用手寫筆點選時展開](images/handwritingview/handwritingview2.gif)

自訂 / [筆跡轉換文字輸入支援的 UWP 文字控制項，例如[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)、 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)及其他控制項，可提供類似的文字輸入的體驗 （例如[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)) 的內建的手寫檢視。

## <a name="overview"></a>概觀

XAML 文字輸入的方塊功能提供手寫筆輸入使用[Windows Ink](../input/pen-and-stylus-interactions.md)的內嵌的支援。 當使用者點選文字輸入方塊，使用 Windows 手寫筆時，文字方塊會轉換成手寫表面，而非開啟另一個輸入的面板中。

當使用者書寫時任何位置中的文字方塊中，並候選項目視窗會顯示辨識結果，是可辨識的文字。 使用者可以點選結果來選擇，或繼續書寫以接受建議的候選字。 候選字視窗會包含逐字 (逐個字母) 辨識結果，因此辨識範圍並不局限於字典中的單字。 當使用者書寫時，接受的文字輸入會轉換為保有自然書寫風格的書寫體字型。

> [!NOTE]
> 手寫檢視會根據預設，啟用，但您可以停用它以每個控制項為基礎，並改為還原為文字輸入面板。

![文字方塊與筆墨和建議](images/handwritingview/handwritingview-inksuggestion1.gif)

使用者可以使用如下所示的標準手勢及動作來編輯他們的文字：

- _刪除線_或_劃掉_：一筆劃過，可刪除一個字或字的一部分
- _聯結_：在單字之間劃弧線，可刪除其間的空格
- _插入_：劃插入號可插入空格
- _覆寫_：在現有文字上面書寫，可加以取代

![文字方塊與筆墨校正](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>停用手寫檢視

預設會啟用內建的手寫檢視。

您可能會想要停用手寫檢視，如果您已經在您的應用程式，提供對等的筆跡轉換文字功能，或您的文字輸入的體驗依賴某種類型的格式或特殊字元 （例如索引標籤） 透過手寫無法使用。

在此範例中，我們來停用手寫檢視的[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)控制項[IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled)屬性設定為 false。 支援手寫檢視的所有文字控制項都支援的類似屬性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>指定手寫檢視的對齊方式

手寫檢視是位於基礎的文字控制項的上方和調整大小以容納使用者的手寫喜好設定 (請參閱**設定]-> [裝置]-> [手寫筆與 Windows Ink 手寫]-> [-> 字型大小，直接在文字欄位撰寫時**). 檢視也會自動靠相對於文字控制項和其應用程式內的位置。

應用程式 UI 不會自動重排以容納較大的控制項，因此，系統可能會造成遮蔽重要的 UI 檢視。

在這裡，我們會示範如何使用的[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment)屬性來指定哪些基礎的文字控制項上的錨點用來對齊手寫檢視。

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

預設會啟用文字建議快顯視窗，以提供一份頂端的筆跡辨識使用者可從中選取以防最佳的候選項目不正確的候選項目。

如果您的應用程式已經提供健全，自訂辨識功能，您可以使用[AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled)屬性來停用內建的建議，如下列範例所示。

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

## <a name="use-handwriting-font-preferences"></a>使用手寫字型喜好設定

使用者可以從集合中選擇預先定義的手寫型時，要使用的字型轉譯文字根據筆墨辨識 (請參閱**設定]-> [裝置]-> [手寫筆與 Windows Ink 手寫]-> [-> 字型，當使用手寫**)。

> [!NOTE]
> 使用者甚至可以建立根據他們自己的手寫的字型。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

您的應用程式可以存取這項設定，並使用文字控制項中已辨識的文字選取的字型。

在此範例中，我們會接聽的[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged)事件，並套用的使用者選取的字型，如果文字變更來自 HandwritingView （或預設字型，如果不是）。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>存取複合控制項中 HandwritingView

也使用[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)或[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)控制項，例如[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)的複合控制項支援[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

若要存取[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)複合控制項中，使用[VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API。

下列 XAML 程式碼片段顯示[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)控制項。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

對應的程式碼後置中，我們顯示如何停用[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)上。

1. 首先，我們處理應用程式的 Loaded 的事件，我們呼叫 FindInnerTextBox 函式開始視覺化樹狀目錄周遊。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 我們再開始逐一查看視覺化樹狀結構 （從 AutoSuggestBox） 呼叫 FindVisualChildByName FindInnerTextBox 函式中。

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

3. 最後，此函式逐一視覺化樹狀結構直到 TextBox 不會擷取。

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

在某些情況下，您可能需要確保[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)涵蓋它否則可能不的 UI 元素。

在這裡，我們會建立 TextBox 支援聽寫 （藉由將 TextBox 及聽寫按鈕放入 StackPanel 實作）。

![聽寫的 TextBox](images/handwritingview/textbox-with-dictation.png)

StackPanel 大於 TextBox，因為[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)可能不會遮蓋到所有的複合 cotnrol。

![聽寫的 TextBox](images/handwritingview/textbox-with-dictation-handwritingview.png)

若要解決這個問題，設定 PlacementTarget 屬性[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) ，它應該對齊的 UI 元素。

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

您也可以設定的大小[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)中，這非常有用時您需要確保檢視不會遮蓋重要的 UI。

如先前範例中，我們會建立 TextBox 支援聽寫 （藉由將 TextBox 及聽寫按鈕放入 StackPanel 實作）。

![聽寫的 TextBox](images/handwritingview/textbox-with-dictation.png)

在此情況下，我們想要確保聽寫按鈕一律顯示。

![聽寫的 TextBox](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

若要這樣做，我們將屬性繫結 MaxWidth 的[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)它應該會遮蓋到 UI 元素的寬度。

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

## <a name="reposition-custom-ui"></a>重新定位自訂 UI

如果您有自訂的 UI 會出現在回應文字輸入，例如資訊的快顯視窗，您可能需要重新定位該 UI，讓它不會遮蓋到手寫檢視。

![自訂 UI 的 TextBox](images/handwritingview/textbox-with-customui.png)

下列範例示範如何將快[顯視窗](https://docs.microsoft.com/uwp/api/windows.ui.popups)的位置設定[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)的[Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)、[已關閉](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)，並以[SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)事件接聽。

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

## <a name="retemplate-the-handwritingview-control"></a>重新範本化 HandwritingView 控制項

如同所有的 XAML 架構控制項，您可以自訂的視覺結構和視覺行為[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)同時適用於您的特定需求。

若要建立自訂範本簽出[建立自訂傳輸控制項](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)的作法或[自訂編輯控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)的完整範例，請參閱。







