---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 選用套件及相關集合的製作
description: 選用套件包含了可與主要套件整合的內容。 這些選用套件相當適合用於可下載內容 (DLC)、將大型應用程式根據尺寸限制進行分割，或是傳送與您原始應用程式分離的額外內容。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10、 uwp、 選用套件，相關的集合、 套件延伸模組、 visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750244"
---
# <a name="optional-packages-and-related-set-authoring"></a>選用套件及相關集合的製作
選用套件包含了可與主要套件整合的內容。 這些適合用於可下載內容 (DLC)、 將大型應用程式的大小越大，或傳送任何額外的內容與您原始應用程式分離。

相關的集合是選用套件延伸--它們允許您跨主要和選用套件強制執行一套嚴格的版本。 也可以讓您從選用套件載入原生程式碼 （c + +）。 

## <a name="prerequisites"></a>必要條件

- Visual Studio 2017，版本 15.1
- Windows10 (版本 1703)
- Windows 10，1703年版 SDK

若要取得所有最新的開發工具，請參閱[下載項目和適用於 Windows 10 的工具](https://developer.microsoft.com/windows/downloads)。

> [!NOTE]
> 若要提交到 Microsoft Store 中使用選用套件和/或相關的集合的應用程式，您將需要權限。 如果不提交至市集，可以沒有合作夥伴中心的許可下線 (LOB) 或企業應用程式使用選用套件及相關的集合。 請參閱 [Windows 開發人員支援](https://developer.microsoft.com/windows/support)，以取得提交使用選用套件和相關集合應用程式的權限。

### <a name="code-sample"></a>程式碼範例
雖然您正在閱讀這篇文章，建議您依照[選用套件的程式碼範例](https://github.com/AppInstaller/OptionalPackageSample)GitHub 上的實機操作的了解如何選用套件的及相關集合工作，在 Visual Studio 內。

## <a name="optional-packages"></a>選用套件
若要在 Visual Studio 中建立的選用套件，您將需要：
1. 請確定您的應用程式的**目標平台最小版本**設定為： 10.0.15063.0 或更高版本。
2. 從您**的主要套件**的專案，開啟`Package.appxmanifest`檔案。 瀏覽至 [封裝 \] 索引標籤，然後記下您的**套件系列名稱**，這是"_"字元之前的所有項目。
3. 從**選用套件**專案，以滑鼠右鍵按一下`Package.appxmanifest`，然後選取**開啟 > XML （文字） 編輯器**。
4. 找出`<Dependencies>`檔案中的項目。 新增下列內容：

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

取代`[MainPackageDependency]`您的**套件系列名稱**從步驟 2。 這會指定您的**選用套件**是取決於您的**主要套件**。

一旦您有您的套件相依性，從設定步驟 1 到 4，您可以繼續進行開發像平常一樣。 如果您想要從選用套件載入程式碼到主要套件，您將需要建置相關的集合。 請參閱[相關設定](#related_sets)一節，如需詳細資訊。

Visual Studio 可以設定為您部署的選用套件每次重新部署您的主要套件中。 若要在 Visual Studio 中設定組建相依性，您應該：

- 選用套件專案上按一下滑鼠右鍵，然後選取**建置相依性 > 專案相依性...**
- 檢查主要套件專案，並選取 「 確定 」。 

現在，每當您輸入 F5 或建置選用套件專案，Visual Studio 會建置主要套件專案，第一次。 這樣可確保您的主要專案和選用的專案是同步。

## 相關的集合<a name="related_sets"></a>

如果您想要從選用套件載入程式碼到主要套件，您將需要建置相關的集合。 若要建置相關的集合，您的主要套件與選用套件必須緊密結合。 相關集合的中繼資料是.appxbundle 或.msixbundle 在檔案中指定的主套件。 Visual Studio 可以協助您在您的檔案中取得正確的中繼資料。 設定您的應用程式的相關集合的解決方案，請使用下列步驟：

1. 以滑鼠右鍵按一下主要套件專案中，選取**新增 > 新項目**
2. 在視窗中，搜尋 「.txt 」 已安裝的範本並加入新的文字檔案。
> [!IMPORTANT]
> 新的文字檔案必須命名為： `Bundle.Mapping.txt`。

3. 在`Bundle.Mapping.txt`檔案，您可以指定任何選用套件專案或套件外部的相對路徑。 範例`Bundle.Mapping.txt`檔案應該看起來像這樣：

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

您的方案設定時如此一來，Visual Studio 會建立主要套件的套件組合資訊清單與所有必要的中繼資料相關集合。 

請注意，像是選用套件，`Bundle.Mapping.txt`相關集合的檔案只會在 Windows 10 版本 1703年或更新版本上運作。 此外，您的應用程式的目標平台最小版本應該必須設定為 10.0.15063.0 或更高版本。

## 已知問題<a name="known_issues"></a>

偵錯相關的集合的選擇性專案目前不支援在 Visual Studio 中。 若要解決此問題，您可以部署和啟動啟用 （Ctrl + F5） 和手動附加至處理程序的 [偵錯工具。 若要附加偵錯工具，請在 Visual Studio 中的 「 偵錯 」 功能表、 選取 「 附加至處理程序...」，並將偵錯工具附加到**主應用程式處理程序**。