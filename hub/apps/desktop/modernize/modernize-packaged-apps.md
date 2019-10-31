---
Description: 瞭解如何在您已封裝在 Windows 應用程式套件中的桌面應用程式中，為 Windows 10 使用者新增現代化體驗。
title: 現代化已封裝的桌面應用程式
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142520"
---
# <a name="features-that-require-package-identity"></a>需要套件身分識別的功能

如果您想要使用[新式 Windows 10 體驗](index.md)來更新您的傳統型應用程式，有許多功能只適用于具有[套件識別](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)的桌面應用程式。 有數種方式可將套件身分識別授與桌面應用程式：

* 將其封裝在[MSIX 套件](/windows/msix/desktop/desktop-to-uwp-root)中。 MSIX 是現代化的應用程式套件格式，可為所有 Windows 應用程式、WPF、Windows Forms 和 Win32 應用程式提供通用封裝體驗。 它提供健全的安裝和更新體驗、具有彈性功能系統的受控安全性模型、Microsoft Store 的支援、企業管理，以及許多自訂的散發模型。 如需詳細資訊，請參閱 MSIX 文件中的[封裝傳統型應用程式](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)。
* 如果您無法採用 MSIX 封裝來部署桌面應用程式，從 Windows 10 Insider preview 組建10.0.19000.0 開始，您可以藉由建立只包含套件資訊清單的*SPARSE MSIX 套件*來授與套件識別。 如需詳細資訊，請參閱[將身分識別授與非封裝的桌面應用程式](grant-identity-to-nonpackaged-apps.md)。

如果您的桌面應用程式具有套件身分識別，您可以在應用程式中使用下列功能。

## <a name="use-uwp-apis-that-require-package-identity"></a>使用需要套件身分識別的 UWP Api

下列 UWP Api 清單需要在桌面應用程式中使用套件識別： [api 清單](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>整合套件擴充功能

如果您的應用程式需要與系統整合（例如：建立防火牆規則），請在應用程式的套件資訊清單中描述這些專案，系統就會執行其餘工作。 針對大部分的工作，您完全不需要撰寫任何程式碼。 在資訊清單中使用一些 XML，您可以執行一些動作，例如當使用者登入時啟動程式、將您的應用程式整合到檔案瀏覽器，以及將出現在其他應用程式中的列印目標清單新增至應用程式。

如需詳細資訊，請參閱[整合桌面應用程式與套件延伸](desktop-to-uwp-extensions.md)模組。

## <a name="extend-with-uwp-components"></a>使用 UWP 元件進行擴充

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 一般來說，您應該先使用 UWP Api 來[強化](desktop-to-uwp-enhance.md)現有的桌面應用程式，以判斷是否可以加入您的體驗。 如果您必須使用 UWP 元件來達到此體驗，則可以將 UWP 專案新增至您的方案，並使用應用程式服務在您的桌面應用程式和 UWP 元件之間進行通訊。

如需詳細資訊，請參閱[使用 UWP 元件擴充您的桌面應用程式](desktop-to-uwp-extend.md)。

## <a name="distribute"></a>散佈

如果您將應用程式封裝在 MSIX 套件中，您可以將它發佈 Microsoft Store 或將它側載到其他系統，藉以散發它。

請參閱[散發已封裝的桌面應用程式](desktop-to-uwp-distribute.md)。
