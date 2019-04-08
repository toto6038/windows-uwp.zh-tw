---
title: 建立客戶資料庫應用程式
description: 建立客戶資料庫的應用程式，並了解如何實作基本的企業應用程式函式。
keywords: enterprise、 教學課程中，客戶資料，建立讀取、 更新 [刪除]，REST 驗證
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: 9c09e0fb73e42fd8a3d0c70bbb5396be32624387
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623243"
---
# <a name="tutorial-create-a-customer-database-application"></a>教學課程：建立客戶資料庫應用程式

本教學課程會建立簡單的應用程式，來管理客戶的清單。 在此情況下，它引入了對於企業 UWP 應用程式的基本概念的選取範圍。 您將會了解如何：

* 實作建立、 讀取、 更新和刪除作業對本機的 SQL database。
* 加入資料格，即可顯示和編輯您的 UI 中的客戶資料。
* 排列在一起的基本表單版面配置中的 UI 項目。

本教學課程的起點是使用最少 UI 和功能，以簡化的版本為基礎的單一頁面應用程式[客戶訂單資料庫範例應用程式](https://github.com/Microsoft/Windows-appsample-customers-orders-database)。 它以C#和 XAML，和我們預期，您有基本的熟悉這些這兩種語言。

![工作應用程式的主頁面](images/customer-database-tutorial/customer-list.png)

### <a name="prerequisites"></a>必要條件

* [請確定您有最新版的 Visual Studio 和 Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* [複製或下載客戶資料庫教學課程範例](https://aka.ms/customer-database-tutorial)

您已複製/下載之後存放庫，您可以編輯專案開啟**CustomerDatabaseTutorial.sln**使用 Visual Studio。

> [!NOTE]
> 請參閱[完整的客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)以查看本教學課程根據應用程式。

## <a name="part-1-code-of-interest"></a>第 1 部分：感興趣的程式碼

如果您開啟後立即執行您的應用程式，您會看到幾個空白畫面頂端的按鈕。 雖然不是您可以看到，應用程式已經包含幾個測試的客戶佈建的本機 SQLite 資料庫。 從這裡開始，您會藉由實作 UI 控制項來顯示這些客戶，啟動，並移至在對資料庫執行的作業中新增。 在開始之前，以下是在您將會處理。

### <a name="views"></a>檢視

**CustomerListPage.xaml**是定義在本教學課程中的單一頁面的 UI 的應用程式的檢視。 每當您需要新增或變更的視覺元素，在 UI 中，您將會進行此檔案中。 本教學課程將引導您完成加入這些項目：

* A **RadDataGrid**來顯示和編輯您的客戶。 
* A **StackPanel**設為新客戶的初始值。

### <a name="viewmodels"></a>Viewmodel

**ViewModels\CustomerListPageViewModel.cs**是應用程式的基本邏輯所在的位置。 在檢視中採取的每個使用者動作將傳入此檔案進行處理。 在本教學課程中，您將新增一些新的程式碼，並實作下列方法：

* **CreateNewCustomerAsync**，其中，初始化新的 CustomerViewModel 物件。
* **DeleteNewCustomerAsync**，以移除新的客戶之前它會顯示在 UI 中。
* **DeleteAndUpdateAsync**，處理 [刪除] 按鈕的邏輯。
* **GetCustomerListAsync**，表示從資料庫擷取客戶清單。
* **SaveInitialChangesAsync**，其在資料庫中加入新的客戶資訊。
* **UpdateCustomersAsync**，會重新整理以反映加入或刪除任何客戶的 UI。

**CustomerViewModel**是客戶的詳細資訊，可追蹤是否最近修改的包裝函式。 您不需要將任何項目新增至此類別，但某些其他位置，您將新增的程式碼會參考它。

如需有關範例的建構方式的詳細資訊，請參閱[應用程式結構概觀](../enterprise/customer-database-app-structure.md)。

## <a name="part-2-add-the-datagrid"></a>第 2 部分：新增資料格

開始處理客戶資料之前，您必須加入 UI 控制項來顯示這些客戶。 若要這樣做，我們將使用預先製作的第三方**RadDataGrid**控制項。 **Telerik.UI.for.UniversalWindowsPlatform** NuGet 套件已包含在此專案。 讓我們加入我們的專案中的方格。

1. 開啟**Views\CustomerListPage.xaml**從 [方案總管]。 新增下列程式碼內**網頁**宣告對應至包含資料格的 Telerik 命名空間的標記。

    ```xaml
        xmlns:telerikGrid="using:Telerik.UI.Xaml.Controls.Grid"
    ```

2. 下面**CommandBar**內之主**RelativePanel**檢視中，加入**RadDataGrid**控制，使用一些基本設定選項：

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

3. 您已新增資料方格中，但您還需要可顯示的資料。 將下列程式碼行新增至它：

    ```xaml
    ItemsSource="{x:Bind ViewModel.Customers}"
    UserEditMode="Inline"
    ```
    既然您已定義資料來源，以顯示，請**RadDataGrid**會為您處理大部分的 UI 邏輯。 不過，如果您執行您的專案時，您仍然不會看到任何資料上顯示。 這是因為 ViewModel 它不尚未載入。

![空白的應用程式，客戶不](images/customer-database-tutorial/blank-customer-list.png)

## <a name="part-3-read-customers"></a>第 3 部分：閱讀客戶

初始化時， **ViewModels\CustomerListPageViewModel.cs**呼叫**GetCustomerListAsync**方法。 方法需要測試的客戶資料擷取的 SQLite 資料庫，會包含在本教學課程。

1. 在  **ViewModels\CustomerListPageViewModel.cs**，更新您**GetCustomerListAsync**方法取代此程式碼：

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
    **GetCustomerListAsync** ViewModel 載入，但在此步驟中之前, 它並沒有進行任何項目時，會呼叫方法。 在這裡，我們已將呼叫新增至**GetAsync**方法中的**存放庫/SqlCustomerRepository**。 這可讓它連絡的儲存機制來擷取客戶物件的可列舉集合。 它接著將這些訊息剖析成個別的物件，再將它們新增到其內部**ObservableCollection**讓它們可以顯示和編輯。

2. 執行您的應用程式-您現在會看到顯示客戶清單的資料格。

![初始清單中的客戶](images/customer-database-tutorial/initial-customers.png)

## <a name="part-4-edit-customers"></a>第 4 部分：編輯客戶

您可以按兩下檔案，編輯資料方格中的項目，但您必須確定您在 UI 中進行任何變更也會對您的客戶程式碼後置的集合。 這表示您必須實作雙向資料繫結。 如果您想要這的詳細資訊，請參閱我們[簡介資料繫結](../get-started/display-customers-in-list-learning-track.md)。

1. 首先，宣告**ViewModels\CustomerListPageViewModel.cs**實作**INotifyPropertyChanged**介面：

    ```csharp
    public class CustomerListPageViewModel : INotifyPropertyChanged
    ```

2. 然後，在主體中的類別，新增下列事件和方法：

    ```csharp
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    ```

    **OnPropertyChanged**方法，可以方便您 setter 引發**PropertyChanged**事件，這是所需的雙向資料繫結。

3. 更新的 setter **SelectedCustomer**使用此函式呼叫：

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

4. 在  **Views\CustomerListPage.xaml**，新增**SelectedCustomer**加入您的資料格的屬性。

    ```xaml
    SelectedItem="{x:Bind ViewModel.SelectedCustomer, Mode=TwoWay}"
    ```

    這會將使用者的選取項目在資料格中關聯對應客戶物件中的程式碼後置。 TwoWay 繫結模式可讓該物件會反映在 UI 中所做的變更。

5. 執行應用程式。 您現在可以看到顯示在方格中，客戶，並對基礎資料中的變更，透過您的 UI。

![編輯資料方格中的客戶](images/customer-database-tutorial/edit-customer-inline.png)

## <a name="part-5-update-customers"></a>第 5 部分：更新客戶

現在，您可以查看和編輯您的客戶，您必須能夠將變更推送至資料庫，並提取其他人所做的任何更新。

1. 返回**ViewModels\CustomerListPageViewModel.cs**，然後瀏覽至**UpdateCustomersAsync**方法。 這段程式碼，將變更推送至資料庫，並擷取任何新資訊來更新它：

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
    此程式碼會利用**IsModified**屬性**ViewModels\CustomerViewModel.cs**，這會自動更新客戶變更時。 這可讓您以避免不必要的呼叫，並將只更新客戶的變更推送至資料庫。

## <a name="part-6-create-a-new-customer"></a>第 6 節：建立新的客戶

加入新的客戶提供一項挑戰，，因為如果您將它新增至 UI 之前提供其屬性的值，將會顯示為空白資料列的客戶。 這不是問題，但此處我們會包辦您更輕鬆地設定客戶的初始值。 在本教學課程中，我們將新增簡單的可摺疊面板中，但如果您有更多的資訊，以新增您無法針對此用途建立個別的頁面。

### <a name="update-the-code-behind"></a>更新程式碼後置

1. 將新的私用欄位和公用屬性，以新增**ViewModels\CustomerListPageViewModel.cs**。 這會用來控制面板可見。

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

2. 將新的公用屬性新增到 ViewModel，值的反轉**AddingNewCustomer**。 這會用來停用一般的命令列按鈕，面板會顯示時。

    ```csharp
    public bool EnableCommandBar => !AddingNewCustomer;
    ```
    現在，您將需要一種方法以顯示可摺疊面板，以及建立要在其中編輯客戶。 

3. 加入新的私用魔和公用屬性到 ViewModel，以保存新建立的客戶。

    ```csharp
    private CustomerViewModel _newCustomer;

    public CustomerViewModel NewCustomer
    {
        get => _newCustomer;
        set
        {
            if {_newCustomer != value}
            {
                _newCustomer = value;
                OnPropertyChanged();
            }
        }
    }
    ```

2.  更新您**CreateNewCustomerAsync**方法來建立新的客戶，將它新增至存放庫中，並將它設為所選客戶：

    ```csharp
    public async Task CreateNewCustomerAsync()
    {
        CustomerViewModel newCustomer = new CustomerViewModel(new Models.Customer());
        NewCustomer = newCustomer;
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        AddingNewCustomer = true;
    }
    ```

3. 更新**SaveInitialChangesAsync**方法將新建立的客戶新增至存放庫中，更新 UI，並關閉 [面板] 中。

    ```csharp
    public async Task SaveInitialChangesAsync()
    {
        await App.Repository.Customers.UpsertAsync(NewCustomer.Model);
        await UpdateCustomersAsync();
        AddingNewCustomer = false;
    }
    ```
4. 將下列程式碼行新增為 setter 內的最後一行**AddingNewCustomer**:

    ```csharp
    OnPropertyChanged(nameof(EnableCommandBar));
    ```

    這可確保**EnableCommandBar**就會自動更新每當**AddingNewCustomer**變更。

### <a name="update-the-ui"></a>更新 UI

1. 瀏覽回到**Views\CustomerListPage.xaml**，並新增**StackPanel**之間的下列屬性與您**CommandBar**和您的資料格：

    ```xaml
    <StackPanel
        x:Name="newCustomerPanel"
        Orientation="Horizontal"
        x:Load="{x:Bind ViewModel.AddingNewCustomer, Mode=OneWay}"
        RelativePanel.Below="mainCommandBar">
    </StackPanel>
    ```
    **X:load**屬性可確保，此面板只會顯示當您新增新的客戶。

2. 對您的資料格，以確保它將向下移，新的面板出現時的位置中進行下列變更：

    ```xaml
    RelativePanel.Below="newCustomerPanel"
    ```

3. 具有四個更新堆疊面板**TextBox**控制項。 它們會繫結至新的客戶，個別屬性，可讓您編輯其值，再將它新增至資料格。

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

4. 簡單的按鈕加入您新的堆疊面板，以儲存新建立的客戶：

    ```xaml
    <StackPanel>
        <!--Text boxes from step 3-->
        <AppBarButton
            x:Name="SaveNewCustomer"
            Click="{x:Bind ViewModel.SaveInitialChangesAsync}"
            Icon="Save"/>
    </StackPanel>
    ```

5. 更新**CommandBar**，因此規則的建立、 刪除和更新 按鈕就會停用堆疊面板會顯示：

    ```xaml
    <CommandBar
        x:Name="mainCommandBar"
        HorizontalAlignment="Stretch"
        IsEnabled="{x:Bind ViewModel.EnableCommandBar, Mode=OneWay}"
        Background="AliceBlue">
        <!--App bar buttons-->
    </CommandBar>
    ```

6. 執行應用程式。 您現在可以建立客戶，並輸入其堆疊面板中的資料。

![建立新的客戶](images/customer-database-tutorial/add-new-customer.png)

## <a name="part-7-delete-a-customer"></a>第 7 節：刪除客戶

刪除客戶是最終的基本作業，您需要實作。 當您刪除您所選取資料格內的客戶時，您會想要立即呼叫**UpdateCustomersAsync**才能更新 UI。 不過，您不需要呼叫該方法，如果您要刪除您剛剛建立的客戶。

1. 瀏覽至**ViewModels\CustomerListPageViewModel.cs**，並更新**DeleteAndUpdateAsync**方法：

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

2. 在  **Views\CustomerListPage.xaml**，更新 堆疊面板加入新的客戶，因此包含第二個按鈕：

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

3. 在  **ViewModels\CustomerListPageViewModel.cs**，更新**DeleteNewCustomerAsync**方法來刪除新的客戶：

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

4. 執行應用程式。 您現在可以刪除在資料格中，或在堆疊面板中的客戶。

![刪除新的客戶](images/customer-database-tutorial/delete-new-customer.png)

## <a name="conclusion"></a>結論

恭喜！ 進行這一切完成，您的應用程式現在會有完整的本機資料庫作業。 您可以建立、 讀取、 更新和刪除您的使用者介面內的客戶，以及這些變更儲存到您的資料庫，並會保存到不同的會啟動您的應用程式。

現在，您已完成，請考慮下列各項：
* 如果您還沒有這麼做，請參閱[應用程式結構概觀](../enterprise/customer-database-app-structure.md)如需有關為什麼應用程式建置其用法。
* 瀏覽[完整的客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)以查看本教學課程根據應用程式。

或者，如果您是進行一項挑戰，您可以繼續及更新版本...

## <a name="going-further-connect-to-a-remote-database"></a>繼續進行：連接到遠端資料庫

我們提供了如何實作這些呼叫，對本機 SQLite 資料庫的逐步解說。 但是，如果您想要改為使用遠端資料庫？

如果您想要放手一試，您將需要您自己的 Azure Active Directory (AAD) 帳戶，並且能夠裝載您自己的資料來源。

您要新增驗證，函式來處理 REST 呼叫，然後再建立 遠端資料庫進行互動。 程式碼正在完整[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)，您可以參考的每個必要的作業。

### <a name="settings-and-configuration"></a>設定和組態

連線到遠端資料庫的必要步驟會在拼[範例的讀我檔案](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/README.md)。 您必須執行下列作業：

* 提供您的 Azure 帳戶的用戶端識別碼來[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 提供的遠端資料庫的 url [Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 提供資料庫的連接字串[Constants.cs](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoRepository/Constants.cs)。
* 將您的應用程式與 Microsoft Store 產生關聯。
* 透過複製[服務專案](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoService)到您的應用程式，並將它部署至 Azure。

### <a name="authentication"></a>Authentication

您必須建立一個按鈕來開始驗證序列，並快顯視窗或個別的頁面，來收集使用者的資訊。 一旦您已建立的您必須提供要求的使用者資訊，並使用它來取得存取權杖的程式碼。 客戶訂單資料庫範例會包裝呼叫 Microsoft Graph **web 帳戶管理員**程式庫來取得權杖，以及處理至 AAD 帳戶的驗證。

* 中實作驗證邏輯[ **AuthenticationViewModel.cs**](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/ViewModels/AuthenticationViewModel.cs)。
* 在驗證程序會顯示在自訂[ **AuthenticationControl.xaml** ](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/UserControls/AuthenticationControl.xaml)控制項。

### <a name="rest-calls"></a>REST 呼叫

您不需要修改任何程式碼的我們已新增在本教學課程才能實作 REST 呼叫。 相反地，您必須執行下列作業：

* 建立新的實作**ICustomerRepository**並**ITutorialRepository**實作函式，而不是 SQLite REST 透過一組相同的介面。 您必須序列化和還原序列化 JSON，並可以將您的 REST 呼叫包裝在個別**HttpHelper**如果您需要的類別。 請參閱[完整的範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/tree/master/ContosoRepository/Rest)如需詳細資訊。
* 在  **App.xaml.cs**、 建立新的函式，來初始化 REST 存放庫中，以及呼叫它，而不要**SqliteDatabase**當初始化應用程式。 同樣地，請參閱[完整的範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database/blob/master/ContosoApp/App.xaml.cs)。

所有三個步驟完成後，您應該能夠向您的 AAD 帳戶，透過您的應用程式。 遠端資料庫的 REST 呼叫將會取代本機 SQLite 呼叫，但使用者體驗應該相同。 如果您覺得更龐大，您可以新增設定 頁面可讓使用者動態地在兩者之間切換。