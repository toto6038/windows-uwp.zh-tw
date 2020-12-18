---
title: 建立資料繫結
description: 了解如何遵循此教學課程，使用 XAML 和 C# 建立資料繫結以在 UI 與資料之間形成直接連結。
keywords: XAML, UWP, 開始使用
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f84ba4e77369730ef9883ab7f14b52751182b773
ms.sourcegitcommit: 40b890c7b862f333879887cc22faff560c49eae6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97598829"
---
# <a name="tutorial-create-data-bindings"></a>教學課程：建立資料繫結

假設您設計並實作了美觀的 UI，其中已填入預留位置影像、「lorem ipsum」重複使用文字以及還沒有任何作用的控制項。 接下來，您就會想要將它連接到實際資料，並將其由設計原型轉換成實用應用程式。

在本教學課程中，您將了解如何將重複使用文字取代成資料繫結，以及建立 UI 與資料之間的其他直接連結。 您也將瞭解如何格式化或轉換資料以供顯示，以及讓 UI 和資料保持同步。當您完成本教學課程時，您將能夠改善 XAML 和 C# 程式碼的簡單性和組織，讓您更輕鬆地進行維護和擴充。

您將會從簡化版的 PhotoLab 範例開始著手。 這個簡易使用包含完整的資料層以及基本的 XAML 頁面配置，但省去次要功能，讓程式碼更易於瀏覽。 本教學課程不會一直建置到完整的應用程式，因此請務必查看最終版本來了解各項功能，例如自訂動畫和彈性配置。 您可以在 [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab) 存放庫的根資料夾中找到最終版本。

PhotoLab 範例應用程式有兩個頁面。 「主頁面」  會顯示影像中心檢視，以及一些關於每個影像檔案的資訊。

![Photolab 主頁面的螢幕擷取畫面。](../design/basics/images/xaml-basics/mainpage.png)

「詳細資料頁面」  會在選取單一相片之後，顯示該相片。 飛出視窗編輯功能表可用來變更、重新命名和儲存相片。

