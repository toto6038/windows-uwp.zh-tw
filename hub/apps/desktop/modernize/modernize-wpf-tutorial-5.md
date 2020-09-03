---
description: 本教學課程示範如何新增 UWP XAML 使用者介面、建立 MSIX 套件，以及將其他新式元件併入您的 UWP 應用程式。
title: 使用 MSIX 封裝和部署
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml islands
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 18b89caa0de947d2b95b46c3deb11378912b6012
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161422"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部分：使用 MSIX 封裝和部署

這是教學課程的最後一個部分，示範如何讓名為 Contoso Expenses 的範例 WPF 傳統型應用程式現代化。 如需教學課程概觀、必要條件和下載範例應用程式的指示，請參閱[教學課程：讓 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已經完成[第 4 部分](modernize-wpf-tutorial-4.md)。

在[第 4 部分](modernize-wpf-tutorial-4.md)中，您已了解有些 WinRT API (包括通知 API) 必須先具備套件識別資料，才能在應用程式中使用。 您可以使用 [MSIX](/windows/msix) (Windows 10 引進的封裝格式，用於封裝和部署 Windows 應用程式) 來封裝 Contoso Expenses，藉此取得套件識別資料。 MSIX 為開發人員和 IT 專業人員提供的優點包括：

- 最佳化的網路使用量和儲存空間。
- 完全乾淨的解除安裝，多虧了應用程式執行所在的輕量型容器。 系統上不會保留任何登錄機碼和暫存檔案。
- 將 OS 更新與應用程式更新和自訂項目分離。
- 簡化安裝、更新和解除安裝程序。

在教學課程的這個部分中，您將了解如何在 MSIX 套件中封裝 Contoso Expenses 應用程式。

## <a name="package-the-application"></a>封裝應用程式

Visual Studio 2019 提供簡單的方法，讓您使用 Windows 應用程式封裝專案來封裝傳統型應用程式。 

1. 在 [方案總管]  中，以滑鼠右鍵按一下 **ContosoExpenses** 方案，然後選擇 [新增] -> [新專案]  。

    ![新增專案](images/wpf-modernize-tutorial/AddNewProject.png)

3. 在 [新增專案]  對話方塊中，搜尋 `packaging`，在 C# 類別中選擇 [Windows 應用程式封裝專案]  專案範本，然後按 [下一步]  。

    ![Windows 應用程式封裝專案](images/wpf-modernize-tutorial/WAP.png)

4. 將新專案命名為 `ContosoExpenses.Package`，然後按一下 [建立]  。

5. 同時對 [目標版本]  和 [最低版本]  選取 [Windows 10 版本1903 (10.0；組建 18362)]  ，然後按一下 [確定]  。

    **ContosoExpenses.Package** 專案會新增至 **ContosoExpenses** 方案。 此專案包含[套件資訊清單](/uwp/schemas/appxpackage/uapmanifestschema/schema-root) (用以描述應用程式)，以及一些用於項目的預設資產，例如 [程式] 功能表中的圖示和 [開始] 畫面中的磚。 不過，與 UWP 專案不同，封裝專案不包含程式碼。 其目的是要封裝現有的傳統型應用程式。

6. 在 **ContosoExpenses.Package** 專案中，以滑鼠右鍵按一下 [應用程式]  節點，然後選擇 [新增參考]  。 這個節點會指定套件中將會包含方案中的哪些應用程式。

6. 在專案清單中，選取 [ContosoExpenses.Core]  ，然後按一下 [確定]  。

7. 展開 [應用程式]  節點，並確認已參考 [ContosoExpense.Core]  專案，並以粗體醒目提示。 這表示其會作為套件的起點使用。

8. 以滑鼠右鍵按一下 [ContosoExpenses.Package]  專案，然後選擇 [設定為啟動專案]  。

9. 按 **F5** 以在偵錯工具中啟動封裝的應用程式。

此時，您可能會注意到一些變更，這表示應用程式目前正以封裝的形式執行：

- 工作列或 [開始] 功能表中的圖示現在是每個 [Windows 應用程式封裝專案]  中包含的預設資產。
- 如果您以滑鼠右鍵按一下 [開始] 功能表中所列的 [ContosoExpense.Package]  應用程式，您會注意到通常保留給從 Microsoft Store 下載之應用程式的選項，例如 [應用程式設定]  、[評分並評論]  和 [共用]  。

    ![開始功能表中的 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 如果您想要將應用程式解除安裝，可以用滑鼠右鍵按一下 [開始] 功能表中的 [ContosoExpense.Package]  ，然後選擇 [解除安裝]  。 應用程式將會立即移除，而不會在系統上留下任何殘餘物。

## <a name="test-the-notification"></a>測試通知

您現在已使用 MSIX 封裝 Contoso Expenses 應用程式，即可測試不會在[第 4 部分](modernize-wpf-tutorial-4.md)結束時並未運作的通知案例。

1. 在 Contoso Expenses 應用程式中，從清單中選擇員工，然後按一下 [新增費用]  按鈕。
2. 完成表單中的所有欄位，然後按 [儲存]  。
3. 確認您看到 OS 通知。

![快顯通知](images/wpf-modernize-tutorial/ToastNotification.png)