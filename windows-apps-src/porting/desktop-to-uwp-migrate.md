---
Description: 桌面應用程式和 UWP 應用程式之間共用程式碼
title: 桌面應用程式和 UWP 應用程式之間共用程式碼
ms.date: 10/03/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 16b75226d6b79b19978ddf7e37231b15ac7a4e3e
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984172"
---
# <a name="move-from-a-desktop-application-to-uwp"></a>移至 UWP 的桌面應用程式

如果您有現有的傳統型應用程式使用 （包括 WPF 和 Windows Form） 的.NET Framework 所建置或C++Win32 Api，您有幾個選項將移到通用 Windows 平台 (UWP) 和 Windows 10。

## <a name="package-your-desktop-application-in-an-msix-package"></a>封裝您 MSIX 封裝中的傳統型應用程式

您可以封裝 MSIX 封裝中您的桌面應用程式以存取許多 Windows 10 功能。 MSIX 是針對所有的 Windows 應用程式，包括 UWP、 WPF、 Windows Form 和 Win32 應用程式提供通用的封裝體驗的新式 Windows 應用程式封裝格式。 封裝 MSIX 封裝中的您傳統型 Windows 應用程式可讓您存取強大的安裝和更新體驗，受管理的安全性模型具有彈性的功能系統、 支援 Microsoft Store、 企業管理和許多自訂散發模型。 您可以封裝您的應用程式是否有原始程式碼，或如果您只需要現有的安裝程式檔案 （例如 MSI 或 APP-V 安裝程式）。 封裝您的應用程式之後，您可以整合 UWP 功能，例如封裝擴充功能和其他 UWP 元件。

如需詳細資訊，請參閱 <<c0> [ 桌面應用程式 （傳統型橋接器） 封裝](/windows/msix/desktop/desktop-to-uwp-root)並[需要的套件識別功能](/windows/apps/desktop/modernize/modernize-packaged-apps)。

## <a name="use-uwp-apis"></a>使用 UWP Api

您可以直接在您的 WPF、 Windows Form 中呼叫許多 UWP Api 或C++來整合新式體驗也淺註冊 Windows 10 使用者的 Win32 桌面應用程式。 例如，您可以呼叫 UWP Api 來將快顯通知新增至您的桌面應用程式。

如需詳細資訊，請參閱 <<c0> [ 桌面應用程式中使用 UWP Api](/windows/apps/desktop/modernize/desktop-to-uwp-enhance)。

## <a name="migrate-a-net-framework-app-to-a-uwp-app"></a>移轉 .NET Framework 應用程式的 UWP 應用程式

如果您的應用程式在.NET Framework 上執行，您可以利用.NET Standard 2.0 將它移轉至 UWP 應用程式。 移動程式碼以及您可以到.NET Standard 2.0 類別庫，然後建立參考您的.NET Standard 2.0 程式庫的 UWP 應用程式。 

### <a name="share-code-in-a-net-standard-20-library"></a>共用 .NET Standard 2.0 程式庫中的程式碼

如果您的應用程式在.NET Framework 上執行，將程式碼，您可以到.NET Standard 2.0 類別庫。 只要您的程式碼使用 Standard 中定義的 API，就可以在 UWP app 中重複使用該程式碼。 這比以往任何時候都還要容易共用 .NET Standard 程式庫的程式碼，因為 .NET Standard 2.0 中包含更加多的 API 了。

以下是告訴您其相關的影片。

