---
author: Jwmsft
label: Buttons
template: detail.hbs
---
# 按鈕
按鈕為使用者提供觸發立即動作的方式。

![按鈕的範例](images/controls/button.png)


<span class="sidebar_heading" style="font-weight: bold;">重要 API</span>

-   [**Button 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
-   [**RepeatButton 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)
-   [**Click 事件**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

## 這是正確的控制項嗎？

按鈕可讓使用者起始直接動作，例如提交表單。

如果動作是瀏覽到另一個頁面，請不要使用按鈕，改為使用連結。 如需詳細資訊，請參閱[超連結](hyperlinks.md)。
    
> 例外：對於精靈瀏覽，請使用標籤為 [上一頁] 和 [下一頁] 的按鈕。 對於其他類型的反向瀏覽或瀏覽到上層，請使用 [上一頁] 按鈕。

## 範例

這個範例是在 Microsoft Edge 瀏覽器的對話方塊中使用兩個按鈕 ([全部關閉] 和 [取消])。 

![按鈕的範例 (用於對話方塊中)](images/control-examples/buttons-edge.png)

## 建立按鈕

這個範例會顯示一個按鈕，它會回應 Click 動作。 

在 XAML 中建立按鈕。

```xaml
<Button Content="Submit" Click="SubmitButton_Click"/>
```

或者，在程式碼中建立按鈕。

```csharp
Button submitButton = new Button();
submitButton.Content = "Submit";
submitButton.Click += SubmitButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(submitButton);
```

處理 Click 事件。

```csharp
private async void SubmitButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to submit form. For example:
    // form.Submit();
    Windows.UI.Popups.MessageDialog messageDialog = 
        new Windows.UI.Popups.MessageDialog("Thank you for your submission.");
    await messageDialog.ShowAsync();
}
```

### 按鈕互動

當您以手指或手寫筆點選按鈕，或在游標位於按鈕上方時按下滑鼠左鍵，按鈕會引發 [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 如果按鈕有鍵盤焦點，則按下 Enter 鍵或空格鍵也會引發 Click 事件。

您通常無法處理按鈕上的低階 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件，因為按鈕本身有 Click 行為。 如需詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/en-us/library/windows/apps/mt185584.aspx)。

您可以變更 [**ClickMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.clickmode.aspx) 屬性，以變更按鈕引發 Click 事件的方式。 預設 ClickMode 值是 **Release**。 如果 ClickMode 是 **Hover**，則使用鍵盤或觸控方式並不能引發 Click 事件。 


### 按鈕內容

按鈕是 [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)。 它的 XAML 內容屬性是 [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)，它可讓您使用如下 XAML 語法︰`<Button>A button's content</Button>`。 您可以將任何物件設定為按鈕的內容。 如果內容是 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)，就會呈現於按鈕中。 如果內容是其他類型的物件，則會在按鈕中顯示它的字串表示。

此處提供的是 **StackPanel**，其中包含一個香蕉的影像，且文字已設定為按鈕的內容。

```xaml
<Button Click="Button_Click" 
        Background="#FF0D6AA3" 
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Slices.png" Height="62"/>
        <TextBlock Text="Orange"  Foreground="White"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

按鈕看起來就像這樣。

![含影像和文字內容的按鈕](images/button-orange.png)

## 建立一個重複按鈕

[
            **RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 是一個按鈕，可以從按鈕被按下的當時到鬆開後為止，重複引發 [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 設定 [**Delay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 屬性以指定 RepeatButton 在它被按下之後以及在它開始重複按一下動作之前，必須等待的時間。 設定 [**Interval**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 屬性以指定重複按下動作之間的時間。 這兩個屬性的時間是以毫秒為單位來指定。

下列範例顯示兩個 RepeatButton 控制項，而且其各自的 Click 事件是用來增加或減少文字區塊中顯示的值。

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## 建議

-   確定使用者能清楚了解按鈕的用途和狀態。
-   使用簡潔、專屬、一目了然的文字，清楚說明按鈕執行的動作。 通常按鈕文字內容是一個動詞單字。
-   如果按鈕的文字內容是動態的 (例如經過當地語系化)，請考慮如何調整按鈕的大小以及調整後周圍的控制項會發生什麼變化。
-   對於包含文字內容的命令按鈕，請使用最小按鈕寬度。
-   避免使用窄、短或高的命令按鈕包含文字內容。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   對於需要在 app 內多個頁面上提供的動作，請不要在多個頁面上複製按鈕，而是考慮改用[底部應用程式列](app-bars.md)。
-   一次只對使用者顯示一或兩個按鈕，例如，[接受] 和 [取消]。 如果需要對使用者顯示更多動作，請考慮使用[核取方塊](checkbox.md)或[選項按鈕](radio-button.md)，使用者可以利用它們選取動作，只要一個命令按鈕即可觸發這些動作。
-   使用預設命令按鈕來指示最常用或建議的動作。
-   考慮自訂您的按鈕。 按鈕的預設形狀是矩形，但是您可以自訂組成按鈕外觀的視覺效果。 按鈕的內容通常是文字，例如 [接受] 或 [取消]，但是您可以使用圖示來取代文字，或是使用圖示加上文字。
-   確定在使用者與按鈕互動時，按鈕會變更狀態和外觀，為使用者提供回饋。 按鈕狀態的範例有正常、已按下、已停用。
-   當使用者點選或按下按鈕時，觸發按鈕的動作。 通常使用者放開按鈕時會觸發動作，但是您也可以設定手指一按下時就觸發按鈕的動作。
-   不要使用命令按鈕設定狀態。
-   請不要在應用程式執行同時變更按鈕文字；例如，不要將按鈕文字「下一步」變更為「繼續」。
-   不要交換預設的送出、重設及按鈕樣式。
-   不要在按鈕內放入過多內容。 讓內容簡潔且容易理解 (一張圖片和一些文字就已足夠)。

## 返回按鈕
返回按鈕是一種系統提供的 UI 能供性，可往回瀏覽上一頁堆疊或使用者的瀏覽歷程記錄。

瀏覽歷程記錄的範圍 (app 內或全域) 取決於裝置和裝置模式。

## <span id="examples"></span><span id="EXAMPLES"></span>範例


系統返回按鈕的 UI 適合每種裝置和輸入類型，但每個裝置和通用 Windows 平台 (UWP) app 的瀏覽體驗卻是全域且一致的。 這些不同的體驗包含：

裝置手機 ![手機上的系統返回](images/nav-back-phone.png)
-   一律顯示。
-   裝置底部的軟體或硬體按鈕。
-   在 app 內和 app 間提供全域返回瀏覽。

<span id="Tablet"></span><span id="tablet"></span><span id="TABLET"></span>平板電腦 ![平板電腦上的系統返回](images/nav-back-tablet.png)
-   在平板電腦模式中一律顯示。

    在桌面模式中無法使用。 但是可改用標題列返回按鈕。 請參閱[電腦、膝上型電腦、平板電腦](#PC)。

    使用者若要在平板電腦模式和桌面模式之間切換，可移至 [設定 &gt; 系統 &gt; 平板電腦模式]**** 並設定 [在將裝置做為平板電腦使用時，讓 Windows 可更容易使用觸控方式操控]****。

-   裝置底部瀏覽列中的軟體按鈕。
-   在 app 內和 app 間提供全域返回瀏覽。

<span id="PC"></span><span id="pc"></span>電腦、膝上型電腦、平板電腦 ![電腦或膝上型電腦上的系統返回](images/nav-back-pc.png)
-   在桌面模式中為選擇性。

    在平板電腦模式中無法使用。 請參閱[平板電腦](#Tablet)。

    預設為停用。 必須選擇加入才能啟用。

    使用者若要在平板電腦模式和桌面模式之間切換，可移至 [設定 &gt; 系統 &gt; 平板電腦模式]**** 並設定 [在將裝置做為平板電腦使用時，讓 Windows 可更容易使用觸控方式操控]****。

-   App 標題列中的軟體按鈕。
-   只在 app 內提供返回瀏覽。 不支援 app 間瀏覽。

Surface Hub ![Surface Hub 上的系統返回](images/nav-back-surfacehub.png)
-   一律顯示。
-   裝置底部的軟體按鈕。
-   在 app 內和 app 間提供返回瀏覽。

 

## 可行與禁止事項


-   啟用返回瀏覽。

    如果未啟用返回瀏覽，您的應用程式會包含在全域的上一頁堆疊中，但不會保留應用程式內頁面瀏覽歷程記錄。

-   在桌面模式中啟用標題列返回按鈕。

    會保留 app 內頁面瀏覽歷程記錄，不支援 app 間的返回瀏覽。

    **注意：**在平板電腦模式中，當使用者從裝置頂端向下撥動或將滑鼠指標移至裝置頂端附近時，即會顯示標題列。 為避免重複和混淆，在平板電腦模式中不會顯示標題列返回按鈕。

     

-   當應用程式內瀏覽歷程記錄被清空或無法取得時，在桌面模式中隱藏或停用標題列返回按鈕。

    提供清楚的指示，讓使用者知道他們已無法再更往前返回瀏覽。

-   每個返回命令應回到返回堆疊中的上一頁，或者，如果不在桌面模式，則是返回上一個應用程式。

    如果返回瀏覽不是直覺式、一致且可預測的，使用者可能會混淆。

## 相關文章

- [選項按鈕](radio-button.md)
- [切換開關](toggles.md)
- [核取方塊](checkbox.md)

**適用於開發人員 (XAML)**
- [**Button 類別**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)




<!--HONumber=May16_HO2-->


