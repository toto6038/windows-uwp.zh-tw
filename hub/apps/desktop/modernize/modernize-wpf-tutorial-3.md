---
description: 本教學課程會示範如何新增 UWP XAML 使用者介面，建立 MSIX 套件，以及您的 WPF 應用程式中納入其他現代的元件。
title: 新增 UWP CalendarView 控制項使用 XAML 群島
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: fce9135267461f61515c7dc04bbaf43b1e4c04ed
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420108"
---
# <a name="part-3-add-a-uwp-calendarview-control-using-xaml-islands"></a>第 3 部分：新增 UWP CalendarView 控制項使用 XAML 群島

這是示範如何將範例 WPF 傳統型應用程式現代化名為 Contoso 費用的教學課程的第三個部分。 如需教學課程、 必要條件和指示，下載範例應用程式的概觀，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已完成[第 2 部分](modernize-wpf-tutorial-2.md)。

在本教學課程的虛構案例中，Contoso 開發團隊希望能讓您更輕鬆地選擇啟用觸控式裝置上的經費支出報表的日期。 在本教學課程的這個部分，您將加入 UWP [CalendarView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/calendar-view)控制項至應用程式。 這是會在工作列上的 Windows 10 的日期和時間功能的相同控制項。

![CalendarViewControl 映像](images/wpf-modernize-tutorial/CalendarViewControl.png)

不同於**InkCanvas**控制您新增至[第 2 部分](modernize-wpf-tutorial-2.md)，Windows 社群工具組不會提供已包裝的 UWP 版本**CalendarView**可用於在 WPF 應用程式. 或者，您將託管**InkCanvas**在泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項。 您可以使用這個控制項來裝載平台或第 3 方廠商所提供的任何 UWP 控制項。 **WindowsXamlHost**控制由提供`Microsoft.Toolkit.Wpf.UI.XamlHost`封裝 NuGet 套件。 此套件會隨附`Microsoft.Toolkit.Wpf.UI.Controls`您在安裝的 NuGet 套件[第 2 部分](modernize-wpf-tutorial-2.md)。

若要使用**WindowsXamlHost**控制項，您必須直接呼叫 WinRT Api，從 WPF 應用程式中的程式碼。 `Microsoft.Windows.SDK.Contracts` NuGet 套件包含可讓您從應用程式呼叫 WinRT Api 所需的參考。 此套件也包含在`Microsoft.Toolkit.Wpf.UI.Controls`您在安裝的 NuGet 套件[第 2 部分](modernize-wpf-tutorial-2.md)。

## <a name="add-the-windowsxamlhost-control"></a>新增 WindowsXamlHost 控制項

1. 在 [**方案總管] 中**，展開**檢視**資料夾中的**ContosoExpenses.Core**專案，然後按兩下**AddNewExpense.xaml**檔案。 這是用來將新的支出新增至清單的形式。 以下是出現在目前版本的應用程式的方式。

    ![加入新的費用](images/wpf-modernize-tutorial/AddNewExpense.png)

    在 WPF 中包含的日期選擇器控制項適用於傳統滑鼠和鍵盤的電腦。 選擇有觸控式螢幕的日期不是實際可行，因為小型規模的控制項及日曆中的每一天之間的有限的空間。

2. 在頂端**AddNewExpense.xaml**檔案中，新增下列屬性以**視窗**項目。

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    之後將這個屬性中，加入**視窗**元素現在看起來應該像這樣。

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

3. 變更**高度**屬性**視窗**從 450 800 的項目。 這需要的因為 UWP **CalendarView**控制項需要較多的空間，比 WPF 日期選擇器。

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