![Photolab 詳細頁面的螢幕擷取畫面。](../design/basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>必要條件

+ Visual Studio 2019：[下載 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (此為免費的社群版本)。
+ Windows 10 SDK (10.0.17763.0 或更新版本)：[下載最新 Windows SDK (免費)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10，版本 1809 或更新版本

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：從 GitHub 取得起始程式碼

針對此教學課程，您將從簡化版的 PhotoLab 範例開始著手。

1. 移至 GitHub 範例頁面：[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。
2. 下一步，您將需要複製或下載範例。 選取 [複製或下載]  按鈕。 子功能表會出現。
    ![PhotoLab 範例的 GitHub 頁面上的複製或下載功能表](../design/basics/images/xaml-basics/clone-repo.png)

    **如果您不熟悉 GitHub：**

    a. 選取 [下載 ZIP]  ，然後在本機儲存此檔案。 這個下載的 .zip 檔案，包含您所需要的所有專案檔案。

    b. 將檔案解壓縮。 使用 [檔案總管] 瀏覽至您下載的 .zip 檔案，以滑鼠右鍵按一下檔案，然後選取 [解壓縮全部...]。

    c. 瀏覽至您範例的本機複製，並移至 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\data-binding` 目錄。

    **如果您熟悉 GitHub：**

    a. 在本機複製存放庫中的主要分支。

    b. 瀏覽至 `Windows-appsample-photo-lab\xaml-basics-starting-points\data-binding` 目錄。

3. 按兩下 `Photolab.sln`，在 Visual Studio 中開啟解決方案。

## <a name="part-1-replace-the-placeholders"></a>第 1 部分：取代預留位置

這裡會使用資料範本 XAML 建立一次性繫結，以顯示實際影像和影像中繼資料，而非顯示預留位置內容。

一次性繫結適用於唯讀不變的資料，意味著這些繫結有高效能且容易建立，可讓您在 `GridView`和 `ListView` 控制項中顯示大型資料集。

### <a name="replace-the-placeholders-with-one-time-bindings"></a>以一次性繫結取代預留位置

1. 開啟 `xaml-basics-starting-points\data-binding` 資料夾，然後在 Visual Studio 中啟動 `PhotoLab.sln` 檔案。

2. 確認 [方案平台] 已設定為 x86 或 x64 (而非 ARM)，然後執行應用程式。 這會在新增繫結之前，顯示包含 UI 預留位置的應用程式的狀態。

    ![執行包含預留位置影像和文字的應用程式](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. 開啟 MainPage.xaml 並搜尋名稱為 **ImageGridView_DefaultItemTemplate** 的 `DataTemplate`。 您會將這個範本更新為使用資料繫結。

    **之前：**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    `ImageGridView` 使用 `x:Key` 值來選取這個用於顯示資料物件的範本。

4. 將 `x:DataType` 值新增至範本。

    **之後：**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
    ```

    `x:DataType` 表示範本適用於哪一個類型。 在此案例中，這是適用於 `ImageFileInfo` 類別的範本 (其中 `local:` 表示本機命名空間，如檔案開頭處 xmlns 宣告中所定義)。

    在資料範本中使用 `x:DataType` 運算式時，需要 `x:Bind`，如下所述。

5. 在 `DataTemplate` 中，尋找名為 `Image` 的 `ItemImage` 元素，並依下述取代 `Source` 值。

    **之前：**

    ```xaml
    <Image x:Name="ItemImage"
           Source="/Assets/StoreLogo.png"
           Stretch="Uniform" />
    ```

    **之後：**

    ```xaml
    <Image x:Name="ItemImage"
           Source="{x:Bind ImageSource}"
           Stretch="Uniform" />
    ```

    `x:Name` 可識別 XAML 元素，因此您可以在 XAML 及程式碼後置中的其他位置參考它。

    `x:Bind` 運算式從 **data-object** 屬性取得值，並將該值提供給 UI 屬性。 在範本中，指示的屬性是 `x:DataType` 所設定類別的屬性。 因此，這個範例中的資料來源是 `ImageFileInfo.ImageSource` 屬性。

    > [!NOTE]
    > `x:Bind` 值也會讓編輯器得知資料類型，因此您可以利用 IntelliSense 來代替在 `x:Bind` 運算式中輸入屬性名稱。 在您剛貼入的程式碼中進行嘗試：直接將游標置於 `x:Bind` 後面，再按空格鍵，即可顯示您可繫結的屬性清單。

6. 以相同方式取代其他 UI 控制項的值。 (嘗試利用 IntelliSense 這樣做，而不要用複製/貼上的方式！)

    **之前：**

    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <muxc:RatingControl Value="3" ... />
    ```

    **之後：**

    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <muxc:RatingControl Value="{x:Bind ImageRating}" ... />
    ```

執行應用程式，看看到目前為止會是什麼樣子。 沒有其他預留位置了！ 我們有一個好的開始。

![在以實際影像和文字替代預留位置的情況下執行應用程式](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> 如果您想要進一步實驗，請嘗試將新的 TextBlock 加入資料範本，並使用 x:Bind IntelliSense 技巧尋找要顯示的屬性。

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>第 2 部分：使用繫結將圖庫 UI 連接至影像

這裡會在頁面 XAML 中建立一次性繫結，將圖庫檢視連接至影像集合，並取代在程式碼後置中執行此作業的現有程序性程式碼。 此外，還會建立 **刪除** 按鈕，用來查看圖庫檢視在影像自集合中移除時的變化情況。 在此同時，您將會了解如何將事件繫結至事件處理常式，以取得比傳統事件處理常式所能提供的還要大的彈性。

目前為止所探討的繫結都是在資料範本之內進行，並參考由 `x:DataType` 值所指示的類別的屬性。 頁面中 XAML 的其餘部分又會是怎樣呢？

資料範本以外的 `x:Bind` 運算式永遠繫結至頁面本身。 這表示您可以參考您放在程式碼後置中或宣告於 XAML 中的任何項目，包括自訂屬性以及頁面上其他 UI 控制項的屬性 (只要這些項目有 `x:Name` 值即可)。

在 PhotoLab 範例中，這類繫結的其中一個用途是將主要 `GridView` 控制項直接連接至影像集合，而不是在程式碼後置中執行此作業。 您稍後將會看到其他範例。

### <a name="bind-the-main-gridview-control-to-the-images-collection"></a>將主要 GridView 控制項繫結至影像集合

1. 在 MainPage.xaml.cs 中，尋找 `GetItemsAsync` 方法，並移除設定 `ItemsSource` 的程式碼。

    **之前：**

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    **之後：**

    ```csharp
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. 在 MainPage.xaml 中，尋找名為 `GridView` 的 `ImageGridView`，並新增 `ItemsSource` 屬性。 如需提供值，請使用參考程式碼後置中實作之 `Images` 屬性的 `x:Bind` 運算式。

    **之前：**

    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **之後：**

    ```xaml
    <GridView x:Name="ImageGridView"
              ItemsSource="{x:Bind Images}"
    ```

    `Images` 屬性的類型是 `ObservableCollection<ImageFileInfo>`，因此 `GridView` 中顯示的個別項目會是 `ImageFileInfo` 類型。 這符合第 1 部分中所述的 `x:DataType` 值。

到目前為止討論到的所有繫結都是一次性唯讀繫結，這是一般 `x:Bind` 運算式的預設行為。 資料只會在初始化時載入，這有助於高效能繫結，用來支援大型資料集的多個複雜檢視，再合適不過。

即使您剛新增的 `ItemsSource` 繫結是對不變屬性值的一次性唯讀繫結，但這裡有一個重要區別需要釐清。 `Images` 屬性的不變值是集合的單一特定執行個體，如這裡所示，已初始化一次。

```csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

`Images` 屬性的值永遠不會變更，但由於該屬性的類型為 `ObservableCollection<T>`，因此集合的「內容」可能會變更，且繫結將會自動發現變更並更新 UI。

為了測試這點，我們要暫時加入一個按鈕來刪除目前選取的影像。 這個按鈕不在最終版本中，因為選取影像就會帶您前往詳細資料頁面。 不過，`ObservableCollection<T>` 的行為在最終 PhotoLab 範例中仍然很重要，因為 XAML 是在頁面建構函式中進行初始化 (透過 `InitializeComponent` 方法呼叫)，而 `Images` 集合則是後來在 `GetItemsAsync` 方法中填入。

### <a name="add-a-delete-button"></a>新增刪除按鈕

1. 在 MainPage.xaml 中，尋找名為 `CommandBar` 的 **CommandBar**，並在縮放按鈕之前加入新的按鈕。 (縮放控制項尚無法運作。 您將會在教學課程的下一個部分連結這些按鈕。)

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    如果您已經熟悉 XAML，這個 `Click` 值可能會看起來很不尋常。 在舊版 XAML 中，您必須將此設定為使用特定事件處理常式特徵標記的方法，通常包括事件引發物件 (event sender) 的參數和事件特定引數物件。 當您需要事件引數時，仍然可以使用這個技術，但您也可以使用 `x:Bind` 連結到其他方法。 比如說，如果您不需要事件資料，則可以連接到沒有參數的方法，就像我們在這裡所做的。

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. 在 MainPage.xaml.cs 中，加入 `DeleteSelectedImage` 方法。

    ```csharp
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    這個方法只是單純從 `Images` 集合中刪除選取的影像。

現在請執行應用程式，並使用此按鈕刪除一些影像。 如您所見，由於資料繫結和 `ObservableCollection<T>` 類型的作用，UI 已自動更新。

> [!Note]
> 這段程式碼只會從執行中應用程式的 `Images` 集合中刪除 `ImageFileInfo` 執行個體。 其不會刪除電腦上的影像檔案。

## <a name="part-3-set-up-the-zoom-slider"></a>第 3 部分：設定縮放滑桿

在此部分，您將會建立單向繫結，從資料範本中的控制項繫結到範本之外的縮放滑桿。 您也將了解，不只是像 `TextBlock.Text` 和 `Image.Source` 這樣最顯而易見的屬性，您還可以使用資料繫結搭配許多控制項屬性。

### <a name="bind-the-image-data-template-to-the-zoom-slider"></a>將影像資料範本繫結至縮放滑桿

+ 尋找名為 `DataTemplate` 的 `ImageGridView_DefaultItemTemplate`，並取代範本最前面 `**Height**` 控制項的 `Width` 和 `Grid` 值。

    **之前**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```

    **之後**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```

是否注意到這些都是 `Binding` 運算式，而非 `x:Bind` 運算式？ 這是執行資料繫結的舊方式，多半已過時。 `x:Bind` 能做的，`Binding` 幾乎都能做，而且還做得更多。 不過，當您在資料範本中使用 `x:Bind` 時，它會繫結至 `x:DataType` 值所宣告的類型。 那麼如何將範本中的某個項目繫結至頁面 XAML 或程式碼後置中的某個項目呢？ 您必須使用舊式 `Binding` 運算式。

`Binding` 運算式無法辨識 `x:DataType` 值，但這些 `Binding` 運算式的 `ElementName` 值卻有幾乎同樣的作用。 這些值會告訴繫結引擎，**Binding Value** 是對頁面上指定元素 (也就是有 `x:Name` 值的元素) 之 `Value` 屬性的繫結。 如果您想要繫結至程式碼後置中的屬性，看起來會有點像 ```{Binding MyCodeBehindProperty, ElementName=page}```，其中 `page` 參考 XAML 中 `x:Name` 元素所設定的 `Page` 值。

> [!NOTE]
> 根據預設，Binding`Binding` 運算式是「單向」  繫結，表示這些運算式會自動在繫結屬性值變更時更新 UI。
>
> 相對地，x:Bind`x:Bind` 預設為「一次性」  繫結，表示會忽略繫結屬性的任何變更。 因為是最高效能選項，且大部分的繫結對象皆為靜態唯讀資料，所以才會預設這樣的繫結。
>
> 這裡學到經驗是，如果您對可能會變更值的屬性使用 `x:Bind`，請務必加入 `Mode=OneWay` 或 `Mode=TwoWay`。 您會在下一節看到有關這點的範例。

執行應用程式，並使用滑桿變更影像範本的尺寸。 如您所見，不需要多少程式碼，就能產生相當強而有力的效果。

![執行的應用程式顯示縮放滑桿](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> 想要一點挑戰的話，請嘗試將其他 UI 屬性繫結至縮放滑桿 `Value` 屬性，或繫結至在縮放滑桿之後加入的其他滑桿。 例如，您可以將 `FontSize` 的 `TitleTextBlock` 屬性繫結至預設值為 **24** 的新滑桿。 請務必設定合理的最小值和最大值。

## <a name="part-4-improve-the-zoom-experience"></a>第 4 部分：改善縮放體驗

此部分會將自訂 `ItemSize` 屬性加入程式碼後置，並建立從影像範本至新屬性的單向繫結。 ItemSize`ItemSize` 值將會因為縮放滑桿和其他因素 (例如 [符合螢幕大小]  切換鈕、視窗大小等) 而更新，使得體驗更加精緻。

與內建控制項屬性不同，您的自訂屬性不會自動更新 UI，即使有單向和雙向繫結也不會。 這些屬性在「一次性」  繫結時可以正常運作，但如果您想要屬性變更實際顯示在 UI 上，就需要做一些工作。

### <a name="create-the-itemsize-property-so-that-it-updates-the-ui"></a>建立 ItemSize 屬性，讓它來更新 UI

1. 在 MainPage.xaml.cs 中，變更 `MainPage` 類別的特徵標記，以便實作 `INotifyPropertyChanged` 介面。

    **之前：**

    ```csharp
    public sealed partial class MainPage : Page
    ```

    **之後：**

    ```csharp
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    這會通知繫結系統，`MainPage` 具有更新 UI 時繫結需接聽的 `PropertyChanged` 事件 (接下來會新增)。

2. 將 `PropertyChanged` 事件加入 `MainPage` 類別。

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    這個事件提供 `INotifyPropertyChanged` 介面所需的完整實作。 不過，要讓它起作用，就必須在您的自訂屬性中明確引發該事件。

3. 加入 `ItemSize` 屬性，並於其 setter 中引發 `PropertyChanged` 事件。

    ```csharp
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    `ItemSize` 屬性會公開私有 `_itemSize` 欄位的值。 使用像這樣的支援欄位可讓屬性檢查新的值是否和其引發可能不必要 `PropertyChanged` 事件之前的舊值相同。

    事件本身是由 `Invoke` 方法引發。 問號標記會檢查 `PropertyChanged` 事件是否為 null，也就是說，是否新增了任何事件處理常式。 每個單向或雙向繫結都會在幕後新增事件處理常式，但要是沒有任何處理常式在接聽，這裡就不會再有任何動作發生。 不過，如果 `PropertyChanged` 不為 null，那麼就會使用指向事件來源的參考 (頁面本身，以 `this` 關鍵字表示) 以及表示屬性名稱的 **event-args** 物件來呼叫 `Invoke`。 有了這項資訊後，任何對 `ItemSize` 屬性的單向或雙向繫結都收到所有變更的通知，好讓這些繫結可以更新繫結的 UI。

4. 在 MainPage.xaml 中，尋找名為 `DataTemplate` 的 `ImageGridView_DefaultItemTemplate`，並取代範本最前面 `Height` 控制項的 `Width` 和 `Grid` 值。 (如果您已在本教學課程之前的部分進行控制項至控制項的繫結，唯一要改變的只是將 `Value` 換成 `ItemSize`，以及將 `ZoomSlider` 換成 `page`。 請務必為 `Height` 和 `Width` 執行這項操作！)

    **之前**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```

    **之後**

    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate"
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

現在 UI 可以回應 `ItemSize` 變更了，您需要實際進行一些變更。 如前述，`ItemSize` 值是從各種不同 UI 控制項的目前狀態計算求出，但每當這些控制項改變狀態時都必須執行計算。 為了這樣做，您將使用事件繫結來讓特定 UI 變更呼叫能更新 `ItemSize` 的 Helper 方法。

### <a name="update-the-itemsize-property-value"></a>更新 ItemSize 屬性值

1. 將 `DetermineItemSize` 方法加入 MainPage.xaml.cs。

    ```csharp
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change,
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DefaultWindowSidePaddingValue"];
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);

            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);

            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. 在 MainPage.xaml 中，瀏覽至檔案最前面，並將 `SizeChanged` 事件繫結加入 `Page` 元素。

    **之前：**

    ```xaml
    <Page x:Name="page"
    ```

    **之後：**

    ```xaml
    <Page x:Name="page"
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. 尋找名為 `ZoomSlider` 的 `Slider` (在 [`Page.Resources`] 區段中)，然後新增 `ValueChanged` 事件繫結。

    **之前：**

    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **之後：**

    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. 尋找名為 `ToggleSwitch` 的 `FitScreenToggle`，並加入 `Toggled` 事件繫結。

    **之前：**

    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **之後：**

    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

執行應用程式，並使用縮放滑桿和 [符合螢幕大小]  切換鈕來變更影像範本的尺寸。 如您所見，最新的變更帶來更精緻的縮放/調整大小體驗，同時也能保持程式碼井然有序。

![執行的應用程式啟用符合螢幕大小](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> 想要一點挑戰的話，請嘗試在 `TextBlock` 之後加入 `ZoomSlider`，並將 `Text` 屬性繫結至 `ItemSize` 屬性。 由於不在資料範本之中，您可以使用 `x:Bind` 替代 `Binding`，就像在先前 `ItemSize` 繫結中一樣。

## <a name="part-5-enable-user-edits"></a>第 5 部分：允許使用者編輯

這裡會建立雙向繫結，讓使用者可以更新各值，包括影像標題、評分以及各種視覺效果。

為了達成此目的，就要更新現有的 `DetailPage`，這個頁面提供單一影像檢視器、縮放控制項和編輯 UI。

不過，首先必須附加 `DetailPage`，好讓應用程式在使用者按一下圖庫檢視中的影像時瀏覽至該頁面。

### <a name="attach-the-detailpage"></a>附加 DetailPage

1. 在 MainPage.xaml 中，尋找名為 `ImageGridView` 的 `GridView`。 若要讓人可點選該項目，將 `IsItemClickEnabled` 設定為 `True`，並加入 `ItemClick` 事件處理常式。

    > [!TIP]
    > 如果您鍵入下方變更，而不是以複製/貼上方式進行，您將會看到 IntelliSense 快顯視窗顯示 "\<New Event Handler\>"。 如果您按下 Tab 鍵，其中會填入預設方法處理常式名稱的值，並自動移除下一個步驟中顯示的方法。 接著可以按 F12 瀏覽至程式碼後置中的方法。

    **之前：**

    ```xaml
    <GridView x:Name="ImageGridView">
    ```

    **之後：**

    ```xaml
    <GridView x:Name="ImageGridView"
              IsItemClickEnabled="True"
              ItemClick="ImageGridView_ItemClick">
    ```

    > [!NOTE]
    > 這裡會使用傳統事件處理常式，而不使用 x:Bind 運算式。 這是因為我們需要查看事件資料，如下所示。

2. 在 MainPage.xaml.cs 中，新增事件處理常式 (如果您使用上一個步驟中的提示，則填入處理常式)。

    ```csharp
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    這個方法只是單純瀏覽至詳細資料頁面，傳入所按下的項目，也就是 `ImageFileInfo` 用來初始化頁面的 **ImageFileInfo** 物件。 您不一定要實作本教學課程中的這個方法，但您可以看看它的效果。

3. (選擇性) 刪除或以註解取消任何您在先前習作時所加入用來處理所選取影像的控制項。 留下這些控制項不會有任何妨礙，但現在要是不瀏覽至詳細資料頁面，就變得更加難以選取影像。

既然您已經連接兩個頁面，就執行應用程式，看一看效果。 一切都正常運作，除了編輯窗格上的控制項以外，當您嘗試變更值時，這個控制項不會回應。

如您所見，標題文字方塊確實顯示了標題，並且允許您輸入變更。 您必須將焦點變更到其他控制項，才能認可變更，但螢幕左上角的標題尚未更新。

所有控制項皆已使用我們在第 1 部分討論的一般 `x:Bind` 運算式繫結在一起。 如果您還記得，這表示那些運算式都是一次性繫結，正好說明為什麼變更的值無法登錄。 若要修正這個問題，我們所需做的就是將運算式變成雙向繫結。

### <a name="make-the-editing-controls-interactive"></a>使編輯控制項有互動功能

1. 在 DetailPage.xaml 中，尋找名為 **TitleTextBlock** 的 `TextBlock` 以及其後的 **RatingControl** 控制項，並將這些控制項的 `x:Bind` 運算式更新為包含 **Mode=TwoWay**。

    **之前：**

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <muxc:RatingControl Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **之後：**

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}"
               ... >
    <muxc:RatingControl Value="{x:Bind item.ImageRating, Mode=TwoWay}"
                            ... >
    ```

2. 對所有排在評分控制項之後的效果滑桿執行相同的動作。

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ...
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ...
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ...
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ...
    ```

如您所預期，雙向模式意味著只要任一邊發生變更，資料就會在兩個方向上移動。

就像前面說明的單向繫結一樣，每當繫結的屬性變更時，這些雙向繫結現在會因為 `ImageFileInfo` 類別中的 `INotifyPropertyChanged` 實作的作用而更新 UI。 不過，在雙向繫結的情況下，每當使用者與控制項互動時，這些值也都會從 UI 送到繫結的屬性。 在 XAML 這邊已不需要再執行任何動作。

執行應用程式，然後嘗試編輯控制項。 如您所見，現在進行變更時會影響影像值，而這些變更在您瀏覽回到主頁面時仍然持續。

## <a name="part-6-format-values-through-function-binding"></a>第 6 部分：透過函式繫結將值格式化

還剩最後一個問題。 當您移動效果滑桿時，旁邊的標籤卻沒變更。

![使用預設標籤值的效果滑桿](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

本教學課程的最後部分是加入可格式化滑桿顯示值的繫結。

### <a name="bind-the-effect-slider-labels-and-format-the-values-for-display"></a>繫結效果滑桿標籤並格式化顯示值

1. 尋找 `Exposure` 滑杆後面的 `TextBlock`，並將 `Text` 值取代為此處顯示的繫結運算式。

    **之前：**

    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **之後：**

    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    因為您繫結的是方法的傳回值，就稱之為函式繫結。 如果您使用資料範本，這個方法必須可以透過頁面的程式碼後置或 `x:DataType` 類型來存取。 在此範例中用的方法是熟悉的 .NET `ToString` 方法，我們透過頁面的項目屬性，然後再透過項目的 `Exposure` 屬性存取這個方法。 (這示範如何繫結至連接鏈結中的深層巢狀方法及屬性。)

    函式繫結是將顯示值格式化的理想方式，因為您可以傳入其他繫結來源做為方法引數，而且正如對單向模式的預期，繫結運算式也會接聽這些值的變更。 在本範例中，**culture** 引數是參考，指向程式碼後置中實作的不變欄位，但很容易就可以變成引發 `PropertyChanged` 事件的屬性。 在這種情況下，屬性值的任何變更都會導致 `x:Bind` 運算式以新的值呼叫 `ToString`，然後再以結果更新 UI。

2. 對其他有標籤的效果滑桿的 `TextBlock` 執行相同的動作。

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

現在您執行應用程式時，一切都運作正常，包括滑桿標籤也不例外。

![標籤正常運作的效果滑桿](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

## <a name="conclusion"></a>結論

本教學課程讓您了解了資料繫結，並且示範一些可用的功能。 但在總結之前，要提醒您注意：並非所有的東西都可繫結，有時候您嘗試連接的值會與您嘗試繫結的屬性不相容。 繫結有很大的彈性，但不是所有的情況都適用。

例如，就和詳細資料頁面縮放功能的情況一樣，當控制項沒有可繫結的合適屬性時，便是繫結無法解決的問題。 這個縮放滑桿必須與顯示影像的 `ScrollViewer` 互動，但是 `ScrollViewer` 只能透過其 `ChangeView` 方法來更新。 在此案例中，我們會使用傳統事件處理常式，讓 `ScrollViewer` 和縮放滑杆保持同步；如需詳細資訊，請參閱 `DetailPage` 中的 `ZoomSlider_ValueChanged` 和 `MainImageScroll_ViewChanged` 方法。

儘管如此，繫結仍是有效且具彈性的方式，可以簡化程式碼，並將 UI 邏輯與資料邏輯區隔開來。 這使得您更容易調整區隔兩邊的工作，同時還能降低將錯誤帶進另一邊的風險。

例如 `ImageFileInfo.ImageTitle` 屬性，便是 UI 與資料區隔的一個範例。 這個屬性 (和 `ImageRating` 屬性) 與您在第 4 部分建立的 `ItemSize` 屬性略有不同，因為值是存放在檔案中繼資料 (透過 `ImageProperties` 類型公開)，而不是存放在欄位中。 此外，如果檔案中繼資料中沒有標題，`ImageTitle` 還會傳回 `ImageName` 值 (設定為檔案名稱)。

```csharp
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

如您所見，setter 會更新 `ImageProperties.Title` 屬性，然後呼叫 `SavePropertiesAsync` 來將新值寫入檔案 。 (這是非同步方法，但我們無法在屬性中使用 `await` 關鍵字，而您也不會想要這樣做，因為屬性的 getter 和 setter 應該會立即完成。 因此，您改為呼叫此方法，並忽略其傳回的 `Task` 物件。)

## <a name="going-further"></a>更進一步

現在完成了這個實習課程，您已具備充足的繫結知識，可以自行解決問題。

您可能已經注意到，如果變更詳細資料頁面上的縮放比例，該頁面就會在您往回瀏覽然後再選取相同影像時自動重設。 您能想出如何個別保留和還原每個影像的縮放比例嗎？ 祝您好運！

您應該已經在本教學課程中獲得您需要的所有資訊，如果還需要更多的指導方針，只要按一下滑鼠即可取得資料繫結文件。 從這裡開始：

+ [{x:Bind} 標記延伸](../xaml-platform/x-bind-markup-extension.md)
+ [深入了解資料繫結](./data-binding-in-depth.md)
