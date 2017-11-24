---
author: stevewhims
Description: "建置期間，資源管理系統會建立所有不同變體 (使用您的 App 封裝) 的資源的索引。 在執行階段，系統會偵測生效的使用者和電腦設定，並載入這些設定的最佳相符項的資源。"
title: "資源管理系統"
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞"
localizationpriority: medium
ms.openlocfilehash: 8ad0d8a22271e21d146809df7199f3f211fedbee
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2017
---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

# <a name="resource-management-system"></a>資源管理系統

資源管理系統有建置時間和執行階段的功能。 在建置期間，系統會建立所有不同變體 (使用您的 App 封裝) 的資源的索引。 此索引套件資源索引或 PRI，並且也會包含在您的應用程式套件中。 在執行階段，系統偵測到已生效的使用者與電腦設定，查詢 PRI 中的資訊，並自動載入最符合這些設定的資源。

## <a name="package-resource-index-pri-file"></a>套件資源索引 (PRI) 檔案

每個應用程式套件應在 App 中包含資源的二進位索引。 此索引是在建置階段建立並且包含在一個或多個套件資源索引 (PRI) 檔案中。

- PRI 檔案包含實際的字串資源和在套件中參照不同檔案的已建立索引的檔案路徑。
- 套件通常會包含每種語言命名為 resources.pri 的單一 PRI 檔案。
- 在每個套件根目錄的 Resources.pri 檔案會在 [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 具現化時自動載入。
- PRI 檔案可以使用工具 [MakePRI.exe](compile-resources-manually-with-makepri.md) 建立及傾印。
- 一般的 App 開發，您不需要 MakePRI.exe，因為它已經整合到 Visual Studio 編譯工作流程中。 而 Visual Studio 支援在專用 UI 中編輯 PRI 檔案。 不過，您的當地語系化人員和他們使用的工具可能仰賴 MakePRI.exe。
- 每個 PRI 檔案包含具名的資源集合，稱為資源地圖。 載入套件的 PRI 檔案時，資源地圖名稱已經過驗證，符合套件識別資料名稱。
- PRI 檔案只包含資料，所以它們不使用可攜式執行檔 (PE) 的格式。 它們是特別設計為純資料，作為 Windows 的資源格式。 它們取代 Win32 App 模型中 DLL 所內含的資源。

## <a name="uwp-api-access-to-app-resources"></a>UWP API 存取 App 資源

### <a name="basic-functionality-resourceloader"></a>基本功能 (ResourceLoader)

以程式設計方式存取您的 App 資源，最簡單的途徑是使用 [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) 命名空間和 [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live) 類別。 **ResourceLoader** 提供您資源檔案集、參考媒體櫃或其他套件的字串資源的基本存取權。

### <a name="advanced-functionality-resourcemanager"></a>進階功能 (ResourceManager)

[**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) 類別 (在 [**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live) 命名空間中) 提供資源的其他的相關資訊，例如列舉和檢查。 這超出 **ResourceLoader** 類別所提供的。

[**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) 物件代表多種語言或其他合格變體的個別邏輯資源。 它描述資產或資源的邏輯檢視，包含字串資源識別碼與，例如 `Header1` 或例如 `logo.jpg` 的資源檔案名稱。

[**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) 物件代表單一實體資源值及其限定詞，例如針對英文的字串「Hello World」或做為合格影像資源的「logo.scale-100.jpg」，其為 **scale-100** 特定的解析度。

App 可用的資源儲存在階層集合中，您可以存取 [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) 物件。 **ResourceManager** 類別提供 App 所使用各種最上層 **ResourceMap** 執行個體的存取權，對應至 App 的各種不同套件。 [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager#windows_applicationmodel_resources_core_resourcemanager_mainresourcemap?branch=live) 值對應至目前應用程式套件的資源地圖，它不包含任何參考的架構套件。 各個 **ResourceMap** 會針對套件資訊清單中指定的套件名稱命名。 在 **ResourceMap** 內是子樹系 (請參閱 [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live#windows_applicationmodel_resources_core_resourcemap_getsubtree_system_string_))，它也包含 **NamedResource** 物件。 子樹系通常對應至包含資源的資源檔案。 如需詳細資訊，請參閱[將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md)和[載入針對縮放比例、佈景主題、高對比及其他設定量身打造的影像和資產](images-tailored-for-scale-theme-contrast.md)。

範例如下。

**C#**
```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**注意** 資源識別碼會被視為統一資源識別項 (URI) 語意受統一資源識別碼 (URI) 片段，遵照 URI 語意。 例如，`GetValue("Caption%20")` 被視為 `GetValue("Caption ")`。 勿在資源識別碼中使用「?」或「#」，因為它們會終止資源路徑評估。 例如，「MyResource?3」會被視為「MyResource」。

**ResourceManager** 不只支援存取 App 的字串資源，也維護列舉和檢查各種不同檔案資源的能力。 為避免檔案和源自檔案內其他資源之間發生衝突，已建立索引的檔案路徑均在保留的「檔案」**ResourceMap** 子樹系內。 例如，檔案 `\Images\logo.png` 對應至資源名稱 `Files/images/logo.png`。

[**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) API 無障礙地處理檔案參照為資源，並且適用於常見使用案例。 **ResourceManager** 應僅用於進階案例，例如當您想要覆寫目前內容時。

### <a name="resourcecontext"></a>ResourceContext

資源候選項目的選擇是根據特定 [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live)，這是資源限定詞值的集合 (語言、縮放比例，對比等)。 預設內容會使用 App 目前針對每個限定詞值的設定，除非被覆寫。 例如，影像之類的資源可以縮放，從一部監視器到另一部都不同，從一個應用程式檢視到另一個也不同。 基於這個原因，每個應用程式檢視具有不同的預設內容。 指定檢視的預設內容可使用 [**ResourceContext.GetForCurrentView**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext.md#Windows_ApplicationModel_Resources_Core_ResourceContext_GetForCurrentView) 取得。 每當您擷取資源候選項目時，您應該在 **ResourceContext** 執行個體中傳遞，為指定的檢視取得最適當的值。

## <a name="important-apis"></a>重要 API

* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>相關主題

* [將 UI 及應用程式套件資訊清單中的字串當地語系化](localize-strings-ui-manifest.md)
* [載入針對縮放比例、佈景主題、高對比及其他設定量身打造的影像和資產](images-tailored-for-scale-theme-contrast.md)
