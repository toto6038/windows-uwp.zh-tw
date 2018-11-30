---
title: 使用 MakeAppx.exe 工具建立應用程式套件
description: MakeAppx.exe 建立、加密、解密應用程式套件與套件組合，並從應用程式套件與套件組合中擷取檔案。
ms.date: 06/21/2018
ms.topic: article
keywords: windows 10, uwp, 封裝
ms.assetid: 7c1c3355-8bf7-4c9f-b13b-2b9874b7c63c
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: dc109fe2e684dd3bc1fef62cece5cac3ab50d246
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2018
ms.locfileid: "8329218"
---
# <a name="create-an-app-package-with-the-makeappxexe-tool"></a>使用 MakeAppx.exe 工具建立應用程式套件


**MakeAppx.exe** 會建立應用程式套件和應用程式套件組合。 **MakeAppx.exe** 也會從應用程式套件或套件組合中擷取檔案，以及加密或解密應用程式套件與套件組合。 此工具包含在 Windows 10 SDK，而且可以從命令提示字元或指令碼檔案使用。

> [!IMPORTANT] 
> 如果您使用 Visual Studio 來開發 App，建議您使用 Visual Studio 精靈建立應用程式套件。 如需詳細資訊，請參閱[使用 Visual Studio 封裝 UWP app](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)。

請注意，**MakeAppx.exe** 不會建立 .appxupload 檔案。 .Appxupload 檔案會建立為 Visual Studio 封裝程序的一部分，並包含其他兩個檔案：.msix 或.appx 和.appxsym。 .Appxsym 檔案是包含用於合作夥伴中心內的[損毀分析](../publish/health-report.md)您 app 的公用符號的壓縮的.pdb 檔案。 一般的 .appx 檔案也可以一起提交，但不會有任何損毀分析或偵錯資訊。 如需有關將套件提交到Microsoft Store的詳細資訊，請參閱[上傳應用程式套件](../publish/upload-app-packages.md)。 

 此工具的最新版本的 Windows 10 中的更新不會影響.appx 套件使用方式。 您可以繼續使用.appx 套件，使用此工具，或使用工具的支援，針對.msix 套件如下所述。

手動建立 .appxupload 檔案︰
- 將.msix 和.appxsym 放在資料夾中
- 壓縮資料夾
- 將壓縮的資料夾副檔名從 .zip 變更為 .appxupload

## <a name="using-makeappxexe"></a>使用 MakeAppx.exe

根據 SDK 的安裝路徑，這就是 **MakeAppx.exe** 在您的 Windows 10 電腦上的位置︰
- x86: C:\Program Files (x86) \Windows Kits\10\bin\\&lt;組建編號&gt;\x86\makeappx.exe
- x64: C:\Program Files (x86) \Windows Kits\10\bin\\&lt;組建編號&gt;\x64\makeappx.exe

此工具沒有 ARM 版本。

### <a name="makeappxexe-syntax-and-options"></a>MakeAppx.exe 語法和選項

一般的 **MakeAppx.exe** 語法︰

``` Usage
MakeAppx <command> [options]      
```

下表描述 **MakeAppx.exe** 的命令。

| **命令**   | **描述**                       |
|---------------|---------------------------------------|
| pack          | 建立套件。                    |
| unpack        | 將指定套件中的所有檔案解壓縮到指定的輸出目錄。 |
| bundle        | 建立套件組合。                     |
| unbundle      | 將所有套件解除封裝到以套件組合完整名稱命名之指定輸出路徑下的子目錄。 |
| encrypt       | 從輸入的套件/套件組合在指定的輸出套件/套件組合建立加密的應用程式套件或套件組合。 |
| decrypt       | 從輸入的應用程式套件/套件組合在指定的輸出套件/套件組合建立解密的應用程式套件或套件組合。 |


這個選項清單適用於所有命令︰

