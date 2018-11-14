---
author: jwmsft
Description: Use content links to embed rich data in your text controls.
title: 文字控制項中的內容連結
label: Content links
template: detail.hbs
ms.author: jimwalk
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3939995aa2f29f4590c8c71a877b69f0cb81d2ec
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2018
ms.locfileid: "6147864"
---
# <a name="content-links-in-text-controls"></a>文字控制項中的內容連結

內容連結提供可在文字控制項中嵌入豐富資料的方法，讓使用者不離開應用程式的內容也能尋找和使用有關個人或位置的詳細資訊。

當使用者在 RichEditBox 中的項目開頭加上 @ 符號時，系統會向他們顯示符合該項目的連絡人和/或地點建議。 例如，使用者接著選擇某個地點時，這個地點的 ContentLink 將會插入文字中。 當使用者從 RichEditBox 叫用內容連結時，飛出視窗會出現，並顯示地圖以及該地點的其他資訊。

> **重要 API**：[ContentLink 類別](/uwp/api/windows.ui.xaml.documents.contentlink)、[ContentLinkInfo 類別](/uwp/api/windows.ui.text.contentlinkinfo)、[RichEditTextRange 類別](/uwp/api/windows.ui.text.richedittextrange)

> [!NOTE]
> 內容連結的 Api 會分散在下列命名空間： Windows.UI.Xaml.Controls、 Windows.UI.Xaml.Documents 和 Windows.UI.Text。



## <a name="content-links-in-rich-edit-vs-text-block-controls"></a>Rich Edit 與文字區塊控制項中的內容連結

有兩種使用內容連結的不同方式：

1. 在 [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) 中，使用者可以藉由在文字開頭加上 @，開啟選擇器以新增內容連結。 內容連結會儲存為 RTF 內容的一部分。
1. 在 [TextBlock](/uwp/api/windows.ui.xaml.controls.textblock) 或 [RichTextBlock](/uwp/api/windows.ui.xaml.controls.richtextblock) 中，連結內容是文字元素，其用法和行為就像[超連結](/uwp/api/windows.ui.xaml.documents.hyperlink)一樣。

以下是內容連結在 RichEditBox 和 TextBlock 中的預設外觀。

![Rich Edit ](images/content-link-default-richedit.png)
![文字方塊中的內容連結](images/content-link-default-textblock.png)

下列章節詳細說明用法、轉譯和行為上的差異。 本表提供內容連結在 RichEditBox 與文字區塊之間主要差異的快速比較。

| 功能   | RichEditBox | 文字區塊 |
| --------- | ----------- | ---------- |
| 用法 | ContentLinkInfo 執行個體 | ContentLink 文字元素 |
| 游標 | 取決於內容連結的類型，無法變更 | 取決於 Cursor 屬性，預設為 **null** |
| ToolTip | 未轉譯 | 顯示次要文字 |

## <a name="enable-content-links-in-a-richeditbox"></a>在 RichEditBox 中啟用內容連結

內容連結最常見的用法是，讓使用者在其文字中連絡人或地點名稱的前面加上 @ 符號，以快速新增資訊。 在 [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) 中啟用時，這會開啟選擇器，讓使用者插入其連絡人清單的人員或附近的地點 (視您所啟用的類型而定)。

內容連結可以和 RTF 內容一起儲存，而且您可以進行擷取以用於應用程式的其他部分。 例如在電子郵件應用程式中，您可能會擷取個人資訊，並用來填入 [收件者] 方塊的電子郵件地址。

> [!NOTE]
> 內容連結選擇器是屬於 Windows 的應用程式，因此會在您的應用程式以外的個別處理程序中執行。

### <a name="content-link-providers"></a>內容連結提供者

您可以藉由將一個或多個內容連結提供者新增至 [RichEditBox.ContentLinkProviders](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkProviders) 集合，在 RichEditBox 中啟用內容連結。 有 2 個內容連結提供者內建於 XAML 架構。

