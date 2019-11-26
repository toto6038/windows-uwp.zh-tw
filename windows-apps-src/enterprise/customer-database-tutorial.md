---
title: 建立客戶資料庫應用程式
description: 建立客戶資料庫應用程式，並瞭解如何執行基本的企業應用程式功能。
keywords: 企業，教學課程，客戶，資料，建立讀取更新刪除，REST，驗證
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b8cf0554464b56337e3d57b58db543092682ffa3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258621"
---
# <a name="tutorial-create-a-customer-database-application"></a>教學課程︰建立客戶資料庫應用程式

本教學課程會建立簡單的應用程式來管理客戶清單。 在這種情況下，它為 UWP 中的企業應用程式引進了基本概念的選擇。 您將會了解如何：

* 針對本機 SQL 資料庫執行建立、讀取、更新和刪除作業。
* 加入資料格，以在 UI 中顯示和編輯客戶資料。
* 在基本表單版面配置中將 UI 元素排列在一起。

本教學課程的起點是一頁應用程式，具有最少的 UI 和功能，以簡化版的[客戶訂單資料庫範例應用程式](https://github.com/Microsoft/Windows-appsample-customers-orders-database)為基礎。 它是以C#和 XAML 撰寫的，我們希望您對這兩種語言都有基本的熟悉度。

![工作應用程式的主頁面](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>必要條件

* [確保您擁有最新版本的 Visual Studio 和 Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [複製或下載客戶資料庫教學課程範例](https://github.com/microsoft/windows-tutorials-customer-database)

複製/下載存放庫之後，您可以使用 Visual Studio 開啟 [ **CustomerDatabaseTutorial** ] 來編輯專案。

> [!NOTE]
> 查看[完整客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，以查看此教學課程的基礎應用程式。

## <a name="part-1-code-of-interest"></a>第1部分：相關程式碼

如果您在開啟應用程式之後立即執行，您會在空白畫面頂端看到幾個按鈕。 雖然您看不到這種情況，但應用程式已經包含了一些測試客戶所布建的本機 SQLite 資料庫。 從這裡開始，您將會先執行 UI 控制項來顯示這些客戶，然後再繼續針對資料庫加入作業。 在開始之前，您可以在這裡著手進行。

### <a name="views"></a>檢視

**CustomerListPage**是應用程式的 View，它會在本教學課程中定義單一頁面的 UI。 每當您需要在 UI 中新增或變更視覺專案時，就會在此檔案中執行此動作。 本教學課程將逐步引導您新增這些元素：

* 用來顯示和編輯客戶的**RadDataGrid** 。 
* 用來設定新客戶之初始值的**StackPanel** 。

### <a name="viewmodels"></a>Viewmodel

**ViewModels\CustomerListPageViewModel.cs**是應用程式的基本邏輯所在的位置。 在此視圖中所採取的每個使用者動作都會傳遞至此檔案進行處理。 在本教學課程中，您將新增一些新的程式碼，並執行下列方法：

* **CreateNewCustomerAsync**，它會初始化新的 CustomerViewModel 物件。
* **DeleteNewCustomerAsync**，這會在使用者于 UI 中顯示之前移除新的客戶。
* **DeleteAndUpdateAsync**，它會處理 [刪除] 按鈕的邏輯。
* **GetCustomerListAsync**，它會從資料庫中抓取客戶清單。
* **SaveInitialChangesAsync**，這會將新客戶的資訊加入至資料庫。
* **UpdateCustomersAsync**，這會重新整理 UI 以反映任何新增或刪除的客戶。

**CustomerViewModel**是客戶資訊的包裝函式，它會追蹤是否最近修改過。 您不需要將任何專案新增至此類別，但您在其他地方新增的一些程式碼將會參考它。

如需如何建立範例的詳細資訊，請參閱[應用程式結構總覽](../enterprise/customer-database-app-structure.md)。

## <a name="part-2-add-the-datagrid"></a>第2部分：新增 DataGrid

開始操作客戶資料之前，您必須新增 UI 控制項以顯示這些客戶。 為了這麼做，我們將使用預先建立的協力廠商**RadDataGrid**控制項。 此專案中已經包含 Microsoft.netcore.universalwindowsplatform NuGet 套件的**Telerik** 。 讓我們在專案中新增方格。

1. 從 [方案總管] 開啟 [ **Views\CustomerListPage.xaml** ]。 在**Page**標記內新增下列程式程式碼，以宣告包含資料方格之 Telerik 命名空間的對應。

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. 在視圖主要**RelativePanel**的 [ **CommandBar** ] 底下，新增**RadDataGrid**控制項，其中包含一些基本設定選項：

    ```xaml
    <Grid
        x:Name="CustomerListRoot"
        Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <RelativePanel>
            <CommandBar
                x:Name="mainCommandBar"
                HorizontalAlignment="Stretch"
                Background="AliceBlue">
                <!--CommandBar content-->
            </CommandBar>
            <telerikGrid:RadDataGrid
                x:Name="DataGrid"
                BorderThickness="0"
                ColumnDataOperationsMode="Flyout"
                GridLinesVisibility="None"
                GroupPanelPosition="Left"
                RelativePanel.AlignLeftWithPanel="True"
                RelativePanel.AlignRightWithPanel="True"
                RelativePanel.Below="mainCommandBar" />
        </RelativePanel>
    </Grid>
    ```

3. 您已加入資料格，但它需要資料才能顯示。 將下列幾行程式碼加入其中：

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    現在您已定義要顯示的資料來源， **RadDataGrid**將會為您處理大部分的 UI 邏輯。 不過，如果您執行專案，您仍然看不到顯示的任何資料。 這是因為 ViewModel 還不會載入它。

![空白應用程式，沒有客戶](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>第3部分：讀取客戶

當它初始化時， **ViewModels\CustomerListPageViewModel.cs**會呼叫**GetCustomerListAsync**方法。 該方法必須從教學課程中所包含的 SQLite 資料庫，抓取測試客戶資料。

1. 在**ViewModels\CustomerListPageViewModel.cs**中，使用下列程式碼更新您的**GetCustomerListAsync**方法：

    ```csharp
    public async Task GetCustomerListAsync()
    {
        var customers = await App.Repository.Customers.GetAsync(); 
        if (customers == null)
        {
            return;
        }
        await DispatcherHelper.ExecuteOnUIThreadAsync(() =>
        {
            Customers.Clear();
            foreach (var c in customers)
            {
                Customers.Add(new CustomerViewModel(c));
            }
        });
    }
    ```
    載入 ViewModel 時，會呼叫**GetCustomerListAsync**方法，但在此步驟之前，它不會執行任何動作。 在這裡，我們已在**Repository/SqlCustomerRepository**中新增對**GetAsync**方法的呼叫。 這可讓它連線到存放庫，以取得 Customer 物件的可列舉集合。 接著，它會將它們剖析成個別的物件，然後再將它們新增至其內部**ObservableCollection** ，以顯示和編輯它們。

2. 執行您的應用程式-您現在會看到顯示客戶清單的資料格。

![客戶的初始清單](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>第4部分：編輯客戶

您可以藉由按兩下來編輯資料方格中的專案，但您必須確定您在 UI 中所做的任何變更也會在程式碼後置中，對您的客戶集合進行。 這表示您必須執行雙向資料系結。 如果您想要更多有關這方面的資訊，請參閱我們[的資料](../get-started/display-customers-in-list-learning-track.md)系結簡介。

1. 首先，宣告**ViewModels\CustomerListPageViewModel.cs**會實作為**INotifyPropertyChanged**介面：

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 然後，在類別的主要主體中，新增下列事件和方法：

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    **OnPropertyChanged**方法可讓您的 setter 輕鬆地引發**PropertyChanged**事件，這是雙向資料系結的必要專案。

3. 使用此函數呼叫來更新**SelectedCustomer**的 setter：

    ```csharp
    public CustomerViewModel SelectedCustomer
    {
        get => _selectedCustomer;
        set
        {
            if (_selectedCustomer != value)
            {
                _selectedCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

4. 在**Views\CustomerListPage.xaml**中，將**SelectedCustomer**屬性新增至您的資料格。

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    這會使使用者在資料方格中的選取專案與程式碼後置中的對應 Customer 物件產生關聯。 TwoWay 系結模式可讓 UI 中所做的變更反映在該物件上。

5. 執行您的應用程式。 您現在可以看到客戶顯示在方格中，並透過您的 UI 對基礎資料進行變更。

![編輯資料方格中的客戶](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>第5部分：更新客戶

現在您可以看到並編輯您的客戶，您必須能夠將您的變更推送到資料庫，並提取其他人所做的更新。

1. 返回**ViewModels\CustomerListPageViewModel.cs**，並流覽至**UpdateCustomersAsync**方法。 使用下列程式碼更新它，以將變更推送到資料庫並抓取任何新的資訊：

    ```csharp
    public async Task UpdateCustomersAsync()
    {
        foreach (var modifiedCustomer in Customers
            .Where(x => x.IsModified).Select(x => x.Model))
        {
            await App.Repository.Customers.UpsertAsync(modifiedCustomer);
        }
        await GetCustomerListAsync();
    }
    ```
    此程式碼會利用**ViewModels\CustomerViewModel.cs**的**IsModified**屬性，這會在客戶每次變更時自動更新。 這可讓您避免不必要的呼叫，並只將變更從更新的客戶推送到資料庫。

## <a name="part-6-create-a-new-customer"></a>第6部分：建立新的客戶

加入新的客戶會帶來挑戰，因為如果您在 UI 中新增其屬性的值，客戶就會顯示為空白資料列。 這不是問題，但我們將更輕鬆地設定客戶的初始值。 在本教學課程中，我們將新增一個簡單的可折迭面板，但如果您有更多要新增的資訊，可以針對此用途建立個別的頁面。

### <a name="update-the-code-behind"></a>更新程式碼後置

1. 將新的私用欄位和公用屬性加入至**ViewModels\CustomerListPageViewModel.cs**。 這會用來控制是否顯示面板。

    ```csharp
    private bool _addingNewCustomer = false;

    public bool AddingNewCustomer
    {
        get => _addingNewCustomer;
        set
        {
            if (_addingNewCustomer != value)
            {
                _addingNewCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2. 將新的公用屬性加入至 ViewModel，這是**AddingNewCustomer**值的反函數。 當面板為可見時，這將用來停用一般的命令列按鈕。

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    現在您將需要一種方法來顯示可折迭的面板，以及建立要在其中編輯的客戶。 

3. 將新的私用 fiend 和公用屬性新增至 ViewModel，以保存新建立的客戶。

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if (_newCustomer != value)
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  更新您的**CreateNewCustomerAsync**方法以建立新的客戶、將其新增至存放庫，並將它設定為選取的客戶：

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. 更新**SaveInitialChangesAsync**方法，將新建立的客戶新增至存放庫、更新 UI，然後關閉面板。

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. 在**AddingNewCustomer**的 setter 中，新增下列程式程式碼作為最後一行：

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    這可確保每當**AddingNewCustomer**變更時， **EnableCommandBar**就會自動更新。

### <a name="update-the-ui"></a>更新 UI

1. 流覽回到**Views\CustomerListPage.xaml**，並在您的**CommandBar**和您的資料格之間新增具有下列屬性的**StackPanel** ：

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    **X:Load**屬性可確保只有在您新增新客戶時，才會出現此面板。

2. 對資料格的位置進行下列變更，以確保它會在新面板出現時向下移動：

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 以四個**TextBox**控制項更新您的堆疊面板。 它們會系結至新客戶的個別屬性，並可讓您編輯其值，然後再將它加入資料方格。

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
        <TextBox
            Header="First name"
            PlaceholderText="First"
            Margin="8,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.FirstName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Last name"
            PlaceholderText="Last"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.LastName, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Address"
            PlaceholderText="1234 Address St, Redmond WA 00000"
            Margin="0,8,16,8"
            MinWidth="280"
            Text="{x:Bind ViewModel.NewCustomer.Address, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
        <TextBox
            Header="Company"
            PlaceholderText="Company"
            Margin="0,8,16,8"
            MinWidth="120"
            Text="{x:Bind ViewModel.NewCustomer.Company, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"/>
    </StackPanel>
    ```

4. 將簡單的按鈕新增至新的堆疊面板，以儲存新建立的客戶：

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. 更新**CommandBar**，讓 [堆疊面板] 可見時，會停用一般 [建立]、[刪除] 和 [更新] 按鈕：

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. 執行您的應用程式。 您現在可以建立客戶，並在 [堆疊] 面板中輸入其資料。

![建立新客戶](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>第7部分：刪除客戶

刪除客戶是您需要執行的最後基本作業。 當您刪除已在資料方格中選取的客戶時，您會想要立即呼叫**UpdateCustomersAsync**來更新 UI。 不過，如果您要刪除剛建立的客戶，則不需要呼叫該方法。

1. 流覽至**ViewModels\CustomerListPageViewModel.cs**，並更新**DeleteAndUpdateAsync**方法：

    ```csharp
    public async void DeleteAndUpdateAsync()
    {
        if (SelectedCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_selectedCustomer.Model.Id);
        }
        await UpdateCustomersAsync();
    }
    ```

2. 在**Views\CustomerListPage.xaml**中，更新 [堆疊] 面板以加入新的客戶，使其包含第二個按鈕：

    ```xaml
    <StackPanel>
        <!--Text boxes for adding a new customer-->
        <AppBarButton
            x:Name="DeleteNewCustomer"
            Click="{x:Bind ViewModel.DeleteNewCustomerAsync}"
            Icon="Cancel"/>
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

3. 在**ViewModels\CustomerListPageViewModel.cs**中，更新**DeleteNewCustomerAsync**方法以刪除新的客戶：

    ```csharp
    public async Task DeleteNewCustomerAsync()
    {
        if (NewCustomer != null)
        {
            await App.Repository.Customers.DeleteAsync(_newCustomer.Model.Id);
            AddingNewCustomer = false;
        }
    }
    ```

4. 執行您的應用程式。 您現在可以在資料方格或 [堆疊] 面板中刪除客戶。

![刪除新的客戶](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>結論

恭喜！ 完成上述所有作業之後，您的應用程式現在有完整範圍的本機資料庫作業。 您可以在 UI 中建立、讀取、更新和刪除客戶，這些變更會儲存到您的資料庫，並且會在不同的應用程式啟動期間保存。

現在您已經完成，請考慮下列事項：
* 如果您還沒有這麼做，請參閱[應用程式結構總覽](../enterprise/customer-database-app-structure.md)，以取得有關應用程式建立方式的詳細資訊。
* 探索[完整客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，以查看本教學課程所依據的應用程式。

或者，如果您遇到挑戰，可以繼續 。

## <a name="going-further-connect-to-a-remote-database"></a>進一步瞭解：連接到遠端資料庫

我們已提供逐步解說，說明如何針對本機 SQLite 資料庫來執行這些呼叫。 但是，如果您想要使用遠端資料庫，又該怎麼做呢？

如果您想要嘗試這麼做，您將需要自己的 Azure Active Directory （AAD）帳戶，以及裝載您自己的資料來源的能力。

您必須加入驗證、處理 REST 呼叫的函式，然後建立要與互動的遠端資料庫。 完整[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)中有程式碼，您可以參考每個必要的作業。

### <a name="settings-and-configuration"></a>設定和設定

在[範例的讀我檔案](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)中，連接到您自己的遠端資料庫的必要步驟已拼出。 您必須執行下列動作：

* 將您的 Azure 帳戶用戶端識別碼提供給[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 提供要[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)之遠端資料庫的 url。
* 提供要[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)之資料庫的連接字串。
* 將您的應用程式與 Microsoft Store 建立關聯。
* 將[服務專案](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService)複製到您的應用程式，並將其部署至 Azure。

### <a name="authentication"></a>Authentication

您必須建立按鈕來啟動驗證序列，以及使用快顯或個別頁面來收集使用者的資訊。 建立該檔案之後，您必須提供要求使用者資訊的程式碼，並使用它來取得存取權杖。 客戶訂單資料庫範例會使用**WebAccountManager**程式庫包裝 Microsoft Graph 呼叫，以取得權杖並處理 AAD 帳戶的驗證。

* 驗證邏輯會在[**AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)中執行。
* 驗證程式會顯示在自訂的[**AuthenticationControl**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml)控制項中。

### <a name="rest-calls"></a>REST 呼叫

您不需要修改我們在本教學課程中加入的任何程式碼，即可執行 REST 呼叫。 相反地，您必須執行下列動作：

* 建立**ICustomerRepository**和**ITutorialRepository**介面的新實作為，透過 REST （而不是 SQLite）來執行一組相同的函式。 您將需要序列化和還原序列化 JSON，並且可以在需要時，將 REST 呼叫包裝在不同的**HttpHelper**類別中。 如需詳細資訊，請參閱[完整範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest)。
* 在**App.xaml.cs**中，建立新的函式來初始化 REST 存放庫，並在應用程式初始化時呼叫它，而不是**SqliteDatabase** 。 同樣地，請參閱[完整的範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)。

完成上述三個步驟之後，您應該能夠透過應用程式向 AAD 帳戶進行驗證。 對遠端資料庫的 REST 呼叫將會取代本機的 SQLite 呼叫，但使用者體驗應該相同。 如果您覺得更確保建構完善，可以加入 [設定] 頁面，讓使用者在兩者之間進行動態切換。
