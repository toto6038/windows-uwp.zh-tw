---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件, 以及將其他現代化元件併入您的 WPF 應用程式中。
title: 使用 MSIX 封裝和部署
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 961157bc3d3429b56d3da24a46d71cbb5b84e7a3
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729493"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部分：使用 MSIX 封裝和部署

這是教學課程的最後一個部分, 示範如何將名為 Contoso 費用的範例 WPF 桌面應用程式現代化。 如需教學課程、必要條件和下載範例應用程式指示的總覽, 請參閱[教學課程:將 WPF 應用程式](modernize-wpf-tutorial.md)現代化。 本文假設您已完成[第4部分](modernize-wpf-tutorial-4.md)。

在[第4部分](modernize-wpf-tutorial-4.md)中, 您已瞭解一些 WinRT api (包括通知 API) 需要套件身分識別, 才能在應用程式中使用。 您可以使用[MSIX](https://docs.microsoft.com/windows/msix)(Windows 10 中引進的封裝格式) 封裝和部署 windows 應用程式, 藉以取得套件身分識別。 MSIX 為開發人員和 IT 專業人員提供資料表的優點, 包括:

- 優化網路使用量和儲存空間。
- 請完成全新卸載, 因為執行應用程式的是輕量容器。 系統上不會保留任何登錄機碼和暫存檔案。
- 將 OS 更新與應用程式更新和自訂分離。
- 簡化安裝、更新和卸載程式。 

在本教學課程的這個部分中, 您將瞭解如何在 MSIX 套件中封裝 Contoso 費用應用程式。

## <a name="package-the-application"></a>封裝應用程式

Visual Studio 2019 提供簡單的方法, 讓您使用 Windows 應用程式封裝專案來封裝桌面應用程式。 

1. 在**方案總管**中, 以滑鼠右鍵按一下**ContosoExpenses**方案, 然後選擇 [**加入 > 新增專案**]。

    ![加入新專案](images/wpf-modernize-tutorial/AddNewProject.png)

3. 在 [**加入新的專案**] 對話方塊中, 搜尋`packaging`, 在C#類別中選擇 [ **Windows 應用程式封裝專案**] 專案範本, 然後按 **[下一步]** 。

    ![Windows 應用程式封裝專案](images/wpf-modernize-tutorial/WAP.png)

4. 將新專案`ContosoExpenses.Package`命名為, 然後按一下 [**建立**]。

5. 選取**Windows 10 版本 1903 (10.0;組建 18362)** , 適用于**目標版本**和**最低版本**, 然後按一下 **[確定]** 。

    **ContosoExpenses**專案會新增至**ContosoExpenses**方案。 此專案包含描述應用程式的[套件資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), 以及一些用於專案的預設資產, 例如 [程式] 功能表中的圖示和 [開始] 畫面中的磚。 不過, 不同于 UWP 專案, 封裝專案不包含程式碼。 其目的是要封裝現有的桌面應用程式。

6. 在 [ **ContosoExpenses** ] 專案中, 以滑鼠右鍵按一下 [**應用程式**] 節點, 然後選擇 [**加入參考**]。 這個節點會指定解決方案中將包含哪些應用程式。

7. 在專案清單中, 選取 [ **ContosoExpenses** ], 然後按一下 **[確定]** 。

8. 展開 [**應用程式**] 節點, 並確認已參考 [ **ContosoExpense** ] 專案, 並以粗體反白顯示。 這表示它會當做封裝的起點使用。

9. 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案, 然後選擇 [**設定為啟始專案**]。

10. 在方案總管中, 以滑鼠右鍵按一下 [ **ContosoExpenses** ] 專案節點, 然後選取 [**編輯專案檔案**]。

11. 從檔案中找出 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 元素。

12. 以下列 XML 取代此元素。

    ``` xml
    <ItemGroup>
        <SDKReference Include="Microsoft.VCLibs,Version=14.0">
        <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
        <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
        <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
        <Implicit>true</Implicit>
        </SDKReference>
    </ItemGroup>
    <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
    <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
        <ItemGroup>
        <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
        <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
        <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
            <SourceProject>
            </SourceProject>
        </_FilteredNonWapProjProjectOutput>
        </ItemGroup>
    </Target>
    ```

13. 儲存專案檔案並將它關閉。

14. 按**F5**以在偵錯工具中啟動已封裝的應用程式。

此時, 您可能會注意到一些變更, 指出應用程式現在正在以封裝方式執行:

- 工作列或 [開始] 功能表中的圖示現在是包含在每個**Windows 應用程式封裝專案**中的預設資產。
- 如果您以滑鼠右鍵按一下 [開始] 功能表中所列的 [ **ContosoExpense** ] 應用程式, 您會注意到通常會保留給從 Microsoft Store 下載之應用程式的選項, 例如**應用程式設定**、**速率和評論**和**共用**.

    ![開始功能表中的 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 如果您想要卸載應用程式, 可以在 [開始] 功能表中的 [ **ContosoExpense** ] 上按一下滑鼠右鍵, 然後選擇 [**卸載**]。 應用程式將會立即移除, 而不會在系統上留下任何剩餘的。

## <a name="test-the-notification"></a>測試通知

現在, 您已使用 MSIX 封裝 Contoso 費用應用程式, 您可以測試在[第4部分](modernize-wpf-tutorial-4.md)結束時無法運作的通知案例。

1. 在 [Contoso 費用] 應用程式中, 從清單中選擇員工, 然後按一下 [新增**費用**] 按鈕。 
2. 完成表單中的所有欄位, 然後按 [**儲存**]。
3. 確認您看到 OS 通知。

![快顯通知](images/wpf-modernize-tutorial/ToastNotification.png)
