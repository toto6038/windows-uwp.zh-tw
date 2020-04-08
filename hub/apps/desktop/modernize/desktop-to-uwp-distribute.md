---
Description: 散發使用傳統型橋接器封裝的應用程式
title: 將您已封裝的傳統型應用程式發佈到 Microsoft Store，或是在一或多個裝置上進行側載。
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 14ad6707b7203dddd9aa7be186e76da677bbd675
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302702"
---
# <a name="distribute-your-packaged-desktop-app"></a>散發已封裝的傳統型應用程式

如果您決定[將傳統型應用程式封裝於 MSIX 套件中](/windows/msix/desktop/desktop-to-uwp-root)，您可以將已封裝的應用程式發佈到 Microsoft Store 或在一或多個裝置上進行側載。

> [!NOTE]
> 您有計畫如何將使用者轉換至您已封裝的應用程式嗎？ 散發應用程式之前，請參閱此指引的[將使用者轉換至您已封裝的應用程式](#transition-users)一節，以取得一些靈感。

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>將您的應用程式發佈到 Microsoft Store 以進行散發

[Microsoft Store](https://www.microsoft.com/store/apps) 是讓客戶取得您應用程式的一種簡便方式。

將您的應用程式發佈到 Microsoft Store，以使觸達人數最為廣泛。 同時，組織客戶也可透過[商務用 Microsoft Store](https://businessstore.microsoft.com/store) 來取得您的應用程式，以在其組織內部進行散發。

如果您計畫發佈到 Microsoft Store，則在提交過程中將會詢問您一些額外問題。 這是因為您的封裝資訊清單會宣告一個名為 **runFullTrust** 的受限制功能，而且我們需要核准您的應用程式使用該功能。 您也可以在這裡閱讀更多有關此需求的資訊：[受限制的功能](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)。

在您將應用程式提交到 Microsoft Store 之前，不需要簽署該應用程式。

>[!IMPORTANT]
> 如果您計畫將應用程式發佈到 Microsoft Store，請確定您的應用程式可在執行 Windows 10 S 的裝置上正確運作。此為 Microsoft Store 需求。 請參閱[針對 Windows 10 S 測試您的 Windows 應用程式](/windows/msix/desktop/desktop-to-uwp-test-windows-s)。

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>不透過 Microsoft Store 散發您的應用程式

若您不想要使用 Microsoft Store 散發應用程式，則可手動將應用程式散發到一或多個裝置。

若您想要進一步控制散發體驗，或者不想參與 Microsoft Store 認證程序，這會是個合理的作法。

若您不想透過 Microsoft Store 將應用程式散發到其他裝置，就必須取得憑證、使用該憑證簽署您的應用程式，然後在那些裝置上側載您的應用程式。

您可以[建立憑證](/windows/msix/package/create-certificate-package-signing)或向熱門的供應商 (例如 [Verisign](https://www.verisign.com/) \(英文\)) 取得憑證。

若您想要將應用程式散發到執行 Windows 10 S 的裝置，您的應用程式必須先經過 Microsoft Store 簽署，因此，您必須先完成 Microsoft Store 提交程序，才能將應用程式散發到那些裝置。

若您建立憑證，就必須在每個執行您應用程式的裝置上，將該憑證安裝到 [受信任的根]  或 [受信任的人]  憑證存放區。 若您向熱門的供應商取得憑證，則除了您的應用程式，不需在其他系統上安裝任何其他項目。  

> [!IMPORTANT]
> 請確定您憑證的發行者名稱符合應用程式的發行者名稱。

若要使用憑證來簽署您的應用程式，請參閱[使用 SignTool 簽署應用程式套件](/windows/msix/package/sign-app-package-using-signtool)。

若要在其他裝置上側載您的應用程式，請參閱[在 Windows 10 中側載 LOB 應用程式](/windows/application-management/sideload-apps-in-windows-10)。

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>將使用者轉換至您已封裝的應用程式

散發您的應用程式之前，請考慮在您的封裝資訊清單新增一些延伸模組，以協助使用者習慣使用您已封裝的應用程式。 以下是幾個您可以嘗試的方法。

* 將現有的開始畫面磚和工作列按鈕指向您已封裝的應用程式。
* 使您已封裝的應用程式和一組檔案類型產生關聯。
* 讓您已封裝的應用程式預設會開啟特定類型的檔案。

如需完整的延伸模組清單及使用這些延伸模組的指引，請參閱[將使用者轉換至您的應用程式](desktop-to-uwp-extensions.md#transition-users-to-your-app)。

此外，請考慮將程式碼新增至您已封裝的應用程式，以完成這些工作：

* 將與您傳統型應用程式相關聯的使用者資料移轉至您已封裝之應用程式的適當資料夾位置。
* 為使用者提供解除安裝您應用程式傳統型版本的選項。

讓我們來討論這其中每一個工作。 我們將從使用者資料移轉開始。

### <a name="migrate-user-data"></a>移轉使用者資料

如果您要新增能移轉使用者資料的程式碼，最好只在應用程式第一次啟動時執行該程式碼。 在您移轉使用者資料之前，先向使用者顯示一個對話方塊，以解釋發生了什麼事、為何建議這樣做，以及會對其現有資料造成何種影響。

以下是如何在 .NET 型已封裝的應用程式中執行此動作的範例。

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

建議您在解除安裝使用者的傳統型應用程式之前，先徵求同意。 顯示對話方塊，徵求使用者同意授與該權限。 使用者可能決定不解除安裝您應用程式的傳統型版本。 若發生這種情形，您將必須決定是否要封鎖傳統型應用程式的使用，或支援這兩種版本應用程式的並列使用。

以下是如何在 .NET 型已封裝的應用程式中執行此動作的範例。

若要檢視此程式碼片段的完整內容，請參閱此範例[具有轉換/移轉/解除安裝的 WPF 圖片檢視器](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition) \(英文\) 中的 **MainWindow.cs** 檔案。

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

## <a name="next-steps"></a>接下來的步驟

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標籤](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在這裡](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)發問。

如果您將應用程式發佈到 Microsoft Store 時遇到問題，這篇[部落格文章](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) \(英文\) 包含一些有用的提示。
