---
author: laurenhughes
title: 資產套件簡介
description: 資產套件是一種套件，做為應用程式的常見檔案的集中位置 – 有效免除架構套件中的重複檔案。
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, 封裝, 套件配置, 資產套件
ms.localizationpriority: medium
ms.openlocfilehash: 8aafac1c1217ce082cd9d6176c530967f32e4cdd
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2018
ms.locfileid: "4743926"
---
# <a name="introduction-to-asset-packages"></a>資產套件簡介

> [!IMPORTANT]
> 如果您想要提交應用程式至 Microsoft Store，您需要連絡[Windows 開發人員支援](https://developer.microsoft.com/windows/support)，並取得核准使用資產套件。

資產套件是一種套件，做為應用程式的常見檔案的集中位置 – 有效免除架構套件中的重複檔案。 資產套件類似於資源套件，兩者設計為包含應用程式執行時所需的靜態內容，但是差異在於所有資產套件永遠都會下載，無論使用者的系統架構、語言或顯示縮放比例為何。

![資產套件組合圖表](images/primary-bundle.png)

因為資產套件包含所有架構、語言和縮放無關檔案，運用資產套件會造成整體的封裝應用程式大小減少（因為這些檔案不再複製），協助您管理大型應用程式的本機開發磁碟空間使用和管理一般應用程式的套件。 

### <a name="how-do-asset-packages-affect-publishing"></a>資產套件如何影響資產？
資產套件的最明顯優點是減少封裝應用程式的大小。 較小的應用程式套件會透過讓 Microsoft Store 處理較少的檔案，加快應用程式的發佈程序，但是這不是最重要的資產套件好處。

建立資產套件時，您可以指定是否應該允許執行套件。 因為資產套件應只包含架構無關檔案，它們通常不包含任何 .dll 或 .exe 檔案，因此通常不需要執行資產套件。 此區別的重要性是在發佈過程中，所有的可執行檔套件都必須掃描，確保不包含惡意程式碼，而對於較大的套件此掃描程序需要較長的時間。 不過，如果套件指定為不可執行，應用程式安裝將確保，無法執行此套件中所包含的檔案。 這項保證免除完整套件掃描，在應用程式發佈期間 (以及更新) 會大幅降低惡意程式碼掃描時間 - 因此使用資產套件的應用程式，發佈速度大幅提升。 請注意，[一般套件組合應用程式套件](flat-bundles.md)必須也用來取得此發佈好處，因為這可讓 microsoft Store 處理每個.appx 或.msix 套件檔案，以並行方式。 


### <a name="should-i-use-asset-packages"></a>我應該使用資產套件嗎？
更新應用程式的檔案結構以運用資產套件，可以產生優點：縮小套件大小和更精簡的開發反覆運算。 如果您所有的架構套件都包含大量的通用檔案或應用程式的大部分是由不可執行的檔案所組成，我們建議您投資額外的時間轉換為使用資產套件。

不過請注意，資產套件不是達成應用程式內容選擇性的一種方法。 資產套件檔案為非選擇性，並將**永遠**下載，無論目標裝置的架構、語言或縮放比例 - 您希望應用程式支援的任何選擇性內容，都應使用[選用套件](optional-packages.md)來實作。 


### <a name="how-to-create-an-asset-package"></a>如何建立資產套件
建立資產套件的最簡單方式是使用封裝配置。 不過，資產套件也可以透過使用 MakeAppx.exe 以手動方式建立。 若要指定資產套件中包含哪些檔案，您必須建立「對應檔案」。 在此範例中，在資產套件的唯一檔案是「Video.mp4」，但所有資產套件的檔案都應列在此處。 請注意，資產套件的**ResourceMetadata**中的**ResourceDimensions**規範已省略（相較於資源套件的對應檔案）。

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

使用 MakeAppx.exe，使用這個命令來建立資產套件： 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
請注意，此處的 AppxManifest（商標檔案）中所參照的所有檔案無法移到資產套件 – 必須在架構套件上複製這些檔案。 資產套件也不應該包含 resources.pri；MRT 不能用來存取資產套件檔案。 若要了解更多有關如何存取資產套件檔案和資產套件需要應用程式安裝到 NTFS 磁碟機的原因，請查看[使用資產套件與套件摺疊進行開發](Package-Folding.md)。

若要控制是否允許資產套件執行，您可以使用 AppxManifest 的**Properties**元素中的**[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)**。您必須也新增**uap6**至最上層 **Package** 元素，變得下列項目： 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 如果未指定，**AllowExecution**的預設值是**true**– 將它設為**false**，可讓沒有可執行檔的資產套件更快速地發佈。  