- [ContactContentLinkProvider](/uwp/api/windows.ui.xaml.documents.contactcontentlinkprovider)：使用 **\[連絡人\]** App 查詢連絡人。
- [PlaceContentLinkProvider](/uwp/api/windows.ui.xaml.documents.placecontentlinkprovider)：使用 **\[地圖\]** App 查詢地點。

> [!IMPORTANT]
> RichEditBox.ContentLinkProviders 屬性的預設值是 **null**，而非空集合。 您必須先明確建立 [ContentLinkProviderCollection](/uwp/api/windows.ui.xaml.documents.contentlinkprovidercollection)，再新增內容連結提供者。

以下是在 XAML 中新增內容連結提供者的方式。

```xaml
<RichEditBox>
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <ContactContentLinkProvider/>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>
```

您也可以在 Style 中新增內容連結提供者，然後像這樣套用至多個 RichEditBox。

```xaml
<Page.Resources>
    <Style TargetType="RichEditBox" x:Key="ContentLinkStyle">
        <Setter Property="ContentLinkProviders">
            <Setter.Value>
                <ContentLinkProviderCollection>
                    <PlaceContentLinkProvider/>
                    <ContactContentLinkProvider/>
                </ContentLinkProviderCollection>
            </Setter.Value>
        </Setter>
    </Style>
</Page.Resources>

<RichEditBox x:Name="RichEditBox01" Style="{StaticResource ContentLinkStyle}" />
<RichEditBox x:Name="RichEditBox02" Style="{StaticResource ContentLinkStyle}" />
```

以下是在程式碼中新增內容連結提供者的方式。

```csharp
RichEditBox editor = new RichEditBox();
editor.ContentLinkProviders = new ContentLinkProviderCollection
{
    new ContactContentLinkProvider(),
    new PlaceContentLinkProvider()
};
```

### <a name="content-link-colors"></a>內容連結色彩

內容連結的外觀取決於其前景、背景和圖示。 在 RichEditBox 中，您可以設定 [ContentLinkForegroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkForegroundColor) 和 [ContentLinkBackgroundColor](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkBackgroundColor) 屬性來變更預設色彩。 

您無法設定游標。 游標是根據內容連結的類型 ([人員](/uwp/api/windows.ui.core.corecursortype)游標適用於人員連結，或是[圖釘](/uwp/api/windows.ui.core.corecursortype)游標適用於地點連結) 由 RichEditbox 轉譯。

### <a name="the-contentlinkinfo-object"></a>ContentLinkInfo 物件

當使用者在連絡人或地點選擇器中進行選取時，系統會建立 [ContentLinkInfo](/uwp/api/windows.ui.text.contentlinkinfo) 物件，並將其新增至目前 [RichEditTextRange](/uwp/api/windows.ui.text.richedittextrange) 的 **ContentLinkInfo** 屬性。

ContentLinkInfo 物件包含用來顯示、叫用和管理內容連結的資訊。

- **DisplayText**：這是轉譯內容連結時顯示的字串。 在 RichEditBox 中，使用者可以在內容連結建立後編輯其文字，這會變更此屬性的值。
- **SecondaryText**：此字串顯示在所轉譯內容連結的工具提示中。
  - 在選擇器所建立的地點內容連結中，它包含位置的地址 (如果有的話)。
- **Uri**：內容連結主題詳細資訊的連結。 此 URI 可以開啟已安裝的應用程式或網站。
- **Id**：這是 RichEditBox 控制項所建立個別控制項的唯讀計數器。 這會在動作 (例如刪除或編輯) 期間用來追蹤此 ContentLinkInfo。 如果將 ContentLinkInfo 剪下並貼到控制項中，它將會取得新的識別碼，識別碼值是增量。
- **LinkContentKind**：描述內容連結類型的字串。 建內容類型是_地點_和_連絡人_。 此值要區分大小寫。

#### <a name="link-content-kind"></a>連結內容類型