| **選項**    | **描述**                       |
|---------------|---------------------------------------|
| /d            | 指定輸入、輸出或內容目錄。 |
| /l            | 供當地語系化的套件使用。 預設驗證是針對當地語系化的套件進行。 此選項只停用該特定的驗證，不需要停用所有的驗證。 |
| /kf           | 使用指定金鑰檔案的金鑰加密或解密套件或套件組合。 這不能搭配 /kt 使用。 |
| /kt           | 使用全域測試金鑰加密或解密套件或套件組合。 這不能搭配 /kf 使用。 |
| /no           | 如果有，可以避免覆寫輸出檔案。 如果您沒有指定此選項或 /o 選項，會詢問使用者是否要覆寫檔案。 |
| /nv           | 略過語意驗證。 如果您沒有指定此選項，工具會對套件執行完整驗證。 |
| /o            | 如果有，會覆寫輸出檔案。 如果您沒有指定此選項或 /no 選項，會詢問使用者是否要覆寫檔案。 |
| /p            | 指定應用程式套件或套件組合。  |
| /v            | 可以將詳細資訊記錄輸出到主控台。 |
| /?            | 顯示說明文字。                   |


下列清單包含可能的引數︰

| **引數**                          | **描述**                       |
|---------------------------------------|---------------------------------------|
| &lt;輸出套件名稱&gt;           | 建立的套件名稱。 這是檔案名稱會附加.msix 或.appx。 |
| &lt;加密的輸出套件名稱&gt; | 建立的加密套件名稱。 這是檔案名稱會附加.emsix 或.eappx。 |
| &lt;輸入套件名稱&gt;            | 套件的名稱。 這是檔案名稱會附加.msix 或.appx。 |
| &lt;加密的輸入套件名稱&gt;  | 加密套件的名稱。 這是檔案名稱會附加.emsix 或.eappx。 |
| &lt;輸出套件組合名稱&gt;            | 建立的套件組合名稱。 這是檔案名稱會附加.msixbundle 或.appxbundle。 |
| &lt;加密的輸出套件組合名稱&gt;  | 建立的加密套件組合名稱。 這是檔案名稱會附加.emsixbundle 或.eappxbundle。 |
| &lt;輸入套件組合名稱&gt;             | 套件組合的名稱。 這是檔案名稱會附加.msixbundle 或.appxbundle。 |
| &lt;加密的輸入套件組合名稱&gt;   | 加密套件組合的名稱。 這是檔案名稱會附加.emsixbundle 或.eappxbundle。 |
| &lt;內容目錄&gt;             | 應用程式套件或套件組合內容的路徑。 |
| &lt;對應檔案&gt;                  | 指定套件來源和目的地的檔案名稱。 |
| &lt;輸出目錄&gt;              | 輸出套件和套件組合的目錄路徑。 |
| &lt;金鑰檔案&gt;                      | 包含加密或解密用之金鑰的檔案名稱。 |
| &lt;演算法識別碼&gt;                  | 建立區塊對應時使用的演算法。 有效的演算法包括︰SHA256 (預設)、SHA384、SHA512。 |


### <a name="create-an-app-package"></a>建立應用程式套件

