---
title: 在 UWP app 中使用 SQL Server 資料庫
description: 在 UWP app 中使用 SQL Server 資料庫。
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, uwp, SQL Server, 資料庫
ms.localizationpriority: medium
ms.openlocfilehash: 5cb4b16cea3368660ffcb1bc4b252391a73ab13e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2018
ms.locfileid: "8808453"
---
# <a name="use-a-sql-server-database-in-a-uwp-app"></a>在 UWP app 中使用 SQL Server 資料庫
您的 app 可以直接連接到 SQL Server 資料庫，然後使用 [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx)命名空間中的類別儲存和擷取資料。

在本指南中，我們將說明怎麼做。 如果您在 SQL Server 執行個體上安裝 [Northwind](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/linq/downloading-sample-databases) 範例資料庫，然後使用這些程式碼片段，您就會有基本 UI 可顯示 Northwind 範例資料庫中的產品。

![Northwind 產品](images/products-northwind.png)

本指南中顯示的程式碼片段是依據這個更為[完整的範例](https://github.com/StefanWickDev/IgniteDemos/tree/master/NorthwindDemo)。

## <a name="first-set-up-your-solution"></a>首先，設定您的解決方案

若要將您的 app 直接連接到 SQL Server 資料庫，請確定您專案的最低版本目標為 Fall Creators Update。  您可以在 UWP 專案的屬性頁面中找到該資訊。

![Windows SDK 的最低版本](images/min-version-fall-creators.png)

在資訊清單設計工具中開啟 UWP 專案的 **Package.appxmanifest** 檔案。

在 **\[功能\]** 索引標籤中，選取 **\[企業驗證\]** 核取方塊。

![企業驗證功能](images/enterprise-authentication.png)

<a id="use-data" />

## <a name="add-and-retrieve-data-in-a-sql-server-database"></a>在 SQL Server 資料庫中新增和擷取資料

本節中，我們會進行下列事項：

:一: 新增連接字串。

:二: 建立類別來保留產品資料。

:三: 從 SQL Server 資料庫擷取產品。

:四: 新增基本使用者介面。

:五: 將產品填入 UI。

>[!NOTE]
> 本節將說明組織資料存取碼的方式。 主要目的僅在於示範如何使用 [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) 儲存和擷取 SQL Server 資料庫中的資料。 您可以用任何最適合您應用程式設計的方式組織程式碼。

### <a name="add-a-connection-string"></a>新增連接字串

在 **App.xaml.cs** 檔案中，新增屬性至 ``App`` 類別，讓解決方案中的其他類別能夠存取連接字串。

我們的連接字串指向 SQL Server Express 執行個體中的 Northwind 資料庫。

```csharp
sealed partial class App : Application
{
    private string connectionString =
        @"Data Source=YourServerName\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

    public string ConnectionString { get => connectionString; set => connectionString = value; }

    ...
}
```

### <a name="create-a-class-to-hold-product-data"></a>建立類別來保留產品資料

我們將建立類別來實作 [INotifyPropertyChanged](https://msdn.microsoft.com/library/system.componentmodel.inotifypropertychanged.aspx) 事件，以便將 XAML UI 中的屬性繫結到此類別中的屬性。

```csharp
public class Product : INotifyPropertyChanged
{
    public int ProductID { get; set; }
    public string ProductCode { get { return ProductID.ToString(); } }
    public string ProductName { get; set; }
    public string QuantityPerUnit { get; set; }
    public decimal UnitPrice { get; set; }
    public string UnitPriceString { get { return UnitPrice.ToString("######.00"); } }
    public int UnitsInStock { get; set; }
    public string UnitsInStockString { get { return UnitsInStock.ToString("#####0"); } }
    public int CategoryId { get; set; }

    public event PropertyChangedEventHandler PropertyChanged;
    private void NotifyPropertyChanged(string propertyName)
    {
        if (PropertyChanged != null)
        {
            PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
        }
    }

}
```

### <a name="retrieve-products-from-the-sql-server-database"></a>從 SQL Server 資料庫擷取產品

建立方法，以便從 Northwind 範例資料庫中取得產品，然後將其做為 ``Product`` 執行個體的 [ObservableCollection](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx) 集合傳回。

```csharp
public ObservableCollection<Product> GetProducts(string connectionString)
{
    const string GetProductsQuery = "select ProductID, ProductName, QuantityPerUnit," +
       " UnitPrice, UnitsInStock, Products.CategoryID " +
       " from Products inner join Categories on Products.CategoryID = Categories.CategoryID " +
       " where Discontinued = 0";

    var products = new ObservableCollection<Product>();
    try
    {
        using (SqlConnection conn = new SqlConnection(connectionString))
        {
            conn.Open();
            if (conn.State == System.Data.ConnectionState.Open)
            {
                using (SqlCommand cmd = conn.CreateCommand())
                {
                    cmd.CommandText = GetProductsQuery;
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        while (reader.Read())
                        {
                            var product = new Product();
                            product.ProductID = reader.GetInt32(0);
                            product.ProductName = reader.GetString(1);
                            product.QuantityPerUnit = reader.GetString(2);
                            product.UnitPrice = reader.GetDecimal(3);
                            product.UnitsInStock = reader.GetInt16(4);
                            product.CategoryId = reader.GetInt32(5);
                            products.Add(product);
                        }
                    }
                }
            }
        }
        return products;
    }
    catch (Exception eSql)
    {
        Debug.WriteLine("Exception: " + eSql.Message);
    }
    return null;
}
```

### <a name="add-a-basic-user-interface"></a>新增基本使用者介面

 將下列 XAML 新增至 UWP 專案的 **MainPage.xaml** 檔案。

 此 XAML 會建立 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 來顯示您在前一個程式碼片段中傳回的每一個產品，並且將 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 中每一列的屬性繫結至我們在 ``Product`` 類別中定義的屬性。

```xml
<Grid Background="{ThemeResource SystemControlAcrylicWindowBrush}">
    <RelativePanel>
        <ListView Name="InventoryList"
                  SelectionMode="Single"
                  ScrollViewer.VerticalScrollBarVisibility="Auto"
                  ScrollViewer.IsVerticalRailEnabled="True"
                  ScrollViewer.VerticalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollMode="Enabled"
                  ScrollViewer.HorizontalScrollBarVisibility="Auto"
                  ScrollViewer.IsHorizontalRailEnabled="True"
                  Margin="20">
            <ListView.HeaderTemplate>
                <DataTemplate>
                    <StackPanel Orientation="Horizontal"  >
                        <TextBlock Text="ID" Margin="8,0" Width="50" Foreground="DarkRed" />
                        <TextBlock Text="Product description" Width="300" Foreground="DarkRed" />
                        <TextBlock Text="Packaging" Width="200" Foreground="DarkRed" />
                        <TextBlock Text="Price" Width="80" Foreground="DarkRed" />
                        <TextBlock Text="In stock" Width="80" Foreground="DarkRed" />
                    </StackPanel>
                </DataTemplate>
            </ListView.HeaderTemplate>
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:Product">
                    <StackPanel Orientation="Horizontal" >
                        <TextBlock Name="ItemId"
                                    Text="{x:Bind ProductCode}"
                                    Width="50" />
                        <TextBlock Name="ItemName"
                                    Text="{x:Bind ProductName}"
                                    Width="300" />
                        <TextBlock Text="{x:Bind QuantityPerUnit}"
                                   Width="200" />
                        <TextBlock Text="{x:Bind UnitPriceString}"
                                   Width="80" />
                        <TextBlock Text="{x:Bind UnitsInStockString}"
                                   Width="80" />
                    </StackPanel>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </RelativePanel>
</Grid>
```

### <a name="show-products-in-the-listview"></a>在 ListView 中顯示產品

開啟 **MainPage.xaml.cs** 檔案，並將程式碼新增至 ``MainPage`` 類別的建構函式，該類別會將 [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) 的 **ItemSource** 屬性設定為 ``Product`` 執行個體的 [ObservableCollection](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx)。

```csharp
public MainPage()
{
    this.InitializeComponent();
    InventoryList.ItemsSource = GetProducts((App.Current as App).ConnectionString);
}
```

開始投影並在 UI 中看見 Northwind 範例資料庫中的產品。

![Northwind 產品](images/products-northwind.png)

探索 [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient.aspx) 命名空間，了解還能如何處理 SQL Server 資料庫中的資料。

## <a name="trouble-connecting-to-your-database"></a>連接到您的資料庫有問題嗎？

在大部分情況中，有些 SQL Server 設定需要變更。 如果您可以從另一種傳統型應用程式 (例如 Windows Forms 或 WPF 應用程式) 連接到您的資料庫，請確定您已啟用 TCP/IP for SQL Server。 您可以在**電腦管理**主控台執行。

![電腦管理](images/computer-management.png)

然後，請確定您的 SQL Server Browser 服務正在執行。

![SQL Server Browser 服務](images/sql-browser-service.png)

## <a name="next-steps"></a>後續步驟

**使用輕量資料庫儲存資料在使用者的裝置上**

請參閱[在 UWP app 中使用 SQLite 資料庫](sqlite-databases.md)。

**在不同平台的不同應用程式之間共用程式碼**

請參閱[在傳統型與 UWP 之間共用程式碼](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)。

**在 Azure SQL 後端新增主要詳細資料頁面**

請參閱[客戶訂單資料庫範例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
