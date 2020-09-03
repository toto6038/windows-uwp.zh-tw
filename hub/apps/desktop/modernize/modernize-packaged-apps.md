---
Description: 了解如何在您已封裝在 Windows 應用程式套件中的傳統型應用程式中，為 Windows 10 使用者新增現代化體驗。
title: 讓封裝的傳統型應用程式現代化
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: d1ce2e7dc434558ac1efd52f6def99d63b38c57e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161512"
---
# <a name="features-that-require-package-identity"></a>需要套件身分識別的功能

如果您想使用[現代化 Windows 10 體驗](index.md)來更新傳統型應用程式，則許多功能僅適用於具有[套件識別資料](/uwp/schemas/appxpackage/uapmanifestschema/element-identity)的傳統型應用程式。 將套件識別資料授與傳統型應用程式的方式有數種：

* 將其封裝在 [MSIX 套件](/windows/msix/desktop/desktop-to-uwp-root)中。 MSIX 是新式應用程式套件格式，為所有 Windows 應用程式、UWP、WPF、Windows Forms 及 Win32 應用程式提供通用封裝體驗。 這可以提供強固的安裝和更新體驗、具有彈性功能系統的受控安全性模型、Microsoft Store 的支援、企業管理，以及許多自訂散發模型。 如需詳細資訊，請參閱 MSIX 文件中的[封裝傳統型應用程式](/windows/msix/desktop/desktop-to-uwp-root)。
* 如果您無法採用 MSIX 封裝來部署傳統型應用程式，請從 Windows 10 版本 2004 開始，藉由建立僅包含套件資訊清單的*疏鬆 MSIX 套件*來授與套件識別。 如需詳細資訊，請參閱[將身分識別授與非封裝的傳統型應用程式](grant-identity-to-nonpackaged-apps.md)。

如果您的傳統型應用程式具有套件識別資料，則可在您的應用程式中使用下列功能。

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>使用需要套件識別資料的 Windows 執行階段 API

下列清單中的 Windows 執行階段 API 都需要傳統型應用程式有套件識別資料：[API 清單](desktop-to-uwp-supported-api.md#list-of-apis)。

## <a name="integrate-with-package-extensions"></a>整合套件擴充功能

如果應用程式需要與系統整合 (例如：建立防火牆規則)，請在應用程式的封裝資訊清單中描述這些項目，系統會替您完成其餘的工作。 針對大部分的工作，您完全不需要撰寫任何程式碼。 只需在資訊清單中提供一些 XML，您就可以執行一些工作，像是在使用者登入時執行處理程序、將應用程式與檔案總管整合，以及將應用程式加入在其他應用程式中出現的列印目標清單。

如需詳細資訊，請參閱[整合您的傳統型應用程式與套件擴充功能](desktop-to-uwp-extensions.md)。

## <a name="extend-with-uwp-components"></a>使用 UWP 元件進行擴充

有些 Windows 10 體驗 (例如：具有觸控功能的 UI 頁面) 必須在現代化應用程式容器中執行。 一般而言，您應該先判斷是否可以使用 Windows 執行階段 API 透過[增強](desktop-to-uwp-enhance.md)現有的傳統型應用程式來新增體驗。 如果您必須使用 UWP 元件來達成體驗，則可以將 UWP 專案加入方案，並使用應用程式服務在傳統型應用程式和 UWP 元件之間通訊。

如需詳細資訊，請參閱[使用 UWP 元件擴充您的傳統型應用程式](desktop-to-uwp-extend.md)。

## <a name="distribute"></a>散佈

如果您將應用程式封裝在 MSIX 套件中，則可在 Microsoft Store 中發佈該應用程式，或將其側載至其他系統，藉此進行散發。

請參閱[散發封裝的傳統型應用程式](desktop-to-uwp-distribute.md)。