應用程式套件是一組完整的應用程式的封裝成.msix 或.appx 套件檔案的檔案。 若要使用 **pack** 命令建立應用程式套件，您必須提供內容目錄或套件位置的對應檔案。 您也可以在建立套件時進行加密。 如果您想要加密套件，您必須使用 /ep，並指定要使用金鑰檔案 (/kf) 或全域測試金鑰 (/kt)。 如需有關建立加密套件的詳細資訊，請參閱[加密或解密套件或套件組合](#encrypt-or-decrypt-a-package-or-bundle)。

**pack** 命令專用的選項︰

| **選項**    | **描述**                       |
|---------------|---------------------------------------|
| /f            | 指定對應檔案。           |
| /h            | 指定建立區塊對應時要使用的雜湊演算法。 只能與 pack 命令搭配使用。 有效的演算法包括︰SHA256 (預設)、SHA384、SHA512。 |
| /m            | 指定輸入應用程式資訊清單的路徑，此路徑將用來做為產生輸出應用程式套件或資源套件之資訊清單的基礎。  當您使用此選項時，您也必須使用 /f，並在對應檔案中包含一個 \[ResourceMetadata\] 區段以指定要在產生的資訊清單中包含的資源維度。|
| /nc           | 防止壓縮套件檔案。 檔案預設會根據偵測到的檔案類型進行壓縮。 |
| /r            | 建置資源套件。 這必須與 /m 搭配使用，也表示使用 /l 選項。 |  


下列使用方式範例顯示 **pack** 命令一些可能的語法選項︰

``` syntax 
MakeAppx pack [options] /d <content directory> /p <output package name>
MakeAppx pack [options] /f <mapping file> /p <output package name>
MakeAppx pack [options] /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /r /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kf <key file>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kt

```
以下顯示 **pack** 命令的命令列範例︰

``` examples
MakeAppx pack /v /h SHA256 /d "C:\My Files" /p MyPackage.msix
MakeAppx pack /v /o /f MyMapping.txt /p MyPackage.msix
MakeAppx pack /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p AppPackage.msix
MakeAppx pack /r /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p ResourcePackage.msix
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kf MyKeyFile.txt
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kt
```

### <a name="create-an-app-bundle"></a>建立應用程式套件組合

應用程式套件組合類似應用程式套件，但是套件組合可以減少使用者下載的應用程式大小。 應用程式套件組合對於語言特定的資產、影像大小變動的資產，或是套用到如特定 Microsoft DirectX 版本的資源很有幫助。 與建立加密的應用程式套件類似，您也可以在組合應用程式套件組合時進行加密。 若要加密應用程式套件組合，請使用 /ep 選項，並指定要使用金鑰檔案 (/kf) 或全域測試金鑰 (/kt)。 如需有關建立加密套件組合的詳細資訊，請參閱[加密或解密套件或套件組合](#encrypt-or-decrypt-a-package-or-bundle)。

**bundle** 命令專用的選項︰

| **選項**    | **描述**                       |
|---------------|---------------------------------------|
| /bv           | 指定套件組合的版本號碼。 版本號碼的格式必須是以句點隔開的四個部分︰&lt;主要&gt;.&lt;次要&gt;.&lt;建置&gt;.&lt;修訂&gt;。 |
| /f            | 指定對應檔案。           |

請注意，如果未指定套件組合版本，或如果設定為 "0.0.0.0"，則套件組合會使用目前的日期時間建立。

下列使用方式範例顯示 **bundle** 命令一些可能的語法選項︰

``` syntax
MakeAppx bundle [options] /d <content directory> /p <output bundle name>
MakeAppx bundle [options] /f <mapping file> /p <output bundle name>
MakeAppx bundle [options] /d <content directory> /ep <encrypted output bundle name> /kf MyKeyFile.txt
MakeAppx bundle [options] /f <mapping file> /ep <encrypted output bundle name> /kt
```
下列區塊包含 **bundle** 命令的範例︰

``` examples
MakeAppx bundle /v /d "C:\My Files" /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kf MyKeyFile.txt
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kt
```

### <a name="extract-files-from-a-package-or-bundle"></a>從套件或套件組合解壓縮檔案

**MakeAppx.exe** 除了封裝與組合應用程式，也可以解除封裝或拆開現有的套件。 您必須為解壓縮的檔案提供內容目錄做為目的地。 如果您嘗試從加密的套件或套件組合解壓縮檔案，您可以使用 /ep 選項並指定要使用金鑰檔案 (/kf) 或全域測試金鑰 (/kt) 進行解密，同時解密並解壓縮檔案。 如需有關解密套件或套件組合的詳細資訊，請參閱[加密或解密套件或套件組合](#encrypt-or-decrypt-a-package-or-bundle)。

**unpack** 和 **unbundle** 命令專用的選項︰

| **選項**    | **描述**                       |
|---------------|---------------------------------------|
| /nd           | 解除封裝或拆開套件/套件組合時不會執行解密。 |
| /pfn          | 將所有檔案解除封裝/拆開到以套件或套件組合完整名稱命名之指定輸出路徑下的子目錄。 |

下列使用方式範例顯示 **unpack** 和 **unbundle** 命令一些可能的語法選項︰

``` syntax
MakeAppx unpack [options] /p <input package name> /d <output directory>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kf <key file>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kt

MakeAppx unbundle [options] /p <input bundle name> /d <output directory>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kf <key file>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kt
```

下列區塊包含使用 **unpack** 和 **unbundle** 命令的範例︰

``` examples
MakeAppx unpack /v /p MyPackage.msix /d "C:\My Files"
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kt

MakeAppx unbundle /v /p MyBundle.msixbundle /d "C:\My Files"
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kt
```

### <a name="encrypt-or-decrypt-a-package-or-bundle"></a>加密或解密套件或套件組合

**MakeAppx.exe** 工具也可以加密或解密現有的套件或套件組合。 您必須提供套件名稱、輸出套件名稱，以及加密或解密要使用金鑰檔案 (/kf) 或全域測試金鑰 (/kt)。

透過 Visual Studio 封裝精靈無法使用加密和解密。 

**encrypt** 和 **decrypt** 命令專用的選項︰

| **選項**    | **描述**                       |
|---------------|---------------------------------------|
| /ep           | 指定加密的應用程式套件或套件組合。 |

下列使用方式範例顯示 **encrypt** 和 **decrypt** 命令一些可能的語法選項︰

``` syntax
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kf <key file>
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kt

MakeAppx decrypt [options] /ep <package name> /p <output package name> /kf <key file>
MakeAppx decrypt [options] /ep <package name> /p <output package name> /kt
```

下列區塊包含使用 **encrypt** 和 **decrypt** 命令的範例︰

``` examples
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt

MakeAppx.exe decrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe decrypt p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt
```

## <a name="key-files"></a>金鑰檔案

金鑰檔案的開頭必須有一行 "[Keys]" 字串，後面接著要用來加密每個套件的金鑰。 每個金鑰由一組以引號括住的字串表示，以空格或 Tab 鍵分隔。 第一個字串代表 base64 編碼的 32 位元金鑰識別碼，第二個字串代表 base64 編碼的 32 位元加密金鑰。 金鑰檔案必須是簡單的文字檔案。

金鑰檔案的範例：

``` Example
[Keys]
"OWVwSzliRGY1VWt1ODk4N1Q4R2Vqc04zMzIzNnlUREU="    "MjNFTlFhZGRGZEY2YnVxMTBocjd6THdOdk9pZkpvelc="
```

## <a name="mapping-files"></a>對應檔案
對應檔案的開頭必須有一行 "[Files]" 字串，後面接著要加入套件的檔案。 每個檔案由一組以引號括住的路徑表示，以空格或 Tab 鍵分隔。 每個檔案代表其來源 (磁碟上) 與目的地 (套件中)。 對應檔案必須是簡單的文字檔案。

對應檔案 (不含 /m 選項) 的範例︰

``` Example
[Files]
"C:\MyApp\StartPage.html"               "default.html"
"C:\Program Files (x86)\example.txt"    "misc\example.txt"
"\\MyServer\path\icon.png"              "icon.png"
"my app files\readme.txt"               "my app files\readme.txt"
"CustomManifest.xml"                    "AppxManifest.xml"
``` 

使用對應檔案時，您可以選擇是否要使用 /m 選項。 /m 選項可讓使用者指定對應檔案中的資源中繼資料要包含在產生的資訊清單中。 如果您使用 /m 選項，對應檔案就必須包含一個以 "[ResourceMetadata]" 為開頭的區段，後面接著指定 "ResourceDimensions" 和 "ResourceId" 的行。 應用程式套件可能會包含多個 "ResourceDimensions"，但只能有一個 "ResourceId"。

對應檔案 (含 /m 選項) 的範例︰

``` Example
[ResourceMetadata]
"ResourceDimensions"                    "language-en-us"
"ResourceId"                            "English"

[Files]
"images\en-us\logo.png"                 "en-us\logo.png"
"en-us.pri"                             "resources.pri"
```

## <a name="semantic-validation-performed-by-makeappxexe"></a>MakeAppx.exe 所執行的語意式驗證

**MakeAppx.exe** 會執行有限的語意式驗證，這種驗證專門設計來攔截最常見的部署錯誤，並協助確保應用程式套件是有效的。 如果您在使用 **MakeAppx.exe** 時想要略過驗證，請參閱 /nv 選項。 

此驗證可確保︰
- 在套件資訊清單中參考的所有檔案都包含在應用程式套件中。
- 應用程式不會有兩個相同的金鑰。
- 應用程式不會針對此清單中禁止的通訊協定進行註冊︰SMB、FILE、MS-WWA-WEB、MS-WWA。 

這不是完整的語意式驗證，只是設計來攔截常見的錯誤。 透過 **MakeAppx.exe** 建置的套件不保證能夠安裝。
