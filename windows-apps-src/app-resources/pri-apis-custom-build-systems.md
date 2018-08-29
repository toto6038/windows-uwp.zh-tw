---
author: stevewhims
Description: With the package resource indexing (PRI) APIs, you can develop a custom build system for your UWP app's resources. The build system will be able to create, version, and dump PRI files to whatever level of complexity your UWP app needs.
title: 套件資源索引 (PRI) API 和自訂建置系統
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 資源, 影像, 資產, MRT, 限定詞
ms.localizationpriority: medium
ms.openlocfilehash: 58473101f0972040fe80c7f7f582ad33eafeb009
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862497"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>套件資源索引 (PRI) API 和自訂建置系統
您可以使用[套件資源索引 (PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)，開發適用於您的 UWP app 資源的自訂組建系統。 建置系統可以依據 UWP app 所需的任何複雜層級，建立套件資源索引 (PRI) 檔案並對這些檔案進行版本控制和傾印 (為 XML)。 如果您的自訂建置系統目前使用 MakePri.exe 命令列工具 (請參閱[使用 MakePri.exe 來手動編譯資源](makepri-exe-command-options.md))，為了提高效能和控制，建議您改為呼叫 PRI API，而不呼叫 MakePri.exe。

PRI API 是在 Windows 10 版本 1803 的 Windows SDK 中引進。 API 採用 Win32 Windows API 的形式，這表示您有一些呼叫選項。 您可以直接從 Win32 應用程式進行呼叫，也可以透過[平台叫用](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)，從 .NET 應用程式甚至 UWP app 進行呼叫。

本主題中的案例示範從 Win32 Visual C++ Windows 主控台應用程式專案呼叫 PRI API。 如需背景資訊，請參閱[資源管理系統](resource-management-system.md)。

PRI 檔案的大小上限是 64 KB。

> [!NOTE]
> 這項限制不大可能構成問題，因為您大概不會想要將您的自訂建置系統應用程式提交至 Microsoft Store。 但要是您選擇要以 UWP app 形式開發自訂建置系統的選項，此應用程式將會是一個很不尋常的 UWP app，光憑這一點，您就無法將其提交至 Microsoft Store。 這是因為使用平台叫用的 UWP app 無法通過 Microsoft Store 認證。 請注意，在這種情況下，平台叫用呼叫*將只存在於您的自訂建置系統*，而*不*在您要推出的 UWP app (您建置 PRI 檔案所針對的應用程式) 中。

## <a name="scenario-walkthroughs"></a>案例逐步解說
|主題|描述|
|-|-|
|[案例 1：從字串資源和資產檔案建立 PRI 檔案](pri-apis-scenario-1.md)|在本案例中，我們會建立新的應用程式來表示我們的自訂建置系統。 我們會建立資源索引子，並將字串以及其他種類的資源新增至其中， 然後產生和傾印 PRI 檔案。|

## <a name="important-apis"></a>重要 API
* [套件資源索引 (PRI) 參考](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>相關主題
* [使用 MakePri.exe 來手動編譯資源](makepri-exe-command-options.md)
* [使用 Unmanaged DLL 函式](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [資源管理系統](resource-management-system.md)