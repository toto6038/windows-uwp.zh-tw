---
description: 本主題說明 MakePri.exe XML 設定檔的結構描述。
title: MakePri.exe 設定檔
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 5427927322f61f44cf3a8b53112f5f7811520290
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031721"
---
# <a name="makepriexe-configuration-file"></a>MakePri.exe 設定檔

本主題說明 [MakePri.exe](compile-resources-manually-with-makepri.md) XML 設定檔的結構描述；也稱為 PRI 設定檔。 MakePri.exe 工具擁有 [createconfig 命令](makepri-exe-command-options.md#createconfig-command)，您可以用來建立新初始化的 PRI 設定檔。

> [!NOTE]
> 當您在安裝 Windows 軟體開發套件時，檢查 **UWP 受管理應用程式的 Windows SDK** 選項時，會安裝 MakePri.exe。 它會安裝到路徑 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (以及針對其他架構) 命名的資料夾中。 例如： `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`。

PRI 設定檔控制要編製索引的資源以及編制方式。 設定 XML 必須符合下列結構描述。

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- `default` 項目指定應該用於執行階段內容不符合任何資源候選項目時解析資源 (語言、縮放、對比等)。 因為此內容是在建置階段指定的，並且不會變更，所以資源是在建立限定詞時解析為此內容。 相符分數是在建置階段儲存。 每個限定詞必須有一些指定的值。 如需如何選擇資源的詳細資訊，請參閱 [ResourceContext](resource-management-system.md#resourcecontext)。
- `index` 項目定義通過資產完成離散索引傳遞。 每次索引傳遞決定要使用的[格式特定索引子](makepri-exe-format-specific-indexers.md)以及要對其編制索引的資源。
- `qualifiers` 項目設定其他資源繼承的第一個檔案或資料夾的初始限定詞。 每個限定詞項目都必須具有有效的名稱及值 (請參閱[針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md))。
- `root` 屬性是索引傳遞用的實體檔案的路徑根目錄。 它可以是相對或絕對的。 若是相對，它會附加到您在命令列中提供的專案根目錄。 若是絕對，它是直接用作索引傳遞根目錄。 可接受反斜線或正斜線。 結尾斜線會被修剪。 索引傳遞的根目錄決定所有資源都被視為相對的資料夾。
- `startIndexAt` 屬性是用於編製索引的初始種子檔案或資料夾。 它是相對於索引傳遞根目錄。 空值會假設索引傳遞根資料夾。

## <a name="default-pri-config-file"></a>預設 PRI 設定檔

MakePri.exe 會在發出 [createconfig 命令](makepri-exe-command-options.md#createconfig-command)時產生這個新初始化的 PRI 設定檔。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>封裝項目

`packaging` 項目定義 PRI 分段資訊。 `packaging` 項目的結構描述會針對自動 (支援 `autoResourcePackage` 以及特定維度) 和手動設定定義。

此範例顯示如何使用 `autoResourcePackage` 以及特定維度。

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

此範例顯示如何使用手動 `resourcePackage`。

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe 不會明確封鎖資源 PRI 檔案和任何特定維度的產生。 限制和特定組的維度是由 MakeAppx.exe 或管線中的其他工具來定義和實作。

MakePri.exe 會剖析所有 `index` 節點之後的 `packaging` 項目，以填入限定詞。 MakePri.exe 會收集這些資料結構的已剖析資訊。

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>resources@isDeploymentMergeable 屬性

此屬性會在產生的 PRI 檔案中設定旗標

- 部署合併以識別此 PRI 檔案可以合併。
- GetFullyQualifiedReference 以傳回錯誤，以防設定此旗標並且資源管理員已初始化檔案。

此屬性的預設值為 `true`。 如果您的目標是 Windows 10，MakePri.exe 只在 PRI 中設定旗標。

如果您的目標是 Windows 10，建議您對資源套件的建立省略 `isDeploymentMergeable` (或將其明確設定為 `true`)。

如果 `makepri dump` 以 `/dt detailed` 選項執行，MakePri.exe 會新增 `isDeploymentMergeable` 的價值到傾印檔案。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>resources@majorVersion 屬性

此屬性的預設值是 1。 如果您提供明確的值，並且您也對 MakePri.exe 工具使用已過時的 `/VersionMajor(vma)` 命令列，則設定檔中的值取得優先。

以下為範例。

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>resources@targetOsVersion 屬性

表示目標作業系統的版本。 下表顯示支援的值；預設值是 6.3.0。

| 值 | 意義 |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (預設值) | Windows 8.1 |
| 6.2.1 | Windows 8 |

以下為範例。

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**注意** 就 PRI 檔案而言，Windows 是回溯相容；但不是一直都正向相容。

如果 `makepri dump` 以 `/dt detailed` 選項執行，MakePri.exe 會新增 `targetOsVersion` 的價值到傾印檔案。

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>驗證錯誤訊息

以下是一些範例錯誤條件和對應的錯誤訊息。

| 條件 | Severity | 訊息 |
| --------- | -------- | ------- |
| 指定 targetOsVersion 以外的其中一個支援的值。 | 錯誤 | 無效的設定：指定了無效的 targetOsVersion。 |
| 指定「6.2.1」的 targetOsVersion 並且 `packaging` 項目不存在。 | 錯誤 | 無效的設定：此 targetOsVersion 不支援「Packaging」節點。 |
| 在設定中找到以個以上的模式。 例如，指定 Manual 與 AutoResourcePackage。 | 錯誤 | 無效的設定：「Packaging」節點不可擁有一個以上的操作模式。 |
| 預設限定詞列在資源套件底下。 | 錯誤 | 無效的設定：<Qualifiername>=<QualifierValue> 是預設限定詞且其候選項目無法新增至資源套件。 |
| AutoResourcePackage 限定詞包含多個限定詞。 例如，language_scale。 | 錯誤 | 無效的設定：不支援含有多個限定詞的 AutoResourcePackage。 |
| ResourcePackage QualifierSet 包含多個限定詞。 例如，language-en-us_scale-100 | 錯誤 | 無效的設定：不支援含有多個限定詞的 QualifierSet。 |
| 找到重複 resourcepack 名稱。 | 錯誤 | 無效的設定：重複的資源套件名稱 <rpname>。 |
| 在兩個資源套件中定義了相同的限定詞組。 | 錯誤 | 無效的設定：找到多個執行個體的 QualifierSet「<qualifier tags>」。 |
| 「ResourcePackage」節點找不到列出的 QualifierSet 的候選項目。 | 警告 | 無效的設定：找不到 <Resource Package Name> 的候選項目。 |
| 「AutoResourcePackage」節點下找不到列出的限定詞的候選項目。 | 警告 | 無效的設定：找不到限定詞 <qualifier name> 的候選項目。 未產生資源套件。 |
| 找不到任何模式。 也就是找到空的「packaging」節點中。 | 警告 | 無效的設定：未指定任何封裝模式。 |

## <a name="related-topics"></a>相關主題

* [使用 MakePri.exe 來手動編譯資源](compile-resources-manually-with-makepri.md)
* [MakePri.exe 命令列選項 &mdash; createconfig 命令](makepri-exe-command-options.md#createconfig-command)
* [針對語言、縮放比例、高對比及其他限定詞量身打造您的資源](tailor-resources-lang-scale-contrast.md)
* [資源管理系統 &mdash; windows.applicationmodel.resources.core.resourcecoNtext](resource-management-system.md#resourcecontext)
