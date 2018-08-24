---
author: stevewhims
Description: MakePri.exe has the set of commands createconfig, dump, new, resourcepack, and versioned. This topic details their use.
title: MakePri.exe 命令列選項
template: detail.hbs
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: c0a3892348baff56bbef8d40dd9aade4e612c50d
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2018
ms.locfileid: "2843629"
---
# <a name="makepriexe-command-line-options"></a>MakePri.exe 命令列選項

[MakePri.exe](compile-resources-manually-with-makepri.md) 擁有命令集 `createconfig`、`dump`、`new`、`resourcepack` 和 `versioned`。 本主題詳述命令列選項的使用。

> [!NOTE]
> 當您檢查安裝 Windows 軟體開發套件時的 [ **Windows sdk （英文） UWP 受管理的應用程式**] 選項安裝 MakePri.exe。 安裝路徑`%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe`（也在名為其他架構] 資料夾中）。 例如，`C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`。

## <a name="makepri-commands"></a>MakePri 命令

執行 `MakePri.exe help` 以查看您可以搭配 MakePri.exe 使用的命令。

```
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Createconfig 命令

`createconfig` 命令會建立的新初始化的 PRI 設定檔，定義您指定的限定詞預設值。 執行 `MakePri.exe createconfig /?` 以查看此命令的詳細說明。

```
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Dump 命令

`dump` 命令會輸出傾印的 xml 檔案，包含指定 PRI 檔案中的所有資源的清單。 執行 `MakePri.exe dump /?` 以查看此命令的詳細說明。

> [!NOTE]
> 無結構描述資源套件是在 PRI 設定檔中使用 *omitSchemaFromResourcePacks* 參數所建立的套件。 若要傾印無結構描述資源套件，請使用 `/es <main_package_PRI_file>` 參數。 如果您沒有指定主要檔案，就會收到錯誤訊息「*套件中的 resources.pri 已損毀，因此加密失敗 (錯誤 PRI222: 0xdef0000f - 發生未指定的錯誤)*」。

```
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>New 命令

`new` 命令會依照您的設定檔的指示，在專案中編製檔案的索引，以建立新的 PRI 檔案。 執行 `MakePri.exe new /?` 以查看此命令的詳細說明。

```
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>ResourcePack 命令

`resourcepack` 命令會依照您的設定檔的指示，在專案中編製檔案的索引，以建立新的 PRI 檔案。 資源套件 PRI 檔案只包含已經在現有 PRI 檔案中指定的資源的其他變體。 執行 `MakePri.exe resourcepack /?` 以查看此命令的詳細說明。

```
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Versioned 命令

`versioned` 命令會依照您的設定檔的指示，在專案中編製檔案的索引，以建立已版本化的 PRI 檔案。 執行 `MakePri.exe versioned /?` 以查看此命令的詳細說明。

```
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll(ex)

您使用擴充 DLL 選項 (/ex) 搭配 `createconfig`、`dump`、`new`、`resourcepack` 和 `versioned`，來指定資源管理系統環境擴充 DLL 的位置。

## <a name="logging47metadata-file"></a>登入&#47中繼資料檔

MakePri 可以在索引子中繼資料檔案中包含資源套件特定的資訊。 以下是`resources.pri` 的記錄檔範例 ，其具有資源 PRI 檔案 `german.pri` 和 `highresolution.pri`。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>&#47;IndexFile(if) 選項

您使用索引檔案選項 (/if) 搭配 `dump`、`resourcepack` 和 `versioned` 來指定輸入的 PRI 檔案。

對於 `resourcepack` 與 `versioned`，不提供 PRI 檔案做為 IndexFile(if) 的輸入參數，可以改為提供結構描述檔案。

```
/IndexFile(if) <FILEPATH>
```

**FILEPATH** 是指定輸入 PRI 檔案或 PRI 結構描述檔案的位置的權杖。

## <a name="47mappingfilemf-option"></a>&#47;MappingFile(mf) 選項

您使用的對應檔案選項 (/mf) 搭配 `new`、`resourcepack` 和 `versioned` 來產生對應檔案。 [MakeAppx.exe](../packaging/create-app-package-with-makeappx-tool.md) 使用對應檔案來產生應用程式套件。

```
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** 是指定對應檔案格式的權杖。 目前僅支援 `appx` 格式。

```
/mf appx
```

這是主要對應檔案的範例內容。

```
"ResourceDimensions"                   "language-de-de"
```

並且這是資源套件對應檔案的範例內容。

```
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>輸出摘要

如果建立資源套件，MakePRI.exe 的輸出摘要是更多詳細資訊的形式。 範例如下。

```
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>&#47;Overwrite(o) 選項

如果未提供覆寫選項 (/o)，並且指定的輸出檔案已存在，則 MakePri.exe 需要在覆寫之前進行確認。

```
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>&#47;OutputFile(of) 選項

您使用輸出檔案選項 (/of) 搭配 `dump`、`new`、`resourcepack` 和 `versioned`，來指定輸出位置和要產生之 PRI 檔案的名稱。 如果 MakePri.exe 產生一個以上的資源 PRI 檔案，將其放入目標檔案的上層資料夾中。 例如，若您指定 `/of MyParentFolder\TargetFile.pri`，然後 MakePri.exe 在 `ParentFolder` 底下產生 `TargetFile.language-en.pri` 和 `TargetFile.scale-100.pri` 以及 `TargetFile.pri`。

以下是範例錯誤條件和對應的錯誤訊息。

| 錯誤條件 | 錯誤訊息 |
| --------------- | ------------- |
| 輸出檔案名稱與設定中的其中一個資源套件名稱相同。 | 無效的設定：資源套件名稱 <resource pack name> 不可與輸出檔案 < outputfilename.pri > 相同。 |

## <a name="reversemaprm-option"></a>/ReverseMap(rm) 選項

您使用反向對應選項 (/rm) 搭配 `new`、`resourcepack` 和 `versioned`，在可以用於偵錯的 PRI 檔案中產生反向對應區段。

## <a name="47schemafilesf-option"></a>&#47;SchemaFile(sf) 選項

您使用結構描述檔案選項 (/sf) 搭配 `new`、`resourcepack` 和 `versioned`，在指定的位置寫入結構描述檔案。

對於 `resourcepack` 與 `versioned`，不提供 PRI 檔案做為 IndexFile(if) 的輸入參數，可以改為提供結構描述檔案。

```
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** 是指定寫入結構描述檔案的位置的權杖。

這是結構描述檔案的範例。

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor(vma) 已過時

主要版本 (/vma) 選項 (適用於 `new` 命令) 已過時，使用它會導致出現此警告訊息。

```
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

若要提供主要版本號碼，請在您的設定檔中使用 [resources@majorVersion](makepri-exe-configuration.md) 屬性。

## <a name="related-topics"></a>相關主題

* [MakePri.exe](compile-resources-manually-with-makepri.md)
