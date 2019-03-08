---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 選用套件及相關集合的製作
description: 選用套件包含了可與主要套件整合的內容。 這些選用套件相當適合用於可下載內容 (DLC)、將大型應用程式根據尺寸限制進行分割，或是傳送與您原始應用程式分離的額外內容。
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, uwp, 選用套件, 相關集合, 套件延伸模組, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594023"
---
# <a name="optional-packages-and-related-set-authoring"></a>選用套件及相關集合的製作
選用套件包含了可與主要套件整合的內容。 這些選用套件相當適合用於可下載內容 (DLC)、將大型應用程式根據尺寸限制進行分割，或是傳送與您原始應用程式分離的額外內容。

相關集合是選用套件的延伸模組，他們可讓您強制使用一組涵蓋了主要與選用套件的嚴格版本集合。 他們也可以讓您從選用套件載入機器碼 (C++)。 

## <a name="prerequisites"></a>必要條件

- Visual Studio 2017，版本 15.1
- Windows 10 版本 1703
- Windows 10，版本 1703 SDK

若要取得所有最新的開發工具，請參閱[適用於 Windows 10 的下載項目與工具](https://developer.microsoft.com/windows/downloads)。

> [!NOTE]
> 若要提交至 Microsoft Store 中使用選擇性的套件及/或相關的設定的應用程式，您需要的權限。 選擇性的套件和相關的設定可以用於企業營運 (LOB) 或企業應用程式，而不需要合作夥伴中心的權限若不提交至市集。 請參閱 [Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得提交使用選用套件和相關集合應用程式的權限。

### <a name="code-sample"></a>程式碼範例
當您在閱讀這篇文章的時候，我們建議您跟隨 GitHub 上的[選用套件程式碼範例](https://github.com/AppInstaller/OptionalPackageSample)，以實際了解選用套件和相關集合在 Visual Studio 中是如何運作的。

## <a name="optional-packages"></a>選用套件
若要在 Visual Studio 中建立選用套件，您將需要︰
1. 請確定您的應用程式**目標平台最小版本**設為：10.0.15063.0 或更高版本。
2. 從您的 **「主要套件」** 專案中，開啟 `Package.appxmanifest` 檔案。 瀏覽至 \[封裝\] 索引標籤，並記下您的**套件系列名稱**，也就是「_」字元之前的所有內容。
3. 從您的 **「選用套件」** 專案中，以滑鼠右鍵按一下 `Package.appxmanifest`，並選取 **\[開啟檔案\] > \[XML (文字) 編輯器\]**。
4. 從檔案中找出 `<Dependencies>` 元素。 加入下列內容：

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

用您在步驟 2 中取得的**套件系列名稱**取代 `[MainPackageDependency]`。 這將會指定您的**選用套件**依存於您的**主要套件**。

當你完成步驟 1 到 4，並且設定好套件相依性後，您就可以繼續像平常一樣進行開發。 若您想要從選用套件將程式碼載入主要套件，您將需要組建相關集合。 請參閱[相關集合](#related_sets)一節，以取得詳細資訊。

您可以設定 Visual Studio，使其在您每次部署選用套件時重新部署您的主要套件。 若要在 Visual Studio 中設定組建相依性，您必須︰

- 在選用套件專案上按一下滑鼠右鍵，然後選取 **\[組建相依性\] > \[專案相依性...\]**
- 檢查主要套件專案，並選取 \[確定\]。 

現在，每次您按下 F5 或建置選用套件專案時，Visual Studio 都會先建置主要套件。 這樣可確保您的主要專案與選用專案同步。

## 相關集合<a name="related_sets"></a>

若您想要從選用套件將程式碼載入主要套件，您將需要組建相關集合。 若要建置相關集合，您的主要套件和選用套件必須緊密結合。 關聯集的中繼資料檔中指定.appxbundle 或.msixbundle 的主要封裝。 Visual Studio 可協助您在您的檔案中取得正確的中繼資料。 若要針對相關集合設定您的應用程式方案，請依照下列步驟︰

1. 以滑鼠右鍵按一下主要套件專案，選取 **\[新增\] > \[新項目...\]**
2. 在視窗中，從已安裝的範本中搜尋「.txt」，然後新增一個新的文字檔案。
> [!IMPORTANT]
> 新的文字檔案必須命名為︰`Bundle.Mapping.txt`。

3. 在 `Bundle.Mapping.txt` 檔案中，您將會指定任何選用套件專案或外部套件的相對路徑。 一個 `Bundle.Mapping.txt` 檔案的範例看起來會像是這樣︰

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

在您使用此方式設定您的方案之後，Visual Studio 就會使用所有相關集合需要的中繼資料，為主要套件建立套件組合資訊清單。 

請注意，例如選擇性的套件，`Bundle.Mapping.txt`相關的設定檔僅適用於 Windows 10 版本 1703 版或更高版本。 此外，您的應用程式目標平台最低版本應該設定為 10.0.15063.0 或更高。

## 已知問題<a name="known_issues"></a>

目前 Visual Studio 並不支援偵錯相關集合選用專案。 若要處理此問題，您可以部署並啟動啟用 (Ctrl + F5)，並手動將偵錯工具附加到處理程序。 若要附加偵錯工具，請移至 Visual Studio 中的「偵錯」功能表，選取「附加到處理程序...」，然後將偵錯工具附加到 **「主要應用程式處理程序」**。