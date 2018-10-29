---
author: stevewhims
Description: In this scenario, we'll make a new app to represent our custom build system. We'll create a resource indexer and add strings and other kinds of resources to it. Then we'll generate and dump a PRI file.
title: 案例 1：從字串資源和資產檔案建立 PRI 檔案
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 7555f4a61f7798fa32d137928cde8c042a7fcdfc
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5742969"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>案例 1：從字串資源和資產檔案建立 PRI 檔案
在本案例中，我們會使用[套件資源索引 (PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)，建立新的應用程式來表示我們的自訂建置系統。 請記住，這個自訂建置系統的目的是建立適用於目標 UWP app 的 PRI 檔案。 因此在這個逐步解說過程中，我們會建立一些範例資源檔案 (包含字串以及其他種類的資源) 來代表該目標 UWP app 的資源。

## <a name="new-project"></a>新專案
在 Microsoft Visual Studio 中，藉由建立新的專案來開始。 建立 **Visual C++ Windows 主控台應用程式**專案，命名為 *CBSConsoleApp* (代表「自訂建置系統主控台應用程式」)。

從 **\[方案平台\]** 下拉式清單選擇 *x64*。

## <a name="headers-static-library-and-dll"></a>標頭、靜態程式庫和 dll
在 MrmResourceIndexer.h 標頭檔案 (已安裝到 `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\`) 中宣告 PRI API。 開啟 `CBSConsoleApp.cpp` 檔案，並將此標頭以及一些您所需的其他標頭加入其中。

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

API 是在 MrmSupport.dll 中實作，您可以連結到靜態程式庫 MrmSupport.lib 來存取。 開啟專案的 **\[屬性\]**、按一下 **[連結器]** > **\[輸入\]**、編輯 **AdditionalDependencies**，然後新增 `MrmSupport.lib`。

建置方案，然後將 `MrmSupport.dll` 從 `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` 複製到您的組建輸出資料夾 (可能是 `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`)。

將下列 Helper 函式新增至 `CBSConsoleApp.cpp`，我們將要用到。

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

在 `main()` 函式中加入初始化和解除初始化 COM 的呼叫。

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>屬於目標 UWP app 的資源檔案
我們現在需要一些範例資源檔案 (包含字串以及其他種類的資源) 來代表該目標 UWP app 的資源。 當然，這些檔案可以位於您的檔案系統的任何位置。 但為了方便進行此逐步解說，我們將這些檔案放在 CBSConsoleApp 的專案資料夾，讓所有內容集中於一處。 您只需要將這些資源檔案新增至檔案系統，而不要新增至 CBSConsoleApp 專案。

在包含 `CBSConsoleApp.vcxproj` 的同一個資料夾中，加入新的子資料夾，名為 `UWPAppProjectRootFolder`。 在這個新的子資料夾中，建立這些範例資源檔案。

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
這個檔案可以包含任何 PNG 影像。

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>建立資源的索引，然後建立 PRI 檔案
在 `main()` 函式中初始化 COM 的呼叫之前，宣告一些我們需要的字串，並且也建立輸出資料夾，我們會在其中產生 PRI 檔案。

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

緊接初始化 COM 的呼叫之後，宣告資源索引子控制代碼，然後呼叫 [**MrmCreateResourceIndexer**]() 以建立資源索引子。

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

以下說明傳遞給 **MrmCreateResourceIndexer** 的引數。

- 目標 UWP app 的套件系列名稱，這會在我們稍後從此資源索引子產生 PRI 檔案時做為資源對應名稱。
- 目標 UWP app 的專案根目錄。 也就是，資源檔案的路徑。 我們這樣指定是為了可以在對相同資源索引子的後續 API 呼叫中，接著指定相對於該根目錄的路徑。
- 要做為目標之 Windows 的版本。
- 預設資源限定詞的清單。
- 可供函式設定之資源索引子控制代碼的指標。

下一個步驟是將我們的資源新增至剛建立的資源索引子。 `resources.resw` 是包含目標 UWP app 中性字串的資源檔案 (.resw)。 如果您想要查看其內容，請向上捲動 (在本主題中)。 `de-DE\resources.resw` 包含德文字串，而 `en-US\resources.resw` 包含英文字串。 為了將資源檔案中的字串資源新增至資源索引子，您呼叫 [**MrmIndexResourceContainerAutoQualifiers**]()。 第三，我們對包含中性影像資源的檔案呼叫資源索引子的 [**MrmIndexFile**]() 函式。

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

在對 **MrmIndexFile** 的呼叫中，"ms-resource:///Files/sample-image.png" 這個值是資源 URI。 第一個路徑區段是「Files」，而就是在我們稍後從此資源索引子產生 PRI 檔案時做為資源對應樹狀子目錄名稱的部分。

簡單介紹資源檔案的資源索引子之後，這就呼叫 [**MrmCreateResourceFile**]() 函式，讓它在磁碟上為我們產生 PRI 檔案。

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

此時，名為 `Generated PRIs` 資料夾中已建立名為 `resources.pri` 的 PRI 檔案。 現在已完成資源索引子，我們會呼叫 [**MrmDestroyIndexerAndMessages**]() 來終結其控制代碼，並釋放所配置的任何電腦資源。

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

由於 PRI 檔案是二進位檔，如果我們將二進位 PRI 檔案傾印成其對應的 XML 檔案，就更容檢視我們剛才產生的內容。 呼叫 [**MrmDumpPriFile**]() 所做的就是這個工作。

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

以下說明傳遞給 **MrmDumpPriFile** 的引數。

- 要傾印的 PRI 檔案的路徑。 我們在此呼叫中不使用資源索引子 (只是將其終結)，因此必須指定完整檔案路徑。
- 沒有結構描述檔案。 我們稍後會在主題中討論什麼是結構描述。
- 只是基本資訊。
- 要建立的 XML 檔案的路徑。

這是在此處傾印為 XML 的 PRI 檔案所包含的內容。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

資訊的開頭是資源對應，這是以目標 UWP app 的套件系列名稱來命名。 資源對應所封入的是兩個資源對應樹狀子目錄：一個用於我們建立索引的檔案資源，另一個用於字串資源。 注意套件系列名稱是如何插入所有的 URI 資源中。

第一個字串資源是 `en-US\resources.resw` 的 *EnOnlyString*，而這只有一個候選項目 (符合 *language-en-US* 限定詞)。 接下來是 `resources.resw` 和 `en-US\resources.resw` 中都有的 *LocalizedString1*。 因此會有兩個候選項目：一個符合 *language-en-US* 的候選項目，以及一個符合任何內部的遞補中性候選項目。 同樣地，*LocalizedString2* 也有兩個候選項目：*language-de-DE* 和中性候選項目。 最後，*NeutralOnlyString* 只有中性形式。 我指定這個名稱給它，是為了釐清這並不是要將它當地語系化。

## <a name="summary"></a>摘要
在本案例中，我們示範了如何使用[套件資源索引 (PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690) 來建立資源索引子。 我們將字串資源和資產檔案新增至資源索引子， 然後使用資源索引子產生二進位 PRI 檔案。 最後，我以 XML 格式傾印二進位 PRI 檔案，這樣才能確認其中包含我們所預期的資訊。

## <a name="important-apis"></a>重要 API
* [套件資源索引 (PRI) 參考](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>相關主題
* [套件資源索引 (PRI) API 和自訂建置系統](pri-apis-custom-build-systems.md)
