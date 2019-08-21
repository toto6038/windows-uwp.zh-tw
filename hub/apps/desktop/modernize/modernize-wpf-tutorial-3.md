---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件, 以及將其他現代化元件併入您的 WPF 應用程式中。
title: 使用 XAML Islands 新增 UWP CalendarViews 控制項
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 830c1cdf2e24e716d51642bc65b5b6783d0d784a
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643367"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部分：使用 XAML Islands 新增 UWP CalendarViews 控制項

這是教學課程的第三個部分, 示範如何將名為 Contoso 費用的範例 WPF 桌面應用程式現代化。 如需教學課程、必要條件和下載範例應用程式指示的總覽, 請參閱[教學課程:將 WPF 應用程式](modernize-wpf-tutorial.md)現代化。 本文假設您已完成[第2部分](modernize-wpf-tutorial-2.md)。

在本教學課程的虛構案例中, Contoso 開發小組想要讓您更輕鬆地在已啟用觸控功能的裝置上選擇支出報表的日期。 在本教學課程的這個部分中, 您會將 UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)控制項新增至應用程式。 這是在工作列上的 Windows 10 [日期和時間] 功能中使用的相同控制項。

![CalendarViewControl 影像](images/wpf-modernize-tutorial/CalendarViewControl.png)

不同于您在[第2部分](modernize-wpf-tutorial-2.md)新增的**InkCanvas**控制項, Windows 社區工具組不會提供可在 WPF 應用程式中使用之 UWP **CalendarView**的包裝版本。 或者, 您會在泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項中裝載**InkCanvas** 。 您可以使用此控制項來裝載 Windows SDK 或 WinUI 程式庫所提供的任何第一方 UWP 控制項, 或由協力廠商所建立的任何自訂 UWP 控制項。 **WindowsXamlHost**控制項是由`Microsoft.Toolkit.Wpf.UI.XamlHost`封裝 NuGet 封裝所提供。 此套件包含在`Microsoft.Toolkit.Wpf.UI.Controls` [第2部分](modernize-wpf-tutorial-2.md)所安裝的 NuGet 套件中。

> [!NOTE]
> 本教學課程只會示範如何使用[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)來裝載 Windows SDK 所提供的第一方**CalendarView**控制項。 如需示範如何裝載自訂控制項的逐步解說, 請參閱[使用 XAML 島在 WPF 應用程式中裝載自訂 UWP 控制項](host-custom-control-with-xaml-islands.md)。

若要使用**WindowsXamlHost**控制項, 您必須直接從 WPF 應用程式中的程式碼呼叫 WinRT api。 `Microsoft.Windows.SDK.Contracts` NuGet 套件包含可讓您從應用程式呼叫 WinRT api 所需的參考。 此套件也包含在`Microsoft.Toolkit.Wpf.UI.Controls` [第2部分](modernize-wpf-tutorial-2.md)所安裝的 NuGet 套件中。

## <a name="add-the-windowsxamlhost-control"></a>新增 WindowsXamlHost 控制項

1. 在**方案總管**中, 展開 [ **ContosoExpenses** ] 專案中的 [ **Views** ] 資料夾, 然後按兩下 [ **AddNewExpense** ] 檔案。 這是用來將新費用加入清單的表單。 以下是它在目前版本的應用程式中的顯示方式。

    ![加入新費用](images/wpf-modernize-tutorial/AddNewExpense.png)

    WPF 中包含的日期選擇器控制項, 適用于使用滑鼠和鍵盤的傳統電腦。 選擇使用觸控式螢幕的日期並不是真正可行的, 因為控制項的大小很小, 而且日曆中的每一天都有有限的空間。

2. 在**AddNewExpense**的頂端, 將下列屬性新增至**Window**元素。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    加入這個屬性之後, **Window**元素現在看起來應該像這樣。

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

3. 將**Window**元素的**Height**屬性從450變更為800。 這是必要的, 因為 UWP **CalendarView**控制項所佔用的空間比 WPF 日期選擇器還多。

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

4. 找出靠近檔案底部的元素,並以下列XAML取代此元素。`DatePicker`

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    此 XAML 會加入**WindowsXamlHost**控制項。 **InitialTypeName**屬性會指出您想要裝載之 UWP 控制項的完整名稱 (在此例中為**CalendarView**)。

5. 按 F5 以在偵錯工具中建立並執行應用程式。 從清單中選擇員工, 然後按 [新增**費用**] 按鈕。 確認下列頁面主控新的 UWP **CalendarView**控制項。

    ![CalendarView 包裝函式](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 關閉應用程式。

## <a name="interact-with-the-windowsxamlhost-control"></a>與 WindowsXamlHost 控制項互動

接下來, 您將更新應用程式來處理選取的日期、將它顯示在螢幕上, 然後填入要儲存在資料庫中的**費用**物件。

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)包含兩個與此案例相關的成員:

- **SelectedDates**屬性包含使用者所選取的日期。
- 當使用者選取日期時, 就會引發**SelectedDatesChanged**事件。