有幾種情況下，LinkContentKind 會顯得很重要。

- 當使用者複製 RichEditBox 中的內容連結，再將它貼到另一個 RichEditBox 時，這兩個控制項都必須有該內容類型的 ContentLinkProvider。 如果沒有，則會將連結貼上為文字。
- 您可以使用 [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) 處理常式中的 LinkContentKind，判斷在應用程式其他部分中使用內容連結時要如何處理它。 如需詳細資訊，請參閱「範例」區段。
- LinkContentKind 會影響叫用連結時系統如何開啟 URI。 我們將會在下次討論 URI 啟動時看到這個結果。

#### <a name="uri-launching"></a>URI 啟動

Uri 屬性的作用很像超連結的 NavigateUri 屬性。 當使用者按一下時，它會啟動預設瀏覽器中的 URI，或註冊 Uri 值所指定特定通訊協定之應用程式中的 URI。

以下說明 2 個內建連結內容類型的行為。

##### <a name="places"></a>Places

地點選擇器會使用 https://maps.windows.com/ 的 URI 根路徑來建立 ContentLinkInfo。 有 3 種方式可以開啟此連結：

- 如果 LinkContentKind = "Places"，則在飛出視窗中開啟資訊卡片。 飛出視窗類似於內容連結選擇器飛出視窗。 這是 Windows 的一部分，並且會在您應用程式以外的個別處理序中執行。
- 如果 LinkContentKind 不是 "Places"，就會嘗試開啟 **\[地圖\]** App 進入指定的位置。 例如，如果您在 ContentLinkChanged 事件處理常式中修改了 LinkContentKind，就會發生這種情況。
- 如果 [地圖] App 無法開啟 URI，則會在預設瀏覽器中開啟地圖。 這通常會在使用者的_網站應用程式_設定不允許使用 **\[地圖\]** App 開啟 URI 時發生。

##### <a name="people"></a>People

連絡人選擇器會使用採用 **ms-people** 通訊協定的 URI 來建立 ContentLinkInfo。

- 如果 LinkContentKind = "People"，則在飛出視窗中開啟資訊卡片。 飛出視窗類似於內容連結選擇器飛出視窗。 這是 Windows 的一部分，並且會在您應用程式以外的個別處理序中執行。
- 如果 LinkContentKind 不是 "People"，則會開啟 **\[連絡人\]** App。 例如，如果您在 ContentLinkChanged 事件處理常式中修改了 LinkContentKind，就會發生這種情況。

> [!TIP]
> 如需從您的應用程式開啟其他應用程式和網站的詳細資訊，請參閱下[啟動 uri 的應用程式](/windows/uwp/launch-resume/launch-app-with-uri)的主題。

#### <a name="invoked"></a>Invoked

當使用者叫用內容連結時，將會在啟動 URI 的預設行為發生之前引發 [ContentLinkInvoked](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkInvoked) 事件。 您可以處理此事件來覆寫或取消預設行為。

這個範例顯示如何覆寫地點內容連結的預設啟動行為。 您不要在地點資訊卡片、[地圖] App 或預設網頁瀏覽器中開啟地圖，反而可以將事件標示為 [已處理] 並在應用程式內的 [WebView](/uwp/api/windows.ui.xaml.controls.webview) 控制項中開啟地圖。

```xaml
<RichEditBox x:Name="editor"
             ContentLinkInvoked="editor_ContentLinkInvoked">
    <RichEditBox.ContentLinkProviders>
        <ContentLinkProviderCollection>
            <PlaceContentLinkProvider/>
        </ContentLinkProviderCollection>
    </RichEditBox.ContentLinkProviders>
</RichEditBox>

<WebView x:Name="webView1"/>
```

```csharp
private void editor_ContentLinkInvoked(RichEditBox sender, ContentLinkInvokedEventArgs args)
{
    if (args.ContentLinkInfo.LinkContentKind == "Places")
    {
        args.Handled = true;
        webView1.Navigate(args.ContentLinkInfo.Uri);
    }
}
```

