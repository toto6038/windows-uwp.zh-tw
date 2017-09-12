---
author: normesta
Description: "本文包含傳統型橋接器的已知問題。"
Search.Product: eADQiWindows 10XVcnh
title: "已知問題 (傳統型橋接器)"
ms.author: normesta
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: bf0e81d4a6ff7bd091f25963b1cf26cdecc93636
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2017
---
# <a name="known-issues-desktop-bridge"></a>已知問題 (傳統型橋接器)

本文包含傳統型橋接器的已知問題。

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>藍色畫面，錯誤碼 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

從 Windows 市集安裝或啟動特定 App 後，電腦可能會發生下列錯誤而意外地重新開機：**0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**。

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

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>C#/VB.NET 和 C++ UWP 專案的已知問題

如果您偏好使用 C# 專案封裝您的應用程式，則須注意下列已知問題。

- 在偵錯模式中建置應用程式會造成此錯誤：_Microsoft.Net.CoreRuntime.targets(235,5)：錯誤：不支援使用自訂進入點可執行檔的應用程式。請檢查套件資訊清單中，應用程式項目的可執行檔屬性。_

  若要修正此問題，請改用發行模式。

- 發行版本中會移除 UWP 專案根資料夾中儲存的 Win32 二進位檔。 .NET Native 編譯器將會從最終套件中移除這些檔案，導致資訊清單驗證錯誤，因為找不到可執行檔進入點。

  若要修正此問題，請在您的專案中建立子資料夾來儲存 win32 二進位檔。


## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>您收到錯誤：MSB4018 "GenerateResource" 工作發生未預期的失敗

發生這種情形時，請嘗試將附屬組件轉換為套件資源索引 (PRI) 檔案。

我們已注意到此問題，正在研究長期解決方案。 暫時的解決方法是，可以透過將這一行 XML 新增到裝載專案檔的第一個 PropertyGroup 元素，來停用資源產生器：

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``
