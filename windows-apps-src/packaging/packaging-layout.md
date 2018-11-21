---
author: laurenhughes
title: 使用封裝配置的套件建立
description: 封裝配置是一份描述應用程式封裝結構的文件。 它會指定應用程式套件組合（主要和選用）、套件組合中的套件，和套件中的檔案。
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, 封裝, 套件配置, 資產套件
ms.localizationpriority: medium
ms.openlocfilehash: 9342b4ce35cb50037813ed2210e2d7246411ad92
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/21/2018
ms.locfileid: "7440241"
---
# <a name="package-creation-with-the-packaging-layout"></a>使用封裝配置的套件建立  

資產套件引入後，開發人員現在可以有工具，除了建置更多套件類型，還能建置更多套件。 當應用程式變得更大、更複雜，通常會包含更多套件，管理這些套件的難度會增加（尤其是您在 Visual Studio 以外建置並使用對應檔案）。 若要簡化應用程式的封裝結構管理，您可以使用 MakeAppx.exe 支援的封裝配置。 

封裝配置是一份描述應用程式封裝結構的 XML 文件。 它會指定應用程式套件組合（主要和選用）、套件組合中的套件，和套件中的檔案。 從不同的資料夾、磁碟機和網路位置，可以選取檔案。 選取或排除檔案，可以使用萬用字元。

應用程式封裝配置設定之後，在單一命令列呼叫中搭配 MakeAppx.exe 建立應用程式的所有套件。 封裝配置可以編輯修改套件結構，以符合您的部署需要。 


## <a name="simple-packaging-layout-example"></a>簡單封裝配置範例

簡單封裝配置範例看起來如下：

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

讓我們分解此範例，了解它的運作方式。

### <a name="packagefamily"></a>PackageFamily
此封裝配置會建立單一的一般應用程式套件組合檔案具有 x64 架構套件和 「 媒體 」 資產套件。 

**PackageFamily** 元素用來定義應用程式套件組合。 您必須使用**ManifestPath**屬性提供套件組合的**AppxManifest**，**AppxManifest**應與對應於的套件組合架構套件的**AppxManifest**。 **ID**屬性必須同時提供。 套件建立期間這可搭配 MakeAppx.exe，您可以只建立此套件（如果您想要）而且這將會是所產生套件的檔案名稱。 **FlatBundle**屬性用來描述您想要建立何種類型的套件組合，**true**表示一般套件組合 (請閱讀本文了解詳細資訊)，**false**表示傳統套件組合。 **ResourceManager**屬性用來指定此套件組合中的資源套件是否使用 MRT 才能存取檔案。 預設是**true**，但是在 Windows 10（版本 1803），這尚未準備好，所以此屬性必須設為**false**。


### <a name="package-and-assetpackage"></a>Package 和 AssetPackage
在**PackageFamily**中，會定義應用程式套件組合包含或參考的套件。 這裡，架構套件（也稱為主要封裝）使用**Package**元素定義的，而資產套件使用**AssetPackage**元素定義的。 架構套件必須指定套件適用的架構，「x64」、「x86」、「arm」或「neutral」其中一項。 （選擇性）也可以再次使用**ManifestPath**屬性，專為此套件提供**AppxManifest**。 如果不提供**AppxManifest**，系統會自動建立一個，從提供給**PackageFamily**的**AppxManifest**。 

根據預設，為套件組合中的每個套件產生**AppxManifest**。 對於資產套件，您也可以設定**AllowExecution**屬性。 將此設定為**false**（預設值），可協助降低應用程式發佈時間，因為不需要執行的套件不會被病毒掃描封鎖發佈程序 (您可以在[資產套件簡介](asset-packages.md)深入了解這點)。 

### <a name="files"></a>Files
在每個套件定義內，您可以使用**File**元素選取要包含在此套件中的檔案。 **SourcePath**屬性是檔案的本機位置。 您可以從不同資料夾（藉由提供相對路徑）、不同磁碟機（藉由提供絕對路徑）或甚至網路共用 (藉由像是提供`\\myshare\myapp\*`)選取檔案。 **DestinationPath**是套件中檔案的目的地位置，相對於套件根目錄。 **ExcludePath**（而不是其他兩個屬性）可以用來選擇要排除的檔案，從相同套件中其他**File**元素的**SourcePath**屬性所選取的檔案中。

