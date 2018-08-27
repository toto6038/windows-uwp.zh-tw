---
author: stevewhims
Description: This topic describes the format-specific indexers used by the MakePri.exe tool to generate its index of resources.
title: MakePri.exe 格式特定的索引子
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 8ec6b2a31f4f577de30dac1c96a411c6aee6e9dc
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "2858397"
---
# <a name="makepriexe-format-specific-indexers"></a>MakePri.exe 格式特定的索引子

本主題說明  [MakePri.exe](compile-resources-manually-with-makepri.md) 工具用來產生資源索引的格式特定索引子。

> [!NOTE]
> 當您檢查安裝 Windows 軟體開發套件時的 [ **Windows sdk （英文） UWP 受管理的應用程式**] 選項安裝 MakePri.exe。 安裝路徑`%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe`（也在名為其他架構] 資料夾中）。 例如，`C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`。

MakePri.exe 通常搭配 `new`、`versioned` 或 `resourcepack` 命令使用。 請參閱 [MakePri.exe 命令列選項](makepri-exe-command-options.md)。 在這些案例中，它會編制來源檔案的索引來產生資源的索引。 MakePri.exe 使用各種個別索引子來讀取不同來源的資源檔案或資源的容器。 最簡單的索引子是資料夾索引子，它會為諸如 `.jpg` 或 `.png` 影像等編制資料夾內容的索引。

您可透過在 [MakePri.exe 設定檔](makepri-exe-configuration.md) 的 `<index>` 項目內指定 `<indexer-config>` 項目，來識別格式特定索引子。 `type` 屬性會辨識所使用的格式特定索引子。

在編製索引期間遇到的資源容器通常是使其內容編好索引而非新增至索引本身。 例如，資料夾索引子找到的 `.resjson` 檔案可能進一步由 `.resjson` 索引子編制索引，此時 `.resjson` 檔案本身不會顯示在索引中。 **注意** 與該容器關聯之索引子的 `<indexer-config>` 項目必須包含在設定檔內才能使其發生。

通常，在包含實體 (例如資料夾或 `.resw` 檔案) 上找到的限定詞會套用到其中的所有資源，例如資料夾中的檔案或 `.resw` 檔案中的字串。

## <a name="folder"></a>資料夾

資料夾索引子可透過 FOLDER 的 `type` 屬性來識別。 它會編制資料夾內容的索引，並判定資料夾和檔案名稱的資源限定詞。 其設定項目符合下列結構描述。

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

`qualifierDelimiter` 屬性指定在檔名 (略過副檔名) 中指定限定詞之後的字元。 預設是「.」。

## <a name="pri"></a>PRI

PRI 索引子可透過 PRI 的 `type` 屬性來識別。 它會編制 PRI 檔案內容的索引。 您通常是在將其他組件、DLL、SDK 或類別庫內所包含的資源編制索引到 App 的 PRI 中時使用它。

PRI 檔案中包含的所有資源名稱、限定詞和值都直接保留在新的 PRI 檔案中。 但是，最上層資源對應並不會保留在最終 PRI 中。 資源對應會合併。

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

PriInfo 索引子可透過 PRIINFO 的 `type` 屬性來識別。 它會編制詳細傾印檔案內容的索引。 您透過使用 `/dt detailed` 選項執行 `makepri dump` 來產生詳細的傾印檔案。 索引子的設定項目符合下列結構描述。

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

這個設定項目允許選擇性的屬性設定 PriInfo 索引子的行為。 `emitStrings` 和 `emitPaths` 的預設值是 `true`。 如果 `emitStrings` 是 `true`，則 `type` 屬性設定為「String」的資源候選項目會被納入索引中，否則它們會被排除。 如果「emitPaths」是 `true`，則 `type` 屬性設定為「Path」的資源候選項目會被納入索引中，否則它們會被排除。

以下是包括 String 資源類型，但略過 Path 資源類型的範例設定。

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

若要編制索引，傾印檔案的結尾必須是延伸 `.pri.xml`，而且必須符合下列結構描述。

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

MakePri.exe 支援傾印類型「Basic」、「Detailed」、「Schema」及「Summary」。 若要設定 MakePri.exe 為發出 PriInfo 索引子可以讀取的傾印類型，請在使用 `dump` 命令時包括「/DumpType Detailed」。

MakePri.exe 會略過 `.pri.xml` 檔案的幾個項目。 這些項目不是在編製索引期間運算，就是在 MakePri.exe 設定檔中指定。 傾印檔案內包含的資源名稱、限定詞和值都直接保留在 PRI 檔案中。 但是，最上層資源對應並不會保留在最終 PRI 中。 資源對應會合併為編製索引的一部分。

這是傾印檔案的有效 String 類型資源的範例。

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

這是傾印檔案的有效 Path 類型資源和兩個候選項目的範例。

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

ResFiles 索引子可透過 RESFILES 的 `type` 屬性來識別。 它會編制 `.resfiles` 檔案內容的索引。 其設定項目符合下列結構描述。

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resfiles` 檔案是包含一般檔案路徑清單的純文字檔。 `.resfiles` 檔案可包含「//」註解。 範例如下。

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

ResJSON 索引子可透過 RESJSON 的 `type` 屬性來識別。 它會編制 `.resjson` 檔案內容的索引，此檔案為字串資源檔案。 其設定項目符合下列結構描述。

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resjson` 檔案包含 JSON 文字 (請參閱 [JavaScript 物件標記法 (JSON) 的 application/json 媒體類型](http://www.ietf.org/rfc/rfc4627.txt))。 檔案必須包含單一 JSON 物件並包含階層式屬性。 每個屬性必須是另一個 JSON 物件或字串值。

名稱開頭為底線 (「_」) 的 JSON 屬性不會編譯到最終 PRI 檔案中，但會保留在記錄檔中。

檔案也可能包含「//」註解，這在剖析期間會被忽略。

`initialPath` 屬性會將所有資源放在這個初始路徑下，方式是在資源的名稱前加上它。 您通常是在編製類別庫資源的索引使用此項。 預設值是 blank。

## <a name="resw"></a>ResW

ResW 索引子可透過 RESW 的 `type` 屬性來識別。 它會編制 `.resw` 檔案內容的索引，此檔案為字串資源檔案。 其設定項目符合下列結構描述。

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resw` 檔案是 XML 檔案，符合下列結構描述。

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

`convertDotsToSlashes` 屬性會轉換資源名稱 (資料項目名稱屬性) 中找到的所有點 (「.」) 字元為正斜線「/」，除非點字元是在「[」和「]」之間。

`initialPath` 屬性會將所有資源放在這個初始路徑下，方式是在資源的名稱前加上它。 您通常是在編製類別庫資源的索引使用此項。 預設值是 blank。

## <a name="related-topics"></a>相關主題

* [手動以 MakePri.exe 編譯資源](compile-resources-manually-with-makepri.md)
* [MakePri.exe 命令列選項](makepri-exe-command-options.md)
* [MakePri.exe 設定檔](makepri-exe-configuration.md)
* [JavaScript 物件標記法 (JSON) 的 application/json 媒體類型](http://www.ietf.org/rfc/rfc4627.txt)