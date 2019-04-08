---
title: 在更新 UWP 應用程式時執行背景工作
description: 了解如何在更新通用 Windows 平台 (UWP) 市集應用程式時執行背景工作。
ms.date: 04/21/2017
ms.topic: article
keywords: windows 10、 uwp、 更新、 背景工作、 updatetask、 背景工作
ms.localizationpriority: medium
ms.openlocfilehash: 8cd7d4494340d1c5e617361f2e3d750b35ebabb9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603523"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>在更新 UWP 應用程式時執行背景工作

了解如何在更新通用 Windows 平台 (UWP) 市集應用程式之後寫入背景工作。

「更新工作」背景工作是在使用者安裝已安裝於裝置之 App 的更新之後，由作業系統叫用。 這可讓 App 在使用者啟動更新的 App 之前執行初始化工作，例如初始化新的推播通知通道、更新資料庫結構描述等工作。

「更新工作」不同於使用 [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) 觸發程序啟動背景工作，因為在此情況下，您的 App 就必須在更新之前執行至少一次，才能註冊將會由 **ServicingComplete** 觸發程序啟動的背景工作。  「更新工作」未註冊，也因此從未執行 App，若不是升級，仍將觸發其更新工作。

## <a name="step-1-create-the-background-task-class"></a>步驟 1：建立背景工作類別

與其他類型的背景工作一樣，您會將「更新工作」背景工作實作為 Windows 執行階段元件。 若要建立此元件，請依照[建立和註冊跨處理序背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)**「建立背景工作類別」** 一節中的步驟進行。 這些步驟包括：

- 將 Windows 執行階段元件專案至您的方案。
- 從 App 建立元件的參考。
- 在實作 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 的元件中建立公用密封類別。
- 實作 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法，這是在執行「更新工作」時呼叫的必要進入點。 如果您要從背景工作進行非同步呼叫，[建立和註冊跨處理序背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) 會說明如何在 **Run** 方法中使用延遲。

您不需要註冊此背景工作 (**「建立和註冊跨處理序背景工作」** 主題中的「註冊要執行的背景工作」一節)，就能使用「更新工作」。 這就是使用「更新工作」的主要原因，因為您不需要新增任何程式碼至 App 來註冊背景工作，而且 App 不必在更新之前至少執行一次，也能註冊背景工作。

下列範例程式碼使用 C# 示範「更新工作」背景工作類別的基本起點： 背景工作類別本身 (及背景工作專案中的所有其他類別) 必須是 **public** 和 **sealed**。 背景工作類別必須衍生自 **IBackgroundTask**，並且具有如下所示簽章的公用 **Run()** 方法：

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>步驟 2：宣告您在套件資訊清單中的背景工作

在 Visual Studio 方案總管中，以滑鼠右鍵按一下 **Package.appxmanifest**，然後按一下**\[檢視程式碼\]** 以檢視封裝資訊清單。 新增下列 `<Extensions>` XML 來宣告更新工作：

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

在上述 XML 中，確定 `EntryPoint` 屬性已設定為更新工作的 namespace.class 名稱。 此名稱區分大小寫。

## <a name="step-3-debugtest-your-update-task"></a>步驟 3：偵錯/測試您的更新工作

確定您已將 App 部署到電腦，讓其中有要更新的項目。

設定背景工作之 Run() 方法的中斷點。

![設定中斷點](images/run-func-breakpoint.png)

接下來，在方案總管中，以滑鼠右鍵按一下 App 的專案 (而非背景工作專案)，然後按一下**\[屬性\]**。 在應用程式的屬性視窗中，按一下左側的 **\[偵錯\]**，然後選取 **\[不啟動，但在我的程式碼啟動時進行偵錯\]**：

![設定偵錯設定](images/do-not-launch-but-debug.png)

接下來，為了確保 UpdateTask 已觸發，請增加封裝的版本號碼。 在 [方案總管] 中，按兩下 App 的 **Package.appxmanifest** 檔案開啟封裝設計工具，然後更新**組建**編號：

![更新版本](images/bump-version.png)

目前在 Visual Studio 2017 中按下 F5 時，將會更新您的 App，而且系統也會在背景中啟動 UpdateTask 元件。 偵錯工具會自動連結到背景處理程序。 觸及中斷點後，您即可逐步檢查更新程式碼邏輯。

背景工作完成時，就可以在相同偵錯工作階段中，從 Windows [開始] 功能表啟動前景 App。 偵錯工具會再次自動連結，但這次是連結到前景處理程序，您可以逐步檢查 App 的邏輯。

> [!NOTE]
> Visual Studio 2015 使用者：上述的步驟適用於 Visual Studio 2017。 如果使用的是 Visual Studio 2015，除了 Visual Studio 不會連結到 UpdateTask 之外，您都可以使用相同的技術來觸發和測試它。 VS 2015 中的替代程序是設定 [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app)，這會將 UpdateTask 設定為其進入點，並直接從前景 App 觸發執行。

## <a name="see-also"></a>請參閱

[建立及註冊跨處理序的背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