4. 找出`DatePicker`項目底部附近的檔案，並以下列 XAML 取代此項目。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  />
    ```

    將此 XAML **WindowsXamlHost**控制項。 **InitialTypeName**屬性會指出您想要裝載的 UWP 控制項的完整名稱 (在此情況下， **Windows.UI.Xaml.Controls.CalendarView**)。

5. 按 F5 以建置並執行應用程式偵錯工具。 從清單中的員工，然後按**加入新的支出** 按鈕。 確認下列頁面裝載新的 UWP **CalendarView**控制項。

    ![CalendarView 包裝函式](images/wpf-modernize-tutorial/CalendarViewWrapper.png)

6. 關閉應用程式。

## <a name="interact-with-the-windowsxamlhost-control"></a>WindowsXamlHost 控制項互動

接下來，您將更新應用程式，以處理選取的日期，顯示在畫面上，填入**費用**儲存在資料庫中的物件。

UWP [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView)包含在此案例與相關的兩個成員：

- **SelectedDates**屬性包含使用者所選取的日期。
- **SelectedDatesChanged**使用者選取日期時，會引發事件。

不過， **WindowsXamlHost**控制項是泛型主機控制*任何*UWP 控制項的種類。 因此，它不會公開屬性，稱為**SelectedDates**或呼叫事件**SelectedDatesChanged**，因為它們是特定的**CalendarView**控制項。 若要存取這些成員，您必須撰寫程式碼轉換**WindowsXamlHost**要**CalendarView**型別。 若要執行這項操作的最佳位置是在回應**ChildChanged**事件**WindowsXamlHost**控制項，其中已呈現裝載的控制項時引發。

1. 在**AddNewExpense.xaml**檔案，加入事件處理常式，如**ChildChanged**事件**WindowsXamlHost**您先前加入的控制項。 當您完成時， **WindowsXamlHost**項目應該看起來像這樣。

    ```xml
    <xamlhost:WindowsXamlHost InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Column="1" Grid.Row="6" Margin="5, 0, 0, 0" x:Name="CalendarUwp"  ChildChanged="CalendarUwp_ChildChanged" />
    ```

2. 在相同的檔案中，找出**加在 Grid.RowDefinitions**項目，主要**格線**。 新增另一個**RowDefinition**項目**高度**等於**自動**子項目清單的結尾。 當您完成時，**加在 Grid.RowDefinitions**項目應該看起來像這樣 (現在應該有 9 **RowDefinition**項目)。

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

4. 加入下列 XAML 之後**WindowsXamlHost**項目和前面 **按鈕**接近檔案結尾處的項目。

    ```xml
    <TextBlock Text="Selected date:" FontSize="16" FontWeight="Bold" Grid.Row="7" Grid.Column="0" />
    <TextBlock Text="{Binding Path=Date}" FontSize="16" Grid.Row="7" Grid.Column="1" />
    ```

5. 找出 **按鈕**即將結束的檔案和變更的項目**Grid.Row**  屬性從**7**至**8**。 這，因為您新增新的資料列，便會進入下一個資料列，在方格中的按鈕。

    ```xml
    <Button Content="Save" Grid.Row="8" Grid.Column="0" Command="{Binding Path=SaveExpenseCommand}" Margin="5, 12, 0, 0" HorizontalAlignment="Left" Width="180" />
    ```

6. 開啟**AddNewExpense.xaml.cs**程式碼檔案。

7. 檔案開頭處新增下列陳述式。

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

8. 新增下列事件處理常式`AddNewExpense`類別。 此程式碼會實作**ChildChanged**事件**WindowsXamlHost**您稍早宣告 XAML 檔案中的控制項。

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

    此程式碼會使用**子系**屬性**WindowsXamlHost**控制存取 UWP **CalendarView**控制項。 程式碼然後訂閱**SelectedDatesChanged**使用者從日曆選取日期時，會觸發的事件。 這個事件處理常式會將選取的日期傳遞至 ViewModel 其中的新**費用**物件建立並儲存到資料庫。 若要這樣做，程式碼會使用 MVVM Light NuGet 封裝所提供的傳訊基礎結構。 程式碼會傳送一則訊息，稱為**SelectedDateMessage**到 ViewModel，這將會接收並設定**日期**屬性與所選取的值。 在此案例**CalendarView**控制項設定了單一選取模式，讓集合將會包含只有一個項目。

    到目前為止，專案仍然不會編譯，因為它遺漏的實作**SelectedDateMessage**類別。 下列步驟實作這個類別。

9. 在 **方案總管 中**，以滑鼠右鍵按一下**訊息**資料夾，然後選擇 **新增-> 類別**。 新類別命名**SelectedDateMessage**然後按一下**新增**。

10. 內容取代**SelectedDateMessage.cs**為下列程式碼的程式碼檔案。 這個程式碼加入 using 陳述式`GalaSoft.MvvmLight.Messaging`（從 MVVM Light NuGet 套件中） 的命名空間繼承自**MessageBase**類別，並新增**DateTime**透過已初始化的屬性公用建構函式。 當您完成時，檔案應該如下所示。

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

11. 接下來，您將在其中更新接收此訊息，並填入 ViewModel**日期**ViewModel 的屬性。 在 **方案總管**，展開**Viewmodel**資料夾，然後開啟**AddNewExpenseViewModel.cs**檔案。

12. 找出的公用建構函式`AddNewExpenseViewModel`類別，並將下列程式碼新增至建構函式的結尾。 此程式碼會註冊以接收**SelectedDateMessage**，從透過擷取所選的日期**SelectedDate**屬性，並使用它來設定**日期**屬性由 ViewModel。 因為這個屬性已繫結與**TextBlock**您所加入的控制項更早版本，您可以看到使用者所選取的日期。

    ```csharp
    Messenger.Default.Register<SelectedDateMessage>(this, message =>
    {
        Date = message.SelectedDate;
    });
    ```

    完成之後，`AddNewExpenseViewModel`建構函式應該看起來像這樣。 

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
    > `SaveExpenseCommand`方法，在相同的程式碼檔案會儲存到資料庫的費用。 這個方法會使用**日期**來建立新的屬性**費用**物件。 這個屬性現在正在填入所**CalendarView**透過控制`SelectedDateMessage`建立您剛才的訊息。

13. 按 F5 以建置並執行應用程式偵錯工具。 選擇其中一個可用的員工，然後按一下**加入新的支出** 按鈕。 完成表單中的所有欄位，並從新選擇日期**CalendarView**控制項。 請確認選取的日期會顯示為字串，以在控制項下方。

14. 按下**儲存** 按鈕。 表單將會關閉，而且費用清單的結尾加入新的費用。 費用日期的第一個資料行應該是您在選取的日期**CalendarView**控制項。

## <a name="next-steps"></a>後續步驟

此時在教學課程中，您已成功取代 WPF 日期時間控制項使用 UWP **CalendarView**支援觸控及數位筆，除了滑鼠和鍵盤輸入的控制項。 雖然 Windows 社群工具組不會提供已包裝的 UWP 版本**CalendarView**控制項可直接在 WPF 應用程式中，您便能夠裝載控制項使用泛型[WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)控制項。

您現在準備好進行[第 4 部分：新增 Windows 10 使用者活動及通知](modernize-wpf-tutorial-4.md)。