使用萬用字元，每個**File**元素可用於來選取多個檔案。 一般而言，單一萬用字元 (`*`) 可用於路徑內任何地方任何次數。 不過，單一萬用字元只符合資料夾中的檔案，不符合任何子資料夾。 例如`C:\MyGame\*\*`適用於**SourcePath**以選取檔案`C:\MyGame\Audios\UI.mp3`和`C:\MyGame\Videos\intro.mp4`，但它無法選取`C:\MyGame\Audios\Level1\warp.mp3`。 雙重萬用字元 (`**`) 也可以用來取代資料夾或檔案名稱，以遞迴比對任何項目（但無法在部分名稱旁邊）。 例如，`C:\MyGame\**\Level1\**`可以選取 `C:\MyGame\Audios\Level1\warp.mp3`和`C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`。 也可以使用萬用字元直接變更檔名，做為封裝程序的一部分，如果在來源和目的地之間的不同位置使用萬用字元。 例如，**SourcePath**為`C:\MyGame\Audios\*`和**DestinationPath**為`Sound\copy_*`，可以選取 `C:\MyGame\Audios\UI.mp3`，使其在套件中顯示為`Sound\copy_UI.mp3`。 一般而言，單一萬用字元和雙重萬用字元的數目必須與單一**File**元素的**SourcePath**和**DestinationPath**是相同。


## <a name="advanced-packaging-layout-example"></a>進階封裝配置範例

以下是更加複雜的封裝配置範例：

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

此範例與簡單範例的差異在於加上**ResourcePackage**和**Optional**元素。

您可以使用**ResourcePackage**元素指定資源套件。 在**ResourcePackage**內，**Resources**元素必須用來指定資源套件的資源限定元。 資源限定元是資源套件支援的資源，這裡，我們可以看到定義了兩個資源套件，各別包含英文和法文特定的檔案。 資源套件可以有多個限定元，可透過在**Resources**內新增另一個**Resource**元素來完成這個動作。 也必須指定資源維度的預設資源，如果維度存在（維度是 language、scale、dxfl）。 這裡，我們會看見英文是預設語言，這表示，不需要設定法文系統語言的使用者，將改為下載英文資源套件並以英文顯示。


選用套件有自己不同的套件系列名稱，必須使用**PackageFamily**元素定義，同時將**Optional**屬性指定為**true**。 **RelatedSet**屬性用來指定選用套件是否在相關集合中（預設是 true）– 選用套件是否應該使用主要套件更新。

**PrebuiltPackage**元素用來新增至包含或參考在要建置的應用程式套件組合檔案中的封裝配置中未定義的套件。 在此情況下，另一個 DLC 選用套件包含以下以便主要的應用程式套件組合檔案可以參考它，並讓它成為相關集合的一部分。


## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>使用封裝配置和 MakeAppx.exe 建置應用程式套件
一旦您的應用程式有封裝配置，您可以開始使用 MakeAppx.exe 建置應用程式的套件。 若要建置封裝配置中定義的所有套件，使用命令：

``` example 
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

不過，如果您要更新您的應用程式，並某些套件不包含任何變更的檔案，您可以只建置已變更的套件。 使用這個頁面上的簡單封裝配置範例和建置 x64 架構套件，我們的命令看起來如下：

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

`/id`旗標可以用來從封裝配置選取要建置的套件，對應於配置中的**ID**屬性。 `/ip`用來表示上一版套件此時的位置。 因為應用程式套件組合檔案仍需要來參考舊版的**媒體**套件，則必須提供先前的版本。 `/iv`旗標用來自動增加建置套件版本 (而不在**AppxManifest**中變更版本)。 或者，參數`/pv`與`/bv`可分別用於直接提供套件版本（適用於所有要建立的套件）及套件組合版本（適用於所有要建立的套件組合）。
使用此頁面上的進階封裝配置範例，若只要建置**Themes**選用套件組合和它所參考的**Themes.main**應用程式套件，請使用此命令：

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

`/bc`旗標用來表示**Themes**套件組合的子系也應該建置 (在這種情形下將建置**Themes.main**)。 `/nbp`旗標用來表示**Themes**套件組合的父系不應該建置。 **Themes**的父系是選用應用程式套件組合，為主要應用程式套件組合：**MyGame**。 通常對於相關集合中的選用套件，主要應用程式套件組合也必須建置，才能安裝選用套件，因為當選用套件在相關集合中時，主要應用程式套件組合中也會參照選用套件 (以確保主要和選用套件之間的版本)。 下圖說明這些套件之間的父子關係：

![封裝配置圖表](images/packaging-layout.png)
