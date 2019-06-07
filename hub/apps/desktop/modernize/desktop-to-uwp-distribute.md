---
Description: 發佈封裝的桌面應用程式 （傳統型橋接器）
title: 您封裝傳統型應用程式發行至 Microsoft Store 或側載到一或多個裝置。
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2e1aa424fe038a50a5e29364c7f8246e324dc07c
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215004"
---
# <a name="distribute-your-packaged-desktop-app"></a>將傳統型應用程式封裝

如果您決定[封裝傳統型應用程式中的 MSIX 套件](/windows/msix/desktop/desktop-to-uwp-root)，您可以發行至 Microsoft Store 或側載應用程式封裝到一個或多個裝置。

> [!NOTE]
> 您是否有您可能會如何轉換使用者已封裝應用程式的計劃？ 在您散布您的應用程式之前，請參閱本文中[轉換使用者至您的已封裝應用程式](#transition-users)一節，以取得一些靈感。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>藉由將其發佈至 Microsoft Store 散發您的應用程式

[Microsoft Store](https://www.microsoft.com/store/apps)是讓客戶取得您應用程式的一種簡便方式。

應用程式發佈至 Microsoft Store 連線到廣泛的觀眾。 此外，組織的客戶可以取得您的應用程式散發給組織的內部[Microsoft Store for Business](https://www.microsoft.com/business-store)。

如果您打算發佈至 Microsoft Store，提交程序中您會被要求詢問一些額外的問題。 這是因為您的封裝資訊清單宣告名為 **runFullTrust** 的受限功能，以及我們需要核准您的應用程式使用該功能。 您可以深入了解這項需求：[限制功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

您不需要登入您的應用程式，再提交至市集。

>[!IMPORTANT]
> 如果您打算在應用程式發佈至 Microsoft Store，請確定您的應用程式可在執行 Windows 10 S 的裝置上正確運作這是存放區需求。 請參閱[針對 Windows 10 S 測試您的 Windows 應用程式](/windows/msix/desktop/desktop-to-uwp-test-windows-s)。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>不需將它放到 Microsoft Store 散發您的應用程式

如果您想而不是散發您的應用程式，而不使用存放區，您可以手動散發至一或多個裝置的應用程式。

在您想要進一步控制散布體驗，或是您不想要參與 Microsoft Store 的認證程序時，這將會是一個合理的作法。

若要散發到其他裝置的應用程式不需將它放在存放區中，您必須取得憑證，簽署應用程式，使用該憑證，然後側載您的應用程式至那些裝置上。

您可以[建立憑證](/windows/uwp/packaging/create-certificate-package-signing)或從受歡迎的供應商，例如 [Verisign](https://www.verisign.com/) 取得憑證。

如果您打算發佈您的應用程式，到執行 Windows 10 S 的裝置，您的應用程式必須經過簽署的 Microsoft Store 因此您必須瀏覽市集提交程序，您可以散發到那些裝置上的應用程式之前。

若您已建立了一個憑證，您必須先將該憑證安裝到每個執行您應用程式裝置上的 **「受信任的根」** 或 **「受信任的人」** 憑證存放區。 若您是從受歡迎的供應商取得憑證，則除了將您的應用程式安裝到其他系統之外，您不需要安裝任何東西。  

> [!IMPORTANT]
> 請確定您憑證的發行者名稱符合您應用程式的發行者名稱。

若要使用的憑證來簽署應用程式，請參閱[登入使用 SignTool 應用程式封裝](/windows/uwp/packaging/sign-app-package-using-signtool)。

若要側載您的應用程式，到其他裝置，請參閱[Windows 10 中的 側載 LOB 應用程式](/windows/application-management/sideload-apps-in-windows-10)。

**影片**

|發佈到 Microsoft Store 應用程式 |散發企業應用程式  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>將使用者轉換至您已封裝的應用程式。

在您散發您的 App 之前，建議您考慮將幾個延伸模組新增至您的封裝資訊清單中，以協助使用者習慣使用您的封裝應用程式。 以下是幾個您可以嘗試的方法。

* 將現有的開始畫面磚和工作列按鈕指向您已封裝的應用程式。
* 關聯的檔案類型的一組封裝的應用程式。
* 請依預設開啟特定檔案類型的已封裝的應用程式。

如需延伸功能的完整清單及如何使用其指南，請參閱[將使用者轉換至您的應用程式](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，請考慮將程式碼加入至封裝的應用程式的方式來完成這些工作：

* 將移轉到適當的資料夾位置已封裝應用程式的桌面應用程式相關聯的使用者資料。
* 提供使用者解除安裝您應用程式傳統型版本的選項。

讓我們談談每一項工作。 我們會先從使用者資料移轉開始。

### <a name="migrate-user-data"></a>移轉使用者資料

如果您要加入將使用者資料移轉的程式碼，最好只在第一次啟動應用程式時，才執行該程式碼。 在您移轉使用者資料之前，請先向使用者顯是一個對話方塊，解釋發生了什麼事情、為何建議這樣做，以及對於他們現有的資料會造成何種影響。

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

它是急著解除安裝而不先徵詢其權限的使用者的桌面應用程式。 您可以藉由顯示對話方塊來徵求同意。 使用者可以選擇不解除安裝您應用程式的傳統型版本。 如果發生這種情況，您必須決定是否要封鎖的桌面應用程式的使用方式，或支援並排顯示使用兩個應用程式。

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

### <a name="video"></a>視訊

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>後續步驟

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

如果您在將應用程式發佈至 Microsoft Store 時遇到問題，這篇[部落格文章](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)包含一些有用的秘訣。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。