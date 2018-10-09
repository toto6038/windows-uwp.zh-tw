---
author: laurenhughes
title: 使用 SignTool 簽署應用程式套件
description: 使用 SignTool，手動簽署具憑證的應用程式套件。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: c238855f4f018e8e3142509842221c6b9d97fae3
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2018
ms.locfileid: "4460805"
---
# <a name="sign-an-app-package-using-signtool"></a>使用 SignTool 簽署應用程式套件


**SignTool** 是使用憑證以數位方式簽署應用程式套件或套件組合的命令列工具。 憑證可以由使用者建立 (適用於測試目的) 或由公司發行 (適用於發佈)。 簽署應用程式套件，為使用者提供驗證：在簽署之後應用程式的資料尚未修改，同時也確認簽署使用者或公司的身分識別。 **SignTool** 可以簽署加密或未加密的應用程式套件和套件組合。

> [!IMPORTANT] 
> 如果您使用 Visual Studio 來開發 App，建議您使用 Visual Studio 精靈建立並簽署應用程式套件。 如需詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

如需有關程式碼簽署以及憑證的一般資訊，請參閱[程式碼簽署簡介](https://msdn.microsoft.com/library/windows/desktop/aa380259.aspx#introduction_to_code_signing)。

## <a name="prerequisites"></a>必要條件
- **已封裝應用程式**  
    若要深入了解手動建立應用程式套件，請參閱[使用 MakeAppx.exe 工具建立應用程式套件](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。 

- **有效簽署的憑證**  
    如需有關建立或匯入有效簽署的憑證的詳細資訊，請參閱[建立或匯入要用於套件簽署的憑證](https://msdn.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)。

- **SignTool.exe**  
    根據 SDK 的安裝路徑，這就是 **SignTool** 在您的 Windows 10 電腦上的位置：
    - x86：C:\Program Files (x86)\Windows Kits\10\bin\x86\SignTool.exe
    - x64：C:\Program Files (x86)\Windows Kits\10\bin\x64\SignTool.exe

## <a name="using-signtool"></a>使用 SignTool

**SignTool** 可以用來簽署檔案、驗證簽章或時間戳記、移除簽章，以及更多。 為了簽署應用程式套件，我們會著重於 **sign** 命令。 如需有關 **SignTool** 的完整資訊，請參閱 [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx) 參考頁面。 

### <a name="determine-the-hash-algorithm"></a>判斷雜湊演算法
使用 **SignTool** 簽署應用程式套件或套件組合，用於 **SignTool** 的雜湊演算法必須是用來封裝應用程式的相同演算法。 例如，如果您使用 **MakeAppx.exe** 以預設設定來建立您的應用程式套件，使用 **SignTool** 時必須指定 SHA256，因為這是 **MakeAppx.exe** 所使用的預設演算法。

若要了解封裝應用程式時使用哪種雜湊演算法，請解壓縮應用程式套件並檢查 AppxBlockMap.xml 檔案。 若要了解如何解開/解壓縮應用程式套件，請參閱[從套件或套件組合解壓縮檔案](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool#extract-files-from-a-package-or-bundle)。 雜湊方法在 BlockMap 元素中，而且採用此格式：
```
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap" 
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

下表顯示每個 HashMethod 值，以及對應的雜湊演算法：


| HashMethod 值                              | 雜湊演算法 |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> 因為 **SignTool** 的預設演算法是 SHA1 (**MakeAppx.exe** 中無法使用)，使用 **SignTool** 時，您必須一律指定雜湊演算法。

### <a name="sign-the-app-package"></a>簽署應用程式套件

一旦您擁有所有必要條件並判斷使用何種雜湊演算法封裝您的應用程式，就已準備好簽署它。 

**SignTool** 套件簽署的一般命令列語法是：
```
SignTool sign [options] <filename(s)>
```

用來簽署您應用程式的憑證必須是 .pfx 檔案，或是安裝在憑證存放區中。

若要使用 .pfx 檔案中的憑證簽署您的應用程式套件，使用下列語法：
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```
請注意，`/a` 選項可讓 **SignTool** 自動選擇最佳的憑證。

如果您的憑證不是 .pfx 檔案，請使用下列語法：
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

或者，您可以指定所要憑證的 SHA1 雜湊，而不是 &lt;Name of Certificate&gt;，請使用這個語法：
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

請注意，某些憑證不使用密碼。 如果您的憑證不使用密碼，請省略命令範例中的 "/p &lt;Your Password&gt;"。

一旦使用有效的憑證來簽署您的應用程式套件，您可以將套件上傳至Microsoft Store。 如需上傳與提交應用程式至Microsoft Store的更多指引，請參閱[提交應用程式](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)。

## <a name="common-errors-and-troubleshooting"></a>常見的錯誤以及疑難排解
使用 **SignTool** 最常見的錯誤類型是內部錯誤，通常看起來像這樣：

```
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

如果錯誤碼開頭是 0x8008，例如 0x80080206 (APPX_E_CORRUPT_CONTENT)，簽署的套件無效。 如果您收到此類型的錯誤，必須重新建立套件，並再次執行 **SignTool**。

**SignTool** 有偵錯選項，可顯示憑證錯誤及篩選。 若要使用偵錯功能，請將 `/debug` 選項直接放在 `sign` 之後，後面接著完整 **SignTool** 命令。
```
SignTool sign /debug [options]
``` 

比較常見的錯誤是 0x8007000B。 對於這種錯誤類型，您可以在事件記錄檔中找到詳細資訊。
 
若要在事件記錄檔中尋找詳細資訊：
- 執行 Eventvwr.msc
- 開啟事件記錄檔：\[事件檢視器 (本機)\] -> \[應用程式及服務記錄檔\] -> \[Microsoft\] -> \[Windows\] -> \[AppxPackagingOM\] -> \[Microsoft-Windows-AppxPackaging/Operational\]
- 找出最近的錯誤事件

內部錯誤 0x8007000B 通常會對應於其中一個值︰

| **事件識別碼** | **範例事件字串** | **建議** |
|--------------|--------------------------|----------------|
| 150          | 錯誤 0x8007000B: 應用程式資訊清單發行者名稱 (CN=Contoso) 必須符合簽署憑證 (CN=Contoso, C=US) 的主體名稱。 | 應用程式資訊清單發行者名稱必須完全符合簽署的主體名稱。               |
| 151          | 錯誤 0x8007000B: 指定的簽章雜湊方法 (SHA512) 必須符合應用程式套件區塊對應中使用的雜湊方法 (SHA256)。     | 在 /fd 參數中指定的 hashAlgorithm 不正確。 使用符合應用程式套件區塊對應的 hashAlgorithm (用來建立應用程式套件)，重新執行 **SignTool**  |
| 152          | 錯誤 0x8007000B: 應用程式套件內容必須通過區塊對應的驗證。                                                           | 應用程式套件損壞，需要重建產生新區塊對應。 如需有關手動建立應用程式套件的詳細資訊，請參閱[使用 MakeAppx.exe 工具建立應用程式套件](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)。 |