> [!VIDEO https://www.youtube-nocookie.com/embed/YI4MurjfMn8?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=1]

#### <a name="add-net-standard-libraries"></a>新增 .NET Standard 程式庫

首先將一個或多個 .NET Standard 類別庫新增至您的方案。  

![新增 DotNet 標準專案](images/desktop-to-uwp/dot-net-standard-project-template.png)

您新增至方案的程式庫數目取決於您想要如何組合管理程式碼。

請確定每個類別庫都是以 **.NET Standard 2.0** 為目標。

![將目標設為 .NET Standard 2.0](images/desktop-to-uwp/target-standard-20.png)

您可以在類別庫專案的屬性頁中找到此設定。

在傳統型應用程式專案中，新增類別庫專案的參考。

![類別庫參考](images/desktop-to-uwp/class-library-reference.png)

接下來，使用工具來判斷您的程式碼有多少符合標準。 如此一來，將程式碼移入程式庫之前就可以決定您可重複使用哪些部分、哪些部分需要進行最基本的修改，以及哪些部分要保留給特定應用程式專用。

#### <a name="check-library-and-code-compatibility"></a>檢查程式庫與程式碼的相容性

我們會從 Nuget 套件及其他從協力廠商取得的 dll 檔案開始著手。

如果您的應用程式使用這其中任何一個，請判斷是否與 .NET Standard 2.0 相容。 若要這樣做，您可以使用 Visual Studio 擴充功能或命令列公用程式。

您也可以使用這些工具來分析程式碼。 從這裡下載工具 ([dotnet-apiport](https://github.com/Microsoft/dotnet-apiport/releases))，然後觀看此影片以了解如何使用這些工具。
&nbsp;
> [!VIDEO https://www.youtube-nocookie.com/embed/rzs_FGPyAlY?list=PLRAdsfhKI4OWx321A_pr-7HhRNk7wOLLY&amp;ecver=2]

如果您的程式碼與標準不相容，請考慮其他可以實作該程式碼的方式。 一開始先開啟 [.NET API 瀏覽器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)。 您可以使用該瀏覽器來檢閱 .NET Standard 2.0 中提供的 API。 請務必將清單範圍限定為 .NET Standard 2.0。

![Dot Net 選項](images/desktop-to-uwp/dot-net-option.png)

您部分的程式碼專屬於特定平台，必須繼續留在傳統型應用程式專案中。

#### <a name="example-migrating-data-access-code-to-a-net-standard-20-library"></a>範例：移轉至.NET Standard 2.0 程式庫的資料存取程式碼

例如，假設我們有非常基本的 Windows Forms 應用程式會顯示從 Northwind 範例資料庫的客戶。

![Windows Forms 應用程式](images/desktop-to-uwp/win-forms-app.png)

專案包含內有名為 **Northwind** 靜態類別的 .NET Standard 2.0 類別庫。 如果我們將此程式碼移入 **Northwind** 類別，程式碼將無法編譯，因為其中使用 ``SQLConnection``、``SqlCommand`` 和 ``SqlDataReader`` 類別，以及那些未在 .NET Standard 2.0 中提供的類別。

```csharp
public static ArrayList GetCustomerNames()
{
    ArrayList customers = new ArrayList();

    using (SqlConnection conn = new SqlConnection())
    {
        conn.ConnectionString =
            @"Data Source=" +
            @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        SqlCommand command = new SqlCommand("select ContactName from customers order by ContactName asc", conn);

        using (SqlDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}

```
您可以使用 [.NET API 瀏覽器](https://docs.microsoft.com/dotnet/api/?view=netstandard-2.0)尋找替代方式。 ``DbConnection``、``DbCommand`` 和 ``DbDataReader`` 類別全都以 .NET Standard 2.0 提供，所以我們可以改為使用它們。  

此修訂版本使用這些類別來取得客戶清單，但要建立 ``DbConnection`` 類別，就必須傳入我們在用戶端應用程式中建立的 Factory 物件。

```csharp
public static ArrayList GetCustomerNames(DbProviderFactory factory)
{
    ArrayList customers = new ArrayList();

    using (DbConnection conn = factory.CreateConnection())
    {
        conn.ConnectionString = @"Data Source=" +
                        @"<Your Server Name>\SQLEXPRESS;Initial Catalog=NORTHWIND;Integrated Security=SSPI";

        conn.Open();

        DbCommand command = factory.CreateCommand();
        command.Connection = conn;
        command.CommandText = "select ContactName from customers order by ContactName asc";

        using (DbDataReader reader = command.ExecuteReader())
        {
            while (reader.Read())
            {
                customers.Add(reader[0]);
            }
        }
    }

    return customers;
}
```  

在 Windows Form 的程式碼後置頁面上，可以只是建立 Factory 執行個體後，再傳入我們的方法中。

```csharp
public partial class Customers : Form
{
    public Customers()
    {
        InitializeComponent();

        dataGridView1.Rows.Clear();

        SqlClientFactory factory = SqlClientFactory.Instance;

        foreach (string customer in Northwind.GetCustomerNames(factory))
        {
            dataGridView1.Rows.Add(customer);
        }
    }
}
```

### <a name="create-a-uwp-app"></a>建立 UWP 應用程式

現在就可以開始將 UWP app 新增至您的方案。

![傳統型轉 UWP 橋接器影像](images/desktop-to-uwp/adaptive-ui.png)

您仍然需要使用 XAML 設計 UI 頁面，並撰寫任何裝置或平台特定程式碼。不過，一旦完成，您就可以將適用範圍涵蓋全系列的 Windows 10 裝置，而您的應用程式頁面也將具備可配合不同螢幕大小及解析度適當調整的現代化觀感。

應用程式除了鍵盤和滑鼠之外，還能回應其他輸入機制，而且功能和設定在所有裝置上都會變得直覺。 這表示使用者了解過一次操作方法後，不論什麼裝置，使用起來都會非常熟悉。

以上所述只是 UWP 附帶的其中幾個好處。 若要深入了解，請參閱[建置美好的 Windows 使用體驗](https://developer.microsoft.com/windows/why-build-for-uwp)。

#### <a name="add-a-uwp-project"></a>新增 UWP 專案

首先，將 UWP 專案新增至您的方案。

![UWP 專案](images/desktop-to-uwp/new-uwp-app.png)

然後，從 UWP 專案新增 .NET Standard 2.0 程式庫專案的參考。

![類別庫參考](images/desktop-to-uwp/class-library-reference2.png)

#### <a name="build-your-pages"></a>建置您的頁面

新增 XAML 頁面，並呼叫在 .NET Standard 2.0 程式庫中的程式碼。

![UWP app](images/desktop-to-uwp/uwp-app.png)

```xml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel x:Name="customerStackPanel">
        <ListView x:Name="customerList"/>
    </StackPanel>
</Grid>
```

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        SqlClientFactory factory = SqlClientFactory.Instance;

        customerList.ItemsSource = Northwind.GetCustomerNames(factory);
    }
}
```

若要開始使用 UWP，請參閱[什麼是 UWP app](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)。

### <a name="reach-ios-and-android-devices"></a>將目標範本擴及 iOS 和 Android 裝置

您可以透過新增 Xamarin 專案，將目標範本擴及 Android 和 iOS 裝置  

![Xamarin 應用程式](images/desktop-to-uwp/xamarin-apps.png)

這些專案允許您使用 C# 來建置可以完整存取平台特定及裝置特定 API 的 Android 和 iOS 應用程式。 這些應用程式充分利用平台特定的硬體加速，並且針對原生效能進行編譯。

這些應用程式可以存取基礎平台及裝置公開的各種功能 (包括 iBeacons 和 Android Fragments 等平台特定功能)，而您將會使用標準原生使用者介面控制項來建置使用者所預期之外觀與風格的 UI。

就像 UWP 一樣，新增 Android 或 iOS 應用程式的成本較低，因為您可以重複使用 .NET Standard 2.0 類別庫中的商務邏輯。 您必須使用 XAML 設計 UI 頁面，並撰寫任何裝置或平台特定的程式碼。

#### <a name="add-a-xamarin-project"></a>新增 Xamarin 專案

首先，將 **\[Android\]**、**\[iOS\]** 或 **\[跨平台\]** 專案新增至您的方案。

您可以在 **\[加入新的專案\]** 對話方塊的 **\[Visual C#\]** 群組底下找到這些範本。

![Xamarin 應用程式](images/desktop-to-uwp/xamarin-projects.png)

>[!NOTE]
>跨平台專案非常適合只有極少平台特定功能的應用程式。 您可以使用這些專案建立一個在 Android、iOS 及 Windows 上執行的原生 XAML 型 UI。 請按一下[這裡](https://www.xamarin.com/forms)進一步了解。

接著，從 Android、iOS 或跨平台專案中新增類別庫專案的參考。

![類別庫參考](images/desktop-to-uwp/class-library-reference3.png)

#### <a name="build-your-pages"></a>建置您的頁面

我們的範例會在 Android 應用程式中顯示客戶清單。

![Android 應用程式](images/desktop-to-uwp/android-app.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="10dp" android:textSize="16sp"
    android:id="@android:id/list">
</TextView>
```

```csharp
[Activity(Label = "MyAndroidApp", MainLauncher = true)]
public class MainActivity : ListActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SqlClientFactory factory = SqlClientFactory.Instance;

        var customers = (string[])Northwind.GetCustomerNames(factory).ToArray(typeof(string));

        ListAdapter = new ArrayAdapter<string>(this, Resource.Layout.list_item, customers);
    }
}
```

若要開始使用 Android、iOS 和跨平台專案，請參閱 [Xamarin 開發人員入口網站](https://developer.xamarin.com/)。

## <a name="next-steps"></a>後續步驟

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
