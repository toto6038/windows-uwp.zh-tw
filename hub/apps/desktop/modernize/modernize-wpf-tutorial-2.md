---
description: 本教學課程會示範如何新增 UWP XAML 使用者介面，建立 MSIX 套件，以及您的 WPF 應用程式中納入其他現代的元件。
title: 新增 UWP InkCanvas 控制項使用 XAML 群島
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 2f8cf18bce7bec880a2cb0bef298c0b565e20208
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420092"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>第 2 部分：新增 UWP InkCanvas 控制項使用 XAML 群島

這是示範如何將範例 WPF 傳統型應用程式現代化名為 Contoso 費用的教學課程的第二個部分。 如需教學課程、 必要條件和指示，下載範例應用程式的概觀，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已完成[第 1 部分](modernize-wpf-tutorial-1.md)。

在本教學課程的虛構案例中，Contoso 開發小組會想要將支援數位簽章新增至 Contoso 費用報銷應用程式中。 UWP **InkCanvas**控制項是此案例中，絕佳的選項，因為它支援數位筆跡和 AI 技術架構的功能，例如能夠辨識的文字和圖形。 若要這樣做，您會使用[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包裝 Windows 社群工具組中，您可以使用 UWP 控制項。 這個控制項包裝的介面和功能 UWP **InkCanvas** WPF 應用程式中使用的控制項。 如需進一步瞭解包裝 UWP 控制項的詳細資訊，請參閱[控制桌面應用程式 (XAML Ostrovy) 中的主機 UWP XAML](xaml-islands.md)。

## <a name="configure-the-project-to-use-xaml-islands"></a>設定專案以使用 XAML 群島

您可以加入之前**InkCanvas** Contoso 費用報銷應用程式，您必須先設定專案以支援 UWP XAML 群島的控制項。

1. 在 Visual Studio 2019，以滑鼠右鍵按一下**ContosoExpenses.Core**專案中**方案總管**，然後選擇 **管理 NuGet 套件**。

    ![管理 Visual Studio 中的 NuGet 套件 功能表](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. 在 [ **NuGet 套件管理員**] 視窗中，按一下**瀏覽**。 選取 **包括發行前版本**選項，請搜尋`Microsoft.Toolkit.Wpf.UI.Controls`套件，並安裝最新預覽版本顯示在結果中的封裝。

    > [!NOTE]
    > 此套件包含所有必要的基礎結構裝載 UWP XAML 群島，在 WPF 應用程式，包括**InkCanvas**包裝 UWP 控制項。 類似的套件，名為`Microsoft.Toolkit.Forms.UI.Controls`適用於 Windows Forms 應用程式。

3. 以滑鼠右鍵按一下**ContosoExpenses.Core**專案中**方案總管**，然後選擇 **加入新項目->** 。

4. 選取**應用程式資訊清單檔**，其命名**app.manifest**，然後按一下**新增**。

5. 在開啟資訊清單檔案中，找出**相容性**區段，然後找出下列加上註解的項目。

    ```xml
    <!-- Windows 10 -->
    <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    ```

6. 此項目，下方新增下列項目。

    ```xml
    <maxversiontested Id="10.0.18362.0"/>
    ```

7. 取消註解**加入 supportedOS**適用於 Windows 10 的項目。 此區段現在看起來應該像這樣。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    <maxversiontested Id="10.0.18362.0"/>
    ```

    > [!NOTE]
    > 這個項目可讓您指定的應用程式需要 Windows 10 版本 1903年 （組建 18362） 或更新版本。 這是支援 XAML 群島的 Windows 10 的第一個版本。 沒有此應用程式資訊清單中的項目，應用程式將會擲回執行階段例外狀況。

8. 在資訊清單檔案中，找出下列加上註解**應用程式**一節。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

9. 刪除這一節，並以下列 XML 取代它。 這會設定為 DPI 感知並更好的控制代碼縮放係數不同 Windows 10 支援的應用程式。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

10. 儲存並關閉`app.manifest`檔案。

12. 在 **方案總管**，以滑鼠右鍵按一下**ContosoExpenses.Core**專案，然後選擇**屬性**。

13. 在 **資源**一節**應用程式**索引標籤上，確定**資訊清單**下拉式清單設定為**app.manifest**。

    ![.NET core 應用程式資訊清單](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

16. 在專案屬性中儲存的變更。

## <a name="add-an-inkcanvas-control-to-the-app"></a>InkCanvas 控制項加入至應用程式

既然您已設定您的專案使用 UWP XAML 群島，您現在已準備好新增[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包裝 UWP 控制項至應用程式。

1. 在 [**方案總管] 中**，展開**檢視**資料夾**ContosoExpenses.Core**專案，然後按兩下**ExpenseDetail.xaml**檔案。

2. 在 **視窗**XAML 檔案中，頂端附近的項目中新增下列屬性。 這會參考的 XAML 命名空間[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包裝 UWP 控制項。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    之後將這個屬性中，加入**視窗**元素現在看起來應該像這樣。

    ```xml
    <Window x:Class="ContosoExpenses.Views.ExpenseDetail"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:converters="clr-namespace:ContosoExpenses.Converters"
            DataContext="{Binding Source={StaticResource ViewModelLocator}, Path=ExpensesDetailViewModel}"
            xmlns:local="clr-namespace:ContosoExpenses"
            mc:Ignorable="d"
            Title="Expense Detail" Height="500" Width="800"
            Background="{StaticResource HorizontalBackground}">
    ```

4. 在  **ExpenseDetail.xaml**檔案，找出在結尾`</Grid>`前面的標記`<!-- Chart -->`註解。 加入下列 XAML，則會在結尾之前`</Grid>`標記。 將此 XAML **InkCanvas**控制項 (前面加上**toolkit**您稍早定義的命名空間的關鍵字) 和簡單**TextBlock** ，做為控制項的標頭。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 儲存**ExpenseDetail.xaml**檔案。

6. 按 F5 以偵錯工具中執行應用程式。

7. 從清單中的員工，然後選擇其中一個可用的費用。 請注意費用詳細資料頁面，包含空間**InkCanvas**控制項。

    ![筆墨畫布手寫筆](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    如果您擁有的裝置可支援數位筆，介面，例如，您將實體機器上執行這個實驗室中移，然後再次嘗試使用它。 您會看到在畫面上顯示數位筆跡。 不過，如果您沒有畫筆功能的裝置，而且您嘗試登入，使用滑鼠，不會發生。 這種現象因為**InkCanvas**控制項預設會啟用只會針對數位的畫筆。 不過，我們可以變更此行為。

8. 關閉應用程式，然後按兩下**ExpenseDetail.xaml.cs**下方的檔案**檢視**資料夾**ContosoExpenses.Core**專案。

9. 類別的頂端新增下列命名空間宣告：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 找出`ExpenseDetail()`建構函式。

11. 加入下列行之後的程式碼權限的`InitializeComponent()`方法，並將儲存的程式碼檔案。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    您可以使用**InkPresenter**來自訂筆跡體驗的預設值的物件。 此程式碼會使用**InputDeviceTypes**啟用滑鼠及手寫筆輸入的屬性。

12. 再次按 F5 以重新建置並執行應用程式偵錯工具。 從清單中的員工，然後選擇其中一個可用的費用。

13. 現在嘗試簽章空間中，使用滑鼠在繪製項目。 此時，您會看到出現在螢幕上的筆墨。

    ![簽名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>後續步驟

此時在教學課程中，您已成功新增 UWP **InkCanvas** Contoso 費用報銷應用程式的控制項。 您現在準備好進行[第 3 部分：新增 UWP CalendarView 控制項使用 XAML 群島](modernize-wpf-tutorial-3.md)。
