---
Description: 散發已封裝的桌面應用程式 (桌面橋接器)
title: 將已封裝的桌面應用程式發佈至 Microsoft Store, 或將其側載至一或多個裝置。
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 597a283fd28b571ed968255312059c7049f3f700
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682567"
---
# <a name="distribute-your-packaged-desktop-app"></a>散發已封裝的桌面應用程式

如果您決定將[桌面應用程式封裝在 MSIX 套件中](/windows/msix/desktop/desktop-to-uwp-root), 您可以將封裝的應用程式發佈至 Microsoft Store, 或將它側載到一或多個裝置上。

> [!NOTE]
> 您是否有計劃可以將使用者轉換成已封裝的應用程式？ 在您散布您的應用程式之前，請參閱本文中[轉換使用者至您的已封裝應用程式](#transition-users)一節，以取得一些靈感。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>將您的應用程式發佈至 Microsoft Store 以散發

[Microsoft Store](https://www.microsoft.com/store/apps)是讓客戶取得您應用程式的一種簡便方式。

將您的應用程式發佈到 Microsoft Store, 以接觸到最廣泛的物件。 此外, 組織的客戶也可以取得您的應用程式, 透過商務用[Microsoft Store](https://businessstore.microsoft.com/store)在內部散發給其組織。

如果您打算發佈至 Microsoft Store，提交程序中您會被要求詢問一些額外的問題。 這是因為您的封裝資訊清單宣告名為 **runFullTrust** 的受限功能，以及我們需要核准您的應用程式使用該功能。 您可以在這裡閱讀更多關於此需求的資訊:[限制的功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

您不需要先簽署應用程式, 就能將它提交至存放區。

>[!IMPORTANT]
> 如果您打算將應用程式發佈至 Microsoft Store, 請確定您的應用程式可在執行 Windows 10 S 的裝置上正確運作。這是存放區需求。 請參閱[針對 Windows 10 S 測試您的 Windows 應用程式](/windows/msix/desktop/desktop-to-uwp-test-windows-s)。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>散發您的應用程式, 而不將它放到 Microsoft Store

如果您想要散發您的應用程式, 而不使用存放區, 您可以手動將應用程式散發至一或多個裝置。

在您想要進一步控制散布體驗，或是您不想要參與 Microsoft Store 的認證程序時，這將會是一個合理的作法。

若要將您的應用程式散發到其他裝置, 而不將它放在存放區中, 您必須取得憑證、使用該憑證簽署應用程式, 然後將應用程式側載至這些裝置。

您可以[建立憑證](/windows/msix/package/create-certificate-package-signing)或從受歡迎的供應商，例如 [Verisign](https://www.verisign.com/) 取得憑證。

如果您打算將應用程式散發至執行 Windows 10 S 的裝置, 您的應用程式必須由 Microsoft Store 簽署, 因此您必須先完成存放區提交程式, 才能將應用程式散發到這些裝置。

若您已建立了一個憑證，您必須先將該憑證安裝到每個執行您應用程式裝置上的 **「受信任的根」** 或 **「受信任的人」** 憑證存放區。 若您是從受歡迎的供應商取得憑證，則除了將您的應用程式安裝到其他系統之外，您不需要安裝任何東西。  

> [!IMPORTANT]
> 請確定您憑證的發行者名稱符合您應用程式的發行者名稱。

若要使用憑證簽署您的應用程式, 請參閱[使用 SignTool 簽署應用程式封裝](/windows/msix/package/sign-app-package-using-signtool)。

若要在其他裝置上側載您的應用程式, 請參閱[在 Windows 10 中側載 LOB 應用](/windows/application-management/sideload-apps-in-windows-10)程式。

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>將使用者轉換至您已封裝的應用程式。

在您散發您的 App 之前，建議您考慮將幾個延伸模組新增至您的封裝資訊清單中，以協助使用者習慣使用您的封裝應用程式。 以下是幾個您可以嘗試的方法。

* 將現有的開始畫面磚和工作列按鈕指向您已封裝的應用程式。
* 將封裝的應用程式與一組檔案類型產生關聯。
* 根據預設, 讓封裝的應用程式開啟特定類型的檔案。

如需延伸功能的完整清單及如何使用其指南，請參閱[將使用者轉換至您的應用程式](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外, 請考慮將程式碼新增至已封裝的應用程式, 以完成這些工作:

* 將與您的桌面應用程式相關聯的使用者資料移轉至已封裝應用程式的適當資料夾位置。
* 提供使用者解除安裝您應用程式傳統型版本的選項。

讓我們談談每一項工作。 我們會先從使用者資料移轉開始。

### <a name="migrate-user-data"></a>移轉使用者資料

如果您要加入可遷移使用者資料的程式碼, 最好只有在第一次啟動應用程式時才執行該程式碼。 在您移轉使用者資料之前，請先向使用者顯是一個對話方塊，解釋發生了什麼事情、為何建議這樣做，以及對於他們現有的資料會造成何種影響。

以下是如何在 .NET 型已封裝應用程式中完成這樣工作的方法。

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>解除安裝您應用程式的傳統型版本

最好不要先要求他們提供許可權, 就不要卸載「使用者」桌面應用程式。 您可以藉由顯示對話方塊來徵求同意。 使用者可以選擇不解除安裝您應用程式的傳統型版本。 如果發生這種情況, 您必須決定是要封鎖桌面應用程式的使用, 還是支援這兩個應用程式的並存使用。

以下是如何在 .NET 型已封裝應用程式中完成這樣工作的方法。

若要檢視此程式碼片段的完整內容，請參閱此範例[具有轉換/移轉/解除安裝的 WPF 圖片檢視器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)中的 **MainWindow.cs** 檔案。

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>後續步驟

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

如果您在將應用程式發佈至 Microsoft Store 時遇到問題，這篇[部落格文章](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)包含一些有用的秘訣。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
