---
author: normesta
Description: Run your packaged app and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your app in a production environment, sign your app and then install it.
Search.Product: eADQiWindows 10XVcnh
title: 執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)
ms.author: normesta
ms.date: 08/31/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: b5110eebde087593f07704e89c2e4708b2fcbb8b
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "5397420"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>執行、 偵錯以及測試封裝的傳統型應用程式

執行您的已封裝應用程式並查看外觀，而不需要簽署它。 然後，設定中斷點和逐步執行程式碼。 當您準備好在生產環境中測試您的應用程式時，簽署您的應用程式，然後再安裝它。 此主題將會告訴您如何執行每個項目。

<a id="run-app" />

## <a name="run-your-application"></a>執行您的應用程式

您可以執行您的應用程式進行測試，在本機而不需要取得憑證和進行簽署。 應用程式的執行方式取決於您工具用來建立套件。

### <a name="you-created-the-package-by-using-visual-studio"></a>使用 Visual Studio 建立套件

將封裝專案設定為起始專案，然後按 CTRL+F5 啟動應用程式。

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>手動或使用 Desktop App Converter 建立套件

開啟 Windows PowerShell 命令提示，並從輸出資料夾的 **PackageFiles** 子資料夾執行此 cmdlet：

```
Add-AppxPackage –Register AppxManifest.xml
```
若要啟動您的應用程式，請在 Windows \[開始\] 功能表中找到您的應用程式。

![在 [開始] 功能表中的已封裝應用程式](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> 已封裝的應用程式一律會以互動式使用者的身分執行，並安裝您已封裝的應用程式在任何磁碟機必須格式化為 NTFS 格式。

## <a name="debug-your-app"></a>偵錯您的應用程式

偵錯應用程式的方式取決於您工具用來建立套件。

若您是使用 Visual Studio 2017 15.4 版中的[新封裝專案](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)建立套件，您只需將封裝專案設定為啟始專案，然後按下 F5 即可偵錯應用程式。

如果使用任何其他工具建立套件，請依照下列步驟執行。

1. 請確定您，讓它安裝在您的本機電腦上啟動您的已封裝應用程式至少一次。

   請參閱上述[執行您的應用程式](#run-app)一節。

2. 啟動 Visual Studio。

   如果您想要偵錯您的應用程式，在提升權限，請使用**系統管理員身分執行**] 選項啟動 Visual Studio。

3. 在 Visual Studio 中，選擇 **\[偵錯\]**->**\[其他偵錯目標\]**->**\[偵錯已安裝的應用程式套件\]**。

4. 在 **\[已安裝的應用程式套件\]** 清單中，選取您的應用程式套件，然後選擇 **\[附加\]** 按鈕。

#### <a name="modify-your-application-in-between-debug-sessions"></a>修改您的應用程式偵錯工作階段之間

如果您變更您的應用程式以修正問題，請將它重新封裝使用 MakeAppx 工具。 請參閱[執行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

### <a name="debug-the-entire-application-lifecycle"></a>偵錯整個應用程式生命週期

在某些情況下，您可能想對偵錯的程序，包括能夠在它開始之前偵錯您的應用程式進行更細微的控制。

您可以使用[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)以取得完整控制應用程式生命週期，包括暫停、 繼續及終止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 隨附於 Windows SDK。

## <a name="test-your-app"></a>測試您的應用程式

若要測試您的應用程式逼真的設定，當您準備散布，最好要登入您的應用程式，然後再安裝它。

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>測試使用 Visual Studio 封裝的應用程式

Visual Studio 使用測試憑證簽署您的應用程式。 您會在輸出資料夾中找到**建立應用程式套件**精靈產生的憑證。 憑證檔案有 *.cer*擴充功能，您必須將該憑證安裝到您想要測試您的應用程式上的電腦上**受信任的根憑證授權單位**存放區。 請參閱[側載您的套件](../packaging/packaging-uwp-apps.md#sideload-your-app-package)。

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>測試使用 Desktop App Converter (DAC) 封裝的應用程式

如果您是使用 Desktop App Converter 封裝您的應用程式，您可以使用``sign``參數來自動使用產生的憑證簽署您的應用程式。 您必須先安裝該憑證，然後再安裝應用程式。 請參閱[執行封裝後的應用程式](desktop-to-uwp-run-desktop-app-converter.md#run-app)。   


### <a name="manually-sign-apps-optional"></a>以手動方式簽署 app（選用）

您也可以手動簽署您的應用程式。 作法如下

1. 建立憑證。 請參閱[建立憑證](../packaging/create-certificate-package-signing.md)。

2. 將該憑證安裝到您系統上的 **「受信任的根」** 或 **「受信任的人」** 憑證存放區。

3. 使用該憑證簽署您的應用程式，請參閱[簽署應用程式套件使用 SignTool](../packaging/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 請確定您憑證的發行者名稱符合您應用程式的發行者名稱。

    **相關範例**

    [SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>測試您的應用程式，適用於 Windows 10 S

發佈您的應用程式之前，請確定它將會正常運作上執行 Windows 10 S 的裝置事實上，如果您打算發行至 Microsoft Store 應用程式，您必須執行此動作因為這是市集需求。 無法在執行 Windows 10 S 的裝置上正確運作的應用程式，無法獲得認證。

請參閱[您的 Windows 應用程式，適用於 Windows 10 S 測試](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器內執行另一個處理程序

您可以在指定應用程式套件的容器內叫用自訂處理序。 這對於測試案例非常有用 (例如，如果您擁有自訂的測試載入器，而且想要測試 app 的輸出)。 若要這樣做，請使用 ```Invoke-CommandInDesktopPackage``` PowerShell Cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
