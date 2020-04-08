---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的 UWP 應用程式。
title: 使用 XAML Islands 新增 UWP CalendarViews 控制項
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643367"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部分：使用 XAML Islands 新增 UWP CalendarViews 控制項

這是教學課程的第三部分，示範如何讓名為 Contoso Expenses 的範例 WPF 傳統型應用程式現代化。 如需教學課程概觀、必要條件和下載範例應用程式的指示，請參閱[教學課程：讓 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已經完成[第 2 部分](modernize-wpf-tutorial-2.md)。

在本教學課程的虛構案例中，Contoso 開發小組想要讓您更輕鬆地在已啟用觸控功能的裝置上選擇費用報表的日期。 在本教學課程的這個部分中，您會將 UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view) 控制項新增至應用程式。 這是在工作列上的 Windows 10 日期和時間功能中使用的相同控制項。

![CalendarViewControl 映像](images/wpf-modernize-tutorial/CalendarViewControl.png)

不同於您在[第 2 部分](modernize-wpf-tutorial-2.md)新增的 **InkCanvas** 控制項，Windows 社群工具組不會提供可在 WPF 應用程式中使用的 UWP **CalendarView** 包裝版本。 或者，您將在泛型 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項中裝載 **InkCanvas**。 您可使用此控制項來裝載 Windows SDK 或 WinUI 程式庫所提供的任何第一方 UWP 控制項，或由第三方所建立的任何自訂 UWP 控制項。 **WindowsXamlHost** 控制項是由 `Microsoft.Toolkit.Wpf.UI.XamlHost` NuGet 套件所提供。 此套件隨附於您在[第 2 部分](modernize-wpf-tutorial-2.md)安裝的 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 套件。

> [!NOTE]
> 本教學課程只會示範如何使用 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 來裝載 Windows SDK 所提供的第一方 **CalendarView** 控制項。 如需示範如何裝載自訂控制項的逐步解說，請參閱[使用 XAML Islands 在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)。

若要使用 **WindowsXamlHost** 控制項，您必須直接從 WPF 應用程式中的程式碼呼叫 WinRT API。 `Microsoft.Windows.SDK.Contracts` NuGet 套件包含可讓您從應用程式呼叫 WinRT API 所需的參考。 此套件也包含於您在[第 2 部分](modernize-wpf-tutorial-2.md)安裝的 `Microsoft.Toolkit.Wpf.UI.Controls` NuGet 套件。

## <a name="add-the-windowsxamlhost-control"></a>新增 WindowsXamlHost 控制項

