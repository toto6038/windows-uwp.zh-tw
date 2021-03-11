---
description: '對 MRT.LOG 核心元件的總覽，以及它們如何運作以將應用程式資源載入 (Project 留尼旺島) '
title: '管理資源的 MRT.LOG 核心 (Project 留尼旺島) '
ms.topic: article
ms.date: 03/09/2021
keywords: MRT.LOG，MRTCore，pri，makepri，資源，資源載入
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: bde540ff99e763a2d5c622eba1d292f722008ef6
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629207"
---
# <a name="manage-resources-with-mrt-core-project-reunion"></a>使用 MRT.LOG Core (Project 留尼旺島) 管理資源

MRT.LOG Core 是新式 Windows [資源管理系統](/windows/uwp/app-resources/resource-management-system) 的精簡版，隨著 [Project 留尼旺島](../index.md)的一部分散發。

MRT.LOG 核心具有組建時間和執行時間功能。 在建置期間，系統會建立所有不同變體 (使用您的 App 封裝) 的資源的索引。 此索引套件資源索引或 PRI，並且也會包含在您的應用程式套件中。

## <a name="package-resource-index-pri-file"></a>套件資源索引 (PRI) 檔案

每個應用程式套件應在 App 中包含資源的二進位索引。 此索引是在組建階段建立的，而且會包含在一或多個資源 ( .resw) 檔案中。

.Resw 檔案包含實際的字串資源，以及一組索引的檔案路徑，參考封裝中的各種檔案。
封裝通常會包含每種語言的單一 .resw 檔案，名為 resources. .resw。 當 ResourceManager 具現化時，會自動載入每個封裝根目錄中的 .resw 檔案。

每個 .resw 檔都包含一個名為資源的資源集合，稱為資源對應。 載入封裝中的 .resw 檔案時，會驗證資源對應名稱以符合套件識別名稱。

.Resw 檔案只包含資料，因此不會使用可移植的可執行檔 (PE) 格式。 它們是特別設計成僅限資料。

## <a name="using-mrt-core-to-access-app-resources"></a>使用 MRT.LOG Core 來存取應用程式資源

### <a name="resource-loader-basic-functionality"></a>資源載入器 (基本功能) 

以程式設計方式存取應用程式資源最簡單的方式，就是使用 [ApplicationModel](/windows/winui/api/microsoft.applicationmodel.resources) 命名空間和 ResourceLoader 類別。 ResourceLoader 提供您資源檔案集、參考媒體櫃或其他套件的字串資源的基本存取權。

### <a name="resource-manager-advanced-functionality"></a>資源管理員 (advanced 功能) 

[ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)類別提供資源的其他相關資訊，例如列舉和檢查。 這超出 **ResourceLoader** 類別所提供的。

[ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) 物件代表單一實體資源值及其限定詞，例如針對英文的字串「Hello World」或做為合格影像資源的「logo.scale-100.jpg」，其為 scale-100 特定的解析度。

App 可用的資源儲存在階層集合中，您可以存取 [ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) 物件。 **ResourceManager** 類別提供 App 所使用各種最上層 **ResourceMap** 執行個體的存取權，對應至 App 的各種不同套件。 [MainResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap)值會對應至目前應用程式套件的資源對應，並排除任何參考的架構套件。 各個 **ResourceMap** 會針對套件資訊清單中指定的套件名稱命名。 **Windows.applicationmodel.resources.core.resourcemap** 中的子樹 (參閱 [windows.applicationmodel.resources.core.resourcemap. GetSubtree](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap.getsubtree)) 。 子樹系通常對應至包含資源的資源檔案。

**ResourceManager** 不只支援存取 App 的字串資源，也維護列舉和檢查各種不同檔案資源的能力。 為避免檔案和源自檔案內其他資源之間發生衝突，已建立索引的檔案路徑均在保留的「檔案」**ResourceMap** 子樹系內。 例如，檔案 ' \Images\logo.png ' 對應至資源名稱 ' Files/images/logo.png '。

### <a name="resourcecontext"></a>ResourceContext

資源候選項目的選擇是根據特定 [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)，這是資源限定詞值的集合 (語言、縮放比例，對比等)。 預設內容會使用 App 目前針對每個限定詞值的設定，除非被覆寫。 例如，影像之類的資源可以縮放，從一部監視器到另一部都不同，從一個應用程式檢視到另一個也不同。 基於這個原因，每個應用程式檢視具有不同的預設內容。 每當您擷取資源候選項目時，您應該在 **ResourceContext** 執行個體中傳遞，為指定的檢視取得最適當的值。

### <a name="important-apis"></a>重要 API

- [ResourceLoader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader)
- [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)
- [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)

## <a name="sample"></a>範例

如需示範如何使用 MRT.LOG 核心 API 的範例，請參閱 [Mrt.log core 範例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)。
