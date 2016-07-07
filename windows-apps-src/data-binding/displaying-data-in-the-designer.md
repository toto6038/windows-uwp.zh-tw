---
author: mcleblanc
ms.assetid: 089660A2-7CAE-4911-9994-F619C5D22287
title: "設計介面上適用於原型設計的範例資料"
description: "針對您的 app，(基於隱私權或效能因素) 可能無法或不希望在 Microsoft Visual Studio 或 Blend for Visual Studio 中的設計介面上顯示即時資料。"
translationtype: Human Translation
ms.sourcegitcommit: 53e807c0d9de8faf2d0b5dc0e1c8e9c380e42d86
ms.openlocfilehash: 2f7ac4b269a167c3b521fa94d77e27091fa490a8

---
設計介面上適用於原型設計的範例資料
=============================================================================================

\[ 針對 Windows 10 上的 UWP app 更新。 如需 Windows 8.x 文章，請參閱[封存](http://go.microsoft.com/fwlink/p/?linkid=619132) \]



            **注意：**您需要範例資料的程度以及它能夠為您提供多少協助，取決於您的繫結是否使用 [{Binding} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204782)或 [{x:Bind} 標記延伸](https://msdn.microsoft.com/library/windows/apps/Mt204783)。 本主題中所述的技術是以 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 的使用方式為依據，因此，它們只適用於 **{Binding}**。 但是，如果您使用的是 **{x:Bind}**，則您的繫結至少會在設計介面上顯示預留位置值 (即使是針對項目控制項也一樣)，因此，您的需求不需要與範例資料完全相同。

針對您的 app，(基於隱私權或效能因素) 可能無法或不希望在 Microsoft Visual Studio 或 Blend for Visual Studio 中的設計介面上顯示即時資料。 為了讓您的控制項能夠填入資料 (讓您能夠在 app 的配置、範本及其他視覺化屬性上運作)，系統提供了各種不同方式，讓您可以使用設計階段的範例資料。 如果您正在建置草圖 (或原型) app，則範例資料也可以是非常實用且省時的。 您可以在執行階段於草圖或原型中使用範例資料來說明您的想法，而不需連線到實際的即時資料。

在標記中設定 DataContext
-----------------------------

有一個相當常見的開發人員做法是，使用命令式程式碼 (在程式碼後置中) 來設定頁面或使用者控制項的 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)，以檢視模型執行個體。

``` csharp
public MainPage()
{
    InitializeComponent();
    this.DataContext = new BookstoreViewModel();
}
```

但是，如果您要這樣做，則您的頁面就不能和它原本一樣是「可設計的」。 原因在於當您在 Visual Studio 或 Blend for Visual Studio 中開啟 XAML 頁面時，永遠不會執行命令式程式碼來指派 **DataContext** 值 (事實上，不會執行您的任何程式碼後置)。 XAML 工具當然會剖析您的標記並將其中宣告的任何物件具現化，但它們實際上不會將您的頁面類型本身具現化。 結果就是您將不會在控制項或 \[建立資料繫結\] 對話方塊中看見任何資料，而您頁面的樣式和配置將更具挑戰性。

![疏鬆設計 UI。](images/displaying-data-in-the-designer-01.png)

第一個解決方式是嘗試將 **DataContext** 指派標記為註解，並改為在頁面標記中設定 **DataContext**。 如此一來，您的即時資料就會在設計階段和執行階段顯示。 若要這樣做，請先開啟您的 XAML 頁面。 接著，在 \[文件大綱\] 視窗中，按一下根設計元素 (通常會與標籤 \[頁面\] 一起) 來選取它。 在 \[屬性\] 視窗中，尋找 DataContext 屬性 (位於常用類別中)，然後按一下 \[新增\]。 在 \[選取物件\] 對話方塊中按一下您的檢視模型類型，然後按一下 \[確定\]。

![用於設定 DataContext 的 UI。](images/displaying-data-in-the-designer-02.png)

以下是所產生的標記外觀。

``` xaml
<Page ... >
    <Page.DataContext>
        <local:BookstoreViewModel/>
    </Page.DataContext>
```

而此處是您的繫結目前可解析的設計介面外觀。 請注意，目前已根據 DataContext 類型及您可繫結的屬性來填入 \[建立資料繫結\] 對話方塊中的 \[路徑\] 選擇器。

![可設計的 UI。](images/displaying-data-in-the-designer-03.png)

\[建立資料繫結\] 對話方塊只需要可從中運作的類型，但是繫結需要使用值來初始化的屬性。 如果您不想在設計階段期間連接到雲端服務 (基於效能、付費資料傳輸、隱私權問題、這類事項等因素)，則您的初始化程式碼會進行檢查，以查看您的 app 是否正在設計工具 (例如 Visual Studio 或 Blend for Visual Studio) 中執行，並且會在該情況下載入僅供設計階段使用的範例資料。

``` csharp
if (Windows.ApplicationModel.DesignMode.DesignModeEnabled)
{
    // Load design-time books.
}
else
{
    // Load books from a cloud service.
}
```

如果您需要將參數傳遞到初始化程式碼，您可以使用檢視模型尋找程式。 檢視模型尋找程式是您可以放入 app 資源的類別。 它具有公開檢視模型的屬性，而您頁面的 **DataContext** 會繫結至該屬性。 尋找程式或檢視模型可使用的另一種模式是相依性插入，可在適當時建構設計階段或執行階段的資料提供者 (每個都會實作常見的介面)。

「來自類別的範例資料」和設計階段屬性
---------------------------------------------------------------------------------------

如果基於任何因素而導致上節中的所有選項都不適合您的情況，則您仍然可透過 XAML 工具的功能和設計階段屬性來使用許多設計階段資料選項。 Blend for Visual Studio 中的 \[從類別建立範例資料\] 功能就是一個很好的選項。 您可以在 \[資料\] 面板頂端的其中一個按鈕中找到該命令。

而您只需指定要使用的命令類別即可。 命令接著會為您執行兩個重要動作。 首先，它會產生 XAML 檔案，其中包含適用於 以遞迴方式合成您所選擇的類別及其所有成員的範例資料 (事實上，工具同樣適用於 XAML 或 JSON 檔案)。 其次，它會在 \[資料\] 面板中填入您所選擇之類別的結構描述。 接著您可以將成員從 \[資料\] 面板拖曳到設計介面來執行各種工作。 根據您的拖曳項目及放置的位置，您可以將繫結新增到現有的控制項 (使用 **{Binding}**)，或建立新的控制項並同時繫結它們。 在這任一種情況下，如果您尚未設定其中一個，操作也會在頁面的配置根目錄中設定設計階段資料內容 (**d:DataContext**)。 這個設計階段資料內容會使用 **d:DesignData** 屬性，從產生的 XAML 檔案中取得範例資料 (此外，您可以隨意在專案中尋找並編輯，以使其包含您想要的範例資料)。

``` xaml
<Page ...
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <Grid ... d:DataContext="{d:DesignData /SampleData/RecordingViewModelSampleData.xaml}"/>
        <ListView ItemsSource="{Binding Recordings}" ... />
        ...
    </Grid>
</Page>
```

各種不同的 xmlns 宣告表示，只有在設計階段才會解譯具有 **d:** 首碼的屬性，在執行階段則會略過。 因此，**d:DataContext** 屬性只會在設計階段影響 [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) 屬性的值，在執行階段則沒有任何作用。 如有需要，您甚至可以在標記中同時設定 **d:DataContext** 和 **DataContext**。 
            **d:DataContext** 將會在設計階段覆寫，而 **DataContext** 將會在執行階段覆寫。 這些相同的覆寫規則適用於所有的設計階段和執行階段屬性。


            **d:DataContext** 屬性及其他所有設計階段屬性都記載於[設計階段屬性](http://go.microsoft.com/fwlink/p/?LinkId=272504)主題中，其仍適用於通用 Windows 平台 (UWP) app。


            [
              **CollectionViewSource**
            ](https://msdn.microsoft.com/library/windows/apps/BR209833) 沒有 **DataContext** 屬性，但具有 **Source** 屬性。 因此，您可以使用 **d:Source** 屬性，在 **CollectionViewSource** 上設定僅限設計階段的範例資料。

``` xaml
    <Page.Resources>
        <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
            d:Source="{d:DesignData /SampleData/RecordingsSampleData.xaml}"/>
    </Page.Resources>

    ...

        <ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}" ... />
    ...
```

若要讓此設定運作，您必須有一個名為 `Recordings : ObservableCollection<Recording>` 的類別，而您要編輯範例資料 XAML 檔案，使其只包含**錄製**物件 (且其中包含**錄製**物件)，如下所示。

``` xml
<Quickstart:Recordings xmlns:Quickstart="using:Quickstart">
    <Quickstart:Recording ArtistName="Mollis massa" CompositionName="Cubilia metus"
        OneLineSummary="Morbi adipiscing sed" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Vulputate nunc" CompositionName="Parturient vestibulum"
        OneLineSummary="Dapibus praesent netus amet vestibulum" ReleaseDateTime="01/01/1800 15:53:17"/>
    <Quickstart:Recording ArtistName="Phasellus accumsan" CompositionName="Sit bibendum"
        OneLineSummary="Vestibulum egestas montes dictumst" ReleaseDateTime="01/01/1800 15:53:17"/>
</Quickstart:Recordings>
```

如果您使用 JSON 範例資料檔案而不是 XAML，就必須設定 **Type** 屬性。

``` xaml
    d:Source="{d:DesignData /SampleData/RecordingsSampleData.json, Type=local:Recordings}"
```

到目前為止，我們已經使用了 **d:DesignData**，從 XAML 或 JSON 檔案載入設計階段範例資料。 替代方法是 **d:DesignInstance** 標記延伸，這會指出設計階段來源是以 **Type** 屬性指定的類別為根據。 這裡提供一個範例。

``` xaml
    <CollectionViewSource x:Name="RecordingsCollection" Source="{Binding Recordings}"
        d:Source="{d:DesignInstance Type=local:Recordings, IsDesignTimeCreatable=True}"/>
```


            **IsDesignTimeCreatable** 屬性會指出設計工具應該實際建立類別的執行個體，表示這個類別具有公用的預設建構函式，以及它會在其本身中填入資料 (真實或範例)。 如果您沒有設定 **IsDesignTimeCreatable** (或將它設定為 **False**)，則將無法取得設計介面上顯示的範例資料。 在這個案例中，設計工具唯一要做的就是剖析適用於其可繫結屬性的類別，並在 \[資料\] 面板和 \[建立資料繫結\] 對話方塊中顯示它們。

適用於原型設計的範例資料
--------------------------------------------------------

針對原型設計，您會想要在設計階段和執行階段使用範例資料。 針對這個使用案例，Blend for Visual Studio 具備 \[新增範例資料\] 功能。 您可以在 \[資料\] 面板頂端的其中一個按鈕中找到該命令。

您可以在 \[資料\] 面板中直接實際設計範例資料的結構描述，而不需指定類別。 您也可以在 \[資料\] 面板中編輯範例資料值：不需開啟和編輯檔案 (儘管如此，如有需要，您仍然可以執行該動作)。

\[新增範例資料\] 功能會使用 DataContext 而非 d:DataContext，因此，當您執行草圖或原型時以及在設計期間就能使用範例資料。 此外，\[資料\] 面板確實能夠加速您的設計和繫結工作。 例如，只要將集合屬性從 \[資料\] 面板拖曳到設計介面，就能產生資料繫結項目控制項和必要的範本，來建置並執行所有就緒的項目。

![適用於原型設計的範例資料。](images/displaying-data-in-the-designer-04.png)



<!--HONumber=Jun16_HO4-->


