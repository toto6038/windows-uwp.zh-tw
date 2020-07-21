---
title: Windows UI 程式庫的 NuGet 套件清單
description: 列出 Windows UI 程式庫中的 NuGet 套件
ms.topic: article
ms.date: 07/15/2020
keywords: windows 10, uwp, 工具組 sdk
ms.openlocfilehash: ca3f2915d77bb83f45744e90bd86e82edba013b8
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492883"
---
# <a name="windows-ui-library-nuget-packages"></a>Windows UI 程式庫 NuGet 套件

NuGet 是內建於 Visual Studio 的 .Net 應用程式標準套件管理員。 從開啟的解決方案中，選擇 [工具] 功能表、[NuGet 套件管理員]、[管理方案的 NuGet 套件...] 以開啟 UI。  輸入下列其中一個套件名稱，以在線上搜尋。

| NuGet 套件名稱 | 說明 |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | 適用於 UWP 應用程式的控制項。 納入來自下列命名空間的 API：[Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml)、[Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers)、[Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls)、[Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives)、[Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes)、[Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media)、[Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | 可讓您在舊版 Windows 10 上使用 [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) API，而不需要撰寫特殊程式碼來處理多個目標 Windows 10 版本。 |


## <a name="search-in-visual-studio"></a>在 Visual Studio 中搜尋

在 Visual Studio 套件管理員中搜尋時，您應該會看到類似下面的清單 (版本號碼可能不同，但名稱應該會相同)。

![NuGet 套件管理員](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>更新 Nuget 套件

我們會定期更新 Windows UI 程式庫以納入新的控制項、服務、API，以及更重要的 Bug 修正。 若要確保您使用的是最新版本，請在 Visual Studio 中開啟專案，選擇 [工具] 功能表，選取 [NuGet 套件管理員] -> [管理方案的 NuGet 套件...]，然後選取 [更新] 索引標籤。選取您要更新的套件，然後按一下 [安裝] 以更新至最新版本。

## <a name="getting-started"></a>開始使用

如需如何在自己的專案中使用這些 NuGet 套件的詳細指示，請參閱[開始使用 Windows UI 程式庫](getting-started.md)。