#### <a name="contentlinkchanged"></a>ContentLinkChanged

您可以使用 [ContentLinkChanged](/uwp/api/windows.ui.xaml.controls.richeditbox.ContentLinkChanged) 事件，在有內容連結新增、修改或移除時收到通知。 這可讓您從文字擷取內容連結，然後在應用程式的其他位置使用此連結。 稍後會在「範例」一節中示範這個動作。

[ContentLinkChangedEventArgs](/uwp/api/windows.ui.xaml.controls.contentlinkchangedeventargs) 的屬性包括：

- **ChangedKind**：此 ContentLinkChangeKind 列舉是 ContentLink 中的動作。 例如，是插入、移除還是編輯 ContentLink。
- **Info**：成了動作的目標的 ContentLinkInfo。

此事件是針對每個 ContentLinkInfo 動作所引發。 例如，如果使用者一次將數個內容連結複製並貼到 RichEditBox 中，則每個新增項目都會引發此事件。 或者，如果使用者同時選取並刪除數個內容連結，則每個刪除項目都會引發此事件。

RichEditBox 會在 **TextChanging** 事件之後和 **TextChanged** 事件之前引發此事件。

#### <a name="enumerate-content-links-in-a-richeditbox"></a>列舉 RichEditBox 中的內容連結

您可以使用 RichEditTextRange.ContentLink 屬性，從 Rich Edit 文件取得內容連結。 TextRangeUnit 列舉含有的值，ContentLink 會用來將內容連結指定做為在瀏覽文字範圍時所要使用的單位。

此範例顯示如何列舉 RichEditBox 中所有的內容連結，以及將連絡人擷取到清單中。

```xaml
<StackPanel Width="300">
    <RichEditBox x:Name="Editor" Height="200">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <Button Content="Get people" Click="Button_Click"/>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}" Background="Transparent"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    PeopleList.Items.Clear();
    RichEditTextRange textRange = Editor.Document.GetRange(0, 0) as RichEditTextRange;

    do
    {
        // The Expand method expands the Range EndPosition 
        // until it finds a ContentLink range. 
        textRange.Expand(TextRangeUnit.ContentLink);
        if (textRange.ContentLinkInfo != null
            && textRange.ContentLinkInfo.LinkContentKind == "People")
        {
            PeopleList.Items.Add(textRange.ContentLinkInfo);
        }
    } while (textRange.MoveStart(TextRangeUnit.ContentLink, 1) > 0);
}
```

## <a name="contentlink-text-element"></a>ContentLink 文字元素

若要在 TextBlock 或 RichTextBlock 控制項中使用內容連結，請使用 [ContentLink](/uwp/api/windows.ui.xaml.documents.contentlink) 文字元素 (在 Windows.UI.Xaml.Documents 命名空間中)。

文字區塊中 ContentLink 的一般來源是：

- 選擇器所建立從 RichTextBlock 控制項擷取的內容連結。
- 您在程式碼中建立的地點內容連結。

超連結文字元素通常適用於其他情況。

### <a name="contentlink-appearance"></a>ContentLink 外觀

內容連結的外觀取決於其前景、背景和游標。 在文字區塊中，您可以設定 Foreground (來自 TextElement) 和 [Background](/uwp/api/windows.ui.xaml.documents.contentlink.background) 屬性來變更預設色彩。

當使用者將滑鼠指標停留在內容連結上方時，預設會顯示[手形](/uwp/api/windows.ui.core.corecursortype)游標。 與 RichEditBlock 不同，文字區塊控制項並不會自動根據連結類型變更游標。 您可以設定 [Cursor](/uwp/api/windows.ui.xaml.documents.contentlink.Cursor) 屬性，根據連結類型或其他因素來變更游標。

