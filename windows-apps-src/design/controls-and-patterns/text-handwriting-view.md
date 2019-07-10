---
Description: 針對 TextBox、RichEditBox 之類的 UWP 文字控制項，以及可提供類似文字輸入體驗的 AutoSuggestBox 等控制項所支援的筆跡轉換文字輸入，自訂內建的手寫檢視。
title: 含手寫檢視的文字輸入
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/13/2019
ms.locfileid: "63774780"
---
# <a name="text-input-with-the-handwriting-view"></a>含手寫檢視的文字輸入

![文字方塊在使用手寫筆點選時展開](images/handwritingview/handwritingview2.gif)

針對 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) 之類的 UWP 文字控制項，以及可衍生自 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 等控制項所支援的筆跡轉換文字輸入，自訂內建的手寫檢視。

## <a name="overview"></a>概觀

XAML 文字輸入方塊使用 [Windows Ink](../input/pen-and-stylus-interactions.md) 特別提供手寫筆輸入的內嵌支援。 當使用者使用 Windows 手寫筆點選文字輸入方塊時，文字方塊轉換成手寫介面，而不是開啟另一個輸入面板。

文字可隨著使用者在文字方塊任何位置書寫時進行辨識，而候選字視窗會顯示辨識結果。 使用者可以點選結果來選擇，或繼續書寫以接受建議的候選字。 候選字視窗會包含逐字 (逐個字母) 辨識結果，因此辨識範圍並不局限於字典中的單字。 當使用者書寫時，接受的文字輸入會轉換為保有自然書寫風格的書寫體字型。

> [!NOTE]
> 預設會啟用手寫檢視，但是您可以按照每個控制項加以停用，並還原為文字輸入面板。

![使用筆跡與建議的文字方塊](images/handwritingview/handwritingview-inksuggestion1.gif)

使用者可以使用如下所示的標準手勢及動作來編輯他們的文字：

- 刪除線  或劃掉  ：一筆劃過，可刪除一個字或字的一部分
- 聯結  ：在單字之間劃弧線，可刪除其間的空格
- 插入  ：劃插入號可插入空格
- 覆寫  ：在現有文字上面書寫，可加以取代

![使用筆跡更正的文字方塊](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>停用手寫檢視

預設會啟用內建手寫檢視。

如果您已經在應用程式中提供同等的筆跡轉換文字功能，或您的文字輸入體驗依賴某種無法透過手寫提供的格式或特殊字元 (例如定位字元)，則可能會想停用手寫檢視。

在此範例中，我們將 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 控制項的 [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) 屬性設為 false，藉此停用手寫檢視。 支援手寫檢視的所有文字控制項都支援類似的屬性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>指定手寫檢視的對齊方式

手寫檢視位於基礎文字控制項上方，並調整大小以配合使用者的手寫喜好設定 (請參閱 [設定] -> [裝置] -> [畫筆與 Windows Ink] -> [手寫] -> [直接寫入文字欄位時的字型大小]  )。 相對於文字控制項及其在應用程式內的位置，此檢視也會自動對齊。

應用程式 UI 不會自動重排以容納較大的控制項，因此系統可能會導致檢視遮蔽重要的 UI。

在此，我們示範如何使用 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) 屬性，指定基礎文字控制項上的哪個錨點會用來對齊手寫檢視。

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

預設會啟用文字建議快顯視窗，以提供一份熱門筆跡辨識候選項目，以便使用者在最熱門的候選項目不正確時從中選取。

如果您的應用程式已經提供強大的自訂辨識功能，您可以使用 [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) 屬性來停用內建建議，如以下範例所示。

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

使用者可以從預先定義的手寫字型集合中選擇根據筆跡辨識轉譯文字所要使用的字型 (請參閱 [設定] -> [裝置] -> [畫筆與 Windows Ink]-> [手寫]-> [使用手寫時的字型]  )。

> [!NOTE]
> 使用者甚至可以根據自己的手寫來建立字型。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

您的應用程式可以存取此設定，並將選取的字型使用於文字控制項中已辨識的文字。

在此範例中，我們會接聽 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)的 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 事件，如果文字變更源自 HandwritingView 則套用使用者選取的字型 (如果不是，則套用預設字型)。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>存取複合控制項中的 HandwritingView

使用 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 或 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) 控制項的複合控制項 (例如 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)) 也支援 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

若要存取複合控制項中的 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)，請使用 [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API。

下列 XAML 程式碼片段會顯示 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 控制項。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

在對應的程式碼後置中，我們會示範如何停用 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)上的 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

1. 首先，我們會處理應用程式的 Loaded 事件，而我們在其中呼叫 FindInnerTextBox 函式來啟動視覺化樹狀結構周遊。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 我們接著會呼叫 FindVisualChildByName，開始逐一查看 FindInnerTextBox 函式中的視覺化樹狀結構 (於 AutoSuggestBox 開始)。

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

3. 最後，此函式會逐一查看視覺化樹狀結構，直到擷取 TextBox 為止。

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

## <a name="reposition-the-handwritingview"></a>調整 HandwritingView 的位置

在某些情況下，您可能需要確保 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 涵蓋它可能沒有的 UI 元素。

在此，我們會建立支援聽寫的 TextBox (實作方式為將 TextBox 和聽寫按鈕放入 StackPanel 中)。

![使用聽寫的 TextBox](images/handwritingview/textbox-with-dictation.png)

StackPanel 現在大於 TextBox，因此 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 可能完全不會遮蔽複合控制項。

![使用聽寫的 TextBox](images/handwritingview/textbox-with-dictation-handwritingview.png)

若要解決此問題，請將 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 PlacementTarget 屬性設定為它應該對齊的 UI 元素。

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

## <a name="resize-the-handwritingview"></a>調整 HandwritingView 的大小

您也可以設定 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的大小，當您必須確保檢視不會遮蔽重要 UI 時，這很有用。

如同先前的範例，我們會建立支援聽寫的 TextBox (實作方式為將 TextBox 和聽寫按鈕放入 StackPanel 中)。

![使用聽寫的 TextBox](images/handwritingview/textbox-with-dictation.png)

在此情況下，我們想要確保聽寫按鈕一律可見。

![使用聽寫的 TextBox](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

若要這樣做，請將 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 MaxWidth 屬性繫結至它應該遮蔽的 UI 元素寬度。

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

## <a name="reposition-custom-ui"></a>調整自訂 UI 的位置

如果您有可回應文字輸入的自訂 UI (例如資訊快顯視窗)，您可能需要調整該 UI 的位置，使其不會遮蔽手寫檢視。

![具有自訂 UI 的 TextBox](images/handwritingview/textbox-with-customui.png)

下列範例示範如何接聽 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)的 [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)、[Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)和 [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 事件，以設定 [Popup](https://docs.microsoft.com/uwp/api/windows.ui.popups) 的位置。

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

## <a name="retemplate-the-handwritingview-control"></a>重新建立 HandwritingView 控制項的範本

透過所有的 XAML 架構控制項，您可以針對您特定的需求來自訂 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的視覺化結構和視覺化行為。

若要查看建立自訂範本的完整範例，請查看[建立自訂傳輸控制項](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)操作說明或[自訂編輯控制項範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)。








