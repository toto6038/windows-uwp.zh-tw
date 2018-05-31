---
author: normesta
Description: This article contains known issues with the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: 已知問題 (傳統型橋接器)
ms.author: normesta
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 78e5ffddfa1c5005bb640baeafed7023ebdd74a3
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/17/2018
ms.locfileid: "1662848"
---
# <a name="known-issues-desktop-bridge"></a>已知問題 (傳統型橋接器)

本文包含傳統型橋接器的已知問題。

<a id="app-converter" />

## <a name="known-issues-with-the-desktop-app-converter"></a>Desktop App Converter 的已知問題

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED an E_STARTING_ISOLATED_ENV_FAILED 錯誤    

若您接收到其中任何一項錯誤，請確定您使用的是[下載中心](https://aka.ms/converterimages)內的有效基礎映像。
如果您使用的是正確的基礎映像，請嘗試在您的命令中使用 ``-Cleanup All``。
若此方法仍然無法解決您的問題，請將您的記錄檔傳送至 converter@microsoft.com，以幫助我們調查。

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork︰物件已經存在錯誤

當您設定一個新的基礎映像時，可能會接收到這個錯誤。 若您在先前已安裝 Desktop App Converter 的程式開發人員電腦上擁有 Windows 測試人員正式發行前小眾測試版，則可能會發生此問題。

若要修正此問題，請嘗試在提升權限的命令提示字元中執行命令 `Netsh int ipv4 reset`，並重新啟動您的電腦。

### <a name="your-net-app-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>您的 .NET 應用程式使用「AnyCPU」組建選項進行編譯，但安裝仍然失敗

若主要可執行檔或任何相依性檔案是存放於 **Program Files** 或 **Windows\System32** 資料夾階層底下，就有可能會發生此問題。

若要修正此問題，請嘗試使用特定架構的桌面安裝程式 (32 位元或 64 位元) 產生 Windows 應用程式套件。

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>發行公用的並列 Fusion 組件無法運作

 在安裝期間，應用程式可以發行公用的並列 Fusion 組件，可存取任何其他處理程序。 在處理序啟用內容建立期間，這些組件是透過名為 CSRSS.exe 的系統處理序來擷取。 針對已轉換的處理程序完成此動作之後，這些組件的啟用內容建立和模組載入將會失敗。 並列 Fusion 組件會登錄於下列位置︰
  + 登錄： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 檔案系統︰%windir%\SideBySide

這是已知的限制，並且目前沒有任何因應措施。 但是，由於收件匣組件 (例如 ComCtl) 均隨附於作業系統，因此取得和它們的相依性是安全的。

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>XML 中找到錯誤。 'Executable' 屬性無效：根據其資料類型，數值 'MyApp.EXE' 無效

若您應用程式中包含了副檔名為大寫 **.EXE** 的可執行檔，就有可能發生這個錯誤。 雖然副檔名的大小寫並不會影響您應用程式的執行，但這仍有可能會使 DAC 產生這個錯誤。

若要修正此問題，請嘗試在封裝時指定 **-AppExecutable** 旗標，並使用小寫的「.exe」作為您主要可執行檔的副檔名 (例如：MYAPP.exe)。    或者，您也可以將您應用程式當中所有可執行檔的副檔名，從小寫變更為大寫 (例如：從 .EXE 改為 .exe)。

### <a name="corrupted-or-malformed-authenticode-signatures"></a>毀損或格式錯誤的 Authenticode 簽章

本節中的詳細資訊包括如何識別 Windows 應用程式套件中可攜式執行 (PE) 檔的問題，且套件中可能包含損毀或格式錯誤的 Authenticode 簽章。 PE 檔 (exe、.dll、.chm 等任何二進位檔案格式) 上無效的 Authenticode 簽章，會導致無法正確簽署您的套件，進而造成無法從 Windows 應用程式套件部署它。

PE 檔的 Authenticode 簽章的位置是由「選用標頭資料目錄」中的「憑證表格」及相關的「屬性憑證表格」來指定。 驗證簽章的期間，會使用這些結構中的資訊來找出 PE 檔上的簽章。 如果這些值已損毀，就可能使檔案的簽署看似無效。

為了使 Authenticode 簽章正確無誤，Authenticode 簽章必須符合下列條件：

- PE 檔中 **WIN_CERTIFICATE** 項目的開頭不可延伸超過可執行檔的結尾
- 
            **WIN_CERTIFCATE** 項目應位在映像的結尾
- 
            **WIN_CERTIFICATE** 項目的大小必須是正值
- 32 位元可執行檔的 **WIN_CERTIFICATE** 項目必須在 **IMAGE_NT_HEADERS32** 結構之後開始，64 位元可執行檔則是在 IMAGE_NT_HEADERS64 結構之後開始

如需詳細資訊，請參閱 [Authenticode 可攜式可執行檔規格](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx)和 [PE 檔格式規格](https://msdn.microsoft.com/windows/hardware/gg463119.aspx)。

請注意，嘗試簽署 Windows 應用程式套件時，SignTool.exe 可以輸出損毀或格式錯誤之二進位檔案的清單。 若要這樣做，請將環境變數 APPXSIP_LOG 設定為 1 (如 ```set APPXSIP_LOG=1```) 以啟用詳細資訊記錄，然後重新執行 SignTool.exe。

若要修正這些格式錯誤的二進位檔，請確定它們符合上述需求。

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>您收到錯誤：MSB4018 "GenerateResource" 工作發生未預期的失敗

發生這種情形時，請嘗試將附屬組件轉換為套件資源索引 (PRI) 檔案。

我們已注意到此問題，正在研究長期解決方案。 暫時的解決方法是，可以透過將這一行 XML 新增到裝載專案檔的第一個 PropertyGroup 元素，來停用資源產生器：

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>藍色畫面，錯誤碼 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

從 Microsoft Store安裝或啟動特定 App 後，電腦可能會發生下列錯誤而意外地重新開機：**0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**。

已知受影響的 App 包括 Kodi、JT2Go、Ear Trumpet、Teslagrad 和其他 App。

2016 年 10 月 27 日已發佈一 個 [Windows 更新 (版本 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954)，其中包含可處理此問題的重要修正。 如果您發生此問題，請更新您的電腦。 如果您因為電腦在可以登入之前就重新開機而無法更新電腦，您應該使用系統還原將您的系統復原至安裝受影響 App 之前的時間點。 如需如何使用系統還原的資訊，請參閱 [Windows10 中的復原選項](https://support.microsoft.com/help/12415/windows-10-recovery-options)。

如果更新無法修正問題，或您不確定如何復原電腦，請連絡 [Microsoft 支援服務](https://support.microsoft.com/contactus/)。

如果您是開發人員，建議您避免在不包含此更新的 Windows 版本上安裝傳統型橋接器 App。 請注意，這麼做將會使您的 App 無法提供給尚未安裝更新的使用者。 若要將您 App 的可用性限制為僅提供給已安裝此更新的使用者，請如下所示修改您的 AppxManifest.xml 檔案：

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

關於 Windows Update 的詳細資料，請參閱：
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>當您簽署您應用程式時可能出現的常見錯誤

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>發行者和憑證不符導致 Signtool 錯誤「錯誤：SignerSign() 失敗」(-2147024885/0x8007000b)

Windows 應用程式套件資訊清單中的發行者項目必須符合您用來簽署的憑證主體。  您可以使用下列任一種方法來檢視憑證的主體。

**選項 1：Powershell**

執行下列 PowerShell 命令。 .cer 或.pfx 可以用來做為憑證檔案，因為它們具有相同的發行者資訊。

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**選項 2：檔案總管**

按兩下 [檔案總管] 中的憑證、選取 *\[詳細資料\]* 索引標籤，然後選取清單中的 *\[主體\]* 欄位。 您接著可以複製內容。

**選項 3：CertUtil**

從 PFX 檔案的命令列執行 **certutil**，然後從輸出複製 *\[主體\]* 欄位。

```cmd
certutil -dump <cert_file.pfx>
```

## <a name="next-steps"></a>後續步驟

**尋找您的問題解答**

有任何問題嗎？ 請在 Stack Overflow 上發問。 我們的團隊會監視這些[標記](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 您也可以[在此處](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)詢問我們。

**提供意見反應或功能建議**

請參閱 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
