---
Description: Hyperlinks navigate the user to another part of the app, to another app, or launch a specific uniform resource identifier (URI) using a separate browser app.
title: 超連結
ms.assetid: 74302FF0-65FC-4820-B59A-718A765EF7F0
label: Hyperlinks
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6d2a4ae2449fcfcccee792f70bb4d90f7b745047
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/11/2018
ms.locfileid: "8940577"
---
# <a name="hyperlinks"></a>超連結

 

超連結會讓使用者瀏覽到應用程式的其他部分、到其他應用程式，或使用不同的瀏覽器應用程式啟動特定的統一資源識別項 (URI)。 有兩種方式可以將超連結新增至 XAML 應用程式：**Hyperlink** 文字元素和 **HyperlinkButton** 控制項。

> **重要 API**：[Hyperlink text element](https://msdn.microsoft.com/library/windows/apps/dn279356)、[HyperlinkButton control](https://msdn.microsoft.com/library/windows/apps/br242739)

![超連結按鈕](images/controls/hyperlink-button.png)


## <a name="is-this-the-right-control"></a>這是正確的控制項嗎？

當您需要在選取文字時會產生回應，並將使用者瀏覽到該文字相關的詳細資訊，請使用超連結。

根據您的需求選擇正確的超連結類型：

-   使用文字控制項內的內嵌 **Hyperlink** 文字元素。 Hyperlink 元素會與其他文字元素一起流動，而且您可以在任何 InlineCollection 中使用它。 若您希望超連結會自動換行，且不需要大的點擊目標，請使用文字超連結。 超連結文字通常比較小且不易對準，特別是使用觸控時。
-   如果是獨立超連結，請使用 **HyperlinkButton**。 HyperlinkButton 是一個特殊 Button 控制項，可在要使用 Button 的任何位置使用。
-   使用具有 [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) 的 **HyperlinkButton** 做為其內容，以設為可點選的影像。

## <a name="examples"></a>範例

<table>
<th align="left">XAML 控制項庫<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果您已安裝 <strong style="font-weight: semi-bold">XAML 控制項庫</strong>應用程式，請按一下這裡<a href="xamlcontrolsgallery:/item/HyperlinkButton">開啟應用程式並查看 HyperlinkButton 運作情形</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">取得 XAML 控制項庫應用程式 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">取得原始碼 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-hyperlink-text-element"></a>建立 Hyperlink 文字元素

此範例示範如何在 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 內使用 Hyperlink 文字元素。

```xml
<StackPanel Width="200">
    <TextBlock Text="Privacy" Style="{StaticResource SubheaderTextBlockStyle}"/>
    <TextBlock TextWrapping="WrapWholeWords">
        <Span xml:space="preserve"><Run>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Read the </Run><Hyperlink NavigateUri="http://www.contoso.com">Contoso Privacy Statement</Hyperlink><Run> in your browser.</Run> Donec pharetra, enim sit amet mattis tincidunt, felis nisi semper lectus, vel porta diam nisi in augue.</Span>
    </TextBlock>
</StackPanel>

```
超連結顯示於行內且隨著周圍文字一起流動：

![超連結做為文字元素的範例](images/controls_hyperlink-element.png) 

> **提示**&nbsp;&nbsp;在 XAML 中，於具有其他文字元素的文字控制項中使用 Hyperlink 時，請將內容放在 [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) 容器中，並將 `xml:space="preserve"` 屬性套用至 Span，以保留 Hyperlink 與其他元素之間的空格。

## <a name="create-a-hyperlinkbutton"></a>建立 HyperlinkButton

以下是如何使用同時包含文字和影像的 HyperlinkButton。

```xml
<StackPanel>
    <TextBlock Text="About" Style="{StaticResource TitleTextBlockStyle}"/>
    <HyperlinkButton NavigateUri="http://www.contoso.com">
        <Image Source="Assets/ContosoLogo.png"/>
    </HyperlinkButton>
    <TextBlock Text="Version: 1.0.0001" Style="{StaticResource CaptionTextBlockStyle}"/>
    <HyperlinkButton Content="Contoso.com" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Acknowledgments" NavigateUri="http://www.contoso.com"/>
    <HyperlinkButton Content="Help" NavigateUri="http://www.contoso.com"/>
</StackPanel>

```

包含文字內容的超連結按鈕會顯示為標記文字。 Contoso 標誌影像也是可點選的超連結︰

![超連結做為按鈕控制項的範例](images/controls_hyperlink-button-image.png)

此範例顯示如何在程式碼中建立 HyperlinkButton。

```csharp
var helpLinkButton = new HyperlinkButton();
helpLinkButton.Content = "Help";
helpLinkButton.NavigateUri = new Uri("http://www.contoso.com");
```

## <a name="handle-navigation"></a>處理瀏覽

針對這兩種類型的超連結，您可以使用相同的方式來處理瀏覽；您可以設定 **NavigateUri** 屬性，或處理 **Click** 事件。

**瀏覽至 URI**

若要使用超連結瀏覽至 URI，請設定 NavigateUri 屬性。 使用者按一下或點選超連結時，會在預設瀏覽器中開啟指定的 URI。 預設瀏覽器會在與您應用程式不同的處理序中執行。

> [!NOTE]
> URI 是以 [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) 類別表示。 使用 .NET 進行程式設計時，此類別會隱藏，您應該使用 [System.Uri](https://docs.microsoft.com/dotnet/api/system.uri) 類別。 如需詳細資訊，請參閱這些類別的參考頁面。

您不一定要使用 **http:** 或 **https:** 配置。 如果適合在瀏覽器中載入這些位置中的資源內容，則可以使用 **ms-appx:**、**ms-appdata:** 或 **ms-resources:** 這類配置。 不過，會特別封鎖 **file:** 配置。 如需詳細資訊，請參閱 [URI 配置](https://msdn.microsoft.com/library/windows/apps/jj655406.aspx)。

使用者按一下超連結時，會將 NavigateUri 屬性的值傳遞給 URI 類型和配置的系統處理常式。 系統接著會啟動針對 NavigateUri 所提供的 URI 配置登錄的應用程式。

如果您不想要超連結在預設網頁瀏覽器中載入內容 (並且不想要顯示瀏覽器)，請不要設定 NavigateUri 的值， 而是處理 Click 事件，並且撰寫您所要執行之動作的程式碼。


**處理 Click 事件**

將 Click 事件用於在瀏覽器中啟動 URI 以外的動作 (例如，在應用程式內進行瀏覽)。 例如，如果您要載入新的應用程式頁面，而不是開啟瀏覽器，請在 Click 事件處理常式內呼叫 [Frame.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.frame.navigate.aspx) 方法，以瀏覽至新的應用程式頁面。 如果您要在 [WebView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.aspx) 控制項內載入也存在於您應用程式中的外部絕對 URI，請呼叫 [WebView.Navigate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webview.navigate.aspx) 做為 Click 處理常式邏輯的一部分。

您通常不會處理 Click 事件以及指定 NavigateUri 值，因為這兩者代表兩種使用超連結元素的不同方式。 如果您的目的是要在預設瀏覽器中開啟 URI，而且已指定 NavigateUri 值，請不要處理 Click 事件。 相反地，如果您處理 Click 事件，請不要指定 NavigateUri。

您在 Click 事件處理常式內無法執行任何動作來防止預設瀏覽器載入任何針對 NavigateUri 指定的有效目標；超連結已啟用但無法透過 Click 事件處理常式予以取消時，會自動 (非同步) 執行該動作。 

## <a name="hyperlink-underlines"></a>超連結底線
超連結預設會加上底線。 這個底線相當重要，因為它有助於符合協助工具需求。 色盲使用者使用底線來區別超連結與其他文字。 如果您停用底線，則應該考慮新增一些其他類型的格式差異，來區別超連結與其他文字 (例如 FontWeight 或 FontStyle)。

**Hyperlink 文字元素**

您可以設定 [UnderlineStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.hyperlink.underlinestyle.aspx) 屬性來停用底線。 如果這麼做，請考慮使用 [FontWeight](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontweight.aspx) 或 [FontStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.fontstyle.aspx) 來區別您的連結文字。

**HyperlinkButton** 

設定字串做為 [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx) 屬性的值時，HyperlinkButton 預設會顯示為加底線文字。

在下列情況下，文字不會加上底線︰
- 您將 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) 設為 Content 屬性的值，並在 TextBlock 上設定 [Text](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.text.aspx) 屬性。
- 您重新建立 HyperlinkButton 的範本，以及變更 [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) 範本組件的名稱。

如果您需要按鈕顯示為非底線文字，請考慮使用標準 Button 控制項，並將內建 `TextBlockButtonStyle` 系統資源套用至其 Style 屬性。

## <a name="notes-for-hyperlink-text-element"></a>Hyperlink 文字元素的附註

本節僅適用於 Hyperlink 文字元素，而非 HyperlinkButton 控制項。

**輸入事件**

因為 Hyperlink 不是 [UIElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.aspx)，所以沒有一組 UI 元素輸入事件 (例如 Tapped、PointerPressed 等)。 相反地，Hyperlink 有它自己的 Click 事件，以及載入任何指定為 NavigateUri 之 URI 的系統的隱含行為。 系統會處理應該叫用 Hyperlink 動作的所有輸入動作，並引發 Click 事件予以回應。

**內容**

Hyperlink 對可存在於其 [Inlines](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.inlines.aspx) 集合中的內容有所限制。 具體而言，Hyperlink 僅允許 [Run](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.run.aspx) 以及不是另一個 Hyperlink 的其他 [Span]() 類型。 [InlineUIContainer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.inlineuicontainer.aspx) 不能在 Hyperlink 的 Inlines 集合中。 嘗試新增限制內容，會擲回無效引數例外狀況或 XAML 剖析例外狀況。

**Hyperlink 和佈景主題/樣式行為**

Hyperlink 不是繼承自 [Control](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.aspx)，因此沒有 [Style](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.style.aspx) 屬性或 [Template](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.template.aspx)。 您可以編輯繼承自 [TextElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.aspx) 的屬性 (例如 Foreground 或 FontFamily) 來變更 Hyperlink 的外觀，但無法使用常用樣式或範本來套用變更。 請考慮使用常見資源做為 Hyperlink 屬性的值以提供一致性，而不是使用範本。 部分 Hyperlink 屬性使用系統所提供 {ThemeResource} 標記延伸值的預設值。 使用者在執行階段變更系統佈景主題時，這會以適當的方式切換 Hyperlink 外觀。

超連結的預設色彩是系統的輔色。 您可以設定 [Foreground](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.textelement.foreground.aspx) 屬性來覆寫這個值。

## <a name="recommendations"></a>建議

-   只使用超連結來瀏覽；不要用於其他動作。
-   針對文字型超連結，請使用字體坡形中的本文樣式。 深入了解[字型和 Windows 10 字體坡形](../style/typography.md)。
-   在各個超連結之間保持足夠距離，讓使用者能分辨它們，並容易選取每個超連結。
-   在超連結新增工具提示，以指示使用者將被導向的位置。 如果使用者將被導向至外部網站，請在工具提示包含最上層網域名稱，並將文字樣式設為次要字型色彩。

## <a name="get-the-sample-code"></a>取得範例程式碼

- [XAML 控制項庫範例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以互動式格式查看所有 XAML 控制項。

## <a name="related-articles"></a>相關文章

- [文字控制項](text-controls.md)
- [工具提示的指導方針](tooltips.md)

**適用於開發人員 (XAML)**
- [Windows.UI.Xaml.Documents.Hyperlink 類別](https://msdn.microsoft.com/library/windows/apps/dn279356)
- [Windows.UI.Xaml.Controls.HyperlinkButton 類別](https://msdn.microsoft.com/library/windows/apps/br242739)
