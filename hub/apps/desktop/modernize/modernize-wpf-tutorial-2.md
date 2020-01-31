---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他現代化元件併入您的 WPF 應用程式中。
title: 使用 XAML Islands 新增 UWP InkCanvas 控制項
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10，uwp，windows forms，wpf，xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 945cc2f1cf225c194e5820990bdbeda584069e4c
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814038"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>第2部分：使用 XAML 群島新增 UWP InkCanvas 控制項

這是教學課程的第二部分，示範如何將名為 Contoso 費用的範例 WPF 桌面應用程式現代化。 如需有關下載範例應用程式的教學課程、必要條件和指示的總覽，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已完成[第1部分](modernize-wpf-tutorial-1.md)。

在本教學課程的虛構案例中，Contoso 開發小組想要將數位簽章的支援新增至 Contoso 費用應用程式。 UWP **InkCanvas**控制項是此案例的絕佳選項，因為它支援數位筆跡和 AI 提供的功能，例如辨識文字和圖形的功能。 若要這麼做，您將使用 Windows 社區工具組中提供的[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包裝 UWP 控制項。 此控制項會包裝 UWP **InkCanvas**控制項的介面和功能，以便在 WPF 應用程式中使用。 如需包裝 UWP 控制項的詳細資訊，請參閱[在桌面應用程式中裝載 UWP XAML 控制項（Xaml 島）](xaml-islands.md)。

> [!NOTE]
> 在本教學課程中，WPF 應用程式只會從 Windows SDK 裝載第一方 UWP 控制項。 因此，本教學課程會省略定義[XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication)類別實例的步驟，如[這裡](host-standard-control-with-xaml-islands.md#required-components)所述。

## <a name="configure-the-project-to-use-xaml-islands"></a>將專案設定為使用 XAML 群島

在您可以將**InkCanvas**控制項新增至 Contoso 費用應用程式之前，您必須先將專案設定為支援 UWP XAML 群島。

1. 在 Visual Studio 2019 中，以滑鼠右鍵按一下**方案總管**中的 [ **ContosoExpenses** ] 專案，然後選擇 [**管理 NuGet 封裝**]。

    ![Visual Studio 中的 [管理 NuGet 套件] 功能表](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. 在 [ **NuGet 套件管理員**] 視窗中，按一下 **[流覽]** 。 搜尋 `Microsoft.Toolkit.Wpf.UI.Controls` 封裝，並安裝版本6.0.0 或更新版本。

    > [!NOTE]
    > 此套件包含在 WPF 應用程式中裝載 UWP XAML Islands 的所有必要基礎結構，包括**InkCanvas**包裝的 uwp 控制項。 Windows Forms 應用程式可使用名為 `Microsoft.Toolkit.Forms.UI.Controls` 的類似套件。

3. 以滑鼠右鍵按一下**方案總管**中的 [ **ContosoExpenses**專案]，然後選擇 [**加入 > 新專案**]。

4. 選取 [**應用程式資訊清單**檔案]，將它命名為**app.config**，然後按一下 [**新增**]。 如需應用程式資訊清單的詳細資訊，請參閱[這篇文章](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests)。

5. 在資訊清單檔案中，取消批註 Windows 10 的下列 `<supportedOS>` 元素。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. 在資訊清單檔中，找出下列已加上批註的 `<application>` 元素。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. 刪除此區段，並以下列 XML 取代它。 這會將應用程式設定為 DPI 感知，並更妥善處理 Windows 10 支援的不同調整因素。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. 儲存並關閉 `app.manifest` 檔案。

9. 在**方案總管**中，以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案，然後選擇 [**屬性**]。

10. 在 [**應用程式**] 索引標籤的 [**資源**] 區段中，確認 [**資訊清單**] 下拉式清單已設定為 [ **app.config**]。

    ![.NET Core 應用程式資訊清單](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. 將變更儲存至專案屬性。

## <a name="add-an-inkcanvas-control-to-the-app"></a>將 InkCanvas 控制項新增至應用程式

現在您已將專案設定為使用 UWP XAML Islands，現在已準備好將[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包裝的 UWP 控制項新增至應用程式。

1. 在**方案總管**中，展開 [ **ContosoExpenses** ] 專案的 [ **Views** ] 資料夾，然後按兩下 [ **ExpenseDetail** ] 檔案。

2. 在 XAML 檔案頂端附近的**Window**元素中，新增下列屬性。 這會參考[InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)包裝 UWP 控制項的 XAML 命名空間。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    加入這個屬性之後， **Window**元素現在看起來應該像這樣。

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

4. 在**ExpenseDetail**中，找出緊接在 `<!-- Chart -->` 批註前面的結尾 `</Grid>` 標記。 在結尾 `</Grid>` 標記之前新增下列 XAML。 此 XAML 會加入**InkCanvas**控制項（前面加上您稍早定義為命名空間的**工具**組關鍵字），以及做為控制項標頭的簡單**TextBlock** 。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 儲存**ExpenseDetail. xaml**檔案。

6. 按 F5 以在偵錯工具中執行應用程式。

7. 從清單中選擇員工，然後選擇其中一個可用的費用。 請注意，[費用詳細資料] 頁面包含**InkCanvas**控制項的空間。

    ![僅限筆墨畫布畫筆](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    如果您的裝置支援數位畫筆（例如介面），而且您是在實體機器上執行此實驗室，請繼續並嘗試使用它。 您會看到數位筆跡出現在螢幕上。 不過，如果您沒有具備畫筆功能的裝置，而且嘗試使用滑鼠來簽署，就不會發生任何事。 發生這種情況的原因是，預設只會針對數位畫筆啟用**InkCanvas**控制項。 不過，我們可以變更此行為。

8. 關閉應用程式，然後在**ContosoExpenses**專案的**Views**資料夾底下，按兩下**ExpenseDetail.xaml.cs**檔案。

9. 將下列命名空間宣告新增至類別的最上方：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 找出 `ExpenseDetail()` 的函式。

11. 在 `InitializeComponent()` 方法後面新增下列程式程式碼，並儲存程式碼檔案。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    您可以使用**InkPresenter**物件來自訂預設的筆跡體驗。 這段程式碼會使用**InputDeviceTypes**屬性來啟用滑鼠和手寫筆輸入。

12. 再按一次 F5，以在偵錯工具中重建並執行應用程式。 從清單中選擇員工，然後選擇其中一個可用的費用。

13. 現在試著使用滑鼠在簽章空間中繪製一些東西。 此時，您會看到筆跡出現在螢幕上。

    ![簽名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>接下來的步驟

在本教學課程中，您已成功將 UWP **InkCanvas**控制項新增至 Contoso 費用應用程式。 您現在已準備就緒，[第3部分：使用 XAML 孤島新增 UWP CalendarView 控制項](modernize-wpf-tutorial-3.md)。