以下是在 TextBlock 中使用 ContentLink 的範例。 ContentLinkInfo 是在程式碼中建立，然後指派給使用 XAML 所建立之 ContentLink 文字元素的 Info 屬性。

```xaml
<StackPanel>
    <TextBlock>
        <Span xml:space="preserve">
            <Run>This valcano erupted in 1980: </Run><ContentLink x:Name="placeContentLink" Cursor="Pin"/>
            <LineBreak/>
        </Span>
    </TextBlock>

    <Button Content="Show" Click="Button_Click"/>
</StackPanel>
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    var info = new Windows.UI.Text.ContentLinkInfo();
    info.DisplayText = "Mount St. Helens";
    info.SecondaryText = "Washington State";
    info.LinkContentKind = "Places";
    info.Uri = new Uri("https://maps.windows.com?cp=46.1912~-122.1944");
    placeContentLink.Info = info;
}
```

> [!TIP]
> 當您在包含其他 XAML 文字元素的文字控制項中使用 ContentLink 時，將內容置入 [Span](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.documents.span.aspx) 容器，然後將 `xml:space="preserve"` 屬性套用至 Span 以保留 ContentLink 與其他元素之間的空白字元。

## <a name="examples"></a>範例

在此範例中，使用者可以將人員或地點內容連結輸入到 RickTextBlock 中。 您可以處理 ContentLinkChanged 事件，來擷取內容連結，並且讓這些連結在連絡人清單或地點清單中保持最新狀態。

在清單的項目範本中，您可以將 TextBlock 與 ContentLink 文字元素搭配用來顯示內容連結資訊。 ListView 會將本身的背景提供給每個項目，因此 ContentLink 背景設定為 Transparent。

```xaml
<StackPanel Orientation="Horizontal" Grid.Row="1">
    <RichEditBox x:Name="Editor"
                 ContentLinkChanged="Editor_ContentLinkChanged"
                 Margin="20" Width="300" Height="200"
                 VerticalAlignment="Top">
        <RichEditBox.ContentLinkProviders>
            <ContentLinkProviderCollection>
                <ContactContentLinkProvider/>
                <PlaceContentLinkProvider/>
            </ContentLinkProviderCollection>
        </RichEditBox.ContentLinkProviders>
    </RichEditBox>

    <ListView x:Name="PeopleList" Header="People">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Person"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>

    <ListView x:Name="PlacesList" Header="Places">
        <ListView.ItemTemplate>
            <DataTemplate x:DataType="ContentLinkInfo">
                <TextBlock>
                    <ContentLink Info="{x:Bind}"
                                 Background="Transparent"
                                 Cursor="Pin"/>
                </TextBlock>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>
</StackPanel>
```

```csharp
private void Editor_ContentLinkChanged(RichEditBox sender, ContentLinkChangedEventArgs args)
{
    var info = args.ContentLinkInfo;
    var change = args.ChangeKind;
    ListViewBase list = null;

    // Determine whether to update the people or places list.
    if (info.LinkContentKind == "People")
    {
        list = PeopleList;
    }
    else if (info.LinkContentKind == "Places")
    {
        list = PlacesList;
    }

    // Determine whether to add, delete, or edit.
    if (change == ContentLinkChangeKind.Inserted)
    {
        Add();
    }
    else if (args.ChangeKind == ContentLinkChangeKind.Removed)
    {
        Remove();
    }
    else if (change == ContentLinkChangeKind.Edited)
    {
        Remove();
        Add();
    }

    // Add content link info to the list. It's bound to the
    // Info property of a ContentLink in XAML.
    void Add()
    {
        list.Items.Add(info);
    }

    // Use ContentLinkInfo.Id to find the item,
    // then remove it from the list using its index.
    void Remove()
    {
        var items = list.Items.Where(i => ((ContentLinkInfo)i).Id == info.Id);
        var item = items.FirstOrDefault();
        var idx = list.Items.IndexOf(item);

        list.Items.RemoveAt(idx);
    }
}
```
