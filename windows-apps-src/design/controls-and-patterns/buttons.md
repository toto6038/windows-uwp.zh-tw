---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: 按鈕
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f663ce9da6453922e35f850a8cd039f33770f434
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494165"
---
# <a name="buttons"></a>按鈕


按鈕為使用者提供觸發立即動作的方式。

> **重要 API**：[Button 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)、[RepeatButton 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)、[Click 事件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![按鈕的範例](images/controls/button.png)

## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

按鈕可讓使用者起始直接動作，例如提交表單。

如果動作是瀏覽到另一個頁面，請不要使用按鈕，改為使用連結。 如需詳細資訊，請參閱[超連結](hyperlinks.md)。
> 例外：對於精靈瀏覽，請使用標籤為 [上一頁] 和 [下一頁] 的按鈕。 對於其他類型的反向瀏覽或瀏覽到上層，請使用 [上一頁] 按鈕。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/Button">開啟應用程式並查看 Button 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

這個範例在要求位置存取權的對話方塊中使用兩個按鈕 ([允許] 和 [封鎖])。

![按鈕的範例 (用於對話方塊中)](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>建立按鈕

這個範例會顯示一個按鈕，它會回應 Click 動作。

在 XAML 中建立按鈕。

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

或者，在程式碼中建立按鈕。

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

處理 Click 事件。

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
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

### <a name="button-interaction"></a>按鈕互動

當您以手指或手寫筆點選按鈕，或在游標位於按鈕上方時按下滑鼠左鍵，按鈕會引發 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 如果按鈕有鍵盤焦點，則按下 Enter 鍵或空格鍵也會引發 Click 事件。

您通常無法處理按鈕上的低階 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件，因為按鈕本身有 Click 行為。 如需詳細資訊，請參閱[事件與路由事件概觀](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx)。

您可以變更 [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode) 屬性，以變更按鈕引發 Click 事件的方式。 預設 ClickMode 值是 **Release**，但您也可以將按鈕的 ClickMode 設定為 **Hover** 或 **Press**。 如果 ClickMode 是 **Hover**，則使用鍵盤或觸控無法引發 Click 事件。


### <a name="button-content"></a>按鈕內容

按鈕是 [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx)。 它的 XAML 內容屬性是 [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx)，它可讓您使用如下 XAML 語法︰`<Button>A button's content</Button>`。 您可以將任何物件設定為按鈕的內容。 如果內容是 [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx)，就會呈現於按鈕中。 如果內容是其他類型的物件，則會在按鈕中顯示它的字串表示。

按鈕的內容通常是文字。 以下是對含有文字內容的按鈕的設計建議：
-   使用簡潔、專屬、一目了然的文字，清楚說明按鈕執行的動作。 通常按鈕文字內容是一個動詞單字。
-   除非您的品牌指導方針指示您使用其他字型，否則使用預設字型。
-   對於簡短文字，避免使用 120px 的最小按鈕寬度將命令按鈕變窄。
- 對於較長文字，避免將文字限制在 26 個字元的最大長度，以使命令按鈕變寬。
-   如果按鈕的文字內容為動態 (例如經過[當地語系化](../globalizing/globalizing-portal.md))，請考慮如何調整按鈕的大小以及調整後周圍的控制項會發生什麼變化。

<table>
<tr>
<td> <b>需要修正：</b><br> 含溢位文字的按鈕。 </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>選項 1：</b><br> 增加按鈕寬度、堆疊按鈕，如果文字長度大於 26 個字元，則換行。 </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>選項 2：</b><br> 增加按鈕高度，並將文字換行。 </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

您也可以自訂形成按鈕外觀的視覺效果。 例如，您可能以圖示取代文字，或使用圖示加上文字。

如下所示，包含影像和文字的 **StackPanel** 已設定為按鈕的內容。

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

按鈕看起來就像這樣。

![含影像和文字內容的按鈕](images/button-orange.png)

## <a name="create-a-repeat-button"></a>建立一個重複按鈕

[RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) 是一個按鈕，可以從按鈕被按下的當時到鬆開後為止，重複引發 [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) 事件。 設定 [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) 屬性以指定 RepeatButton 在它被按下之後以及在它開始重複按一下動作之前，必須等待的時間。 設定 [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) 屬性以指定重複按下動作之間的時間。 這兩個屬性的時間是以毫秒為單位來指定。

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

## <a name="recommendations"></a>建議
-   確定使用者能清楚了解按鈕的用途和狀態。
-   同樣的決定 (例如確認對話方塊) 有多個按鈕時，依下列順序顯示認可按鈕 (其中，[執行] 和 [不執行] 是主要指令的特定回應)：
    -   確定/[執行]/是
    -   [不執行]/否
    -   取消
-   一次只對使用者顯示一或兩個按鈕，例如，[接受] 和 [取消]。 如果需要對使用者顯示更多動作，請考慮使用[核取方塊](checkbox.md)或[選項按鈕](radio-button.md)，使用者可以利用它們選取動作，只要一個命令按鈕即可觸發這些動作。
-   對於需要在 app 內多個頁面上提供的動作，請不要在多個頁面上複製按鈕，而是考慮改用[底部應用程式列](app-bars.md)。

### <a name="recommended-single-button-layout"></a>建議使用的單一按鈕配置

如果您的配置只需要一個按鈕，此按鈕應根據其容器內容靠左或靠右對齊。

-   只有一個按鈕的對話方塊必須讓按鈕**靠右對齊**。 如果對話方塊只包含一個按鈕，請確保按鈕執行的是安全、不具破壞性的動作。 如果您使用 [ContentDialog](dialogs.md) 並指定單一按鈕，此按鈕將會自動靠右對齊。

![對話方塊內的按鈕](images/pushbutton_doc_dialog.png)

-   如果您的按鈕會在容器 UI 中出現 (例如，在快顯通知、飛出視窗或的清單檢視項目中)，就必須讓按鈕在容器內**靠右對齊**。

![容器內的按鈕](images/pushbutton_doc_container.png)

-   在包含單一按鈕 (例如，設定頁面底部的 [套用] 按鈕) 的頁面中，您應該讓按鈕**靠左對齊**。 這樣可確保按鈕對齊其餘的網頁內容。

![頁面上的按鈕](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>返回按鈕

返回按鈕是一種系統提供的 UI 元素，可透過使用者的返回堆疊或瀏覽歷程記錄啟用向後瀏覽。 您不需要自行建立返回按鈕，但您可能必須花一點心力來提供良好的向後瀏覽體驗。 如需詳細資訊，請參閱[歷程記錄和向後瀏覽](../basics/navigation-history-and-backwards-navigation.md)。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。


## <a name="related-articles"></a>相關文章
- [Button 類別](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [選項按鈕](radio-button.md)
- [核取方塊](checkbox.md)
- [切換開關](toggles.md)
