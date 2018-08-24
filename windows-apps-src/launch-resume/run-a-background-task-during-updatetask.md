---
author: TylerMSFT
title: 在更新 UWP app 時執行背景工作
description: 了解如何在更新通用 Windows 平台 (UWP) 市集應用程式時執行背景工作。
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 更新、 背景工作、 updatetask、 背景工作
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2831070"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>在更新 UWP app 時執行背景工作

了解如何撰寫背景工作執行後會更新通用 Windows 平台 (UWP) 市集應用程式。

更新任務背景工作是由作業系統叫用的使用者在裝置已安裝的應用程式以安裝更新之後。 這可讓您執行初始設定工作，例如初始化新的推入通知通道，更新資料庫結構描述等，才能使用者啟動您更新的應用程式的應用程式。

更新任務不同於啟動背景工作使用[ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)觸發程序，因為在此情況下您的應用程式必須至少執行一次以登錄背景工作會藉由**啟用更新之前ServicingComplete**觸發程序。  更新任務不註冊與因此應用程式的永不已經執行，但升級的則仍有觸發其更新任務。

## <a name="step-1-create-the-background-task-class"></a>步驟 1： 建立背景工作類別

為與其他類型的背景工作您實作更新任務背景任務為 Windows 執行階段元件。 若要建立此元件，請遵循**Create 背景工作類別**] 區段中的[建立與註冊程序 （英文） 背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)中的步驟。 步驟包括：

- Windows 執行階段元件將專案新增至您的解決方案。
- 建立從您的應用程式參照至元件。
- 元件中建立公用、 密封類別會實作[**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)。
- 實作[**Run**](https://msdn.microsoft.com/library/windows/apps/br224811)方法，即會呼叫時執行更新工作的必要的進入點。 如果您要從背景工作非同步撥打、[建立與註冊程序 （英文） 背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)會說明如何在您**執行**方法中使用延期。

您不需要使用更新任務登錄此背景工作 （"登錄執行背景工作 」 主題中的小節**建立與註冊程序 （英文） 背景工作**）。 這是因為您不需要新增至您的應用程式註冊之任務的任何程式碼和應用程式都不會有至少執行一次之前要更新登錄背景工作使用更新工作的主要原因。

下列範例程式碼會顯示以 C# 更新任務背景工作類別的基本起點。 背景工作類別本身-和背景工作專案中的所有其他類別-需要**公用**和**密封**。 您的背景工作類別必須衍生自**IBackgroundTask**與下面所顯示之簽章與具有公用**Run()** 方法：

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>步驟 2： 宣告封裝資訊清單中的將背景工作

在 Visual Studio 方案總管中，以滑鼠右鍵按一下**Package.appxmanifest**然後按一下 [**檢視程式碼**來檢視套件資訊清單。 新增下列`<Extensions>`XML 轉換成宣告更新工作：

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

在上述 XML 確定`EntryPoint`屬性設為 namespace.class 您更新工作類別的名稱。 名稱會區分大小寫。

## <a name="step-3-debugtest-your-update-task"></a>步驟 3： 偵錯/測試更新工作

請確定您已您的應用程式部署到電腦以便有更新的某個項目。

在 [背景工作 Run() 方法設定中斷點。

![設定中斷點](images/run-func-breakpoint.png)

接下來，在方案總管中，以滑鼠右鍵按一下您的應用程式的專案 （非背景工作專案） 和 [**屬性**。 在 [應用程式內容] 視窗左側，按一下 [**偵錯**然後選取 [**未啟動，但偵錯時我程式碼**：

![設定偵錯設定](images/do-not-launch-but-debug.png)

下一步]，以確保會觸發 UpdateTask、 增加套件的版本號碼。 在方案總管中，按兩下以開啟 [封裝設計工具中，您的應用程式**Package.appxmanifest**檔案並再更新的**組建**編號：

![更新版本](images/bump-version.png)

現在，在 Visual Studio 2017 當您按 F5、 將更新您的應用程式，系統將會啟用在背景中的您 UpdateTask 元件。 偵錯程式將會自動附加至背景處理程序。 取得叫用您中斷點及您可透過您更新程式碼邏輯步驟。

當背景工作完成後時，您可在相同的偵錯工作階段啟動前景應用程式從 Windows start 功能表。 偵錯程式將會自動重新附加至您的前景程序，這次及您可透過您的應用程式邏輯步驟。

> [!NOTE]
> Visual Studio 2015 使用者： 上面的步驟適用於 Visual Studio 2017。 如果您使用 Visual Studio 2015，您可以使用觸發程序及測試 UpdateTask 除了 Visual Studio 不會附加至其的相同技術。 VS 2015 的替代程序是安裝程式將 UpdateTask 設為其進入點， [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app)並觸發直接從前景應用程式執行。

## <a name="see-also"></a>也請參閱

[建立及註冊跨處理序的背景工作](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