不過, **WindowsXamlHost**控制項是適用于*任何*一種 UWP 控制項的泛型主控制項。 因此, 它不會公開名為**SelectedDates**的屬性或稱為**SelectedDatesChanged**的事件, 因為它們是**CalendarView**控制項的特定專案。 若要存取這些成員, 您必須撰寫程式碼, 將**WindowsXamlHost**轉換為**CalendarView**類型。 執行此動作的最佳位置是回應**WindowsXamlHost**控制項的**ChildChanged**事件, 這會在已呈現裝載的控制項時引發。

1. 在**AddNewExpense**中, 新增您稍早新增的**WindowsXamlHost**控制項**ChildChanged**事件的事件處理常式。 當您完成時, **WindowsXamlHost**元素看起來應該像這樣。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 在相同的檔案中, 找出主要**方格**的**RowDefinitions**元素。 在子專案清單結尾處, 新增一個**RowDefinition**元素, 其**高度**等於 [**自動**]。 當您完成時, **RowDefinitions**元素看起來應該像這樣 (現在應該有9個**RowDefinition**元素)。

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

4. 將下列 XAML 新增至**WindowsXamlHost**元素後面, 並在接近檔案結尾的**Button**元素之前。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 找出靠近檔案結尾處的**Button**元素, 並將**Grid. Row**屬性從**7**變更為**8**。 這會將按鈕向下移動方格中的一個資料列, 因為您已加入新的資料列。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 開啟**AddNewExpense.xaml.cs**程式碼檔案。

7. 將下列語句新增至檔案頂端。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 將下列事件處理常式新增至`AddNewExpense`類別。 此程式碼會執行您稍早在 XAML 檔案中宣告之**WindowsXamlHost**控制項的**ChildChanged**事件。

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

    這段程式碼會使用**WindowsXamlHost**控制項的**Child**屬性來存取 UWP **CalendarView**控制項。 然後, 程式碼會訂閱**SelectedDatesChanged**事件, 而當使用者從行事曆選取日期時, 就會觸發此事件。 這個事件處理常式會將選取的日期傳遞至 ViewModel, 其中會建立新的**Expense**物件, 並將其儲存至資料庫。 若要這樣做, 程式碼會使用 MVVM Light NuGet 封裝所提供的訊息基礎結構。 程式碼會將名為**SelectedDateMessage**的訊息傳送至 ViewModel, 它會接收它, 並使用選取的值來設定**Date**屬性。 在此案例中, **CalendarView**控制項是針對單一選取模式所設定, 因此集合只會包含一個元素。

    此時, 專案仍然不會編譯, 因為它遺漏了**SelectedDateMessage**類別的實作為。 下列步驟會執行此類別。

9. 在**方案總管**中, 以滑鼠右鍵按一下 [**訊息**] 資料夾, 然後選擇 [**加入 > 類別**]。 將新類別命名為**SelectedDateMessage** ,然後按一下 [新增]。

10. 將**SelectedDateMessage.cs**程式碼檔案的內容取代為下列程式碼。 這段程式碼會為`GalaSoft.MvvmLight.Messaging`命名空間新增 using 語句 (從 MVVM light NuGet 封裝)、繼承自**MessageBase**類別, 並加入透過公用函式初始化的**DateTime**屬性。 當您完成時, 檔案看起來應該像這樣。

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

11. 接下來, 您將更新 ViewModel 以接收此訊息, 並填入 ViewModel 的**Date**屬性。 在**方案總管**中, 展開 [ **viewmodel** ] 資料夾, 然後開啟**AddNewExpenseViewModel.cs**檔案。

12. 找出`AddNewExpenseViewModel`類別的公用函式, 並將下列程式碼加入至此函式的結尾。 此程式碼會註冊以接收**SelectedDateMessage**、透過**SelectedDate**屬性將選取的日期解壓縮, 並使用它來設定 ViewModel 所公開的**日期**屬性。 因為此屬性會與您稍早新增的**TextBlock**控制項系結, 所以您可以看到使用者選取的日期。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    當您完成時, `AddNewExpenseViewModel`此函式看起來應該像這樣。 

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
    > 相同`SaveExpenseCommand`程式碼檔案中的方法會執行將費用儲存到資料庫的工作。 這個方法會使用**Date**屬性來建立新的**Expense**物件。 **CalendarView**控制項現在會透過您剛建立的`SelectedDateMessage`訊息填入此屬性。

13. 按 F5 以在偵錯工具中建立並執行應用程式。 選擇其中一個可用的員工, 然後按一下 [新增**費用**] 按鈕。 完成表單中的所有欄位, 然後從新的**CalendarView**控制項中選擇日期。 確認選取的日期顯示為控制項底下的字串。

14. 按 [**儲存**] 按鈕。 表單會關閉, 並將新的費用加入至費用清單的結尾。 具有 [支出日期] 的第一個資料行應該是您在**CalendarView**控制項中選取的日期。

## <a name="next-steps"></a>後續步驟

此時在教學課程中, 您已成功將 WPF 日期時間控制項取代為 UWP **CalendarView**控制項, 它除了滑鼠和鍵盤輸入之外, 還支援觸控和數位筆。 雖然 Windows 社區工具組未提供可直接在 WPF 應用程式中使用之 UWP **CalendarView**控制項的包裝版本, 但您可以使用泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項來裝載控制項。

您現在已準備好[開始進行第4部分:新增 Windows 10 使用者活動和通知](modernize-wpf-tutorial-4.md)。
