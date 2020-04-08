---
description: 此教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的 UWP 應用程式。
title: 使用 XAML Islands 新增 UWP InkCanvas 控制項
ms.topic: article
ms.date: 01/24/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 945cc2f1cf225c194e5820990bdbeda584069e4c
ms.sourcegitcommit: 1455e12a50f98823bfa3730c1d90337b1983b711
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76814038"
---
# <a name="part-2-add-a-uwp-inkcanvas-control-using-xaml-islands"></a>第 2 部分：使用 XAML Islands 新增 UWP InkCanvas 控制項

這是教學課程的第二部分，示範如何將名為 Contoso Expenses 的範例 WPF 傳統型應用程式現代化。 如需教學課程概觀、先決條件和下載範例應用程式的指示，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 此文章假設您已經完成[第 1 部分](modernize-wpf-tutorial-1.md)。

在此教學課程的虛構案例中，Contoso 開發小組想要在 Contoso Expenses 應用程式中新增對數位簽章的支援。 UWP **InkCanvas** 控制項對此案例來說是很棒的選擇，因其支援數位筆跡和 AI 支援的功能，例如，辨識文字和圖形的能力。 若要這麼做，您將使用 Windows 社群工具組中提供的 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) \(英文\) 包裝的 UWP 控制項。 此控制項會包裝 UWP **InkCanvas** 控制項的介面和功能，以便在 WPF 應用程式中使用。 如需包裝 UWP 控制項的詳細資訊，請參閱[在傳統型應用程式中裝載 UWP XAML 控制項 (XAML Islands)](xaml-islands.md)。

