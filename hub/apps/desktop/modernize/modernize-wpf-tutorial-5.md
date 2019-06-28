---
description: 本教學課程會示範如何新增 UWP XAML 使用者介面，建立 MSIX 套件，以及您的 WPF 應用程式中納入其他現代的元件。
title: 封裝和部署使用 MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10、 uwp、 windows form、 wpf、 xaml 群島
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420096"
---
# <a name="part-5-package-and-deploy-with-msix"></a>第 5 部分：封裝和部署使用 MSIX

這是示範如何將範例 WPF 傳統型應用程式現代化名為 Contoso 費用的教學課程的最後一個部分。 如需教學課程、 必要條件和指示，下載範例應用程式的概觀，請參閱[教學課程：將 WPF 應用程式現代化](modernize-wpf-tutorial.md)。 本文假設您已完成[第 4 部分](modernize-wpf-tutorial-4.md)。

在 [第 4 部分](modernize-wpf-tutorial-4.md)您已了解一些 WinRT Api，包括通知 API，需要封裝識別碼，才能在應用程式中使用。 您可以取得封裝識別碼的封裝使用的 Contoso 費用[MSIX](https://docs.microsoft.com/windows/msix)，來封裝及部署 Windows 應用程式的 Windows 10 中推出的封裝格式。 MSIX 優點資料表針對開發人員和 IT 專業人員，包括：

- 最佳化網路使用量和儲存體空間。
- 清除完成解除安裝，因為應用程式執行所在的輕量級容器。 沒有登錄機碼和暫存檔案會保留在系統上。
- 減少從應用程式更新和自訂的作業系統更新。
- 簡化了安裝、 更新及解除安裝程序。 

在本教學課程的這個部分中，您將了解如何將封裝 MSIX 封裝中的 Contoso 費用報銷應用程式。

## <a name="package-the-application"></a>封裝應用程式

Visual Studio 2019 提供簡單的方法來封裝傳統型應用程式使用 Windows 應用程式封裝專案。 

1. 在 **方案總管 中**，以滑鼠右鍵按一下**ContosoExpenses**方案，然後選擇 **新增-> 新增專案**。

    ![加入新的專案](images/wpf-modernize-tutorial/AddNewProject.png)

3. 在 [**加入新的專案**] 對話方塊中，搜尋`packaging`，選擇**Windows 應用程式封裝專案**中的專案範本C#類別目錄，然後按一下 [**下一步]** .

    ![Windows 應用程式封裝專案](images/wpf-modernize-tutorial/WAP.png)

4. 將新專案命名`ContosoExpenses.Package`，按一下 **建立**。

5. 選取**Windows 10 版本 (10.0; 1903，建置 18362）** 兩者**目標版本**並**最低版本**，按一下  **確定**。

    **ContosoExpenses.Package**專案加入至**ContosoExpenses**解決方案。 此專案包含[封裝資訊清單](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，其中描述應用程式，以及一些項目，例如 [程式] 功能表中的圖示和 [開始] 畫面中的圖格所使用的預設資產。 不過，不同於 UWP 專案時，封裝專案不包含程式碼。 其目的是要將現有的桌面應用程式封裝。

6. 在  **ContosoExpenses.Package**專案中，以滑鼠右鍵按一下**應用程式**節點，然後選擇 **將參考加入**。 此節點會指定在您的方案中的哪些應用程式將會包含在封裝中。

7. 在專案清單中，選取**ContosoExpenses.Core**然後按一下**確定**。

8. 依序展開**應用程式**節點，並確認**ContosoExpense.Core**專案參考，並以粗體反白顯示。 這表示，它將用於做為起點的封裝。

9. 以滑鼠右鍵按一下**ContosoExpenses.Package**專案，然後選擇**設定為啟始專案**。

10. 按下**F5**啟動偵錯工具中已封裝的應用程式。

此時，您可以注意到一些變更，表示應用程式現在以執行封裝：

- 在工作列中，或在 [開始] 功能表中的圖示現在是預設資產中包含每個**Windows 應用程式封裝專案**。
- 如果您以滑鼠右鍵按一下**ContosoExpense.Package**應用程式列在 [開始] 功能表中，您會注意到選項通常保留給應用程式從 Microsoft Store 中，例如下載**應用程式設定**，**速率和檢閱**並**共用**。

    ![在 [開始] 功能表中的 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 如果您想要解除安裝應用程式，您可以以滑鼠右鍵按一下**ContosoExpense.Package**在 [開始] 功能表，然後選擇**解除安裝**。 應用程式將會立即移除，而不將任何剩餘留在系統上。

## <a name="test-the-notification"></a>測試通知

既然您已預先封裝 MSIX Contoso 費用報銷應用程式，您可以測試無法使用的結尾處的通知案例[第 4 部分](modernize-wpf-tutorial-4.md)。

1. 在 Contoso 費用報銷應用程式中，選擇員工從清單中，然後按一下**加入新的支出** 按鈕。 
2. 完成表單，然後按下的所有欄位**儲存**。
3. 確認您看到的 OS 通知。

![快顯通知](images/wpf-modernize-tutorial/ToastNotification.png)
