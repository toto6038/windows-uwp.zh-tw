---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 選用套件及相關集合的製作
description: 選用套件包含了可與主要套件整合的內容。 這些選用套件相當適合用於可下載內容 (DLC)、將大型應用程式根據尺寸限制進行分割，或是傳送與您原始應用程式分離的額外內容。
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 選用的套件、 一組相關、 套件延伸模組、 visual studio
ms.localizationpriority: medium
ms.openlocfilehash: d66a511211396190393e31bfd553149a1e89fad0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2018
ms.locfileid: "406078"
---
# <a name="optional-packages-and-related-set-authoring"></a>選用套件及相關集合的製作
選用套件包含了可與主要套件整合的內容。 這些是有用的可下載內容 (DLC) 餘數大小越大的大型應用程式或進行傳送的任何其他內容隔開您原始的應用程式。

相關的設定是選用的套件的副檔名--他們允許您跨主要及選用套件，強制執行一組 strict 版本。 也可以讓您從選用套件載入原生程式碼 （c + +）。 

## <a name="prerequisites"></a>必要條件

- Visual Studio 2017、 版本 15.1
- Windows 10 版本 1703
- 第 10 Windows 版本 1703 sdk （英文)

若要取得所有的最新的開發工具，請參閱[下載及工具的 Windows 10](https://developer.microsoft.com/windows/downloads)。

> [!NOTE]
> 送出至 Microsoft 儲存使用選用的套件及 （或） 相關的設定應用程式，您將需要權限。 選用套件和相關集合可在沒有開發人員中心的許可下用於企業營運 (LOB) 或企業應用程式，只要沒有提交到 Microsoft Store 即可。 請參閱 [Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得提交使用選用套件和相關集合應用程式的權限。

### <a name="code-sample"></a>程式碼範例
當您正在閱讀這篇文章時，建議您依照以及[選用的套件程式碼範例](https://github.com/AppInstaller/OptionalPackageSample)上 GitHub 瞭解如何選用套件的實機操作概念及相關 Visual Studio 內將工時。

## <a name="optional-packages"></a>選用的套件
若要在 Visual Studio 建立的選用套件，您需要：
1. 請確定您的應用程式**目標平台 Min 版本**設為： 10.0.15063.0。
2. 從 [**主要套件**專案中，開啟`Package.appxmanifest`檔案。 瀏覽至"封裝"的索引標籤，然後記下您的**套件系列產品名稱**，這是"_"字元前面的每個項目。
3. 從**選用套件**專案，以滑鼠右鍵按一下`Package.appxmanifest`選取**中開啟 > XML （文字） 編輯器**。
4. 找出`<Dependencies>`檔案中的項目。 新增以下內容：

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

取代`[MainPackageDependency]`與步驟 2 您**套件系列名稱**。 這會指定您的**選用套件**是取決於您的**主要套件**。

當您設定從步驟 1 到 4 的套件相依性，您可以繼續開發像平常一樣。 如果您想要從選用套件的程式碼載入主要套件，您必須建立一組相關。 請參閱[相關設定](#related_sets)] 區段中的如需詳細資訊。

Visual Studio 可以設定為重新部署選用的套件部署每次您主要的套件。 若要在 Visual Studio 中設定組建相依性，您應該：

- 以滑鼠右鍵按一下 [選用套件專案並選取**建立相依性 >...專案相依性**
- 檢查主要套件專案並選取"OK"。 

現在，每次您輸入 F5 或建立選用套件專案、 Visual Studio 會建立主要套件專案第一次。 如此可確保您的主要專案和選用的專案會同步。

## 相關的設定<a name="related_sets"></a>

如果您想要從選用套件的程式碼載入主要套件，您必須建立一組相關。 若要建立一組相關主要套件及選用套件必須緊密的結合。 相關設定的中繼資料中所指定`.appxbundle`主要套件的檔案。 Visual Studio 可協助您取得您的檔案中的正確的中繼資料。 若要設定您的應用程式解決方案的相關的設定，請使用下列步驟：

1. 以滑鼠右鍵按一下 [主要套件專案中，選取**新增 > 新增項目**
2. 從視窗，搜尋已安裝的範本".txt"，並新增新的文字檔案。
> [!IMPORTANT]
> 新的文字檔案必須命名為： `Bundle.Mapping.txt`。
3. 在`Bundle.Mapping.txt`將指定的任何選擇性套件專案或外部套件的相對路徑的檔案。 樣本`Bundle.Mapping.txt`檔案看起來應該類似如下：

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

當您的解決方案設定如此一來時，Visual Studio 將所有必要的中繼資料的相關的設定建立主要套件包資訊清單。 

請注意，像是選用套件`Bundle.Mapping.txt`相關的設定檔僅會處理 Windows 10、 版本 1703年。 此外，您的應用程式目標平台 Min 版本應設為 10.0.15063.0。

## 已知問題<a name="known_issues"></a>

偵錯的一組相關的選用專案目前不支援在 Visual Studio 中。 若要解決此問題，您可以部署和啟動 （Ctrl + F5） 啟用及手動將偵錯程式附加到程序。 若要偵錯程式附加移 Visual Studio 的"Debug"] 功能表、 選取"附加至處理序..."，並附加偵錯程式**主應用程式程序**。