1. 在 [方案總管]  中，展開 **ContosoExpenses.Core** 專案的 **Views** 資料夾，然後按兩下 **AddNewExpense.xaml** 檔案。 這是用來將新費用新增至清單的表單。 以下是其在目前應用程式版本中的顯示方式。

    ![新增費用](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF 中包含的日期選擇器控制項，主要用於具有滑鼠和鍵盤的傳統電腦。 使用觸控式螢幕選擇日期並未真正可行，因為控制項的大小很小，且行事曆中每一天之間的空間有限。

2. 在 **AddNewExpense.xaml**欄位的頂端，將下列屬性新增至 **Window** 元素。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    新增此屬性之後，**Window** 元素現在應該如下所示。

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="450" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

3. 將 **Window** 元素的 **Height** 屬性從 450 變更為 800。 必須這麼做，因為 UWP **CalendarView** 控制項所佔用的空間比 WPF 日期選擇器還多。

    ```xml
    <Window x:Class="ContosoExpenses.Views.AddNewExpense"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            DataContext="{Binding Source={StaticResource ViewModelLocator},Path=AddNewExpenseViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Add new expense" Height="800" Width="800"
            Background="{StaticResource AddNewExpenseBackground}">
    ```

4. 找出靠近檔案底部的 `DatePicker` 元素，並以下列 XAML 取代此元素。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    此 XAML 會新增 **WindowsXamlHost** 控制項。 **InitialTypeName** 屬性會指出您想要裝載的 UWP 控制項完整名稱 (在此案例中為 **Windows.UI.Xaml.Controls.CalendarView**)。

5. 按 F5 以在偵錯工具中建置和執行應用程式。 從清單中選擇員工，然後按 [新增費用]  按鈕。 確認下列頁面裝載新的 UWP **CalendarView** 控制項。

    ![CalendarView 包裝函式](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 關閉應用程式。

## <a name="interact-with-the-windowsxamlhost-control"></a>與 WindowsXamlHost 控制項互動

接下來，您將會更新應用程式以處理所選的日期、將其顯示在畫面上，然後填入要儲存在資料庫中的 **Expense** 物件。

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) 包含與此案例相關的兩個成員：

- **SelectedDates** 屬性包含使用者所選取的日期。
- 當使用者選取日期時，就會引發 **SelectedDatesChanged** 事件。

不過，**WindowsXamlHost** 控制項是泛型主控制項，適用於「任何」  種類的 UWP 控制項。 因此，其不會公開名為 **SelectedDates** 的屬性或名為 **SelectedDatesChanged** 的事件，因為這些是 **CalendarView** 控制項專屬的項目。 若要存取這些成員，您必須撰寫程式碼，將 **WindowsXamlHost** 轉換成 **CalendarView** 類型。 執行此動作的最佳位置是在 **WindowsXamlHost** 控制項的 **ChildChanged** 事件回應中，已轉譯裝載的控制項時會引發此事件。

1. 在 **AddNewExpense.xaml** 檔案中，為您稍早所新增 **WindowsXamlHost** 控制項的 **ChildChanged** 事件新增事件處理常式。 當您完成時，**WindowsXamlHost** 元素應該如下所示。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 在相同的檔案中，找出主要 **Grid**的 **Grid.RowDefinitions** 元素。 再新增一個 **RowDefinition** 元素，其 **Height** 等於子元素清單結尾的 **Auto**。 當您完成時，**Grid.RowDefinitions** 元素應該如下所示 (現在應該有 9 個 **RowDefinition** 元素)。

    ```xml
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    ```

4. 將下列 XAML 新增至 **WindowsXamlHost** 元素之後，且在接近檔案結尾的 **Button** 元素之前。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 找出靠近檔案結尾的 **Button** 元素，然後將 **Grid.Row** 屬性從 **7** 變更為 **8**。 這會使按鈕在方格中向下移動一列，因為您已新增新的一列。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 開啟 **AddNewExpense.xaml.cs** 程式碼檔案。

7. 將以下陳述式新增至檔案頂端。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 將以下事件處理常式新增至 `AddNewExpense` 類別。 此程式碼會針對您稍早在 XAML 檔案中宣告的 **WindowsXamlHost** 控制項，實作 **ChildChanged** 事件。

    ```csharp
    private void CalendarUwp_ChildChanged(object sender, System.EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    Messenger.Default.Send<SelectedDateMessage>(new SelectedDateMessage(args.AddedDates[0].DateTime));
                }
            };
        }
    }
    ```

    此程式碼會使用 **WindowsXamlHost** 控制項的 **Child** 屬性，存取 UWP **CalendarView** 控制項。 此程式碼會接著訂閱 **SelectedDatesChanged** 事件，當使用者從行事曆選取日期時，就會觸發此事件。 此事件處理常式會將選取的日期傳遞至 ViewModel，其中會建立新的 **Expense** 物件並儲存到資料庫。 為了這麼做，程式碼會使用 MVVM Light NuGet 套件所提供的訊息基礎結構。 程式碼會將名為 **SelectedDateMessage** 的訊息傳送至 ViewModel，其會接收該訊息，並使用所選的值來設定 **Date** 屬性。 在此案例中，**CalendarView** 控制項已設為單選模式，因此集合只會包含一個元素。

    此時，專案仍然不會編譯，因為它遺漏了 **SelectedDateMessage** 類別的實作。 下列步驟會實作此類別。

9. 在 [方案總管]  中，以滑鼠右鍵按一下 [訊息]  資料夾，然後選擇 [新增] -> [類別]  。 將新類別命名為 **SelectedDateMessage**，然後按一下 [新增]  。

10. 使用下列程式碼取代 **SelectedDateMessage.cs**程式碼檔案的內容。 此程式碼會為 `GalaSoft.MvvmLight.Messaging` 命名空間新增 using 陳述式 (從 MVVM Light NuGet 套件)、繼承自 **MessageBase** 類別，以及新增透過公用建構函式初始化的 **DateTime** 屬性。 當您完成時，檔案應該如下所示。

    ```csharp
    using GalaSoft.MvvmLight.Messaging;
    using System;
    
    namespace ContosoExpenses.Messages
    {
        public class SelectedDateMessage: MessageBase
        {
            public DateTime SelectedDate { get; set; }
    
            public SelectedDateMessage(DateTime selectedDate)
            {
                this.SelectedDate = selectedDate;
            }
        }
    }
    ```

11. 接著，您會更新 ViewModel 以接收此訊息，並填入 ViewModel 的 **Date** 屬性。 在 [方案總管]  中，展開 **ViewModels** 資料夾，然後開啟 **AddNewExpenseViewModel.cs** 檔案。

12. 找出 `AddNewExpenseViewModel` 類別的公用建構函式，並在建構函式的結尾新增下列程式碼。 此程式碼會註冊以接收 **SelectedDateMessage**，並透過 **SelectedDate** 屬性從中擷取所選的日期，而我們會使用該日期來設定 ViewModel 所公開的 **Date** 屬性。 因為此屬性會與您稍早新增的 **TextBlock** 控制項繫結，所以您可看到使用者所選取的日期。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    當您完成時，`AddNewExpenseViewModel` 建構函式應該如下所示。 

    ```csharp
    public AddNewExpenseViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        this.databaseService = databaseService;
        this.storageService = storageService;

        Date = DateTime.Today;

        Messenger.Default.Register<SelectedDateMessage>(this, message =>
        {
            Date = message.SelectedDate;
        });
    }
    ```

    > [!NOTE]
    > 相同程式碼檔案中的 `SaveExpenseCommand` 方法會進行將費用儲存到資料庫的工作。 此方法會使用 **Date** 屬性來建立新的 **Expense** 物件。 此屬性現在會透過您剛建立的 `SelectedDateMessage` 訊息，由 **CalendarView** 控制項填入資料。

13. 按 F5 以在偵錯工具中建置和執行應用程式。 選擇其中一個可用的員工，然後按一下 [新增費用]  按鈕。 完成表單中的所有欄位，然後從新的 **CalendarView** 控制項中選擇日期。 確認所選的日期會顯示為控制項下方的字串。

14. 按 [儲存]  按鈕。 表單將會關閉，而新費用會新增至費用清單的結尾。 具有費用日期的第一個資料行應該是您在 **CalendarView** 控制項中選取的日期。

## <a name="next-steps"></a>接下來的步驟

目前在此教學課程中，您已成功將 WPF 日期時間控制項取代為 UWP **CalendarView** 控制項，除了滑鼠和鍵盤輸入之外，該控制項還支援觸控筆和數位筆。 雖然 Windows 社群工具組不提供 UWP **CalendarView** 控制項的包裝版本 (可直接在 WPF 應用程式中使用)，但您可使用泛型 [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) 控制項來裝載此控制項。

您現在已準備好開始進行[第 4 部分：新增 Windows 10 使用者活動及通知](modernize-wpf-tutorial-4.md)。
