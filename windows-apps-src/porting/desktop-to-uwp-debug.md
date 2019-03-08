---
Description: 執行您的已封裝應用程式來查看外觀，而不需要簽署。 然後，設定中斷點和逐步執行程式碼。 當您準備好在生產環境測試您的應用程式時，請簽署您的應用程式，然後再安裝它。
Search.Product: eADQiWindows 10XVcnh
title: 執行、偵錯以及測試封裝的傳統型橋接器應用程式 (傳統型橋接器)
ms.date: 08/31/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 8b2350c8164548121baec231335e747166f1c082
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589753"
---
# <a name="run-debug-and-test-a-packaged-desktop-application"></a>執行、 偵錯和測試封裝的桌面應用程式

執行封裝的應用程式，並查看其外觀而不必加以簽署。 然後，設定中斷點和逐步執行程式碼。 當您準備好在生產環境中測試您的應用程式時，請登入您的應用程式，然後再安裝它。 此主題將會告訴您如何執行每個項目。

<a id="run-app" />

## <a name="run-your-application"></a>執行您的應用程式

您可以執行您的應用程式加以測試在本機而不需要取得憑證並加以簽署。 執行應用程式的方式取決於哪些工具用來建立封裝。

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
> 封裝的應用程式一律會執行非互動式的使用者，並安裝已封裝的應用程式上的任何磁碟機必須格式化為 NTFS 格式。

## <a name="debug-your-app"></a>偵錯您的應用程式

如何偵錯應用程式相依於哪些工具用來建立封裝。

若您是使用 Visual Studio 2017 15.4 版中的[新封裝專案](desktop-to-uwp-packaging-dot-net.md#new-packaging-project)建立套件，您只需將封裝專案設定為啟始專案，然後按下 F5 即可偵錯應用程式。

如果使用任何其他工具建立套件，請依照下列步驟執行。

1. 請確定您會啟動已封裝的應用程式至少一次，使它安裝在本機電腦上。

   請參閱上述[執行您的應用程式](#run-app)一節。

2. 啟動 Visual Studio。

   如果您想要偵錯您的應用程式，以提高權限，請使用啟動 Visual Studio**系統管理員身分執行**選項。

3. 在 Visual Studio 中，選擇 **\[偵錯\]**->**\[其他偵錯目標\]**->**\[偵錯已安裝的應用程式套件\]**。

4. 在 **\[已安裝的應用程式套件\]** 清單中，選取您的應用程式套件，然後選擇 **\[附加\]** 按鈕。

#### <a name="modify-your-application-in-between-debug-sessions"></a>修改您的應用程式偵錯工作階段之間

若要變更您的應用程式，若要修正的 bug，請使用 MakeAppx 工具將它重新封裝。 請參閱[執行 MakeAppx 工具](desktop-to-uwp-manual-conversion.md#make-appx)。

### <a name="debug-the-entire-application-lifecycle"></a>偵錯整個應用程式生命週期

在某些情況下，您可以更精細的控制，透過 偵錯的處理序，並包括偵錯您的應用程式，則在開始之前的能力。

您可以使用[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)來完全掌控應用程式生命週期，包括暫停、 繼續及終止。

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) 隨附於 Windows SDK。

## <a name="test-your-app"></a>測試您的應用程式

當您準備散發，請在實際的設定中測試應用程式，最好是登入您的應用程式，然後再安裝它。

### <a name="test-an-application-that-you-packaged-by-using-visual-studio"></a>測試您使用 Visual Studio 一起封裝的應用程式

Visual Studio 會使用測試憑證，以將您的應用程式。 您會在輸出資料夾中找到**建立應用程式套件**精靈產生的憑證。 憑證檔案具有 *.cer*延伸模組，您會需要該憑證安裝到**受信任的根憑證授權單位**儲存在您想要測試您的應用程式上的電腦上。 請參閱[側載您的套件](../packaging/packaging-uwp-apps.md#sideload-your-app-package)。

### <a name="test-an-application-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>測試您使用 Desktop App Converter (DAC) 封裝的應用程式

如果您使用 Desktop App Converter 來封裝您的應用程式，您可以使用``sign``自動使用產生的憑證，以登入您的應用程式的參數。 您必須先安裝該憑證，然後再安裝應用程式。 請參閱[執行封裝後的應用程式](desktop-to-uwp-run-desktop-app-converter.md#run-app)。   


### <a name="manually-sign-apps-optional"></a>以手動方式簽署 app（選用）

您也可以手動簽署您的應用程式。 作法如下

1. 建立憑證。 請參閱[建立憑證](../packaging/create-certificate-package-signing.md)。

2. 將該憑證安裝到您系統上的 **「受信任的根」** 或 **「受信任的人」** 憑證存放區。

3. 使用該憑證來簽署您的應用程式，請參閱 <<c0> [ 登入應用程式套件使用 SignTool](../packaging/sign-app-package-using-signtool.md)。

  > [!IMPORTANT]
  > 請確定您憑證的發行者名稱符合您應用程式的發行者名稱。

**相關的範例**

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-application-for-windows-10-s"></a>測試您的應用程式的 Windows 10 S

發行您的應用程式之前，請確定它會正確運作的裝置執行 Windows 10 s。事實上，如果您打算在應用程式發佈至 Microsoft Store，您必須這麼因為它是存放區需求。 無法在執行 Windows 10 S 的裝置上正確運作的應用程式，無法獲得認證。

請參閱[測試您的 Windows 應用程式的 Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)。

### <a name="run-another-process-inside-the-full-trust-container"></a>在完全信任的容器內執行另一個處理序

您可以在指定應用程式套件的容器內叫用自訂處理序。 這對於測試案例非常有用 (例如，如果您擁有自訂的測試載入器，而且想要測試 app 的輸出)。 若要這樣做，請使用 ```Invoke-CommandInDesktopPackage``` PowerShell Cmdlet：

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>後續步驟

**尋找問題的解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或提出功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