> [!NOTE]
> 在此教學課程中，WPF 應用程式只會裝載來自 Windows SDK 的第一方 UWP 控制項。 因此，此教學課程會省略定義 [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) 類別執行個體的步驟，如[這裡](host-standard-control-with-xaml-islands.md#required-components)所述。

## <a name="configure-the-project-to-use-xaml-islands"></a>將專案設定為使用 XAML Islands

您必須先將專案設定為支援 UWP XAML Islands，才能將 **InkCanvas** 控制項新增至 Contoso Expenses 應用程式。

1. 在 Visual Studio 2019 中，以滑鼠右鍵按一下 [方案總管]  中的 [ContosoExpenses.Core]  專案，然後選擇 [管理 NuGet 套件]  。

    ![Visual Studio 中的 [管理 NuGet 套件] 功能表](images/wpf-modernize-tutorial//ManageNuGetPackages.png)

2. 在 [NuGet 套件管理員]  視窗中，按一下 [瀏覽]  。 搜尋 `Microsoft.Toolkit.Wpf.UI.Controls` 套件，並安裝 6.0.0 版或更新版本。

    > [!NOTE]
    > 此套件包含在 WPF 應用程式中裝載 UWP XAML Islands 的所有必要基礎結構，包括 **InkCanvas** 包裝的 UWP 控制項。 Windows Forms 應用程式可以使用名為 `Microsoft.Toolkit.Forms.UI.Controls` 的類似套件。

3. 以滑鼠右鍵按一下 [方案總管]  中的 [ContosoExpenses.Core]  專案，然後選擇 [新增] -> [新增項目]  。

4. 選取 [應用程式資訊清單檔案]  、將其命名為 **app.manifest**，然後按一下 [新增]  。 如需應用程式資訊清單的詳細資訊，請參閱[這篇文章](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) \(英文\)。

5. 在資訊清單檔中，將下列適用於 Windows 10 的 `<supportedOS>` 元素取消註解。

    ```xml
    <!-- Windows 10 -->
    <supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />
    ```

6. 在資訊清單檔中，找出下列已加上註解的 `<application>` 元素。

    ```xml
    <!--
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
        <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
      </windowsSettings>
    </application>
    -->
    ```

7. 刪除此區段，並使用下列 XML 來取代。 這會將應用程式設定為 DPI 感知，且更妥善地處理 Windows 10 支援的不同調整係數。

    ```xml
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
      <windowsSettings>
          <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true/PM</dpiAware>
          <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2, PerMonitor</dpiAwareness>
      </windowsSettings>
    </application>
    ```

8. 儲存並關閉 `app.manifest` 檔案。

9. 在 [方案總管]  中，以滑鼠右鍵按一下 [ContosoExpenses.Core]  專案，然後選擇 [屬性]  。

10. 在 [應用程式]  索引標籤的 [資源]  區段中，確定已將 [資訊清單]  下拉式清單設定為 [app.manifest]  。

    ![.NET Core 應用程式資訊清單](images/wpf-modernize-tutorial/NetCoreAppManifest.png)

11. 將變更儲存至專案屬性。

## <a name="add-an-inkcanvas-control-to-the-app"></a>將 InkCanvas 控制項新增至應用程式

既然您已將專案設定為使用 UWP XAML Islands，現在就準備好將 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) \(英文\) 包裝的 UWP 控制項新增至應用程式。

1. 在 [方案總管]  中，展開 [ContosoExpenses.Core]  專案的 [檢視]  資料夾，然後按兩下 **ExpenseDetail.xaml** 檔案。

2. 在靠近 XAML 檔案頂端的 **Window** 元素中，新增下列屬性。 這會參考 [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) \(英文\) 包裝之 UWP 控制項的 XAML 命名空間。

    ```xml
    xmlns:toolkit="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    新增此屬性之後，**Window** 元素現在看起來應該像這樣。

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

4. 在 **ExpenseDetail.xaml** 檔案中，找出位於 `<!-- Chart -->` 註解正前方的結尾 `</Grid>` 標記。 在結尾 `</Grid>` 標記正前方新增下列 XAML。 這個 XAML 會將 **InkCanvas** 控制項 (前面加上您稍早定義為命名空間的 **toolkit** 關鍵字) 和簡單的 **TextBlock** 作為控制項的標頭。

    ```xml
    <TextBlock Text="Signature:" FontSize="16" FontWeight="Bold" Grid.Row="5" />

    <toolkit:InkCanvas x:Name="Signature" Grid.Row="6" />
    ```

5. 儲存 **ExpenseDetail.xaml** 檔案。

6. 按 F5，以在偵錯工具中執行應用程式。

7. 從清單中選擇員工，然後選擇其中一個可用的費用。 請注意，[費用詳細資料] 頁面包含適用於 **InkCanvas** 控制項的空間。

    ![僅限筆跡畫布手寫筆](images/wpf-modernize-tutorial/InkCanvasPenOnly.png)

    如果您的裝置支援數位手寫筆 (例如 Surface)，而且您在實體機器上執行此實驗，請繼續並嘗試使用它。 您將會看到數位筆跡出現在螢幕上。 不過，如果您沒有具備手寫筆功能的裝置，並嘗試使用滑鼠來簽署，則不會發生任何作用。 發生這種情況的原因是，預設只會針對數位手寫筆啟用 **InkCanvas** 控制項。 不過，我們可以變更此行為。

8. 關閉應用程式，並且在 **ContosoExpenses.Core** 專案的 [檢視]  資料夾下方，按兩下 **ExpenseDetail.xaml.cs** 檔案。

9. 在類別頂端新增下列命名空間宣告：

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

10. 找出 `ExpenseDetail()` 建構函式。

11. 在 `InitializeComponent()` 方法之後新增下列程式碼行，並儲存程式碼檔案。

    ```csharp
    Signature.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    您可以使用 **InkPresenter** 物件來自訂預設筆跡體驗。 此程式碼會使用 **InputDeviceTypes** 屬性來啟用滑鼠和手寫筆輸入。

12. 再次按 F5，以在偵錯工具中重建並執行應用程式。 從清單中選擇員工，然後選擇其中一個可用的費用。

13. 現在試著使用滑鼠，在簽章空間中繪製一些內容。 此時，您將會看到筆跡出現在螢幕上。

    ![簽名](images/wpf-modernize-tutorial/Signature.png)

## <a name="next-steps"></a>接下來的步驟

目前在此教學課程中，您已成功將 UWP **InkCanvas** 控制項新增至 Contoso Expenses 應用程式。 您現在已準備好開始進行[第 3 部分：使用 XAML Islands 新增 UWP CalendarView 控制項](modernize-wpf-tutorial-3.md)。
